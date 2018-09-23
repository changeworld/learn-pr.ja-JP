<span data-ttu-id="ad9f6-101">元のシナリオである CRM ソフトウェアをテストする VM の作成を思い出してください。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-101">Recall our original scenario - creating VMs to test our CRM software.</span></span> <span data-ttu-id="ad9f6-102">新しいビルドを使用できるようになったので、クリーンなイメージからインストール エクスペリエンス全体をテストできるように、新しい VM を起動してみたいと思います。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-102">When a new build is available, we want to spin up a new VM so we can test the full install experience from a clean image.</span></span> <span data-ttu-id="ad9f6-103">それが完了したら、VM を削除します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-103">Then when we are finished, we want to delete the VM.</span></span>

<span data-ttu-id="ad9f6-104">VM の作成に使用するコマンドを試してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-104">Let's try the commands you would use to create a VM.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm-with-azure-powershell"></a><span data-ttu-id="ad9f6-105">Azure PowerShell を使用して Linux VM を作成する</span><span class="sxs-lookup"><span data-stu-id="ad9f6-105">Create a Linux VM with Azure PowerShell</span></span>

<span data-ttu-id="ad9f6-106">Azure サンドボックスを使用しているので、リソース グループを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-106">Since we are using the Azure Sandbox, you won't have to create a Resource Group.</span></span> <span data-ttu-id="ad9f6-107">代わりに、**<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-107">Instead, use the Resource Group **<rgn>[Sandbox resource group name]</rgn>**.</span></span> <span data-ttu-id="ad9f6-108">また、場所の制限についても注意してください。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-108">In addition, be aware of the location restrictions.</span></span>

<span data-ttu-id="ad9f6-109">PowerShell を使用して新しい Azure VM を作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-109">Let's create a new Azure VM with PowerShell.</span></span>

1. <span data-ttu-id="ad9f6-110">`New-AzureRmVm` コマンドレットを使用して VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-110">Use the `New-AzureRmVm` cmdlet to create a VM.</span></span>
    - <span data-ttu-id="ad9f6-111">**<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-111">Use the Resource Group **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
    - <span data-ttu-id="ad9f6-112">VM に名前を付けます。通常、VM の目的、場所、(複数ある場合は) インスタンス番号を識別できる何か意味のある名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-112">Give the VM a name - typically you want to use something meaningful that identifies the purposes of the VM, location, and (if there is more than one) instance number.</span></span> <span data-ttu-id="ad9f6-113">ここでは、"米国東部内のテスト VM、インスタンス 1" を表す "testvm-eus-01" を使用します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-113">We'll use "testvm-eus-01" for "Test VM in East US, instance 1".</span></span> <span data-ttu-id="ad9f6-114">VM の配置場所に基づいて独自の名前を付けてください。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-114">Come up with your own name based on where you place the VM.</span></span>
    - <span data-ttu-id="ad9f6-115">Azure サンドボックスで使用できる次の一覧から、自分に近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-115">Select a location close to you from the following list available in the Azure Sandbox.</span></span> <span data-ttu-id="ad9f6-116">コピーと貼り付けを使用する場合は、次のコマンド例の値を必ず変更してください。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-116">Make sure to change the value in the below example command if you are using copy and paste.</span></span>

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - <span data-ttu-id="ad9f6-117">イメージには "UbuntuLTS" を使用します (これは Ubuntu Linux です)。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-117">Use "UbuntuLTS" for the image - this is Ubuntu Linux.</span></span>
    - <span data-ttu-id="ad9f6-118">`Get-Credential` コマンドレットを使用し、結果を `Credential` パラメーターに指定します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-118">Use the `Get-Credential` cmdlet and feed the results into the `Credential` parameter.</span></span>
    - <span data-ttu-id="ad9f6-119">`-OpenPorts` パラメーターを追加し、ポートとして "22" を渡します。これで、SSH でマシンに接続できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-119">Add the `-OpenPorts` parameter and pass "22" as the port - this will let us SSH into the machine.</span></span>
 
    ```powershell
    New-AzureRmVm -ResourceGroupName <rgn>[Sandbox resource group name]</rgn> -Name "testvm-eus-01" -Credential (Get-Credential) -Location "East US" -Image UbuntuLTS -OpenPorts 22
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
    
1. <span data-ttu-id="ad9f6-120">これが完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-120">This will take a few minutes to complete.</span></span> <span data-ttu-id="ad9f6-121">完了したら、クエリを実行し、変数 (`$vm`) に VM オブジェクトを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-121">Once it does, you can query it and assign the VM object to a variable (`$vm`).</span></span>

    ```powershell
    $vm = Get-AzureRmVM -Name "testvm-eus-01" -ResourceGroupName <rgn>[Sandbox resource group name]</rgn>
    ```
    
1. <span data-ttu-id="ad9f6-122">次に、値のクエリを実行し、VM に関する情報をダンプします。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-122">Then query the value to dump out the information about the VM:</span></span>

    ```powershell
    $vm
    ```

    <span data-ttu-id="ad9f6-123">次のような結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-123">You should see something like:</span></span>

    ```output
    ResourceGroupName : <rgn>[Sandbox resource group name]</rgn>
    Id                : /subscriptions/xxxxxxxx-xxxx-aaaa-bbbb-cccccccccccc/resourceGroups/<rgn>[Sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/testvm-eus-01
    VmId              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    Name              : testvm-eus-01
    Type              : Microsoft.Compute/virtualMachines
    Location          : eastus
    Tags              : {}
    HardwareProfile   : {VmSize}
    NetworkProfile    : {NetworkInterfaces}
    OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
    ProvisioningState : Succeeded
    StorageProfile    : {ImageReference, OsDisk, DataDisks}
    ```
    
1. <span data-ttu-id="ad9f6-124">複合オブジェクトを指定するには、ドット (".") 構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-124">You can reach into complex objects through a dot (".") syntax.</span></span> <span data-ttu-id="ad9f6-125">たとえば、HardwareProfile セクションに関連付けられた `VMSize` オブジェクトのプロパティを表示するには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-125">For example, to see the properties in the `VMSize` object associated with the HardwareProfile section you can type:</span></span>

    ```powershell
    $vm.HardwareProfile
    ```

1. <span data-ttu-id="ad9f6-126">また、ディスクのいずれかに関する情報を取得するには、次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-126">Or to get information on one of the disks:</span></span>

    ```powershell
    $vm.StorageProfile.OsDisk
    ```

1. <span data-ttu-id="ad9f6-127">VM オブジェクトを他のコマンドレットに渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-127">You can even pass the VM object into other cmdlets.</span></span> <span data-ttu-id="ad9f6-128">VM のパブリック IP アドレスを取得する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-128">For example, this will retrieve the public IP address of your VM:</span></span>

    ```powershell
    $vm | Get-AzureRmPublicIpAddress
    ```

1. <span data-ttu-id="ad9f6-129">IP アドレスを指定し、SSH を使用して VM に接続することができます。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-129">With the IP address, you can connect to the VM with SSH.</span></span> <span data-ttu-id="ad9f6-130">たとえば、ユーザー名 "bob" を使用し、IP アドレスが "205.22.16.5" の場合、このコマンドで Linux マシンに接続されます。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-130">For example, if you used the username "bob", and the IP address is "205.22.16.5", then this command would connect to the Linux machine:</span></span>

```powershell
ssh bob@205.22.16.5
```

## <a name="delete-a-vm"></a><span data-ttu-id="ad9f6-131">VM の削除</span><span class="sxs-lookup"><span data-stu-id="ad9f6-131">Delete a VM</span></span>

<span data-ttu-id="ad9f6-132">その他のコマンドを試すために、VM を削除してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-132">Just to try out some more commands, let's delete the VM.</span></span> <span data-ttu-id="ad9f6-133">まずシャットダウンします。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-133">We'll shut it down first.</span></span>

```powershell
Stop-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

<span data-ttu-id="ad9f6-134">次に `Remove-AzureRmVM` コマンドレットを使用して VM を削除します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-134">Now, let's delete the VM with the `Remove-AzureRmVM` cmdlet:</span></span>

```powershell
Remove-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

<span data-ttu-id="ad9f6-135">このコマンドで、リソース グループ内のすべてのリソースを一覧表示してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-135">Try this command to list all the resources in your resource group:</span></span>

```powershell
Get-AzureRmResource -ResourceGroupName $vm.ResourceGroupName | ft
```

<span data-ttu-id="ad9f6-136">まだ存在するすべてのリソース (ディスク、仮想ネットワークなど) が一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-136">You should see a bunch of resources (disks, virtual networks, etc.) that all still exist.</span></span> 

```output
Microsoft.Compute/disks
Microsoft.Network/networkInterfaces
Microsoft.Network/networkSecurityGroups
Microsoft.Network/publicIPAddresses
Microsoft.Network/virtualNetworks
```

<span data-ttu-id="ad9f6-137">これは、`Remove-AzureRmVM` コマンドで削除されるのは _VM のみ_のためです。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-137">This is because the `Remove-AzureRmVM` command _just deletes the VM_.</span></span> <span data-ttu-id="ad9f6-138">他のリソースはまったくクリーンアップされません。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-138">It doesn't cleanup any of the other resources!</span></span> <span data-ttu-id="ad9f6-139">この時点では、リソース グループ自体を削除するだけで完了です。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-139">At this point, we'd likely just delete the Resource Group itself and be done with it.</span></span> <span data-ttu-id="ad9f6-140">手動でクリーンアップするには、この演習全体を実行してください。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-140">However, let's just run through the exercise to clean it up manually.</span></span> <span data-ttu-id="ad9f6-141">コマンドのパターンがわかるはずです。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-141">You should see a pattern in the commands.</span></span>

1. <span data-ttu-id="ad9f6-142">ネットワーク インターフェイスを削除します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-142">Delete the Network Interface.</span></span>

    ```powershell
    $vm | Remove-AzureRmNetworkInterface –Force
    ```
    
1. <span data-ttu-id="ad9f6-143">マネージド OS ディスクとストレージ アカウントを削除します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-143">Delete the managed OS disks and storage account</span></span>

    ```powershell
    Get-AzureRmDisk -ResourceGroupName $vm.ResourceGroupName -DiskName $vm.StorageProfile.OSDisk.Name | Remove-AzureRmDisk -Force
    ```

1. <span data-ttu-id="ad9f6-144">次に仮想ネットワークを削除します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-144">Next, delete the virtual network.</span></span>

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmVirtualNetwork -Force
    ```

1. <span data-ttu-id="ad9f6-145">ネットワーク セキュリティ グループを削除します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-145">Delete the network security group.</span></span>

    ```powershell
    Get-AzureRmNetworkSecurityGroup -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmNetworkSecurityGroup -Force
    ```

1. <span data-ttu-id="ad9f6-146">最後に、パブリック IP アドレスです。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-146">And finally, the public IP address.</span></span>

    ```powershell
    Get-AzureRmPublicIpAddress -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmPublicIpAddress -Force
    ```

<span data-ttu-id="ad9f6-147">リソース グループを見て、作成したすべてのリソースに対応したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-147">We should have caught all the created resources; check the resource group just to be sure.</span></span> <span data-ttu-id="ad9f6-148">ここでは多数の手動コマンドを実行しましたが、後でこのロジックを再利用して VM を作成または削除できるように、_スクリプト_を作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-148">We did a lot of manual commands here but a better approach would have been to write a _script_ so we could reuse this logic later to create or delete a VM.</span></span> <span data-ttu-id="ad9f6-149">PowerShell を使用したスクリプトを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="ad9f6-149">Let's look at scripting with PowerShell.</span></span>