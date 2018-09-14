Container Instances で環境変数を設定すると、コンテナーによって実行されるアプリケーションまたはスクリプトの動的な構成を提供できます。 環境変数の設定は、コンテナーの作成時に Azure CLI、PowerShell、または Azure portal を使用して行います。 コンテナー操作の出力に機密情報が表示されるのを防ぐには、セキュリティで保護された環境変数を使います。

このユニットでは、Azure Cosmos DB インスタンスが作成され、その後に Azure コンテナー インスタンスが環境変数として格納された Azure Cosmos DB インスタンスの接続情報を使って実行されます。 コンテナー内のアプリケーションは、Azure Cosmos DB からのデータの読み書きに変数を使用します。 このユニットでは、環境変数と、セキュアな環境変数の双方を作成します。

## <a name="deploy-azure-cosmos-db"></a>Azure Cosmos DB の展開

`az Azure Cosmos DB create` コマンドを使用して Azure Cosmos DB インスタンスを作成します。 また、この例では、Azure Cosmos DB のエンドポイント アドレス *COSMOS_DB_ENDPOINT* という名前の変数に設定します。

このコマンドの完了には数分を要することがあります。

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query documentEndpoint -o tsv)
```

次に、`az cosmosdb list-keys` のコマンドを使って Azure Cosmos DB 接続キーを取得し、*COSMOS_DB_MASTERKEY* という名の変数内に保存します。

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a>コンテナー インスタンスの展開

`az container create` のコマンドを使って Azure コンテナー インスタンスを作成します。 2 つの環境変数の値 `COSMOS_DB_ENDPOINT` と `COSMOS_DB_ENDPOINT` が作成されたことを確認します。 これらの変数は、Azure Cosmos DB への接続に必要な値を保持します。

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

コンテナーが作成されたら、`az container show` のコマンドを使って IP アドレスを取得します。

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query ipAddress.ip --output tsv
```

ブラウザーを開き、コンテナーの IP アドレスに移動します。 次のアプリケーションが表示されます。 投票を行うと、投票は Azure Cosmos DB インスタンス内に保存されます。

![猫または犬という 2 つの選択肢がある Azure 投票アプリケーション。](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a>セキュリティで保護された環境変数

上の演習では、コンテナーは 2 つの環境変数に格納されている Azure Cosmos DB に対する接続情報を使用して作成されました。 デフォルトでは、環境変数は Azure portal およびコマンドライン ツールに平文で表示されます。

たとえば、`az container show` のコマンドを使って前回の練習で作成されたコンテナーに関する情報を取得する場合、環境変数は平文でアクセス可能となります。

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

次の例を実行して、セキュリティで保護された環境変数を利用する *aci-demo-secure* という名前のコンテナーを作成します。

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

これで、`az container show` のコマンドを使ってコンテナーが返される際、環境変数は表示されなくなります。

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

## <a name="summary"></a>まとめ

このユニットでは、Azure Cosmos DB インスタンスおよび Azure コンテナー インスタンスの作成を行いました。 環境変数はコンテナー インスタンス内にセットされ、コンテナー内で実行されるアプリケーションが Azure Cosmos DB インスタンスにアクセスできるようになっていました。

次のユニットでは、データ保持のために Azure コンテナー インスタンスへデータ ボリュームをマウントする方法を紹介します。
