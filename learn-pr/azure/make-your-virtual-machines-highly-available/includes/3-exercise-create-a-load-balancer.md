<span data-ttu-id="82f94-101">Woodgrove Bank で働いていて、オンライン銀行サービスを立ち上げようとしていたことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="82f94-101">Recall that you work for Woodgrove Bank, and that you are about to launch online banking services.</span></span> <span data-ttu-id="82f94-102">このセクターは競争が激しいので、99.99% 以上のサービス可用性を保証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82f94-102">This sector is highly competitive, so you need to guarantee of a minimum of 99.99% service availability.</span></span> <span data-ttu-id="82f94-103">あなたは、3 つの仮想マシンを含むプールを使用する Azure Load Balancer でこの目標が達成されると判断しました。</span><span class="sxs-lookup"><span data-stu-id="82f94-103">You have determined that Azure Load Balancer with a pool of three virtual machines will meet this goal.</span></span>

<span data-ttu-id="82f94-104">この演習では、Azure portal を使用してロード バランサーと仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="82f94-104">In this exercise, you will create a load balancer and a virtual network using the Azure portal.</span></span> <span data-ttu-id="82f94-105">必要なのは 1 つだけなので、ポータルで作成するのが簡単です。</span><span class="sxs-lookup"><span data-stu-id="82f94-105">Since we only need one of these, the portal is an easy way to create it.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-public-load-balancer"></a><span data-ttu-id="82f94-106">パブリック ロード バランサーを作成する</span><span class="sxs-lookup"><span data-stu-id="82f94-106">Create a public load balancer</span></span>

1. <span data-ttu-id="82f94-107">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="82f94-107">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="82f94-108">サイドバーで **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-108">In the sidebar, click **Create a resource**.</span></span>

1. <span data-ttu-id="82f94-109">**[ネットワーキング]** セクションを選択し、**[Load Balancer]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-109">Select the **Networking** section, and then click **Load Balancer**.</span></span> <span data-ttu-id="82f94-110">選択肢が表示されない場合は、検索ボックスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="82f94-110">If you don't see that choice, you can use the search box.</span></span>

    ![[ネットワーキング] セクションが選択されて [Load Balancer] が強調表示されている Azure Marketplace を示すスクリーンショット](../media/3-azure-marketplace.png)

1. <span data-ttu-id="82f94-112">**[ロード バランサーの作成]** ブレードで次の情報を入力または選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-112">In the **Create load balancer** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="82f94-113">**名前:** _woodgrove-LB_</span><span class="sxs-lookup"><span data-stu-id="82f94-113">**Name:** _woodgrove-LB_</span></span>
    - <span data-ttu-id="82f94-114">**種類:** _パブリック_</span><span class="sxs-lookup"><span data-stu-id="82f94-114">**Type:** _Public_</span></span>
    - <span data-ttu-id="82f94-115">**SKU:** _Basic_</span><span class="sxs-lookup"><span data-stu-id="82f94-115">**SKU:** _Basic_</span></span>
    - <span data-ttu-id="82f94-116">**パブリック IP アドレス**: **[新規作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-116">**Public IP address:** Select **Create new**.</span></span> <span data-ttu-id="82f94-117">テキスト ボックスに「_woodgrove-LB-ip_」と入力します。</span><span class="sxs-lookup"><span data-stu-id="82f94-117">In the text box, type _woodgrove-LB-ip_.</span></span> <span data-ttu-id="82f94-118">[割り当て] は _[動的]_ のままにします。</span><span class="sxs-lookup"><span data-stu-id="82f94-118">Leave the Assignment as _Dynamic_.</span></span>
    - <span data-ttu-id="82f94-119">**サブスクリプション:** "_コンシェルジェ サブスクリプション_" が既に選択されているはずです。</span><span class="sxs-lookup"><span data-stu-id="82f94-119">**Subscription:** It should already have _Concierge Subscription_ selected.</span></span>
    - <span data-ttu-id="82f94-120">**リソース グループ**: **[既存のものを使用]** を選択し、_<rgn>[サンドボックス リソース グループ名]</rgn>_ を選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-120">**Resource group:** Select **Use existing** and choose _<rgn>[sandbox resource group name]</rgn>_.</span></span>
    - <span data-ttu-id="82f94-121">**場所**: 次の一覧から、近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-121">**Location:** Select a region near you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    ![新しいロード バランサーの作成画面のスクリーンショット](../media/3-create-load-balancer.png)

1. <span data-ttu-id="82f94-123">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-123">Click **Create**.</span></span>

<span data-ttu-id="82f94-124">ロード バランサー リソースが作成されてデプロイされている間に、バックエンド サブネットに使用する Azure 仮想ネットワークを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="82f94-124">While the load balancer resource is being created and deployed, let's create the Azure Virtual Network we'll use for the backend subnet.</span></span>

## <a name="create-a-virtual-network"></a><span data-ttu-id="82f94-125">仮想ネットワークを作成する</span><span class="sxs-lookup"><span data-stu-id="82f94-125">Create a virtual network</span></span>

1. <span data-ttu-id="82f94-126">左側のメニューで、**[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-126">In the left menu, click **Create a resource**.</span></span> <span data-ttu-id="82f94-127">**[新規]** ブレードで **[ネットワーキング]** をクリックして、**[仮想ネットワーク]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-127">In the **New** blade, click **Networking**, and then click **Virtual network**.</span></span>

    ![[ネットワーキング] セクションが選択されて [Load Balancer] が強調表示されている Azure Marketplace を示すスクリーンショット](../media/3-azure-marketplace-2.png)

1. <span data-ttu-id="82f94-129">**[仮想ネットワークの作成]** ブレードで次の情報を入力または選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-129">In the **Create virtual network** blade, enter or select the following information:</span></span>
    - <span data-ttu-id="82f94-130">**名前:** _woodgrove-VNET_</span><span class="sxs-lookup"><span data-stu-id="82f94-130">**Name:** _woodgrove-VNET_</span></span>
    - <span data-ttu-id="82f94-131">**アドレス空間:** _172.20.0.0/16_</span><span class="sxs-lookup"><span data-stu-id="82f94-131">**Address space:** _172.20.0.0/16_</span></span>
    - <span data-ttu-id="82f94-132">**サブスクリプション:** "_コンシェルジェ サブスクリプション_" が既に選択されているはずです。</span><span class="sxs-lookup"><span data-stu-id="82f94-132">**Subscription:** It should already have _Concierge Subscription_ selected.</span></span>
    - <span data-ttu-id="82f94-133">**リソース グループ**: 一覧から既存の _<rgn>[サンドボックス リソース グループ名]</rgn>_ リソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-133">**Resource group:** Select the existing _<rgn>[Resource Group Name]</rgn>_ resource group from the list.</span></span>
    - <span data-ttu-id="82f94-134">**サブネット名:** _backendSubnet_</span><span class="sxs-lookup"><span data-stu-id="82f94-134">**Subnet name:** _backendSubnet_</span></span>
    - <span data-ttu-id="82f94-135">**サブネットのアドレス範囲:** _172.20.0.0/24_</span><span class="sxs-lookup"><span data-stu-id="82f94-135">**Subnet address range:** _172.20.0.0/24_</span></span>
    - <span data-ttu-id="82f94-136">**DDoS 保護:** _Basic_</span><span class="sxs-lookup"><span data-stu-id="82f94-136">**DDoS protection:** _Basic_</span></span>
    - <span data-ttu-id="82f94-137">**サービス エンドポイント:** _無効_</span><span class="sxs-lookup"><span data-stu-id="82f94-137">**Service endpoints:** _Disabled_</span></span>

    ![新しいロード バランサーの作成画面のスクリーンショット](../media/3-create-vnet.png)

1. <span data-ttu-id="82f94-139">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-139">Click **Create**.</span></span>

<span data-ttu-id="82f94-140">仮想ネットワークがデプロイされている間に、ネットワーク セキュリティ グループを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="82f94-140">While the virtual network is being deployed, let's create one more thing: a network security group.</span></span>

## <a name="create-and-configure-a-network-security-group"></a><span data-ttu-id="82f94-141">ネットワーク セキュリティ グループを作成して構成する</span><span class="sxs-lookup"><span data-stu-id="82f94-141">Create and configure a network security group</span></span>

1. <span data-ttu-id="82f94-142">**[リソースの作成]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="82f94-142">Click **Create a resource**</span></span>

1. <span data-ttu-id="82f94-143">**[ネットワーキング]** グループを選択して、**[ネットワーク セキュリティ グループ]** 項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-143">Select the **Networking** group, and Click on the **Network security group** item.</span></span>

    ![[ネットワーキング] セクションが選択されて [ネットワーク セキュリティ グループ] が強調表示されている Azure Marketplace を示すスクリーンショット](../media/3-azure-marketplace-3.png)


1. <span data-ttu-id="82f94-145">**woodgrove-NSG** という名前を付けて、Azure サンドボックス リソース グループに入れます。</span><span class="sxs-lookup"><span data-stu-id="82f94-145">Give it the name **woodgrove-NSG** and put it into the Azure sandbox resource group.</span></span>

1. <span data-ttu-id="82f94-146">Azure Load Balancer と同じ場所にあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="82f94-146">Make sure it's in the same location as the Azure Load Balancer.</span></span>

    ![ネットワーク セキュリティ グループ作成画面のスクリーンショット](../media/3-create-nsg.png)

1. <span data-ttu-id="82f94-148">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="82f94-148">Click **Create**.</span></span>

<span data-ttu-id="82f94-149">ロード バランサー、仮想ネットワーク、NSG がデプロイされるのを待ちます。</span><span class="sxs-lookup"><span data-stu-id="82f94-149">Wait until the load balancer, virtual network, and NSG are deployed.</span></span> <span data-ttu-id="82f94-150">その後、ネットワーク セキュリティを構成できます。</span><span class="sxs-lookup"><span data-stu-id="82f94-150">Then we can configure the network security.</span></span>

## <a name="configure-the-network-security-group"></a><span data-ttu-id="82f94-151">ネットワーク セキュリティ グループを構成する</span><span class="sxs-lookup"><span data-stu-id="82f94-151">Configure the network security group</span></span>

1. <span data-ttu-id="82f94-152">デプロイの通知から、または Azure portal の上部にある検索バーを使用して、新しいネットワーク セキュリティ グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-152">Select your new network security group - either from the deployment notification, or through the search bar at the top of the Azure portal.</span></span>

    ![[通知] パネルのデプロイ成功を示すスクリーンショット](../media/3-deployment-success.png)

1. <span data-ttu-id="82f94-154">**[設定]** > **[受信セキュリティ規則]** セクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-154">Select the **Settings** > **Inbound security rules** section.</span></span> <span data-ttu-id="82f94-155">常に適用される 3 つの定義済み規則があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="82f94-155">Notice that we have three pre-defined rules that are always applied.</span></span> <span data-ttu-id="82f94-156">次のような規則です。</span><span class="sxs-lookup"><span data-stu-id="82f94-156">These rules are:</span></span>
    - <span data-ttu-id="82f94-157">**AllowVnetInbound** - 仮想ネットワーク上を流れるすべての内部トラフィックを許可します。</span><span class="sxs-lookup"><span data-stu-id="82f94-157">**AllowVnetInbound** - Allow all internal traffic flowing on the virtual network.</span></span> <span data-ttu-id="82f94-158">この規則により、ネットワークを共有する VM が相互に通信できます。</span><span class="sxs-lookup"><span data-stu-id="82f94-158">This rule allows VMs that share the network to talk to each other.</span></span>
    - <span data-ttu-id="82f94-159">**AllowAzureLoadBalancerInBound** - サービスが動作しているかどうかを確認するため、ロード バランサーがネットワーク上のサービスに対して "ping" を行うのを許可します。</span><span class="sxs-lookup"><span data-stu-id="82f94-159">**AllowAzureLoadBalancerInBound** - Allow the load balancer to "ping" services on the network to see whether they are alive.</span></span>
    - <span data-ttu-id="82f94-160">**DenyAllInbound** - 他のすべてのトラフィックを拒否します。</span><span class="sxs-lookup"><span data-stu-id="82f94-160">**DenyAllInbound** - Deny all other traffic.</span></span>

    <span data-ttu-id="82f94-161">**DenyAllInbound** 規則は特に重要です。これにより、優先度の高い規則を持たないすべての受信トラフィックが確実にブロックされます。</span><span class="sxs-lookup"><span data-stu-id="82f94-161">The **DenyAllInbound** rule is particularly important - it ensures that all inbound traffic that doesn't have a higher priority rule is blocked.</span></span> <span data-ttu-id="82f94-162">これが理由で、Web サーバーに対する HTTP (80) トラフィックを許可する新しい規則を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="82f94-162">That's why we will need to add a new rule to allow HTTP (80) traffic for our web servers.</span></span>

    > [!NOTE]
    > <span data-ttu-id="82f94-163">NSG の規則の優先順位は 0 から 65500 で、規則は順番に評価されます。</span><span class="sxs-lookup"><span data-stu-id="82f94-163">Priority in NSG rules goes from 0 - 65500 and rules are evaluated in order.</span></span> <span data-ttu-id="82f94-164">最初に一致した規則によって動作が決まります。</span><span class="sxs-lookup"><span data-stu-id="82f94-164">The first matching rule becomes the decision maker.</span></span> <span data-ttu-id="82f94-165">独自の規則は常にある程度低くしておきたいので、定義済みの規則より優先されるように 1000 あたりから始めます。</span><span class="sxs-lookup"><span data-stu-id="82f94-165">You will always want to place your rules fairly low - starting around 1000 so they take precedence over the pre-defined ones.</span></span>

1. <span data-ttu-id="82f94-166">**[追加]** をクリックして新しい規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="82f94-166">Click **Add** to add a new rule.</span></span>

    ![[追加] ボタンが強調表示されている受信ネットワーク セキュリティ規則を示すスクリーンショット。](../media/3-inbound-security-rules.png)

1. <span data-ttu-id="82f94-168">上部にある **[基本]** をクリックして、"基本" ビューに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="82f94-168">Click the **Basic** button at the top to switch the the "basic" view.</span></span>

1. <span data-ttu-id="82f94-169">新しい規則の詳細を入力します。</span><span class="sxs-lookup"><span data-stu-id="82f94-169">Fill in the details for the new rule.</span></span>
    - <span data-ttu-id="82f94-170">**[サービス]** では _[HTTP]_ を選択します。</span><span class="sxs-lookup"><span data-stu-id="82f94-170">Select _HTTP_ for the **Service**.</span></span>
    - <span data-ttu-id="82f94-171">**[優先度]** を _1000_ に設定します。</span><span class="sxs-lookup"><span data-stu-id="82f94-171">Set the **Priority** to _1000_.</span></span>
    - <span data-ttu-id="82f94-172">規則の名前を指定します (または、既定のままにします)。</span><span class="sxs-lookup"><span data-stu-id="82f94-172">Give the rule a name (or leave the default).</span></span>

    ![HTTP の詳細を入力した受信セキュリティ規則追加ダイアログ ボックスのスクリーンショット](../media/3-add-inbound-rule.png)

1. <span data-ttu-id="82f94-174">**[追加]** をクリックして規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="82f94-174">Click **Add** to add the rule.</span></span>

    ![NSG 規則一覧で新しく追加した HTTP 規則を示すスクリーンショット](../media/3-new-added-rule.png)

## <a name="apply-the-network-security-group-to-the-vnet"></a><span data-ttu-id="82f94-176">VNet にネットワーク セキュリティ グループを適用する</span><span class="sxs-lookup"><span data-stu-id="82f94-176">Apply the network security group to the VNet</span></span>

<span data-ttu-id="82f94-177">次に、この NSG を仮想ネットワークに適用してみましょう。</span><span class="sxs-lookup"><span data-stu-id="82f94-177">Next, let's apply this NSG to the virtual network.</span></span>

1. <span data-ttu-id="82f94-178">自分の仮想ネットワークを探します。上部にある検索ボックスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="82f94-178">Find your virtual network - you can use the search box at the top.</span></span> <span data-ttu-id="82f94-179">名前は **woodgrove-VNET** です。</span><span class="sxs-lookup"><span data-stu-id="82f94-179">It's named **woodgrove-VNET**.</span></span>

1. <span data-ttu-id="82f94-180">**[設定]** > **[サブネット]** を選択して、定義したサブネットに移動します。</span><span class="sxs-lookup"><span data-stu-id="82f94-180">Select **Settings** > **Subnets** to get to your defined subnets.</span></span>

1. <span data-ttu-id="82f94-181">**backendSubnet** エントリをクリックして、プロパティを開きます。</span><span class="sxs-lookup"><span data-stu-id="82f94-181">Click the **backendSubnet** entry to open the properties.</span></span>

    ![サブネットの [サブネット] 領域の backendSubnet エントリを示すスクリーンショット](../media/3-subnets.png)

1. <span data-ttu-id="82f94-183">**[ネットワーク セキュリティ グループ]** の [なし] エントリをクリックします</span><span class="sxs-lookup"><span data-stu-id="82f94-183">Click the "None" entry on the **Network security group**</span></span>

    ![backendSubnet の空のネットワーク セキュリティを示すスクリーンショット](../media/3-add-network-security-group.png)

1. <span data-ttu-id="82f94-185">**woodgrove-NSG** を選択してこの VNET に追加します。</span><span class="sxs-lookup"><span data-stu-id="82f94-185">Select the **woodgrove-NSG** to add it to this VNET.</span></span>

1. <span data-ttu-id="82f94-186">**[保存]** をクリックして変更を適用します。</span><span class="sxs-lookup"><span data-stu-id="82f94-186">Click **Save** to apply the change.</span></span>

<span data-ttu-id="82f94-187">ネットワークの準備ができたので、このネットワークに配置する仮想マシンをいくつか作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="82f94-187">Now that our network is ready, let's create some virtual machines to sit on this network!</span></span>