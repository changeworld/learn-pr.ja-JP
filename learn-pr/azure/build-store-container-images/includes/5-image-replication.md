<span data-ttu-id="df5ed-101">ベスト プラクティスとして、イメージが実行されている各リージョンにコンテナー レジストリを配置してネットワーク上の近い場所での操作を可能にすることで、高速で信頼性の高いイメージ レイヤー転送を実現します。</span><span class="sxs-lookup"><span data-stu-id="df5ed-101">As a best practice, placing a container registry in each region where images are run allows network-close operations, enabling fast, reliable image layer transfers.</span></span> <span data-ttu-id="df5ed-102">geo レプリケーションにより、Azure コンテナー レジストリが単一のレジストリとして機能することが可能になり、マルチマスター リージョン レジストリで複数のリージョンに対応できます。</span><span class="sxs-lookup"><span data-stu-id="df5ed-102">Geo-replication enables an Azure container registry to function as a single registry, serving multiple regions with multi-master regional registries.</span></span>

<span data-ttu-id="df5ed-103">geo レプリケートされたレジストリには次の利点があります。</span><span class="sxs-lookup"><span data-stu-id="df5ed-103">A geo-replicated registry provides the following benefits:</span></span>

* <span data-ttu-id="df5ed-104">複数のリージョンで 1 つのレジストリ/イメージ/タグ名を使用できる</span><span class="sxs-lookup"><span data-stu-id="df5ed-104">Single registry/image/tag names can be used across multiple regions</span></span>
* <span data-ttu-id="df5ed-105">リージョン デプロイからネットワーク上の近い場所のレジストリにアクセスできる</span><span class="sxs-lookup"><span data-stu-id="df5ed-105">Network-close registry access from regional deployments</span></span>
* <span data-ttu-id="df5ed-106">コンテナー ホストと同じリージョンにあるレプリケートされたローカルのレジストリからイメージがプルされるので、追加のエグレス料金が発生しない</span><span class="sxs-lookup"><span data-stu-id="df5ed-106">No additional egress fees because images are pulled from a local, replicated registry in the same region as your container host</span></span>
* <span data-ttu-id="df5ed-107">複数のリージョンにまたがってレジストリを一元管理できる</span><span class="sxs-lookup"><span data-stu-id="df5ed-107">Single management of a registry across multiple regions</span></span>

## <a name="replicate-image-to-multiple-locations"></a><span data-ttu-id="df5ed-108">イメージを複数の場所にレプリケートする</span><span class="sxs-lookup"><span data-stu-id="df5ed-108">Replicate image to multiple locations</span></span>

<span data-ttu-id="df5ed-109">`az acr replication create` コマンドを使用して、コンテナー イメージを別のリージョンにレプリケートします。</span><span class="sxs-lookup"><span data-stu-id="df5ed-109">Use the `az acr replication create` command to replicate your container images to another region.</span></span> <span data-ttu-id="df5ed-110">この例では、`japaneast` リージョン用のレプリケーションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="df5ed-110">In this example, a replication is created for the `japaneast` region.</span></span> <span data-ttu-id="df5ed-111">`<acrName>` を、コンテナー レジストリの名前で更新します。</span><span class="sxs-lookup"><span data-stu-id="df5ed-111">Update `<acrName>` with the name of your container registry.</span></span>

```azurecli
az acr replication create --registry <acrName> --location japaneast
```

<span data-ttu-id="df5ed-112">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="df5ed-112">The output should look similar to the following:</span></span>

```console
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

<span data-ttu-id="df5ed-113">すべてのコンテナー イメージ レプリカの一覧を表示するには、`az acr replication list` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="df5ed-113">To see a list of all container image replicas, use the `az acr replication list` command.</span></span> <span data-ttu-id="df5ed-114">`<acrName>` を、コンテナー レジストリの名前で更新します。</span><span class="sxs-lookup"><span data-stu-id="df5ed-114">Update `<acrName>` with the name of your container registry.</span></span>

```azurecli
az acr replication list --registry <acrName> --output table
```

<span data-ttu-id="df5ed-115">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="df5ed-115">The output should look similar to the following:</span></span>

```console
NAME       LOCATION    PROVISIONING STATE    STATUS
---------  ----------  --------------------  --------
japaneast  japaneast   Succeeded             Ready
eastus     eastus      Succeeded             Ready
```

<span data-ttu-id="df5ed-116">この演習では Azure Portal を使用しませんでしたが、Portal のエクスペリエンスを確認しておくとよいでしょう。</span><span class="sxs-lookup"><span data-stu-id="df5ed-116">While the Azure portal was not used in this exercise, it is neat to see the portal experience.</span></span>

<span data-ttu-id="df5ed-117">Azure Portal 内から、Azure コンテナー レジストリに対して `Replications` を選択すると、現在のレプリケーションの詳細を示すマップが表示されます。</span><span class="sxs-lookup"><span data-stu-id="df5ed-117">From within the Azure portal, selecting `Replications` for an Azure container registry displays a map that details current replications.</span></span> <span data-ttu-id="df5ed-118">マップ上のリージョンを選択すると、追加のリージョンにコンテナー イメージをレプリケートできます。</span><span class="sxs-lookup"><span data-stu-id="df5ed-118">Container images can be replicated to additional regions by selecting the regions on the map.</span></span>

![Azure Portal に表示されるコンテナー レプリケーション マップ](../media/replication-map.png)

## <a name="clean-up"></a><span data-ttu-id="df5ed-120">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="df5ed-120">Clean up</span></span>

<span data-ttu-id="df5ed-121">これは、Azure Container Registry のラーニング モジュールの最後のユニットです。</span><span class="sxs-lookup"><span data-stu-id="df5ed-121">This is the last unit of the Azure Container Registry learning module.</span></span> <span data-ttu-id="df5ed-122">この時点で、リソース グループを削除して、作成したリソースをクリーンアップすることができます。</span><span class="sxs-lookup"><span data-stu-id="df5ed-122">At this point, you can clean up the created resources by deleting the resource group.</span></span> <span data-ttu-id="df5ed-123">そのためには、`az group delete` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="df5ed-123">To do so, use the `az group delete` command.</span></span>

```azurecli
az group delete --name myResourceGroup --no-wait
```

## <a name="summary"></a><span data-ttu-id="df5ed-124">まとめ</span><span class="sxs-lookup"><span data-stu-id="df5ed-124">Summary</span></span>

<span data-ttu-id="df5ed-125">このモジュールでは、Azure CLI を使用して、コンテナー イメージを複数の Azure リージョンにレプリケートしました。</span><span class="sxs-lookup"><span data-stu-id="df5ed-125">In this module, you replicated a container image to multiple Azure regions using the Azure CLI.</span></span>