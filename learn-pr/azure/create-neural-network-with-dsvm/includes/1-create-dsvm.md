### <a name="exercise-1-create-an-ubuntu-data-science-vm"></a><span data-ttu-id="0b7a4-101">演習 1: Ubuntu Data Science VM の作成</span><span class="sxs-lookup"><span data-stu-id="0b7a4-101">Exercise 1: Create an Ubuntu Data Science VM</span></span>

<span data-ttu-id="0b7a4-102">Linux 用の Data Science Virtual Machine は、データ サイエンスの使用の開始を簡単にする仮想マシン イメージです。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-102">The Data Science Virtual Machine for Linux is a virtual-machine image that simplifies getting started with data science.</span></span> <span data-ttu-id="0b7a4-103">すぐに起動して実行できるように、複数のツールが既に構築、インストール、および構成されています。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-103">Multiple tools are already built, installed, and configured in order to get you up and running quickly.</span></span> <span data-ttu-id="0b7a4-104">[Jupyter](http://jupyter.org/)、一部のサンプル Jupyter ノートブック、[TensorFlow](https://www.tensorflow.org/) のように、NVIDIA GPU ドライバー、[NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads)、[NVIDIA CUDA ディープ ニューラル ネットワーク](https://developer.nvidia.com/cudnn) (cuDNN) ライブラリも含まれます。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-104">The NVIDIA GPU driver, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads), and [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn) (cuDNN) library are also included, as are [Jupyter](http://jupyter.org/), several sample Jupyter notebooks, and [TensorFlow](https://www.tensorflow.org/).</span></span> <span data-ttu-id="0b7a4-105">事前にインストールされているフレームワークはすべて、GPU 対応ですが、CPU でも動作します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-105">All pre-installed frameworks are GPU-enabled but work on CPUs as well.</span></span> <span data-ttu-id="0b7a4-106">この演習では、Azure で Linux Data Science Virtual Machine のインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-106">In this exercise, you will create an instance of the Data Science Virtual Machine for Linux on Azure.</span></span>

1. <span data-ttu-id="0b7a4-107">ブラウザーで [Azure portal](https://portal.azure.com) を開きます。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-107">Open the [Azure Portal](https://portal.azure.com) in your browser.</span></span> <span data-ttu-id="0b7a4-108">ログインを求められたら、Microsoft アカウントを使用してサインインします。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-108">If asked to log in, do so using your Microsoft account.</span></span>

1. <span data-ttu-id="0b7a4-109">ポータルの左側にあるメニューで **[+ リソースの作成]** をクリックし、検索ボックスに「data science」と入力します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-109">Click **+ Create a resource** in the menu on the left side of the portal, and then type "data science" (without quotation marks) into the search box.</span></span> <span data-ttu-id="0b7a4-110">検索一覧から **[Linux (Ubuntu) Data Science Virtual Machine]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-110">Select **Data Science Virtual Machine for Linux (Ubuntu)** from the results list.</span></span>

    ![Ubuntu Data Science VM の検索](../images/new-data-science-vm.png)

    <span data-ttu-id="0b7a4-112">"Ubuntu Data Science VM の検索"__</span><span class="sxs-lookup"><span data-stu-id="0b7a4-112">_Finding the Ubuntu Data Science VM_</span></span>

1. <span data-ttu-id="0b7a4-113">少しばかり時間をとって、VM に含まれているツールの一覧をご確認ください。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-113">Take a moment to review the list of tools included in the VM.</span></span> <span data-ttu-id="0b7a4-114">次に、ブレードの下部にある **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-114">Then click **Create** at the bottom of the blade.</span></span>

1. <span data-ttu-id="0b7a4-115">[基本] ブレードに次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-115">Fill in the "Basics" blade as shown below.</span></span> <span data-ttu-id="0b7a4-116">12 文字以上で大文字、小文字、数字、特殊文字を含むパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-116">Provide a password that's at least 12 characters long containing a mix of uppercase letters, lowercase letters, numbers and special characters.</span></span> <span data-ttu-id="0b7a4-117">*入力したユーザー名とパスワードは必ず覚えておいてください。後に演習で必要になります。*</span><span class="sxs-lookup"><span data-stu-id="0b7a4-117">*Be sure to remember the user name and password that you enter, because you will need them later in the lab.*</span></span>

    ![VM に関する基本的な情報の入力。](../images/create-data-science-vm-1.png)

    <span data-ttu-id="0b7a4-119">_基本設定の入力_</span><span class="sxs-lookup"><span data-stu-id="0b7a4-119">_Entering basic settings_</span></span>

1. <span data-ttu-id="0b7a4-120">[サイズの選択] ブレードで **[DS1_V2 Standard]** を選択します。データ サイエンス VM を低コストで実験できます。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-120">In the "Choose a size" blade, select **DS1_V2 Standard**, which provides a low-cost way to experiment with Data Science VMs.</span></span> <span data-ttu-id="0b7a4-121">次に、ブレードの下部にある **[選択]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-121">Then click the **Select** button at the bottom of the blade.</span></span>

    ![VM サイズの選択](../images/create-data-science-vm-2.png)

    <span data-ttu-id="0b7a4-123">"VM サイズの選択"__</span><span class="sxs-lookup"><span data-stu-id="0b7a4-123">_Choosing a VM size_</span></span>

1. <span data-ttu-id="0b7a4-124">クライアントがポート 22 で [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) プロトコルを利用して VM に接続できるように、[設定] ブレードの受信ポートの一覧で **[SSH (22)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-124">In the "Settings" blade, check **SSH (22)** in the list of inbound ports so clients can connect to the VM using the [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protocol on port 22.</span></span> <span data-ttu-id="0b7a4-125">次に、**[OK]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="0b7a4-125">Then click **OK**.</span></span>

    ![VM の作成](../images/create-data-science-vm-3.png)

    <span data-ttu-id="0b7a4-127">"VM の作成"__</span><span class="sxs-lookup"><span data-stu-id="0b7a4-127">_Creating the VM_</span></span>

1. <span data-ttu-id="0b7a4-128">[作成] ブレードで少し時間をとって VM に選択したオプションを見直し、**[作成]** をクリックして VM 作成プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-128">In the "Create" blade, take a moment to review the options you selected for the VM, and click **Create** to start the VM creation process.</span></span>

    ![VM の作成](../images/create-data-science-vm-4.png)

    <span data-ttu-id="0b7a4-130">"VM の作成"__</span><span class="sxs-lookup"><span data-stu-id="0b7a4-130">_Creating the VM_</span></span>

1. <span data-ttu-id="0b7a4-131">ポータルの左側のメニューで **[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-131">Click **Resource groups** in the menu on the left side of the portal.</span></span> <span data-ttu-id="0b7a4-132">次に "data-science-rg" リソース グループをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-132">Then click the "data-science-rg" resource group.</span></span>

    ![リソース グループを開く](../images/open-resource-group.png)

    <span data-ttu-id="0b7a4-134">"リソース グループを開く"__</span><span class="sxs-lookup"><span data-stu-id="0b7a4-134">_Opening the resource group_</span></span>

1. <span data-ttu-id="0b7a4-135">"デプロイ中" が "成功" に変わり、DSVM と補助 Azure リソースが作成されたことが示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-135">Wait until "Deploying" changes to "Succeeded" indicating that DSVM and supporting Azure resources have been created.</span></span> <span data-ttu-id="0b7a4-136">通常、デプロイに要する時間は 5 分以下です。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-136">Deployment typically takes 5 minutes or less.</span></span> <span data-ttu-id="0b7a4-137">ブレードの上部にある **[更新]** をときどきクリックして、デプロイの状態を更新します。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-137">Periodically click **Refresh** at the top of the blade to refresh the deployment status.</span></span>

    ![デプロイ状態の監視](../images/deployment-succeeded.png)

    <span data-ttu-id="0b7a4-139">_デプロイ状態の監視_</span><span class="sxs-lookup"><span data-stu-id="0b7a4-139">_Monitoring the deployment_</span></span>

<span data-ttu-id="0b7a4-140">デプロイが完了したら、次の演習に進みます。</span><span class="sxs-lookup"><span data-stu-id="0b7a4-140">Once the deployment has completed, proceed to the next exercise.</span></span>