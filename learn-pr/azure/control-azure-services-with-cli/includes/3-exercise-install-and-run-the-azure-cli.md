---
zone_pivot_groups: platform
ms.openlocfilehash: 5e0a236b9cf0c3c0b23beb1324f35a34dade2e92
ms.sourcegitcommit: 926510a198d738c5726081f6d7994fe9b6fc6edb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/29/2018
ms.locfileid: "43179830"
---
<span data-ttu-id="29a91-101">Azure CLI をローカル コンピューターにインストールしてから、シンプルなコマンドを実行してインストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="29a91-101">Let's install the Azure CLI on your local machine, and then run a simple command to verify your installation.</span></span> <span data-ttu-id="29a91-102">Azure CLI をインストールするために使用する方法は、お使いのコンピューターのオペレーティング システムによって異なります。</span><span class="sxs-lookup"><span data-stu-id="29a91-102">The method you use for installing the Azure CLI depends on the operating system of your computer.</span></span> <span data-ttu-id="29a91-103">お使いのオペレーティング システムの手順を選択してください。</span><span class="sxs-lookup"><span data-stu-id="29a91-103">Please choose the steps for your operating system.</span></span>

<span data-ttu-id="29a91-104">::: zone pivot="linux"</span><span class="sxs-lookup"><span data-stu-id="29a91-104">::: zone pivot="linux"</span></span>

### <a name="linux"></a><span data-ttu-id="29a91-105">Linux</span><span class="sxs-lookup"><span data-stu-id="29a91-105">Linux</span></span>
<span data-ttu-id="29a91-106">ここでは、Advanced Packaging Tool (**apt**) と Bash コマンド ラインを使用して、**Ubuntu Linux** 上に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="29a91-106">Here you will install the Azure CLI on **Ubuntu Linux** using the Advanced Packaging Tool (**apt**) and the Bash command line.</span></span>

> [!WARNING]
> <span data-ttu-id="29a91-107">次のコマンドは、Ubuntu バージョン 18.04 用です。</span><span class="sxs-lookup"><span data-stu-id="29a91-107">The commands listed below are for Ubuntu version 18.04.</span></span> <span data-ttu-id="29a91-108">別のバージョンの Ubuntu を使用している場合は、別のリポジトリを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29a91-108">If you are using a different version of Ubuntu, you must add a different repository.</span></span> <span data-ttu-id="29a91-109">詳細については、「[apt での Azure CLI 2.0 のインストール](https://docs.microsoft.com/cli/azure/install-azure-cli-apt)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="29a91-109">For details see [Install the Azure CLI 2.0 with apt](https://docs.microsoft.com/cli/azure/install-azure-cli-apt).</span></span>

1. <span data-ttu-id="29a91-110">Microsoft リポジトリが登録されるように、自分のソース リストを変更すると、パッケージ マネージャーで Azure CLI パッケージを検索できるようになります。</span><span class="sxs-lookup"><span data-stu-id="29a91-110">Modify your sources list so that the Microsoft repository is registered, and the package manager can locate the Azure CLI package.</span></span>

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" | \
    sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

1. <span data-ttu-id="29a91-111">Microsoft Ubuntu リポジトリ用の暗号化キーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="29a91-111">Import the encryption key for the Microsoft Ubuntu repository.</span></span> <span data-ttu-id="29a91-112">これにより、インストールする Azure CLI パッケージが Microsoft から提供されていることをパッケージ マネージャーで確認できます。</span><span class="sxs-lookup"><span data-stu-id="29a91-112">This will allow the package manager to verify that the Azure CLI package you install comes from Microsoft.</span></span>

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. <span data-ttu-id="29a91-113">Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="29a91-113">Install the Azure CLI.</span></span>

    ```bash
    sudo apt-get install apt-transport-https
    sudo apt-get update && sudo apt-get install azure-cli
    ```

<span data-ttu-id="29a91-114">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29a91-114">::: zone-end</span></span>

<span data-ttu-id="29a91-115">::: zone pivot="macos"</span><span class="sxs-lookup"><span data-stu-id="29a91-115">::: zone pivot="macos"</span></span>

### <a name="macos"></a><span data-ttu-id="29a91-116">macOS</span><span class="sxs-lookup"><span data-stu-id="29a91-116">macOS</span></span>
<span data-ttu-id="29a91-117">ここでは、Homebrew パッケージ マネージャーを使用して macOS に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="29a91-117">Here you will install the Azure CLI on macOS using the Homebrew package manager.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29a91-118">**brew** コマンドを使用できない場合は、Homebrew パッケージ マネージャーのインストールが必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="29a91-118">If the **brew** command is unavailable, you may need to install the Homebrew package manager.</span></span> <span data-ttu-id="29a91-119">詳しくは、[Homebrew の Web サイト](https://brew.sh/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="29a91-119">For details see the [Homebrew website](https://brew.sh/).</span></span>

1. <span data-ttu-id="29a91-120">最新の Azure CLI パッケージを確実に入手するため、brew リポジトリを更新します。</span><span class="sxs-lookup"><span data-stu-id="29a91-120">Update your brew repository to make sure you get the latest Azure CLI package.</span></span>

    ```bash
    brew update
    ```

1. <span data-ttu-id="29a91-121">Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="29a91-121">Install the Azure CLI.</span></span>

    ```bash
    brew install azure-cli
    ```

<span data-ttu-id="29a91-122">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29a91-122">::: zone-end</span></span>

<span data-ttu-id="29a91-123">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="29a91-123">::: zone pivot="windows"</span></span>

### <a name="windows"></a><span data-ttu-id="29a91-124">Windows</span><span class="sxs-lookup"><span data-stu-id="29a91-124">Windows</span></span>
<span data-ttu-id="29a91-125">ここでは、MSI インストーラーを使用して Windows に Azure CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="29a91-125">Here you will install the Azure CLI on Windows using the MSI installer.</span></span>

1. <span data-ttu-id="29a91-126">[https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) に移動して、ブラウザーのセキュリティ ダイアログ ボックスで、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29a91-126">Go to [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows), and in the browser security dialog box, click **Run**.</span></span>
1. <span data-ttu-id="29a91-127">インストーラーで、ライセンス条項に同意し、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29a91-127">In the installer, accept the license terms, and then click **Install**.</span></span>
1. <span data-ttu-id="29a91-128">**[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="29a91-128">In the **User Account Control** dialog, select **Yes**.</span></span>

<span data-ttu-id="29a91-129">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29a91-129">::: zone-end</span></span>

## <a name="running-the-azure-cli"></a><span data-ttu-id="29a91-130">Azure CLI の実行</span><span class="sxs-lookup"><span data-stu-id="29a91-130">Running the Azure CLI</span></span>
<span data-ttu-id="29a91-131">Azure CLI を実行するには、bash シェルを開く (Linux と macOS) か、コマンド プロンプトまたは PowerShell (Windows) から実行します。</span><span class="sxs-lookup"><span data-stu-id="29a91-131">You run the Azure CLI by opening a bash shell (Linux and macOS), or from the command prompt or PowerShell (Windows).</span></span>

1. <span data-ttu-id="29a91-132">Azure CLI を起動し、バージョン チェックを実行して、インストールを確認します。</span><span class="sxs-lookup"><span data-stu-id="29a91-132">Start the Azure CLI and verify your installation by running the version check.</span></span>

    ```bash
    az --version
    ```

<span data-ttu-id="29a91-133">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="29a91-133">::: zone pivot="windows"</span></span>

> [!NOTE]
> <span data-ttu-id="29a91-134">PowerShell から Azure CLI を実行することには、Windows コマンド プロンプトから Azure CLI を実行することに比べて、いくつかの利点があります。</span><span class="sxs-lookup"><span data-stu-id="29a91-134">Running the Azure CLI from PowerShell has some advantages over running the Azure CLI from the Windows command prompt.</span></span> <span data-ttu-id="29a91-135">PowerShell では、コマンド プロンプトよりも多くのタブ補完機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="29a91-135">PowerShell provides additional tab completion features over those available from the command prompt.</span></span> 

<span data-ttu-id="29a91-136">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29a91-136">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="29a91-137">まとめ</span><span class="sxs-lookup"><span data-stu-id="29a91-137">Summary</span></span>
<span data-ttu-id="29a91-138">Azure CLI を使用して Azure リソースを管理できるように、ローカル コンピューターを設定しました。</span><span class="sxs-lookup"><span data-stu-id="29a91-138">You set up your local machines to administer Azure resources with the Azure CLI.</span></span> <span data-ttu-id="29a91-139">Azure CLI をローカルに使用して、コマンドを入力したりスクリプトを実行したりできるようになりました。</span><span class="sxs-lookup"><span data-stu-id="29a91-139">You can now use the Azure CLI locally to enter commands or execute scripts.</span></span> <span data-ttu-id="29a91-140">入力したコマンドは、Azure CLI によって Azure データセンターに転送され、Azure サブスクリプションの内部で実行されます。</span><span class="sxs-lookup"><span data-stu-id="29a91-140">The Azure CLI will forward your commands to the Azure datacenters where they will run inside your Azure subscription.</span></span>