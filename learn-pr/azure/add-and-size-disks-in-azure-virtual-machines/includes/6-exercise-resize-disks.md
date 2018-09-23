<span data-ttu-id="98574-101">一部のアップロードの規模を低く見積もったため、アップロード ディスクの領域が不足しています。</span><span class="sxs-lookup"><span data-stu-id="98574-101">We underestimated how big some of the uploads would be and our upload disk is running out of space.</span></span> <span data-ttu-id="98574-102">領域を 2 倍にし、64 GB から 128 GB にアップグレードすることにします。</span><span class="sxs-lookup"><span data-stu-id="98574-102">You decide to double the space and upgrade it from 64 GB to 128 GB.</span></span>

## <a name="resize-the-data-disk"></a><span data-ttu-id="98574-103">データ ディスクのサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="98574-103">Resize the data disk</span></span>

<span data-ttu-id="98574-104">ディスクのサイズを変更するには、ディスクの ID または名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="98574-104">To resize a disk, we need the ID or name of the disk.</span></span> <span data-ttu-id="98574-105">ここでは、名前 (uploadDataDisk1) は既にわかっています。</span><span class="sxs-lookup"><span data-stu-id="98574-105">In this case we already know the name (uploadDataDisk1).</span></span> <span data-ttu-id="98574-106">しかし、それを記憶しなかった場合や、それが他のユーザーによって作成された場合、`az disk list` コマンドを使用してディスクのリストを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="98574-106">But in case we didn't remember that, or it was created by someone else we can get a list of disks using the `az disk list` command.</span></span>

1. <span data-ttu-id="98574-107">まず、リソース グループ内のマネージド ディスクのリストを取得します。単一のリソース グループに VM が複数ある場合、これには他のディスクが含まれる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="98574-107">Start by getting a list of the managed disks in the resource group; this might include other disks if you have multiple VMs in a single resource group.</span></span> <span data-ttu-id="98574-108">この例では、Web サーバーのみです。</span><span class="sxs-lookup"><span data-stu-id="98574-108">For our example, there should just be our web server.</span></span>

    ```azurecli
    az disk list \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

1. <span data-ttu-id="98574-109">次に、`az vm deallocate` を使用して VM を停止し、割り当てを解除します。</span><span class="sxs-lookup"><span data-stu-id="98574-109">Next, stop and deallocate the VM using `az vm deallocate`.</span></span> 

    ```azurecli
    az vm deallocate --name support-web-vm01
    ```
1. <span data-ttu-id="98574-110">これで、`az disk update` コマンドを使用してディスクのサイズを変更できます。</span><span class="sxs-lookup"><span data-stu-id="98574-110">Now we can resize the disk with the `az disk update` command.</span></span>

    ```azurecli
    az disk update --name uploadDataDisk1 --size-gb 128
    ```
    
1. <span data-ttu-id="98574-111">サイズ変更の操作が完了したら、VM を再起動します。</span><span class="sxs-lookup"><span data-stu-id="98574-111">Once the resize operation has completed, restart the VM.</span></span>

    ```azurecli
    az vm start --name support-web-vm01
    ```

## <a name="expand-the-disk-partition"></a><span data-ttu-id="98574-112">ディスク パーティションを拡張する</span><span class="sxs-lookup"><span data-stu-id="98574-112">Expand the disk partition</span></span>

<span data-ttu-id="98574-113">最後の手順では、利用可能な領域について OS に伝えます。</span><span class="sxs-lookup"><span data-stu-id="98574-113">The final step is to tell the OS about the available space.</span></span> <span data-ttu-id="98574-114">パーティション分割とフォーマットについて前に行った手順と同様、オンプレミスのディスク拡張に関するこのプロセスは同じです。</span><span class="sxs-lookup"><span data-stu-id="98574-114">Just like the partitioning and format steps we did earlier, this process is identical for on-premise disk expansions.</span></span> 

1. <span data-ttu-id="98574-115">まず、実際の VM のパブリック IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="98574-115">First, get the public IP address of your VM.</span></span> <span data-ttu-id="98574-116">VM は再起動されたため、変更されている可能性があります。</span><span class="sxs-lookup"><span data-stu-id="98574-116">Since it was rebooted, it has likely changed.</span></span> <span data-ttu-id="98574-117">今度は別のアプローチを試してみましょう。その場合、パブリック IP アドレスを返すようにフィルターで `az vm show` を使用します。</span><span class="sxs-lookup"><span data-stu-id="98574-117">Let's try a different approach this time and use `az vm show` with a filter to return the public IP address.</span></span>

    > [!TIP]
    > <span data-ttu-id="98574-118">IP アドレスは既定で動的になります。</span><span class="sxs-lookup"><span data-stu-id="98574-118">IP addresses are dynamic by default.</span></span> <span data-ttu-id="98574-119">Azure DNS では IP 変更が自動的に補正されます。静的 IP アドレスを使用して、自分で動作を変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="98574-119">Azure DNS will automatically compensate for the IP change, or you can alter the behavior by using static IP addresses.</span></span>

    ```azurecli
    az vm show --name support-web-vm01 -d --query [publicIps] --o tsv
    ```
    
1. <span data-ttu-id="98574-120">Linux コンピューターに ssh 接続します。</span><span class="sxs-lookup"><span data-stu-id="98574-120">SSH into the Linux machine.</span></span> <span data-ttu-id="98574-121">正しい IP アドレスを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="98574-121">You will need to supply your correct IP address.</span></span>

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. <span data-ttu-id="98574-122">ディスクのマウントを解除します。</span><span class="sxs-lookup"><span data-stu-id="98574-122">Unmount the disk.</span></span> <span data-ttu-id="98574-123">`/dev/sdc1` であることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="98574-123">Recall that it was `/dev/sdc1`.</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

1. <span data-ttu-id="98574-124">管理者特権のシェルで `parted` を起動する</span><span class="sxs-lookup"><span data-stu-id="98574-124">Launch `parted` in an elevated shell</span></span>

    ```bash
    sudo parted /dev/sdc
    ```
    
1. <span data-ttu-id="98574-125">`resizepart` コマンドを使用して、パーティションを拡張します。</span><span class="sxs-lookup"><span data-stu-id="98574-125">Expand the partition with the `resizepart` command.</span></span> <span data-ttu-id="98574-126">パーティション 1 と新しいサイズ (128 GB) を入力します。</span><span class="sxs-lookup"><span data-stu-id="98574-126">Enter partition 1 and the new size (128GB).</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [64GB]? 128GB
    ```

    > [!WARNING]
    > <span data-ttu-id="98574-127">サイズには注意してください。</span><span class="sxs-lookup"><span data-stu-id="98574-127">Be careful about the size.</span></span> <span data-ttu-id="98574-128">パーティションのサイズを変更するとパーティションの縮小も可能になり、データ損失が発生する可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="98574-128">Resizing the partition allows you to shrink a partition too, and that will likely result in data loss.</span></span>
    
1. <span data-ttu-id="98574-129">「`quit`」と入力して、ツールを終了します。</span><span class="sxs-lookup"><span data-stu-id="98574-129">Exit the tool by typing `quit`.</span></span>

1. <span data-ttu-id="98574-130">パーティション ツールによってドライブが自動的に_再マウント_されます。</span><span class="sxs-lookup"><span data-stu-id="98574-130">The partition tool will automatically _remount_ the drive.</span></span> <span data-ttu-id="98574-131">そのため、もう一度マウントを解除して、フォーマットできるようにします。</span><span class="sxs-lookup"><span data-stu-id="98574-131">So unmount it again so we can format it.</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```
    
1. <span data-ttu-id="98574-132">`e2fsck` を使用してパーティションの整合性を確認します。</span><span class="sxs-lookup"><span data-stu-id="98574-132">Verify the partition consistency with `e2fsck`.</span></span> <span data-ttu-id="98574-133">この手順はどうしても必要になりますが、ディスク ボリュームのサイズを変更する際には随時行うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="98574-133">This step is absolutely necessary but is a good idea any time you are changing sizes on a disk volume.</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. <span data-ttu-id="98574-134">`resize2fs` を使用して、ファイルシステムのサイズを変更します。</span><span class="sxs-lookup"><span data-stu-id="98574-134">Resize the filesystem with `resize2fs`.</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. <span data-ttu-id="98574-135">最後に、ドライブをマウント ポイントにマウントし直します。</span><span class="sxs-lookup"><span data-stu-id="98574-135">Finally, mount the drive back to the mount point.</span></span>

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```

<span data-ttu-id="98574-136">OS ディスクのサイズが変更されたことを確認するには、`df -h` を使用します。</span><span class="sxs-lookup"><span data-stu-id="98574-136">To verify the OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="98574-137">ここでドライブが 128 GB であることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="98574-137">It should now show that the drive is 128 GB.</span></span>
