<span data-ttu-id="42796-101">このユニットでは、お使いのローカル コンピューターに **PowerShell** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="42796-101">In this unit, you will install **PowerShell** on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="42796-102">この演習では、PowerShell ツールをローカルでインストールする手順を説明します。</span><span class="sxs-lookup"><span data-stu-id="42796-102">This exercise guides you through installing the PowerShell tools locally.</span></span> <span data-ttu-id="42796-103">モジュールの残りの部分では Azure Cloud Shell を使用して、Microsoft Learn で無料のサブスクリプション サポートを利用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="42796-103">The remainder of the module will use the Azure Cloud Shell so you can leverage the free subscription support in Microsoft Learn.</span></span> <span data-ttu-id="42796-104">この演習は省略可能なアクティビティと考え、必要に応じて手順を確認できます。</span><span class="sxs-lookup"><span data-stu-id="42796-104">You can consider this exercise as an optional activity and just review the instructions if you prefer.</span></span>

<span data-ttu-id="42796-105">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="42796-105">::: zone pivot="linux"</span></span>

## <a name="linux"></a><span data-ttu-id="42796-106">Linux</span><span class="sxs-lookup"><span data-stu-id="42796-106">Linux</span></span>

<span data-ttu-id="42796-107">Linux 用 PowerShell のインストールでは、パッケージ マネージャーを使用します。</span><span class="sxs-lookup"><span data-stu-id="42796-107">Installing PowerShell for Linux will involve using a package manager.</span></span> <span data-ttu-id="42796-108">この例では **Ubuntu 18.04** を使用しますが、[他のバージョンやディストリビューションに関する詳細な説明についてはこちらのドキュメント](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="42796-108">We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).</span></span>

<span data-ttu-id="42796-109">Advanced Packaging Tool (**apt**) と Bash コマンド ラインを使用して、Ubuntu Linux 上に PowerShell Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="42796-109">You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span> 

1. <span data-ttu-id="42796-110">Microsoft Ubuntu リポジトリ用の暗号化キーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="42796-110">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="42796-111">これにより、インストールする PowerShell Core パッケージが Microsoft から提供されていることをパッケージ マネージャーで確認できます。</span><span class="sxs-lookup"><span data-stu-id="42796-111">This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="42796-112">パッケージ マネージャーで PowerShell Core パッケージを検索することができるように、**Microsoft Ubuntu リポジトリ**を登録します。</span><span class="sxs-lookup"><span data-stu-id="42796-112">Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.</span></span>

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. <span data-ttu-id="42796-113">パッケージの一覧を更新します。</span><span class="sxs-lookup"><span data-stu-id="42796-113">Update the list of packages.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="42796-114">PowerShell Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="42796-114">Install PowerShell Core.</span></span>

    ```bash
    sudo apt-get install -y powershell
    ```

1. <span data-ttu-id="42796-115">PowerShell を起動して、正常にインストールされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="42796-115">Start PowerShell to verify that it installed successfully.</span></span>

    ```bash
    pwsh
    ```
<span data-ttu-id="42796-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="42796-116">::: zone-end</span></span>

<span data-ttu-id="42796-117">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="42796-117">::: zone pivot="macos"</span></span>

## <a name="macos"></a><span data-ttu-id="42796-118">macOS</span><span class="sxs-lookup"><span data-stu-id="42796-118">MacOS</span></span>

<span data-ttu-id="42796-119">macOS では、最初に **PowerShell Core** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="42796-119">On macOS, the first step is to install **PowerShell Core**.</span></span> <span data-ttu-id="42796-120">これは、Homebrew パッケージ マネージャーを使用して行われます。</span><span class="sxs-lookup"><span data-stu-id="42796-120">This is done using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="42796-121">**brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="42796-121">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="42796-122">詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="42796-122">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="42796-123">Homebrew-Cask をインストールして、PowerShell Core パッケージなどの他のパッケージを取得します。</span><span class="sxs-lookup"><span data-stu-id="42796-123">Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:</span></span>

    ```bash
    brew tap caskroom/cask
    ```

1. <span data-ttu-id="42796-124">PowerShell Core をインストールします。</span><span class="sxs-lookup"><span data-stu-id="42796-124">Install PowerShell Core:</span></span>

    ```bash
    brew cask install powershell
    ```

1. <span data-ttu-id="42796-125">PowerShell Core を起動して、正常にインストールされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="42796-125">Start PowerShell Core to verify that it installed successfully:</span></span>

    ```bash
    pwsh
    ```

<span data-ttu-id="42796-126">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="42796-126">::: zone-end</span></span>

<span data-ttu-id="42796-127">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="42796-127">::: zone pivot="windows"</span></span>

## <a name="windows"></a><span data-ttu-id="42796-128">Windows</span><span class="sxs-lookup"><span data-stu-id="42796-128">Windows</span></span>
<span data-ttu-id="42796-129">PowerShell は Windows に付属していますが、お使いのコンピューターに使用可能な更新プログラムがある場合があります。</span><span class="sxs-lookup"><span data-stu-id="42796-129">PowerShell is included with Windows, however there may be an update available for your machine.</span></span> <span data-ttu-id="42796-130">使用する予定の Azure サポートでは、PowerShell バージョン 5.0 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="42796-130">The Azure support we are going to use requires PowerShell version 5.0 or higher.</span></span> <span data-ttu-id="42796-131">インストールされているバージョンを確認するには、次の手順を行います。</span><span class="sxs-lookup"><span data-stu-id="42796-131">You can check the version you have installed through the following steps:</span></span>

1. <span data-ttu-id="42796-132">**[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="42796-132">Open the **Start** menu and type **Windows PowerShell**.</span></span> <span data-ttu-id="42796-133">使用可能なショートカット リンクが複数ある場合があります。</span><span class="sxs-lookup"><span data-stu-id="42796-133">There may be multiple shortcut links available:</span></span>
    - <span data-ttu-id="42796-134">Windows PowerShell: これは 64 ビット バージョンで、通常はこれを選択します。</span><span class="sxs-lookup"><span data-stu-id="42796-134">Windows PowerShell - this is the 64-bit version and generally what you should choose.</span></span>
    - <span data-ttu-id="42796-135">Windows PowerShell (x86): これは 64 ビット Windows にインストールされている 32 ビット バージョンです。</span><span class="sxs-lookup"><span data-stu-id="42796-135">Windows PowerShell (x86) - this is a 32-bit version installed on 64-bit Windows.</span></span>
    - <span data-ttu-id="42796-136">Windows PowerShell ISE: Integrated Scripting Environment (ISE) は、PowerShell でスクリプトを記述するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="42796-136">Windows PowerShell ISE - The Integrated Scripting Environment (ISE) is used for writing scripts in PowerShell.</span></span> 
    - <span data-ttu-id="42796-137">Windows PowerShell ISE (x86): ISE の 32 ビット バージョンです。</span><span class="sxs-lookup"><span data-stu-id="42796-137">Windows PowerShell ISE (x86) - A 32-bit version of the ISE.</span></span>

1. <span data-ttu-id="42796-138">Windows PowerShell アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="42796-138">Select the Windows PowerShell icon.</span></span>

1. <span data-ttu-id="42796-139">インストールされている PowerShell のバージョンを確認するには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="42796-139">Type the following command to determine the version of PowerShell installed.</span></span>

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
<span data-ttu-id="42796-140">バージョン番号が 5.0 より小さい場合は、[既存の Windows PowerShell のアップグレード](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="42796-140">If the version number is lower than 5.0, follow these instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).</span></span>

<span data-ttu-id="42796-141">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="42796-141">::: zone-end</span></span>

<span data-ttu-id="42796-142">PowerShell をサポートするように、ローカル コンピューターをセットアップしました。</span><span class="sxs-lookup"><span data-stu-id="42796-142">You have setup your local machine(s) to support PowerShell.</span></span> <span data-ttu-id="42796-143">次は、Azure モジュールなど、追加できるその他のコマンドについてご説明します。</span><span class="sxs-lookup"><span data-stu-id="42796-143">Next, we will talk about additional commands you can add including the Azure module.</span></span>