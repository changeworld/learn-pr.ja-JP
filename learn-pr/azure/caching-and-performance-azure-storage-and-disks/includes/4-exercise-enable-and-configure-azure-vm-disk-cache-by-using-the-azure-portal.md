<span data-ttu-id="d2ed1-101">あなたは、写真共有サイトを運営しているとします。データは Azure Virtual Machines (VM) に格納され、SQL Server とカスタム アプリケーションを実行しています。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-101">Suppose you run a photo sharing site, with data stored on Azure virtual machines (VMs) running SQL Server and custom applications.</span></span> <span data-ttu-id="d2ed1-102">あなたは次の調整を行いたいと思っています。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-102">You want to make the following adjustments.</span></span>

- <span data-ttu-id="d2ed1-103">VM でディスク キャッシュ設定を変更する必要がある</span><span class="sxs-lookup"><span data-stu-id="d2ed1-103">You need change the disk cache settings on a VM</span></span>
- <span data-ttu-id="d2ed1-104">キャッシュを有効にして新しいデータ ディスクを VM に追加したい</span><span class="sxs-lookup"><span data-stu-id="d2ed1-104">You want dd a new data disk to the VM with caching enabled</span></span>

<span data-ttu-id="d2ed1-105">あなたは、Azure portal でこれらの変更を行うことにしました。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-105">You've decided to make these changes through the Azure portal.</span></span>

<span data-ttu-id="d2ed1-106">この演習では、先に説明した VM に実際に変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-106">In this exercise, we'll walk through making the changes to a VM that we described above.</span></span> <span data-ttu-id="d2ed1-107">まず、ポータルにサインインして、VM を作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-107">First, let's sign in to the portal and create a VM.</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="d2ed1-108">Azure portal にサインインする</span><span class="sxs-lookup"><span data-stu-id="d2ed1-108">Sign in to the Azure portal</span></span>
<!---TODO: Update for sandbox?--->

1. <span data-ttu-id="d2ed1-109">[Azure portal](https://portal.azure.com/?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-109">Sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

## <a name="create-a-virtual-machine"></a><span data-ttu-id="d2ed1-110">仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="d2ed1-110">Create a virtual machine</span></span>

<span data-ttu-id="d2ed1-111">この手順では、次のプロパティを持つ VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-111">In this step, we're going to create a VM with the following properties.</span></span>

|<span data-ttu-id="d2ed1-112">プロパティ</span><span class="sxs-lookup"><span data-stu-id="d2ed1-112">Property</span></span>  |<span data-ttu-id="d2ed1-113">値</span><span class="sxs-lookup"><span data-stu-id="d2ed1-113">Value</span></span>  |
|---------|---------|
|<span data-ttu-id="d2ed1-114">イメージ</span><span class="sxs-lookup"><span data-stu-id="d2ed1-114">Image</span></span>     |   <span data-ttu-id="d2ed1-115">**Windows Server 2016 Datacenter**</span><span class="sxs-lookup"><span data-stu-id="d2ed1-115">**Windows Server 2016 Datacenter**</span></span>      |
|<span data-ttu-id="d2ed1-116">名前</span><span class="sxs-lookup"><span data-stu-id="d2ed1-116">Name</span></span>     |   <span data-ttu-id="d2ed1-117">**fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="d2ed1-117">**fotoshareVM**</span></span>     |
|<span data-ttu-id="d2ed1-118">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="d2ed1-118">Resource group</span></span>     |   <span data-ttu-id="d2ed1-119">**fotoshare-rg**</span><span class="sxs-lookup"><span data-stu-id="d2ed1-119">**fotoshare-rg**</span></span>      |


1. <span data-ttu-id="d2ed1-120">ポータルの左側のメニューで **[Virtual Machines]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-120">In the left menu of the portal, select **Virtual machines**</span></span>

1. <span data-ttu-id="d2ed1-121">次に、**[Virtual Machines]** 画面の左上の **[+ 追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-121">Now select **+ Add** in the top left of the **Virtual machines** screen.</span></span> <span data-ttu-id="d2ed1-122">これにより作成プロセスが開始します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-122">This action starts the creation process.</span></span>

1. <span data-ttu-id="d2ed1-123">使用可能な VM イメージを一覧表示する [コンピューティング] パネルで、検索ボックスに「*Windows Server 2016 Datacenter*」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-123">On the Compute panel that lists available VM images, type *Windows Server 2016 Datacenter* into the search box.</span></span>

1. <span data-ttu-id="d2ed1-124">検索結果から **[Windows Server 2016 Datacenter]** を選択し、**[作成]** を選択して VM の作成プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-124">Select **Windows Server 2016 Datacenter** from the search results and then select **Create** to start the VM creation process.</span></span>

1. <span data-ttu-id="d2ed1-125">**[基本]** パネルの **[名前]** ボックスに、「**fotoshareVM**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-125">In the **Basics** panel, in the **Name** box, enter **fotoshareVM**</span></span>

1. <span data-ttu-id="d2ed1-126">**[ユーザー名]** ボックスと **[パスワード]** ボックスに、このサーバーの管理者アカウントの名前とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-126">In the **Username** and **Password boxes**, enter a name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="d2ed1-127">**[サブスクリプション]** ドロップダウンでご自分の Azure サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-127">In the **Subscription** dropdown, select your Azure subscription.</span></span>

1. <span data-ttu-id="d2ed1-128">**[リソース グループ]** で **[新規作成]** を選択し、ボックスに「**fotoshare-rg**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-128">Under **Resource Group**, select **Create new**, and in the box, type **fotoshare-rg**.</span></span>

1. <span data-ttu-id="d2ed1-129">**[場所]** ドロップダウン リストで、近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-129">In the **Location** drop-down list, select a region near you.</span></span>

<span data-ttu-id="d2ed1-130">次に、入力後の **[基本]** 構成の例を示します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-130">The following is an example of what the **Basics** configuration looks like when filled out.</span></span>

![VM の入力後の基本構成のスクリーンショット。](../media-draft/vm-basics-settings.PNG)

1. <span data-ttu-id="d2ed1-132">**[OK]** を選択して、次のステップに進みます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-132">Select **OK** to move onto the next step.</span></span>

<span data-ttu-id="d2ed1-133">次は、VM のサイズを選択して、デプロイを開始する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-133">You now need to choose a size for the VM, and then start the deployment:</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2ed1-134">L シリーズと B シリーズの仮想マシンのディスク キャッシュは変更できないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-134">Remember that disk caching can't be changed for L-Series and B-series virtual machines.</span></span> <span data-ttu-id="d2ed1-135">別のサイズを選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-135">We'll select a different size.</span></span>

1. <span data-ttu-id="d2ed1-136">**[サイズの選択]** セクションで、**[F1s]** などの **[Standard]** SKU を選択してから、**[選択]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-136">In the **Choose a size** section, select a **Standard** SKU, such as **F1s**, and then choose **Select**.</span></span>

1. <span data-ttu-id="d2ed1-137">**[設定]** ウィンドウを下にスクロールして、ディスク キャッシュを構成するオプションがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-137">Scroll down the **Settings** pane and observe that there is no option to configure disk caching.</span></span> <span data-ttu-id="d2ed1-138">この手順では既定値をそのまま使用して、ウィンドウの下部で **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-138">Let's accept the defaults on this step and choose **OK** at the bottom of the pane.</span></span>

1. <span data-ttu-id="d2ed1-139">**[作成]** パネルで、概要を確認してから、**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-139">On the **Create** panel, review the summary, and then choose **Create**.</span></span>

1. <span data-ttu-id="d2ed1-140">VM の作成にはしばらく時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-140">VM creation can take a while.</span></span> <span data-ttu-id="d2ed1-141">VM がデプロイされるまで待ってから、演習を続けます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-141">Wait until the VM has deployed before continuing with the exercise.</span></span> <span data-ttu-id="d2ed1-142">プロセスが完了すると、Notification Hub にメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-142">You'll get a message in the Notification hub when the process is complete.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2ed1-143">この VM は次のレッスンで使用するため、しばらくの間このままにしておきます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-143">We'll use this VM in the next lesson, so keep it around for a while!</span></span>

## <a name="view-os-disk-cache-status-in-the-portal"></a><span data-ttu-id="d2ed1-144">ポータルで OS ディスク キャッシュの状態を表示する</span><span class="sxs-lookup"><span data-stu-id="d2ed1-144">View OS disk cache status in the portal</span></span>

<span data-ttu-id="d2ed1-145">VM をデプロイすると、次の手順を使用して、OS ディスクのキャッシュ状態を確認できます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-145">Once our VM is deployed, we can confirm the caching status of the OS disk using the following steps.</span></span>

1. <span data-ttu-id="d2ed1-146">左側のメニューで、**[すべてのリソース]** をクリックし、ご自分の VM **[fotoshareVM]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-146">In the left menu, click **All resources**, and then select your VM,  **fotoshareVM**.</span></span>

1. <span data-ttu-id="d2ed1-147">**[仮想マシン]** 画面の **[設定]** で、**[ディスク]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-147">On the **Virtual machine** screen, under **SETTINGS**, select **Disks**.</span></span>

1. <span data-ttu-id="d2ed1-148">**[ディスク]** ウィンドウでは、VM に 1 つのディスク (OS ディスク) があります。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-148">On the **Disks** pane, the VM has one disk, the OS disk.</span></span> <span data-ttu-id="d2ed1-149">そのキャッシュの種類は現在、既定値の **[読み取り/書き込み]** に設定されています。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-149">Its cache type is currently set to the default value of **Read/write**.</span></span>

![OS ディスクとデータ ディスクのスクリーンショットでは、どちらも読み取り専用キャッシュに設定されています。](../media-draft/os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a><span data-ttu-id="d2ed1-151">ポータルで OS ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="d2ed1-151">Change the cache settings of the OS disk in the portal</span></span>

1. <span data-ttu-id="d2ed1-152">**[ディスク]** ウィンドウで、画面の左上の **[編集]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-152">On the **Disks** pane, select **Edit** in the upper left of the screen.</span></span>

1. <span data-ttu-id="d2ed1-153">OS ディスクの **[ホスト キャッシュ]** 値をドロップダウンを使用して **[読み取り専用]** に変更してから、画面の左上で **[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-153">Change the **HOST CACHING** value for the OS disk to **Read-only** using the dropdown and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="d2ed1-154">この更新にはしばらく時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-154">This update takes a while.</span></span> <span data-ttu-id="d2ed1-155">この理由は、Azure ディスクのキャッシュ設定を変更すると、対象となるディスクがデタッチされ再アタッチされるからです。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-155">The reason is that changing the cache setting of an Azure disk detaches and reattaches the target disk.</span></span> <span data-ttu-id="d2ed1-156">オペレーティング システム ディスクの場合は、VM も再起動されます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-156">If it's the operating system disk, the VM is also restarted.</span></span> <span data-ttu-id="d2ed1-157">更新が完了すると、次の通知のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-157">You'll get a message similar to the following notification when the update has finished.</span></span>

![キャッシュ設定の更新が完了したときに表示される通知の例。](../media-draft/vm-disk-update-complete.PNG)

4. <span data-ttu-id="d2ed1-159">完了すると、OS ディスクのキャッシュの種類が**読み取り専用**に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-159">Once complete, the OS disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="d2ed1-160">データ ディスクのキャッシュの構成に進みましょう。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-160">Let's move on to data disk cache configuration.</span></span> <span data-ttu-id="d2ed1-161">ディスクを構成するには、まずディスクを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-161">To configure a disk, we'll need to first create one.</span></span>

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a><span data-ttu-id="d2ed1-162">データ ディスクを VM に追加してキャッシュの種類を設定する</span><span class="sxs-lookup"><span data-stu-id="d2ed1-162">Add a data disk to the VM and set caching type</span></span>

1. <span data-ttu-id="d2ed1-163">ポータルでご自分の VM の **[ディスク]** ビューに戻り、**[データ ディスクの追加]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-163">Back on the **Disks** view of our VM in the portal, go ahead and select **Add data disk**.</span></span> <span data-ttu-id="d2ed1-164">**[名前]** フィールドには、このフィールドを空にできないことを示すエラーがすぐに表示されます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-164">An error immediately appears in the **Name** field telling us that the field can't be empty.</span></span> <span data-ttu-id="d2ed1-165">データ ディスクがまだないので、1 つ作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-165">We don't have a data disk yet, so let's create one.</span></span>

1. <span data-ttu-id="d2ed1-166">**[名前]** ボックスの一覧をクリックして、**[ディスクの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-166">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="d2ed1-167">**[マネージド ディスクの作成]** ウィンドウで、**[名前]** ボックスに「**fotosharesVM-data**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-167">In the **Create managed disk** pane, in the **Name** box, type **fotosharesVM-data**.</span></span>

1. <span data-ttu-id="d2ed1-168">**[リソース グループ]** で **[既存のものを使用]** を選択し、ドロップダウン メニューから **[fotoshare rg]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-168">Under **Resource Group**, select **Use existing**, and select **fotoshare-rg** from the dropdown menu.</span></span>

1. <span data-ttu-id="d2ed1-169">画面の下部にある **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-169">Select **Create** at the bottom of the screen.</span></span>

1. <span data-ttu-id="d2ed1-170">ディスクが作成されるまで待ってから、次に進みます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-170">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="d2ed1-171">新しいデータ ディスクの **[ホスト キャッシュ]** 値をドロップダウンを使用して **[読み取り専用]** に変更してから、画面の左上で **[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-171">Change the **HOST CACHING** value for our new data disk to **Read-only** using the dropdown and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="d2ed1-172">VM が更新されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-172">Wait for the VM to update.</span></span> <span data-ttu-id="d2ed1-173">この設定を変更するため、Azure でデータ ディスクのデタッチと再アタッチが行われるため、更新にはしばらく時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-173">Updating takes a while because Azure detaches and reattaches the data disk to change this setting.</span></span>

1. <span data-ttu-id="d2ed1-174">完了すると、データ ディスクのキャッシュの種類が**読み取り専用**に設定されます。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-174">Once complete, the data disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="d2ed1-175">この演習では、Azure portal を使用して、新しい VM でキャッシュを構成し、既存ディスクのキャッシュ設定を変更し、新しいデータ ディスクでキャッシュを構成しました。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-175">In this exercise, we used the Azure portal to configure caching on a new VM, change cache settings on an existing disk, and to configure caching on a new data disk.</span></span> <span data-ttu-id="d2ed1-176">次のスクリーンショットは最終的な構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="d2ed1-176">The following screenshot shows the final configuration.</span></span> 

![OS ディスクとデータ ディスクのスクリーンショットでは、どちらも読み取り専用キャッシュに設定されています。](../media-draft/disks-final-config-portal.PNG)