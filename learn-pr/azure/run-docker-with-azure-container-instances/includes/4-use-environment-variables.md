Container instances で環境変数を使用すると、アプリケーションまたはコンテナーによって実行されるスクリプトの動的な構成を提供できます。 Azure CLI、PowerShell、または Azure portal を使用して、コンテナーを作成するときに変数を設定します。 コンテナー操作の出力に機密情報が表示されるのを防ぐには、セキュリティで保護された環境変数を使います。

ここでは、Azure Cosmos DB インスタンスと Azure コンテナー インスタンスに接続情報を渡す使用の環境変数を作成します。 コンテナー内のアプリケーションでは、Cosmos DB からデータを読み書きする変数を使用します。 環境変数と、セキュリティで保護された環境変数の両方を作成します。

## <a name="deploy-azure-cosmos-db"></a>Azure Cosmos DB をデプロイする

`az Azure Cosmos DB create` コマンドを使用して Azure Cosmos DB インスタンスを作成します。 また、この例では、Azure Cosmos DB のエンドポイント アドレスを *COSMOS_DB_ENDPOINT* という名前の変数に配置します。

このコマンドが完了するまで数分かかることがあります。

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query documentEndpoint -o tsv)
```

次に、`az cosmosdb list-keys` コマンドを使用して Azure Cosmos DB の接続キーを取得し、*COSMOS_DB_MASTERKEY* という名前の変数に格納します。

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a>コンテナー インスタンスをデプロイする

`az container create` コマンドを使用して、Azure コンテナー インスタンスを作成します。 `COSMOS_DB_ENDPOINT` および `COSMOS_DB_ENDPOINT` という 2 つの環境変数が作成されていることに注目してください。 これらの変数には、Azure Cosmos DB インスタンスに接続するために必要な値が保持されています。

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

コンテナーが作成された後、`az container show` コマンドを使用して IP アドレスを取得します。

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query ipAddress.ip --output tsv
```

ブラウザーを開き、コンテナーの IP アドレスに移動します。 次のアプリケーションが表示されます。 投票をキャストすると、投票は、Azure Cosmos DB インスタンスに格納されます。

![猫または犬という 2 つの選択肢がある Azure 投票アプリケーション。](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a>セキュリティで保護された環境変数

上の演習では、コンテナーは 2 つの環境変数に格納されている Azure Cosmos DB に対する接続情報を使用して作成されました。 既定では、環境変数は Azure portal とコマンド ライン ツールにプレーン テキストで表示されます。

たとえば、前の演習で作成したコンテナーに関する情報を `az container show` コマンドで取得する場合、環境変数にはプレーン テキストでアクセスできます。

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
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
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

コンテナーが `az container show` コマンドで返されたとき、環境変数は表示されません。

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo-secure --query containers[0].environmentVariables
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