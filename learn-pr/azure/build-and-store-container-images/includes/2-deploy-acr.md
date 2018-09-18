このユニットでは、Azure CLI を使用して Azure コンテナー レジストリを作成します。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a>Azure コンテナー レジストリの作成

作業は無料のサンド ボックスで行うため、自分のリソース グループを作成する必要はありません。 `az acr create` コマンドを使用して Azure コンテナー レジストリを作成します。 コンテナー レジストリ名は、Azure 内で一意にする必要があります。また、5 から 50 文字の英数字を含める必要があります。 `<acrName>` を、レジストリの一意の名前に置き換えます。

この例では、Premium レジストリ SKU をデプロイします。 Premium SKU は、geo レプリケーションに必須です。 Cloud Shell エディターにこのコマンドを入力します。

```azurecli
az acr create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <acrName> --sku Premium
```

新しい Azure コンテナー レジストリの出力例を次に示します。

```output
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