<span data-ttu-id="1ca4d-101">ここでは、ほとんどまたはまったくダウンタイムを発生させずに、コンテンツをステージングしてデプロイする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-101">Now, let's see how we can stage our content and deploy with little or no downtime.</span></span>

## <a name="what-is-a-deployment-slot"></a><span data-ttu-id="1ca4d-102">デプロイ スロットとは</span><span class="sxs-lookup"><span data-stu-id="1ca4d-102">What is a deployment slot?</span></span>

<span data-ttu-id="1ca4d-103">デプロイ スロットは、独自のコンテンツ、構成、さらには一意のホスト名を持つ、App Service の**独立した**アプリです。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-103">A deployment slot is an **independent** app in App Service with its own content, configuration, and even a unique host name.</span></span> <span data-ttu-id="1ca4d-104">そのため、App Service 内の他のアプリと同様に機能します。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-104">Therefore, it functions like any other app in App Service.</span></span>

<span data-ttu-id="1ca4d-105">各デプロイ スロットには、その一意の URL からアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-105">Each deployment slot is accessible from its unique URL.</span></span> <span data-ttu-id="1ca4d-106">たとえば、`BESTBIKE` という名前で**ステージング** デプロイ スロットを追加したとします。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-106">For instance, let's say I've added a **staging** deployment slot with the name `BESTBIKE`.</span></span> <span data-ttu-id="1ca4d-107">アプリケーションとデプロイ スロットの URL は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-107">The URLs for the application and deployment slot are:</span></span>

- https://BESTBIKE.azurewebsites.net/
- https://BESTBIKE-staging.azurewebsites.net/

## <a name="why-are-deployment-slots-useful"></a><span data-ttu-id="1ca4d-108">デプロイ スロットが便利な理由</span><span class="sxs-lookup"><span data-stu-id="1ca4d-108">Why are deployment slots useful?</span></span>

<span data-ttu-id="1ca4d-109">FTP や Web 配置、Git、または別の方法であれ、従来の方法で Web アプリをデプロイすると、次のような弱点がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-109">Deploying your web app in the traditional way, whether it be via FTP, Web Deploy, Git, or another way, has some weaknesses:</span></span>

- <span data-ttu-id="1ca4d-110">デプロイが完了すると、アプリケーションの**コールド スタート**を引き起こすアプリが再起動する場合があります。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-110">After the deployment completes, the app might restart, causing a **cold start** for the application.</span></span> <span data-ttu-id="1ca4d-111">アプリケーションへの最初の要求が低速になります。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-111">The first request to the application will be slower.</span></span>

- <span data-ttu-id="1ca4d-112">場合によっては、アプリの "*不良*" バージョンをデプロイしている可能性があり、クライアントをリリースする前に、それを (運用環境) でテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-112">Potentially, you are deploying a *bad* version of your app, and you should test it (in production) before releasing it to your client.</span></span>

<span data-ttu-id="1ca4d-113">ここで、デプロイ スロットが作用します。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-113">This is where deployment slots come into play.</span></span> <span data-ttu-id="1ca4d-114">**運用**デプロイ スロットにアクセスしているユーザーに影響を与えずに、**ステージング** デプロイ スロットのアプリに変更を加えて、変更をテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-114">You can make changes to your app to a **staging** deployment slot and test the changes without impacting users who are accessing the **production** deployment slot.</span></span> <span data-ttu-id="1ca4d-115">新しい機能を運用環境に移動する準備ができたら、ステージング スロットと運用スロットを**スワップ**するだけです。**ダウンタイムは発生しません**。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-115">When you are ready to move the new features into production, you can just **swap** the staging and production slots with **no downtime**.</span></span>

<span data-ttu-id="1ca4d-116">デプロイ スロットを使用するもう 1 つのメリットは、運用スロットにスワップする前に、ステージング スロットでアプリケーションを**ウォーム アップ**できることです。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-116">Another benefit of using deployment slots is that you can **warm up** your application in a staging slot before swapping it into the production slot.</span></span> <span data-ttu-id="1ca4d-117">**コールド スタート**の遅延と時間のかかる初期化コードを回避できます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-117">You will avoid the delays of a **cold start** and the lengthy initialization code.</span></span>

<span data-ttu-id="1ca4d-118">最後に、アプリケーションの新しいバージョンが期待どおりに動作しないことが判明したら、前のデプロイ スロットに**戻す**ことができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-118">Finally, you can **swap back** to the previous deployment slot if you realize that the new version of your application is not working as you expected.</span></span>

## <a name="what-is-automated-deployment"></a><span data-ttu-id="1ca4d-119">自動デプロイとは</span><span class="sxs-lookup"><span data-stu-id="1ca4d-119">What is automated deployment?</span></span>

<span data-ttu-id="1ca4d-120">自動デプロイ、または継続的インテグレーションは、エンド ユーザーへの影響を最小限に抑えて、新機能とバグ修正を高速かつ反復的なパターンでプッシュ アウトするために使用されるプロセスです。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-120">Automated deployment, or continuous integration, is a process used to push out new features and bug fixes in a fast and repetitive pattern with minimal impact on end users.</span></span>

<span data-ttu-id="1ca4d-121">Azure では、複数のソースから直接、自動デプロイをサポートします。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-121">Azure supports automated deployment directly from several sources.</span></span> <span data-ttu-id="1ca4d-122">次のオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-122">The following options are available:</span></span>

- <span data-ttu-id="1ca4d-123">**Visual Studio Team Services (VSTS)**: VSTS にコードをプッシュし、クラウドでコードをビルドし、テストを実行し、コードからリリースを生成し、最後にコードを Azure Web アプリにプッシュできます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-123">**Visual Studio Team Services (VSTS)**: You can push your code to VSTS, build your code in the cloud, run the tests, generate a release from the code, and finally, push your code to an Azure Web App.</span></span>

- <span data-ttu-id="1ca4d-124">**GitHub**: Azure では、GitHub から直接、自動デプロイをサポートします。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-124">**GitHub**: Azure supports automated deployment directly from GitHub.</span></span> <span data-ttu-id="1ca4d-125">自動デプロイのために GitHub リポジトリを Azure に接続すると、GitHub 上の運用ブランチにプッシュするすべての変更が自動的にデプロイされるようになります。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-125">When you connect your GitHub repository to Azure for automated deployment, any changes you push to your production branch on GitHub will be automatically deployed for you.</span></span>

- <span data-ttu-id="1ca4d-126">**Bitbucket**: GitHub との類似性により、Bitbucket を使用して自動デプロイを構成できます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-126">**Bitbucket**: With its similarities to GitHub, you can configure an automated deployment with Bitbucket.</span></span>

- <span data-ttu-id="1ca4d-127">**OneDrive**: 自分の OneDrive アカウント上のいずれかのファイルを変更すると、Azure によって自動的に検出されて、その Web アプリ ファイルのすべての変更が取得されるように、自分の OneDrive アカウントと App Service を接続することができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-127">**OneDrive**: You can connect your OneDrive account with App Service so that when you change any file on your OneDrive account, Azure will automatically detect it and pull in any changes on the web app files.</span></span> <span data-ttu-id="1ca4d-128">これは、静的な Web サイトに最適なオプションです。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-128">This is a great option for static websites.</span></span> <span data-ttu-id="1ca4d-129">Azure でのすべての変更が円滑かつ瞬時に反映されるローカル ファイル システムを扱う感覚が得られます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-129">It will give you the feeling that you are dealing with a local file system that reflects any changes on Azure, smoothly and instantly.</span></span>

- <span data-ttu-id="1ca4d-130">**Dropbox**: OneDrive と同様に、自分の Web アプリ ファイルを Dropbox アカウントでホストして、それらを Web アプリに自動的にプッシュしてデプロイすることができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-130">**Dropbox**: Similar to OneDrive, you can host your web app files in a Dropbox account and have them automatically push and be deployed over a web app.</span></span>

- <span data-ttu-id="1ca4d-131">**外部リポジトリ**: 外部の Git リポジトリを使用して、自動デプロイを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-131">**External repository**: You can configure automated deployment with any external Git repository.</span></span>

### <a name="non-automated-deployment-to-azure"></a><span data-ttu-id="1ca4d-132">Azure への非自動デプロイ</span><span class="sxs-lookup"><span data-stu-id="1ca4d-132">Non-automated deployment to Azure</span></span>

<span data-ttu-id="1ca4d-133">手動でコードを Azure にプッシュするために使用できるいくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-133">There are a few options that you can use to manually push your code to Azure:</span></span>

- <span data-ttu-id="1ca4d-134">**FTP/S**: FTP または FTPS は、コードを任意のホスティング環境にプッシュする従来の方法です。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-134">**FTP/S**: FTP or FTPS is a traditional way of pushing your code to any hosting environment.</span></span> <span data-ttu-id="1ca4d-135">現在は推奨されていませんが、引き続き利用することができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-135">Despite the fact that it is not recommended anymore, you can still make use of it.</span></span>

- <span data-ttu-id="1ca4d-136">**Web 配置/IDE**: Visual Studio IDE を使用して Azure に Web アプリを発行することができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-136">**Web Deploy/IDE**: You can use the Visual Studio IDE to publish your web app to Azure.</span></span> <span data-ttu-id="1ca4d-137">Visual Studio の発行メカニズムでは、Web 配置テクノロジを利用してコードを Azure にプッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-137">The Visual Studio publishing mechanism can make use of Web Deploy technology to push your code to Azure.</span></span>

- <span data-ttu-id="1ca4d-138">**Git**: Azure では、アプリケーション用に**ローカル Git リポジトリ**が維持されます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-138">**Git**: Azure maintains a **local Git repository** for your application.</span></span> <span data-ttu-id="1ca4d-139">コードをそこに直接**コミット**できます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-139">You can **commit** your code directly to it.</span></span>

> <span data-ttu-id="1ca4d-140">このモジュールでは、Git を使用して非自動デプロイを実行します。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-140">In this module, we are going to perform a non-automated deployment using Git.</span></span>

## <a name="summary"></a><span data-ttu-id="1ca4d-141">まとめ</span><span class="sxs-lookup"><span data-stu-id="1ca4d-141">Summary</span></span>

<span data-ttu-id="1ca4d-142">Azure では、現在のワークフローにより適合するように、コードをアップロードする複数の方法が提供されています。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-142">Azure provides multiple ways to upload your code to help it better fit in with your current workflow.</span></span> <span data-ttu-id="1ca4d-143">デプロイ スロットを使用して、ユーザーのダウンタイムを防ぐこともできます。</span><span class="sxs-lookup"><span data-stu-id="1ca4d-143">You can also use deployment slots to help prevent downtime for your users.</span></span>