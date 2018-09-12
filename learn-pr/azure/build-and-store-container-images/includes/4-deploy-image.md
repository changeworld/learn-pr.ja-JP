Azure Container Registry や、Azure Container Instances、Azure Kubernetes Registry、Docker for Windows/Mac などの多くのコンテナー管理プラットフォームから、コンテナー イメージをプルできます。 Azure Container Registry からコンテナー イメージを実行するときは、認証資格情報が必要になる場合があります。 Container Registry での認証には、Azure サービス プリンシパルを使用することをお勧めします。 さらに、Azure Key Vault で Azure サービス プリンシパルの資格情報をセキュリティ保護することも勧めします。

このユニットでは、Azure Container Registry 用のサービス プリンシパルを作成し、それを Azure Key Vault に格納してから、サービス プリンシパルの資格情報を使用して Azure Container Instances にコンテナーを展開します。

## <a name="configure-registry-authentication"></a>レジストリの認証を構成する

すべての運用シナリオでは、サービス プリンシパルを使用して Azure コンテナー レジストリにアクセスします。 サービス プリンシパルを使用すると、コンテナー イメージにロールベースのアクセス制御 (RBAC) を提供できます。 たとえば、レジストリに対するプルのみのアクセス権を持つサービス プリンシパルを構成できます。

Azure Key Vault にコンテナーがまだない場合は、Azure CLI の次のコマンドを使用して作成します。

最初に、コンテナー レジストリの名前で変数を作成します。 この変数は、このユニット全体で使われます。

```azurecli
ACR_NAME=<acrName>
```

`az keyvault create` コマンドで Azure キー コンテナーを作成します。

```azurecli
az keyvault create --resource-group myResourceGroup --name $ACR_NAME-keyvault
```

次に、サービス プリンシパルを作成し、その資格情報をキー コンテナーに格納する必要があります。

サービス プリンシパルを作成するには、`az ad sp create-for-rbac` コマンドを使用します。 `--role` 引数により、*reader* ロールを持つサービス プリンシパルが構成され、レジストリに対するプルのみのアクセス権が付与されます。 プッシュ アクセス権とプル アクセス権の両方を付与するには、`--role` 引数を *contributor* に変更します。

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

サービス プリンシパルの作成の出力は次のようになります。 `appId` と `password` の値を書き留めておきます。 これらは Azure キー コンテナーに格納されます。

```bash
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

次に、`az keyvault secret set` コマンドを使用して、サービス プリンシパルの *appId* をコンテナーに格納します。 `<appId>` は、サービス プリンシパルの `appId` に置き換えます。

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <appId>
```

次に、`az keyvault secret set` コマンドを使用して、サービス プリンシパルの *password* をコンテナーに格納します。 `<password>` は、サービス プリンシパルの `password` に置き換えます。

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
```

Azure キー コンテナーを作成し、そこに 2 つのシークレットを格納しました。

* `$ACR_NAME-pull-usr`: サービス プリンシパルの ID。コンテナー レジストリの**ユーザー名**として使用します。
* `$ACR_NAME-pull-pwd`: サービス プリンシパルのパスワード。コンテナー レジストリの**パスワード**として使用します。

これらのシークレットは、ユーザーまたはアプリケーションやサービスがレジストリからイメージをプルするときに名前で参照できます。

### <a name="deploy-a-container-with-azure-cli"></a>Azure CLI でコンテナーを展開する

サービス プリンシパルの資格情報を Azure Key Vault に格納したので、アプリケーションやサービスでそれを使用してプライベート レジストリにアクセスできます。

コンテナー インスタンスを展開するには、次の `az container create` コマンドを実行します。 このコマンドでは、Azure Key Vault に格納されているサービス プリンシパルの資格情報を使用して、コンテナー レジストリに対する認証を行います。

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

Azure コンテナー インスタンスの IP アドレスを取得します。

```azurecli
az container show --resource-group myResourceGroup --name acr-build --query ipAddress.ip --output table
```

ブラウザーを開き、コンテナーの IP アドレスに移動します。 すべてが正しく構成されている場合、次の結果が表示されるはずです。

![テキスト Hello World が表示されるサンプル Web アプリケーション](../media/hello.png)

