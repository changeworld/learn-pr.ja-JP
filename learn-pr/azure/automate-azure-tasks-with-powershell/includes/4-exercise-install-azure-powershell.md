<span data-ttu-id="085e5-101">この演習では、ローカル コンピューターに **Azure PowerShell** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-101">In this exercise, you will install **Azure PowerShell** on your local machine.</span></span> <span data-ttu-id="085e5-102">お使いのオペレーティング システムに該当するセクションを選択してください。</span><span class="sxs-lookup"><span data-stu-id="085e5-102">Choose the appropriate section for your operating system.</span></span>

## <a name="linux-and-mac"></a><span data-ttu-id="085e5-103">Linux および Mac</span><span class="sxs-lookup"><span data-stu-id="085e5-103">Linux and Mac</span></span>
<span data-ttu-id="085e5-104">Linux と macOS では、最初に **PowerShell Core** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-104">On Linux and macOS, the first step is to install **PowerShell Core**.</span></span>

### <a name="linux"></a><span data-ttu-id="085e5-105">Linux</span><span class="sxs-lookup"><span data-stu-id="085e5-105">Linux</span></span>
<span data-ttu-id="085e5-106">最後のユニットで説明されているように、Linux 用 PowerShell のインストールではパッケージ マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="085e5-106">As mentioned in the last unit, installing PowerShell for Linux will involve using a package manager.</span></span> <span data-ttu-id="085e5-107">この例では **Ubuntu 18.04** を使用しますが、[他のバージョンやディストリビューションに関する詳細な説明についてはこちらのドキュメント](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="085e5-107">We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span></span>

<span data-ttu-id="085e5-108">Advanced Packaging Tool (**apt**) と Bash コマンド ラインを使用して、Ubuntu Linux 上に PowerShell Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-108">You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span> 

1. <span data-ttu-id="085e5-109">Microsoft Ubuntu リポジトリ用の暗号化キーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="085e5-109">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="085e5-110">これにより、インストールする PowerShell Core パッケージが Microsoft から提供されていることをパッケージ マネージャーで確認できます。</span><span class="sxs-lookup"><span data-stu-id="085e5-110">This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="085e5-111">パッケージ マネージャーで PowerShell Core パッケージを検索することができるように、**Microsoft Ubuntu リポジトリ**を登録します。</span><span class="sxs-lookup"><span data-stu-id="085e5-111">Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.</span></span>

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. <span data-ttu-id="085e5-112">パッケージの一覧を更新します。</span><span class="sxs-lookup"><span data-stu-id="085e5-112">Update the list of packages.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="085e5-113">PowerShell Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-113">Install PowerShell Core.</span></span>

    ```bash
    sudo apt-get install -y powershell
    ```

1. <span data-ttu-id="085e5-114">PowerShell を起動して、正常にインストールされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="085e5-114">Start PowerShell to verify that it installed successfully.</span></span>

    ```bash
    pwsh
    ```

### <a name="macos"></a><span data-ttu-id="085e5-115">macOS</span><span class="sxs-lookup"><span data-stu-id="085e5-115">macOS</span></span>
<span data-ttu-id="085e5-116">次に、Homebrew パッケージ マネージャーを使用して macOS に **PowerShell Core** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-116">Next, install **PowerShell Core** on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="085e5-117">**brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="085e5-117">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="085e5-118">詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="085e5-118">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="085e5-119">Homebrew-Cask をインストールして、PowerShell Core パッケージなどの他のパッケージを取得します。</span><span class="sxs-lookup"><span data-stu-id="085e5-119">Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:</span></span>

    ```bash
    brew tap caskroom/cask
    ```

1. <span data-ttu-id="085e5-120">PowerShell Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-120">Install PowerShell Core:</span></span>

    ```bash
    brew cask installs powershell
    ```

1. <span data-ttu-id="085e5-121">PowerShell Core を起動して、正常にインストールされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="085e5-121">Start PowerShell Core to verify that it installed successfully:</span></span>

    ```bash
    pwsh
    ```

## <a name="install-azure-powershell"></a><span data-ttu-id="085e5-122">Azure PowerShell をインストールする</span><span class="sxs-lookup"><span data-stu-id="085e5-122">Install Azure PowerShell</span></span>
<span data-ttu-id="085e5-123">ベース **PowerShell** 製品をインストールした後、**Azure PowerShell** をインストールして Azure 固有のコマンドを追加します。</span><span class="sxs-lookup"><span data-stu-id="085e5-123">After installing the base **PowerShell** product, install **Azure PowerShell** to add the Azure-specific commands.</span></span>

### <a name="windows"></a><span data-ttu-id="085e5-124">Windows</span><span class="sxs-lookup"><span data-stu-id="085e5-124">Windows</span></span>
<span data-ttu-id="085e5-125">`Install-Module` PowerShell コマンドを使用して、Windows に Azure PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-125">Install Azure PowerShell on Windows using the `Install-Module` PowerShell command.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="085e5-126">Azure PowerShell をインストールするには、PowerShell バージョン 5.0 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="085e5-126">You must have PowerShell version 5.0 or higher to install Azure PowerShell.</span></span> <span data-ttu-id="085e5-127">PowerShell のバージョンを確認するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="085e5-127">To check your version of PowerShell, use the following command:</span></span> 
>
> `$PSVersionTable.PSVersion` 
>
><span data-ttu-id="085e5-128">バージョン番号が 5.0 より小さい場合は、[既存の Windows PowerShell をアップグレードする](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)手順に従ってください。</span><span class="sxs-lookup"><span data-stu-id="085e5-128">If the version number is lower than 5.0, follow the instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span></span>

1. <span data-ttu-id="085e5-129">**[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="085e5-129">Open the **Start** menu and type **Windows PowerShell**.</span></span>

1. <span data-ttu-id="085e5-130">**[Windows PowerShell]** アイコンを右クリックし、**[管理者として実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="085e5-130">Right-click the **Windows PowerShell** icon and select **Run as administrator**.</span></span>

1. <span data-ttu-id="085e5-131">**[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="085e5-131">In the **User Account Control** dialog, select **Yes**.</span></span>

1. <span data-ttu-id="085e5-132">次のコマンドを入力して、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="085e5-132">Type the following command, and then press Enter:</span></span>

    ```powershell
    Install-Module -Name AzureRM
    ```

1. <span data-ttu-id="085e5-133">PSGallery からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="085e5-133">If you are asked whether you trust modules from PSGallery, answer **Yes** or **Yes to All**.</span></span>

> [!TIP]
> <span data-ttu-id="085e5-134">Azure PowerShell モジュールの何らかのバージョンが既にインストールされていることを示すエラー メッセージが表示された場合は、次のコマンドを実行して "_最新_" のバージョンに更新できます。</span><span class="sxs-lookup"><span data-stu-id="085e5-134">If you get an error message indicating that a version of the Azure PowerShell module is already installed, you can update to the _latest_ version by issuing the command:</span></span>
> 
> `Update-Module -Name AzureRM`
> 
> <span data-ttu-id="085e5-135">`Install-Module` コマンドと同じように、モジュールの信頼の確認を求められたら **[はい]** または **[すべてはい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="085e5-135">As with the `Install-Module` command, answer **Yes** or **Yes to All** when prompted to trust the module.</span></span>

### <a name="linux-or-macos"></a><span data-ttu-id="085e5-136">Linux または macOS</span><span class="sxs-lookup"><span data-stu-id="085e5-136">Linux or macOS</span></span>
<span data-ttu-id="085e5-137">同じ基本プロセスを使用して、Linux または macOS に Azure PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-137">We use the same basic process to install the Azure PowerShell on either Linux or macOS.</span></span> <span data-ttu-id="085e5-138">手順はどちらのオペレーティング システムでも同じです。</span><span class="sxs-lookup"><span data-stu-id="085e5-138">The procedure is the same for both operating systems.</span></span> <span data-ttu-id="085e5-139">違っているのは、PowerShell Core セッションを管理者特権にする点です。</span><span class="sxs-lookup"><span data-stu-id="085e5-139">The difference is in getting an elevated PowerShell Core session.</span></span>

1. <span data-ttu-id="085e5-140">ターミナルで次のコマンドを入力し、管理者特権で PowerShell Core を起動します。</span><span class="sxs-lookup"><span data-stu-id="085e5-140">In a terminal, type the following command to launch PowerShell Core with elevated privileges.</span></span>

    ```bash
    sudo pwsh
    ```

1. <span data-ttu-id="085e5-141">PowerShell Core プロンプトで次のコマンドを実行して、Azure PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="085e5-141">Run the following command at the PowerShell Core prompt to install Azure PowerShell.</span></span>

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. <span data-ttu-id="085e5-142">**PSGallery** からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="085e5-142">If you are asked whether you trust modules from **PSGallery**, answer **Yes** or **Yes to All**.</span></span>

## <a name="summary"></a><span data-ttu-id="085e5-143">まとめ</span><span class="sxs-lookup"><span data-stu-id="085e5-143">Summary</span></span>
<span data-ttu-id="085e5-144">Azure PowerShell を使用して Azure のリソースを管理できるようにローカル コンピューターをセットアップしました。</span><span class="sxs-lookup"><span data-stu-id="085e5-144">You have setup your local machine(s) to administer Azure resources with Azure PowerShell.</span></span> <span data-ttu-id="085e5-145">Azure PowerShell をローカルで使用して、コマンドを入力したりスクリプトを実行したりできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="085e5-145">You can now use Azure PowerShell locally to enter commands or execute scripts.</span></span> <span data-ttu-id="085e5-146">入力したコマンドは、Azure PowerShell によって Azure データセンターに転送され、Azure サブスクリプションの内部で実行されます。</span><span class="sxs-lookup"><span data-stu-id="085e5-146">Azure PowerShell will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>