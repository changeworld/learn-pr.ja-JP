このユニットでは、Azure CLI を使用して Azure コンテナー レジストリを作成します。

## <a name="create-an-azure-container-registry"></a>Azure コンテナー レジストリの作成

Azure コンテナー レジストリを作成する前に、そのデプロイ先となる*リソース グループ*が必要です。 リソース グループは、Azure リソースをまとめてデプロイして管理するための論理上のコレクションです。

`az group create` コマンドでリソース グループを作成します。 次の例では、*myResourceGroup* という名前のリソース グループが *eastus* リージョンに作成されます。

```azurecli
az group create --name myResourceGroup --location eastus
```

リソース グループを作成したら、`az acr create` コマンドで Azure Container Registry を作成します。 コンテナー レジストリ名は、Azure 内で一意にする必要があります。また、5 から 50 文字の英数字を含める必要があります。 `<acrName>` を、レジストリの一意の名前に置き換えます。

この例では、Premium レジストリ SKU をデプロイします。 Premium SKU は、geo レプリケーションに必要です。 Container Registry SKU の詳細については、「[Azure Container Registry SKUs](https://docs.microsoft.com/azure/container-registry/container-registry-skus)」 (Azure Container Registry SKU) を参照してください。

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Premium
```

新しい Azure コンテナー レジストリの出力例を次に示します。

```console
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-15T19:19:07.042178+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myACR0007",
  "location": "eastus",
  "loginServer": "myacr.azurecr.io",
  "name": "myACR",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

このモジュールの残りの部分では、この手順で選択したコンテナー レジストリ名のプレースホルダーとして `<acrName>` を使用します。

## <a name="summary"></a>まとめ

このユニットでは、Azure CLI を使用して Azure コンテナー レジストリを作成しました。