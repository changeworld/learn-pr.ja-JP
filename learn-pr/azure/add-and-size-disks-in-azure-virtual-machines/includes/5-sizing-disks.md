<span data-ttu-id="2f131-101">Azure では、Azure Storage アカウントにページ BLOB として VHD イメージが格納されます。</span><span class="sxs-lookup"><span data-stu-id="2f131-101">Azure stores your VHD images as page blobs in an Azure Storage account.</span></span> <span data-ttu-id="2f131-102">マネージド ディスクの場合、ストレージの管理は Azure で自動的に行われます。これが、マネージド ディスクを選ぶ最大の理由の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="2f131-102">With managed disks, Azure takes care of managing the storage on your behalf - it's one of the best reasons to choose managed disks.</span></span>

<span data-ttu-id="2f131-103">VM を作成するときに、OS ディスクのサイズが選択されます。</span><span class="sxs-lookup"><span data-stu-id="2f131-103">When you create the VM, it chooses a size for the OS disk.</span></span> <span data-ttu-id="2f131-104">特定のサイズは、選択するイメージに基づきます。</span><span class="sxs-lookup"><span data-stu-id="2f131-104">The specific size is based on the image you select.</span></span> <span data-ttu-id="2f131-105">Linux では、多くの場合、30 GB 前後で、Windows では約 127 GB となります。</span><span class="sxs-lookup"><span data-stu-id="2f131-105">On Linux, it's often around 30 GB, and on Windows about 127 GB.</span></span>

<span data-ttu-id="2f131-106">データ ディスクを追加して記憶域スペースを増やすことはできますが、既存のディスクを拡張する必要がある場合もあります。レガシ アプリケーションではそのデータを複数のドライブに分割できないか、あるいは物理 PC のドライブを Azure に移行し、より大きな OS ドライブが必要になる可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="2f131-106">You can add data disks to provide for additional storage space, but you may also wish to expand an existing disk - perhaps a legacy application cannot split its data across drives, or you are migrating a physical PC's drive to Azure and need a larger OS drive.</span></span>

> [!NOTE]
> <span data-ttu-id="2f131-107">_より大きい_サイズへのディスクのサイズ変更のみを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2f131-107">You can only resize a disk to a _larger_ size.</span></span> <span data-ttu-id="2f131-108">現在、マネージド ディスクの縮小はサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="2f131-108">Shrinking managed disks is not supported today.</span></span>

<span data-ttu-id="2f131-109">ディスクのサイズを変更することで、ディスクのレベルを (たとえば、P10 から P20 に) 変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="2f131-109">Changing the size of the disk can also change the level of the disk (for example from P10 to P20).</span></span> <span data-ttu-id="2f131-110">注意点 - これはパフォーマンスのアップグレードには役立ちますが、Premium レベルを上げると、コストも上がります。</span><span class="sxs-lookup"><span data-stu-id="2f131-110">Keep this in mind - this can be beneficial for performance upgrades, but will also cost more as you move up the premium tiers.</span></span>

## <a name="vm-size-vs-disk-size"></a><span data-ttu-id="2f131-111">VM サイズとディスク サイズ</span><span class="sxs-lookup"><span data-stu-id="2f131-111">VM size vs. Disk size</span></span>

<span data-ttu-id="2f131-112">VM の作成時に選択する VM サイズによって、割り当てることができるリソースの数が決まります。</span><span class="sxs-lookup"><span data-stu-id="2f131-112">The VM size you choose when you create your VM will determine how many resources it can allocate.</span></span> <span data-ttu-id="2f131-113">ストレージの場合、サイズによって、VM に追加できるディスクの数と各ディスクの最大サイズが制御されます。</span><span class="sxs-lookup"><span data-stu-id="2f131-113">For storage, the size will control the number of disks you can add to the VM and the max size of each disk.</span></span> 

<span data-ttu-id="2f131-114">前のユニットで説明したとおり、一部の VM サイズでは (I/O のパフォーマンスが制限される) Standard Storage ドライブのみがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="2f131-114">As mentioned in the previous unit, some VM sizes only support Standard storage drives - limiting the I/O performance.</span></span>

<span data-ttu-id="2f131-115">ご利用の VM サイズで許容される量より多いストレージが必要と思われる場合は、VM サイズを変更できます。</span><span class="sxs-lookup"><span data-stu-id="2f131-115">If you find that you need more storage than what your VM size allows for, you can change the VM size.</span></span> <span data-ttu-id="2f131-116">そのトピックは「**Azure Virtual Machines の概要**」モジュールに含まれています。</span><span class="sxs-lookup"><span data-stu-id="2f131-116">We cover that topic in the **Introduction to Azure Virtual Machines** module.</span></span>

## <a name="expanding-a-disk-using-the-azure-cli"></a><span data-ttu-id="2f131-117">Azure CLI を使用するディスクの拡張</span><span class="sxs-lookup"><span data-stu-id="2f131-117">Expanding a disk using the Azure CLI</span></span>

> [!WARNING]
> <span data-ttu-id="2f131-118">ディスクのサイズ変更操作を実行する前に、常にデータをバックアップしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="2f131-118">Always make sure that you back up your data before performing disk resize operations!</span></span>

<span data-ttu-id="2f131-119">VHD に対する操作は、実行中の VM で行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="2f131-119">Operations on VHDs cannot be performed with the VM running.</span></span> <span data-ttu-id="2f131-120">まず、`az vm deallocate` を使用して VM の名前とリソース グループの名前を指定し、VM を停止して割り当てを解除します。</span><span class="sxs-lookup"><span data-stu-id="2f131-120">The first step is to stop and deallocate the VM with `az vm deallocate` supplying the VM name and resource group name.</span></span>

<span data-ttu-id="2f131-121">VM を単に_停止する_場合とは異なり、VM の割り当てを解除すると、関連付けられているコンピューティング リソースが解放され、仮想化されたハードウェアの構成変更を Azure でできるようになります。</span><span class="sxs-lookup"><span data-stu-id="2f131-121">Deallocating a VM, unlike just _stopping_ a VM releases the associated computing resources and allows Azure to make configuration changes to the virtualized hardware.</span></span>

```azurecli
az vm deallocate --resource-group <resource-group-name> --name <vm-name>
```

<span data-ttu-id="2f131-122">次に、ディスクのサイズを変更するために、`az disk update` を使用して、ディスク名、リソース グループ名、新たに要求されたサイズを渡します。</span><span class="sxs-lookup"><span data-stu-id="2f131-122">Next, to resize a disk, you use `az disk update`, passing the disk name, resource group name, and newly requested size.</span></span> <span data-ttu-id="2f131-123">マネージド ディスクを拡張すると、指定されたサイズがマネージド ディスクの最も近いサイズにマップされます。</span><span class="sxs-lookup"><span data-stu-id="2f131-123">When you expand a managed disk, the specified size is mapped to the nearest managed disk size.</span></span>

```azurecli
az disk update \
    --resource-group <resource-group-name> \
    --name <disk-name> \
    --size-gb 200
```

<span data-ttu-id="2f131-124">最後に、`az vm start` を使用して、再度 VM を起動します。</span><span class="sxs-lookup"><span data-stu-id="2f131-124">Finally, you start the VM again with `az vm start`:</span></span>

```azurecli
az vm start --resource-group <resource-group-name> --name <vm-name>
```

## <a name="expanding-a-disk-using-the-azure-portal"></a><span data-ttu-id="2f131-125">Azure portal を使用するディスクの拡張</span><span class="sxs-lookup"><span data-stu-id="2f131-125">Expanding a disk using the Azure portal</span></span>

<span data-ttu-id="2f131-126">Azure portal を使用するディスクの拡張はさらに簡単です。</span><span class="sxs-lookup"><span data-stu-id="2f131-126">Expanding a disk using the Azure portal is even easier.</span></span>

1. <span data-ttu-id="2f131-127">VM の **[概要]** ビューのツール バーにある **[停止]** ボタンを使用して、VM を停止します。</span><span class="sxs-lookup"><span data-stu-id="2f131-127">Stop the VM using the **Stop** button in the toolbar on the **Overview** view of the VM.</span></span>

1. <span data-ttu-id="2f131-128">**[設定]** セクションで **[ディスク]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2f131-128">Click **Disks** in the **Settings** section.</span></span>

1. <span data-ttu-id="2f131-129">サイズを変更するデータ ディスクを選択します。</span><span class="sxs-lookup"><span data-stu-id="2f131-129">Select the data disk you want to resize.</span></span>

    ![編集する VHD が強調表示された VM のディスク セクションを示すスクリーンショット](../media/5-portal-disks.png)

1. <span data-ttu-id="2f131-131">ディスクの詳細で、現在のサイズより_大きい_サイズを入力します。</span><span class="sxs-lookup"><span data-stu-id="2f131-131">In the disk details, type a size _larger_ than the current size.</span></span> <span data-ttu-id="2f131-132">ここで Premium から Standard (またはその逆) に変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="2f131-132">You can also change from Premium to Standard (or vice-versa) here.</span></span> <span data-ttu-id="2f131-133">予測される IOPS のセクションで示すように、これらの設定でパフォーマンスが調整されます。</span><span class="sxs-lookup"><span data-stu-id="2f131-133">These settings will adjust your performance as shown in the predicted IOPS section.</span></span>

    ![新しいサイズ フィールドが強調表示されている VHD 編集画面を示すスクリーンショット](../media/5-resize-disk.png)

1. <span data-ttu-id="2f131-135">**[保存]** をクリックして変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="2f131-135">Click **Save** to save the changes.</span></span>

1. <span data-ttu-id="2f131-136">VM を再起動します。</span><span class="sxs-lookup"><span data-stu-id="2f131-136">Restart the VM.</span></span>


### <a name="expanding-the-partition"></a><span data-ttu-id="2f131-137">パーティションの拡張</span><span class="sxs-lookup"><span data-stu-id="2f131-137">Expanding the partition</span></span>

<span data-ttu-id="2f131-138">新しいデータ ディスクを追加する場合と同じように、パーティションとファイルシステムを拡張するまで、拡張ディスクでは使用可能なスペースが追加されません。</span><span class="sxs-lookup"><span data-stu-id="2f131-138">Just like adding a new data disk, an expanded disk won't add any usable space until you expand the partition and filesystem.</span></span> <span data-ttu-id="2f131-139">これは、VM で利用可能な OS ツールを使用して行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="2f131-139">This must be done using the OS tools available to the VM.</span></span> 

<span data-ttu-id="2f131-140">Windows では、ディスク マネージャー ツールまたは `diskpart` コマンド ライン ツールを使用します。</span><span class="sxs-lookup"><span data-stu-id="2f131-140">On Windows, we would use the Disk Manager tool or the `diskpart` command line tool.</span></span>

<span data-ttu-id="2f131-141">Linux では、`parted` と `resize2fs` を使用します。</span><span class="sxs-lookup"><span data-stu-id="2f131-141">On Linux, you will use `parted` and `resize2fs`.</span></span>

<span data-ttu-id="2f131-142">これらのコマンドのいくつかを試してみましょう。</span><span class="sxs-lookup"><span data-stu-id="2f131-142">Let's try out some of these commands.</span></span>