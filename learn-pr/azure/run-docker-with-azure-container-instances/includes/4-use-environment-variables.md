コンテナー インスタンス内の環境変数により、コンテナーによって実行されるアプリケーションまたはスクリプトの動的な構成を指定できます。 Azure CLI、PowerShell、または Azure portal を使用して、コンテナーを作成するときに変数を設定できます。 コンテナーの出力に機密情報が表示されるのを防ぐには、セキュリティで保護された環境変数を使います。

ここでは、Azure Cosmos DB インスタンスを作成し、環境変数を使用して接続情報を Azure コンテナー インスタンスに渡します。 コンテナー内のアプリケーションでは、変数を使用して Cosmos DB のデータを読み書きします。 環境変数とセキュリティで保護された環境変数の両方を作成します。

## <a name="deploy-azure-cosmos-db"></a>Azure Cosmos DB をデプロイする

`az cosmosdb create` コマンドを使用して Azure Cosmos DB インスタンスを作成します。 また、この例では、Azure Cosmos DB のエンドポイント アドレスを *COSMOS_DB_ENDPOINT* という名前の変数に配置します。 `[cosmos-db-name]` には一意の名前を指定する必要があります。

このコマンドが完了するまで数分かかることがあります。

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query documentEndpoint --output tsv)
```

次に、`az cosmosdb list-keys` コマンドを使用して Azure Cosmos DB の接続キーを取得し、それを *COSMOS_DB_MASTERKEY* という名前の変数に格納します。 `[cosmos-db-name]` をもう一度置換することを忘れないでください。

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query primaryMasterKey --output tsv)
```

## <a name="deploy-a-container-instance"></a>コンテナー インスタンスをデプロイする

`az container create` コマンドを使用して、Azure コンテナー インスタンスを作成します。 2 つの環境変数の値 `COSMOS_DB_ENDPOINT` と `COSMOS_DB_MASTERKEY` が作成されたことを確認します。 これらの変数には、Azure Cosmos DB インスタンスに接続するために必要な値が保持されています。

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

コンテナーが作成された後、`az container show` コマンドを使用して IP アドレスを取得します。

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --query ipAddress.ip \
    --output tsv
```

ブラウザーを開き、コンテナーの IP アドレスに移動します。 

> [!IMPORTANT]
> コンテナーが完全に起動し、接続を受信できるようになるまで 1 分から 2 分かかる場合があります。 ブラウザー内で IP アドレスに移動しても応答がない場合は、しばらく待ってからページを更新します。

 アプリが使用できるようになると、次のアプリケーションが表示されます。 投票を行うと、Azure Cosmos DB インスタンスに投票が格納されます。

![猫または犬という 2 つの選択肢がある Azure 投票アプリケーション。](../media/4-azure-vote.png)

## <a name="secured-environment-variables"></a>セキュリティで保護された環境変数

上の演習では、コンテナーは 2 つの環境変数に格納されている Azure Cosmos DB に対する接続情報を使用して作成されました。 既定では、環境変数は Azure portal とコマンド ライン ツールにプレーン テキストで表示されます。

たとえば、前の演習で作成したコンテナーに関する情報を `az container show` コマンドで取得する場合、環境変数にはプレーン テキストでアクセスできます。

```azurecli
az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
```

出力例:

```json
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": "https://aci-cosmos.documents.azure.com:443/"
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": "Xm5BwdLlCllBvrR26V00000000S2uOusuglhzwkE7dOPMBQ3oA30n3rKd8PKA13700000000095ynys863Ghgw=="
  }
]
```

セキュリティで保護された環境変数を使用するとクリア テキストで出力されなくなります。 セキュリティで保護された環境変数を使用するには、`--environment-variables` 引数を `--secure-environment-variables` 引数に置き換えます。

セキュリティで保護された環境変数を使用する *aci-demo-secure* という名前のコンテナーを作成するには、次の例を実行します。

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

コンテナーが `az container show` コマンドで返されたとき、環境変数は表示されません。

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --query containers[0].environmentVariables
```

出力例:

```json
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": null
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": null
  }
]
```