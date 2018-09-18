<span data-ttu-id="90708-101">この演習では、Azure portal を使用してロード バランサー、仮想ネットワーク、および複数の仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="90708-101">In this exercise, you will create a load balancer, a virtual network, and multiple virtual machines using the Azure portal.</span></span>

<span data-ttu-id="90708-102">あなたは、オンライン銀行サービスを立ち上げようとしているスタートアップ企業である Woodgrove Bank で働いているものとします。</span><span class="sxs-lookup"><span data-stu-id="90708-102">Suppose you work for Woodgrove Bank, a startup that is about to launch online banking services.</span></span> <span data-ttu-id="90708-103">このセクターは競争が激しいので、99.99% 以上のサービス可用性を保証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="90708-103">This sector is highly competitive, so you need to guarantee of a minimum of 99.99% service availability.</span></span> <span data-ttu-id="90708-104">あなたは、3 つの仮想マシンを含むプールを使用する Azure Load Balancer でこの目標が達成されると判断しました。</span><span class="sxs-lookup"><span data-stu-id="90708-104">You have determined that an Azure Load Balancer with a pool of three virtual machines will meet this goal.</span></span>

## <a name="create-a-public-load-balancer"></a><span data-ttu-id="90708-105">パブリック ロード バランサーを作成する</span><span class="sxs-lookup"><span data-stu-id="90708-105">Create a public load balancer</span></span>

1. <span data-ttu-id="90708-106">ブラウザーで [Azure portal](https://portal.azure.com/?azure-portal=true) に移動し、自分のアカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="90708-106">In a browser, navigate to the [Azure portal](https://portal.azure.com/?azure-portal=true) and sign in to your account.</span></span>

1. <span data-ttu-id="90708-107">サイドバーで **[リソースの作成]** をクリックし、**[新規]** ブレードで **[ネットワーキング]** をクリックして、**[ロード バランサー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-107">In the sidebar, click **Create a resource**, then in the **New** blade, click **Networking**, and then click **Load Balancer**.</span></span>

1. <span data-ttu-id="90708-108">[ロード バランサーの作成] ブレードで次の情報を入力または選択します。</span><span class="sxs-lookup"><span data-stu-id="90708-108">In the Create load balancer blade, enter or select the following information:</span></span>
    - <span data-ttu-id="90708-109">名前: **woodgrove-LB**</span><span class="sxs-lookup"><span data-stu-id="90708-109">Name: **woodgrove-LB**</span></span>
    - <span data-ttu-id="90708-110">種類: **パブリック**</span><span class="sxs-lookup"><span data-stu-id="90708-110">Type: **Public**</span></span>
    - <span data-ttu-id="90708-111">SKU: **Basic**</span><span class="sxs-lookup"><span data-stu-id="90708-111">SKU: **Basic**</span></span>
    - <span data-ttu-id="90708-112">パブリック IP アドレス: **[新規作成]** を選択し、とテキスト ボックスに「**woodgrove-LB-ip**」と入力して、[割り当て] は **[動的]** のままにします</span><span class="sxs-lookup"><span data-stu-id="90708-112">Public IP address: Select **Create new** and in the text box, type **woodgrove-LB-ip** in the text box, and leave the Assignment as **Dynamic**</span></span>
    - <span data-ttu-id="90708-113">リソース グループ: **[新規作成]** を選択し、ボックスに「**woodgrove-RG**」と入力します</span><span class="sxs-lookup"><span data-stu-id="90708-113">Resource group: Select **Create new**, and in the box, type **woodgrove-RG**</span></span>
    - <span data-ttu-id="90708-114">場所: 近くのリージョンを選択します</span><span class="sxs-lookup"><span data-stu-id="90708-114">Location: Select a region near you</span></span>

1. <span data-ttu-id="90708-115">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-115">Click **Create**.</span></span>

1. <span data-ttu-id="90708-116">ロード バランサーが展開されるまで待ってから、演習を続けます。</span><span class="sxs-lookup"><span data-stu-id="90708-116">Wait until the load balancer has deployed before continuing with the exercise.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="90708-117">仮想ネットワークを作成する</span><span class="sxs-lookup"><span data-stu-id="90708-117">Create a Virtual Network</span></span>

1. <span data-ttu-id="90708-118">左側のメニューで **[リソースの作成]** をクリックし、**[新規]** ブレードで **[ネットワーキング]** をクリックして、**[仮想ネットワーク]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-118">In the left menu, click **Create a resource**, then in the **New** blade, click **Networking**, and then click **Virtual network**.</span></span>

1. <span data-ttu-id="90708-119">**[仮想ネットワークの作成]** ブレードで次の情報を入力または選択します。</span><span class="sxs-lookup"><span data-stu-id="90708-119">In the **Create virtual network** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="90708-120">名前: **woodgrove-VNET**</span><span class="sxs-lookup"><span data-stu-id="90708-120">Name: **woodgrove-VNET**</span></span>
    - <span data-ttu-id="90708-121">アドレス空間: **172.20.0.0/16**</span><span class="sxs-lookup"><span data-stu-id="90708-121">Address space: **172.20.0.0/16**</span></span>
    - <span data-ttu-id="90708-122">リソース グループ: **[既存のものを使用]** を選択し、**woodgrove-RG** を選択します。</span><span class="sxs-lookup"><span data-stu-id="90708-122">Resource group: Select **Use existing**, and select **woodgrove-RG**.</span></span>
    - <span data-ttu-id="90708-123">サブネット: **backendSubnet**</span><span class="sxs-lookup"><span data-stu-id="90708-123">Subnet: **backendSubnet**</span></span>
    - <span data-ttu-id="90708-124">アドレス空間: **172.20.0.0/24**</span><span class="sxs-lookup"><span data-stu-id="90708-124">Address space: **172.20.0.0/24**</span></span>
    - <span data-ttu-id="90708-125">DDoS protection: **Basic (基本)**</span><span class="sxs-lookup"><span data-stu-id="90708-125">DDoS protection: **Basic**</span></span>
    - <span data-ttu-id="90708-126">サービス エンドポイント: **無効**</span><span class="sxs-lookup"><span data-stu-id="90708-126">Service endpoints: **Disabled**</span></span>

1. <span data-ttu-id="90708-127">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-127">Click **Create**.</span></span>

1. <span data-ttu-id="90708-128">仮想ネットワークが展開されるまで待ってから、演習を続けます。</span><span class="sxs-lookup"><span data-stu-id="90708-128">Wait until the virtual network has deployed before continuing with the exercise.</span></span>

## <a name="create-a-vm-template"></a><span data-ttu-id="90708-129">VM テンプレートを作成する</span><span class="sxs-lookup"><span data-stu-id="90708-129">Create a VM template</span></span>

<span data-ttu-id="90708-130">最初に、VM の基本的な情報を定義します。</span><span class="sxs-lookup"><span data-stu-id="90708-130">Start by defining the basic VM information:</span></span>

1. <span data-ttu-id="90708-131">Azure portal の左側のメニューで **[Virtual Machines]** をクリックし、**[仮想マシンの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-131">In the Azure portal, in the left menu, click **Virtual machines**, and then click **Create virtual machine**.</span></span>

1. <span data-ttu-id="90708-132">[Compute] ブレードの **[お勧め]** セクションで、**[Windows Server]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-132">On the Compute blade, in the **Recommended** section, click **Windows Server**.</span></span>

1. <span data-ttu-id="90708-133">**[Windows Server]** ブレードで、**[Windows Server 2016 Datacenter]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-133">In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.</span></span>

1. <span data-ttu-id="90708-134">**[Windows Server 2016 Datacenter]** ブレードで、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-134">In the **Windows Server 2016 Datacenter** blade, click **Create**.</span></span>

1. <span data-ttu-id="90708-135">**[基本]** ブレードで、**[名前]** ボックスに「**woodgrove-SVR01**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="90708-135">In the **Basics** blade, in the **Name** box, type **woodgrove-SVR01**.</span></span>

1. <span data-ttu-id="90708-136">**[ユーザー名]** ボックスと **[パスワード]** ボックスに、このサーバーの管理者アカウントの安全な名前とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="90708-136">In the **Username** and **Password boxes**, type a secure name and password for an administrator account on this server.</span></span>

1. <span data-ttu-id="90708-137">**[サブスクリプション]** ボックスで Azure サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="90708-137">In the **Subscription** box, select your Azure subscription.</span></span>

1. <span data-ttu-id="90708-138">**[リソース グループ]** で **[既存のものを使用]** を選択し、一覧で **woodgrove-RG** を選択します。</span><span class="sxs-lookup"><span data-stu-id="90708-138">Under **Resource group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="90708-139">**[場所]** ドロップダウン リストで、近くのリージョンを選択します</span><span class="sxs-lookup"><span data-stu-id="90708-139">In the **Location** drop-down list, select a region near you.SAME</span></span>

1. <span data-ttu-id="90708-140">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-140">Click **OK**.</span></span>

<span data-ttu-id="90708-141">VM のサイズを選択し、設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="90708-141">Choose a size for the VM and configure settings:</span></span>

1. <span data-ttu-id="90708-142">**[サイズの選択]** ブレードで、**[D2s_v3]** などの **[Standard]** を選択してから、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-142">On the **Choose a size** blade, select a **Standard** SKU, such as **D2s_v3**, and then click **Select**.</span></span>

1. <span data-ttu-id="90708-143">**[設定]** ブレードで **[可用性セット]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-143">On the **Settings** blade, click **Availability set**.</span></span>

1. <span data-ttu-id="90708-144">**[可用性セットの変更]** ブレードで、**[新規作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-144">On the **Change availability set** blade, click **Create new**.</span></span>

1. <span data-ttu-id="90708-145">**[新規作成]** ブレードで、**[名前]** ボックスに「**woodgrove-AS**」と入力して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-145">On the **Create new** blade, in the **Name** box, type **woodgrove-AS**, and then click **OK**.</span></span>

1. <span data-ttu-id="90708-146">**[設定]** ブレードで、**[ネットワーク セキュリティ グループ]** の **[詳細設定]** をクリックして、**(新規) woodgrove-SVR01 nsg** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-146">On the **Settings** blade, under **Network Security Group**, click **Advanced**, and then click **(new) woodgrove-SVR01-nsg**.</span></span>

1. <span data-ttu-id="90708-147">**[ネットワーク セキュリティ グループの作成]** ブレードの **[名前]** ボックスで、名前を「**woodgrove-NSG**」に変更して、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-147">On the **Create network Security group** blade, in the **Name** box, change the name to **woodgrove-NSG**, and then click **OK**.</span></span>

1. <span data-ttu-id="90708-148">**[設定]** ブレードで **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-148">On the **Settings** blade, click **OK**.</span></span>

<span data-ttu-id="90708-149">設定をテンプレートに保存し、複数の VM を簡単に展開できるようにします。</span><span class="sxs-lookup"><span data-stu-id="90708-149">Save the settings to a template, so that you easily deploy multiple VMs.</span></span>

1. <span data-ttu-id="90708-150">**[作成]** ブレードで、**[テンプレートとパラメーターのダウンロード]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-150">On the **Create** blade, click **Download template and parameters**.</span></span>

1. <span data-ttu-id="90708-151">**[テンプレート]** ブレードで、**[ライブラリに追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-151">On the **Template** blade, click **Add to library**.</span></span>

1. <span data-ttu-id="90708-152">**[テンプレートの保存]** ブレードで、**[名前]** ボックスと **[説明]** ボックスに「**woodgrove-server-template**」と入力して、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-152">On the **Save template** blade, in the **Name** and **Description** boxes, type **woodgrove-server-template**, and then click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="90708-153">このテンプレートを探す必要がある場合は、左側のメニューで **[すべてのサービス]** をクリックし、フィルター ボックスに「**テンプレート**」と入力して、**[Templates (PREVIEW)]\(テンプレート (プレビュー)\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-153">If you need to find this template, click **All services** in the left menu, type **template** in the filter box, and then click **Templates (PREVIEW)**.</span></span>

## <a name="use-the-template-to-provision-the-first-vm"></a><span data-ttu-id="90708-154">テンプレートを使用して最初の VM をプロビジョニングする</span><span class="sxs-lookup"><span data-stu-id="90708-154">Use the template to provision the first VM</span></span>

1. <span data-ttu-id="90708-155">**[テンプレート]** ブレードで、**[デプロイ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-155">On the **Template** blade, click **Deploy**.</span></span>

1. <span data-ttu-id="90708-156">**[カスタム デプロイ]** ブレードの **[リソース グループ]** で **[既存のものを使用]** を選択し、一覧で **woodgrove-RG** を選択します。</span><span class="sxs-lookup"><span data-stu-id="90708-156">On the **Custom deployment** blade, under **Resource Group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="90708-157">**[カスタム デプロイ]** ブレードの **[管理パスワード]** ボックスに、以前に使用したものと同じパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="90708-157">On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.</span></span>

1. <span data-ttu-id="90708-158">**[カスタム デプロイ]** ブレードで、**[上記のご契約条件に同意します]** チェック ボックスをオンにし、**[購入]** をクリックします (コストは通常の Azure コンピューティング料金であり、VM の価格レベルに依存します)。</span><span class="sxs-lookup"><span data-stu-id="90708-158">On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).</span></span>

1. <span data-ttu-id="90708-159">VM が展開されるまで待ってから、演習を続行します。これにより、他の VM のプロビジョニングに使用する前にテンプレートが正しく構成されていることを確認でき、関連するすべてのリソースが作成されます。</span><span class="sxs-lookup"><span data-stu-id="90708-159">Wait until the VM has deployed before continuing with the exercise; this is so you can be sure the template is correctly configured before you use it to provision additional VMs, and all the associated resources have been created.</span></span>

## <a name="use-the-template-to-provision-two-additional-vms"></a><span data-ttu-id="90708-160">テンプレートを使用して他の 2 つの VM をプロビジョニングする</span><span class="sxs-lookup"><span data-stu-id="90708-160">Use the template to provision two additional VMs</span></span>

1. <span data-ttu-id="90708-161">Azure portal の **[テンプレート]** ブレードで、**[デプロイ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="90708-161">In the Azure portal, on the **Template** blade, click **Deploy**.</span></span>

1. <span data-ttu-id="90708-162">**[カスタム デプロイ]** ブレードの **[リソース グループ]** で **[既存のものを使用]** を選択し、一覧で **woodgrove-RG** を選択します。</span><span class="sxs-lookup"><span data-stu-id="90708-162">On the **Custom deployment** blade, under **Resource Group**, select **Use existing**, and in the list, select **woodgrove-RG**.</span></span>

1. <span data-ttu-id="90708-163">**[カスタム デプロイ]** ブレードで、**[仮想マシン名]** ボックスの名前を「**woodgrove-SVR02**」に変更します。</span><span class="sxs-lookup"><span data-stu-id="90708-163">On the **Custom deployment** blade, in the **Virtual Machine Name** box, change the name to **woodgrove-SVR02**.</span></span>

1. <span data-ttu-id="90708-164">**[カスタム デプロイ]** ブレードで、**[Network Interface Name]\(ネットワーク インターフェイス名\)** ボックスの名前を「**woodgrovesvr02222**」に変更します。</span><span class="sxs-lookup"><span data-stu-id="90708-164">On the **Custom deployment** blade, in the **Network Interface Name** box, change the name to **woodgrovesvr02222**.</span></span>

1. <span data-ttu-id="90708-165">**[カスタム デプロイ]** ブレードの **[管理パスワード]** ボックスに、以前に使用したものと同じパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="90708-165">On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.</span></span>

1. <span data-ttu-id="90708-166">**[カスタム デプロイ]** ブレードで、**[パブリック IP アドレス名]** ボックスの名前を「**woodgrove-SVR02-ip**」に変更します。</span><span class="sxs-lookup"><span data-stu-id="90708-166">On the **Custom deployment** blade, in the **Public Ip Address Name** box, change the name to **woodgrove-SVR02-ip**.</span></span>

1. <span data-ttu-id="90708-167">**[カスタム デプロイ]** ブレードで、**[上記のご契約条件に同意します]** チェック ボックスをオンにし、**[購入]** をクリックします (コストは通常の Azure コンピューティング料金であり、VM の価格レベルに依存します)。</span><span class="sxs-lookup"><span data-stu-id="90708-167">On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).</span></span>

1. <span data-ttu-id="90708-168">次の情報を使用して、手順 1 - 7 を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="90708-168">Repeat steps 1 - 7, using the following information:</span></span>
    - <span data-ttu-id="90708-169">仮想マシン名: **woodgrove-SVR03**</span><span class="sxs-lookup"><span data-stu-id="90708-169">Virtual Machine Name: **woodgrove-SVR03**</span></span>
    - <span data-ttu-id="90708-170">Network Interface Name (ネットワーク インターフェイス名): **woodgrovesvr03333**</span><span class="sxs-lookup"><span data-stu-id="90708-170">Network Interface Name: **woodgrovesvr03333**</span></span>
    - <span data-ttu-id="90708-171">パブリック IP アドレス名: **woodgrove-SVRr03-ip**</span><span class="sxs-lookup"><span data-stu-id="90708-171">Public Ip Address Name: **woodgrove-SVRr03-ip**</span></span>

1. <span data-ttu-id="90708-172">VM が展開されるまで待ってから、演習を続けます。</span><span class="sxs-lookup"><span data-stu-id="90708-172">Wait until the VMs have deployed before continuing with the exercise.</span></span>

<span data-ttu-id="90708-173">パブリック ロード バランサーを構成できる状態になり、このロード バランサーで 3 つの VM を使用する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="90708-173">You now have a public load balancer ready to configure, and three VMs ready to use with this load balancer.</span></span>