<span data-ttu-id="80e0f-101">目標は、Apache を実行している既存の Linux サーバーを Azure に移動することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="80e0f-101">Recall that our goal is to move an existing Linux server running Apache to Azure.</span></span> <span data-ttu-id="80e0f-102">最初に、Ubuntu Linux サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-102">We'll start by creating an Ubuntu Linux server.</span></span>

## <a name="create-a-new-linux-virtual-machine"></a><span data-ttu-id="80e0f-103">新しい Linux 仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="80e0f-103">Create a new Linux virtual machine</span></span>

<span data-ttu-id="80e0f-104">Linux VM の作成は、Azure portal、Azure CLI、または Azure PowerShell で行うことができます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-104">We can create Linux VMs with the Azure portal, the Azure CLI, or Azure PowerShell.</span></span> <span data-ttu-id="80e0f-105">Azure で始めるときの最も簡単な方法は、ポータルを使用することです。そうすれば、作成の間に、必要な情報が順番に示され、ヒントと有用なメッセージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-105">The easiest approach when you are starting with Azure is to use the portal because it walks you through the required information and provides hints and helpful messages during the creation:</span></span>

1. <span data-ttu-id="80e0f-106">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-106">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="80e0f-107">Azure portal の左上隅にある **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-107">Click **Create a resource** in the upper-left corner of the Azure portal.</span></span>

1. <span data-ttu-id="80e0f-108">検索ボックスに「**Ubuntu Server**」と入力し、使用できるバージョンを表示します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-108">In the search box, enter  **Ubuntu Server** to see the different versions available.</span></span> <span data-ttu-id="80e0f-109">表示された一覧から **Ubuntu Server 18.04** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-109">Select **Ubuntu Server 18.04** from the presented list.</span></span>

1. <span data-ttu-id="80e0f-110">**[作成]** ボタンをクリックして、VM の構成を始めます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-110">Click the **Create** button to start configuring the VM.</span></span>

## <a name="configure-the-vm-settings"></a><span data-ttu-id="80e0f-111">VM 設定を構成する</span><span class="sxs-lookup"><span data-stu-id="80e0f-111">Configure the VM settings</span></span>

<span data-ttu-id="80e0f-112">ポータルでの VM 作成エクスペリエンスは**ウィザード**の形式で提示され、VM のすべての構成領域が順番に示されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-112">The VM creation experience in the portal is presented in a **wizard** format to walk you through all the configuration areas for the VM.</span></span> <span data-ttu-id="80e0f-113">**[次へ]** ボタンをクリックすると、次の構成可能なセクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-113">Clicking the **Next** button will take you to the next configurable section.</span></span> <span data-ttu-id="80e0f-114">ただし、上部に並んでいるタブで各部分が示されており、自由にセクション間を移動できます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-114">However, you can move between the sections at will with the tabs running across the top that identify each part.</span></span>

![Ubuntu Server マシン用の最初の仮想マシン作成ブレードが表示されている Azure portal のスクリーンショット。](../media/3-azure-portal-create-vm.png)

<span data-ttu-id="80e0f-116">必須のオプション (赤い星で示されたもの) をすべて入力したら、残りのウィザード エクスペリエンスをスキップし、下部にある **[確認および作成]** ボタンをクリックして VM の作成を始めることができます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-116">Once you fill in all the required options (identified with red stars), you can skip the remainder of the wizard experience and start creating the VM through the **Review + Create** button at the bottom.</span></span>

<span data-ttu-id="80e0f-117">最初は **[基本]** セクションです。</span><span class="sxs-lookup"><span data-stu-id="80e0f-117">We'll start with the **Basics** section.</span></span>

### <a name="configure-basic-vm-settings"></a><span data-ttu-id="80e0f-118">VM の基本設定を構成する</span><span class="sxs-lookup"><span data-stu-id="80e0f-118">Configure basic VM settings</span></span>

1. <span data-ttu-id="80e0f-119">VM 時間の請求対象である **[サブスクリプション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-119">Select the **Subscription** that should be billed for VM hours.</span></span>

1. <span data-ttu-id="80e0f-120">**[リソース グループ]** で **[新規作成]** を選択し、リソース グループに「**ExerciseResources**」という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-120">For **Resource group**, select **Create new** and give the resource group the name **ExerciseResources**.</span></span>

> [!NOTE]  
> <span data-ttu-id="80e0f-121">設定を変更してフィールドから移動すると、各値が自動的に検証されて、問題がない場合は横に緑色のチェック マークが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-121">As you change settings and tab out of each field, Azure will validate each value automatically and place a green check mark next to it when it's good.</span></span> <span data-ttu-id="80e0f-122">エラー インジケーターをポイントすると、検出された問題についての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-122">You can hover over error indicators to get more information on issues it discovers.</span></span>

1. <span data-ttu-id="80e0f-123">**[インスタンスの詳細]** セクションで、Web サーバー VM の名前を入力します (例: **test-web-eus-vm1**)。</span><span class="sxs-lookup"><span data-stu-id="80e0f-123">In the **INSTANCE DETAILS** section, enter a name for your web server VM, such as **test-web-eus-vm1**.</span></span> <span data-ttu-id="80e0f-124">この名前は、環境 (**test**)、ロール (**web**)、場所 (**米国東部**)、サービス (**vm**)、インスタンス番号 (**1**) を示します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-124">This indicates the environment (**test**), the role (**web**), location (**East US**), service (**vm**), and instance number (**1**).</span></span>
    - <span data-ttu-id="80e0f-125">目的を簡単に識別できるように、リソース名を標準化するのがベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="80e0f-125">It's a best practice to standardize your resource names, so you can quickly identify their purpose.</span></span> <span data-ttu-id="80e0f-126">Linux VM の名前は、1 から 64 文字の範囲で、数字、文字、ダッシュで構成されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="80e0f-126">Linux VM names must be between 1 and 64 characters and be comprised of numbers, letters, and dashes.</span></span>

1. <span data-ttu-id="80e0f-127">近くのリージョンを選択します (ここでは**米国東部**)。</span><span class="sxs-lookup"><span data-stu-id="80e0f-127">Select a region close to you; we've chosen **East US**.</span></span>

1. <span data-ttu-id="80e0f-128">**可用性オプション**は **[なし]** のままにします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-128">Leave **Availability options** as **None**.</span></span> <span data-ttu-id="80e0f-129">このオプションは、複数の VM をグループ化して計画的または計画外のメンテナンス イベントや停止に対処することで、VM を高可用性にするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-129">This option is used to ensure the VM is highly available by grouping multiple VMs together as a set to deal with planned or unplanned maintenance events or outages.</span></span>

1. <span data-ttu-id="80e0f-130">イメージが **Ubuntu Server 18.04 LTS** に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-130">Ensure that the image is set to **Ubuntu Server 18.04 LTS**.</span></span> <span data-ttu-id="80e0f-131">ドロップダウン リストを開いて、使用可能なすべてのオプションを確認できます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-131">You can open the drop-down list to see all the options available.</span></span>

1. <span data-ttu-id="80e0f-132">**[サイズ]** フィールドを直接編集することはできず、汎用コンピューティングの選択肢の 1 つである既定サイズ **[DS2_v3]** になっています。</span><span class="sxs-lookup"><span data-stu-id="80e0f-132">The **Size** field is not directly editable and has a **DS2_v3** default size, which is one of the general-purpose computing selections.</span></span> <span data-ttu-id="80e0f-133">この選択はパブリック Web サーバーに最適ですが、**[Change size]\(サイズの変更\)** リンクをクリックして他の VM サイズを調べます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-133">This choice is perfect for a public web server, but click the **Change size** link to explore other VM sizes.</span></span> <span data-ttu-id="80e0f-134">結果のダイアログでは、**CPU の数**、**名前**、**ディスクの種類**に基づいてフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-134">The resulting dialog allows you to filter based on **# of CPUs**, **Name**, and **Disk Type**.</span></span> <span data-ttu-id="80e0f-135">2 個の vCPU と 8 GB の RAM を提供する同じ **[DS2_v3]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-135">Select the same **DS2_v3** choice, which gives you two vCPUs with 8 GB of RAM.</span></span>

    > [!TIP]
    > <span data-ttu-id="80e0f-136">ビューを左側にスライドして新しいウィンドウを開いたときの VM 設定に戻し、ウィンドウをスライドしてそれを表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-136">You can also slide the view to the left to get back to the VM settings as it opened a new window off to the right and slid the window over to view it.</span></span>

1. <span data-ttu-id="80e0f-137">**[ADMINISTRATOR ACCESS]\(管理者アクセス\)** セクションで、**[認証の種類]** に対して SSH 公開キー オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-137">In the **ADMINISTRATOR ACCESS** section, select the SSH public key option for **Authentication type**.</span></span>

1. <span data-ttu-id="80e0f-138">SSH でサインインするために使用する**ユーザー名**を入力します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-138">Enter a **username** you'll use to sign in with SSH.</span></span>

1. <span data-ttu-id="80e0f-139">公開キー ファイルから SSH キーをコピーして、**[SSH 公開キー]** フィールドに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-139">Copy the SSH key from your public key file and paste it into the **SSH public key** field.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="80e0f-140">Azure portal に公開キーをコピーするときは、余分な空白文字や改行文字が追加されないように注意してください。</span><span class="sxs-lookup"><span data-stu-id="80e0f-140">When you copy the public key into the Azure portal, make sure not to add any additional whitespace or linefeed characters.</span></span>

1. <span data-ttu-id="80e0f-141">**[受信ポートの規則]** セクションで一覧を開き、**[なし]** をオフにします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-141">In the **INBOUND PORT RULES** section, open the list and uncheck **None**.</span></span> <span data-ttu-id="80e0f-142">これは Linux VM なので、リモートで SSH を使用して VM にアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-142">Since this is a Linux VM, we want to be able to access the VM using SSH remotely.</span></span> <span data-ttu-id="80e0f-143">必要に応じて一覧をスクロールし、**ssh (22)** を見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-143">Scroll the list if necessary until you find **ssh (22)** and select it.</span></span> <span data-ttu-id="80e0f-144">UI の注意事項が示すとおり、VM を作成した後でネットワーク ポートを調整することもできます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-144">As the note in the UI indicates, we can also adjust the network ports after we create the VM.</span></span>

    ![説明したように SSH アクセス用に Linux VM を開くための管理者アカウントと受信ポートの設定が表示されている Azure portal のスクリーンショット。](../media/3-open-ports.png) 

## <a name="configure-disks-for-the-vm"></a><span data-ttu-id="80e0f-146">VM 用にディスクを構成する</span><span class="sxs-lookup"><span data-stu-id="80e0f-146">Configure disks for the VM</span></span>

1. <span data-ttu-id="80e0f-147">**[次へ]** をクリックして、**[ディスク]** セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-147">Click **Next** to move to the **Disks** section.</span></span>

    ![[仮想マシンの作成] ブレードの [ディスク] セクションが表示されている Azure portal のスクリーンショット。](../media/3-configure-disks.png) 

1. <span data-ttu-id="80e0f-149">**[OS ディスクの種類]** で **[Premium SSD]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-149">Choose **Premium SSD** for the **OS disk type**.</span></span>

1. <span data-ttu-id="80e0f-150">マネージド ディスクを使用するので、ストレージ アカウントを使用する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="80e0f-150">Use managed disks, so we don't have to work with storage accounts.</span></span> <span data-ttu-id="80e0f-151">GUI でスイッチを切り替えて、Azure で必要な情報の違いを確認できます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-151">You can flip the switch in the GUI to see the difference in information that Azure needs if you like.</span></span>

### <a name="create-a-data-disk"></a><span data-ttu-id="80e0f-152">データ ディスクを作成する</span><span class="sxs-lookup"><span data-stu-id="80e0f-152">Create a data disk</span></span>

<span data-ttu-id="80e0f-153">OS ディスク (/dev/sda) と一時ディスク (/dev/sda) を使用することを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="80e0f-153">Recall that we will get an OS disk (/dev/sda) and a temporary disk (/dev/sdb).</span></span> <span data-ttu-id="80e0f-154">データ ディスクも追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="80e0f-154">Let's add a data disk as well:</span></span>

1. <span data-ttu-id="80e0f-155">**[データ ディスク]** セクションで **[Create and attach a new disk]\(新しいディスクの作成とアタッチ\)** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-155">Click the **Create and attach a new disk** link in the **DATA DISKS** section.</span></span>

    ![[新しいディスクを作成する] ブレードが表示されている Azure portal のスクリーンショット。](../media/3-add-data-disk.png) 

1. <span data-ttu-id="80e0f-157">すべての既定値を使用できます (Premium SSD、1023 GB、および**なし** (空のディスク))。ただし、ここではスナップショットまたは Azure Blob Storage を使用して VHD を作成することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="80e0f-157">You can take all the defaults: Premium SSD, 1023 GB, and **None** (empty disk), although notice that here is where we could use a snapshot or Azure Blob storage to create a VHD.</span></span>

1. <span data-ttu-id="80e0f-158">**[OK]** をクリックしてディスクを作成し、**[データ ディスク]** セクションに戻ります。</span><span class="sxs-lookup"><span data-stu-id="80e0f-158">Click **OK** to create the disk and go back to the **DATA DISKS** section.</span></span>

1. <span data-ttu-id="80e0f-159">最初の行に新しいディスクが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="80e0f-159">There should now be a new disk in the first row.</span></span>

    ![VM 作成プロセスで新しく作成されたデータ ディスクの行が表示されている Azure portal のスクリーンショット。](../media/3-new-disk.png) 

## <a name="configure-the-network"></a><span data-ttu-id="80e0f-161">ネットワークを構成する</span><span class="sxs-lookup"><span data-stu-id="80e0f-161">Configure the network</span></span>

1. <span data-ttu-id="80e0f-162">**[次へ]** をクリックして、**[ネットワーク]** セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-162">Click **Next** to move to the **Networking** section.</span></span>

1. <span data-ttu-id="80e0f-163">既に他のコンポーネントがある運用システムでは、"_既存の_" 仮想ネットワークを利用します。</span><span class="sxs-lookup"><span data-stu-id="80e0f-163">In a production system where we already have other components, we'd want to utilize an _existing_ virtual network.</span></span> <span data-ttu-id="80e0f-164">そうすれば、VM はソリューション内の他のクラウド サービスと通信できます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-164">That way, our VM can communicate with the other cloud services in our solution.</span></span> <span data-ttu-id="80e0f-165">この場所でまだ定義されていない場合は、ここで作成して構成できます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-165">If there isn't one defined in this location yet, we can create it here and configure the:</span></span>
    - <span data-ttu-id="80e0f-166">**[アドレス空間]**: このネットワークに使用できる IPV4 空間全域。</span><span class="sxs-lookup"><span data-stu-id="80e0f-166">**Address space**: The overall IPV4 space available to this network.</span></span>
    - <span data-ttu-id="80e0f-167">**[Subnet range]\(サブネット範囲\)**: アドレス空間を分割する最初のサブネット。定義されているアドレス空間内に収まる必要があります。</span><span class="sxs-lookup"><span data-stu-id="80e0f-167">**Subnet range**: The first subnet to subdivide the address space - it must fit within the defined address space.</span></span> <span data-ttu-id="80e0f-168">VNet が作成された後、別のサブネットを追加できます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-168">Once the VNet is created, you can add additional subnets.</span></span>

> [!NOTE]  
> <span data-ttu-id="80e0f-169">既定では、Azure によって VM 用に仮想ネットワーク、ネットワーク インターフェイス、パブリック IP が作成されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-169">By default, Azure will create a virtual network, network interface, and public IP for your VM.</span></span> <span data-ttu-id="80e0f-170">VM が作成された後でネットワーク オプションを変更するのは簡単ではないので、Azure で作成するサービスでのネットワーク割り当てを常に再確認してください。</span><span class="sxs-lookup"><span data-stu-id="80e0f-170">It's not trivial to change the networking options after the VM has been created, so always double-check the network assignments on services you create in Azure.</span></span>

## <a name="finish-configuring-the-vm-and-create-the-image"></a><span data-ttu-id="80e0f-171">VM の構成を完了してイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="80e0f-171">Finish configuring the VM and create the image</span></span>

<span data-ttu-id="80e0f-172">残りのオプションは既定値で問題ないので、どれも変更する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="80e0f-172">The rest of the options have reasonable defaults, and there's no need to change any of them.</span></span> <span data-ttu-id="80e0f-173">そうしたければ、他のタブを調べてもかまいません。</span><span class="sxs-lookup"><span data-stu-id="80e0f-173">You can explore the other tabs if you like.</span></span> <span data-ttu-id="80e0f-174">個々のオプションの横には `(i)` アイコンがあり、オプションを説明するヘルプ バブルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-174">The individual options have an `(i)` icon next to them that will show a help bubble to explain the option.</span></span> <span data-ttu-id="80e0f-175">これは、VM の構成に使用できるさまざまなオプションについて学習するのによい方法です。</span><span class="sxs-lookup"><span data-stu-id="80e0f-175">This is a great way to learn about the various options you can use to configure the VM:</span></span>

1. <span data-ttu-id="80e0f-176">パネルの下部にある **[確認および作成]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-176">Click the **Review + create** button at the bottom of the panel.</span></span>

1. <span data-ttu-id="80e0f-177">システムによってオプションが検証され、作成される VM についての詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-177">The system will validate your options and give you details about the VM being created.</span></span>

1. <span data-ttu-id="80e0f-178">**[作成]** をクリックして VM を作成し、デプロイします。</span><span class="sxs-lookup"><span data-stu-id="80e0f-178">Click **Create** to create and deploy the VM.</span></span> <span data-ttu-id="80e0f-179">Azure ダッシュボードにはデプロイされている VM が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80e0f-179">The Azure dashboard will show the VM that's being deployed.</span></span> <span data-ttu-id="80e0f-180">これには数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="80e0f-180">This may take several minutes.</span></span>

<span data-ttu-id="80e0f-181">デプロイが行われている間に、この VM でできることを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="80e0f-181">While that's deploying, let's look at what we can do with this VM.</span></span>