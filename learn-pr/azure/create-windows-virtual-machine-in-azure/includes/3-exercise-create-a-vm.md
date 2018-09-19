<span data-ttu-id="f2739-101">前に説明したように、会社は Windows VM 上でビデオ コンテンツを処理しています。</span><span class="sxs-lookup"><span data-stu-id="f2739-101">Recall that our company processes video content on Windows VMs.</span></span> <span data-ttu-id="f2739-102">ある都市と交通カメラの処理契約を新たに結びましたが、そのカメラはこれまでに扱ったことがないモデルです。</span><span class="sxs-lookup"><span data-stu-id="f2739-102">A new city has contracted us to process their traffic cameras, but it's a model we've not worked with before.</span></span> <span data-ttu-id="f2739-103">画像の処理と分析を始めるために、新しい Windows VM を作成し、専用コーデックをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2739-103">We need to create a new Windows VM and install some proprietary codecs so we can begin processing and analyzing their images.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-new-windows-virtual-machine"></a><span data-ttu-id="f2739-104">新しい Windows 仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="f2739-104">Create a new Windows virtual machine</span></span>

<span data-ttu-id="f2739-105">Windows VM の作成は、Azure portal、Azure CLI、または Azure PowerShell で行うことができます。</span><span class="sxs-lookup"><span data-stu-id="f2739-105">We can create Windows VMs with the Azure portal, Azure CLI, or Azure PowerShell.</span></span> <span data-ttu-id="f2739-106">最も簡単な方法は、ポータルを使用することです。そうすれば、作成の間に、必要な情報が順番に示され、ヒントと有用なメッセージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-106">The easiest approach is to the portal because it walks you through the required information and provides hints and helpful messages during the creation.</span></span>

1. <span data-ttu-id="f2739-107">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="f2739-107">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="f2739-108">Azure portal の左上隅にある **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2739-108">Click **Create a resource** in the upper left corner of the Azure portal.</span></span>

1. <span data-ttu-id="f2739-109">検索ボックスに「**Windows Server 2016 Datacenter**」と入力し、表示された一覧の中から同じタイトルのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2739-109">In the search box, enter  **Windows Server 2016 Datacenter**  and then click on the link with the same title in the presented list.</span></span>

1. <span data-ttu-id="f2739-110">**[作成]** ボタンをクリックして、VM の構成を始めます。</span><span class="sxs-lookup"><span data-stu-id="f2739-110">Click the **Create** button to start configuring the VM.</span></span>

## <a name="configure-the-vm-settings"></a><span data-ttu-id="f2739-111">VM 設定を構成する</span><span class="sxs-lookup"><span data-stu-id="f2739-111">Configure the VM settings</span></span>

<span data-ttu-id="f2739-112">ポータルでの VM 作成エクスペリエンスは "ウィザード" の形式で提示され、VM のすべての構成領域が順番に示されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-112">The VM creation experience in the portal is presented in a "wizard" format to walk you through all the configuration areas for the VM.</span></span> <span data-ttu-id="f2739-113">[次へ] ボタンをクリックすると、次の構成可能なセクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="f2739-113">Clicking the "Next" button will take you to the next configurable section.</span></span> <span data-ttu-id="f2739-114">ただし、上部に並んでいるタブで各セクションが示されており、自由にセクション間を移動できます。</span><span class="sxs-lookup"><span data-stu-id="f2739-114">However, you can move between the sections at will with the tabs running across the top that identify each section.</span></span>

![Azure portal で仮想マシンを作成する様子を示すスクリーンショット。](../media/3-azure-portal-create-vm.png)

<span data-ttu-id="f2739-116">必須のオプション (赤い星で示されたもの) をすべて入力したら、ウィザードの残りの手順をスキップし、下部にある **[確認および作成]** ボタンをクリックして VM の作成を始めることができます。</span><span class="sxs-lookup"><span data-stu-id="f2739-116">Once you fill in all the required options (identified with red stars), you can skip the remainder of the wizard experience and start creating the VM through the **Review + Create** button at the bottom.</span></span>

<span data-ttu-id="f2739-117">最初は **[基本]** セクションです。</span><span class="sxs-lookup"><span data-stu-id="f2739-117">We'll start with the **Basics** section.</span></span>

### <a name="configure-basic-vm-settings"></a><span data-ttu-id="f2739-118">VM の基本設定を構成する</span><span class="sxs-lookup"><span data-stu-id="f2739-118">Configure basic VM settings</span></span>

1. <span data-ttu-id="f2739-119">VM 時間の請求対象である **[サブスクリプション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2739-119">Select the **Subscription** that should be billed for VM hours.</span></span>

1. <span data-ttu-id="f2739-120">**[リソース グループ]** で **[既存のものを使用]** を選択し、<rgn>[サンドボックス リソース グループ名]</rgn> を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2739-120">For **Resource group**, select **Use existing** and choose <rgn>[Sandbox resource group name]</rgn>.</span></span>

    > [!NOTE]  
    > <span data-ttu-id="f2739-121">設定を変更してフィールドから移動すると、各値が自動的に検証されて、問題がない場合は横に緑色のチェック マークが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-121">As you change settings and tab out of each field, Azure will validate each value automatically and place a green check mark next to it when it's good.</span></span> <span data-ttu-id="f2739-122">エラー インジケーターをポイントすると、検出された問題についての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-122">You can hover over error indicators to get more information on issues it discovers.</span></span>
    
#### <a name="select-a-location"></a><span data-ttu-id="f2739-123">場所を選択する</span><span class="sxs-lookup"><span data-stu-id="f2739-123">Select a location</span></span>

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="f2739-124">**[インスタンスの詳細]** セクションで、VM の名前を入力します (たとえば、Test Video Processor VM #2 を表す "**test-vp-vm2**")。</span><span class="sxs-lookup"><span data-stu-id="f2739-124">In the **INSTANCE DETAILS** section, enter a name for your VM, such as **test-vp-vm2** (for Test Video Processor VM #2).</span></span>
    - <span data-ttu-id="f2739-125">リソースの目的がすぐにわかるように、リソース名を標準化するのがベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="f2739-125">It's best practice to standardize your resource names so you can easily identify their purpose.</span></span> <span data-ttu-id="f2739-126">Windows VM の名前には制限があります。2 文字から 15 文字までの範囲で、数字、文字、ダッシュで構成されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2739-126">Windows VM names are a bit limited - they must be between 2 and 15 characters and be comprised of numbers, letters, and dashes.</span></span>

1. <span data-ttu-id="f2739-127">近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="f2739-127">Select a region close to you.</span></span> <span data-ttu-id="f2739-128">上記の一覧にある利用可能な場所から必ず選択してください。</span><span class="sxs-lookup"><span data-stu-id="f2739-128">Make sure to select one from the available locations listed above.</span></span>

1. <span data-ttu-id="f2739-129">**[可用性オプション]** は [なし] のままにします。</span><span class="sxs-lookup"><span data-stu-id="f2739-129">Leave **Availability options** as "None".</span></span> <span data-ttu-id="f2739-130">このオプションは、複数の VM をグループ化して計画的または計画外のメンテナンス イベントや停止に対処することで、VM を高可用性にするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-130">This option is used to ensure the VM is highly available by grouping multiple VMs together a set to deal with planned or unplanned maintenance events or outages.</span></span>

1. <span data-ttu-id="f2739-131">イメージが "Windows Server 2016 Datacenter" に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f2739-131">Ensure the image is set to "Windows Server 2016 Datacenter".</span></span> <span data-ttu-id="f2739-132">ドロップダウン リストを開いて、使用可能なすべてのオプションを確認できます。</span><span class="sxs-lookup"><span data-stu-id="f2739-132">You can open the drop-down list to see all the options available.</span></span>

1. <span data-ttu-id="f2739-133">**[サイズ]** フィールドは直接編集することはできず、DS1 の既定のサイズが設定されています。</span><span class="sxs-lookup"><span data-stu-id="f2739-133">The **Size** field is not directly editable and has a DS1 default size.</span></span> <span data-ttu-id="f2739-134">他の VM サイズを確認するには、**[Change size]\(サイズの変更\)** のリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2739-134">Click the **Change size** link to explore other VM sizes.</span></span> <span data-ttu-id="f2739-135">結果のダイアログでは、CPU の数、名前、ディスクの種類に基づいてフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="f2739-135">The resulting dialog allows you to filter based on # of CPUs, Name, and Disk Type.</span></span> <span data-ttu-id="f2739-136">終わったら、"Standard DS1 v2" (通常は既定値) を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2739-136">Select "Standard DS1 v2" (normally the default) when you are done.</span></span> <span data-ttu-id="f2739-137">これにより、VM に 1 個の CPU と 3.5 GB のメモリが設定されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-137">That will give the VM 1 CPU and 3.5 GB of memory.</span></span>

    > [!TIP]
    > <span data-ttu-id="f2739-138">ビューを左側にスライドして新しいウィンドウが右に開いたときの VM 設定に戻し、ウィンドウをスライドしてそれを表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="f2739-138">You can also just slide the view to the left to get back to the VM settings as it opened a new window off to the right and slid the window over to view it.</span></span>

1. <span data-ttu-id="f2739-139">**[管理者アカウント]** セクションの **[ユーザー名]** フィールドに、VM へのサインインに使用するユーザー名に設定します。</span><span class="sxs-lookup"><span data-stu-id="f2739-139">In the **ADMINISTRATOR ACCOUNT** section, set the **Username** field to a username you will use to sign in to the VM.</span></span>

1. <span data-ttu-id="f2739-140">**[パスワード]** フィールドには、少なくとも 12 文字の長さのパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="f2739-140">In the **Password** field, enter a password that's at least 12 characters long.</span></span> <span data-ttu-id="f2739-141">このとき、1 個の小文字、1 個の大文字、1 個の数字、'\'' または '-' 以外の 1 個の特殊文字のうち、3 種類の文字を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2739-141">It must have three of the following: one lower case character, one uppercase character, one number, and one special character that is not '\' or '-'.</span></span> <span data-ttu-id="f2739-142">覚えやすいものにするか、書き留めておいてください。後で必要になります。</span><span class="sxs-lookup"><span data-stu-id="f2739-142">Use something you will remember or write it down, you will need it later.</span></span>

1. <span data-ttu-id="f2739-143">**パスワード**を確認入力します。</span><span class="sxs-lookup"><span data-stu-id="f2739-143">Confirm the **password**.</span></span>

1. <span data-ttu-id="f2739-144">**[受信ポートの規則]** セクションで一覧を開き、_[Allow selected ports]\(選択したポートを許可する\)_ を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2739-144">In the **INBOUND PORT RULES** section, open the list and choose _Allow selected ports_.</span></span> <span data-ttu-id="f2739-145">これは Windows VM なので、RDP を使用してデスクトップにアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="f2739-145">Since this is a Windows VM, we want to be able to access the desktop using RDP.</span></span> <span data-ttu-id="f2739-146">必要に応じて一覧をスクロールし、RDP (3389) を見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="f2739-146">Scroll the list if necessary until you find RDP (3389) and select it.</span></span> <span data-ttu-id="f2739-147">UI の注意事項が示すとおり、VM を作成した後でネットワーク ポートを調整することもできます。</span><span class="sxs-lookup"><span data-stu-id="f2739-147">As the note in the UI indicates, we can also adjust the network ports after we create the VM.</span></span>

    ![Windows VM で RDP アクセスのポートを開くためのドロップダウンを示すスクリーンショット。](../media/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a><span data-ttu-id="f2739-149">VM 用にディスクを構成する</span><span class="sxs-lookup"><span data-stu-id="f2739-149">Configure Disks for the VM</span></span>

1. <span data-ttu-id="f2739-150">**[次へ]** をクリックし、[ディスク] セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="f2739-150">Click **Next** to move to the Disks section.</span></span>

    ![VM のディスクの構成セクションを示すスクリーンショット。](../media/3-configure-disks.png)

1. <span data-ttu-id="f2739-152">**[OS ディスクの種類]** で [Premium SSD] を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2739-152">Choose "Premium SSD" for the **OS disk type**.</span></span>

1. <span data-ttu-id="f2739-153">ストレージ アカウントを使用せずに済むように、マネージド ディスクを使用します。</span><span class="sxs-lookup"><span data-stu-id="f2739-153">Use managed disks so we don't have to work with storage accounts.</span></span> <span data-ttu-id="f2739-154">GUI でスイッチを切り替えて、Azure で必要な情報の違いを確認できます。</span><span class="sxs-lookup"><span data-stu-id="f2739-154">You can flip the switch in the GUI to see the difference in information that Azure needs if you like.</span></span>

### <a name="create-a-data-disk"></a><span data-ttu-id="f2739-155">データ ディスクを作成する</span><span class="sxs-lookup"><span data-stu-id="f2739-155">Create a data disk</span></span>

<span data-ttu-id="f2739-156">OS ディスク (C:) と一時ディスク (D:) を使用することを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="f2739-156">Recall we will get an OS disk (C:) and Temporary disk (D:).</span></span> <span data-ttu-id="f2739-157">データ ディスクも追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f2739-157">Let's add a data disk as well.</span></span>

1. <span data-ttu-id="f2739-158">**[データ ディスク]** セクションで **[Create and attach a new disk]\(新しいディスクの作成とアタッチ\)** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2739-158">Click the **Create and attach a new disk** link in the **DATA DISKS** section.</span></span>

    ![ポータルの、新しい VM ディスクを作成するためのダイアログを示すスクリーンショット。](../media/3-add-data-disk.png)

1. <span data-ttu-id="f2739-160">すべての既定値を使用できます (Premium SSD、1023 GB、および "なし" (空のディスク))。ただし、ここではスナップショットまたはストレージ BLOB を使用して VHD を作成することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f2739-160">You can take all the defaults: Premium SSD, 1023 GB, and None (empty disk); although notice that here is where we could use a snapshot, or Storage Blob to create a VHD.</span></span>

1. <span data-ttu-id="f2739-161">**[OK]** をクリックしてディスクを作成し、**[データ ディスク]** セクションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="f2739-161">Click **OK** to create the disk and go back to the **DATA DISKS** section.</span></span>

1. <span data-ttu-id="f2739-162">最初の行に新しいディスクが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="f2739-162">There should now be a new disk in the first row.</span></span>

    ![VM で新しく追加されたディスクを示すスクリーンショット。](../media/3-new-disk.png)

## <a name="configure-the-network"></a><span data-ttu-id="f2739-164">ネットワークを構成する</span><span class="sxs-lookup"><span data-stu-id="f2739-164">Configure the Network</span></span>

1. <span data-ttu-id="f2739-165">**[次へ]** をクリックして、[ネットワーク] セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="f2739-165">Click **Next** to move to the Networking section.</span></span>

1. <span data-ttu-id="f2739-166">既に他のコンポーネントがある運用システムでは、"_既存の_" 仮想ネットワークを利用します。</span><span class="sxs-lookup"><span data-stu-id="f2739-166">In a production system where we already have other components, we'd want to utilize an _existing_ virtual network.</span></span> <span data-ttu-id="f2739-167">そうすれば、VM はソリューション内の他のクラウド サービスと通信できます。</span><span class="sxs-lookup"><span data-stu-id="f2739-167">That way our VM can communicate with the other cloud services in our solution.</span></span> <span data-ttu-id="f2739-168">この場所でまだ定義されていない場合は、ここで作成して構成できます。</span><span class="sxs-lookup"><span data-stu-id="f2739-168">If there isn't one defined in this location yet, we can create it here and configure the:</span></span>
    - <span data-ttu-id="f2739-169">**[アドレス空間]**: このネットワークに使用できる IPV4 空間全域。</span><span class="sxs-lookup"><span data-stu-id="f2739-169">**Address space**: the overall IPV4 space available to this network.</span></span>
    - <span data-ttu-id="f2739-170">**[Subnet range]\(サブネット範囲\)**: アドレス空間を分割する最初のサブネット。定義されているアドレス空間内に収まる必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2739-170">**Subnet range**: the first subnet to subdivide the address space - it must fit within the defined address space.</span></span> <span data-ttu-id="f2739-171">VNet が作成された後、別のサブネットを追加できます。</span><span class="sxs-lookup"><span data-stu-id="f2739-171">Once the VNet is created you can add additional subnets.</span></span>

1. <span data-ttu-id="f2739-172">`172.xxx` IP アドレス空間を使用するように既定の範囲を変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f2739-172">Let's change the default ranges to use the `172.xxx` IP address space.</span></span> <span data-ttu-id="f2739-173">[仮想ネットワーク] の下にある **[新規作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2739-173">Click **Create New** under Virtual Network.</span></span>
    - <span data-ttu-id="f2739-174">**[アドレス空間]** フィールドを `172.16.0.0/16` に変更して全アドレス範囲を指定します。</span><span class="sxs-lookup"><span data-stu-id="f2739-174">Change the **Address space** field to be `172.16.0.0/16` to give it the full range of addresses</span></span>
    - <span data-ttu-id="f2739-175">**[Subnet range]\(サブネット範囲\)** フィールドを `172.16.1.0/24` に変更して空間の 256 個の IP アドレスを指定します。</span><span class="sxs-lookup"><span data-stu-id="f2739-175">Change the **Subnet range** field to be `172.16.1.0/24` to give it 256 IP addresses of the space.</span></span>

1. <span data-ttu-id="f2739-176">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2739-176">Click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="f2739-177">既定では、Azure によって VM 用に仮想ネットワーク、ネットワーク インターフェイス、パブリック IP が作成されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-177">By default, Azure will create a virtual network, network interface, and public IP for your VM.</span></span> <span data-ttu-id="f2739-178">VM が作成された後でネットワーク オプションを変更するのは簡単ではないので、Azure で作成するサービスでのネットワーク割り当てを常に再確認してください。</span><span class="sxs-lookup"><span data-stu-id="f2739-178">It's not trivial to change the networking options after the VM has been created so always double-check the network assignments on services you create in Azure.</span></span>

## <a name="finish-configuring-the-vm-and-create-the-image"></a><span data-ttu-id="f2739-179">VM の構成を完了してイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="f2739-179">Finish configuring the VM and create the image</span></span>

<span data-ttu-id="f2739-180">残りのオプションは既定値で問題ないので、どれも変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f2739-180">The rest of the options have reasonable defaults and there's no need to change any of them.</span></span> <span data-ttu-id="f2739-181">そうしたければ、他のタブを調べてもかまいません。</span><span class="sxs-lookup"><span data-stu-id="f2739-181">You can explore the other tabs if you like.</span></span> <span data-ttu-id="f2739-182">個々のオプションの横には `(i)` アイコンがあり、オプションを説明するヘルプ バブルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-182">The individual options have an `(i)` icon next to them that will show a help bubble to explain the option.</span></span> <span data-ttu-id="f2739-183">これは、VM の構成に使用できるさまざまなオプションについて学習するのによい方法です。</span><span class="sxs-lookup"><span data-stu-id="f2739-183">This is a great way to learn about the various options you can use to configure the VM.</span></span>

1. <span data-ttu-id="f2739-184">パネルの下部にある **[確認および作成]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2739-184">Click the **Review + create** button at the bottom of the panel.</span></span>

1. <span data-ttu-id="f2739-185">システムによってオプションが検証され、作成される VM についての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-185">The system will validate your options and give you details about the VM being created.</span></span>

1. <span data-ttu-id="f2739-186">**[作成]** をクリックして VM を作成し、デプロイします。</span><span class="sxs-lookup"><span data-stu-id="f2739-186">Click **Create** to create and deploy the VM.</span></span> <span data-ttu-id="f2739-187">Azure ダッシュボードにはデプロイされている VM が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2739-187">The Azure dashboard will show the VM that's being deployed.</span></span> <span data-ttu-id="f2739-188">これには数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f2739-188">This may take several minutes.</span></span>

<span data-ttu-id="f2739-189">デプロイが行われている間に、この VM でできることを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="f2739-189">While that's deploying, let's look at what we can do with this VM.</span></span>