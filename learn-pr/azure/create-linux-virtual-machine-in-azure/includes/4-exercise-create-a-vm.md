<span data-ttu-id="3d575-101">目標は、Apache を実行している既存の Linux サーバーを Azure に移動することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="3d575-101">Recall that our goal is to move an existing Linux server running Apache to Azure.</span></span> <span data-ttu-id="3d575-102">最初に、Ubuntu Linux サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3d575-102">We'll start by creating an Ubuntu Linux server.</span></span>

## <a name="create-a-new-linux-virtual-machine"></a><span data-ttu-id="3d575-103">新しい Linux 仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="3d575-103">Create a new Linux virtual machine</span></span>

<span data-ttu-id="3d575-104">Linux VM の作成は、Azure portal、Azure CLI、または Azure PowerShell で行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3d575-104">We can create Linux VMs with the Azure portal, the Azure CLI, or Azure PowerShell.</span></span> <span data-ttu-id="3d575-105">Azure で始めるときの最も簡単な方法は、ポータルを使用することです。そうすれば、作成の間に、必要な情報が順番に示され、ヒントと有用なメッセージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-105">The easiest approach when you are starting with Azure is to use the portal because it walks you through the required information and provides hints and helpful messages during the creation:</span></span>

1. <span data-ttu-id="3d575-106">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="3d575-106">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="3d575-107">Azure portal の左上隅にある **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d575-107">Click **Create a resource** in the upper-left corner of the Azure portal.</span></span>

1. <span data-ttu-id="3d575-108">検索ボックスに「**Ubuntu Server**」と入力し、使用できるバージョンを表示します。</span><span class="sxs-lookup"><span data-stu-id="3d575-108">In the search box, enter  **Ubuntu Server** to see the different versions available.</span></span> <span data-ttu-id="3d575-109">表示された一覧から **Ubuntu Server 18.04 LTS** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3d575-109">Select **Ubuntu Server 18.04 LTS** from the presented list.</span></span>

1. <span data-ttu-id="3d575-110">**[作成]** ボタンをクリックして、VM の構成を始めます。</span><span class="sxs-lookup"><span data-stu-id="3d575-110">Click the **Create** button to start configuring the VM.</span></span>

## <a name="configure-the-vm-settings"></a><span data-ttu-id="3d575-111">VM 設定を構成する</span><span class="sxs-lookup"><span data-stu-id="3d575-111">Configure the VM settings</span></span>

<span data-ttu-id="3d575-112">ポータルでの VM 作成エクスペリエンスはウィザードの形式で提示され、VM のすべての構成領域が順番に示されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-112">The VM creation experience in the portal is presented in a wizard format to walk you through all the configuration areas for the VM.</span></span> <span data-ttu-id="3d575-113">**[次へ]** ボタンをクリックすると、次の構成可能なセクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="3d575-113">Clicking the **Next** button will take you to the next configurable section.</span></span> <span data-ttu-id="3d575-114">ただし、上部に並んでいるタブで各部分が示されており、自由にセクション間を移動できます。</span><span class="sxs-lookup"><span data-stu-id="3d575-114">However, you can move between the sections at will with the tabs running across the top that identify each part.</span></span>

![Ubuntu Server マシン用の最初の仮想マシン作成ブレードが表示されている Azure portal のスクリーンショット。](../media/3-azure-portal-create-vm.png)

<span data-ttu-id="3d575-116">必須のオプション (赤いアスタリスクで示されたもの) をすべて入力したら、ウィザードの残りの手順をスキップし、下部にある **[確認および作成]** ボタンをクリックして VM の作成を始めることができます。</span><span class="sxs-lookup"><span data-stu-id="3d575-116">Once you fill in all the required options (identified with red asterisks), you can skip the remainder of the wizard experience and start creating the VM through the **Review + Create** button at the bottom.</span></span>

<span data-ttu-id="3d575-117">最初は **[基本]** セクションです。</span><span class="sxs-lookup"><span data-stu-id="3d575-117">We'll start with the **Basics** section.</span></span>

### <a name="configure-basic-vm-settings"></a><span data-ttu-id="3d575-118">VM の基本設定を構成する</span><span class="sxs-lookup"><span data-stu-id="3d575-118">Configure basic VM settings</span></span>

1. <span data-ttu-id="3d575-119">**[サブスクリプション]** には、既定でサンドボックスのサブスクリプションが選択されているはずです。</span><span class="sxs-lookup"><span data-stu-id="3d575-119">For **Subscription**, the sandbox subscription should be selected for you by default.</span></span>

1. <span data-ttu-id="3d575-120">**[リソース グループ]** には、既定で **<rgn>[サンドボックス リソース グループ名]</rgn>** という名前のリソース グループが選択されているはずです。</span><span class="sxs-lookup"><span data-stu-id="3d575-120">For **Resource group**, the resource group with the name **<rgn>[sandbox resource group name]</rgn>** should be selected for you by default.</span></span>

1. <span data-ttu-id="3d575-121">**[インスタンスの詳細]** セクションで、Web サーバー VM の名前を入力します (例: **test-web-eus-vm1**)。</span><span class="sxs-lookup"><span data-stu-id="3d575-121">In the **INSTANCE DETAILS** section, enter a name for your web server VM, such as **test-web-eus-vm1**.</span></span> <span data-ttu-id="3d575-122">この名前は、環境 (**test**)、ロール (**web**)、場所 (**米国東部**)、サービス (**vm**)、インスタンス番号 (**1**) を示します。</span><span class="sxs-lookup"><span data-stu-id="3d575-122">This indicates the environment (**test**), the role (**web**), location (**East US**), service (**vm**), and instance number (**1**).</span></span>
    - <span data-ttu-id="3d575-123">目的を簡単に識別できるように、リソース名を標準化するのがベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="3d575-123">It's a best practice to standardize your resource names, so you can quickly identify their purpose.</span></span> <span data-ttu-id="3d575-124">Linux VM の名前は、1 から 64 文字の範囲で、数字、文字、ダッシュで構成されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d575-124">Linux VM names must be between 1 and 64 characters and be comprised of numbers, letters, and dashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3d575-125">設定を変更してフリーテキスト フィールドから移動すると、各値が自動的に検証されて、問題がない場合は横に緑色のチェック マークが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-125">As you change settings and tab out of each free-text field, Azure will validate each value automatically and place a green check mark next to it when it's good.</span></span> <span data-ttu-id="3d575-126">エラー インジケーターをポイントすると、検出された問題についての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-126">You can hover over error indicators to get more information on issues it discovers.</span></span>

1. <span data-ttu-id="3d575-127">場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="3d575-127">Select a location.</span></span>

    <span data-ttu-id="3d575-128"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]</span><span class="sxs-lookup"><span data-stu-id="3d575-128"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]</span></span>

1. <span data-ttu-id="3d575-129">**[Availability options]\(可用性オプション\)** は **[No infrastructure redundancy required]\(インフラストラクチャの冗長性不要\)** のままにしておきます。</span><span class="sxs-lookup"><span data-stu-id="3d575-129">Leave **Availability options** as **No infrastructure redundancy required**.</span></span> <span data-ttu-id="3d575-130">このオプションは、複数の VM をグループ化して計画的または計画外のメンテナンス イベントや停止に対処することで、VM の高可用性を確保するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-130">This option is used to ensure the VM is highly available by grouping multiple VMs together as a set to deal with planned or unplanned maintenance events or outages.</span></span>

1. <span data-ttu-id="3d575-131">イメージが **Ubuntu Server 18.04 LTS** に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3d575-131">Ensure that the image is set to **Ubuntu Server 18.04 LTS**.</span></span> <span data-ttu-id="3d575-132">ドロップダウン リストを開いて、使用可能なすべてのオプションを確認できます。</span><span class="sxs-lookup"><span data-stu-id="3d575-132">You can open the drop-down list to see all the options available.</span></span>

1. <span data-ttu-id="3d575-133">**[サイズ]** フィールドを直接編集することはできず、汎用コンピューティングの選択肢の 1 つである既定サイズ **[DS2_v3]** になっています。</span><span class="sxs-lookup"><span data-stu-id="3d575-133">The **Size** field is not directly editable and has a **DS2_v3** default size, which is one of the general-purpose computing selections.</span></span> <span data-ttu-id="3d575-134">この選択はパブリック Web サーバーに最適ですが、**[サイズの変更]** リンクをクリックして他の VM サイズを調べます。</span><span class="sxs-lookup"><span data-stu-id="3d575-134">This choice is perfect for a public web server, but click the **Change size** link to explore other VM sizes.</span></span> <span data-ttu-id="3d575-135">結果のダイアログでは、**vCPU の数**、**名前**、**ディスクの種類**に基づいてフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="3d575-135">The resulting dialog allows you to filter based on **# of vCPUs**, **Name**, and **Disk Type**.</span></span> <span data-ttu-id="3d575-136">2 個の vCPU と 8 GB の RAM を提供する同じ **[DS2_v3]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3d575-136">Select the same **DS2_v3** choice, which gives you two vCPUs with 8 GB of RAM.</span></span>

    > [!TIP]
    > <span data-ttu-id="3d575-137">ビューを左側にスライドして新しいウィンドウを開いたときの VM 設定に戻し、ウィンドウをスライドしてそれを表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="3d575-137">You can also slide the view to the left to get back to the VM settings as it opened a new window off to the right and slid the window over to view it.</span></span>

1. <span data-ttu-id="3d575-138">SSH でサインインするために使用する**ユーザー名**を入力します。</span><span class="sxs-lookup"><span data-stu-id="3d575-138">Enter a **username** you'll use to sign in with SSH.</span></span>

1. <span data-ttu-id="3d575-139">**[管理者アクセス]** セクションで、**[認証の種類]** に対して SSH 公開キー オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="3d575-139">In the **ADMINISTRATOR ACCESS** section, select the SSH public key option for **Authentication type**.</span></span>

1. <span data-ttu-id="3d575-140">公開キー ファイルから SSH キーをコピーして、**[SSH 公開キー]** フィールドに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="3d575-140">Copy the SSH key from your public key file and paste it into the **SSH public key** field.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3d575-141">Azure portal に公開キーをコピーするときは、余分な空白文字や改行文字が追加されないように注意してください。</span><span class="sxs-lookup"><span data-stu-id="3d575-141">When you copy the public key into the Azure portal, make sure not to add any additional whitespace or line-feed characters.</span></span>

1. <span data-ttu-id="3d575-142">**[受信ポートの規則]** セクションで一覧を開き、**[No public inbound ports]\(パブリック公開ポートなし\)** をオフにします。</span><span class="sxs-lookup"><span data-stu-id="3d575-142">In the **INBOUND PORT RULES** section, open the list and uncheck **No public inbound ports**.</span></span> <span data-ttu-id="3d575-143">これは Linux VM なので、リモートで SSH を使用して VM にアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="3d575-143">Since this is a Linux VM, we want to be able to access the VM using SSH remotely.</span></span> <span data-ttu-id="3d575-144">必要に応じて一覧をスクロールし、**[SSH (22)]** を見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="3d575-144">Scroll the list if necessary until you find **SSH (22)** and select it.</span></span> <span data-ttu-id="3d575-145">UI の注意事項が示すとおり、VM を作成した後でネットワーク ポートを調整することもできます。</span><span class="sxs-lookup"><span data-stu-id="3d575-145">As the note in the UI indicates, we can also adjust the network ports after we create the VM.</span></span>

    ![説明したように SSH アクセス用に Linux VM を開くための管理者アカウントと受信ポートの設定が表示されている Azure portal のスクリーンショット。](../media/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a><span data-ttu-id="3d575-147">VM 用にディスクを構成する</span><span class="sxs-lookup"><span data-stu-id="3d575-147">Configure disks for the VM</span></span>

1. <span data-ttu-id="3d575-148">**[Next: Disks >]\(次へ: ディスク >\)** をクリックし、**[ディスク]** セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="3d575-148">Click **Next: Disks >** to move to the **Disks** section.</span></span>

    ![[仮想マシンの作成] ブレードの [ディスク] セクションが表示されている Azure portal のスクリーンショット。](../media/3-configure-disks.png)

1. <span data-ttu-id="3d575-150">**[OS ディスクの種類]** で **[Premium SSD]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3d575-150">Choose **Premium SSD** for the **OS disk type**.</span></span>

1. <span data-ttu-id="3d575-151">マネージド ディスクを使用するので、ストレージ アカウントを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3d575-151">Use managed disks, so we don't have to work with storage accounts.</span></span> <span data-ttu-id="3d575-152">GUI でスイッチを切り替えて、Azure で必要な情報の違いを確認できます。</span><span class="sxs-lookup"><span data-stu-id="3d575-152">You can flip the switch in the GUI to see the difference in information that Azure needs if you like.</span></span>

### <a name="create-a-data-disk"></a><span data-ttu-id="3d575-153">データ ディスクを作成する</span><span class="sxs-lookup"><span data-stu-id="3d575-153">Create a data disk</span></span>

<span data-ttu-id="3d575-154">OS ディスク (/dev/sda) と一時ディスク (/dev/sda) を使用することを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="3d575-154">Recall that we will get an OS disk (/dev/sda) and a temporary disk (/dev/sdb).</span></span> <span data-ttu-id="3d575-155">データ ディスクも追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="3d575-155">Let's add a data disk as well:</span></span>

1. <span data-ttu-id="3d575-156">**[データ ディスク]** セクションで **[Create and attach a new disk]\(新しいディスクの作成とアタッチ\)** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d575-156">Click the **Create and attach a new disk** link in the **DATA DISKS** section.</span></span>

    ![[新しいディスクを作成する] ブレードが表示されている Azure portal のスクリーンショット。](../media/3-add-data-disk.png)

1. <span data-ttu-id="3d575-158">すべての既定値を使用できます (Premium SSD、1023 GB、および**なし** (空のディスク))。ただし、ここではスナップショットまたは Azure Blob Storage を使用して VHD を作成することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3d575-158">You can take all the defaults: Premium SSD, 1023 GB, and **None** (empty disk), although notice that here is where we could use a snapshot or Azure Blob storage to create a VHD.</span></span>

1. <span data-ttu-id="3d575-159">**[OK]** をクリックしてディスクを作成し、**[データ ディスク]** セクションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="3d575-159">Click **OK** to create the disk and go back to the **DATA DISKS** section.</span></span>

1. <span data-ttu-id="3d575-160">最初の行に新しいディスクが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="3d575-160">There should now be a new disk in the first row.</span></span>

    ![VM 作成プロセスで新しく作成されたデータ ディスクの行が表示されている Azure portal のスクリーンショット。](../media/3-new-disk.png)

## <a name="configure-the-network"></a><span data-ttu-id="3d575-162">ネットワークを構成する</span><span class="sxs-lookup"><span data-stu-id="3d575-162">Configure the network</span></span>

1. <span data-ttu-id="3d575-163">**[Next: Networking >]\(次へ: ネットワーク >\)** をクリックして、**[ネットワーク]** セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="3d575-163">Click **Next: Networking >** to move to the **Networking** section.</span></span>

1. <span data-ttu-id="3d575-164">既に他のコンポーネントがある運用システムでは、"_既存の_" 仮想ネットワークを利用します。</span><span class="sxs-lookup"><span data-stu-id="3d575-164">In a production system where we already have other components, we'd want to utilize an _existing_ virtual network.</span></span> <span data-ttu-id="3d575-165">そうすれば、VM はソリューション内の他のクラウド サービスと通信できます。</span><span class="sxs-lookup"><span data-stu-id="3d575-165">That way, our VM can communicate with the other cloud services in our solution.</span></span> <span data-ttu-id="3d575-166">この場所でまだ定義されていない場合は、ここで作成して構成できます。</span><span class="sxs-lookup"><span data-stu-id="3d575-166">If there isn't one defined in this location yet, we can create it here and configure the:</span></span>
    - <span data-ttu-id="3d575-167">**[アドレス空間]**: このネットワークに使用できる IPV4 空間全域。</span><span class="sxs-lookup"><span data-stu-id="3d575-167">**Address space**: The overall IPV4 space available to this network.</span></span>
    - <span data-ttu-id="3d575-168">**[Subnet range]\(サブネット範囲\)**: アドレス空間を分割する最初のサブネット。定義されているアドレス空間内に収まる必要があります。</span><span class="sxs-lookup"><span data-stu-id="3d575-168">**Subnet range**: The first subnet to subdivide the address space - it must fit within the defined address space.</span></span> <span data-ttu-id="3d575-169">VNet が作成された後、別のサブネットを追加できます。</span><span class="sxs-lookup"><span data-stu-id="3d575-169">Once the VNet is created, you can add additional subnets.</span></span>

> [!NOTE]
> <span data-ttu-id="3d575-170">既定では、Azure によって VM 用に仮想ネットワーク、ネットワーク インターフェイス、パブリック IP が作成されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-170">By default, Azure will create a virtual network, network interface, and public IP for your VM.</span></span> <span data-ttu-id="3d575-171">VM が作成された後でネットワーク オプションを変更するのは簡単ではないので、Azure で作成するサービスでのネットワーク割り当てを常に再確認してください。</span><span class="sxs-lookup"><span data-stu-id="3d575-171">It's not trivial to change the networking options after the VM has been created, so always double-check the network assignments on services you create in Azure.</span></span>

## <a name="finish-configuring-the-vm-and-create-the-image"></a><span data-ttu-id="3d575-172">VM の構成を完了してイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="3d575-172">Finish configuring the VM and create the image</span></span>

<span data-ttu-id="3d575-173">残りのオプションは既定値で問題ないので、どれも変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3d575-173">The rest of the options have reasonable defaults, and there's no need to change any of them.</span></span> <span data-ttu-id="3d575-174">そうしたければ、他のタブを調べてもかまいません。</span><span class="sxs-lookup"><span data-stu-id="3d575-174">You can explore the other tabs if you like.</span></span> <span data-ttu-id="3d575-175">個々のオプションの横には `(i)` アイコンがあり、オプションを説明するヘルプ ヒントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-175">The individual options have an `(i)` icon next to them that will show a help tip to explain the option.</span></span> <span data-ttu-id="3d575-176">これは、VM の構成に使用できるさまざまなオプションについて学習するのによい方法です。</span><span class="sxs-lookup"><span data-stu-id="3d575-176">This is a great way to learn about the various options you can use to configure the VM:</span></span>

1. <span data-ttu-id="3d575-177">パネルの下部にある **[確認および作成]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3d575-177">Click the **Review + create** button at the bottom of the panel.</span></span>

1. <span data-ttu-id="3d575-178">システムによってオプションが検証され、作成される VM についての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-178">The system will validate your options and give you details about the VM being created.</span></span>

1. <span data-ttu-id="3d575-179">**[作成]** をクリックして VM を作成し、デプロイします。</span><span class="sxs-lookup"><span data-stu-id="3d575-179">Click **Create** to create and deploy the VM.</span></span> <span data-ttu-id="3d575-180">Azure ダッシュボードにはデプロイされている VM が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3d575-180">The Azure dashboard will show the VM that's being deployed.</span></span> <span data-ttu-id="3d575-181">これには数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="3d575-181">This may take several minutes.</span></span>

<span data-ttu-id="3d575-182">デプロイが行われている間に、この VM でできることを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="3d575-182">While that's deploying, let's look at what we can do with this VM.</span></span>