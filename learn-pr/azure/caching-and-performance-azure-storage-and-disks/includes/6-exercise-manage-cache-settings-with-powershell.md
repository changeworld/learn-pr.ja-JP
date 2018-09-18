
<span data-ttu-id="a4880-101">前回の演習では、Azure portal を使用して以下のタスクを実行しました。</span><span class="sxs-lookup"><span data-stu-id="a4880-101">In the previous exercise, we performed the following tasks using the Azure portal.</span></span>

- <span data-ttu-id="a4880-102">OS ディスクのキャッシュ状態を表示する</span><span class="sxs-lookup"><span data-stu-id="a4880-102">View OS disk cache status</span></span>
- <span data-ttu-id="a4880-103">OS ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="a4880-103">Change the cache settings of the OS disk</span></span>
- <span data-ttu-id="a4880-104">VM にデータ ディスクを追加する</span><span class="sxs-lookup"><span data-stu-id="a4880-104">Add a data disk to the VM</span></span>
- <span data-ttu-id="a4880-105">新しいデータ ディスク上のキャッシュの種類を変更する</span><span class="sxs-lookup"><span data-stu-id="a4880-105">Change caching type on a new data disk</span></span>

<span data-ttu-id="a4880-106">Azure PowerShell を使用してこれらの操作を練習してみましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-106">Let's practice these operations using Azure PowerShell.</span></span> <span data-ttu-id="a4880-107">ここでは、これまでの演習で作成した VM を使用します。</span><span class="sxs-lookup"><span data-stu-id="a4880-107">We're going to use the VM we created in the previous exercise.</span></span> <span data-ttu-id="a4880-108">このラボの操作では、以下のことを前提としています。</span><span class="sxs-lookup"><span data-stu-id="a4880-108">The operations in this lab assume:</span></span>

- <span data-ttu-id="a4880-109">VM は存在し、**fotoshareVM** という名前です</span><span class="sxs-lookup"><span data-stu-id="a4880-109">Our VM exists and is called **fotoshareVM**</span></span>
- <span data-ttu-id="a4880-110">VM は、**fotoshare-rg** というリソース グループ内に存在します。</span><span class="sxs-lookup"><span data-stu-id="a4880-110">Our VM lives in a resource group called **fotoshare-rg**</span></span>

<span data-ttu-id="a4880-111">これらの値は異なる名前を使用した場合は、これらの値を実際の名前で置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="a4880-111">If you've gone with a different set of names, just replace these values with yours.</span></span> 

<span data-ttu-id="a4880-112">前回の演習の VM ディスクは、次のような状態です。</span><span class="sxs-lookup"><span data-stu-id="a4880-112">Here's the current state of our VM disks from the last exercise.</span></span> 

![OS ディスクとデータ ディスクのスクリーンショットでは、どちらも読み取り専用キャッシュに設定されています。](../media-draft/disks-final-config-portal.PNG)

<span data-ttu-id="a4880-114">ポータルを使用して、OS ディスクとデータ ディスクの両方に \***[ホスト キャッシュ]** フィールドを設定しています。</span><span class="sxs-lookup"><span data-stu-id="a4880-114">We used the portal to set the \***HOST CACHING** field for both the OS and data disks.</span></span> <span data-ttu-id="a4880-115">以下の手順を行う際には、この初期状態を念頭に置いてください。</span><span class="sxs-lookup"><span data-stu-id="a4880-115">Keep this initial state in mind as we work through the following steps.</span></span> 

### <a name="set-up-some-variables"></a><span data-ttu-id="a4880-116">変数をいくつか設定する</span><span class="sxs-lookup"><span data-stu-id="a4880-116">Set up some variables</span></span>
<span data-ttu-id="a4880-117">まず、後で使用できるように、いくつかのリソース名を用意しておきましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-117">First, let's  store some resource names so we can use them later.</span></span>

<span data-ttu-id="a4880-118">右側の Azure Cloud Shell のターミナルを使用して、次の PowerShell コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a4880-118">Use the Azure Cloud Shell terminal on the right to run the following Powershell commands.</span></span> 

```powershell
$myRgName = "fotoshare-rg"
$myVMName = "fotoshareVM"
```

> [!TIP]
> <span data-ttu-id="a4880-119">Cloud Shell セッションがタイムアウトした場合は、これらの変数を再度設定する必要があります。そのため、可能であれば、単一のセッションでこのラボ全体を実行します。</span><span class="sxs-lookup"><span data-stu-id="a4880-119">You'll have to set these variables again if your Cloud Shell session times out. So, if possible, work through this entire lab in a single session.</span></span> 

### <a name="get-info-about-our-vm"></a><span data-ttu-id="a4880-120">VM に関する情報を取得する</span><span class="sxs-lookup"><span data-stu-id="a4880-120">Get info about our VM</span></span>

<span data-ttu-id="a4880-121">次のコマンドを実行して、VM のプロパティを取得します。</span><span class="sxs-lookup"><span data-stu-id="a4880-121">Run the following command to get back the properties of our VM.</span></span>
 
```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVMName
```
<span data-ttu-id="a4880-122">`$myVM` 変数に応答を保存します。</span><span class="sxs-lookup"><span data-stu-id="a4880-122">We store the response in our `$myVM` variable.</span></span> <span data-ttu-id="a4880-123">次のコマンドを実行すると、ここで指定したプロパティを表示できます。</span><span class="sxs-lookup"><span data-stu-id="a4880-123">We can run the following command to just show us the properties we specify here.</span></span>

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

<span data-ttu-id="a4880-124">次のスクリーンショットが示すように、この VM が目的の VM です。</span><span class="sxs-lookup"><span data-stu-id="a4880-124">As the following screenshot shows, this VM is indeed the VM we're after.</span></span> <span data-ttu-id="a4880-125">それでは次に進みましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-125">So, let's move on.</span></span> 

![最後に実行した 4 つのコマンドの結果を表示する PowerShell コンソール。](../media-draft/ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a><span data-ttu-id="a4880-127">OS ディスクのキャッシュ状態を表示する</span><span class="sxs-lookup"><span data-stu-id="a4880-127">View OS disk cache status</span></span>

<span data-ttu-id="a4880-128">次のように、`StorageProfile` オブジェクトを介してキャッシュの設定を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="a4880-128">We can check the caching  setting through  the `StorageProfile` object as follows.</span></span>

```powershell
$myVM.StorageProfile.OsDisk.Caching
```
<span data-ttu-id="a4880-129">この例では、現在の値は **None** です。</span><span class="sxs-lookup"><span data-stu-id="a4880-129">In this example, the current value is **None**.</span></span> <span data-ttu-id="a4880-130">これを OS ディスクの既定に戻してみましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-130">Let's change it back to the default for an OS disk.</span></span>

![キャッシュ値が "None" の OS ディスクが表示されている PowerShell コンソール。](../media-draft/ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a><span data-ttu-id="a4880-132">OS ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="a4880-132">Change the cache settings of the OS disk</span></span>

<span data-ttu-id="a4880-133">次のように、同じ StorageProfile オブジェクトを使用してキャッシュの種類の値を設定できます。</span><span class="sxs-lookup"><span data-stu-id="a4880-133">We can set the value for the cache type using the same StorageProfile\` object as follows.</span></span>
 
```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

<span data-ttu-id="a4880-134">このコマンドは高速で実行されます。ローカルで何らかの処理が実行されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="a4880-134">This command runs fast, which should tell you it's doing something locally.</span></span> <span data-ttu-id="a4880-135">このコマンドを実行すると、単に myVM オブジェクトのプロパティが変更されます。</span><span class="sxs-lookup"><span data-stu-id="a4880-135">The command just changes the property on the myVM object.</span></span> <span data-ttu-id="a4880-136">次のスクリーンショットが示すように、`$myVM` 変数を更新しても、VM 上のキャッシュ値は変更されません。</span><span class="sxs-lookup"><span data-stu-id="a4880-136">As the following screenshot shows, if you refresh the `$myVM` variable,  the caching value won't have changed on the VM.</span></span>

![実際には VM を更新しなかったため、"myVM" オブジェクトを更新すると、キャッシュが "none" にリセットされることを示す PowerShell コンソール。](../media-draft/ps-commands-2.PNG)

<span data-ttu-id="a4880-138">VM 自体を変更するには、次のように `Update-AzureRmVM` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="a4880-138">To  make the change on the VM itself, call `Update-AzureRmVM` as follows.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="a4880-139">この呼び出しが完了するまでには時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="a4880-139">Notice that this call takes a while to complete.</span></span> <span data-ttu-id="a4880-140">これは、実際の VM を更新しており、変更を反映するために、Azure は VM を再起動するためです。</span><span class="sxs-lookup"><span data-stu-id="a4880-140">That's because we're updating the actual VM and Azure restarts the VM  to make the change.</span></span>

![キャッシュ値が "None" の OS ディスクが表示されている PowerShell コンソール。](../media-draft/ps-oscaching-rw.PNG)

<span data-ttu-id="a4880-142">`$myVM` 変数を再度更新すると、オブジェクトの変更が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a4880-142">If you refresh the `$myVM` variable again, you'll see the change on the object.</span></span> <span data-ttu-id="a4880-143">ポータルでディスクを表示して変更を確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="a4880-143">Looking at the disk in the portal, you'd also see the change there.</span></span> <span data-ttu-id="a4880-144">それでは、新しいデータ ディスクの作成に進みましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-144">Ok, let's move on to creating a new data disk.</span></span>  

### <a name="list-data-disk-info"></a><span data-ttu-id="a4880-145">データ ディスク情報を一覧表示する</span><span class="sxs-lookup"><span data-stu-id="a4880-145">List data disk info</span></span>

<span data-ttu-id="a4880-146">VM 上にあるデータ ディスクを確認するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a4880-146">To see what data disks we have on our VM, run the following command.</span></span> 

```powershell
$myVM.StorageProfile.DataDisks
```

<span data-ttu-id="a4880-147">現在、データ ディスクは 1 つしかありません。</span><span class="sxs-lookup"><span data-stu-id="a4880-147">We've only one data disk at the moment.</span></span> <span data-ttu-id="a4880-148">`Lun` フィールドは重要です。</span><span class="sxs-lookup"><span data-stu-id="a4880-148">The `Lun` field is important.</span></span> <span data-ttu-id="a4880-149">これは一意の論理ユニット番号 (**L\*\*\*\*U\*\*\*\*N**) です。</span><span class="sxs-lookup"><span data-stu-id="a4880-149">It's the unique **L**ogical **U**nit **N**umber.</span></span> <span data-ttu-id="a4880-150">別のデータ ディスクを追加する場合は、一意の `Lun` 値を付けます。</span><span class="sxs-lookup"><span data-stu-id="a4880-150">When we add another data disk, we'll give it a unique `Lun` value.</span></span> 

### <a name="add-a-new-data-disk-to-our-vm"></a><span data-ttu-id="a4880-151">VM に新しいデータ ディスクを追加する</span><span class="sxs-lookup"><span data-stu-id="a4880-151">Add a new data disk to our VM</span></span> 

<span data-ttu-id="a4880-152">便宜的に、新しいディスク名を用意します。</span><span class="sxs-lookup"><span data-stu-id="a4880-152">For convenience, we'll store our new disk name.</span></span>

```powershell
$newDiskName = "fotoshareVM-data2"
```

<span data-ttu-id="a4880-153">次の `Add-AzureRmVMDataDisk` コマンドを実行して、新しいディスクを定義します。</span><span class="sxs-lookup"><span data-stu-id="a4880-153">Run the following `Add-AzureRmVMDataDisk` command to define a new disk.</span></span> 

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

<span data-ttu-id="a4880-154">1 の Lun 値は使用されていないため、このディスクに使用しました。</span><span class="sxs-lookup"><span data-stu-id="a4880-154">We've given this disk a Lun value of 1, since that's not taken.</span></span> <span data-ttu-id="a4880-155">作成するディスクを定義したので、次は `Update-AzureRmVM` を実行して実際に変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-155">We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change.</span></span> 

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="a4880-156">データ ディスク情報をもう一度見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-156">Let's look at our data disk info again.</span></span>

```powershell
$myVM.StorageProfile.DataDisks
```

![2 つのデータ ディスクが表示されている PowerShell コンソール。](../media-draft/2-data-disks-part1.png)

<span data-ttu-id="a4880-158">これで 2 つのディスクができました。</span><span class="sxs-lookup"><span data-stu-id="a4880-158">We now have two disks.</span></span> <span data-ttu-id="a4880-159">新しいディスクの **Lun** 値は 1 で、**Caching** の既定値は **None** です。</span><span class="sxs-lookup"><span data-stu-id="a4880-159">Our new disk has a **Lun** of 1 and the default value for **Caching** is **None**.</span></span> <span data-ttu-id="a4880-160">この値を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="a4880-160">Let's change that value.</span></span>

### <a name="change-cache-settings-of-new-data-disk"></a><span data-ttu-id="a4880-161">新しいデータ ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="a4880-161">Change cache settings of new data disk</span></span>

<span data-ttu-id="a4880-162">次のように、`Set-AzureRmVMDataDisk` コマンドレットを使用して仮想マシン データ ディスクのプロパティを変更します。</span><span class="sxs-lookup"><span data-stu-id="a4880-162">We modify properties of a virtual machine data disk with the `Set-AzureRmVMDataDisk` cmdlet as follows.</span></span>

```powershell
Set-AzureRmVMDataDisk -Lun "1" -Caching ReadWrite
```

<span data-ttu-id="a4880-163">いつものように、`Update-AzureRmVM` を使用して変更をコミットします。</span><span class="sxs-lookup"><span data-stu-id="a4880-163">As always, commit the changes with `Update-AzureRmVM`.</span></span>

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

<span data-ttu-id="a4880-164">この演習で実行した結果のポータルのビューを次に示します。</span><span class="sxs-lookup"><span data-stu-id="a4880-164">Here's a view from the portal of what we've accomplished in this exercise.</span></span> <span data-ttu-id="a4880-165">VM には 2 つのデータ ディスクがあり、すべての **[ホスト キャッシュ]** の設定を調整しました。</span><span class="sxs-lookup"><span data-stu-id="a4880-165">Our VM now has two data disks and we've adjust all **HOST Caching** settings.</span></span> <span data-ttu-id="a4880-166">そのすべてをいくつかのコマンドだけで実行しました。</span><span class="sxs-lookup"><span data-stu-id="a4880-166">We did all of that with just a few commands.</span></span> <span data-ttu-id="a4880-167">これが Azure PowerShell の機能です。</span><span class="sxs-lookup"><span data-stu-id="a4880-167">That's the power of Azure PowerShell.</span></span>

![2 つのデータ ディスクが表示されている PowerShell コンソール。](../media-draft/disks-final-config-portal2.png)
