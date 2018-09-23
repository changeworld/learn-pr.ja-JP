<span data-ttu-id="62da7-101">このユニットでは、Azure CLI を使用して Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="62da7-101">In this unit, you will create an Azure container registry using the Azure CLI.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]
 
## <a name="create-an-azure-container-registry"></a><span data-ttu-id="62da7-102">Azure コンテナー レジストリを作成する</span><span class="sxs-lookup"><span data-stu-id="62da7-102">Create an Azure container registry</span></span>

<span data-ttu-id="62da7-103">作業は無料のサンドボックスで行うため、自分のリソース グループを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="62da7-103">We'll be working in a free Sandbox, so there's no need to create your own Resource Group.</span></span> <span data-ttu-id="62da7-104">`az acr create` コマンドを使用して Azure コンテナー レジストリを作成します。</span><span class="sxs-lookup"><span data-stu-id="62da7-104">Create an Azure container registry with the `az acr create` command.</span></span> <span data-ttu-id="62da7-105">コンテナー レジストリ名は、Azure 内で一意にする必要があります。また、5 から 50 文字の英数字を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="62da7-105">The container registry name must be unique within Azure and contain between 5 and 50 alphanumeric characters.</span></span> <span data-ttu-id="62da7-106">`<acrName>` を、レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="62da7-106">Replace `<acrName>` with a unique name for your registry.</span></span>

<span data-ttu-id="62da7-107">この例では、Premium レジストリ SKU がデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="62da7-107">In this example, a premium registry SKU is deployed.</span></span> <span data-ttu-id="62da7-108">Premium SKU は、geo レプリケーションに必要です。</span><span class="sxs-lookup"><span data-stu-id="62da7-108">The premium SKU is required for geo-replication.</span></span> <span data-ttu-id="62da7-109">次のコマンドを Cloud Shell エディターに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="62da7-109">Enter the following command into the cloud shell editor.</span></span>

```azurecli
az acr create --resource-group <rgn>[Sandbox resource group name]</rgn> --name <acrName> --sku Premium
```

<span data-ttu-id="62da7-110">新しい Azure コンテナー レジストリの出力例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="62da7-110">Here's an example output for a new Azure container registry:</span></span>

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

<span data-ttu-id="62da7-111">このモジュールの残りの部分では、この手順で選択したコンテナー レジストリ名のプレースホルダーとして `<acrName>` を使用します。</span><span class="sxs-lookup"><span data-stu-id="62da7-111">The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.</span></span>

<span data-ttu-id="62da7-112">このユニットでは、Azure CLI を使用して Azure コンテナー レジストリを作成しました。</span><span class="sxs-lookup"><span data-stu-id="62da7-112">In this unit, you created an Azure container registry using the Azure CLI.</span></span>