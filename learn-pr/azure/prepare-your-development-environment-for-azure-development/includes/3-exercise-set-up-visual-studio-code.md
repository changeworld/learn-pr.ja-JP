<span data-ttu-id="aa680-101">この演習では、Visual Studio Code と Azure App Service 拡張機能をインストールします。これで、Microsoft Azure 向けに Web アプリを開発し、デプロイする準備が整います。</span><span class="sxs-lookup"><span data-stu-id="aa680-101">In this exercise, you will install Visual Studio Code and the Azure App Service extension, which will get you ready to develop for Microsoft Azure and to deploy a web app.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="aa680-102">演習の手順</span><span class="sxs-lookup"><span data-stu-id="aa680-102">Exercise steps</span></span>

<span data-ttu-id="aa680-103">まず、使用しているオペレーティング システムを特定し、以下の適切なセクションの手順に従って Visual Studio Code をインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa680-103">First, identify which operating system you are using, and follow the steps in the appropriate section below to install Visual Studio Code.</span></span>

### <a name="windows"></a><span data-ttu-id="aa680-104">Windows</span><span class="sxs-lookup"><span data-stu-id="aa680-104">Windows</span></span>

1. <span data-ttu-id="aa680-105">Windows 用 Visual Studio Code インストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="aa680-105">Download the Visual Studio Code installer for Windows.</span></span>
2. <span data-ttu-id="aa680-106">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="aa680-106">Run the installer.</span></span> <span data-ttu-id="aa680-107">この操作に長い時間はかかりません。</span><span class="sxs-lookup"><span data-stu-id="aa680-107">This won't take long.</span></span>
3. <span data-ttu-id="aa680-108">インストール フォルダーに移動して VS Code を開きます (64 ビット マシンの場合、既定のパスは C:\Program Files\Microsoft VS Code です)。</span><span class="sxs-lookup"><span data-stu-id="aa680-108">Open VS Code by navigating to the installation folder (the default path is C:\Program Files\Microsoft VS Code for a 64-bit machine).</span></span>

### <a name="macos"></a><span data-ttu-id="aa680-109">macOS</span><span class="sxs-lookup"><span data-stu-id="aa680-109">macOS</span></span>

1. <span data-ttu-id="aa680-110">macOS 用 Visual Studio Code をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="aa680-110">Download Visual Studio Code for macOS.</span></span>
2. <span data-ttu-id="aa680-111">ダウンロードしたアーカイブをダブルクリックして内容を展開します。</span><span class="sxs-lookup"><span data-stu-id="aa680-111">Double-click on the downloaded archive to expand the contents.</span></span>
3. <span data-ttu-id="aa680-112">Visual Studio Code.app を Applications フォルダーにドラッグし、Launchpad で使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="aa680-112">Drag Visual Studio Code.app to the Applications folder, making it available in the Launchpad.</span></span>
4. <span data-ttu-id="aa680-113">アイコンを右クリックし、[オプション] > [Dock に追加] の順に選択し、Dock に VS Code を追加します。</span><span class="sxs-lookup"><span data-stu-id="aa680-113">Add VS Code to your Dock by right-clicking on the icon, and choosing Options > Keep in Dock.</span></span>

### <a name="linux--debian-and-ubuntu"></a><span data-ttu-id="aa680-114">Linux - Debian と Ubuntu</span><span class="sxs-lookup"><span data-stu-id="aa680-114">Linux – Debian and Ubuntu</span></span>

1. <span data-ttu-id="aa680-115">グラフィカル ソフトウェア センター (使用できる場合) またはコマンド ラインを使用して [.deb package (64 ビット)](https://go.microsoft.com/fwlink/?LinkID=760868) をダウンロードしてインストールします (`<file>` はダウンロードした .deb ファイル名で置き換えます)。</span><span class="sxs-lookup"><span data-stu-id="aa680-115">Download and install the [.deb package (64-bit)](https://go.microsoft.com/fwlink/?LinkID=760868), either through the graphical software center, if it's available, or through the command line (replacing `<file>` with the .deb filename you downloaded):</span></span>

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

### <a name="linux--rhel-fedora-and-centos"></a><span data-ttu-id="aa680-116">Linux - RHEL、Fedora、CentOS</span><span class="sxs-lookup"><span data-stu-id="aa680-116">Linux – RHEL, Fedora, and CentOS</span></span>

1. <span data-ttu-id="aa680-117">次のスクリプトを使用して、キーとリポジトリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa680-117">Use the following script to install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. <span data-ttu-id="aa680-118">パッケージ キャッシュを更新し、dnf (Fedora 22 以降) を使用してパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa680-118">Update the package cache, and install the package by using dnf (Fedora 22 and above):</span></span>

    ```bash
    dnf check-update
    sudo dnf install code
    ```

### <a name="linux--opensuse-and-sle"></a><span data-ttu-id="aa680-119">Linux - openSUSE と SLE</span><span class="sxs-lookup"><span data-stu-id="aa680-119">Linux – openSUSE and SLE</span></span>

1. <span data-ttu-id="aa680-120">yum リポジトリは、openSUSE と SLE ベースのシステムでも機能します。</span><span class="sxs-lookup"><span data-stu-id="aa680-120">The yum repository also works for openSUSE and SLE based systems.</span></span> <span data-ttu-id="aa680-121">次のスクリプトで、キーとリポジトリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa680-121">The following script will install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. <span data-ttu-id="aa680-122">パッケージ キャッシュを更新し、以下を使用してパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa680-122">Update the package cache and install the package by using:</span></span>

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> <span data-ttu-id="aa680-123">さまざまな Linux ディストリビューションで VS Code をインストールまたは更新する方法の詳細については、[Linux 上で VS Code を実行する方法に関するドキュメント](https://code.visualstudio.com/docs/setup/linux)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa680-123">For further details about installing or updating VS Code on various Linux distributions, please see the [Running VS Code on Linux documentation](https://code.visualstudio.com/docs/setup/linux).</span></span>

## <a name="install-azure-app-service-extension"></a><span data-ttu-id="aa680-124">Azure App Service 拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="aa680-124">Install Azure App Service extension</span></span>

<span data-ttu-id="aa680-125">VS Code のインストールが完了したら開きます。</span><span class="sxs-lookup"><span data-stu-id="aa680-125">Once you have installed VS Code, open it.</span></span>

1. <span data-ttu-id="aa680-126">[拡張機能] タブに移動します。</span><span class="sxs-lookup"><span data-stu-id="aa680-126">Go to the Extensions tab.</span></span>
2. <span data-ttu-id="aa680-127">Azure App Service を検索します。</span><span class="sxs-lookup"><span data-stu-id="aa680-127">Search for Azure App Service.</span></span>
3. <span data-ttu-id="aa680-128">[インストール] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="aa680-128">Click Install.</span></span>

    <span data-ttu-id="aa680-129">次のスクリーン ショットは、Visual Studio Code 拡張機能の検索結果から選択した Azure App Service 拡張機能を示しています。</span><span class="sxs-lookup"><span data-stu-id="aa680-129">The following screenshot shows the Azure App Service extension selected from the Visual Studio Code extension search results.</span></span>

    ![[拡張機能] タブが表示され、検索結果で Azure App Service 拡張機能が強調表示されている VS Code のスクリーンショット。](../media/3-install-azure-extension.png)

<span data-ttu-id="aa680-131">これで、拡張機能がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="aa680-131">This will install the extension.</span></span> <span data-ttu-id="aa680-132">Azure サブスクリプションに接続し、Web、モバイル、または API アプリを開発して Azure App Service にデプロイする準備が整います。</span><span class="sxs-lookup"><span data-stu-id="aa680-132">You will be ready to connect to your Azure subscription, and develop for and deploy your web, mobile, or API app to an Azure App Service.</span></span>
