<span data-ttu-id="926a4-101">サーバーはビデオ データを処理する準備ができています。後は、サーバーにビデオ ファイルをアップロードするために交通カメラが使用するポートを開くだけです。</span><span class="sxs-lookup"><span data-stu-id="926a4-101">Our server is ready to process video data; the last thing we need to do is open the ports that the traffic cameras will use to upload video files to our server.</span></span> 

## <a name="create-a-network-security-group"></a><span data-ttu-id="926a4-102">ネットワーク セキュリティ グループの作成</span><span class="sxs-lookup"><span data-stu-id="926a4-102">Create a network security group</span></span>

<span data-ttu-id="926a4-103">リモート デスクトップ アクセスが必要であることを示してあるため、Azure ではセキュリティ グループが作成されているはずです。</span><span class="sxs-lookup"><span data-stu-id="926a4-103">Azure should have created a security group for us because we indicated we wanted Remote Desktop access.</span></span> <span data-ttu-id="926a4-104">しかし、ここではプロセス全体を確認するために、新しいセキュリティ グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="926a4-104">But let's create a new security group so you can walk through the entire process.</span></span> <span data-ttu-id="926a4-105">VM の _前_ に仮想ネットワークを作成することにした場合、これは特に重要です。</span><span class="sxs-lookup"><span data-stu-id="926a4-105">This is particularly important if you decide to create your virtual network _before_ your VMs.</span></span> <span data-ttu-id="926a4-106">前述のとおり、セキュリティ グループは_省略可能_であり、必ずしもネットワークと共に作成されるわけではありません。</span><span class="sxs-lookup"><span data-stu-id="926a4-106">As mentioned earlier, security groups are _optional_ and not necessarily created with the network.</span></span>

> [!NOTE]
> <span data-ttu-id="926a4-107">これは 2 番目の VM であることが_想定_されるため、ネットワークに適用するセキュリティ グループは既に存在しますが、ここではまだ存在しないか、この VM に対するルールが異なると仮定します。</span><span class="sxs-lookup"><span data-stu-id="926a4-107">Since this is _supposed_ to be the second VM, we would already have a security group to apply to our network, but let's pretend for a moment that we don't, or that the rules are different for this VM.</span></span>

1. <span data-ttu-id="926a4-108">[Azure Portal](https://portal.azure.com?azure-portal=true) で、左隅のサイドバーにある **[リソースの作成]** ボタンをクリックして、新しいリソースの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="926a4-108">In the [Azure portal](https://portal.azure.com?azure-portal=true), click the **Create a resource** button in the left corner sidebar to start a new resource creation.</span></span>

1. <span data-ttu-id="926a4-109">フィルター ボックスに「ネットワーク セキュリティ グループ」と入力し、一覧で一致する項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="926a4-109">Type "Network security group" into the filter box and select the matching item in the list.</span></span>

1. <span data-ttu-id="926a4-110">**[リソース マネージャー]** デプロイメント モデルが選択されていることを確認し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="926a4-110">Make sure the **Resource Manager** deployment model is selected and click **Create**.</span></span>

1. <span data-ttu-id="926a4-111">セキュリティ グループの **[名前]** を指定します。</span><span class="sxs-lookup"><span data-stu-id="926a4-111">Provide a **Name** for your security group.</span></span> <span data-ttu-id="926a4-112">ここでも、名前付け規則を使用することをお勧めします。"Test Video Processor Network Security Group #2" の場合は "test-vp-nsg2" を使用します。</span><span class="sxs-lookup"><span data-stu-id="926a4-112">Again, naming conventions are a good idea here, let's use "test-vp-nsg2" for "Test Video Processor Network Security Group #2".</span></span>

1. <span data-ttu-id="926a4-113">適切な **[サブスクリプション]** を選択し、既存の **[リソース グループ]** を使用します。</span><span class="sxs-lookup"><span data-stu-id="926a4-113">Select the proper **Subscription** and use your existing **Resource group**.</span></span>

1. <span data-ttu-id="926a4-114">最後に、VM / Virtual Network と同じ**場所**に配置します。</span><span class="sxs-lookup"><span data-stu-id="926a4-114">Finally, put it into the same **Location** as the VM / Virtual Network.</span></span> <span data-ttu-id="926a4-115">これは重要なことです。場所が異なる場合、このリソースを適用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="926a4-115">This is important - you won't be able to apply this resource if it's in a different location.</span></span>

1. <span data-ttu-id="926a4-116">**[作成]** をクリックしてグループを作成します。</span><span class="sxs-lookup"><span data-stu-id="926a4-116">Click **Create** to create the group.</span></span>

## <a name="add-a-new-inbound-rule-to-our-network-security-group"></a><span data-ttu-id="926a4-117">ネットワーク セキュリティ グループに新しい受信規則を追加する</span><span class="sxs-lookup"><span data-stu-id="926a4-117">Add a new inbound rule to our Network Security Group</span></span>

<span data-ttu-id="926a4-118">デプロイメントはすぐに完了するはずです。</span><span class="sxs-lookup"><span data-stu-id="926a4-118">Deployment should complete quickly.</span></span>

1. <span data-ttu-id="926a4-119">Azure Portal で新しいセキュリティ グループのリソースを見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="926a4-119">Find the new security group resource and select it in the Azure portal.</span></span>

1. <span data-ttu-id="926a4-120">概要ページには、ネットワークをロックダウンするために作成された既定の規則がいくつか表示されます。</span><span class="sxs-lookup"><span data-stu-id="926a4-120">On the overview page, you'll find that it has some default rules created to lock down the network.</span></span>

    <span data-ttu-id="926a4-121">受信側:</span><span class="sxs-lookup"><span data-stu-id="926a4-121">On the inbound side:</span></span>

    - <span data-ttu-id="926a4-122">VNet 間の受信トラフィックはすべて許可されます。</span><span class="sxs-lookup"><span data-stu-id="926a4-122">All inbound traffic from one VNet to another is allowed.</span></span> <span data-ttu-id="926a4-123">これにより、VNet 上のリソースは相互に通信することができます。</span><span class="sxs-lookup"><span data-stu-id="926a4-123">This lets resources on the VNet talk to each other.</span></span>
    - <span data-ttu-id="926a4-124">Azure Load Balancer の "プローブ" によって、VM が有効であることを確認するように求められます</span><span class="sxs-lookup"><span data-stu-id="926a4-124">Azure Load balancer "probe" requests to ensure the VM is alive</span></span>
    - <span data-ttu-id="926a4-125">他の受信トラフィックはすべて拒否されます。</span><span class="sxs-lookup"><span data-stu-id="926a4-125">All other inbound traffic is denied.</span></span>
    <span data-ttu-id="926a4-126">送信側:</span><span class="sxs-lookup"><span data-stu-id="926a4-126">On the outbound side:</span></span>
    - <span data-ttu-id="926a4-127">VNet 上のすべてのネットワーク内のトラフィックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="926a4-127">All in-network traffic on the VNet is allowed.</span></span>
    - <span data-ttu-id="926a4-128">インターネットへの送信トラフィックはすべて許可されます。</span><span class="sxs-lookup"><span data-stu-id="926a4-128">All outbound traffic to the Internet is allowed.</span></span>
    - <span data-ttu-id="926a4-129">他の送信トラフィックはすべて拒否されます。</span><span class="sxs-lookup"><span data-stu-id="926a4-129">All other outbound traffic is denied.</span></span>

> [!NOTE]
> <span data-ttu-id="926a4-130">これらの既定の規則には優先度の高い値が設定されています。これは、これらの規則が_最後_に評価されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="926a4-130">These default rules are set with high priority values, which means that they get evaluated _last_.</span></span> <span data-ttu-id="926a4-131">これらを変更したり、削除したりすることはできませんが、優先度の低い値のトラフィックと一致するようにより具体的な規則を作成することで、_上書きする_ことが できます。</span><span class="sxs-lookup"><span data-stu-id="926a4-131">They cannot be changed or deleted, but you can _override_ them by creating more specific rules to match your traffic with a lower priority value.</span></span>

1. <span data-ttu-id="926a4-132">セキュリティ グループの **[設定]** パネルで **[受信セキュリティ規則]** セクションをクリックします。</span><span class="sxs-lookup"><span data-stu-id="926a4-132">Click the **Inbound security rules** section in the **Settings** panel for the security group.</span></span>

1. <span data-ttu-id="926a4-133">**[+ 追加]** をクリックして新しいセキュリティ規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="926a4-133">Click **+ Add** to add a new security rule.</span></span>

    ![セキュリティ規則を追加する](../media-drafts/8-add-rule.png)

    <span data-ttu-id="926a4-135">セキュリティ規則に必要な情報を入力する方法は 2 つ (基本と高度) あります。</span><span class="sxs-lookup"><span data-stu-id="926a4-135">There are two ways to enter the information necessary for a security rule: basic and advanced.</span></span> <span data-ttu-id="926a4-136">[追加] パネルの上部にあるボタンをクリックして、これらを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="926a4-136">You can switch between them by clicking the button at the top of the "add" panel.</span></span>

    ![基本と高度な規則の入力](../media-drafts/8-advanced-create-rule.png)

    <span data-ttu-id="926a4-138">高度なモードでは規則を完全にカスタマイズできますが、既知のプロトコルを構成するだけの場合は、基本モードのほうが操作は少し簡単です。</span><span class="sxs-lookup"><span data-stu-id="926a4-138">The advanced mode provides the ability to completely customize the rule, however, if you just need to configure a known protocol, the basic mode is a bit easier to work with.</span></span>

1. <span data-ttu-id="926a4-139">基本モードに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="926a4-139">Switch to the Basic mode.</span></span>

1. <span data-ttu-id="926a4-140">FTP 規則に関する情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="926a4-140">Add the information for our FTP rule.</span></span>

    - <span data-ttu-id="926a4-141">**[サービス]** を FTP に設定します。</span><span class="sxs-lookup"><span data-stu-id="926a4-141">Set the **Service** to be FTP.</span></span> <span data-ttu-id="926a4-142">これでポートの範囲が設定されます。</span><span class="sxs-lookup"><span data-stu-id="926a4-142">This will set your port range up for you.</span></span>
    - <span data-ttu-id="926a4-143">**[優先度]** を "1000" に設定します。</span><span class="sxs-lookup"><span data-stu-id="926a4-143">Set the **Priority** to "1000".</span></span> <span data-ttu-id="926a4-144">既定値の **[拒否]** 規則よりも低い数値にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="926a4-144">It has to be a lower number than the default **Deny** rule.</span></span> <span data-ttu-id="926a4-145">任意の値で範囲を開始することができますが、後で例外を作成する必要がある場合は、余裕を持たせることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="926a4-145">You can start the range at any value, but it's recommended you give yourself some space in case an exception needs to be created later.</span></span>
    - <span data-ttu-id="926a4-146">規則に名前を付けます。ここでは、"traffic-cam-ftp-upload-2" を使用します。</span><span class="sxs-lookup"><span data-stu-id="926a4-146">Give the rule a name, we'll use "traffic-cam-ftp-upload-2".</span></span>
    - <span data-ttu-id="926a4-147">規則の説明を入力します。</span><span class="sxs-lookup"><span data-stu-id="926a4-147">Give the rule a description.</span></span>

1. <span data-ttu-id="926a4-148">**高度な**モードに戻ります。</span><span class="sxs-lookup"><span data-stu-id="926a4-148">Switch back to the **Advanced** mode.</span></span> <span data-ttu-id="926a4-149">設定がまだ存在することに注目してください。</span><span class="sxs-lookup"><span data-stu-id="926a4-149">Notice that our settings are still present.</span></span> <span data-ttu-id="926a4-150">このパネルを使用して、より詳細な設定を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="926a4-150">We can use this panel to create more fine-grained settings.</span></span> <span data-ttu-id="926a4-151">たとえば、**[ソース]** を特定の IP アドレスまたはカメラに固有の IP アドレスの範囲に制限できます。</span><span class="sxs-lookup"><span data-stu-id="926a4-151">In particular, we would likely restrict the **Source** to be a specific IP address or range of IP addresses specific to the cameras.</span></span> <span data-ttu-id="926a4-152">ローカル コンピューターの現在の IP アドレスがわかっている場合は、それを試すことができます。</span><span class="sxs-lookup"><span data-stu-id="926a4-152">If you know the current IP address of your local computer, you can try that.</span></span> <span data-ttu-id="926a4-153">それ以外の場合は、設定を "任意" のままにして、規則をテストできるようにします。</span><span class="sxs-lookup"><span data-stu-id="926a4-153">Otherwise, leave the setting as "Any" so you can test the rule.</span></span>

1. <span data-ttu-id="926a4-154">**[追加]** をクリックして、規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="926a4-154">Click **Add** to create the rule.</span></span> <span data-ttu-id="926a4-155">これで受信規則の一覧が更新されます。規則が優先順になっていることに注目してください。これは規則を確認する順序です。</span><span class="sxs-lookup"><span data-stu-id="926a4-155">This will update the list of inbound rules - notice they are in priority order, which is how they will be examined.</span></span>
    
## <a name="apply-the-security-group"></a><span data-ttu-id="926a4-156">セキュリティ グループを適用する</span><span class="sxs-lookup"><span data-stu-id="926a4-156">Apply the security group</span></span>

<span data-ttu-id="926a4-157">セキュリティ グループを単一の VM を保護するためにネットワーク インターフェイスに適用するか、サブネットに適用し、そのサブネット上のリソースに適用できることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="926a4-157">Recall that we can apply the security group to a network interface to guard a single VM, or to a subnet where it would apply to any resources on that subnet.</span></span> <span data-ttu-id="926a4-158">後者のアプローチが最も一般的であるため、これを使用します。</span><span class="sxs-lookup"><span data-stu-id="926a4-158">The latter approach tends to be the most common so let's do that.</span></span> <span data-ttu-id="926a4-159">Azure でこのリソースにアクセスすることができます。その場合、仮想ネットワーク リソースを経由するか、仮想ネットワークを使用している VM 経由で間接的にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="926a4-159">We could get to this resource in Azure through either the virtual network resource or indirectly through the VM which is using the virtual network.</span></span>

1. <span data-ttu-id="926a4-160">仮想マシンの **[概要]** パネルに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="926a4-160">Switch to the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="926a4-161">**[すべてのリソース]** の下に VM があります。</span><span class="sxs-lookup"><span data-stu-id="926a4-161">You can find the VM under **All Resources**.</span></span>

1. <span data-ttu-id="926a4-162">**[設定]** セクションで **[ネットワーク]** という項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="926a4-162">Select the **Networking** item in the **Settings** section.</span></span>

    ![VM 設定のネットワーク項目](../media-drafts/8-network-settings.png)

1. <span data-ttu-id="926a4-164">ネットワーク プロパティには、**[仮想ネットワーク/サブネット]** を含む、VM に適用されるネットワークに関する情報があります。</span><span class="sxs-lookup"><span data-stu-id="926a4-164">In the networking properties, you will find information about the networking applied to the VM including the **Virtual network/subnet**.</span></span> <span data-ttu-id="926a4-165">これは、リソースにアクセスするためのクリック可能なリンクです。</span><span class="sxs-lookup"><span data-stu-id="926a4-165">This is a clickable link to get to the resource.</span></span> <span data-ttu-id="926a4-166">これをクリックして仮想ネットワークを開きます。</span><span class="sxs-lookup"><span data-stu-id="926a4-166">Click it to open the virtual network.</span></span> <span data-ttu-id="926a4-167">このリンクは、VM の **[概要]** パネル "_でも_" 使用できます。</span><span class="sxs-lookup"><span data-stu-id="926a4-167">This link is _also_ available on the **Overview** panel of the VM.</span></span> <span data-ttu-id="926a4-168">これらのいずれかをクリックすると、仮想ネットワークの **[概要]** が開きます。</span><span class="sxs-lookup"><span data-stu-id="926a4-168">Either of these will open the **Overview** of the virtual network.</span></span>

1. <span data-ttu-id="926a4-169">**[設定]** セクションで、**[サブネット]** 項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="926a4-169">In the **Settings** section, select the **Subnets** item.</span></span>

1. <span data-ttu-id="926a4-170">以前に VM とネットワークを作成したときの定義された (既定) 単一のサブネットがあるはずです。</span><span class="sxs-lookup"><span data-stu-id="926a4-170">We should have a single subnet defined (default) from when we created the VM + network earlier.</span></span> <span data-ttu-id="926a4-171">一覧の項目をクリックして詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="926a4-171">Click the item in the list to open the details.</span></span>

1. <span data-ttu-id="926a4-172">**[ネットワーク セキュリティ グループ]** エントリをクリックします。</span><span class="sxs-lookup"><span data-stu-id="926a4-172">Click the **Network security group** entry.</span></span>

1. <span data-ttu-id="926a4-173">**test-vp-nsg2** という新しいセキュリティ グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="926a4-173">Select your new security group: **test-vp-nsg2**.</span></span>

1. <span data-ttu-id="926a4-174">**[保存]** をクリックして変更内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="926a4-174">Click **Save** to save the change.</span></span> <span data-ttu-id="926a4-175">ネットワークに適用されるまで少し時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="926a4-175">It will take a minute to apply to the network.</span></span>

## <a name="verify-the-rules"></a><span data-ttu-id="926a4-176">規則を確認する</span><span class="sxs-lookup"><span data-stu-id="926a4-176">Verify the rules</span></span>

<span data-ttu-id="926a4-177">変更内容を検証しましょう。</span><span class="sxs-lookup"><span data-stu-id="926a4-177">Let's validate the change.</span></span>

1. <span data-ttu-id="926a4-178">仮想マシンの **[概要]** パネルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="926a4-178">Switch back to the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="926a4-179">**[すべてのリソース]** の下に VM があります。</span><span class="sxs-lookup"><span data-stu-id="926a4-179">You can find the VM under **All Resources**.</span></span>

1. <span data-ttu-id="926a4-180">**[設定]** セクションで **[ネットワーク]** という項目を選択します。</span><span class="sxs-lookup"><span data-stu-id="926a4-180">Select the **Networking** item in the **Settings** section.</span></span>

1. <span data-ttu-id="926a4-181">ネットワークの **[概要]** パネルには、\*\*有効なセキュリティ規則のリンクがあります。これをクリックすると、規則がどのように評価されるかがわかります。</span><span class="sxs-lookup"><span data-stu-id="926a4-181">In the **Overview** panel for the network, there is a link for \*\*Effective security rules that will quickly show you how rules are going to be evaluated.</span></span> <span data-ttu-id="926a4-182">リンクをクリックして分析を開き、FTP 規則が表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="926a4-182">Click the link to open the analysis and make sure you see your FTP rule.</span></span>

    ![ネットワークの有効なセキュリティ規則](../media-drafts/8-effective-rules.png)

1. <span data-ttu-id="926a4-184">FTP サーバー ロールをインストールした場合は、FTP エンドポイントに接続できるようになっているはずです。</span><span class="sxs-lookup"><span data-stu-id="926a4-184">If you installed the FTP server role, you should be able to connect to the FTP endpoint now.</span></span> <span data-ttu-id="926a4-185">実際にやってみましょう。</span><span class="sxs-lookup"><span data-stu-id="926a4-185">Try it out.</span></span>

## <a name="one-more-thing"></a><span data-ttu-id="926a4-186">もう 1 つ注意すべき点</span><span class="sxs-lookup"><span data-stu-id="926a4-186">One more thing</span></span>

<span data-ttu-id="926a4-187">セキュリティ規則は少し扱いにくいです。</span><span class="sxs-lookup"><span data-stu-id="926a4-187">Security rules are tricky to get right.</span></span> <span data-ttu-id="926a4-188">実際、この新しいセキュリティ グループを適用したときにミスを起こしてしまいました。リモート デスクトップ アクセスが失なわれてしまったのです。</span><span class="sxs-lookup"><span data-stu-id="926a4-188">We actually made a mistake when we applied this new security group - we lost our Remote Desktop access!</span></span> <span data-ttu-id="926a4-189">これを修正するために、RDP アクセスがサポートされるようにセキュリティ グループに別の規則を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="926a4-189">To fix this, you can add another rule to the security group to support RDP access.</span></span> <span data-ttu-id="926a4-190">必ず、規則に対する受信 TCP/IP アドレスを所有しているものに限定してください。</span><span class="sxs-lookup"><span data-stu-id="926a4-190">Make sure to restrict the inbound TCP/IP addresses for the rule to be the ones you own.</span></span>

> [!WARNING]
> <span data-ttu-id="926a4-191">常に、管理アクセスに使用するポートをロックダウンするようにしてください。</span><span class="sxs-lookup"><span data-stu-id="926a4-191">Always make sure to lock down ports used for administrative access.</span></span> <span data-ttu-id="926a4-192">さらに優れたアプローチは、プライベート ネットワークに仮想ネットワークをリンクするための VPN を作成し、そのアドレス範囲からの RDP または SSH 要求のみを許可することです。</span><span class="sxs-lookup"><span data-stu-id="926a4-192">An even better approach is to create a VPN to link the virtual network to your private network and only allow RDP or SSH requests from that address range.</span></span> <span data-ttu-id="926a4-193">RDP で使用されるポートを、既定の 3389 以外のものに変更することもできます。</span><span class="sxs-lookup"><span data-stu-id="926a4-193">You can also change the port used by RDP to be something other than the default 3389.</span></span> <span data-ttu-id="926a4-194">ポートの変更は攻撃を防止するには不十分であり、これによって、検出を少し困難にすることしかできません。</span><span class="sxs-lookup"><span data-stu-id="926a4-194">Keep in mind that changing ports is not sufficient to stop attacks, it simply makes it a little harder to discover.</span></span>