<span data-ttu-id="745e5-101">仮想マシンのサイズは、予定される作業に適したものであることが必要です。</span><span class="sxs-lookup"><span data-stu-id="745e5-101">Virtual machines must be sized appropriately for the expected work.</span></span> <span data-ttu-id="745e5-102">VM のメモリの大きさや CPU が適切でない場合は、負荷を処理できなくなることや、動作が遅すぎて実用的ではなくなることがあります。</span><span class="sxs-lookup"><span data-stu-id="745e5-102">A VM without the correct amount of memory or CPU will fail under load or run too slowly to be effective.</span></span> 

<span data-ttu-id="745e5-103">仮想マシンを作成するときに、_VM サイズ_値を指定することができます。この値によって、その VM に割り当てられるコンピューティング リソースの量が決まります。</span><span class="sxs-lookup"><span data-stu-id="745e5-103">When you create a virtual machine, you can supply a _VM size_ value that will determine the amount of compute resources that will be devoted to the VM.</span></span> <span data-ttu-id="745e5-104">これには、Azure から仮想マシンに使用できるようにする CPU、GPU、およびメモリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="745e5-104">This includes CPU, GPU, and memory that are made available to the virtual machine from Azure.</span></span>

<span data-ttu-id="745e5-105">Azure では、予想される使用量に基づいて選択するための Linux と Windows 用の[定義済みの VM サイズ](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)のセットが定義されています。</span><span class="sxs-lookup"><span data-stu-id="745e5-105">Azure defines a set of [pre-defined VM sizes](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) for Linux and Windows to choose from based on the expected usage.</span></span> 

| <span data-ttu-id="745e5-106">type</span><span class="sxs-lookup"><span data-stu-id="745e5-106">Type</span></span> | <span data-ttu-id="745e5-107">サイズ</span><span class="sxs-lookup"><span data-stu-id="745e5-107">Sizes</span></span> | <span data-ttu-id="745e5-108">説明</span><span class="sxs-lookup"><span data-stu-id="745e5-108">Description</span></span> |
|------|-------|-------------|
| <span data-ttu-id="745e5-109">汎用</span><span class="sxs-lookup"><span data-stu-id="745e5-109">General purpose</span></span>   | <span data-ttu-id="745e5-110">Dsv3、Dv3、DSv2、Dv2、DS、D、Av2、A0 - 7</span><span class="sxs-lookup"><span data-stu-id="745e5-110">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span> | <span data-ttu-id="745e5-111">CPU とメモリのバランスがとれています。</span><span class="sxs-lookup"><span data-stu-id="745e5-111">Balanced CPU-to-memory.</span></span> <span data-ttu-id="745e5-112">開発/テスト環境や、小中規模のアプリケーションとデータ ソリューションに最適です。</span><span class="sxs-lookup"><span data-stu-id="745e5-112">Ideal for dev/test and small to medium applications and data solutions.</span></span> |
| <span data-ttu-id="745e5-113">コンピューティング最適化</span><span class="sxs-lookup"><span data-stu-id="745e5-113">Compute optimized</span></span> | <span data-ttu-id="745e5-114">Fs、F</span><span class="sxs-lookup"><span data-stu-id="745e5-114">Fs, F</span></span> | <span data-ttu-id="745e5-115">メモリに対する CPU の比が大きくなっています。</span><span class="sxs-lookup"><span data-stu-id="745e5-115">High CPU-to-memory.</span></span> <span data-ttu-id="745e5-116">トラフィックが中程度のアプリケーション、ネットワーク アプライアンス、バッチ処理に適しています。</span><span class="sxs-lookup"><span data-stu-id="745e5-116">Good for medium-traffic applications, network appliances, and batch processes.</span></span> |
| <span data-ttu-id="745e5-117">メモリ最適化</span><span class="sxs-lookup"><span data-stu-id="745e5-117">Memory optimized</span></span>  | <span data-ttu-id="745e5-118">Esv3、Ev3、M、GS、G、DSv2、DS、Dv2、D</span><span class="sxs-lookup"><span data-stu-id="745e5-118">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="745e5-119">コアに対するメモリの比が大きくなっています。</span><span class="sxs-lookup"><span data-stu-id="745e5-119">High memory-to-core.</span></span> <span data-ttu-id="745e5-120">リレーショナル データベース、中から大規模のキャッシュ、およびインメモリ分析に適しています。</span><span class="sxs-lookup"><span data-stu-id="745e5-120">Great for relational databases, medium to large caches, and in-memory analytics.</span></span> |
| <span data-ttu-id="745e5-121">ストレージ最適化</span><span class="sxs-lookup"><span data-stu-id="745e5-121">Storage optimized</span></span> | <span data-ttu-id="745e5-122">Ls</span><span class="sxs-lookup"><span data-stu-id="745e5-122">Ls</span></span> | <span data-ttu-id="745e5-123">高いディスク スループットと IO。</span><span class="sxs-lookup"><span data-stu-id="745e5-123">High disk throughput and IO.</span></span> <span data-ttu-id="745e5-124">ビッグ データ、SQL、および NoSQL のデータベースに最適です。</span><span class="sxs-lookup"><span data-stu-id="745e5-124">Ideal for big data, SQL, and NoSQL databases.</span></span> |
| <span data-ttu-id="745e5-125">GPU 最適化</span><span class="sxs-lookup"><span data-stu-id="745e5-125">GPU optimized</span></span> | <span data-ttu-id="745e5-126">NV、NC</span><span class="sxs-lookup"><span data-stu-id="745e5-126">NV, NC</span></span> | <span data-ttu-id="745e5-127">負荷の大きいグラフィックスのレンダリングやビデオ編集に特化した VM です。</span><span class="sxs-lookup"><span data-stu-id="745e5-127">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span> |
| <span data-ttu-id="745e5-128">高性能</span><span class="sxs-lookup"><span data-stu-id="745e5-128">High performance</span></span> | <span data-ttu-id="745e5-129">H、A8 ～ 11</span><span class="sxs-lookup"><span data-stu-id="745e5-129">H, A8-11</span></span> | <span data-ttu-id="745e5-130">オプションで高スループットのネットワーク インターフェイス (RDMA) を備えた、最も強力な CPU VM です。</span><span class="sxs-lookup"><span data-stu-id="745e5-130">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> | 

<span data-ttu-id="745e5-131">使用可能なサイズは、VM を作成するリージョンに基づいて変わります。</span><span class="sxs-lookup"><span data-stu-id="745e5-131">The available sizes change based on the region you're creating the VM in.</span></span> <span data-ttu-id="745e5-132">`vm list-sizes` コマンドを使用して、使用可能なサイズのリストを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="745e5-132">You can get a list of the available sizes using the `vm list-sizes` command.</span></span> <span data-ttu-id="745e5-133">これを Azure Cloud Shell に入力してみてください。</span><span class="sxs-lookup"><span data-stu-id="745e5-133">Try typing this into Azure Cloud Shell:</span></span>

```azurecli
az vm list-sizes --location eastus --output table
```

<span data-ttu-id="745e5-134">`eastus` の省略形の応答は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="745e5-134">Here's an abbreviated response for `eastus`:</span></span>

```
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          2048  Standard_B1ms                         1           1047552                    4096
                 2          1024  Standard_B1s                          1           1047552                    2048
                 4          8192  Standard_B2ms                         2           1047552                   16384
                 4          4096  Standard_B2s                          2           1047552                    8192
                 8         16384  Standard_B4ms                         4           1047552                   32768
                16         32768  Standard_B8ms                         8           1047552                   65536
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
                16         14336  Standard_DS3_v2                       4           1047552                   28672
                32         28672  Standard_DS4_v2                       8           1047552                   57344
                64         57344  Standard_DS5_v2                      16           1047552                  114688
        ....
                64       3891200  Standard_M128-32ms                  128           1047552                 4096000
                64       3891200  Standard_M128-64ms                  128           1047552                 4096000
                64       3891200  Standard_M128ms                     128           1047552                 4096000
                64       2048000  Standard_M128s                      128           1047552                 4096000
                64       1024000  Standard_M64                         64           1047552                 8192000
                64       1792000  Standard_M64m                        64           1047552                 8192000
                64       2048000  Standard_M128                       128           1047552                16384000
                64       3891200  Standard_M128m                      128           1047552                16384000
```

<span data-ttu-id="745e5-135">VM を作成したときにサイズを指定しなかったので、Azure によって `Standard_DS1_v2`の既定の汎用サイズが選択されました。</span><span class="sxs-lookup"><span data-stu-id="745e5-135">We didn't specify a size when we created our VM - so Azure selected a default general-purpose size for us of `Standard_DS1_v2`.</span></span> <span data-ttu-id="745e5-136">ただし、サイズは `vm create` コマンドの一部として `--size` パラメーターを使用して指定できます。</span><span class="sxs-lookup"><span data-stu-id="745e5-136">However, we can specify the size as part of the `vm create` command using the `--size` parameter.</span></span> <span data-ttu-id="745e5-137">たとえば、次のコマンドを使用して 16 コアの仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="745e5-137">For example, you could use the following command to create a 16-core virtual machine:</span></span>

```azurecli
az vm create --resource-group ExerciseResources --name SampleVM \
  --image Debian --admin-username aldis --generate-ssh-keys --verbose \
  --size "Standard_DS5_v2"
```

> [!WARNING]
> <span data-ttu-id="745e5-138">ご利用のサブスクリプション レベルに応じて、作成できるリソース数に[制限が課され](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits)、さらにそのリソースの合計サイズも制限されます。</span><span class="sxs-lookup"><span data-stu-id="745e5-138">Your subscription tier [enforces limits](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits) on how many resources you can create, as well as the total size of those resources.</span></span> <span data-ttu-id="745e5-139">たとえば、従量課金制サブスクリプションでの仮想 CPU 数の制限は **20** で、無料レベルの場合は **4** となっています。</span><span class="sxs-lookup"><span data-stu-id="745e5-139">For example, you are capped to **20 virtual CPUs** with the pay-as-you-go subscription, and only **4 vCPUs** for a free tier.</span></span> <span data-ttu-id="745e5-140">Azure CLI には、これを超えたときに**クォータ超過**エラーで通知する機能があります。</span><span class="sxs-lookup"><span data-stu-id="745e5-140">The Azure CLI will let you know when you exceed this with a **Quota Exceeded** error.</span></span> <span data-ttu-id="745e5-141">アーキテクチャを作成するときにこのエラーが発生した場合は、有料サブスクリプションに関連付けられている制限の引き上げ (仮想 CPU の上限は 10,000 個) を申請することができます。申請は[オンラインで行います (無料)](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-quota-errors)。</span><span class="sxs-lookup"><span data-stu-id="745e5-141">If you hit this error when creating your architecture, you can request to raise the limits associated with your paid subscription (up to 10,000 vCPUs!) through a [free online request](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-quota-errors).</span></span> 

## <a name="resizing-an-existing-vm"></a><span data-ttu-id="745e5-142">既存の VM のサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="745e5-142">Resizing an existing VM</span></span>
<span data-ttu-id="745e5-143">ワークロードが変化した場合や、作成時に正しくサイズが設定されなかった場合にも、既存の VM のサイズを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="745e5-143">We can also resize an existing VM if the workload changes, or if it was incorrectly sized at creation.</span></span> <span data-ttu-id="745e5-144">サイズ変更を要求する前に、VM が属するクラスター内で目的のサイズが使用できるかどうかを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="745e5-144">Before a resize is requested, we must check to see if the desired size is available in the cluster our VM is part of.</span></span> <span data-ttu-id="745e5-145">これを行うには、`vm list-vm-resize-options` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="745e5-145">We can do this with the `vm list-vm-resize-options` command:</span></span>

```azurecli
az vm list-vm-resize-options --resource-group ExerciseResources --name SampleVM --output table
```

<span data-ttu-id="745e5-146">これにより、リソース グループ内で使用可能なすべてのサイズ構成のリストが返されます。</span><span class="sxs-lookup"><span data-stu-id="745e5-146">This will return a list of all the possible size configurations available in the resource group.</span></span> <span data-ttu-id="745e5-147">目的のサイズがクラスターでは使用できないものの、リージョンでは使用_できる_場合は、[VM を割り当て解除](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate)することができます。</span><span class="sxs-lookup"><span data-stu-id="745e5-147">If the size we want isn't available in our cluster, but _is_ available in the region, we can [deallocate the VM](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate).</span></span> <span data-ttu-id="745e5-148">このコマンドは、実行中の VM を停止し、リソースを失うことなく、現在のクラスターからその VM を削除します。</span><span class="sxs-lookup"><span data-stu-id="745e5-148">This command will stop the running VM and remove it from the current cluster without losing any resources.</span></span> <span data-ttu-id="745e5-149">その後、VM のサイズを変更できます。つまり、目的のサイズ構成が使用できる新しいクラスター内に VM を再作成します。</span><span class="sxs-lookup"><span data-stu-id="745e5-149">Then we can resize it, which will re-create the VM in a new cluster where the size configuration is available.</span></span>

<span data-ttu-id="745e5-150">VM のサイズを変更するには、`vm resize` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="745e5-150">To resize a VM, we use the `vm resize` command.</span></span> <span data-ttu-id="745e5-151">たとえば、現在の VM リソースを、必要最小限の 2 G のメモリ、1 CPU コア、4 G のディスク領域に削減してみましょう。</span><span class="sxs-lookup"><span data-stu-id="745e5-151">For example, let's reduce our current VM resources to the bare minimum: 2G of memory, 1 CPU core, and 4G of disk space.</span></span> <span data-ttu-id="745e5-152">Cloud Shell に次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="745e5-152">Type this command in Cloud Shell:</span></span>

```azurecli
az vm resize --resource-group ExerciseResources --name SampleVM --size "Standard_B1ms"
```

<span data-ttu-id="745e5-153">このコマンドで VM のリソースを削減するには数分かかります。完了すると、新しい JSON 構成が返されます。</span><span class="sxs-lookup"><span data-stu-id="745e5-153">This command will take a few minutes to reduce the resources of the VM, and once it's done, it will return a new JSON configuration.</span></span>