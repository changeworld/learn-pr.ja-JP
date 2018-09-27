<span data-ttu-id="80b58-101">前の演習では、Azure portal を使用して以下のタスクを実行しました。</span><span class="sxs-lookup"><span data-stu-id="80b58-101">In the previous exercise, we performed the following tasks using the Azure portal:</span></span>

- <span data-ttu-id="80b58-102">OS ディスクのキャッシュ状態を表示する</span><span class="sxs-lookup"><span data-stu-id="80b58-102">View OS disk cache status</span></span>
- <span data-ttu-id="80b58-103">OS ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="80b58-103">Change the cache settings of the OS disk</span></span>
- <span data-ttu-id="80b58-104">VM にデータ ディスクを追加する</span><span class="sxs-lookup"><span data-stu-id="80b58-104">Add a data disk to the VM</span></span>
- <span data-ttu-id="80b58-105">新しいデータ ディスク上のキャッシュの種類を変更する</span><span class="sxs-lookup"><span data-stu-id="80b58-105">Change caching type on a new data disk</span></span>

<span data-ttu-id="80b58-106">Azure PowerShell を使用してこれらの操作を練習してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80b58-106">Let's practice these operations using Azure PowerShell.</span></span> 

> [!NOTE]
> <span data-ttu-id="80b58-107">ここでは Azure PowerShell を使用しますが、コンソール ベースのツールとして同様の機能を提供する Azure CLI を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="80b58-107">We're going to use Azure PowerShell, but you could also use the Azure CLI which provides similar functionality as a console-based tool.</span></span> <span data-ttu-id="80b58-108">macOS、Linux、Windows で動作します。</span><span class="sxs-lookup"><span data-stu-id="80b58-108">It runs on macOS, Linux, and Windows.</span></span> <span data-ttu-id="80b58-109">Azure CLI について詳しく学習したい場合は、「**Azure CLI を使用して仮想マシンを管理する**」モジュールをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="80b58-109">If you are interested in learning more about the Azure CLI, check out the **Manage Virtual Machines with the Azure CLI** module.</span></span>

<span data-ttu-id="80b58-110">ここでは、これまでの演習で作成した VM を使用します。</span><span class="sxs-lookup"><span data-stu-id="80b58-110">We're going to use the VM we created in the previous exercise.</span></span> <span data-ttu-id="80b58-111">このラボの操作では、以下のことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="80b58-111">The operations in this lab assume:</span></span>

- <span data-ttu-id="80b58-112">VM は存在しており、**fotoshareVM** という名前です。</span><span class="sxs-lookup"><span data-stu-id="80b58-112">Our VM exists and is called **fotoshareVM**</span></span>
- <span data-ttu-id="80b58-113">この VM は、**<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループに存在します。</span><span class="sxs-lookup"><span data-stu-id="80b58-113">Our VM lives in a resource group called **<rgn>[sandbox resource group name]</rgn>**</span></span>

<span data-ttu-id="80b58-114">異なる名前のセットを使用した場合は、これらの値を実際の名前で置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="80b58-114">If you've gone with a different set of names, replace these values with yours.</span></span>

<span data-ttu-id="80b58-115">前回の演習の VM ディスクは、次のような状態です。</span><span class="sxs-lookup"><span data-stu-id="80b58-115">Here's the current state of our VM disks from the last exercise:</span></span>

![OS ディスクとデータ ディスクのスクリーンショットでは、どちらも読み取り専用キャッシュに設定されています。](../media/disks-final-config-portal.PNG)

<span data-ttu-id="80b58-117">ポータルを使用して、OS ディスクとデータ ディスクの両方に **[ホスト キャッシュ]** フィールドを設定しています。</span><span class="sxs-lookup"><span data-stu-id="80b58-117">We used the portal to set the **HOST CACHING** field for both the OS and data disks.</span></span> <span data-ttu-id="80b58-118">以下の手順を行う際には、この初期状態を念頭に置いてください。</span><span class="sxs-lookup"><span data-stu-id="80b58-118">Keep this initial state in mind as we work through the following steps.</span></span>

### <a name="set-up-some-variables"></a><span data-ttu-id="80b58-119">変数をいくつか設定する</span><span class="sxs-lookup"><span data-stu-id="80b58-119">Set up some variables</span></span>

<span data-ttu-id="80b58-120">まず、後で使用できるように、いくつかのリソース名を用意しておきましょう。</span><span class="sxs-lookup"><span data-stu-id="80b58-120">First, let's store some resource names so we can use them later.</span></span>

1. <span data-ttu-id="80b58-121">右側の Azure Cloud Shell のターミナルを使用して、次の PowerShell コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80b58-121">Use the Azure Cloud Shell terminal on the right to run the following PowerShell commands:</span></span>

    > [!NOTE]
    > <span data-ttu-id="80b58-122">まだ行っていない場合は、コマンドを試す前に Cloud Shell セッションを **PowerShell** に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="80b58-122">Switch your Cloud Shell session to **PowerShell** before trying these commands, if it isn't already.</span></span>
    
    ```powershell
    $myRgName = "<rgn>[sandbox resource group name]</rgn>"
    $myVMName = "fotoshareVM"
    ```
    
    > [!TIP]
    > <span data-ttu-id="80b58-123">Cloud Shell セッションがタイムアウトした場合は、これらの変数を再度設定する必要があります。そのため、可能であれば、単一のセッションでこのラボ全体を実行します。</span><span class="sxs-lookup"><span data-stu-id="80b58-123">You'll have to set these variables again if your Cloud Shell session times out. So, if possible, work through this entire lab in a single session.</span></span>
    
### <a name="get-info-about-our-vm"></a><span data-ttu-id="80b58-124">VM に関する情報を取得する</span><span class="sxs-lookup"><span data-stu-id="80b58-124">Get info about our VM</span></span>

1. <span data-ttu-id="80b58-125">次のコマンドを実行して、VM のプロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="80b58-125">Run the following command to get back the properties of our VM:</span></span>

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    ```
    
1. <span data-ttu-id="80b58-126">`$myVM` 変数に応答を保存します。</span><span class="sxs-lookup"><span data-stu-id="80b58-126">We store the response in our `$myVM` variable.</span></span> <span data-ttu-id="80b58-127">出力を `select-object` コマンドレットにパイプして、特定のプロパティが表示されるようにフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="80b58-127">We can pipe the output into the `select-object` cmdlet to filter the display to specific properties:</span></span>

    ```powershell
    $myVM | select-object -property ResourceGroupName, Name, Type, Location
    ```
    
1. <span data-ttu-id="80b58-128">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="80b58-128">You should get something like the following.</span></span>

    ```output
    ResourceGroupName Name        Type                              Location
    ----------------- ----        ----                              --------
    <rgn>[sandbox resource group name]</rgn> fotoshareVM Microsoft.Compute/virtualMachines eastus
    ```
    
### <a name="view-os-disk-cache-status"></a><span data-ttu-id="80b58-129">OS ディスクのキャッシュ状態を表示する</span><span class="sxs-lookup"><span data-stu-id="80b58-129">View OS disk cache status</span></span>

1. <span data-ttu-id="80b58-130">次のように、`StorageProfile` オブジェクトを介してキャッシュの設定を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="80b58-130">We can check the caching  setting through  the `StorageProfile` object, as follows:</span></span>

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching
    ```

    ```output
    ReadOnly
    ```
   
1. <span data-ttu-id="80b58-131">これを OS ディスクの既定値である _ReadWrite_ に戻してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80b58-131">Let's change it back to the default for an OS disk which is _ReadWrite_.</span></span>

### <a name="change-the-cache-settings-of-the-os-disk"></a><span data-ttu-id="80b58-132">OS ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="80b58-132">Change the cache settings of the OS disk</span></span>

1. <span data-ttu-id="80b58-133">次のように、同じ `StorageProfile` オブジェクトを使用してキャッシュの種類の値を設定できます。</span><span class="sxs-lookup"><span data-stu-id="80b58-133">We can set the value for the cache type using the same `StorageProfile` object, as follows:</span></span>

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
    ```
    
    <span data-ttu-id="80b58-134">このコマンドは高速で実行されます。ローカルで何らかの処理が実行されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="80b58-134">This command runs fast, which should tell you it's doing something locally.</span></span> <span data-ttu-id="80b58-135">このコマンドを実行すると、単に `myVM` オブジェクトのプロパティが変更されます。</span><span class="sxs-lookup"><span data-stu-id="80b58-135">The command only changes the property on the `myVM` object.</span></span> <span data-ttu-id="80b58-136">次のスクリーンショットが示すように、`Get-AzureRmVM` コマンドレットを使用して再割り当てすることによって `$myVM` 変数を更新しても、VM 上のキャッシュ値は変更されません。</span><span class="sxs-lookup"><span data-stu-id="80b58-136">As the following screenshot shows, if you refresh the `$myVM` variable by reassigning it using the `Get-AzureRmVM` cmdlet, the caching value won't have changed on the VM.</span></span>

1. <span data-ttu-id="80b58-137">VM 自体を変更するには、次のように `Update-AzureRmVM` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="80b58-137">To make the change on the VM itself, call `Update-AzureRmVM`, as follows:</span></span>

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
    <span data-ttu-id="80b58-138">この呼び出しが完了するまでには時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="80b58-138">Notice that this call takes a while to complete.</span></span> <span data-ttu-id="80b58-139">これは、実際の VM を更新しており、変更を反映するために、Azure は VM を再起動するためです。</span><span class="sxs-lookup"><span data-stu-id="80b58-139">That's because we're updating the actual VM, and Azure restarts the VM  to make the change.</span></span>

    ```output
    RequestId IsSuccessStatusCode StatusCode ReasonPhrase
    --------- ------------------- ---------- ------------
                             True         OK OK
    ```
    
1. <span data-ttu-id="80b58-140">`$myVM` 変数を再度更新すると、オブジェクトの変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80b58-140">If you refresh the `$myVM` variable again, you'll see the change on the object.</span></span> <span data-ttu-id="80b58-141">ポータルでディスクを表示して変更を確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="80b58-141">Looking at the disk in the portal, you'd also see the change there.</span></span> 

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    $myVM.StorageProfile.OsDisk.Caching
    ```
    
    ```output
    ReadWrite
    ```
    
### <a name="list-data-disk-info"></a><span data-ttu-id="80b58-142">データ ディスク情報を一覧表示する</span><span class="sxs-lookup"><span data-stu-id="80b58-142">List data disk info</span></span>

1. <span data-ttu-id="80b58-143">VM 上にあるデータ ディスクを確認するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="80b58-143">To see what data disks we have on our VM, run the following command:</span></span>

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    ```
    
<span data-ttu-id="80b58-144">現在、データ ディスクは 1 つしかありません。</span><span class="sxs-lookup"><span data-stu-id="80b58-144">We have only one data disk at the moment.</span></span> <span data-ttu-id="80b58-145">`Lun` フィールドは重要です。</span><span class="sxs-lookup"><span data-stu-id="80b58-145">The `Lun` field is important.</span></span> <span data-ttu-id="80b58-146">これは一意の論理ユニット番号 (**L\*\*\*\*U\*\*\*\*N**) です。</span><span class="sxs-lookup"><span data-stu-id="80b58-146">It's the unique **L**ogical **U**nit **N**umber.</span></span> <span data-ttu-id="80b58-147">別のデータ ディスクを追加する場合は、一意の `Lun` 値を付けます。</span><span class="sxs-lookup"><span data-stu-id="80b58-147">When we add another data disk, we'll give it a unique `Lun` value.</span></span>

### <a name="add-a-new-data-disk-to-our-vm"></a><span data-ttu-id="80b58-148">VM に新しいデータ ディスクを追加する</span><span class="sxs-lookup"><span data-stu-id="80b58-148">Add a new data disk to our VM</span></span>

1. <span data-ttu-id="80b58-149">便宜的に、新しいディスク名を用意します。</span><span class="sxs-lookup"><span data-stu-id="80b58-149">For convenience, we'll store our new disk name:</span></span>

    ```powershell
    $newDiskName = "fotoshareVM-data2"
    ```
    
1. <span data-ttu-id="80b58-150">次の `Add-AzureRmVMDataDisk` コマンドを実行して、新しく 1 GB の空のデータ ディスクを定義します。</span><span class="sxs-lookup"><span data-stu-id="80b58-150">Run the following `Add-AzureRmVMDataDisk` command to define a new empty 1 GB data disk:</span></span>

    ```powershell
    Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
    ```
    <span data-ttu-id="80b58-151">次のような応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80b58-151">You'll get a response like:</span></span>

    ```output
    ResourceGroupName  : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx
    Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxx-xxxxxxx/resourceGroups/<rgn>[sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/fotoshareVM
    VmId               : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
    Name               : fotoshareVM
    Type               : Microsoft.Compute/virtualMachines
    Location           : eastus
    Tags               : {}
    DiagnosticsProfile : {BootDiagnostics}
    HardwareProfile    : {VmSize}
    NetworkProfile     : {NetworkInterfaces}
    OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
    ProvisioningState  : Succeeded
    StorageProfile     : {ImageReference, OsDisk, DataDisks}
        ```
    
1. We've given this disk a `Lun` value of `1` because it's not taken. We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change:

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
1. <span data-ttu-id="80b58-152">データ ディスク情報をもう一度見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="80b58-152">Let's look at our data disk info again:</span></span>

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    
    Name            : fotoshareVM-data2
    DiskSizeGB      : 1
    Lun             : 1
    Caching         : None
    CreateOption    : Empty
    SourceImage     :
    VirtualHardDisk :
    ```

<span data-ttu-id="80b58-153">これで 2 つのディスクができました。</span><span class="sxs-lookup"><span data-stu-id="80b58-153">We now have two disks.</span></span> <span data-ttu-id="80b58-154">新しいディスクは `Lun` が `1` で、`Caching` の既定値は `None` です。</span><span class="sxs-lookup"><span data-stu-id="80b58-154">Our new disk has a `Lun` of `1` and the default value for `Caching` is `None`.</span></span> <span data-ttu-id="80b58-155">この値を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80b58-155">Let's change that value.</span></span>

### <a name="change-cache-settings-of-new-data-disk"></a><span data-ttu-id="80b58-156">新しいデータ ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="80b58-156">Change cache settings of new data disk</span></span>

1. <span data-ttu-id="80b58-157">次のように、`Set-AzureRmVMDataDisk` コマンドレットを使用して仮想マシン データ ディスクのプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="80b58-157">We modify properties of a virtual machine data disk with the `Set-AzureRmVMDataDisk` cmdlet, as follows:</span></span>

    ```powershell
    Set-AzureRmVMDataDisk -VM $myVM -Lun "1" -Caching ReadWrite
    ```
    
1. <span data-ttu-id="80b58-158">いつものように、`Update-AzureRmVM` を使用して変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="80b58-158">As always, commit the changes with `Update-AzureRmVM`:</span></span>

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
<span data-ttu-id="80b58-159">この演習で実行した結果のポータルのビューを次に示します。</span><span class="sxs-lookup"><span data-stu-id="80b58-159">Here's a view from the portal of what we've accomplished in this exercise.</span></span> <span data-ttu-id="80b58-160">VM には 2 つのデータ ディスクがあり、すべての **[ホスト キャッシュ]** の設定を調整しました。</span><span class="sxs-lookup"><span data-stu-id="80b58-160">Our VM now has two data disks, and we've adjusted all **HOST CACHING** settings.</span></span> <span data-ttu-id="80b58-161">そのすべてをいくつかのコマンドだけで実行しました。</span><span class="sxs-lookup"><span data-stu-id="80b58-161">We did all of that with just a few commands.</span></span> <span data-ttu-id="80b58-162">これが Azure PowerShell の機能です。</span><span class="sxs-lookup"><span data-stu-id="80b58-162">That's the power of Azure PowerShell.</span></span>

![2 つのデータ ディスクが表示されている VM ブレードの [ディスク] セクションを示す Azure portal のスクリーンショット。](../media/disks-final-config-portal2.png)
