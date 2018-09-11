<span data-ttu-id="a894c-101">新興企業の財務管理アプリケーションを開発しているものとします。</span><span class="sxs-lookup"><span data-stu-id="a894c-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="a894c-102">顧客のすべてのデータが保護されるようにするため、このアプリケーションをホストするサーバー上のすべての OS ディスクとデータ ディスクに ADE を実装することにしました。</span><span class="sxs-lookup"><span data-stu-id="a894c-102">You want to ensure that all your customers' data is secured, so you have decided to implement ADE across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="a894c-103">コンプライアンス要件の一部として、独自の暗号化キーの管理も自分で行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="a894c-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<span data-ttu-id="a894c-104">このユニットでは、既存の Windows VM 上のディスクを暗号化し、独自の Azure キー コンテナーを使用して暗号化キーを管理します。</span><span class="sxs-lookup"><span data-stu-id="a894c-104">In this unit you'll encrypt disks on existing Windows VMs, and manage the encryption keys using your own Azure Key Vault.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="a894c-105">この演習では、Azure PowerShell がお使いのコンピューターにインストールされていてるものとします。Azure PowerShell をインストールする方法については、「[PowerShellGet を使用した Windows への Azure PowerShell のインストール](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="a894c-105">This exercise assumes that Azure PowerShell is installed on your computer; go to [Install Azure PowerShell on Windows with PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0) for information on how to install Azure PowerShell.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="a894c-106">環境を準備する</span><span class="sxs-lookup"><span data-stu-id="a894c-106">Prepare the environment</span></span>

<span data-ttu-id="a894c-107">最初に、Windows VM を新しいリソース グループに展開した後、VM にデータ ディスクを追加します。</span><span class="sxs-lookup"><span data-stu-id="a894c-107">We'll start by deploying a Windows VM to a new resource group and then add a data disk to the VM.</span></span>

### <a name="deploy-windows-vm-using-azure-portal"></a><span data-ttu-id="a894c-108">Azure portal を使用して Windows VM を展開する</span><span class="sxs-lookup"><span data-stu-id="a894c-108">Deploy Windows VM using Azure portal</span></span>

<span data-ttu-id="a894c-109">ここでは、Azure portal を使用して Windows VM を作成および展開します。</span><span class="sxs-lookup"><span data-stu-id="a894c-109">Here you'll install use the Azure portal to create and deploy a Windows VM.</span></span> <span data-ttu-id="a894c-110">最初に、VM の基本的な情報を定義します。</span><span class="sxs-lookup"><span data-stu-id="a894c-110">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="a894c-111">ブラウザーで [Azure portal](http://portal.azure.com) に移動し、通常の資格情報でサインインします。</span><span class="sxs-lookup"><span data-stu-id="a894c-111">In a browser, navigate to the [Azure portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="a894c-112">サイドバーで **[Virtual Machines]** をクリックし、**[仮想マシンの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-112">In the sidebar, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="a894c-113">[Compute] ブレードの **[お勧め]** セクションで、**[Windows Server]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-113">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="a894c-114">**[Windows Server]** ブレードで、**[Windows Server 2016 Datacenter]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-114">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="a894c-115">**[Windows Server 2016 Datacenter]** ブレードで、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-115">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="a894c-116">**[基本]** ブレードで、**[名前]** ボックスに「**moneyappsvr01**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a894c-116">In the **Basics** blade, in the **Name** box, type **moneyappsvr01.**</span></span>

1. <span data-ttu-id="a894c-117">**[ユーザー名]** ボックスと **[パスワード]** ボックスに、このサーバーの管理者アカウントの名前とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="a894c-117">In the **Username** and **Password boxes**, type a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="a894c-118">**[サブスクリプション]** ボックスで Azure サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="a894c-118">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="a894c-119">**[リソース グループ]** で **[新規作成]** を選択し、ボックスに「**moneyapprg**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a894c-119">Under **Resource Group**, select **Create new**, and in the box, type **moneyapprg**.</span></span>

1. <span data-ttu-id="a894c-120">**[場所]** ドロップダウン リストで、近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="a894c-120">In the **Location** drop-down list, select a region near you.</span></span>

1. <span data-ttu-id="a894c-121">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-121">Click **OK**.</span></span>

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a><span data-ttu-id="a894c-122">VM のサイズを選択して展開を開始する</span><span class="sxs-lookup"><span data-stu-id="a894c-122">Choose a size for the VM, and start the deployment</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a894c-123">Basic レベルの VM は、ADE をサポートしていないことを思い出してください</span><span class="sxs-lookup"><span data-stu-id="a894c-123">Remember that basic tier VMs do not support ADE</span></span>

1. <span data-ttu-id="a894c-124">**[サイズの選択]** ブレードで、**[B1s]** などの **[Standard]** を選択してから、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-124">On the **Choose a size** blade, select a **Standard** SKU, such as **B1s**, and then click **Select**.</span></span>

1. <span data-ttu-id="a894c-125">**[設定]** ブレードの **[パブリック受信ポートを選択]** の一覧で **[RDP]** をクリックし、下にスクロールして **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-125">On the **Settings** blade, in the **Select public inbound ports** list, click **RDP**, and then scroll down and click **OK**.</span></span>

1. <span data-ttu-id="a894c-126">**[作成]** ブレードで、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-126">On the **Create** blade, click **Create**.</span></span>

1. <span data-ttu-id="a894c-127">VM が展開されるまで待ってから、演習を続けます。</span><span class="sxs-lookup"><span data-stu-id="a894c-127">Wait until the VM has deployed before continuing with the exercise.</span></span>

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="a894c-128">VM にデータ ディスクを追加する</span><span class="sxs-lookup"><span data-stu-id="a894c-128">Add a data disk to the VM</span></span>

1. <span data-ttu-id="a894c-129">左側のメニューで **[すべてのリソース]** をクリックし、**moneyappsvr01** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-129">In the left menu, click **All resources**, and then click **moneyappsvr01**.</span></span>

1. <span data-ttu-id="a894c-130">**[仮想マシン]** ブレードの **[設定]** で、**[ディスク]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-130">On the **Virtual machine** blade, under **SETTINGS**, click **Disks**.</span></span>

1. <span data-ttu-id="a894c-131">**[ディスク]** ブレードで、OS ディスクの暗号化の状態が現在は **[有効になっていません]** であることを確認し、**[データ ディスクの追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-131">On the **Disks** blade, note that the OS disk encryption status is currently **Not enabled**, and then click **Add data disk**.</span></span>

1. <span data-ttu-id="a894c-132">**[名前]** ボックスの一覧をクリックして、**[ディスクの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-132">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="a894c-133">**[マネージド ディスクの作成]** ブレードで、**[名前]** ボックスに「**moneyappsvr01_data**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a894c-133">In the **Create managed disk** blade, in the **Name** box, type **moneyappsvr01_data**.</span></span>

1. <span data-ttu-id="a894c-134">**[リソース グループ]** で **[既存のものを使用]** を選択し、一覧で **moneyapprg** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a894c-134">Under **Resource Group**, select **Use existing**, and in the list, select **moneyapprg**.</span></span>

1. <span data-ttu-id="a894c-135">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-135">Click **Create**.</span></span>

1. <span data-ttu-id="a894c-136">ディスクが作成されるまで待ってから、次に進みます。</span><span class="sxs-lookup"><span data-stu-id="a894c-136">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="a894c-137">**[ディスク]** ブレードで、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-137">On the **Disks** blade, click **Save**.</span></span> <span data-ttu-id="a894c-138">データ ディスクの暗号化の状態が現在は **_[有効になっていません]_** であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="a894c-138">Note that the data disk encryption status is currently **_Not enabled_**.</span></span>

## <a name="configure-disk-encryption-pre-requisites"></a><span data-ttu-id="a894c-139">ディスク暗号化の前提条件を構成する</span><span class="sxs-lookup"><span data-stu-id="a894c-139">Configure disk encryption pre-requisites</span></span>

<span data-ttu-id="a894c-140">次に、Azure Disk Encryption の前提条件構成スクリプトを使用して、ディスク暗号化のすべての前提条件を構成します。</span><span class="sxs-lookup"><span data-stu-id="a894c-140">You'll now use the Azure disk encryption prerequisites configuration script to configure all the disk encryption pre-requisites.</span></span> <span data-ttu-id="a894c-141">このスクリプトは、VM と同じリージョンにキー コンテナーを作成して準備します。</span><span class="sxs-lookup"><span data-stu-id="a894c-141">This script will create and prepare a Key Vault in the same region as your VM.</span></span>

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="a894c-142">Azure Disk Encryption の前提条件セットアップ スクリプトを準備する</span><span class="sxs-lookup"><span data-stu-id="a894c-142">Prepare the Azure Disk Encryption Prerequisite Setup Script</span></span>

1. <span data-ttu-id="a894c-143">[Azure Disk Encryption 前提条件セットアップ スクリプト](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)の GitHub ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="a894c-143">Go to the [Azure Disk Encryption Prerequisite Setup Script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)  GitHub page.</span></span>

1. <span data-ttu-id="a894c-144">GibHub ページで、**[Raw]\(未加工\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-144">On the GibHub page, click **Raw**.</span></span>

1. <span data-ttu-id="a894c-145">Ctrl + A キーを押してそのページのテキストすべてを選択し、Ctrl + C キーを押して、選択したテキストをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="a894c-145">Use CTRL-A to select all the text on the page and then use CTRL-C to copy all the text on the page to the clipboard.</span></span>

1. <span data-ttu-id="a894c-146">お使いのコンピューターで **[スタート]** をクリックし、**[Windows PowerShell ISE]** を参照します。</span><span class="sxs-lookup"><span data-stu-id="a894c-146">On your computer, click **Start**, then browse to **Windows PowerShell ISE**.</span></span>

1. <span data-ttu-id="a894c-147">**[Windows PowerShell ISE]** を右クリックし、**[管理者として実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-147">Right click **Windows PowerShell ISE** and click **Run as administrator**.</span></span>

1. <span data-ttu-id="a894c-148">[Administrator: Windows PowerShell ISE]\(管理者: Windows PowerShell ISE\) ウィンドウで、**[表示]** をクリックし、**[Show Script Pane]\(スクリプト ウィンドウの表示\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-148">In the Administrator: Windows PowerShell ISE window, click **View** and then click **Show Script Pane**.</span></span>

1. <span data-ttu-id="a894c-149">コピーしたテキストをスクリプト ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="a894c-149">Paste the copied text into the script pane.</span></span>

1. <span data-ttu-id="a894c-150">スクリプト ウィンドウで、次のコード ブロックを見つけます。</span><span class="sxs-lookup"><span data-stu-id="a894c-150">In the script pane, locate the following block of code:</span></span>

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. <span data-ttu-id="a894c-151">コード ブロックで、`$false` を `$true` に変更します。</span><span class="sxs-lookup"><span data-stu-id="a894c-151">In the code block, change `$false` to `$true`.</span></span>

1. <span data-ttu-id="a894c-152">**[ファイル]** をクリックし、**[名前を付けて保存]** をクリックして、スクリプトを保存するフォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="a894c-152">Click **File**, then click **Save As**, and navigate to the folder you'd like to use to save the script.</span></span>

1. <span data-ttu-id="a894c-153">**[ファイル名]** ボックスに「**ADEPrereqScript.ps1**」と入力し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-153">In the **File name** box, type **ADEPrereqScript.ps1**, and click **Save**.</span></span>

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a><span data-ttu-id="a894c-154">Azure Disk Encryption の前提条件セットアップ スクリプトを実行する</span><span class="sxs-lookup"><span data-stu-id="a894c-154">Run the Azure Disk Encryption Prerequisite Setup Script</span></span>

1. <span data-ttu-id="a894c-155">PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-155">In the  PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. <span data-ttu-id="a894c-156">PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-156">In the PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   <span data-ttu-id="a894c-157">**[実行ポリシーの変更]** ダイアログ ボックスが表示される場合は、**[すべてはい]** または **[はい]** (_[すべてはい]_ オプションが表示されない場合) をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-157">If you get an **Execution Policy Change** dialog box, click either **Yes to all** or **Yes** (if you do not get a _Yes to all_ option).</span></span>

1. <span data-ttu-id="a894c-158">PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-158">In the  PowerShell ISE console pane, type the following command, and press **ENTER**:</span></span>

   ```powershell
   Login-AzureRmAccount
   ```

1. <span data-ttu-id="a894c-159">Azure の資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="a894c-159">Enter your Azure credentials.</span></span>

1. <span data-ttu-id="a894c-160">**SubscriptionId** の文字列を選択して、クリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="a894c-160">Select your **SubscriptionId** string, and copy it to the clipboard.</span></span>

1. <span data-ttu-id="a894c-161">PowerShell ISE で **[ファイル]** をクリックし、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a894c-161">In the  PowerShell ISE, click **File**, and then click **Run**.</span></span>

1. <span data-ttu-id="a894c-162">コンソール ウィンドウの **[resourceGroupName:]** というプロンプトで、「**moneyapprg**」と入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-162">In the console pane, at the **resourceGroupName:** prompt, type  **moneyapprg**, and press **ENTER**.</span></span>

1. <span data-ttu-id="a894c-163">コンソール ウィンドウの **[keyVaultName:]** というプロンプトで、「**moneyappkv**」と入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-163">In the console pane, at the **keyVaultName:** prompt, type **moneyappkv**, and press **ENTER**.</span></span>

1. <span data-ttu-id="a894c-164">コンソール ウィンドウの **[location:]** というプロンプトで、VM の作成時に使用した場所を入力します。</span><span class="sxs-lookup"><span data-stu-id="a894c-164">In the console pane, at the **location:** prompt, type the location you used when creating your VM.</span></span>

1. <span data-ttu-id="a894c-165">コンソール ウィンドウの **[subscriptionId:]** というプロンプトで、お使いのサブスクリプション ID を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="a894c-165">In the console pane, at the **subscriptionId:** prompt, paste your subscription ID.</span></span>

1. <span data-ttu-id="a894c-166">**Moneyappkv** キー コンテナーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a894c-166">The **moneyappkv** key vault will now be created.</span></span> <span data-ttu-id="a894c-167">これが完了したら、(緑色の) 概要テキストを選択し、メモ帳にコピーします。</span><span class="sxs-lookup"><span data-stu-id="a894c-167">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="a894c-168">**Enter** キーを押して続行します。</span><span class="sxs-lookup"><span data-stu-id="a894c-168">Press **ENTER** to continue.</span></span>

1. <span data-ttu-id="a894c-169">コンソール ウィンドウの **[aadAppName:]** というプロンプトで、「**moneyapp**」と入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-169">In the console pane, at the **aadAppName:**  prompt, type **moneyapp**, and press **ENTER**.</span></span>

1. <span data-ttu-id="a894c-170">**moneyapp** Azure AD アプリケーションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a894c-170">The **moneyapp** Azure AD application will now be created.</span></span> <span data-ttu-id="a894c-171">これが完了したら、(緑色の) 概要テキストを選択し、メモ帳にコピーします。</span><span class="sxs-lookup"><span data-stu-id="a894c-171">When this has completed, select the summary text (in green), and copy it to Notepad.</span></span>

1. <span data-ttu-id="a894c-172">**Enter** キーを押して続行します。</span><span class="sxs-lookup"><span data-stu-id="a894c-172">Press **ENTER** to continue.</span></span>

### <a name="encrypt-your-vm-disks-with-powershell"></a><span data-ttu-id="a894c-173">PowerShell を使用して VM ディスクを暗号化する</span><span class="sxs-lookup"><span data-stu-id="a894c-173">Encrypt your VM disks with PowerShell</span></span>

<span data-ttu-id="a894c-174">OS ディスクとデータ ディスクの暗号化の状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="a894c-174">Verify the encryption status of the OS and data disks:</span></span>

1. <span data-ttu-id="a894c-175">PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-175">In the PowerShell ISE console pane, type the following command, and press ENTER:</span></span>

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > <span data-ttu-id="a894c-176">VM 名は、一重引用符で囲む必要があります。</span><span class="sxs-lookup"><span data-stu-id="a894c-176">The VM name must be enclosed in single quotes.</span></span>

1. <span data-ttu-id="a894c-177">PowerShell ISE スクリプト ウィンドウで、次のコマンドを入力して、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a894c-177">In the PowerShell ISE script pane, enter the following command, and press ENTER:</span></span>

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. <span data-ttu-id="a894c-178">**[Enable AzureDiskEncryption on the VM]\(VM で AzureDiskEncryption を有効にする\)** ダイアログ ボックスで **[はい]** をクリックし、暗号化が完了するまでに 10 ～ 15 分かかる場合があることを示すメッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="a894c-178">In the **Enable AzureDiskEncryption on the VM** dialog box, click **Yes**, and note the message that encryption may take 10-15 minutes to complete.</span></span>

>[!IMPORTANT]
> <span data-ttu-id="a894c-179">コマンドが完了するまで待ってから、この演習を続けます。</span><span class="sxs-lookup"><span data-stu-id="a894c-179">Wait until the command has completed before continuing with this exercise</span></span>

### <a name="verify-the-encryption-status-of-your-vm-disks"></a><span data-ttu-id="a894c-180">VM ディスクの暗号化の状態を確認する</span><span class="sxs-lookup"><span data-stu-id="a894c-180">Verify the encryption status of your VM disks</span></span>

<span data-ttu-id="a894c-181">Azure portal に切り替えて、**moneyappsvr01** の **[ディスク]** ブレードで、OS ディスクとデータ ディスクのディスク暗号化の状態が **[有効]** になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a894c-181">Switch to the Azure portal, and on the **Disks** blade for **moneyappsvr01**, note that the disk encryption status for the OS and Data disks is now **Enabled**.</span></span>
