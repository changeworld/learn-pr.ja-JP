<span data-ttu-id="31e90-101">この演習では、仮想マシンを作成した後、ポータルと Azure PowerShell を使用してそのサイズを変更します。</span><span class="sxs-lookup"><span data-stu-id="31e90-101">In this exercise, you will create a virtual machine and then resize it using the portal and Azure PowerShell.</span></span>

## <a name="create-a-vm"></a><span data-ttu-id="31e90-102">VM を作成する</span><span class="sxs-lookup"><span data-stu-id="31e90-102">Create a VM</span></span>

1. <span data-ttu-id="31e90-103">Web ブラウザーで [Azure portal](https://portal.azure.com?azure-portal=true) に移動し、自分のアカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="31e90-103">In your web browser, navigate to the [Azure Portal](https://portal.azure.com?azure-portal=true) and sign into your account.</span></span>

1. <span data-ttu-id="31e90-104">Azure portal で、新しいリソースを作成します。</span><span class="sxs-lookup"><span data-stu-id="31e90-104">In the Azure portal, create a new resource.</span></span> <span data-ttu-id="31e90-105">**[新規]** ブレードで、検索ボックスに「**virtual machine**」と入力して、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="31e90-105">In the **New** blade, type **virtual machine** in the search box, and then press ENTER.</span></span>

1. <span data-ttu-id="31e90-106">**[Everything]** ブレードの **[結果]** で、**Windows Server 2016 Datacenter** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-106">In the **Everything** blade, under **Results**, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="31e90-107">**Windows Server 2016 Datacenter** のブレードで、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-107">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="31e90-108">**[基本]** ブレードで次の情報を使用して詳細を設定し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-108">In the **Basics** blade, complete the details using the following information, and then click **OK**.</span></span>

    |<span data-ttu-id="31e90-109">設定</span><span class="sxs-lookup"><span data-stu-id="31e90-109">Setting</span></span>|<span data-ttu-id="31e90-110">値</span><span class="sxs-lookup"><span data-stu-id="31e90-110">Value</span></span>|
    |---|---|
    |<span data-ttu-id="31e90-111">名前</span><span class="sxs-lookup"><span data-stu-id="31e90-111">Name</span></span>|<span data-ttu-id="31e90-112">DB01</span><span class="sxs-lookup"><span data-stu-id="31e90-112">DB01</span></span>|
    |<span data-ttu-id="31e90-113">ユーザー名</span><span class="sxs-lookup"><span data-stu-id="31e90-113">Username</span></span>|<span data-ttu-id="31e90-114">LocalAdmin</span><span class="sxs-lookup"><span data-stu-id="31e90-114">LocalAdmin</span></span>|
    |<span data-ttu-id="31e90-115">パスワードとパスワードの確認</span><span class="sxs-lookup"><span data-stu-id="31e90-115">Password and Confirm password</span></span>|<span data-ttu-id="31e90-116">Adm1nPa$$word</span><span class="sxs-lookup"><span data-stu-id="31e90-116">Adm1nPa$$word</span></span>|
    |<span data-ttu-id="31e90-117">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="31e90-117">Resource group</span></span>|<span data-ttu-id="31e90-118">ExerciseRG</span><span class="sxs-lookup"><span data-stu-id="31e90-118">ExerciseRG</span></span>|
    |<span data-ttu-id="31e90-119">場所</span><span class="sxs-lookup"><span data-stu-id="31e90-119">Location</span></span>|<span data-ttu-id="31e90-120">米国中央部</span><span class="sxs-lookup"><span data-stu-id="31e90-120">Central US</span></span>|

1. <span data-ttu-id="31e90-121">**[サイズの選択]** ブレードで、**[D2s_v3]** を選択して、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-121">On the **Choose a size** blade, select **D2s_v3**, and then click **Select**.</span></span>

1. <span data-ttu-id="31e90-122">**[設定]** ブレードでは、**[パブリック受信ポートを選択]** で **[HTTP]**、**[HTTPS]**、**[RDP (3389)]** を選択し、**[Boot diagnostics]\(ブート診断\)** で **[無効]** をクリックし、他のすべての設定は既定値のままにして、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-122">On the **Settings** blade, under **Select public inbound ports** select **HTTP**, **HTTPS**, and **RDP (3389)**, under **Boot diagnostics** click **Disabled**, leave all other settings at the default value, and then click **OK**.</span></span>

1. <span data-ttu-id="31e90-123">**[作成]** ブレードで、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-123">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="31e90-124">展開が完了するまで待ってから、演習を続行します。</span><span class="sxs-lookup"><span data-stu-id="31e90-124">Wait until the deployment is complete before continuing the exercise.</span></span>

## <a name="resize-using-the-portal"></a><span data-ttu-id="31e90-125">ポータルを使用してサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="31e90-125">Resize using the portal</span></span>

1. <span data-ttu-id="31e90-126">Azure portal で ExerciseRG リソース グループを参照し、**ExerciseRG** ブレードで **DB01** 仮想マシン オブジェクトをクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-126">In the Azure portal, browse to the ExerciseRG resource group, and in the **ExerciseRG** blade, click the **DB01** virtual machine object.</span></span>

1. <span data-ttu-id="31e90-127">**DB01** ブレードで、**[サイズ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-127">On the **DB01** blade, click **Size**.</span></span> <span data-ttu-id="31e90-128">現在強調表示されているサイズが、仮想マシンの作成時に選択したサイズであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="31e90-128">Note the currently highlighted size is the size you selected when creating the virtual machine.</span></span>

1. <span data-ttu-id="31e90-129">**[サイズの選択]** ブレードで **F2s_v2** サイズを探します。VM が現在実行されていて、このサイズは異なるファミリのものなので、サイズの一覧ではこのサイズを使用できません。</span><span class="sxs-lookup"><span data-stu-id="31e90-129">On the **Choose a size** blade, try to find the **F2s_v2** size - it should not be available in the list of sizes because the VM is currently running and that size is from a different family.</span></span> <span data-ttu-id="31e90-130">**[サイズの選択]** ブレードを閉じます。</span><span class="sxs-lookup"><span data-stu-id="31e90-130">Close the **Choose a size** blade.</span></span>

1. <span data-ttu-id="31e90-131">**DB01** ブレードで、**[停止]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-131">In the **DB01** blade, click **Stop**.</span></span> <span data-ttu-id="31e90-132">**[この仮想マシンの停止]** ダイアログ ボックスで **[はい]** をクリックし、仮想マシンの状態が **[停止済み (割り当て解除)]** と表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="31e90-132">In the **Stop this virtual machine** dialog box, click **Yes**, and wait for the virtual machine status to show **Stopped (deallocated)**.</span></span>

1. <span data-ttu-id="31e90-133">**DB01** ブレードで、**[サイズ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-133">On the **DB01** blade, click **Size**.</span></span> <span data-ttu-id="31e90-134">**[サイズの選択]** ブレードで、**[F2s_v2]** を選択して、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-134">On the **Choose a size** blade, select **F2s_v2** and then click **Select**.</span></span> <span data-ttu-id="31e90-135">仮想マシンのサイズ変更についての通知を確認します。</span><span class="sxs-lookup"><span data-stu-id="31e90-135">Notice the notification about resizing the virtual machine.</span></span>

1. <span data-ttu-id="31e90-136">**DB01** ブレードで **[概要]** をクリックし、**[開始]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="31e90-136">On the **DB01** blade, click **Overview**, then click **Start**.</span></span>

## <a name="resize-using-powershell"></a><span data-ttu-id="31e90-137">PowerShell を使用してサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="31e90-137">Resize using PowerShell</span></span>

1. <span data-ttu-id="31e90-138">Azure portal で Cloud Shell を開きます。</span><span class="sxs-lookup"><span data-stu-id="31e90-138">In the Azure portal, open the Cloud Shell.</span></span>

1. <span data-ttu-id="31e90-139">次のコマンドレットを使用して、使用可能な仮想マシンのサイズの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="31e90-139">Use the following cmdlet to get the list of available virtual machine sizes.</span></span>

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. <span data-ttu-id="31e90-140">次のコマンドレットを使用して、仮想マシンのサイズを F4s_v2 に変更します。</span><span class="sxs-lookup"><span data-stu-id="31e90-140">Use the following cmdlet to resize the virtual machine to an F4s_v2 size.</span></span>

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. <span data-ttu-id="31e90-141">PowerShell コマンドの完了を待機している間に、DB01 ブレードの [更新] ボタンをクリックします。仮想マシンが再起動し、サイズの変更が反映されることがわかります。</span><span class="sxs-lookup"><span data-stu-id="31e90-141">Click the Refresh button in the DB01 blade while you are waiting for the PowerShell command to complete - you should notice that the virtual machine is restarting to accommodate the change in size.</span></span>

<span data-ttu-id="31e90-142">この演習では、仮想マシンを作成し、2 つの異なるツールを使用してサイズを変更しました。</span><span class="sxs-lookup"><span data-stu-id="31e90-142">In this exercise, you created a virtual machine and resized it with two different tools.</span></span> <span data-ttu-id="31e90-143">覚えておくとよいヒントは、仮想マシンの実行中は目的のサイズを使用できない可能性があるということです。仮想マシンを停止すると、より多くのサイズを選択できます。</span><span class="sxs-lookup"><span data-stu-id="31e90-143">A good tip to keep in mind is that the target size may not be available while the virtual machine is running; stopping the virtual machine lets you choose more sizes.</span></span>