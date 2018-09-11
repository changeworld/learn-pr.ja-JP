<span data-ttu-id="3e8b2-101">ローカル コンピューターでアプリを稼働状態にしたので、次にアプリを Azure に発行します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-101">Now that you've got your app up and running on your local machine, it's time to get it published to Azure.</span></span> 

<span data-ttu-id="3e8b2-102">この演習では、ローカル コンピューターで新しい ASP.NET Web アプリケーションを作成、ビルド、実行します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-102">In this exercise, you will create, build, and run a new ASP.NET web application on your local machine.</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="3e8b2-103">新しいプロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="3e8b2-103">Create a new project</span></span>

### <a name="visual-studio-for-windows"></a><span data-ttu-id="3e8b2-104">Visual Studio for Windows</span><span class="sxs-lookup"><span data-stu-id="3e8b2-104">Visual Studio for Windows</span></span>

<span data-ttu-id="3e8b2-105">最初に、Visual Studio を起動して、ローカル ASP.NET Core Web アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-105">The first step is to start Visual Studio and create a local ASP.NET Core web application.</span></span>

1. <span data-ttu-id="3e8b2-106">Visual Studio の開始ページで、**[ファイル]** を選択し、**[新規]** をクリックして、**[プロジェクト...]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-106">On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.</span></span>

1. <span data-ttu-id="3e8b2-107">**[新しいプロジェクト]** ダイアログ ボックスの左側のウィンドウで、**[Web]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-107">In the **New Project** dialog box, on the left-hand pane, select **Web**.</span></span>

1. <span data-ttu-id="3e8b2-108">中央のウィンドウで、**[ASP.NET Core Web アプリケーション]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-108">In the center pane, click **ASP.NET Core Web Application**.</span></span>

1. <span data-ttu-id="3e8b2-109">ダイアログの下部にある **[名前]** フィールドに、「**Alpine Ski House**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-109">At the bottom of the dialog box, in the **Name** field, enter **Alpine Ski House**.</span></span>

1. <span data-ttu-id="3e8b2-110">新しいソリューションの **[場所]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-110">Select a **Location** for your new solution.</span></span>

1. <span data-ttu-id="3e8b2-111">**[OK]** ボタンをクリックしてプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-111">Click the **OK** button to create your project.</span></span>

1. <span data-ttu-id="3e8b2-112">**[新しい ASP.NET Core Web アプリケーション]** ダイアログ ボックスで、開始用テンプレートの選択肢が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-112">In the **New ASP.NET Core Web Application** dialog box, you will see a selection of starting templates.</span></span> <span data-ttu-id="3e8b2-113">この演習では **[Web アプリケーション]** を選択し、**[OK]** をクリックしてプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-113">For this exercise, select **Web Application**, and then click **OK** to create your project.</span></span>

    ![[新しいプロジェクト] ダイアログ](../media-draft/3-aspnet-templates.png)

    > [!NOTE]
    > <span data-ttu-id="3e8b2-115">このダイアログ ボックスでは、Web 開発の要件に応じて別の開始テンプレートを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-115">You can also select different starting templates in this dialog box depending on your web development requirements.</span></span> <span data-ttu-id="3e8b2-116">ダイアログ ボックスの上部では、ASP.NET Core のバージョンを選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-116">At the top of the dialog box, you are also able to select the version of ASP.NET Core.</span></span> <span data-ttu-id="3e8b2-117">ASP.NET Core 2.0 以降を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-117">You should select ASP.NET Core 2.0 or later.</span></span>

1. <span data-ttu-id="3e8b2-118">これで、新しい ASP.NET Core Web アプリケーション ソリューションが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-118">You should now have your new ASP.NET Core web application solution.</span></span>

    ![[新しいプロジェクト] ダイアログ](../media-draft/3-new-solution.png)

### <a name="visual-studio-mac"></a><span data-ttu-id="3e8b2-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3e8b2-120">Visual Studio Mac</span></span>

1. <span data-ttu-id="3e8b2-121">Visual Studio の開始ページで、**[ファイル]** を選択し、**[新規]** をクリックして、**[プロジェクト...]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-121">On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.</span></span>

1. <span data-ttu-id="3e8b2-122">**[.NET Core]** で **[ASP.NET Core Web アプリ]** を選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-122">Under .**NET Core**, select an **ASP.NET Core Web App**, and then click **Next**.</span></span>

1. <span data-ttu-id="3e8b2-123">**[プロジェクト名]** に「**AlpineSkiHouse**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-123">For the **Project Name**, type **AlpineSkiHouse**.</span></span> <span data-ttu-id="3e8b2-124">ソリューション名にも同じ名前が自動的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-124">This should also autopopulate the solution name.</span></span>

1. <span data-ttu-id="3e8b2-125">プロジェクトに使用するローカル コンピューター上の**場所**を選択します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-125">Select a **location** on your local machine for the project.</span></span>

1. <span data-ttu-id="3e8b2-126">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-126">Click **Create**.</span></span>

## <a name="build-and-test-on-your-local-machine"></a><span data-ttu-id="3e8b2-127">ローカル コンピューター上でビルドしてテストする</span><span class="sxs-lookup"><span data-stu-id="3e8b2-127">Build and test on your local machine</span></span>

<span data-ttu-id="3e8b2-128">次に、ローカル コンピューター上で新しいプロジェクトをビルドしてテストし、Azure に展開する前にローカルでビルドと展開を確認します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-128">Now, let's build and test your new project on your local machine to ensure it builds and deploys locally before deploying to Azure.</span></span>

1. <span data-ttu-id="3e8b2-129">**F5** キーを押してデバッグ モードでアプリを実行するか、または **Ctrl + F5** キーを押してデバッガーをアタッチしないで実行します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-129">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.</span></span>

    ![[新しいプロジェクト] ダイアログ](../media-draft/3-webapp-launch.png)

<span data-ttu-id="3e8b2-131">Visual Studio で IIS Express が開始されて、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-131">Visual Studio starts IIS Express and runs the app.</span></span> <span data-ttu-id="3e8b2-132">Visual Studio で Web プロジェクトが作成されるとき、Web サーバーにはランダムなポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-132">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="3e8b2-133">上の図では、ポート番号は 44381 です。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-133">In the preceding image, the port number is 44381.</span></span> <span data-ttu-id="3e8b2-134">ご自分のアプリを実行すると、おそらく別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-134">When you run the app, you'll likely see a different port number.</span></span>

> [!TIP]
> <span data-ttu-id="3e8b2-135">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動すると、コードを変更してファイルを保存し、ブラウザーを更新して、コードの変更を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-135">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="3e8b2-136">多くの開発者は、非デバッグ モードを使用してすばやくアプリを起動して変更を見ることを好みます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-136">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e8b2-137">Web ページの上部のセクションに、プライバシーと Cookie 使用ポリシーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-137">You might notice the section at the top of the web page that provides a place for your privacy and cookie use policy.</span></span> <span data-ttu-id="3e8b2-138">**[同意する]** を選択して、追跡に同意します。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-138">Select **Accept** to consent to tracking.</span></span> <span data-ttu-id="3e8b2-139">このアプリでは、個人情報は追跡されません。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-139">This app doesn't track personal information.</span></span> <span data-ttu-id="3e8b2-140">テンプレートによって生成されるコードには、一般データ保護規則 (GDPR) を満たすための資産が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-140">The template-generated code includes assets to help meet General Data Protection Regulation (GDPR).</span></span>

## <a name="summary"></a><span data-ttu-id="3e8b2-141">まとめ</span><span class="sxs-lookup"><span data-stu-id="3e8b2-141">Summary</span></span>

<span data-ttu-id="3e8b2-142">ASP.NET サイトを稼働させる最初の手順は、ローカルで作成して実行することです。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-142">The first step to getting your ASP.NET site up and running is to create it and run it locally.</span></span> <span data-ttu-id="3e8b2-143">サイトの作成が済んだので、Azure に展開できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="3e8b2-143">Now that your site is created, you are ready to deploy it to Azure.</span></span>
