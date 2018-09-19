<span data-ttu-id="be9e1-101">ネットワーク セキュリティ グループをネットワークに適用し、サーバーに対して HTTP トラフィックだけを許可してみましょう。</span><span class="sxs-lookup"><span data-stu-id="be9e1-101">Let's apply a network security group to our network, so that we only allow HTTP traffic through our server.</span></span>

## <a name="create-a-network-security-group"></a><span data-ttu-id="be9e1-102">ネットワーク セキュリティ グループを作成する</span><span class="sxs-lookup"><span data-stu-id="be9e1-102">Create a network security group</span></span>

<span data-ttu-id="be9e1-103">SSH アクセスが必要であることを示したため、Azure ではセキュリティ グループが作成されているはずです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-103">Azure should have created a security group for us because we indicated we wanted SSH access.</span></span> <span data-ttu-id="be9e1-104">しかし、ここではプロセス全体を確認するために、新しいセキュリティ グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-104">But let's create a new security group, so you can walk through the entire process.</span></span> <span data-ttu-id="be9e1-105">VM の "_前_" に仮想ネットワークを作成することにした場合、これは特に重要です。</span><span class="sxs-lookup"><span data-stu-id="be9e1-105">This is particularly important if you decide to create your virtual network _before_ your VMs.</span></span> <span data-ttu-id="be9e1-106">前述のとおり、セキュリティ グループは "_省略可能_" であり、必ずしもネットワークと共に作成されるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="be9e1-106">As mentioned earlier, security groups are _optional_ and not necessarily created with the network.</span></span>

1. <span data-ttu-id="be9e1-107">[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) で、左隅のサイドバーにある **[リソースの作成]** ボタンをクリックして、新しいリソースの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-107">In the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true), click the **Create a resource** button in the left-corner sidebar to start a new resource creation.</span></span>

1. <span data-ttu-id="be9e1-108">フィルター ボックスに「**ネットワーク セキュリティ グループ**」と入力し、一覧で一致する項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-108">Type **Network security group** into the filter box and select the matching item in the list.</span></span>

1. <span data-ttu-id="be9e1-109">**[リソース マネージャー]** デプロイ モデルが選択されていることを確認し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="be9e1-109">Make sure the **Resource Manager** deployment model is selected and click **Create**.</span></span>

1. <span data-ttu-id="be9e1-110">セキュリティ グループの **[名前]** を指定します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-110">Provide a **Name** for your security group.</span></span> <span data-ttu-id="be9e1-111">ここでも、名前付け規則を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="be9e1-111">Again, naming conventions are a good idea here.</span></span> <span data-ttu-id="be9e1-112">**Test Web Network Security Group #1 in East US** の場合は **test-web-eus-nsg1** を使用します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-112">Let's use **test-web-eus-nsg1** for **Test Web Network Security Group #1 in East US**.</span></span> <span data-ttu-id="be9e1-113">セキュリティ グループをどこに配置するかに応じて、名前の場所の部分を変更してください。</span><span class="sxs-lookup"><span data-stu-id="be9e1-113">You'll likely want to change the location portion of the name to reflect where you put the security group.</span></span>

1. <span data-ttu-id="be9e1-114">適切な**サブスクリプション**を選択し、既存の**リソース グループ**を使用します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-114">Select the proper **Subscription** and use your existing **Resource group**.</span></span>

1. <span data-ttu-id="be9e1-115">最後に、VM/仮想ネットワークと同じ**場所**に配置します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-115">Finally, put it into the same **Location** as the VM / virtual network.</span></span> <span data-ttu-id="be9e1-116">これは重要なことです。場所が異なる場合、このリソースを適用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-116">This is important; you won't be able to apply this resource if it's in a different location.</span></span>

1. <span data-ttu-id="be9e1-117">**[作成]** をクリックしてグループを作成します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-117">Click **Create** to create the group.</span></span>

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a><span data-ttu-id="be9e1-118">ネットワーク セキュリティ グループに新しい受信規則を追加する</span><span class="sxs-lookup"><span data-stu-id="be9e1-118">Add a new inbound rule to our network security group</span></span>

<span data-ttu-id="be9e1-119">デプロイはすぐに完了するはずです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-119">Deployment should complete quickly.</span></span> <span data-ttu-id="be9e1-120">完了したら、新しい規則をセキュリティ グループに追加できます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-120">When it's finished, we can add new rules to our security group:</span></span>

1. <span data-ttu-id="be9e1-121">Azure portal で新しいセキュリティ グループのリソースを見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-121">Find the new security group resource and select it in the Azure portal.</span></span>

1. <span data-ttu-id="be9e1-122">概要ページには、ネットワークをロックダウンするために作成された既定の規則がいくつか表示されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-122">On the overview page, you'll find that it has some default rules created to lock down the network.</span></span>

    <span data-ttu-id="be9e1-123">受信側:</span><span class="sxs-lookup"><span data-stu-id="be9e1-123">On the inbound side:</span></span>

    - <span data-ttu-id="be9e1-124">VNet 間の受信トラフィックはすべて許可されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-124">All incoming traffic from one VNet to another is allowed.</span></span> <span data-ttu-id="be9e1-125">これにより、VNet 上のリソースは相互に通信することができます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-125">This lets resources on the VNet talk to each other.</span></span>
    - <span data-ttu-id="be9e1-126">Azure Load Balancer の**プローブ**によって、VM が有効であることを確認するように求められます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-126">Azure Load Balancer **probe** requests to ensure the VM is alive.</span></span>
    - <span data-ttu-id="be9e1-127">他の受信トラフィックはすべて拒否されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-127">All other inbound traffic is denied.</span></span>  

    <span data-ttu-id="be9e1-128">送信側:</span><span class="sxs-lookup"><span data-stu-id="be9e1-128">On the outbound side:</span></span>  
    - <span data-ttu-id="be9e1-129">VNet 上のすべてのネットワーク内のトラフィックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-129">All in-network traffic on the VNet is allowed.</span></span>
    - <span data-ttu-id="be9e1-130">インターネットへの送信トラフィックはすべて許可されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-130">All outbound traffic to the internet is permitted.</span></span>
    - <span data-ttu-id="be9e1-131">他の送信トラフィックはすべて拒否されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-131">All other outbound traffic is denied.</span></span>

    > [!NOTE]  
    > <span data-ttu-id="be9e1-132">これらの既定の規則には優先度の高い値が設定されています。これは、これらの規則が "_最後_" に評価されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-132">These default rules are set with high-priority values, which means that they get evaluated _last_.</span></span> <span data-ttu-id="be9e1-133">これを変更したり、削除したりすることはできませんが、優先度の低い値のトラフィックと一致するようにより具体的な規則を作成することで、"_上書き_" することができます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-133">They cannot be changed or deleted, but you can _override_ them by creating more specific rules to match your traffic with a lower priority value.</span></span>

1. <span data-ttu-id="be9e1-134">セキュリティ グループの **[設定]** パネルで **[受信セキュリティ規則]** セクションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="be9e1-134">Click the **Inbound security rules** section in the **Settings** panel for the security group.</span></span>

1. <span data-ttu-id="be9e1-135">**[+ 追加]** をクリックして新しいセキュリティ規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-135">Click **+ Add** to add a new security rule.</span></span>

    ![[追加] ボタンが強調表示されている受信セキュリティ規則の設定を示す Azure portal のスクリーンショット。](../media/8-add-rule.png)

    <span data-ttu-id="be9e1-137">セキュリティ規則に必要な情報を入力する方法には、基本と高度の 2 つがあります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-137">There are two ways to enter the information necessary for a security rule: basic and advanced.</span></span> <span data-ttu-id="be9e1-138">**[追加]** パネルの上部にあるボタンをクリックして、これらを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-138">You can switch between them by clicking the button at the top of the **add** panel.</span></span>

    ![Azure portal の 2 つのスクリーンショット。基本と詳細のルール入力を切り替える様子を確認できます。トグル ボタンの 2 つの状態を結ぶ矢印が強調表示されています。](../media/8-advanced-create-rule.png)

    <span data-ttu-id="be9e1-140">高度モードでは、ルールを完全にカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-140">The advanced mode provides the ability to customize the rule completely.</span></span> <span data-ttu-id="be9e1-141">ただし、既知のプロトコルを構成するだけの場合は、基本モードの方が操作は少し簡単です。</span><span class="sxs-lookup"><span data-stu-id="be9e1-141">However, if you need to configure a known protocol, the basic mode is a bit easier to work with.</span></span>

1. <span data-ttu-id="be9e1-142">基本モードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-142">Switch to the basic mode.</span></span>

1. <span data-ttu-id="be9e1-143">HTTP 規則に関する情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-143">Add the information for our HTTP rule:</span></span>

    - <span data-ttu-id="be9e1-144">**[サービス]** を HTTP に設定します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-144">Set the **Service** to be HTTP.</span></span> <span data-ttu-id="be9e1-145">これでポートの範囲が設定されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-145">This will set your port range up for you.</span></span>
    - <span data-ttu-id="be9e1-146">**[優先度]** を **1000** に設定します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-146">Set the **Priority** to **1000**.</span></span> <span data-ttu-id="be9e1-147">既定値の **[拒否]** 規則よりも低い数値にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-147">It has to be a lower number than the default **Deny** rule.</span></span> <span data-ttu-id="be9e1-148">任意の値で範囲を開始することができますが、後で例外を作成する必要がある場合は、余裕を持たせることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="be9e1-148">You can start the range at any value, but it's recommended that you give yourself some space in case an exception needs to be created later.</span></span>
    - <span data-ttu-id="be9e1-149">規則に名前を付けます。ここでは、**allow-http-traffic** を使用します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-149">Give the rule a name; we'll use **allow-http-traffic**.</span></span>
    - <span data-ttu-id="be9e1-150">規則の説明を入力します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-150">Give the rule a description.</span></span>

1. <span data-ttu-id="be9e1-151">**高度な**モードに戻ります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-151">Switch back to the **Advanced** mode.</span></span> <span data-ttu-id="be9e1-152">設定がまだ存在することに注目してください。</span><span class="sxs-lookup"><span data-stu-id="be9e1-152">Notice that our settings are still present.</span></span> <span data-ttu-id="be9e1-153">このパネルを使用して、より詳細な設定を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-153">We can use this panel to create more fine-grained settings.</span></span> <span data-ttu-id="be9e1-154">たとえば、**[ソース]** を特定の IP アドレスまたはカメラに固有の IP アドレスの範囲に制限できます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-154">In particular, we would likely restrict the **Source** to be a specific IP address or range of IP addresses specific to the cameras.</span></span> <span data-ttu-id="be9e1-155">ローカル コンピューターの現在の IP アドレスがわかっている場合は、それを試すことができます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-155">If you know the current IP address of your local computer, you can try that.</span></span> <span data-ttu-id="be9e1-156">それ以外の場合は、設定を **[任意]** のままにして、規則をテストできるようにします。</span><span class="sxs-lookup"><span data-stu-id="be9e1-156">Otherwise, leave the setting as **Any**, so you can test the rule.</span></span>

1. <span data-ttu-id="be9e1-157">**[追加]** をクリックして、規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-157">Click **Add** to create the rule.</span></span> <span data-ttu-id="be9e1-158">これで受信規則の一覧が更新されます。規則が優先順になっていることに注目してください。これは規則を確認する順序です。</span><span class="sxs-lookup"><span data-stu-id="be9e1-158">This will update the list of inbound rules - notice they are in priority order, which is how they will be examined.</span></span>

## <a name="apply-the-security-group"></a><span data-ttu-id="be9e1-159">セキュリティ グループを適用する</span><span class="sxs-lookup"><span data-stu-id="be9e1-159">Apply the security group</span></span>

<span data-ttu-id="be9e1-160">セキュリティ グループを単一の VM を保護するためにネットワーク インターフェイスに適用するか、サブネットに適用し、そのサブネット上のリソースに適用できることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="be9e1-160">Recall that we can apply the security group to a network interface to guard a single VM or to a subnet where it would apply to any resources on that subnet.</span></span> <span data-ttu-id="be9e1-161">後者のアプローチが最も一般的であるため、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-161">The latter approach tends to be the most common, so let's do that.</span></span> <span data-ttu-id="be9e1-162">Azure でこのリソースにアクセスすることができます。その場合、仮想ネットワーク リソースを経由するか、仮想ネットワークを使用している VM 経由で間接的にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-162">We could get to this resource in Azure through either the virtual network resource or indirectly through the VM that is using the virtual network.</span></span>

1. <span data-ttu-id="be9e1-163">仮想マシンの **[概要]** パネルに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-163">Switch to the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="be9e1-164">**[すべてのリソース]** の下に VM があります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-164">You can find the VM under **All Resources**.</span></span>

1. <span data-ttu-id="be9e1-165">**[設定]** セクションで **[ネットワーク]** という項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-165">Select the **Networking** item in the **Settings** section.</span></span>

1. <span data-ttu-id="be9e1-166">ネットワーク プロパティには、**[仮想ネットワーク/サブネット]** を含む、VM に適用されるネットワークに関する情報があります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-166">In the networking properties, you will find information about the networking applied to the VM, including the **Virtual network/subnet**.</span></span> <span data-ttu-id="be9e1-167">これは、リソースにアクセスするためのクリック可能なリンクです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-167">This is a clickable link to get to the resource.</span></span> <span data-ttu-id="be9e1-168">これをクリックして仮想ネットワークを開きます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-168">Click it to open the virtual network.</span></span> <span data-ttu-id="be9e1-169">このリンクは、VM の **[概要]** パネル "_でも_" 使用できます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-169">This link is _also_ available on the **Overview** panel of the VM.</span></span> <span data-ttu-id="be9e1-170">これらのいずれかをクリックすると、仮想ネットワークの **[概要]** が開きます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-170">Either of these will open the **Overview** of the virtual network.</span></span>

1. <span data-ttu-id="be9e1-171">**[設定]** セクションで、**[サブネット]** 項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-171">In the **Settings** section, select the **Subnets** item.</span></span>

1. <span data-ttu-id="be9e1-172">以前に VM とネットワークを作成したときの定義された (既定) 単一のサブネットがあるはずです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-172">We should have a single subnet defined (default) from when we created the VM + network earlier.</span></span> <span data-ttu-id="be9e1-173">一覧の項目をクリックして詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-173">Click the item in the list to open the details.</span></span>

1. <span data-ttu-id="be9e1-174">**[ネットワーク セキュリティ グループ]** エントリをクリックします。</span><span class="sxs-lookup"><span data-stu-id="be9e1-174">Click the **Network security group** entry.</span></span>

1. <span data-ttu-id="be9e1-175">**test-web-eus-nsg1** という新しいセキュリティ グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-175">Select your new security group: **test-web-eus-nsg1**.</span></span> <span data-ttu-id="be9e1-176">ここには VM で作成された別のグループもあるはずです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-176">There should be another group here as well that was created with the VM.</span></span>

1. <span data-ttu-id="be9e1-177">**[保存]** をクリックして変更内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-177">Click **Save** to save the change.</span></span> <span data-ttu-id="be9e1-178">ネットワークに適用されるまで少し時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-178">It will take a minute to apply to the network.</span></span>

## <a name="update-the-nsg-on-the-network-interface"></a><span data-ttu-id="be9e1-179">ネットワーク インターフェイスで NSG を更新する</span><span class="sxs-lookup"><span data-stu-id="be9e1-179">Update the NSG on the network interface</span></span>

<span data-ttu-id="be9e1-180">サブネットに適用されている NSG でポート 80 が開いていますが、ネットワーク インターフェイスに適用されている NSG では現在許可されていないため、ブロックされます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-180">Port 80 is open on the NSG applied to the subnet, but it's still going to be blocked because it's not currently allowed on the NSG applied to the network interface.</span></span> <span data-ttu-id="be9e1-181">これを修正し、接続できるようにしましょう。</span><span class="sxs-lookup"><span data-stu-id="be9e1-181">Let's fix that and then we should be able to connect.</span></span>

1. <span data-ttu-id="be9e1-182">仮想マシンの **[概要]** パネルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-182">Switch back to the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="be9e1-183">**[すべてのリソース]** の下に VM があります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-183">You can find the VM under **All Resources**.</span></span>

1. <span data-ttu-id="be9e1-184">**[設定]** セクションで、**[ネットワーク]** 項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-184">In the **Settings** section, select the **Networking** item.</span></span>

1. <span data-ttu-id="be9e1-185">**[受信ポートの規則]** セクションに、サブネットの NSG ルールが表示され、そのすぐ下にネットワーク インターフェイスの NSG ルールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-185">In the **Inbound port rules** section you should see the NSG rules for the subnet, then the NSG rules for the network interface just below it.</span></span> <span data-ttu-id="be9e1-186">ネットワーク インターフェイスの NSG ルールで、**[受信ポートの規則を追加する]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-186">In the NSG rules for the network interface, select **Add inbound port rule**.</span></span>

1. <span data-ttu-id="be9e1-187">基本モードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-187">Switch to the basic mode.</span></span>

1. <span data-ttu-id="be9e1-188">HTTP 規則に関する情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-188">Add the information for our HTTP rule:</span></span>

    - <span data-ttu-id="be9e1-189">**[サービス]** を HTTP に設定します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-189">Set the **Service** to be HTTP.</span></span> <span data-ttu-id="be9e1-190">これにより、ポート範囲が設定されます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-190">This will set your port range up for you.</span></span>
    - <span data-ttu-id="be9e1-191">**[優先度]** を **310** に設定します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-191">Set the **Priority** to **310**.</span></span>
    - <span data-ttu-id="be9e1-192">規則に名前を付けます。ここでは、**allow-http-traffic** を使用します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-192">Give the rule a name; we'll use **allow-http-traffic**.</span></span>
    - <span data-ttu-id="be9e1-193">規則の説明を入力します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-193">Give the rule a description.</span></span>

1. <span data-ttu-id="be9e1-194">**[追加]** をクリックして、規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-194">Click **Add** to create the rule.</span></span>

## <a name="verify-the-rules"></a><span data-ttu-id="be9e1-195">規則を確認する</span><span class="sxs-lookup"><span data-stu-id="be9e1-195">Verify the rules</span></span>

<span data-ttu-id="be9e1-196">変更内容を検証しましょう。</span><span class="sxs-lookup"><span data-stu-id="be9e1-196">Let's validate the change:</span></span>

1. <span data-ttu-id="be9e1-197">仮想マシンの **[概要]** パネルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-197">Switch back to the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="be9e1-198">**[すべてのリソース]** の下に VM があります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-198">You can find the VM under **All Resources**.</span></span>

1. <span data-ttu-id="be9e1-199">**[設定]** セクションで **[ネットワーク]** という項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-199">Select the **Networking** item in the **Settings** section.</span></span>

1. <span data-ttu-id="be9e1-200">ネットワーク インターフェイスの詳細には、**[有効なセキュリティ規則]** のリンクがあります。これをクリックすると、規則がどのように評価されるのかがすぐにわかります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-200">In the network interface details, there is a link for **Effective security rules** that will quickly show you how rules are going to be evaluated.</span></span> <span data-ttu-id="be9e1-201">リンクをクリックして分析を開き、新しい規則が表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-201">Click the link to open the analysis and make sure you see your new rules.</span></span>

    ![ネットワークの [有効なセキュリティ規則] ブレードが表示された Azure portal のスクリーンショット。](../media/8-effective-rules.png)

1. <span data-ttu-id="be9e1-203">言うまでもなく、規則がすべて機能していることを検証する最良の方法は、サーバーに HTTP 要求を送信することです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-203">Of course, the best way to validate it's all working is to hit our server with an HTTP request.</span></span> <span data-ttu-id="be9e1-204">規則が機能していることがわかります。</span><span class="sxs-lookup"><span data-stu-id="be9e1-204">It should now work.</span></span>

    ![新しい Linux VM の IP でホストされている Apache の既定の Web ページを表示する Web ブラウザーのスクリーンショット。](../media/6-apache-works.png)

## <a name="one-more-thing"></a><span data-ttu-id="be9e1-206">もう 1 つの注意点</span><span class="sxs-lookup"><span data-stu-id="be9e1-206">One more thing</span></span>

<span data-ttu-id="be9e1-207">セキュリティ規則を理解するのは大変です。</span><span class="sxs-lookup"><span data-stu-id="be9e1-207">Security rules are tricky to get right.</span></span> <span data-ttu-id="be9e1-208">この新しいセキュリティ グループを適用したときにミスをしてしまいました。SSH アクセスが失われてしまったのです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-208">We made a mistake when we applied this new security group - we lost our SSH access!</span></span> <span data-ttu-id="be9e1-209">これを修正するには、サブネットに適用されているセキュリティ グループに、SSH アクセスを許可する別のルールを追加します。</span><span class="sxs-lookup"><span data-stu-id="be9e1-209">To fix this, you can add another rule to the security group applied to the subnet to allow SSH access.</span></span> <span data-ttu-id="be9e1-210">必ず、規則の受信 TCP/IP アドレスを、所有しているアドレスに限定してください。</span><span class="sxs-lookup"><span data-stu-id="be9e1-210">Make sure to restrict the inbound TCP/IP addresses for the rule to be the ones you own.</span></span>

> [!WARNING]  
> <span data-ttu-id="be9e1-211">常に、管理アクセスに使用するポートをロックダウンするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="be9e1-211">Always make sure to lock down ports used for administrative access.</span></span> <span data-ttu-id="be9e1-212">さらに優れたアプローチは、プライベート ネットワークに仮想ネットワークをリンクするための VPN を作成し、そのアドレス範囲からの RDP または SSH 要求のみを許可することです。</span><span class="sxs-lookup"><span data-stu-id="be9e1-212">An even better approach is to create a VPN to link the virtual network to your private network and only allow RDP or SSH requests from that address range.</span></span> <span data-ttu-id="be9e1-213">SSH で使用されるポートを、既定以外のポートに変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="be9e1-213">You can also change the port used by SSH to be something other than the default.</span></span> <span data-ttu-id="be9e1-214">ポートの変更は攻撃を防止するには不十分です。</span><span class="sxs-lookup"><span data-stu-id="be9e1-214">Keep in mind that changing ports is not sufficient to stop attacks.</span></span> <span data-ttu-id="be9e1-215">これによって、検出を少し困難にすることしかできません。</span><span class="sxs-lookup"><span data-stu-id="be9e1-215">It simply makes it a little harder to discover.</span></span>