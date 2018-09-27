<span data-ttu-id="6e398-101">Azure 開発に Visual Studio Code を使用するには、Visual Studio Code と 1 つまたは複数の Azure 拡張機能をローカルにインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6e398-101">To use Visual Studio Code for Azure development, you'll need to install Visual Studio Code locally and one or more Azure extensions.</span></span> <span data-ttu-id="6e398-102">この演習では、**Azure App Service** 拡張機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="6e398-102">In this exercise, we'll add the **Azure App Service** extension.</span></span>

## <a name="install-visual-studio-code"></a><span data-ttu-id="6e398-103">Visual Studio Code のインストール</span><span class="sxs-lookup"><span data-stu-id="6e398-103">Install Visual Studio Code</span></span>

<span data-ttu-id="6e398-104">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="6e398-104">::: zone pivot="windows"</span></span>

### <a name="windows"></a><span data-ttu-id="6e398-105">Windows</span><span class="sxs-lookup"><span data-stu-id="6e398-105">Windows</span></span>

1. <span data-ttu-id="6e398-106">[Windows 用 Visual Studio Code インストーラーをダウンロードします](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="6e398-106">[Download the Visual Studio Code installer for Windows](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="6e398-107">インストーラーを実行します。</span><span class="sxs-lookup"><span data-stu-id="6e398-107">Run the installer.</span></span>

1. <span data-ttu-id="6e398-108">Windows キーを押すか、タスク バーの Windows アイコンをクリックし、「Visual Studio Code」と入力し、結果の **Visual Studio Code** をクリックして Visual Studio Code を開きます。</span><span class="sxs-lookup"><span data-stu-id="6e398-108">Open Visual Studio Code by pressing the Windows key or clicking the Windows icon on the task bar, typing "Visual Studio Code" and clicking on the **Visual Studio Code** result.</span></span>

<span data-ttu-id="6e398-109">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="6e398-109">::: zone-end</span></span>

<span data-ttu-id="6e398-110">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="6e398-110">::: zone pivot="macos"</span></span>

### <a name="macos"></a><span data-ttu-id="6e398-111">macOS</span><span class="sxs-lookup"><span data-stu-id="6e398-111">macOS</span></span>

1. <span data-ttu-id="6e398-112">[macOS 用 Visual Studio Code をダウンロードします](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="6e398-112">[Download Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span>

1. <span data-ttu-id="6e398-113">ダウンロードしたアーカイブをダブルクリックして内容を展開します。</span><span class="sxs-lookup"><span data-stu-id="6e398-113">Double-click on the downloaded archive to expand the contents.</span></span>

1. <span data-ttu-id="6e398-114">Visual Studio Code.app を Applications フォルダーにドラッグします。</span><span class="sxs-lookup"><span data-stu-id="6e398-114">Drag Visual Studio Code.app to the Applications folder.</span></span>

1. <span data-ttu-id="6e398-115">Apps セクションのアイコンをクリックするか、Spotlight で Visual Studio Code を検索して Visual Studio Code を開きます。</span><span class="sxs-lookup"><span data-stu-id="6e398-115">Open Visual Studio Code by clicking on the icon the Apps section or by searching for Visual Studio Code in Spotlight.</span></span>

<span data-ttu-id="6e398-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="6e398-116">::: zone-end</span></span>

<span data-ttu-id="6e398-117">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="6e398-117">::: zone pivot="linux"</span></span>

### <a name="linux"></a><span data-ttu-id="6e398-118">Linux</span><span class="sxs-lookup"><span data-stu-id="6e398-118">Linux</span></span> 

#### <a name="debian-and-ubuntu"></a><span data-ttu-id="6e398-119">Debian と Ubuntu</span><span class="sxs-lookup"><span data-stu-id="6e398-119">Debian and Ubuntu</span></span>

1. <span data-ttu-id="6e398-120">グラフィカル ソフトウェア センター (使用できる場合) またはコマンド ラインを使用して、[.deb package (64 ビット)](https://go.microsoft.com/fwlink/?LinkID=760868) をダウンロードしてインストールします (`<file>` はダウンロードした .deb ファイル名で置き換えます)。</span><span class="sxs-lookup"><span data-stu-id="6e398-120">Download and install the [.deb package (64-bit)](https://go.microsoft.com/fwlink/?LinkID=760868) through the graphical software center, if it's available, or through the command line (replacing `<file>` with the .deb filename you downloaded):</span></span>

    ```bash
    sudo dpkg -i <file>.deb
    sudo apt-get install -f # Install dependencies
    ```

#### <a name="rhel-fedora-and-centos"></a><span data-ttu-id="6e398-121">RHEL、Fedora、および CentOS</span><span class="sxs-lookup"><span data-stu-id="6e398-121">RHEL, Fedora, and CentOS</span></span>

1. <span data-ttu-id="6e398-122">次のスクリプトを使用して、キーとリポジトリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6e398-122">Use the following script to install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
    ```

1. <span data-ttu-id="6e398-123">パッケージ キャッシュを更新し、dnf (Fedora 22 以降) を使用してパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6e398-123">Update the package cache, and install the package by using dnf (Fedora 22 and above):</span></span>

    ```bash
    dnf check-update
    sudo dnf install code
    ```

#### <a name="opensuse-and-sle"></a><span data-ttu-id="6e398-124">openSUSE と SLE</span><span class="sxs-lookup"><span data-stu-id="6e398-124">openSUSE and SLE</span></span>

1. <span data-ttu-id="6e398-125">yum リポジトリは、openSUSE と SLE ベースのシステムでも機能します。</span><span class="sxs-lookup"><span data-stu-id="6e398-125">The yum repository also works for openSUSE and SLE based systems.</span></span> <span data-ttu-id="6e398-126">次のスクリプトで、キーとリポジトリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6e398-126">The following script will install the key and repository:</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/zypp/repos.d/vscode.repo'
    ```

1. <span data-ttu-id="6e398-127">パッケージ キャッシュを更新し、以下を使用してパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6e398-127">Update the package cache and install the package by using:</span></span>

    ```bash
    sudo zypper refresh
    sudo zypper install code
    ```

> [!NOTE]
> <span data-ttu-id="6e398-128">さまざまな Linux ディストリビューションで Visual Studio Code をインストールまたは更新する方法の詳細については、[Linux 上で Visual Studio Code を実行する方法に関するドキュメント](https://code.visualstudio.com/docs/setup/linux)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="6e398-128">For further details about installing or updating Visual Studio Code on various Linux distributions, please see the [Running Visual Studio Code on Linux documentation](https://code.visualstudio.com/docs/setup/linux).</span></span>

<span data-ttu-id="6e398-129">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="6e398-129">::: zone-end</span></span>

## <a name="install-azure-app-service-extension"></a><span data-ttu-id="6e398-130">Azure App Service 拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="6e398-130">Install Azure App Service extension</span></span>

1. <span data-ttu-id="6e398-131">Visual Studio Code を開いていない場合は開きます。</span><span class="sxs-lookup"><span data-stu-id="6e398-131">If you haven't already, open Visual Studio Code.</span></span>

1. <span data-ttu-id="6e398-132">拡張機能ブラウザーを開きます (左側のメニューからアクセスできます)。</span><span class="sxs-lookup"><span data-stu-id="6e398-132">Open the Extensions browser; it's accessed via the menu on the left.</span></span>

1. <span data-ttu-id="6e398-133">**Azure App Service** を検索します。</span><span class="sxs-lookup"><span data-stu-id="6e398-133">Search for **Azure App Service**.</span></span>

1. <span data-ttu-id="6e398-134">結果の **Azure App Service** を選択し、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6e398-134">Select the **Azure App Service** result and click **Install**.</span></span>

    <span data-ttu-id="6e398-135">次のスクリーン ショットは、Visual Studio Code 拡張機能の検索結果から選択した Azure App Service 拡張機能を示しています。</span><span class="sxs-lookup"><span data-stu-id="6e398-135">The following screenshot shows the Azure App Service extension selected from the Visual Studio Code extension search results.</span></span>

    ![[拡張機能] タブが表示され、検索結果で Azure App Service 拡張機能が強調表示されている Visual Studio Code のスクリーンショット。](../media/3-install-azure-extension.png)

<span data-ttu-id="6e398-137">これで、拡張機能がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="6e398-137">This will install the extension.</span></span> <span data-ttu-id="6e398-138">Azure サブスクリプションに接続し、Web、モバイル、または API アプリを Azure App Service にデプロイする準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="6e398-138">You're now ready to connect to your Azure subscription and deploy a web, mobile, or API app to an Azure App Service.</span></span>
