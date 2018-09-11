<span data-ttu-id="eeabc-101">別のシナリオを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="eeabc-101">Let's consider an alternate scenario.</span></span> <span data-ttu-id="eeabc-102">あなたは、ある学校に所属しているとします。この学校では Windows 仮想マシンを使用してテスト環境を作成しています。学生はこの環境で Web アプリをインストールし、ドメインを構成し、Windows のサービスと機能を試しているので、学校のコンピューターに影響を与えることはありません。</span><span class="sxs-lookup"><span data-stu-id="eeabc-102">Perhaps your organization is a school that uses Windows virtual machines to spin up test environments on which students install web apps, configure domains, and explore Windows services and features without affecting the school's computers.</span></span> <span data-ttu-id="eeabc-103">教師は、この環境の VM に RDP を使用して接続し、学生の進捗状況を調べています。</span><span class="sxs-lookup"><span data-stu-id="eeabc-103">Teachers can then connect to these VMs by using RDP and check on student progress.</span></span> <span data-ttu-id="eeabc-104">このような環境を Azure でどのようにして構築するかを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="eeabc-104">Let's explore how you might build something like this with Azure.</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="eeabc-105">Windows VM を作成する</span><span class="sxs-lookup"><span data-stu-id="eeabc-105">Create a Windows VM</span></span>

<span data-ttu-id="eeabc-106">Windows VM を作成するには、次の手順を完了します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-106">To create a Windows VM, complete the following steps:</span></span>

1. <span data-ttu-id="eeabc-107">[Azure portal](https://portal.azure.com?azure-portal=true) から Azure にログオンします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-107">Log onto Azure through the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="eeabc-108">Azure portal の左上隅にある **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-108">Click **Create a resource** in the upper left corner of the Azure portal.</span></span>

1. <span data-ttu-id="eeabc-109">**検索バー**に、「**Windows Server 2016 Datacenter**」と入力し、同じタイトルのリンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-109">In the **search bar**, enter  **Windows Server 2016 Datacenter**  and then click on the link with the same title.</span></span>

### <a name="configure-the-vm-settings"></a><span data-ttu-id="eeabc-110">VM 設定を構成する</span><span class="sxs-lookup"><span data-stu-id="eeabc-110">Configure the VM settings</span></span>

1. <span data-ttu-id="eeabc-111">**[基本]** の下にある **[名前]** フィールドに、"StudentVM" など、ご利用の VM の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-111">Under **Basics**, in the **Name** field, enter a name for your VM, such as "StudentVM."</span></span>

1. <span data-ttu-id="eeabc-112">**[VM ディスクの種類]** フィールドで、ドロップダウン メニューをクリックしてオプションを表示します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-112">In the **VM Disk Type** field, click the drop-down menu to see the options.</span></span> <span data-ttu-id="eeabc-113">**[SSD]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-113">Ensure that **SSD** is selected.</span></span>

1. <span data-ttu-id="eeabc-114">**[ユーザー名]** フィールドには、VM へのサインインに使用するのに適したユーザー名を入力します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-114">In the **Username** field, enter a suitable user name to use to sign in to the VM.</span></span>

1. <span data-ttu-id="eeabc-115">**[パスワード]** フィールドには、少なくとも 12 文字の長さのパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-115">In the **Password** field, enter a password that's at least 12 characters long.</span></span> <span data-ttu-id="eeabc-116">大文字と小文字、数字、特殊文字を含める必要もあります。</span><span class="sxs-lookup"><span data-stu-id="eeabc-116">It must also have upper and lowercase characters, numbers, and special characters.</span></span>

1. <span data-ttu-id="eeabc-117">**[リソース グループ]** で、**[新規作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-117">Under **Resource group**, click **Create new**.</span></span> <span data-ttu-id="eeabc-118">リソース グループには、"myTestRG" などの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="eeabc-118">Give the resource group a name, such as "myTestRG."</span></span>

1. <span data-ttu-id="eeabc-119">作成する VM に適した場所を選択してから、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-119">Select a suitable location for the VM to be created, and then click **OK**.</span></span>

### <a name="select-the-vm-image-size-and-options"></a><span data-ttu-id="eeabc-120">VM イメージのサイズとオプションを選択する</span><span class="sxs-lookup"><span data-stu-id="eeabc-120">Select the VM image size and options</span></span>

1. <span data-ttu-id="eeabc-121">**[サイズの選択]** ページで、**[B1s]** イメージ、**[選択]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-121">In the **Choose a size** page, click the **B1s** image, and then click **Select**.</span></span>

   > [!Note] 
   > <span data-ttu-id="eeabc-122">B1 イメージには 1 GB の RAM しかないため、最初にサインインしたときにメモリ エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-122">The B1 image has only 1 GB of RAM and will result in memory errors when you first sign in.</span></span> <span data-ttu-id="eeabc-123">しかし、後で実習中にイメージのサイズを変更することができます。</span><span class="sxs-lookup"><span data-stu-id="eeabc-123">However, you can resize the image later as part of a later lab.</span></span>

1. <span data-ttu-id="eeabc-124">**[設定]** ページの **[パブリック受信ポートを選択]** で、**[パブリック受信ポートなし]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-124">In the **Settings** page, under **Select Public Inbound Ports**, select **No public inbound ports**.</span></span> <span data-ttu-id="eeabc-125">RDP アクセスは後で構成します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-125">We'll configure RDP access later.</span></span>

### <a name="finish-configuring-the-vm-and-create-the-image"></a><span data-ttu-id="eeabc-126">VM の構成を完了してイメージを作成する</span><span class="sxs-lookup"><span data-stu-id="eeabc-126">Finish configuring the VM and create the image</span></span>

1. <span data-ttu-id="eeabc-127">下部までスクロールし、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-127">Scroll to the bottom and click **OK**.</span></span>

1. <span data-ttu-id="eeabc-128">**[作成]** で、構成した設定を確認します。</span><span class="sxs-lookup"><span data-stu-id="eeabc-128">Under **Create**, check the settings that you configured.</span></span> <span data-ttu-id="eeabc-129">下部にある **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="eeabc-129">At the bottom, click **Create**.</span></span> <span data-ttu-id="eeabc-130">Azure ダッシュボードにはデプロイされている VM が表示されます。</span><span class="sxs-lookup"><span data-stu-id="eeabc-130">The Azure dashboard will show the VM that's being deployed.</span></span> <span data-ttu-id="eeabc-131">これには数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="eeabc-131">This may take several minutes.</span></span>

## <a name="summary"></a><span data-ttu-id="eeabc-132">まとめ</span><span class="sxs-lookup"><span data-stu-id="eeabc-132">Summary</span></span>

<span data-ttu-id="eeabc-133">これで、学生の要件に適しており、学校のネットワークからアクセス可能な Windows Server 仮想マシンが作成されました。</span><span class="sxs-lookup"><span data-stu-id="eeabc-133">You've now created a Windows Server virtual machine that's suitable for student requirements and accessible from the school network.</span></span> <span data-ttu-id="eeabc-134">次の演習では、RDP を使用して、その VM に接続して管理する方法を見ていきます。</span><span class="sxs-lookup"><span data-stu-id="eeabc-134">In the next unit, you will look at how you connect to and manage that VM by using RDP.</span></span>
