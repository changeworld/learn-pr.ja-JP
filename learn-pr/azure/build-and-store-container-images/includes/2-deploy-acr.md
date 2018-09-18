<span data-ttu-id="d6011-101">このユニットでは、Azure CLI を使用して Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d6011-101">In this unit, you will create an Azure container registry using the Azure CLI.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a><span data-ttu-id="d6011-102">Azure コンテナー レジストリの作成</span><span class="sxs-lookup"><span data-stu-id="d6011-102">Create an Azure container registry</span></span>

<span data-ttu-id="d6011-103">作業は無料のサンド ボックスで行うため、自分のリソース グループを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d6011-103">We'll be working in the free sandbox, so no need to create your own Resource Group.</span></span> <span data-ttu-id="d6011-104">`az acr create` コマンドを使用して Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d6011-104">Create an Azure container registry with the `az acr create` command.</span></span> <span data-ttu-id="d6011-105">コンテナー レジストリ名は、Azure 内で一意にする必要があります。また、5 から 50 文字の英数字を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6011-105">The container registry name must be unique within Azure, and contain between 5 and 50 alphanumeric characters.</span></span> <span data-ttu-id="d6011-106">`<acrName>` を、レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="d6011-106">Replace `<acrName>` with a unique name for your registry.</span></span>

<span data-ttu-id="d6011-107">この例では、Premium レジストリ SKU をデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d6011-107">For this example, a premium registry SKU is deployed.</span></span> <span data-ttu-id="d6011-108">Premium SKU は、geo レプリケーションに必須です。</span><span class="sxs-lookup"><span data-stu-id="d6011-108">The premium SKU is required for geo-replication.</span></span> <span data-ttu-id="d6011-109">Cloud Shell エディターにこのコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d6011-109">Enter this command into the cloud shell editor.</span></span>

```azurecli
az acr create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <acrName> --sku Premium
```

<span data-ttu-id="d6011-110">新しい Azure コンテナー レジストリの出力例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="d6011-110">Here's example output for a new Azure container registry:</span></span>

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

<span data-ttu-id="d6011-111">このモジュールの残りの部分では、この手順で選択したコンテナー レジストリ名のプレースホルダーとして `<acrName>` を使用します。</span><span class="sxs-lookup"><span data-stu-id="d6011-111">The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.</span></span>

## <a name="summary"></a><span data-ttu-id="d6011-112">まとめ</span><span class="sxs-lookup"><span data-stu-id="d6011-112">Summary</span></span>

<span data-ttu-id="d6011-113">このユニットでは、Azure CLI を使用して Azure コンテナー レジストリを作成しました。</span><span class="sxs-lookup"><span data-stu-id="d6011-113">In this unit, you created an Azure container registry using the Azure CLI.</span></span>