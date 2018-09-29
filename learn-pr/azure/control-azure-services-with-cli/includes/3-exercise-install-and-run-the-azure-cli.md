<span data-ttu-id="c8486-101">Azure CLI をローカル コンピューターにインストールしてから、コマンドを実行してインストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="c8486-101">Let's install the Azure CLI on your local machine, and then run a command to verify your installation.</span></span> <span data-ttu-id="c8486-102">Azure CLI をインストールするために使用する方法は、お使いのコンピューターのオペレーティング システムによって異なります。</span><span class="sxs-lookup"><span data-stu-id="c8486-102">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="c8486-103">お使いのオペレーティング システムの手順を選択してください。</span><span class="sxs-lookup"><span data-stu-id="c8486-103">Choose the steps for your operating system.</span></span>

> [!NOTE]
> <span data-ttu-id="c8486-104">この演習では、Azure CLI ツールをローカルでインストールする手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="c8486-104">This exercise guides you through installing the Azure CLI tool locally.</span></span> <span data-ttu-id="c8486-105">モジュールの残りの部分では Azure Cloud Shell を使用して、Microsoft Learn で無料のサブスクリプション サポートを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="c8486-105">The remainder of the module will use the Azure Cloud Shell so you can leverage the free subscription support in Microsoft Learn.</span></span> <span data-ttu-id="c8486-106">この演習は省略可能なアクティビティと考え、必要に応じて手順を確認できます。</span><span class="sxs-lookup"><span data-stu-id="c8486-106">You can consider this exercise as an optional activity and just review the instructions if you prefer.</span></span>

<span data-ttu-id="c8486-107">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="c8486-107">::: zone pivot="linux"</span></span>

## <a name="linux"></a><span data-ttu-id="c8486-108">Linux</span><span class="sxs-lookup"><span data-stu-id="c8486-108">Linux</span></span>

<span data-ttu-id="c8486-109">ここでは、Advanced Packaging Tool (**apt**) と Bash コマンド ラインを使用して、**Ubuntu Linux** 上に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="c8486-109">Here you will install the Azure CLI on **Ubuntu Linux** using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!TIP]
> <span data-ttu-id="c8486-110">次のコマンドは、Ubuntu バージョン 18.04 用です。</span><span class="sxs-lookup"><span data-stu-id="c8486-110">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="c8486-111">Linux のその他のバージョンやディストリビューションの手順は異なります。</span><span class="sxs-lookup"><span data-stu-id="c8486-111">Other versions and distributions of Linux have different instructions.</span></span> <span data-ttu-id="c8486-112">別の Linux バージョンを使用する場合は、[公式ドキュメント](https://docs.microsoft.com/cli/azure/install-azure-cli)を確認してください。</span><span class="sxs-lookup"><span data-stu-id="c8486-112">Check the [official documentation](https://docs.microsoft.com/cli/azure/install-azure-cli) if you are using a different Linux version.</span></span>

1. <span data-ttu-id="c8486-113">Microsoft リポジトリが登録されるように、自分のソース リストを変更すると、パッケージ マネージャーで Azure CLI パッケージを検索できるようになります。</span><span class="sxs-lookup"><span data-stu-id="c8486-113">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. <span data-ttu-id="c8486-114">Microsoft Ubuntu リポジトリ用の暗号化キーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="c8486-114">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="c8486-115">これにより、インストールする Azure CLI パッケージが Microsoft から提供されていることをパッケージ マネージャーで確認できます。</span><span class="sxs-lookup"><span data-stu-id="c8486-115">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="c8486-116">Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="c8486-116">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

<span data-ttu-id="c8486-117">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="c8486-117">::: zone-end</span></span>

<span data-ttu-id="c8486-118">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="c8486-118">::: zone pivot="macos"</span></span>

## <a name="macos"></a><span data-ttu-id="c8486-119">macOS</span><span class="sxs-lookup"><span data-stu-id="c8486-119">macOS</span></span>

<span data-ttu-id="c8486-120">ここでは、Homebrew パッケージ マネージャーを使用して macOS に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="c8486-120">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c8486-121">**brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="c8486-121">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="c8486-122">詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c8486-122">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="c8486-123">最新の Azure CLI パッケージを確実に入手するため、brew リポジトリを更新します。</span><span class="sxs-lookup"><span data-stu-id="c8486-123">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```

1. <span data-ttu-id="c8486-124">Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="c8486-124">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

<span data-ttu-id="c8486-125">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="c8486-125">::: zone-end</span></span>

<span data-ttu-id="c8486-126">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="c8486-126">::: zone pivot="windows"</span></span>

## <a name="windows"></a><span data-ttu-id="c8486-127">Windows</span><span class="sxs-lookup"><span data-stu-id="c8486-127">Windows</span></span>

<span data-ttu-id="c8486-128">ここでは、MSI インストーラーを使用して Windows に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="c8486-128">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="c8486-129">[https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) に移動して、ブラウザーのセキュリティ ダイアログ ボックスで、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8486-129">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="c8486-130">インストーラーで、ライセンス条項に同意し、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c8486-130">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="c8486-131">**[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c8486-131">In the **User Account Control** dialog, select **Yes**.</span></span>

<span data-ttu-id="c8486-132">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="c8486-132">::: zone-end</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="c8486-133">Azure CLI の実行</span><span class="sxs-lookup"><span data-stu-id="c8486-133">Running the Azure CLI</span></span>

<span data-ttu-id="c8486-134">Azure CLI を実行するには、bash シェルを開く (Linux と macOS) か、コマンド プロンプトまたは PowerShell (Windows) から実行します。</span><span class="sxs-lookup"><span data-stu-id="c8486-134">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="c8486-135">Azure CLI を起動し、バージョン チェックを実行して、インストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="c8486-135">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```azurecli
    az --version
    ```

<span data-ttu-id="c8486-136">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="c8486-136">::: zone pivot="windows"</span></span>

> [!TIP]
> <span data-ttu-id="c8486-137">PowerShell から Azure CLI を実行することには、Windows コマンド プロンプトから Azure CLI を実行することに比べて、いくつかの利点があります。</span><span class="sxs-lookup"><span data-stu-id="c8486-137">Running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the Windows command prompt.</span></span> <span data-ttu-id="c8486-138">PowerShell では、コマンド プロンプトよりも多くのタブ補完機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="c8486-138">PowerShell provides additional tab completion features over those available from the command prompt.</span></span>

<span data-ttu-id="c8486-139">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="c8486-139">::: zone-end</span></span>

<span data-ttu-id="c8486-140">Azure CLI を使用して Azure リソースを管理できるように、ローカル コンピューターを設定しました。</span><span class="sxs-lookup"><span data-stu-id="c8486-140">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="c8486-141">Azure CLI をローカルに使用して、コマンドを入力したりスクリプトを実行したりできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="c8486-141">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="c8486-142">入力したコマンドは、Azure CLI によって Azure データセンターに転送され、Azure サブスクリプションの内部で実行されます。</span><span class="sxs-lookup"><span data-stu-id="c8486-142">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>