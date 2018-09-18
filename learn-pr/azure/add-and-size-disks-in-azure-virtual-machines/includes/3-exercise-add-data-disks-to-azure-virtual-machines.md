<span data-ttu-id="2e102-101">この演習では、会社で簡易メール転送プロトコル (SMTP) メール サーバーを実行していると想定します。</span><span class="sxs-lookup"><span data-stu-id="2e102-101">In this exercise, let's assume your company runs a Simple Mail Transfer Protocol (SMTP) email server.</span></span> <span data-ttu-id="2e102-102">このサーバーを Azure に移行したいと考えています。</span><span class="sxs-lookup"><span data-stu-id="2e102-102">You want to migrate this server into Azure.</span></span> <span data-ttu-id="2e102-103">また、SMTP サーバーで、自社ドメインの受信メッセージを、専用の VHD 上にある "Drop" というフォルダーに保存したいと考えています。</span><span class="sxs-lookup"><span data-stu-id="2e102-103">You want the SMTP server to store incoming messages for your own domain in a folder called "Drop" on a dedicated VHD.</span></span>

<span data-ttu-id="2e102-104">この演習の目標は、Windows 仮想マシン (VM) を作成し、"Drop" ディレクトリを保存する "Incoming" という新しい仮想ハード ディスク (VHD) を接続することです。</span><span class="sxs-lookup"><span data-stu-id="2e102-104">The goal of the exercise is to create a Windows virtual machine (VM) and attach a new virtual hard disk (VHD) called "Incoming" to store the "Drop" directory.</span></span>

## <a name="sign-in-to-azure"></a><span data-ttu-id="2e102-105">Azure へのサインイン</span><span class="sxs-lookup"><span data-stu-id="2e102-105">Sign in to Azure</span></span>
<!---TODO: Need update for sanbox?--->

1. <span data-ttu-id="2e102-106">[Azure portal](https://portal.azure.com/?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="2e102-106">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

## <a name="create-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="2e102-107">Azure portal で Windows VM を作成する</span><span class="sxs-lookup"><span data-stu-id="2e102-107">Create a Windows VM in the Azure portal</span></span>

<span data-ttu-id="2e102-108">データ ドライブで SMTP サーバーをホストする VM を作成するには、次の手順を行います。</span><span class="sxs-lookup"><span data-stu-id="2e102-108">To create a VM to host the SMTP server with its data drives, follow these steps:</span></span>

1. <span data-ttu-id="2e102-109">Azure portal の左上にある **[リソースの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-109">Choose **Create a resource** in the upper left corner of the Azure portal.</span></span>

1. <span data-ttu-id="2e102-110">Azure Marketplace リソースの一覧の上にある検索ボックスで **Windows Server 2016 Datacenter** を検索して選択し、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-110">In the search box above the list of Azure Marketplace resources, search for and select **Windows Server 2016 Datacenter**, and then choose **Create**.</span></span>

1. <span data-ttu-id="2e102-111">右側に表示される **[Basics]** ウィンドウで、次のプロパティ値を入力します。</span><span class="sxs-lookup"><span data-stu-id="2e102-111">In the **Basics** pane that opens to the right, enter the following property values.</span></span> 


|<span data-ttu-id="2e102-112">プロパティ</span><span class="sxs-lookup"><span data-stu-id="2e102-112">Property</span></span>  |<span data-ttu-id="2e102-113">値</span><span class="sxs-lookup"><span data-stu-id="2e102-113">Value</span></span>  |<span data-ttu-id="2e102-114">メモ</span><span class="sxs-lookup"><span data-stu-id="2e102-114">Notes</span></span>  |
|---------|---------|---------|
|<span data-ttu-id="2e102-115">名前</span><span class="sxs-lookup"><span data-stu-id="2e102-115">Name</span></span>     |   <span data-ttu-id="2e102-116">**MailSenderVM**</span><span class="sxs-lookup"><span data-stu-id="2e102-116">**MailSenderVM**</span></span>      |         |
|<span data-ttu-id="2e102-117">VM ディスクの種類</span><span class="sxs-lookup"><span data-stu-id="2e102-117">VM disk type</span></span>     |  <span data-ttu-id="2e102-118">**Standard HDD**</span><span class="sxs-lookup"><span data-stu-id="2e102-118">**Standard HDD**</span></span>       |   <span data-ttu-id="2e102-119">ドロップダウン リストからこの値を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-119">Select this value from the dropdown.</span></span>      |
|<span data-ttu-id="2e102-120">ユーザー名</span><span class="sxs-lookup"><span data-stu-id="2e102-120">User name</span></span>     |  <span data-ttu-id="2e102-121">**mailmaster**</span><span class="sxs-lookup"><span data-stu-id="2e102-121">**mailmaster**</span></span>       |         |
|<span data-ttu-id="2e102-122">パスワード</span><span class="sxs-lookup"><span data-stu-id="2e102-122">Password</span></span>     |  <span data-ttu-id="2e102-123">パスワードは 12 文字以上で、[定義された複雑さの要件](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm)を満たす必要があります。</span><span class="sxs-lookup"><span data-stu-id="2e102-123">The password must be at least 12 characters long and meet the [defined complexity requirements](https://docs.microsoft.com/azure/virtual-machines/windows/faq#what-are-the-password-requirements-when-creating-a-vm).</span></span>       | <span data-ttu-id="2e102-124">このユーザー名とパスワードはモジュール全体で使用するため、記憶しておいてください。</span><span class="sxs-lookup"><span data-stu-id="2e102-124">Make sure to remember this user name and password because we'll use them throughout the module.</span></span>         |
|<span data-ttu-id="2e102-125">サブスクリプション</span><span class="sxs-lookup"><span data-stu-id="2e102-125">Subscription</span></span>     |  <span data-ttu-id="2e102-126">サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-126">Choose your subscription.</span></span>       |  <span data-ttu-id="2e102-127">ドロップダウン リストからこの値を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-127">Select this value from the dropdown.</span></span>       |
|<span data-ttu-id="2e102-128">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="2e102-128">Resource group</span></span>     |  <span data-ttu-id="2e102-129">**[新規作成]** を選択して、「**MailInfrastructure**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="2e102-129">Select **Create new**, and then type **MailInfrastructure**.</span></span>       |  <span data-ttu-id="2e102-130">このモジュールで使用されるすべてのリソースを 1 つのリソース グループに集めます。</span><span class="sxs-lookup"><span data-stu-id="2e102-130">We'll gather all resource used in this module into one resource group.</span></span>       |
|<span data-ttu-id="2e102-131">場所</span><span class="sxs-lookup"><span data-stu-id="2e102-131">Location</span></span>     |   <span data-ttu-id="2e102-132">お近くの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-132">A location near you.</span></span>      | <span data-ttu-id="2e102-133">ドロップダウン リストからこの値を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-133">Select this value from the dropdown.</span></span>        |

4. <span data-ttu-id="2e102-134">**[Basics]** ページの下部にある **[OK]** を選択して、**[サイズ]** 構成パネルに進みます。</span><span class="sxs-lookup"><span data-stu-id="2e102-134">Select **OK** at the bottom of the **Basics** page to continue to the **Size** configuration pane.</span></span>

1. <span data-ttu-id="2e102-135">**[サイズ]** 構成ウィンドウで、**B1ms** を検索して選択し、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-135">In the **Size** configuration pane, search for and select **B1ms**, and then click **Select**.</span></span>

1. <span data-ttu-id="2e102-136">**[設定]** ウィンドウの **[マネージド ディスクの使用]** で **[いいえ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-136">In the **Settings** pane, under **Use managed disks**, click **No**.</span></span> <span data-ttu-id="2e102-137">マネージド ディスクについては、このモジュールで後述します。</span><span class="sxs-lookup"><span data-stu-id="2e102-137">We'll discuss managed disks later in this module.</span></span>

1. <span data-ttu-id="2e102-138">**[パブリック受信ポートを選択]** で **[RDP (3389)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-138">In the **Select public inbound ports** dropdown list, select **RDP (3389)**.</span></span> <span data-ttu-id="2e102-139">作成後、このポートを使用して VM にリモート接続します。</span><span class="sxs-lookup"><span data-stu-id="2e102-139">We'll use this port to remote into our VM after it's created.</span></span>

1. <span data-ttu-id="2e102-140">他のすべての設定を既定値のままにして、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-140">Leave all the other settings at their default, and then click **OK**.</span></span>

1. <span data-ttu-id="2e102-141">**[作成]** ウィンドウで、構成を確認します。</span><span class="sxs-lookup"><span data-stu-id="2e102-141">In the **Create** pane, review the configuration.</span></span>

1. <span data-ttu-id="2e102-142">構成を確認したら、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-142">When you have reviewed the configuration,  select **Create**.</span></span> <span data-ttu-id="2e102-143">Azure で新しい VM を作成して起動します。</span><span class="sxs-lookup"><span data-stu-id="2e102-143">Azure creates and starts the new VM.</span></span>

> [!TIP]
> <span data-ttu-id="2e102-144">VM を作成して Azure にデプロイするには、数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="2e102-144">Creating your VM and deploying it in Azure can take a few minutes.</span></span> <span data-ttu-id="2e102-145">進行状況は、**[通知]** ハブで見ることができます。</span><span class="sxs-lookup"><span data-stu-id="2e102-145">You can watch the progress in the **Notifications** hub.</span></span> <span data-ttu-id="2e102-146">Azure では、終了時に通知ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2e102-146">Azure will display a notification dialog when it finishes.</span></span>

## <a name="add-an-empty-data-disk-to-our-vm"></a><span data-ttu-id="2e102-147">空のデータ ディスクを VM に追加する</span><span class="sxs-lookup"><span data-stu-id="2e102-147">Add an empty data disk to our VM</span></span>

<span data-ttu-id="2e102-148">SMTP サーバーの "受信" 用のディスク ストアに "Drop" ディレクトリと名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="2e102-148">We're going to name the disk stores the "Drop" directory for your SMTP server "Incoming".</span></span> <span data-ttu-id="2e102-149">次の手順で新しい空のディスクをサーバーに追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="2e102-149">Let's add a new empty disk to the server using the following steps:</span></span>

1. <span data-ttu-id="2e102-150">左側のナビゲーションの **[お気に入り]** で、**[Virtual Machines]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-150">In the navigation on the left, under **FAVORITES**, select **Virtual machines**.</span></span>

1. <span data-ttu-id="2e102-151">VM の一覧で、**[MailSenderVM]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-151">In the list of VMs, select **MailSenderVM**.</span></span>

1. <span data-ttu-id="2e102-152">左側の **[MailSenderVM]** 構成メニューの **[設定]** で、**[ディスク]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-152">Under **SETTINGS** of the **MailSenderVM** configuration menu on the left, select **Disks**.</span></span>

1. <span data-ttu-id="2e102-153">**[データ ディスク]** で、**[データ ディスクの追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-153">Under **Data disks**, select **Add data disk**.</span></span>

1. <span data-ttu-id="2e102-154">**[アンマネージド ディスクの接続]** ウィンドウで、次のプロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="2e102-154">In the **Attach unmanaged disks** pane, set the following properties.</span></span>


|<span data-ttu-id="2e102-155">プロパティ</span><span class="sxs-lookup"><span data-stu-id="2e102-155">Property</span></span>  |<span data-ttu-id="2e102-156">値</span><span class="sxs-lookup"><span data-stu-id="2e102-156">Value</span></span>  |<span data-ttu-id="2e102-157">メモ</span><span class="sxs-lookup"><span data-stu-id="2e102-157">Notes</span></span>  |
|---------|---------|---------|
|<span data-ttu-id="2e102-158">名前</span><span class="sxs-lookup"><span data-stu-id="2e102-158">Name</span></span>     |   <span data-ttu-id="2e102-159">**MailSenderVMIncoming**</span><span class="sxs-lookup"><span data-stu-id="2e102-159">**MailSenderVMIncoming**</span></span>      |         |
|<span data-ttu-id="2e102-160">ソースの種類</span><span class="sxs-lookup"><span data-stu-id="2e102-160">Source type</span></span>     |  <span data-ttu-id="2e102-161">**新規 (空のディスク)**</span><span class="sxs-lookup"><span data-stu-id="2e102-161">**New (empty disk)**</span></span>       |   <span data-ttu-id="2e102-162">ドロップダウン リストからこの値を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-162">Select this value from the dropdown.</span></span>       |
|<span data-ttu-id="2e102-163">アカウントの種類</span><span class="sxs-lookup"><span data-stu-id="2e102-163">Account type</span></span>     |  <span data-ttu-id="2e102-164">**Standard HDD**</span><span class="sxs-lookup"><span data-stu-id="2e102-164">**Standard HDD**</span></span>       |  <span data-ttu-id="2e102-165">ドロップダウン リストからこの値を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-165">Select this value from the dropdown.</span></span>        |


6. <span data-ttu-id="2e102-166">**[ストレージ コンテナー]** フィールドの左で、**[参照]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-166">To the left of the **Storage container** field, select **Browse**.</span></span>

1. <span data-ttu-id="2e102-167">ストレージ アカウントの一覧で、**mailinfrastructure** から始まる名前のストレージ アカウントを検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-167">In the list of storage accounts, search for the storage account whose name begins with **mailinfrastructure** and select it.</span></span>

1. <span data-ttu-id="2e102-168">コンテナーの一覧で **[vhd]** をクリックし、**[選択]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-168">In the list of containers, click **vhds** and then choose **Select**.</span></span>

1. <span data-ttu-id="2e102-169">**[アンマネージド ディスクの接続]** 画面に戻り、**[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-169">Back on the **Attach unmanaged disk** screen, select **OK**.</span></span>

1. <span data-ttu-id="2e102-170">**[MailSenderVM - ディスク]** 画面に戻り、**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-170">Back on the **MailSenderVM - Disks** screen, select **Save**.</span></span>

<span data-ttu-id="2e102-171">ここでは、**MainSenderVMIncoming** というディスクを定義しました。</span><span class="sxs-lookup"><span data-stu-id="2e102-171">We've now defined a disk called **MainSenderVMIncoming**.</span></span> <span data-ttu-id="2e102-172">ディスクを使用するには、まず VM にログインするときにディスクをパーティション分割してフォーマットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2e102-172">To use the disk, we'll first need to partition and format it when we log into the VM.</span></span> 

## <a name="partition-and-format-a-data-disk"></a><span data-ttu-id="2e102-173">データ ディスクのパーティション分割とフォーマット</span><span class="sxs-lookup"><span data-stu-id="2e102-173">Partition and format a data disk</span></span>

<span data-ttu-id="2e102-174">物理ディスクと同様に、使用する前に VHD 上のパーティションを初期化してフォーマットします。</span><span class="sxs-lookup"><span data-stu-id="2e102-174">As with physical disks, initiate and format a partition on a VHD before it can be used.</span></span>

### <a name="log-into-our-windows-vm-using-rdp"></a><span data-ttu-id="2e102-175">RDP を使用して Windows VM にログインする</span><span class="sxs-lookup"><span data-stu-id="2e102-175">Log into our Windows VM using RDP</span></span>

1. <span data-ttu-id="2e102-176">**MailSenderVM** 仮想マシンのメイン画面で、**[概要]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-176">In the **MailSenderVM** virtual machine main screen, select **Overview**.</span></span>

1. <span data-ttu-id="2e102-177">概要画面の左上から **[接続]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-177">Select **Connect** from the top left of the overview screen.</span></span>

1. <span data-ttu-id="2e102-178">右側に表示される **[仮想マシンに接続する]** ダイアログで、**[Download to RDP File]\(RDP ファイルにダウンロード\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-178">In the **Connect to virtual machine** dialog that opens on the right, select **Download to RDP File**.</span></span>

   ![[Download to RDP File]\(RDP ファイルにダウンロード\) ボタンが強調表示された [仮想マシンに接続する] ダイアログのスクリーンショット。](../media-draft/download-rdp.png)

4. <span data-ttu-id="2e102-180">**MailSenderVM.rdp** というファイルがローカルの `Downloads` フォルダーにダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="2e102-180">A file called **MailSenderVM.rdp** is downloaded to your local `Downloads` folder.</span></span> <span data-ttu-id="2e102-181">このファイルは、MailSenderVM 仮想マシンのリモート デスクトップ構成ファイルです。</span><span class="sxs-lookup"><span data-stu-id="2e102-181">This file is the remote desktop configuration file for the MailSenderVM virtual machine.</span></span> <span data-ttu-id="2e102-182">ファイルを開き、接続プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="2e102-182">Open the file to start the connection process.</span></span>

1. <span data-ttu-id="2e102-183">**[リモート デスクトップ接続]** ダイアログで、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-183">In the **Remote Desktop Connection** dialog, click **Connect**.</span></span>

1. <span data-ttu-id="2e102-184">**[Windows セキュリティ]** ダイアログで、**[別のアカウントを使用]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-184">In the **Windows Security** dialog, click **Use another account**.</span></span>

1. <span data-ttu-id="2e102-185">**[ユーザー名]** ボックスに、「**mailmaster**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="2e102-185">In the **Username** textbox, type **mailmaster**.</span></span>

1. <span data-ttu-id="2e102-186">**[パスワード]** ボックスに、この演習でこのユーザー名に入力したパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="2e102-186">In the **Password** textbox, type the password you entered for this user name in this exercise.</span></span> 

1. <span data-ttu-id="2e102-187">**[リモート デスクトップ接続]** ダイアログで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-187">In the **Remote Desktop Connection** dialog, click **Yes**.</span></span>

<span data-ttu-id="2e102-188">仮想マシンへのリモート デスクトップ セッションが開始されます。</span><span class="sxs-lookup"><span data-stu-id="2e102-188">A remote desktop session to the virtual machine is now started.</span></span> <span data-ttu-id="2e102-189">初めてサインインするには数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2e102-189">It might take a few moments to sign in for the first time.</span></span> <span data-ttu-id="2e102-190">サインインが完了すると、**サーバー マネージャー** ツールが画面に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2e102-190">When sign-in is finished, the **Server Manager** tool will be displayed on the screen.</span></span>

### <a name="partition-and-format-our-data-disk-using-server-manager"></a><span data-ttu-id="2e102-191">サーバー マネージャーを使用してデータ ディスクをパーティション分割してフォーマットする</span><span class="sxs-lookup"><span data-stu-id="2e102-191">Partition and format our data disk using Server Manager</span></span>

1. <span data-ttu-id="2e102-192">**サーバー マネージャー**が表示されたら、左側のナビゲーションで **[ファイル サービスと記憶域サービス]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-192">When **Server Manager** is displayed, select **File and Storage Services** in the navigation on the left.</span></span>

1. <span data-ttu-id="2e102-193">**[ボリューム]** で **[ディスク]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-193">Under **Volumes**, select **Disks**.</span></span>

1. <span data-ttu-id="2e102-194">ディスクの一覧で、ディスク **0** はオペレーティング システムのディスク、ディスク **1** は一時ディスクです。</span><span class="sxs-lookup"><span data-stu-id="2e102-194">In the list of disks, disk **0** is the operating system disk and disk **1** is the temporary disk.</span></span> <span data-ttu-id="2e102-195">追加したばかりの新しい VHD であるディスク **2** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-195">Select disk **2**, which is the new VHD you just added.</span></span>

1. <span data-ttu-id="2e102-196">**[ボリューム]** ウィンドウの上部で、**[タスク]**、**[新しいボリューム]** の順に選択します。次のように、メニューは画面の右上に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2e102-196">At the top of the **VOLUMES** pane, select **TASKS** followed by **New Volume...**. The menu is in the top right of the screen as follows.</span></span>

   ![展開され、3 つのメニュー項目が表示された [タスク] メニューのスクリーンショット。](../media-draft/tasks-menu.png)


1. <span data-ttu-id="2e102-199">**[新しいボリューム]** ウィザードで **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-199">In the **New Volume** wizard, click **Next**.</span></span>

1. <span data-ttu-id="2e102-200">**[サーバーとディスクの選択]** ページで **[MailSenderVM]** と **[ディスク 2]** を選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-200">In the **Select server and disk** page, select **MailSenderVM** and **Disk 2**, and then click **Next**.</span></span>

1. <span data-ttu-id="2e102-201">**[オフラインまたは未初期化のディスク]** ダイアログで **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-201">In the **Offline or Uninitiated Disk** dialog, click **OK**.</span></span>

1. <span data-ttu-id="2e102-202">**[ボリュームのサイズの指定]** ページで、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-202">In the **Specify the size of the volume** page, click **Next**.</span></span>

1. <span data-ttu-id="2e102-203">**[ドライブ文字を割り当てる]** ページで **[F:]**、**[次へ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-203">In the **Assign a drive letter** page, select **F:** followed by **Next**.</span></span>

1. <span data-ttu-id="2e102-204">**[ファイル システム形式の選択]** ページで、**[ボリューム ラベル]** ボックスに「**Incoming**」と入力し、**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-204">In the **Select file system settings** page, in the **Volume label** textbox, type **Incoming** and then select **Next**.</span></span>

1. <span data-ttu-id="2e102-205">**[選択内容の確認]** ページで、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-205">In the **Confirm selections** page, select **Create**.</span></span> <span data-ttu-id="2e102-206">Windows でディスクを初期化し、新しいパーティションをフォーマットします。</span><span class="sxs-lookup"><span data-stu-id="2e102-206">Windows initiates the disk and formats the new partition.</span></span>

1. <span data-ttu-id="2e102-207">**[完了]** ページで **[閉じる]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-207">In the **Completion** page, select **Close**.</span></span>

<span data-ttu-id="2e102-208">エクスプローラーで新しいディスク ボリュームを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="2e102-208">Let's have a look at our new disk volume in File Explorer.</span></span> 
1. <span data-ttu-id="2e102-209">**エクスプローラー**を開きます。</span><span class="sxs-lookup"><span data-stu-id="2e102-209">Open **File Explorer**.</span></span>

1. <span data-ttu-id="2e102-210">左側のナビゲーションで **[PC]** をクリックし、**[Incoming (F:)]** をダブル クリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-210">In the navigation in the left, click **This PC** and then double-click **Incoming (F:)**.</span></span>

1. <span data-ttu-id="2e102-211">**[ホーム]** を選択し、**[新しいフォルダー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2e102-211">Select **Home**, and then **New Folder**.</span></span>

1. <span data-ttu-id="2e102-212">「**Drop**」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2e102-212">Type **Drop** and then press Enter.</span></span>

<span data-ttu-id="2e102-213">VHD 上に **Incoming** という新しいボリュームが作成され、そのボリュームに **Drop** という名前のフォルダーが作成されました。</span><span class="sxs-lookup"><span data-stu-id="2e102-213">We now have a new volume created on our VHD called **Incoming**, and we've created a folder called **Drop** on that volume.</span></span>  

1. <span data-ttu-id="2e102-214">Windows のエクスプローラーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="2e102-214">Close Windows Explorer.</span></span>

1. <span data-ttu-id="2e102-215">**タスク バー** の **[スタート]** ボタンをクリックし、**[電源]** ボタンをクリックして **[切断]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2e102-215">On the **Task Bar**, click the **Start** button, click the **Power** button, and then click **Disconnect**.</span></span>

<span data-ttu-id="2e102-216">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="2e102-216">Congratulations!</span></span> <span data-ttu-id="2e102-217">Windows VM を作成し、新しい VHD を接続し、その VHD にボリュームを作成し、そのボリュームにフォルダーを追加しました。</span><span class="sxs-lookup"><span data-stu-id="2e102-217">You've successfully created a Windows VM, attached a new VHD, created a volume on that VHD and added a folder to that volume.</span></span> <span data-ttu-id="2e102-218">前述のように、新しい VHD に使用したディスクの種類は **Standard HDD** でした。</span><span class="sxs-lookup"><span data-stu-id="2e102-218">If you recall, the disk type we used for the new VHD was a **Standard HDD**.</span></span> <span data-ttu-id="2e102-219">次のユニットでは、Standard Storage と Premium Storage の違いについて学習します。</span><span class="sxs-lookup"><span data-stu-id="2e102-219">In the next unit, we'll learn the differences between standard and premium storage.</span></span> 
