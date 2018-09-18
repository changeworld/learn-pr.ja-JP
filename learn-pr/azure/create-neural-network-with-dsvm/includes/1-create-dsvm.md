### <a name="create-an-ubuntu-data-science-vm"></a><span data-ttu-id="06f90-101">Ubuntu Data Science VM の作成</span><span class="sxs-lookup"><span data-stu-id="06f90-101">Create an Ubuntu Data Science VM</span></span>

<span data-ttu-id="06f90-102">Linux 用の Data Science Virtual Machine は、データ サイエンスの使用の開始を簡単にする仮想マシン イメージです。</span><span class="sxs-lookup"><span data-stu-id="06f90-102">The Data Science Virtual Machine for Linux is a virtual-machine image that simplifies getting started with data science.</span></span> <span data-ttu-id="06f90-103">すぐに起動して実行できるように、複数のツールが既に構築、インストール、および構成されています。</span><span class="sxs-lookup"><span data-stu-id="06f90-103">Multiple tools are already built, installed, and configured in order to get you up and running quickly.</span></span> <span data-ttu-id="06f90-104">[Jupyter](http://jupyter.org/)、一部のサンプル Jupyter ノートブック、[TensorFlow](https://www.tensorflow.org/) のように、NVIDIA GPU ドライバー、[NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads)、[NVIDIA CUDA ディープ ニューラル ネットワーク](https://developer.nvidia.com/cudnn) (cuDNN) ライブラリも含まれます。</span><span class="sxs-lookup"><span data-stu-id="06f90-104">The NVIDIA GPU driver, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) and the [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn) (cuDNN) library are also included, as are [Jupyter](http://jupyter.org/), several sample Jupyter notebooks, and [TensorFlow](https://www.tensorflow.org/).</span></span> <span data-ttu-id="06f90-105">事前にインストールされているフレームワークはすべて、GPU 対応ですが、CPU でも動作します。</span><span class="sxs-lookup"><span data-stu-id="06f90-105">All pre-installed frameworks are GPU-enabled but work on CPUs as well.</span></span> <span data-ttu-id="06f90-106">このユニットでは、Azure で Linux Data Science Virtual Machine (DSVM) のインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="06f90-106">In this unit, you will create an instance of the Data Science Virtual Machine (DSVM) for Linux on Azure.</span></span>

1. <span data-ttu-id="06f90-107">ブラウザーで [Azure portal](https://portal.azure.com/?azure-portal=true) を開きます。</span><span class="sxs-lookup"><span data-stu-id="06f90-107">Open the [Azure Portal](https://portal.azure.com/?azure-portal=true) in your browser.</span></span>

1. <span data-ttu-id="06f90-108">ポータルの左側にあるメニューで **[リソースの作成]** をクリックし、検索ボックスに「data science」と入力します。</span><span class="sxs-lookup"><span data-stu-id="06f90-108">Click **Create a resource** in the menu on the left side of the portal, and then type "data science" (without quotation marks) into the search box.</span></span> <span data-ttu-id="06f90-109">検索一覧から **[Linux (Ubuntu) Data Science Virtual Machine]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="06f90-109">Select **Data Science Virtual Machine for Linux (Ubuntu)** from the results list.</span></span>

    ![Ubuntu Data Science VM の検索](../media-draft/1-new-data-science-vm.png)

1. <span data-ttu-id="06f90-111">少しばかり時間をとって、VM に含まれているツールの一覧をご確認ください。</span><span class="sxs-lookup"><span data-stu-id="06f90-111">Take a moment to review the list of tools included in the VM.</span></span> <span data-ttu-id="06f90-112">次に、ブレードの下部にある **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="06f90-112">Then, click **Create** at the bottom of the blade.</span></span>

1. <span data-ttu-id="06f90-113">[基本] ブレードに次のように入力します。</span><span class="sxs-lookup"><span data-stu-id="06f90-113">Fill in the "Basics" blade as shown below.</span></span> <span data-ttu-id="06f90-114">12 文字以上で大文字、小文字、数字、特殊文字を含むパスワードを指定します。</span><span class="sxs-lookup"><span data-stu-id="06f90-114">Provide a password that's at least 12 characters long containing a mix of uppercase letters, lowercase letters, numbers, and special characters.</span></span> <span data-ttu-id="06f90-115">"*入力したユーザー名とパスワードは必ず覚えておいてください。後にモジュールで必要になります。*"</span><span class="sxs-lookup"><span data-stu-id="06f90-115">*Be sure to remember the user name and password that you enter, because you will need them later in the module.*</span></span>

    ![VM に関する基本的な情報の入力。](../media-draft/1-create-data-science-vm-1.png)

1. <span data-ttu-id="06f90-117">[サイズの選択] ブレードで **[DS1_V2 Standard]** を選択します。データ サイエンス VM を低コストで実験できます。</span><span class="sxs-lookup"><span data-stu-id="06f90-117">In the "Choose a size" blade, select **DS1_V2 Standard**, which provides a low-cost way to experiment with Data Science VMs.</span></span> <span data-ttu-id="06f90-118">次に、ブレードの下部にある **[選択]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="06f90-118">Then, click the **Select** button at the bottom of the blade.</span></span>

    ![VM サイズの選択](../media-draft/1-create-data-science-vm-2.png)

1. <span data-ttu-id="06f90-120">**[設定]** ブレードで、既定値のまま **[OK]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="06f90-120">In the **Settings** blade, accept the defaults, and click the **OK** button.</span></span>

1. <span data-ttu-id="06f90-121">**[作成]** ブレードで少し時間をとって VM に選択したオプションを見直し、**[作成]** をクリックして VM 作成プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="06f90-121">In the **Create** blade, take a moment to review the options you selected for the VM, and click **Create** to start the VM creation process.</span></span>

    ![VM の作成](../media-draft/1-create-data-science-vm-4.png)

1. <span data-ttu-id="06f90-123">ポータルの左側のメニューで **[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="06f90-123">Click **Resource groups** in the menu on the left side of the portal.</span></span> <span data-ttu-id="06f90-124">次に **data-science-rg** リソース グループをクリックします。</span><span class="sxs-lookup"><span data-stu-id="06f90-124">Then, click the **data-science-rg** resource group.</span></span>

    ![リソース グループを開く](../media-draft/1-open-resource-group.png)

  
1. <span data-ttu-id="06f90-126">"デプロイ中" が "成功" に変わり、DSVM と補助 Azure リソースが作成されたことが示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="06f90-126">Wait until "Deploying" changes to "Succeeded", indicating that DSVM and supporting Azure resources have been created.</span></span> <span data-ttu-id="06f90-127">通常、デプロイに要する時間は 5 分以下です。</span><span class="sxs-lookup"><span data-stu-id="06f90-127">Deployment typically takes five minutes or less.</span></span> <span data-ttu-id="06f90-128">ブレードの上部にある **[更新]** をときどきクリックして、デプロイの状態を更新します。</span><span class="sxs-lookup"><span data-stu-id="06f90-128">Periodically click **Refresh** at the top of the blade to refresh the deployment status.</span></span>

    ![デプロイ状態の監視](../media-draft/1-deployment-succeeded.png)
