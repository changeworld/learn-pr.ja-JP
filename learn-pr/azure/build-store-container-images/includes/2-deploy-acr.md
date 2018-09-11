<span data-ttu-id="0843f-101">このユニットでは、Azure CLI を使用して Azure Container Registry を作成します。</span><span class="sxs-lookup"><span data-stu-id="0843f-101">In this unit, you will create an Azure Container Registry using the Azure CLI.</span></span>

## <a name="create-an-azure-container-registry"></a><span data-ttu-id="0843f-102">Azure Container Registry を作成する</span><span class="sxs-lookup"><span data-stu-id="0843f-102">Create an Azure Container Registry</span></span>

<span data-ttu-id="0843f-103">Azure Container Registry (ACR) を作成する前に、そのデプロイ先となる*リソース グループ*が必要です。</span><span class="sxs-lookup"><span data-stu-id="0843f-103">Before you create your Azure Container Registry (ACR), you need a *resource group* to deploy it to.</span></span> <span data-ttu-id="0843f-104">リソース グループは、Azure リソースをまとめてデプロイして管理するための論理上のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="0843f-104">A resource group is a logical collection into which all Azure resources are deployed and managed.</span></span>

<span data-ttu-id="0843f-105">`az group create` コマンドでリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="0843f-105">Create a resource group with the `az group create` command.</span></span> <span data-ttu-id="0843f-106">次の例では、*myResourceGroup* という名前のリソース グループが *eastus* リージョンに作成されます。</span><span class="sxs-lookup"><span data-stu-id="0843f-106">In the following example, a resource group named *myResourceGroup* is created in the *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="0843f-107">リソース グループを作成したら、`az acr create` コマンドで Azure Container Registry を作成します。</span><span class="sxs-lookup"><span data-stu-id="0843f-107">Once you've created the resource group, create an Azure container registry with the `az acr create` command.</span></span> <span data-ttu-id="0843f-108">コンテナー レジストリ名は、Azure 内で一意にする必要があります。また、5 から 50 文字の英数字を含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="0843f-108">The container registry name must be unique within Azure, and contain 5-50 alphanumeric characters.</span></span> <span data-ttu-id="0843f-109">`<acrName>` を、レジストリの一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="0843f-109">Replace `<acrName>` with a unique name for your registry.</span></span>

<span data-ttu-id="0843f-110">この例では、Premium レジストリ SKU をデプロイします。</span><span class="sxs-lookup"><span data-stu-id="0843f-110">For this example, a premium registry SKU is deployed.</span></span> <span data-ttu-id="0843f-111">Premium SKU は、geo レプリケーションに必要です。</span><span class="sxs-lookup"><span data-stu-id="0843f-111">The premium SKU is required for geo-replication.</span></span> <span data-ttu-id="0843f-112">Container Registry SKU の詳細については、「[Azure Container Registry SKUs](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus)」 (Azure Container Registry SKU) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0843f-112">For more information on Container Registry SKUs, see, [Azure Container Registry SKUs](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus)</span></span>

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Premium
```

<span data-ttu-id="0843f-113">新しい Azure Container Registry の出力例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0843f-113">Here's example output for a new Azure container registry.</span></span>

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

<span data-ttu-id="0843f-114">このモジュールの残りの部分では、この手順で選択したコンテナー レジストリ名のプレースホルダーとして `<acrName>` を使用します。</span><span class="sxs-lookup"><span data-stu-id="0843f-114">The rest of this module refers to `<acrName>` as a placeholder for the container registry name that you chose in this step.</span></span>

## <a name="summary"></a><span data-ttu-id="0843f-115">まとめ</span><span class="sxs-lookup"><span data-stu-id="0843f-115">Summary</span></span>

<span data-ttu-id="0843f-116">このユニットでは、Azure CLI を使用して Azure Container Registry を作成しました。</span><span class="sxs-lookup"><span data-stu-id="0843f-116">In this unit, an Azure Container Registry was created using the Azure CLI.</span></span> <span data-ttu-id="0843f-117">次のユニットでは、Azure Container Registry を使用してコンテナー イメージを作成し、このイメージをコンテナー レジストリに格納します。</span><span class="sxs-lookup"><span data-stu-id="0843f-117">In the next unit, you will use Azure Container Registry to create a container image and store this image in the container registry.</span></span>