
<span data-ttu-id="a46c5-101">あなたは、写真共有サイトを運営しているとします。データは Azure Virtual Machines (VM) に格納され、SQL Server とカスタム アプリケーションを実行しています。</span><span class="sxs-lookup"><span data-stu-id="a46c5-101">Suppose you run a photo sharing site, with data stored on Azure virtual machines (VMs) running SQL Server and custom applications.</span></span> <span data-ttu-id="a46c5-102">あなたは次の調整を行いたいと思っています。</span><span class="sxs-lookup"><span data-stu-id="a46c5-102">You want to make the following adjustments:</span></span>

- <span data-ttu-id="a46c5-103">VM でディスク キャッシュ設定を変更する必要がある。</span><span class="sxs-lookup"><span data-stu-id="a46c5-103">You need to change the disk cache settings on a VM.</span></span>
- <span data-ttu-id="a46c5-104">キャッシュを有効にして新しいデータ ディスクを VM に追加したい。</span><span class="sxs-lookup"><span data-stu-id="a46c5-104">You want to add a new data disk to the VM with caching enabled.</span></span>

<span data-ttu-id="a46c5-105">あなたは、Azure portal でこれらの変更を行うことにしました。</span><span class="sxs-lookup"><span data-stu-id="a46c5-105">You've decided to make these changes through the Azure portal.</span></span>

<span data-ttu-id="a46c5-106">この演習では、先に説明した VM に実際に変更を加えます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-106">In this exercise, we'll walk through making the changes to a VM that we described above.</span></span> <span data-ttu-id="a46c5-107">まず、ポータルにサインインして、VM を作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="a46c5-107">First, let's sign in to the portal and create a VM.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-virtual-machine"></a><span data-ttu-id="a46c5-108">仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="a46c5-108">Create a virtual machine</span></span>

<span data-ttu-id="a46c5-109">この手順では、次のプロパティを持つ VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-109">In this step, we're going to create a VM with the following properties:</span></span>

| <span data-ttu-id="a46c5-110">プロパティ</span><span class="sxs-lookup"><span data-stu-id="a46c5-110">Property</span></span>        | <span data-ttu-id="a46c5-111">値</span><span class="sxs-lookup"><span data-stu-id="a46c5-111">Value</span></span>   |
|-----------------|---------|
| <span data-ttu-id="a46c5-112">イメージ</span><span class="sxs-lookup"><span data-stu-id="a46c5-112">Image</span></span>           | <span data-ttu-id="a46c5-113">**Windows Server 2016 Datacenter**</span><span class="sxs-lookup"><span data-stu-id="a46c5-113">**Windows Server 2016 Datacenter**</span></span> |
| <span data-ttu-id="a46c5-114">名前</span><span class="sxs-lookup"><span data-stu-id="a46c5-114">Name</span></span>            | <span data-ttu-id="a46c5-115">**fotoshareVM**</span><span class="sxs-lookup"><span data-stu-id="a46c5-115">**fotoshareVM**</span></span> |
| <span data-ttu-id="a46c5-116">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="a46c5-116">Resource group</span></span>  |   <span data-ttu-id="a46c5-117">**<rgn>[サンドボックス リソース グループ名]</rgn>**</span><span class="sxs-lookup"><span data-stu-id="a46c5-117">**<rgn>[sandbox resource group name]</rgn>**</span></span> |
| <span data-ttu-id="a46c5-118">場所</span><span class="sxs-lookup"><span data-stu-id="a46c5-118">Location</span></span>        | <span data-ttu-id="a46c5-119">次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="a46c5-119">See below.</span></span> |

1. <span data-ttu-id="a46c5-120">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="a46c5-120">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="a46c5-121">左のサイドバー メニューで、**[リソースの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-121">Select **Create a resource** in the sidebar menu on the left.</span></span>

1. <span data-ttu-id="a46c5-122">**[人気順]** Marketplace 要素の一覧に _Windows Server 2016 VM_ があるはずです。</span><span class="sxs-lookup"><span data-stu-id="a46c5-122">_Windows Server 2016 VM_ should be in the list of **Popular** Marketplace elements.</span></span> <span data-ttu-id="a46c5-123">ない場合は、上にある検索ボックスを使用して "Windows Server 2016 DataCenter" を検索してみてください。</span><span class="sxs-lookup"><span data-stu-id="a46c5-123">If not, try searching for "Windows Server 2016 DataCenter" using the search box on the top.</span></span>

1. <span data-ttu-id="a46c5-124">Windows VM を選択し、**[作成]** をクリックして VM の作成プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-124">Select the Windows VM and click **Create** to start the VM creation process.</span></span>

1. <span data-ttu-id="a46c5-125">**[基本]** パネルで、選択されている **[サブスクリプション]** が_コンシェルジェ サブスクリプション_であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-125">In the **Basics** panel, verify the selected **Subscription** is _Concierge Subscription_.</span></span>

1. <span data-ttu-id="a46c5-126">**[リソース グループ]** で **[既存のものを使用]** を選択し、_<rgn>[サンドボックス リソース グループ名]</rgn>_ を選びます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-126">Under **Resource Group**, select **Use Existing** and choose _<rgn>[sandbox resource group name]</rgn>_.</span></span>

1. <span data-ttu-id="a46c5-127">**[仮想マシン名]** ボックスに、「_fotoshareVM_」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-127">In the **Virtual machine name** box, enter _fotoshareVM_.</span></span>

1. <span data-ttu-id="a46c5-128">**[場所]** ドロップダウン リストで、次の一覧から最も近いリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-128">In the **Location** drop-down list, select the closest region to you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="a46c5-129">VM の **[サイズ]** の既定値 **[DS1 v2]** では、1 つの CPU と 3.5 GB のメモリが提供されます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-129">For the VM **Size**, the default is **DS1 v2** which gives you a single CPU and 3.5 GB of memory.</span></span> <span data-ttu-id="a46c5-130">この例ではそれで問題ありません。</span><span class="sxs-lookup"><span data-stu-id="a46c5-130">That's fine for this example.</span></span>

1. <span data-ttu-id="a46c5-131">**[管理者アカウント]** セクションで、**[ユーザー名]** と **[パスワード]**/**[パスワードの確認]** に新しい VM での管理者アカウントの値を入力します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-131">In **ADMINISTRATOR ACCOUNT** section, enter a **Username** and **Password**/**Confirm password** for an administrator account on the new VM.</span></span>

1. <span data-ttu-id="a46c5-132">次の図は、入力後の **[基本]** 構成の例です。残りのタブとフィールドは既定値のままにして、**[確認および作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a46c5-132">The following image is an example of what the **Basics** configuration looks like when filled out. Leave the defaults for the remaining tabs and fields and click **Review + create**.</span></span>

    ![説明した構成例が [基本] に入力された [仮想マシンの作成] ブレードを示す Azure portal のスクリーンショット。](../media/4-basics-vm.png)

1. <span data-ttu-id="a46c5-134">新しい VM の設定を確認した後、**[作成]** をクリックして新しい VM の展開を始めます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-134">After reviewing your new VM settings, click **Create** to start the deploying your new VM.</span></span>

<span data-ttu-id="a46c5-135">仮想マシンをサポートするためのさまざまなリソース (ストレージ、ネットワーク インターフェイスなど) がすべて作成されるので、VM の作成には数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="a46c5-135">VM creation can take a few minutes as it creates all the various resources (storage, network interface, etc.) to support the virtual machine.</span></span> <span data-ttu-id="a46c5-136">VM が展開されるまで待ってから、演習を続けます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-136">Wait until the VM has deployed before continuing with the exercise.</span></span>

## <a name="view-os-disk-cache-status-in-the-portal"></a><span data-ttu-id="a46c5-137">ポータルで OS ディスク キャッシュの状態を表示する</span><span class="sxs-lookup"><span data-stu-id="a46c5-137">View OS disk cache status in the portal</span></span>

<span data-ttu-id="a46c5-138">VM をデプロイすると、次の手順を使用して、OS ディスクのキャッシュ状態を確認できます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-138">Once our VM is deployed, we can confirm the caching status of the OS disk using the following steps:</span></span>

1. <span data-ttu-id="a46c5-139">ポータルで **fotoshareVM** リソースを選択して、VM の詳細を開きます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-139">Select the **fotoshareVM** resource to open the VM details in the portal.</span></span> <span data-ttu-id="a46c5-140">または、左のサイドバーで **[すべてのリソース]** をクリックして、**fotoshareVM** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-140">Alternatively, you can click **All resources** in the left sidebar, and then select your VM, **fotoshareVM**.</span></span>

1. <span data-ttu-id="a46c5-141">**[設定]** で **[ディスク]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-141">Under **Settings**, select **Disks**.</span></span>

1. <span data-ttu-id="a46c5-142">**[ディスク]** ウィンドウでは、VM に 1 つのディスク (OS ディスク) があります。</span><span class="sxs-lookup"><span data-stu-id="a46c5-142">On the **Disks** pane, the VM has one disk, the OS disk.</span></span> <span data-ttu-id="a46c5-143">そのキャッシュの種類は現在、既定値の **[読み取り/書き込み]** に設定されています。</span><span class="sxs-lookup"><span data-stu-id="a46c5-143">Its cache type is currently set to the default value of **Read/write**.</span></span>

![[VM] ブレードの [ディスク] セクションで OS ディスクが読み取り専用キャッシュに設定されている Azure portal のスクリーンショット。](../media/4-os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a><span data-ttu-id="a46c5-145">ポータルで OS ディスクのキャッシュ設定を変更する</span><span class="sxs-lookup"><span data-stu-id="a46c5-145">Change the cache settings of the OS disk in the portal</span></span>

1. <span data-ttu-id="a46c5-146">**[ディスク]** ウィンドウで、画面の左上の **[編集]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-146">On the **Disks** pane, select **Edit** in the upper left of the screen.</span></span>

1. <span data-ttu-id="a46c5-147">OS ディスクの **[ホスト キャッシュ]** の値をドロップダウン リストを使用して **[読み取り専用]** に変更してから、画面の左上で **[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-147">Change the **HOST CACHING** value for the OS disk to **Read-only** using the drop-down list, and then select **Save** in the upper left of the screen.</span></span>

1. <span data-ttu-id="a46c5-148">この更新には時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="a46c5-148">This update can take some time.</span></span> <span data-ttu-id="a46c5-149">この理由は、Azure ディスクのキャッシュ設定を変更すると、対象となるディスクがデタッチされ再アタッチされるからです。</span><span class="sxs-lookup"><span data-stu-id="a46c5-149">The reason is that changing the cache setting of an Azure disk detaches and reattaches the target disk.</span></span> <span data-ttu-id="a46c5-150">オペレーティング システム ディスクの場合は、VM も再起動されます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-150">If it's the operating system disk, the VM is also restarted.</span></span> <span data-ttu-id="a46c5-151">操作が完了すると、VM ディスクが更新されたという通知が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-151">When the operation completes, you'll get a notification saying the VM disks have been updated.</span></span>

1. <span data-ttu-id="a46c5-152">完了すると、OS ディスクのキャッシュの種類が**読み取り専用**に設定されます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-152">Once complete, the OS disk cache type is set to **Read-only**.</span></span>

<span data-ttu-id="a46c5-153">データ ディスクのキャッシュの構成に進みましょう。</span><span class="sxs-lookup"><span data-stu-id="a46c5-153">Let's move on to data disk cache configuration.</span></span> <span data-ttu-id="a46c5-154">ディスクを構成するには、まずディスクを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a46c5-154">To configure a disk, we'll need first to create one.</span></span>

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a><span data-ttu-id="a46c5-155">データ ディスクを VM に追加してキャッシュの種類を設定する</span><span class="sxs-lookup"><span data-stu-id="a46c5-155">Add a data disk to the VM and set caching type</span></span>

1. <span data-ttu-id="a46c5-156">ポータルで VM の **[ディスク]** ビューに戻り、**[データ ディスクの追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a46c5-156">Back on the **Disks** view of our VM in the portal, go ahead and click **Add data disk**.</span></span> <span data-ttu-id="a46c5-157">**[名前]** フィールドには、このフィールドを空にできないことを示すエラーがすぐに表示されます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-157">An error immediately appears in the **Name** field, telling us that the field can't be empty.</span></span> <span data-ttu-id="a46c5-158">データ ディスクがまだないので、1 つ作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="a46c5-158">We don't have a data disk yet, so let's create one.</span></span>

1. <span data-ttu-id="a46c5-159">**[名前]** ボックスの一覧をクリックして、**[ディスクの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a46c5-159">Click in the **Name** list, and then click **Create disk**.</span></span>

1. <span data-ttu-id="a46c5-160">**[マネージド ディスクの作成]** ウィンドウで、**[名前]** ボックスに「**fotosharesVM-data**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="a46c5-160">In the **Create managed disk** pane, in the **Name** box, type **fotosharesVM-data**.</span></span>

1. <span data-ttu-id="a46c5-161">**[リソース グループ]** で **[既存のものを使用]** を選択し、_<rgn>[サンドボックス リソース グループ名]</rgn>_ を選びます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-161">Under **Resource Group**, select **Use existing**, and select _<rgn>[sandbox resource group name]</rgn>_.</span></span>

1. <span data-ttu-id="a46c5-162">残りのフィールドの既定値に注意してください。</span><span class="sxs-lookup"><span data-stu-id="a46c5-162">Note the defaults for the remaining fields:</span></span>
    - <span data-ttu-id="a46c5-163">Premium SSD</span><span class="sxs-lookup"><span data-stu-id="a46c5-163">Premium SSD</span></span>
    - <span data-ttu-id="a46c5-164">サイズ 1023 GB</span><span class="sxs-lookup"><span data-stu-id="a46c5-164">1023 GB in size</span></span>
    - <span data-ttu-id="a46c5-165">VM と同じ場所 (変更不可)。</span><span class="sxs-lookup"><span data-stu-id="a46c5-165">In the same location as the VM (not changeable).</span></span>
    - <span data-ttu-id="a46c5-166">IOPS 制限 - 5000</span><span class="sxs-lookup"><span data-stu-id="a46c5-166">IOPS limit - 5000</span></span>
    - <span data-ttu-id="a46c5-167">スループット制限 (MB/秒) - 200</span><span class="sxs-lookup"><span data-stu-id="a46c5-167">Throughput limit (MB/s) - 200</span></span>

1. <span data-ttu-id="a46c5-168">画面の下部にある **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a46c5-168">Click **Create** at the bottom of the screen.</span></span>

    <span data-ttu-id="a46c5-169">ディスクが作成されるまで待ってから、次に進みます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-169">Wait until the disk has been created before continuing.</span></span>

1. <span data-ttu-id="a46c5-170">新しいデータ ディスクの **[ホスト キャッシュ]** の値をドロップダウン リストを使用して **[読み取り専用]** に変更してから (既に設定されているかもしれません)、画面の左上で **[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a46c5-170">Change the **HOST CACHING** value for our new data disk to **Read-only** using the drop-down list (it might be set already), and then click **Save** in the upper left of the screen.</span></span>

    <span data-ttu-id="a46c5-171">VM で新しいデータ ディスクの更新が完了するまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="a46c5-171">Wait for the VM to finish updating the new data disk.</span></span> <span data-ttu-id="a46c5-172">完了すると、仮想マシン上に新しいデータ ディスクが存在するようになります。</span><span class="sxs-lookup"><span data-stu-id="a46c5-172">Once complete, you will have a new data disk on your virtual machine.</span></span>

<span data-ttu-id="a46c5-173">この演習では、Azure portal を使用して、新しい VM でキャッシュを構成し、既存ディスクのキャッシュ設定を変更し、新しいデータ ディスクでキャッシュを構成しました。</span><span class="sxs-lookup"><span data-stu-id="a46c5-173">In this exercise, we used the Azure portal to configure caching on a new VM, change cache settings on an existing disk, and configure caching on a new data disk.</span></span> <span data-ttu-id="a46c5-174">次のスクリーンショットは最終的な構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="a46c5-174">The following screenshot shows the final configuration:</span></span>

![[VM] ブレードの [ディスク] セクションで両方とも読み取り専用キャッシュに設定された OS ディスクと新しいデータ ディスクが示されている Azure portal のスクリーンショット。](../media/disks-final-config-portal.PNG)
