Azure Container Instances を使用すると、仮想マシンをプロビジョニングしたり、より高度なレベルのサービスを採用したりしなくても、Azure の Docker コンテナーを簡単に作成、管理できます。 このユニットでは、Azure でコンテナーを作成し、完全修飾ドメイン名 (FQDN) を使用してインターネットに公開します。

## <a name="create-a-resource-group"></a>リソース グループの作成

Azure コンテナー インスタンスは、すべての Azure リソースと同様に、リソース グループに配置する必要があります。リソース グループとは、Azure リソースのデプロイと管理に使用する論理コレクションです。

`az group create` コマンドでリソース グループを作成します。

次の例では、*myResourceGroup* という名前のリソース グループを *eastus* に作成します。

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="creat-a-container"></a>コンテナーを作成する

コンテナーを作成するには、名前、Docker イメージ、および Azure リソース グループを az container create コマンドに指定します。 必要に応じて、DNS 名ラベルを指定してコンテナーをインターネットに公開できます。 この例では、小型の Web アプリをホストするコンテナーをデプロイします。

次のコマンドを実行して、コンテナー インスタンスを開始します。 *--dns-name-label* の値は、インスタンスを作成する Azure リージョン内で一意である必要があります。そのため、場合によっては一意性を確保するためにこの値を変更する必要があります。

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name mycontainer \
    --image microsoft/aci-helloworld \
    --ports 80 \
    --dns-name-label aci-demo
```

数秒のうちに、要求に対する応答が得られます。 最初に、コンテナーは**作成中**の状態ですが、数秒のうちに起動されます。 `az container show` コマンドを使用して状態を確認することができます。

```azurecli
az container show \
    --resource-group myResourceGroup \
    --name mycontainer \
    --query "{FQDN:ipAddress.fqdn,ProvisioningState:provisioningState}" \
    --out table
```

コマンドを実行すると、コンテナーの完全修飾ドメイン名 (FQDN) とそのプロビジョニング状態が表示されます。

```bash
FQDN                                    ProvisioningState
--------------------------------------  -------------------
aci-demo.eastus.azurecontainer.io       Succeeded
```

コンテナーが成功状態に推移したら、ブラウザーでその FQDN に移動します。

![Azure コンテナー インスタンスで実行されているアプリケーションを示すブラウザー スクリーンショット](../media-draft/aci-app-browser.png)

## <a name="summary"></a>まとめ

このユニットでは、Web サーバーとアプリケーションを実行する Azure Container Instances を作成しました。 また、このアプリケーションには、コンテナー インスタンスの FQDN を使用してアクセスしました。

次のユニットでは、コンテナー インスタンスの再起動のポリシーを構成します。

