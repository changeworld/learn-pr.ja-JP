<span data-ttu-id="01041-101">この演習では、Visual Studio を Windows または macOS コンピューターにインストールします。</span><span class="sxs-lookup"><span data-stu-id="01041-101">In this exercise, you will install Visual Studio on either your Windows or your macOS computer.</span></span> <span data-ttu-id="01041-102">Windows には、開発ワークロードをインストールします。</span><span class="sxs-lookup"><span data-stu-id="01041-102">On Windows, the Azure development workload will need to be installed.</span></span> <span data-ttu-id="01041-103">そして Visual Studio for Mac では、組み込みの接続済みサービス ワークフローにより、Azure App Service 用にアプリを構築します。</span><span class="sxs-lookup"><span data-stu-id="01041-103">And on Visual Studio for Mac, the built-in Connected Services workflow will enable you to build apps for Azure App Service.</span></span> <span data-ttu-id="01041-104">最後には、アプリケーションを作成する準備が完了し、それを Azure に公開できるようになります。</span><span class="sxs-lookup"><span data-stu-id="01041-104">At the end, you will be ready to start creating applications and publishing them to Azure.</span></span>

## <a name="exercise-steps"></a><span data-ttu-id="01041-105">演習の手順</span><span class="sxs-lookup"><span data-stu-id="01041-105">Exercise steps</span></span>

<span data-ttu-id="01041-106">Visual Studio を Windows と macOS にインストールする場合には、若干の違いがあります。</span><span class="sxs-lookup"><span data-stu-id="01041-106">There are slight differences in installing Visual Studio between Windows and macOS.</span></span> <span data-ttu-id="01041-107">次のセクションで、これらの違いを説明します。</span><span class="sxs-lookup"><span data-stu-id="01041-107">The following sections outline these differences.</span></span>

### <a name="windows"></a><span data-ttu-id="01041-108">Windows</span><span class="sxs-lookup"><span data-stu-id="01041-108">Windows</span></span>

1. <span data-ttu-id="01041-109">Visual Studio のインストーラーを、 https://visualstudio.microsoft.com/downloads/ からダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="01041-109">Download the Visual Studio installer from https://visualstudio.microsoft.com/downloads/.</span></span>

1. <span data-ttu-id="01041-110">インストーラーを実行すると、ワークロード ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="01041-110">Run the installer and it will open the Workloads window.</span></span>

1. <span data-ttu-id="01041-111">**[Azure の開発]** ワークロードを選びます。</span><span class="sxs-lookup"><span data-stu-id="01041-111">Choose the **Azure development** workload.</span></span>

    <span data-ttu-id="01041-112">次のスクリーンショットは、Visual Studio で Azure を開発するために Visual Studio インストーラーのワークロードが選択されているところを示しています。</span><span class="sxs-lookup"><span data-stu-id="01041-112">The following screenshot shows the Visual Studio Installer workload selected to allow Azure development within Visual Studio.</span></span>

    ![Azure 開発ワークロードが強調表示された Visual Studio インストーラーのスクリーンショット。](../media/5-select-azure-workload.png)

1. <span data-ttu-id="01041-114">(省略可能) Azure で使用する Web アプリケーションを作成するために、ASP.NET および Web 開発ワークロードをインストールします。</span><span class="sxs-lookup"><span data-stu-id="01041-114">(Optional) Install the ASP.NET and web development workload to be ready to create web applications for Azure.</span></span>

1. <span data-ttu-id="01041-115">**[インストール]** をクリックし、Visual Studio がインストールされるのを待機します。</span><span class="sxs-lookup"><span data-stu-id="01041-115">Click **Install**, and wait for Visual Studio to install.</span></span>

1. <span data-ttu-id="01041-116">インストールが完了したら、Visual Studio を開きます。</span><span class="sxs-lookup"><span data-stu-id="01041-116">When the installation is complete, open Visual Studio.</span></span>

1. <span data-ttu-id="01041-117">Visual Studio の [表示] メニューに移動し、**[Cloud Explorer]** オプションがあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="01041-117">Go to the View menu in Visual Studio and make sure you have the **Cloud Explorer** option.</span></span>

    <span data-ttu-id="01041-118">次のスクリーン ショットでは、Azure 開発ワークロードがインストールされている場合に存在する Cloud Explorer のメニュー オプションを示します。</span><span class="sxs-lookup"><span data-stu-id="01041-118">The following screenshot shows the Cloud Explorer menu option that will be present if you have the Azure development workload installed.</span></span>

    ![Cloud Explorer のメニュー オプションが強調表示された Visual Studio の [表示] メニューのスクリーンショット。](../media/5-verify-cloud-explorer.png)

### <a name="macos"></a><span data-ttu-id="01041-120">macOS</span><span class="sxs-lookup"><span data-stu-id="01041-120">macOS</span></span>

1. <span data-ttu-id="01041-121">https://visualstudio.microsoft.com/ に移動し、Visual Studio for Mac インストーラーをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="01041-121">Go to https://visualstudio.microsoft.com/ and download the Visual Studio for Mac installer.</span></span>

1. <span data-ttu-id="01041-122">VisualStudioInstaller.dmg ファイルをクリックしてインストーラーをマウントし、ロゴをダブルクリックして実行します。</span><span class="sxs-lookup"><span data-stu-id="01041-122">Click the VisualStudioInstaller.dmg file to mount the installer and then run it by double-clicking the logo.</span></span>

1. <span data-ttu-id="01041-123">表示されたらプライバシーとライセンス条項に同意します。</span><span class="sxs-lookup"><span data-stu-id="01041-123">Acknowledge the Privacy and License terms when presented.</span></span>

1. <span data-ttu-id="01041-124">インストーラーにより、どのコンポーネントをインスト―ルするか尋ねられます。</span><span class="sxs-lookup"><span data-stu-id="01041-124">The installer will ask which components you wish to install.</span></span> <span data-ttu-id="01041-125">Azure のコンポーネントは既に Visual Studio for Mac の一部ですが、Azure 用の Web エクスペリエンスを開発するには、**.NET Core** プラットフォームをインストールすることが推奨されます。</span><span class="sxs-lookup"><span data-stu-id="01041-125">Azure components are already part of Visual Studio for Mac, but it is recommended to install the **.NET Core** platform to develop web experiences for Azure.</span></span>

    <span data-ttu-id="01041-126">次のスクリーン ショットは、Visual Studio for Mac で Azure を開発する機能を追加するために必要な .NET Core プラットフォームを示しています。</span><span class="sxs-lookup"><span data-stu-id="01041-126">The following screenshot shows the .NET Core platform required to add Azure development capabilities to Visual Studio for Mac.</span></span>

    ![選択されている .NET Core プラットフォーム オプションが強調表示された Visual Studio for Mac インストーラーのスクリーン ショット。](../media/5-vsmac-install-net-core.png)

1. <span data-ttu-id="01041-128">選択に問題がなければ、**[Install and Update]** \(インストールして更新\) をクリックし、インストーラーが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="01041-128">Click **Install and Update** once you are happy with the selections, and wait for the installer to complete.</span></span>

1. <span data-ttu-id="01041-129">必要なアクセス許可を昇格するように求められた場合、それの実行に管理者の資格情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="01041-129">If you are prompted to elevate the permissions needed, use your administrator credentials to do so.</span></span>

1. <span data-ttu-id="01041-130">インストーラーが完了したら、Visual Studio for Mac を開始します。</span><span class="sxs-lookup"><span data-stu-id="01041-130">Once the installer is complete, start Visual Studio for Mac.</span></span>
