<span data-ttu-id="80ef9-101">これでローカルで実行中の ASP.NET Core Web アプリケーションが作成されました。</span><span class="sxs-lookup"><span data-stu-id="80ef9-101">You now have a local running ASP.NET Core web application.</span></span> <span data-ttu-id="80ef9-102">この演習では、自分のアプリケーションを Azure App Service に発行します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-102">In this exercise, you'll publish your application to Azure App Service.</span></span>

### <a name="visual-studio-for-windows"></a><span data-ttu-id="80ef9-103">Visual Studio for Windows</span><span class="sxs-lookup"><span data-stu-id="80ef9-103">Visual Studio for Windows</span></span>

1. <span data-ttu-id="80ef9-104">ソリューション エクスプローラーで、プロジェクトを右クリックして **[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-104">In Solution Explorer, right-click your project and select **Publish**.</span></span>

1. <span data-ttu-id="80ef9-105">表示されたダイアログ ボックスの左側で、発行先に **[App Service]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-105">In the dialog box that appears, on the left, choose **App Service** as your publish target.</span></span>  <span data-ttu-id="80ef9-106">右側で、**[新規作成]** を選択して、新しい App Service を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-106">On the right, select **Create New** to create a new app service.</span></span>

1. <span data-ttu-id="80ef9-107">**[発行]** ボタンをクリックして、新しい App Service を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-107">Click the **Publish** button to create your new app service.</span></span>

> [!NOTE]
> <span data-ttu-id="80ef9-108">アプリをデプロイするときに、新しい App Service プランを作成するか、または引き続き既存のプランにアプリを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="80ef9-108">When you deploy your apps, you can create a new App Service plan or you can continue to add apps to an existing plan.</span></span> <span data-ttu-id="80ef9-109">この演習では、新しい **Azure App Service** を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-109">For this exercise, you will create a new **Azure app service**.</span></span>

### <a name="visual-studio-mac"></a><span data-ttu-id="80ef9-110">Visual Studio Mac</span><span class="sxs-lookup"><span data-stu-id="80ef9-110">Visual Studio Mac</span></span>

1. <span data-ttu-id="80ef9-111">ソリューション エクスプローラーでプロジェクトを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef9-111">In Solution Explorer, right-click your project.</span></span>

1. <span data-ttu-id="80ef9-112">**[発行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-112">Select **Publish**.</span></span>

1. <span data-ttu-id="80ef9-113">**[Azure に発行する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef9-113">Click **Publish to Azure**.</span></span> <span data-ttu-id="80ef9-114">これにより自分の Azure アカウントに接続されます。</span><span class="sxs-lookup"><span data-stu-id="80ef9-114">This will connect to your Azure account.</span></span> <span data-ttu-id="80ef9-115">接続して現在のサービスが一覧表示されるまで数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="80ef9-115">It may take a few minutes to connect and list your current services.</span></span>

1. <span data-ttu-id="80ef9-116">**[新規]** をクリックして、新しい App Service を作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-116">Click **New** to create a new app service.</span></span>

## <a name="configure-your-new-azure-app-service"></a><span data-ttu-id="80ef9-117">新しい Azure App Service を構成する</span><span class="sxs-lookup"><span data-stu-id="80ef9-117">Configure your new Azure App Service</span></span>

1. <span data-ttu-id="80ef9-118">**[App Service の作成]** ダイアログ ボックスで、**[アカウントの追加]** をクリックし、Azure サブスクリプションにサインインします。</span><span class="sxs-lookup"><span data-stu-id="80ef9-118">In the **Create App Service dialog** box, click **Add an account**, and sign in to your Azure subscription.</span></span> <span data-ttu-id="80ef9-119">既にサインインしている場合は、目的のサブスクリプションを含んだアカウントをドロップダウン メニューから選択します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-119">If you're already signed in, select the account containing the desired subscription from the drop-down menu.</span></span>

1. <span data-ttu-id="80ef9-120">App Service プランに関する必要な情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-120">Enter the required information about your App Service plan.</span></span>

    ![App Service を作成する](../media-draft/5-CreateAppService.png)

    <span data-ttu-id="80ef9-122">**[App Service の作成]** ダイアログ ボックスで、次の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-122">In the **Create App Service** dialog box, you will provide the following information:</span></span>

    - <span data-ttu-id="80ef9-123">**アプリ名**: これはご自分のアプリケーションの名前です。</span><span class="sxs-lookup"><span data-stu-id="80ef9-123">**App Name**: This is the name of your application.</span></span>  <span data-ttu-id="80ef9-124">これにより、発行されたアプリケーションの URL が決まります。つまり、 https://_AppName_.azurewebsites.net になります。</span><span class="sxs-lookup"><span data-stu-id="80ef9-124">This will determine the URL of the published application, which will be https://_AppName_.azurewebsites.net.</span></span>  <span data-ttu-id="80ef9-125">一意の値を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="80ef9-125">It has to be a unique value.</span></span> <span data-ttu-id="80ef9-126">そのため、予約されていない名前を探すため、いくつか異なる名前を試す必要があります。</span><span class="sxs-lookup"><span data-stu-id="80ef9-126">Therefore, you will have to try out some different names to find one that is not reserved.</span></span>

    - <span data-ttu-id="80ef9-127">**サブスクリプション**: App Service をデプロイする Azure サブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="80ef9-127">**Subscription**: The Azure subscription you wish to deploy the app service to.</span></span>

    - <span data-ttu-id="80ef9-128">**リソース グループ:** リソース グループの横にある **[新規]** ボタンをクリックして、リソース グループの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-128">**Resource Group:** Click the **New...** button next to the resource group, and enter a unique name for the resource group.</span></span>

        ![新しいリソース グループ](../media-draft/5-NewResourceGroup.png)

    - <span data-ttu-id="80ef9-130">**ホスティング プラン:** ホスティング プランは、アプリをホストする Web サーバー ファームの場所、サイズ、機能を規定します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-130">**Hosting Plan:** The hosting plan specifies the location, size, and features of the web server farm that hosts your app.</span></span> <span data-ttu-id="80ef9-131">既存のホスティング プランを選択するか、新しいホスティング プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-131">You can select an existing hosting plan or create a new one.</span></span> <span data-ttu-id="80ef9-132">この演習では、新しいホスティング プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-132">For this exercise, you will create a new one.</span></span>

        <span data-ttu-id="80ef9-133">ホスティング プランの横にある **[新規]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="80ef9-133">Click the **New...** button next to the hosting plan.</span></span>

        ![新しいホスティング プラン](../media-draft/5-NewHostingPlan.png)

        <span data-ttu-id="80ef9-135">ホスティング プランに名前を付けて、場所とサイズを指定します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-135">Give your hosting plan a name, and then specify the location and the size.</span></span>  
        
        > [!NOTE]
        > <span data-ttu-id="80ef9-136">異なる場所とサイズを指定すると、サービスのコストに影響します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-136">Specifying different locations and sizes will impact the cost of the service.</span></span> <span data-ttu-id="80ef9-137">この演習では、コストが発生しない **Free** サイズを指定することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="80ef9-137">For the exercise, it is recommended that you specify the **Free** size, which will ensure you won't incur any costs.</span></span>

    - <span data-ttu-id="80ef9-138">**Application Insights:** アプリケーションで Application Insights を使用するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-138">**Application Insights:** Specifies if you want to use Application Insights for your application.</span></span> <span data-ttu-id="80ef9-139">この演習では、**[なし]** を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="80ef9-139">For this exercise, we recommend that you choose **None.**</span></span>

1. <span data-ttu-id="80ef9-140">**[作成]** ボタンをクリックして App Service のプロビジョニングを開始します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-140">Click the **Create** button to start provisioning your app service.</span></span> <span data-ttu-id="80ef9-141">デプロイの進行状況が表示されます。</span><span class="sxs-lookup"><span data-stu-id="80ef9-141">You will see progress as it deploys:</span></span>

    ![デプロイの進行状況](../media-draft/5-DeployProgress.png)

1. <span data-ttu-id="80ef9-143">App Service がプロビジョニングされると、Visual Studio によって Web アプリがビルドされデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="80ef9-143">Once the app service is provisioned, Visual Studio will build and deploy your web app.</span></span>  <span data-ttu-id="80ef9-144">Visual Studio のビルド出力で、デプロイの出力を確認できます。</span><span class="sxs-lookup"><span data-stu-id="80ef9-144">In your Visual Studio build output, you can see the output of the deployment.</span></span>

    ![発行の結果](../media-draft/5-PublishResult.png)

1. <span data-ttu-id="80ef9-146">お疲れさまでした。ASP.NET Core Web アプリケーションが発行されライブになりました。</span><span class="sxs-lookup"><span data-stu-id="80ef9-146">Congratulations, your ASP.NET Core web application is now published and live.</span></span> <span data-ttu-id="80ef9-147">ビルド出力と、Visual Studio の発行ページでも、自分のサイトの最終的な URL が確認できるようになります。</span><span class="sxs-lookup"><span data-stu-id="80ef9-147">You will be able to see the final URL for your site in the build output and also the on publishing page in Visual Studio.</span></span>

    ![発行の結果](../media-draft/5-PublishPage.png)

1. <span data-ttu-id="80ef9-149">Web サイトをテストするには、表示された URL に移動します。</span><span class="sxs-lookup"><span data-stu-id="80ef9-149">To test your website, go to the URL indicated.</span></span>

    ![ライブ サイト](../media-draft/5-WebPageLive.png)

## <a name="summary"></a><span data-ttu-id="80ef9-151">まとめ</span><span class="sxs-lookup"><span data-stu-id="80ef9-151">Summary</span></span>

<span data-ttu-id="80ef9-152">この演習では、Azure App Service をプロビジョニングして、ASP.NET Core Web アプリケーションをデプロイするプロセスを実際に行ってみました。</span><span class="sxs-lookup"><span data-stu-id="80ef9-152">This exercise demonstrated the process of provisioning an Azure app service and deploying an ASP.NET Core web application.</span></span> <span data-ttu-id="80ef9-153">これでライブ Web アプリが作成されました。</span><span class="sxs-lookup"><span data-stu-id="80ef9-153">You now have a live web app.</span></span>
