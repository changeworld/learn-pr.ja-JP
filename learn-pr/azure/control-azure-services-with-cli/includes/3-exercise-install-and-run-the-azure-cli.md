
<span data-ttu-id="1c814-101">この演習では、Azure CLI をローカル コンピューターにインストールしてから、シンプルなコマンドを実行してインストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="1c814-101">In this exercise, you will install the Azure CLI on your local machine, and then run a simple command to verify your installation.</span></span> 

## <a name="installing-the-azure-cli"></a><span data-ttu-id="1c814-102">Azure CLI のインストール</span><span class="sxs-lookup"><span data-stu-id="1c814-102">Installing the Azure CLI</span></span>
<span data-ttu-id="1c814-103">Azure CLI をインストールするために使用する方法は、お使いのコンピューターのオペレーティング システムによって異なります。</span><span class="sxs-lookup"><span data-stu-id="1c814-103">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="1c814-104">お使いのオペレーティング システムの手順を選択してください。</span><span class="sxs-lookup"><span data-stu-id="1c814-104">Please choose the steps for your operating system.</span></span>

### <a name="linux"></a><span data-ttu-id="1c814-105">Linux</span><span class="sxs-lookup"><span data-stu-id="1c814-105">Linux</span></span>
<span data-ttu-id="1c814-106">ここでは、Advanced Packaging Tool (**apt**) と Bash コマンド ラインを使用して、Ubuntu Linux 上に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1c814-106">Here you will install the Azure CLI on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!NOTE]
> <span data-ttu-id="1c814-107">次のコマンドは、Ubuntu バージョン 18.04 用です。</span><span class="sxs-lookup"><span data-stu-id="1c814-107">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="1c814-108">別のバージョンの Ubuntu を使用している場合は、別のリポジトリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c814-108">If you are using a different version of Ubuntu, you must add a different repository.</span></span> <span data-ttu-id="1c814-109">詳細については、「[apt での Azure CLI 2.0 のインストール](https://docs.microsoft.com/cli/azure/install-azure-cli-apt)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1c814-109">For details see [Install the Azure CLI 2.0 with apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span></span>

1. <span data-ttu-id="1c814-110">Microsoft リポジトリが登録されるように、自分のソース リストを変更すると、パッケージ マネージャーで Azure CLI パッケージを検索できるようになります。</span><span class="sxs-lookup"><span data-stu-id="1c814-110">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```
1. <span data-ttu-id="1c814-111">Microsoft Ubuntu リポジトリ用の暗号化キーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="1c814-111">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="1c814-112">これにより、インストールする Azure CLI パッケージが Microsoft から提供されていることをパッケージ マネージャーで確認できます。</span><span class="sxs-lookup"><span data-stu-id="1c814-112">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```
1. <span data-ttu-id="1c814-113">Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1c814-113">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

### <a name="macos"></a><span data-ttu-id="1c814-114">macOS</span><span class="sxs-lookup"><span data-stu-id="1c814-114">macOS</span></span>
<span data-ttu-id="1c814-115">ここでは、Homebrew パッケージ マネージャーを使用して macOS に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1c814-115">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1c814-116">**brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="1c814-116">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="1c814-117">詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1c814-117">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="1c814-118">最新の Azure CLI パッケージを確実に入手するため、brew リポジトリを更新します。</span><span class="sxs-lookup"><span data-stu-id="1c814-118">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```
1. <span data-ttu-id="1c814-119">Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1c814-119">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

### <a name="windows"></a><span data-ttu-id="1c814-120">Windows</span><span class="sxs-lookup"><span data-stu-id="1c814-120">Windows</span></span>
<span data-ttu-id="1c814-121">ここでは、MSI インストーラーを使用して Windows に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="1c814-121">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="1c814-122">[https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) に移動して、ブラウザーのセキュリティ ダイアログ ボックスで、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c814-122">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="1c814-123">インストーラーで、ライセンス条項に同意し、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1c814-123">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="1c814-124">**[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="1c814-124">In the **User Account Control** dialog, select **Yes**.</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="1c814-125">Azure CLI の実行</span><span class="sxs-lookup"><span data-stu-id="1c814-125">Running the Azure CLI</span></span>
<span data-ttu-id="1c814-126">Azure CLI を実行するには、bash シェルを開く (Linux と macOS) か、コマンド プロンプトまたは PowerShell (Windows) から実行します。</span><span class="sxs-lookup"><span data-stu-id="1c814-126">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="1c814-127">Azure CLI を起動し、バージョン チェックを実行して、インストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="1c814-127">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```bash
    az --version
    ```

> [!NOTE]
> <span data-ttu-id="1c814-128">Windows では、コマンド プロンプトから Azure CLI を実行するよりも、PowerShell から Azure CLI を実行する方がいくつかの利点があります。たとえば、PowerShell では、コマンド プロンプトよりも多くのタブ補完機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="1c814-128">In Windows, running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the command prompt; for example, PowerShell provides additional tab completion features over those available from the command prompt.</span></span> 

## <a name="summary"></a><span data-ttu-id="1c814-129">まとめ</span><span class="sxs-lookup"><span data-stu-id="1c814-129">Summary</span></span>
<span data-ttu-id="1c814-130">Azure CLI を使用して Azure リソースを管理できるように、ローカル コンピューターを設定しました。</span><span class="sxs-lookup"><span data-stu-id="1c814-130">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="1c814-131">Azure CLI をローカルに使用して、コマンドを入力したりスクリプトを実行したりできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="1c814-131">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="1c814-132">入力したコマンドは、Azure CLI によって Azure データセンターに転送され、Azure サブスクリプションの内部で実行されます。</span><span class="sxs-lookup"><span data-stu-id="1c814-132">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>
