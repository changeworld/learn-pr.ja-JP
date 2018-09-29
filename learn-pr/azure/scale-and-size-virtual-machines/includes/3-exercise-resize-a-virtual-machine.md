<span data-ttu-id="14816-101">この演習では、仮想マシンを作成した後、ポータルと Azure PowerShell を使用してそのサイズを変更します。</span><span class="sxs-lookup"><span data-stu-id="14816-101">In this exercise, you will create a virtual machine and then resize it using the portal and Azure PowerShell.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-vm-with-powershell"></a><span data-ttu-id="14816-102">PowerShell を使用して VM を作成する</span><span class="sxs-lookup"><span data-stu-id="14816-102">Create a VM with PowerShell</span></span>

1. <span data-ttu-id="14816-103">Azure PowerShell で `New-AzureRmVm` コマンドレットを使用して、VM を作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="14816-103">Let's use the `New-AzureRmVm` cmdlet in Azure PowerShell to create a VM.</span></span>
    - <span data-ttu-id="14816-104">**<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="14816-104">Use the Resource Group **<rgn>[sandbox resource group name]</rgn>**.</span></span>
    - <span data-ttu-id="14816-105">これに "my-test-vm" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="14816-105">Name it "my-test-vm".</span></span>
    - <span data-ttu-id="14816-106">**[サイズ]** パラメーターの _Standard_DS2_v2_ を渡します。</span><span class="sxs-lookup"><span data-stu-id="14816-106">Pass in _Standard_DS2_v2_ for the **Size** parameter.</span></span>
    - <span data-ttu-id="14816-107">Azure サンドボックスで使用できる次のリストから、自分に近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="14816-107">Select a location close to you from the following list available in the Azure sandbox.</span></span> <span data-ttu-id="14816-108">コピーと貼り付けを使用する場合は、次のコマンド例の値を必ず変更してください。</span><span class="sxs-lookup"><span data-stu-id="14816-108">Make sure to change the value in the below example command if you are using copy and paste.</span></span>

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - <span data-ttu-id="14816-109">`Get-Credential` コマンドレットを使用し、次のように結果を `Credential` パラメーターに指定します。</span><span class="sxs-lookup"><span data-stu-id="14816-109">Use the `Get-Credential` cmdlet and feed the results into the `Credential` parameter as shown below.</span></span>

       <span data-ttu-id="14816-110">資格情報の入力を求められたら、ユーザー名として LocalAdmin、パスワードとして Adm1nPa$$word を使用します。</span><span class="sxs-lookup"><span data-stu-id="14816-110">When prompted to enter credentials, use LocalAdmin as the user name and Adm1nPa$$word as the password.</span></span>

    ```powershell
    New-AzureRmVm `
        -ResourceGroupName <rgn>[sandbox resource group name]</rgn> `
        -Name my-test-vm `
        -Credential (Get-Credential) `
        -Size "Standard_DS2_v2" `
        -Location eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


1. <span data-ttu-id="14816-111">デプロイが完了するまで待ってから、演習を続行します。</span><span class="sxs-lookup"><span data-stu-id="14816-111">Wait until the deployment is complete before continuing the exercise.</span></span>

## <a name="resize-using-the-portal"></a><span data-ttu-id="14816-112">ポータルを使用してサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="14816-112">Resize using the portal</span></span>

1. <span data-ttu-id="14816-113">サンドボックスをアクティブ化したときと同じアカウントを使用して、[サンドボックスの Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="14816-113">Sign into the [Azure portal for sandbox](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="14816-114">左側のバーから **[リソース グループ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="14816-114">Select **Resource Groups** from the left side bar.</span></span>

1. <span data-ttu-id="14816-115"><rgn>[サンドボックス リソース グループ名]</rgn> というリソース グループを選択し、概要で **[my-test-vm]** 仮想マシン オブジェクトをクリックします。</span><span class="sxs-lookup"><span data-stu-id="14816-115">Select the <rgn>[sandbox resource group name]</rgn> resource group, and in the overview, click the **my-test-vm** virtual machine object.</span></span>

1. <span data-ttu-id="14816-116">仮想マシンの **[概要]** では、VM の現在のサイズが、先ほど作成した _Standard DS2 v2 (2 vCPU、7 GB メモリ)_ になっていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="14816-116">On the **Overview** of the virtual machine, notice that the current size of the VM is _Standard DS2 v2 (2 vcpus, 7 GB memory)_ which is what we created a moment ago.</span></span>

1. <span data-ttu-id="14816-117">**[設定]** セクションで、**[サイズ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="14816-117">In the **Settings** section, select **Size**.</span></span>

1. <span data-ttu-id="14816-118">**[サイズの選択]** ブレードで、**F2s_v2** というサイズを探してみてください。</span><span class="sxs-lookup"><span data-stu-id="14816-118">On the **Choose a size** blade, try to find the **F2s_v2** size.</span></span> <span data-ttu-id="14816-119">VM は現在実行中であり、そのサイズが別のファミリのものであるため、利用可能なサイズのリストには表示されません。</span><span class="sxs-lookup"><span data-stu-id="14816-119">You will not see it in the list of available sizes because the VM is currently running and that size is from a different family.</span></span> <span data-ttu-id="14816-120">利用可能な VM サイズをすべて表示するには、VM の停止が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="14816-120">In some cases, you will need to stop the VM in order to see all available VM sizes.</span></span>

1. <span data-ttu-id="14816-121">この VM の実行中に現在利用できるサイズを選択してみましょう。</span><span class="sxs-lookup"><span data-stu-id="14816-121">Let's choose a size that is currently available while this VM is running.</span></span> <span data-ttu-id="14816-122">引き続き **[サイズの選択]** ブレードで、_DS3_v2 Standard_ を選んでから **[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="14816-122">While still on the **Choose a size** blade, select _DS3_v2 Standard_ and then click **Select**.</span></span> <span data-ttu-id="14816-123">仮想マシンのサイズ変更についての通知に注目してください。</span><span class="sxs-lookup"><span data-stu-id="14816-123">Notice the notification about resizing the virtual machine.</span></span>

1. <span data-ttu-id="14816-124">**[概要]** パネルで、VM のサイズが _Standard DS3 v2 (4 vCPU、14 GB メモリ)_ に変更されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="14816-124">On the **Overview** panel, confirm that the VM has been resized to _Standard DS3 v2 (4 vcpus, 14 GB memory)_.</span></span>

## <a name="resize-using-powershell"></a><span data-ttu-id="14816-125">PowerShell を使用してサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="14816-125">Resize using PowerShell</span></span>

1. <span data-ttu-id="14816-126">Azure portal で、上部のツール バーにある Cloud Shell ボタンをクリックして、Azure Cloud Shell を開きます。</span><span class="sxs-lookup"><span data-stu-id="14816-126">In the Azure portal, open Azure Cloud Shell by clicking the Cloud Shell button in the top toolbar.</span></span>

    <span data-ttu-id="14816-127">Cloud Shell ウィンドウの左上で、Cloud Shell が Bash ではなく確実に PowerShell を使用するように設定します。</span><span class="sxs-lookup"><span data-stu-id="14816-127">Ensure that the Cloud Shell is set to use PowerShell and not Bash, in the top left of the Cloud Shell window.</span></span>

1. <span data-ttu-id="14816-128">次のコマンドレットを使用して、利用可能な仮想マシン サイズのリストを取得します。</span><span class="sxs-lookup"><span data-stu-id="14816-128">Use the following cmdlet to get the list of available virtual machine sizes.</span></span>

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    ```

1. <span data-ttu-id="14816-129">次のコマンドレットを使用して、仮想マシンのサイズを _DS2_v2_ に戻します。</span><span class="sxs-lookup"><span data-stu-id="14816-129">Use the following cmdlet to resize the virtual machine back to a _DS2_v2_ size.</span></span>

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    $vm.HardwareProfile.VmSize = "Standard_DS2_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```

1. <span data-ttu-id="14816-130">PowerShell コマンドが完了するのを待っている間に、**[my-test-vm]** ブレードの **[更新]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="14816-130">Click the **Refresh** button in the **my-test-vm** blade while you are waiting for the PowerShell command to finish.</span></span> <span data-ttu-id="14816-131">仮想マシンが再起動し、サイズの変更が反映されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="14816-131">You should notice that the virtual machine is restarting to accommodate the change in size.</span></span>

<span data-ttu-id="14816-132">この演習では、仮想マシンを作成し、2 つの異なるツールを使用してサイズを変更しました。</span><span class="sxs-lookup"><span data-stu-id="14816-132">In this exercise, you created a virtual machine and resized it with two different tools.</span></span> <span data-ttu-id="14816-133">覚えておくとよいヒントは、仮想マシンの実行中は目的のサイズを使用できない可能性があるということです。仮想マシンを停止すると、より多くのサイズを選択できます。</span><span class="sxs-lookup"><span data-stu-id="14816-133">A good tip to keep in mind is that the target size may not be available while the virtual machine is running; stopping the virtual machine lets you choose more sizes.</span></span>