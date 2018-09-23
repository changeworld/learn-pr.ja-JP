<span data-ttu-id="65dc3-101">あなたの会社で、分散顧客ベースを提供するローカル プレゼンスを確実に確保できるように、コンピューティング ワークロードが複数のリージョンにデプロイされると想定します。</span><span class="sxs-lookup"><span data-stu-id="65dc3-101">Suppose your company has compute workloads deployed to several regions to make sure you have a local presence to serve your distributed customer base.</span></span> 

<span data-ttu-id="65dc3-102">イメージが実行される各リージョンにコンテナー レジストリを配置することを目標とします。</span><span class="sxs-lookup"><span data-stu-id="65dc3-102">Your aim is to place a container registry in each region where images are run.</span></span> <span data-ttu-id="65dc3-103">この戦略により、ネットワーク上の近い場所での操作が実現され、高速で信頼性の高いイメージ レイヤー転送が可能になります。</span><span class="sxs-lookup"><span data-stu-id="65dc3-103">This strategy will allow for network-close operations, enabling fast, reliable image layer transfers.</span></span> 

<span data-ttu-id="65dc3-104">geo レプリケーションにより、Azure コンテナー レジストリが単一のレジストリとして機能することが可能になり、マルチマスター リージョン レジストリでいくつかのリージョンに対応できます。</span><span class="sxs-lookup"><span data-stu-id="65dc3-104">Geo-replication enables an Azure container registry to function as a single registry, serving several regions with multi-master regional registries.</span></span>

<span data-ttu-id="65dc3-105">geo レプリケートされたレジストリには次の利点があります。</span><span class="sxs-lookup"><span data-stu-id="65dc3-105">A geo-replicated registry provides the following benefits:</span></span>

- <span data-ttu-id="65dc3-106">複数のリージョンで 1 つのレジストリ/イメージ/タグ名を使用できる</span><span class="sxs-lookup"><span data-stu-id="65dc3-106">Single registry/image/tag names can be used across multiple regions</span></span>
- <span data-ttu-id="65dc3-107">リージョン デプロイからネットワーク上の近い場所のレジストリにアクセスできる</span><span class="sxs-lookup"><span data-stu-id="65dc3-107">Network-close registry access from regional deployments</span></span>
- <span data-ttu-id="65dc3-108">コンテナー ホストと同じリージョンにあるレプリケートされたローカルのレジストリからイメージがプルされるので、追加のエグレス料金が発生しない</span><span class="sxs-lookup"><span data-stu-id="65dc3-108">No additional egress fees, as images are pulled from a local, replicated registry in the same region as your container host</span></span>
- <span data-ttu-id="65dc3-109">複数のリージョンにまたがってレジストリを一元管理できる</span><span class="sxs-lookup"><span data-stu-id="65dc3-109">Single management of a registry across multiple regions</span></span>

## <a name="replicate-an-image-to-multiple-locations"></a><span data-ttu-id="65dc3-110">イメージを複数の場所にレプリケートする</span><span class="sxs-lookup"><span data-stu-id="65dc3-110">Replicate an image to multiple locations</span></span>

<span data-ttu-id="65dc3-111">`az acr replication create` Azure CLI コマンド を使用して、ご利用のコンテナー イメージをリージョン間でレプリケートします。</span><span class="sxs-lookup"><span data-stu-id="65dc3-111">You'll use the `az acr replication create` Azure CLI command to replicate your container images from one region to another.</span></span> <span data-ttu-id="65dc3-112">この例では、`japaneast` リージョン用のレプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="65dc3-112">In this example, you'll create a replication for the `japaneast` region.</span></span> <span data-ttu-id="65dc3-113">`<acrName>` を、ご利用のコンテナー レジストリの名前で更新します。</span><span class="sxs-lookup"><span data-stu-id="65dc3-113">Update `<acrName>` with the name of your Container Registry.</span></span>

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

<span data-ttu-id="65dc3-114">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="65dc3-114">The output should look similar to the following:</span></span>

```output
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myACR0007/replications/japaneast",
  "location": "japaneast",
  "name": "japaneast",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": {
    "displayStatus": "Syncing",
    "message": null,
    "timestamp": "2018-08-15T20:22:09.275792+00:00"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries/replications"
}
```

<span data-ttu-id="65dc3-115">最後の手順として、作成されたコンテナー イメージのレプリカをすべて取得することができます。</span><span class="sxs-lookup"><span data-stu-id="65dc3-115">As a final step, you're able to retrieve all container image replicas created.</span></span> <span data-ttu-id="65dc3-116">`az acr replication list` コマンドを使用して、この一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="65dc3-116">You'll use the `az acr replication list` command to retrieve this list.</span></span> <span data-ttu-id="65dc3-117">`<acrName>` を、ご利用のコンテナー レジストリの名前で更新します。</span><span class="sxs-lookup"><span data-stu-id="65dc3-117">Update `<acrName>` with the name of your Container Registry.</span></span>

```azurecli
az acr replication list --registry <acrName> --output table
```

<span data-ttu-id="65dc3-118">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="65dc3-118">The output should look similar to the following:</span></span>

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

<span data-ttu-id="65dc3-119">ご利用のイメージ レプリカを一覧表示する方法は、Azure CLI だけではないことに留意してください。</span><span class="sxs-lookup"><span data-stu-id="65dc3-119">Keep in mind that you are not limited to the Azure CLI to list your image replicas.</span></span> <span data-ttu-id="65dc3-120">Azure portal 内から、Azure Container Registry に対して `Replications` を選択すると、現在のレプリケーションの詳細を示すマップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="65dc3-120">From within the Azure portal, selecting `Replications` for an Azure Container Registry displays a map that details current replications.</span></span> <span data-ttu-id="65dc3-121">マップ上のリージョンを選択すると、追加のリージョンにコンテナー イメージをレプリケートできます。</span><span class="sxs-lookup"><span data-stu-id="65dc3-121">Container images can be replicated to additional regions by selecting the regions on the map.</span></span>

![Azure portal に表示されるコンテナー レプリケーション マップ](../media/replication-map.png)

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]
 

## <a name="summary"></a><span data-ttu-id="65dc3-123">まとめ</span><span class="sxs-lookup"><span data-stu-id="65dc3-123">Summary</span></span>

<span data-ttu-id="65dc3-124">これで、Azure CLI を使用して、コンテナー イメージが複数の Azure データセンターに正常にレプリケートされました。</span><span class="sxs-lookup"><span data-stu-id="65dc3-124">You've now successfully replicated a container image to multiple Azure datacenters using the Azure CLI.</span></span> 