<span data-ttu-id="6de41-101">サーバーはビデオ データを処理する準備ができています。後は、サーバーにビデオ ファイルをアップロードするために交通カメラが使用するポートを開くだけです。</span><span class="sxs-lookup"><span data-stu-id="6de41-101">Our server is ready to process video data; the last thing we need to do is open the ports that the traffic cameras will use to upload video files to our server.</span></span>

## <a name="create-a-network-security-group"></a><span data-ttu-id="6de41-102">ネットワーク セキュリティ グループの作成</span><span class="sxs-lookup"><span data-stu-id="6de41-102">Create a network security group</span></span>

<span data-ttu-id="6de41-103">リモート デスクトップ アクセスが必要であることを示してあるため、Azure ではセキュリティ グループが作成されているはずです。</span><span class="sxs-lookup"><span data-stu-id="6de41-103">Azure should have created a security group for us because we indicated we wanted Remote Desktop access.</span></span> <span data-ttu-id="6de41-104">しかし、ここではプロセス全体を確認するために、新しいセキュリティ グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="6de41-104">But let's create a new security group so you can walk through the entire process.</span></span> <span data-ttu-id="6de41-105">VM の _前_ に仮想ネットワークを作成することにした場合、これは特に重要です。</span><span class="sxs-lookup"><span data-stu-id="6de41-105">This is particularly important if you decide to create your virtual network _before_ your VMs.</span></span> <span data-ttu-id="6de41-106">前述のとおり、セキュリティ グループは_省略可能_であり、必ずしもネットワークと共に作成されるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="6de41-106">As mentioned earlier, security groups are _optional_ and not necessarily created with the network.</span></span>

> [!NOTE]
> <span data-ttu-id="6de41-107">これは 2 番目の VM であることが_想定_されるため、ネットワークに適用するセキュリティ グループは既に存在しますが、ここではまだ存在しないか、この VM に対するルールが異なると仮定します。</span><span class="sxs-lookup"><span data-stu-id="6de41-107">Since this is _supposed_ to be the second VM, we would already have a security group to apply to our network, but let's pretend for a moment that we don't, or that the rules are different for this VM.</span></span>

1. <span data-ttu-id="6de41-108">[Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) で、左隅のサイドバーにある **[リソースの作成]** ボタンをクリックして、新しいリソースの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="6de41-108">In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), click the **Create a resource** button in the left corner sidebar to start a new resource creation.</span></span>

1. <span data-ttu-id="6de41-109">フィルター ボックスに「ネットワーク セキュリティ グループ」と入力し、一覧で一致する項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="6de41-109">Type "Network security group" into the filter box and select the matching item in the list.</span></span>

1. <span data-ttu-id="6de41-110">**[リソース マネージャー]** デプロイメント モデルが選択されていることを確認し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6de41-110">Make sure the **Resource Manager** deployment model is selected and click **Create**.</span></span>

1. <span data-ttu-id="6de41-111">セキュリティ グループの **[名前]** を指定します。</span><span class="sxs-lookup"><span data-stu-id="6de41-111">Provide a **Name** for your security group.</span></span> <span data-ttu-id="6de41-112">ここでも、名前付け規則を使用することをお勧めします。"Test Video Processor Network Security Group #2" の場合は "test-vp-nsg2" を使用します。</span><span class="sxs-lookup"><span data-stu-id="6de41-112">Again, naming conventions are a good idea here, let's use "test-vp-nsg2" for "Test Video Processor Network Security Group #2".</span></span>

1. <span data-ttu-id="6de41-113">適切な**サブスクリプション**を選択し、既存の**リソース グループ**である "<rgn>[サンドボックス リソース グループ名]</rgn>" を使用します。</span><span class="sxs-lookup"><span data-stu-id="6de41-113">Select the proper **Subscription** and use your existing **Resource group**, "<rgn>[sandbox resource group name]</rgn>".</span></span>

1. <span data-ttu-id="6de41-114">最後に、VM/仮想ネットワークと同じ**場所**に配置します。</span><span class="sxs-lookup"><span data-stu-id="6de41-114">Finally, put it into the same **Location** as the VM / Virtual Network.</span></span> <span data-ttu-id="6de41-115">これは重要なことです。場所が異なる場合、このリソースを適用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="6de41-115">This is important - you won't be able to apply this resource if it's in a different location.</span></span>

1. <span data-ttu-id="6de41-116">**[作成]** をクリックしてグループを作成します。</span><span class="sxs-lookup"><span data-stu-id="6de41-116">Click **Create** to create the group.</span></span>

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a><span data-ttu-id="6de41-117">ネットワーク セキュリティ グループに新しい受信規則を追加する</span><span class="sxs-lookup"><span data-stu-id="6de41-117">Add a new inbound rule to our Network Security Group</span></span>

<span data-ttu-id="6de41-118">デプロイメントはすぐに完了するはずです。</span><span class="sxs-lookup"><span data-stu-id="6de41-118">Deployment should complete quickly.</span></span>

1. <span data-ttu-id="6de41-119">Azure Portal で新しいセキュリティ グループのリソースを見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="6de41-119">Find the new security group resource and select it in the Azure portal.</span></span>

1. <span data-ttu-id="6de41-120">概要ページには、ネットワークをロックダウンするために作成された既定の規則がいくつか表示されます。</span><span class="sxs-lookup"><span data-stu-id="6de41-120">On the overview page, you'll find that it has some default rules created to lock down the network.</span></span>

    <span data-ttu-id="6de41-121">受信側:</span><span class="sxs-lookup"><span data-stu-id="6de41-121">On the inbound side:</span></span>

    - <span data-ttu-id="6de41-122">VNet 間の受信トラフィックはすべて許可されます。</span><span class="sxs-lookup"><span data-stu-id="6de41-122">All inbound traffic from one VNet to another is allowed.</span></span> <span data-ttu-id="6de41-123">これにより、VNet 上のリソースは相互に通信することができます。</span><span class="sxs-lookup"><span data-stu-id="6de41-123">This lets resources on the VNet talk to each other.</span></span>
    - <span data-ttu-id="6de41-124">Azure Load Balancer の "プローブ" によって、VM が有効であることを確認するように求められます</span><span class="sxs-lookup"><span data-stu-id="6de41-124">Azure Load balancer "probe" requests to ensure the VM is alive</span></span>
    - <span data-ttu-id="6de41-125">他の受信トラフィックはすべて拒否されます。</span><span class="sxs-lookup"><span data-stu-id="6de41-125">All other inbound traffic is denied.</span></span>

    <span data-ttu-id="6de41-126">送信側:</span><span class="sxs-lookup"><span data-stu-id="6de41-126">On the outbound side:</span></span>
    - <span data-ttu-id="6de41-127">VNet 上のすべてのネットワーク内のトラフィックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="6de41-127">All in-network traffic on the VNet is allowed.</span></span>
    - <span data-ttu-id="6de41-128">インターネットへの送信トラフィックはすべて許可されます。</span><span class="sxs-lookup"><span data-stu-id="6de41-128">All outbound traffic to the Internet is allowed.</span></span>
    - <span data-ttu-id="6de41-129">他の送信トラフィックはすべて拒否されます。</span><span class="sxs-lookup"><span data-stu-id="6de41-129">All other outbound traffic is denied.</span></span>

> [!NOTE]
> <span data-ttu-id="6de41-130">これらの既定の規則には優先度の高い値が設定されています。これは、これらの規則が_最後_に評価されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="6de41-130">These default rules are set with high priority values, which means that they get evaluated _last_.</span></span> <span data-ttu-id="6de41-131">これらを変更したり、削除したりすることはできませんが、優先度の低い値のトラフィックと一致するようにより具体的な規則を作成することで、_上書きする_ことが できます。</span><span class="sxs-lookup"><span data-stu-id="6de41-131">They cannot be changed or deleted, but you can _override_ them by creating more specific rules to match your traffic with a lower priority value.</span></span>

1. <span data-ttu-id="6de41-132">セキュリティ グループの **[設定]** パネルで **[受信セキュリティ規則]** セクションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6de41-132">Click the **Inbound security rules** section in the **Settings** panel for the security group.</span></span>

1. <span data-ttu-id="6de41-133">**[+ 追加]** をクリックして新しいセキュリティ規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="6de41-133">Click **+ Add** to add a new security rule.</span></span>

    !["セキュリティ規則を追加する" ダイアログを示すスクリーンショット。](../media/8-add-rule.png)

    <span data-ttu-id="6de41-135">セキュリティ規則に必要な情報を入力する方法には、基本と高度の 2 つがあります。</span><span class="sxs-lookup"><span data-stu-id="6de41-135">There are two ways to enter the information necessary for a security rule: basic and advanced.</span></span> <span data-ttu-id="6de41-136">[追加] パネルの上部にあるボタンをクリックして、これらを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="6de41-136">You can switch between them by clicking the button at the top of the "add" panel.</span></span>

    ![Azure portal の 2 つのスクリーンショット。基本と詳細のルール入力を切り替える様子を確認できます。トグル ボタンの 2 つの状態を結ぶ矢印が強調表示されています。](../media/8-advanced-create-rule.png)

    <span data-ttu-id="6de41-138">高度なモードでは規則を完全にカスタマイズできますが、既知のプロトコルを構成するだけの場合は、基本モードの方が操作は少し簡単です。</span><span class="sxs-lookup"><span data-stu-id="6de41-138">The advanced mode provides the ability to completely customize the rule, however, if you just need to configure a known protocol, the basic mode is a bit easier to work with.</span></span>

1. <span data-ttu-id="6de41-139">パネルは **[高度]** モードで開始されます。上部の **[基本]** ボタンをクリックして、入力するオプションがより少ない基本モードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="6de41-139">The panel starts in **Advanced** mode, click the **Basic** button at the top to switch to basic mode which has less options to fill out.</span></span>

1. <span data-ttu-id="6de41-140">FTP 規則に関する情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="6de41-140">Add the information for our FTP rule.</span></span>

    - <span data-ttu-id="6de41-141">**[サービス]** を FTP に設定します。</span><span class="sxs-lookup"><span data-stu-id="6de41-141">Set the **Service** to be FTP.</span></span> <span data-ttu-id="6de41-142">これでポートの範囲が設定されます。</span><span class="sxs-lookup"><span data-stu-id="6de41-142">This will set your port range up for you.</span></span>
    - <span data-ttu-id="6de41-143">**[優先度]** を "1000" に設定します。</span><span class="sxs-lookup"><span data-stu-id="6de41-143">Set the **Priority** to "1000".</span></span> <span data-ttu-id="6de41-144">既定値の **[拒否]** 規則よりも低い数値にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6de41-144">It has to be a lower number than the default **Deny** rule.</span></span> <span data-ttu-id="6de41-145">任意の値で範囲を開始することができますが、後で例外を作成する必要がある場合は、余裕を持たせることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="6de41-145">You can start the range at any value, but it's recommended you give yourself some space in case an exception needs to be created later.</span></span>
    - <span data-ttu-id="6de41-146">規則に名前を付けます。ここでは、"traffic-cam-ftp-upload-2" を使用します。</span><span class="sxs-lookup"><span data-stu-id="6de41-146">Give the rule a name, we'll use "traffic-cam-ftp-upload-2".</span></span>
    - <span data-ttu-id="6de41-147">規則の説明を入力します。</span><span class="sxs-lookup"><span data-stu-id="6de41-147">Give the rule a description.</span></span>

1. <span data-ttu-id="6de41-148">上部の **[高度]** ボタンをクリックし、**[高度]** モードに戻ります。</span><span class="sxs-lookup"><span data-stu-id="6de41-148">Switch back to the **Advanced** mode by clicking the **Advanced** button at the top.</span></span> <span data-ttu-id="6de41-149">設定がまだ存在することに注目してください。</span><span class="sxs-lookup"><span data-stu-id="6de41-149">Notice that our settings are still present.</span></span> <span data-ttu-id="6de41-150">このパネルを使用して、より詳細な設定を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="6de41-150">We can use this panel to create more fine-grained settings.</span></span> <span data-ttu-id="6de41-151">たとえば、**[ソース]** を特定の IP アドレスまたはカメラに固有の IP アドレスの範囲に制限できます。</span><span class="sxs-lookup"><span data-stu-id="6de41-151">In particular, we would likely restrict the **Source** to be a specific IP address or range of IP addresses specific to the cameras.</span></span> <span data-ttu-id="6de41-152">ローカル コンピューターの現在の IP アドレスがわかっている場合は、それを試すことができます。</span><span class="sxs-lookup"><span data-stu-id="6de41-152">If you know the current IP address of your local computer, you can try that.</span></span> <span data-ttu-id="6de41-153">それ以外の場合は、設定を "任意" のままにして、規則をテストできるようにします。</span><span class="sxs-lookup"><span data-stu-id="6de41-153">Otherwise, leave the setting as "Any" so you can test the rule.</span></span>

1. <span data-ttu-id="6de41-154">**[追加]** をクリックして、規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="6de41-154">Click **Add** to create the rule.</span></span> <span data-ttu-id="6de41-155">これで受信規則の一覧が更新されます。規則が優先順になっていることに注目してください。これは規則を確認する順序です。</span><span class="sxs-lookup"><span data-stu-id="6de41-155">This will update the list of inbound rules - notice they are in priority order, which is how they will be examined.</span></span>

## <a name="apply-the-security-group"></a><span data-ttu-id="6de41-156">セキュリティ グループを適用する</span><span class="sxs-lookup"><span data-stu-id="6de41-156">Apply the security group</span></span>

<span data-ttu-id="6de41-157">セキュリティ グループを単一の VM を保護するためにネットワーク インターフェイスに適用するか、サブネットに適用し、そのサブネット上のリソースに適用できることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="6de41-157">Recall that we can apply the security group to a network interface to guard a single VM, or to a subnet where it would apply to any resources on that subnet.</span></span> <span data-ttu-id="6de41-158">後者のアプローチが最も一般的であるため、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="6de41-158">The latter approach tends to be the most common so let's do that.</span></span> <span data-ttu-id="6de41-159">Azure でこのリソースにアクセスすることができます。その場合、仮想ネットワーク リソースを経由するか、仮想ネットワークを使用している VM 経由で間接的にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="6de41-159">We could get to this resource in Azure through either the virtual network resource or indirectly through the VM, which is using the virtual network.</span></span>

1. <span data-ttu-id="6de41-160">仮想マシンの **[概要]** パネルに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="6de41-160">Switch to the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="6de41-161">**[すべてのリソース]** の下に VM があります。</span><span class="sxs-lookup"><span data-stu-id="6de41-161">You can find the VM under **All Resources**.</span></span>

1. <span data-ttu-id="6de41-162">**[設定]** セクションで **[ネットワーク]** という項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="6de41-162">Select the **Networking** item in the **Settings** section.</span></span>

    ![VM 設定のネットワーク項目の詳細を示すスクリーンショット。ネットワーク セキュリティ グループが適用されています。](../media/8-network-settings.png)

1. <span data-ttu-id="6de41-164">ネットワーク プロパティには、**[仮想ネットワーク/サブネット]** を含む、VM に適用されるネットワークに関する情報があります。</span><span class="sxs-lookup"><span data-stu-id="6de41-164">In the networking properties, you will find information about the networking applied to the VM including the **Virtual network/subnet**.</span></span> <span data-ttu-id="6de41-165">これは、リソースにアクセスするためのクリック可能なリンクです。</span><span class="sxs-lookup"><span data-stu-id="6de41-165">This is a clickable link to get to the resource.</span></span> <span data-ttu-id="6de41-166">これをクリックして仮想ネットワークを開きます。</span><span class="sxs-lookup"><span data-stu-id="6de41-166">Click it to open the virtual network.</span></span> <span data-ttu-id="6de41-167">このリンクは、VM の **[概要]** パネル "_でも_" 使用できます。</span><span class="sxs-lookup"><span data-stu-id="6de41-167">This link is _also_ available on the **Overview** panel of the VM.</span></span> <span data-ttu-id="6de41-168">これらのいずれかをクリックすると、仮想ネットワークの **[概要]** が開きます。</span><span class="sxs-lookup"><span data-stu-id="6de41-168">Either of these will open the **Overview** of the virtual network.</span></span>

1. <span data-ttu-id="6de41-169">**[設定]** セクションで、**[サブネット]** 項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="6de41-169">In the **Settings** section, select the **Subnets** item.</span></span>

1. <span data-ttu-id="6de41-170">以前に VM とネットワークを作成したときの定義された (既定) 単一のサブネットがあるはずです。</span><span class="sxs-lookup"><span data-stu-id="6de41-170">We should have a single subnet defined (default) from when we created the VM + network earlier.</span></span> <span data-ttu-id="6de41-171">一覧の項目をクリックして詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="6de41-171">Click the item in the list to open the details.</span></span>

1. <span data-ttu-id="6de41-172">**[ネットワーク セキュリティ グループ]** エントリをクリックします。</span><span class="sxs-lookup"><span data-stu-id="6de41-172">Click the **Network security group** entry.</span></span>

1. <span data-ttu-id="6de41-173">**test-vp-nsg2** という新しいセキュリティ グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="6de41-173">Select your new security group: **test-vp-nsg2**.</span></span>

1. <span data-ttu-id="6de41-174">**[保存]** をクリックして変更内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="6de41-174">Click **Save** to save the change.</span></span> <span data-ttu-id="6de41-175">ネットワークに適用されるまで少し時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="6de41-175">It will take a minute to apply to the network.</span></span>

## <a name="verify-the-rules"></a><span data-ttu-id="6de41-176">規則を確認する</span><span class="sxs-lookup"><span data-stu-id="6de41-176">Verify the rules</span></span>

<span data-ttu-id="6de41-177">変更内容を検証しましょう。</span><span class="sxs-lookup"><span data-stu-id="6de41-177">Let's validate the change.</span></span>

1. <span data-ttu-id="6de41-178">仮想マシンの **[概要]** パネルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="6de41-178">Switch back to the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="6de41-179">**[すべてのリソース]** の下に VM があります。</span><span class="sxs-lookup"><span data-stu-id="6de41-179">You can find the VM under **All Resources**.</span></span>

1. <span data-ttu-id="6de41-180">**[設定]** セクションで **[ネットワーク]** という項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="6de41-180">Select the **Networking** item in the **Settings** section.</span></span>

1. <span data-ttu-id="6de41-181">ネットワークの **[概要]** パネルには、**[有効なセキュリティ規則]** のリンクがあります。これをクリックすると、規則がどのように評価されるかがすぐにわかります。</span><span class="sxs-lookup"><span data-stu-id="6de41-181">In the **Overview** panel for the network, there is a link for **Effective security rules** that will quickly show you how rules are going to be evaluated.</span></span> <span data-ttu-id="6de41-182">リンクをクリックして分析を開き、FTP 規則が表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6de41-182">Click the link to open the analysis and make sure you see your FTP rule.</span></span>

    ![ネットワークの有効なセキュリティ規則を示すスクリーンショット。](../media/8-effective-rules.png)

1. <span data-ttu-id="6de41-184">この規則では、FTP サーバーに接続できます。FTP worker ロールを追加し、フォルダーを構成した場合、FTP クライアントを使用してサーバーに接続できます。</span><span class="sxs-lookup"><span data-stu-id="6de41-184">This rule would let you connect to an FTP server; if we had added the FTP worker role and configured folders, you would be able to use an FTP client to connect to the server.</span></span>

## <a name="one-more-thing"></a><span data-ttu-id="6de41-185">もう 1 つ注意すべき点</span><span class="sxs-lookup"><span data-stu-id="6de41-185">One more thing</span></span>

<span data-ttu-id="6de41-186">セキュリティ規則は少し扱いにくいです。</span><span class="sxs-lookup"><span data-stu-id="6de41-186">Security rules are tricky to get right.</span></span> <span data-ttu-id="6de41-187">実際、この新しいセキュリティ グループを適用したときにミスを起こしてしまいました。リモート デスクトップ アクセスが失なわれてしまったのです。</span><span class="sxs-lookup"><span data-stu-id="6de41-187">We actually made a mistake when we applied this new security group - we lost our Remote Desktop access!</span></span> <span data-ttu-id="6de41-188">これを修正するために、RDP アクセスがサポートされるようにセキュリティ グループに別の規則を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="6de41-188">To fix this, you can add another rule to the security group to support RDP access.</span></span> <span data-ttu-id="6de41-189">必ず、規則に対する受信 TCP/IP アドレスを所有しているものに限定してください。</span><span class="sxs-lookup"><span data-stu-id="6de41-189">Make sure to restrict the inbound TCP/IP addresses for the rule to be the ones you own.</span></span>

> [!WARNING]
> <span data-ttu-id="6de41-190">常に、管理アクセスに使用するポートをロックダウンするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="6de41-190">Always make sure to lock down ports used for administrative access.</span></span> <span data-ttu-id="6de41-191">さらに優れたアプローチは、プライベート ネットワークに仮想ネットワークをリンクするための VPN を作成し、そのアドレス範囲からの RDP または SSH 要求のみを許可することです。</span><span class="sxs-lookup"><span data-stu-id="6de41-191">An even better approach is to create a VPN to link the virtual network to your private network and only allow RDP or SSH requests from that address range.</span></span> <span data-ttu-id="6de41-192">RDP で使用されるポートを、既定の 3389 以外のものに変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="6de41-192">You can also change the port used by RDP to be something other than the default 3389.</span></span> <span data-ttu-id="6de41-193">ポートの変更は攻撃を防止するには不十分であり、これによって、検出を少し困難にすることしかできません。</span><span class="sxs-lookup"><span data-stu-id="6de41-193">Keep in mind that changing ports is not sufficient to stop attacks, it simply makes it a little harder to discover.</span></span>