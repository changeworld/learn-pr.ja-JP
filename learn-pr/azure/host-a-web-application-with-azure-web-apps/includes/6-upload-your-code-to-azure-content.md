<span data-ttu-id="fc746-101">ここでは、ほとんどまたはまったくダウンタイムを発生させずに、コンテンツをステージングしてデプロイする方法を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="fc746-101">Now, let's see how we can stage our content and deploy with little or no downtime.</span></span>

## <a name="what-is-a-deployment-slot"></a><span data-ttu-id="fc746-102">デプロイ スロットとは</span><span class="sxs-lookup"><span data-stu-id="fc746-102">What is a deployment slot?</span></span>

<span data-ttu-id="fc746-103">デプロイ スロットは、独自のコンテンツ、構成、さらには一意のホスト名を持つ**独立した** Web アプリです。</span><span class="sxs-lookup"><span data-stu-id="fc746-103">A deployment slot is an **independent** web app with its own content, configuration, and even a unique host name.</span></span> <span data-ttu-id="fc746-104">そのため、他の Web アプリと同様に機能します。</span><span class="sxs-lookup"><span data-stu-id="fc746-104">Therefore, it functions like any other web app.</span></span>

> <span data-ttu-id="fc746-105">Azure では、デプロイ スロットの使用に対して追加料金は発生しません。</span><span class="sxs-lookup"><span data-stu-id="fc746-105">Azure doesn't charge you extra for using deployment slots!</span></span>

<span data-ttu-id="fc746-106">各デプロイ スロットには、その一意の URL からアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="fc746-106">Each deployment slot is accessible from its unique URL.</span></span> <span data-ttu-id="fc746-107">たとえば、`BESTBIKE` という名前で**ステージング** デプロイ スロットを追加したとします。</span><span class="sxs-lookup"><span data-stu-id="fc746-107">For instance, let's say I've added a **staging** deployment slot with the name `BESTBIKE`.</span></span> <span data-ttu-id="fc746-108">アプリケーションとデプロイ スロットの URL は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="fc746-108">The URLs for the application and deployment slot are:</span></span>

- https://BESTBIKE.azurewebsites.net/
- https://BESTBIKE-staging.azurewebsites.net/

## <a name="why-are-deployment-slots-useful"></a><span data-ttu-id="fc746-109">デプロイ スロットが便利な理由</span><span class="sxs-lookup"><span data-stu-id="fc746-109">Why are deployment slots useful?</span></span>

<span data-ttu-id="fc746-110">FTP や Web 配置、Git、または別の方法であれ、従来の方法で Web アプリをデプロイすると、次のような弱点がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="fc746-110">Deploying your web app in the traditional way, whether it be via FTP, Web Deploy, Git, or another way, has some weaknesses:</span></span>

- <span data-ttu-id="fc746-111">デプロイが完了すると、アプリケーションの**コールド スタート**を引き起こす Web アプリが再起動する場合があります。</span><span class="sxs-lookup"><span data-stu-id="fc746-111">After the deployment completes, the web app might restart, causing a **cold start** for the application.</span></span> <span data-ttu-id="fc746-112">アプリケーションへの最初の要求が低速になります。</span><span class="sxs-lookup"><span data-stu-id="fc746-112">The first request to the application will be slower.</span></span>

- <span data-ttu-id="fc746-113">場合によっては、Web アプリの*不良*バージョンをデプロイしている可能性があり、クライアントをリリースする前に、それを (運用環境) でテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fc746-113">Potentially, you are deploying a *bad* version of your web app, and you should test it (in production) before releasing it to your client.</span></span>

<span data-ttu-id="fc746-114">ここで、デプロイ スロットが作用します。</span><span class="sxs-lookup"><span data-stu-id="fc746-114">This is where deployment slots come into play.</span></span> <span data-ttu-id="fc746-115">**運用**デプロイ スロットにアクセスしているユーザーに影響を与えずに、**ステージング** デプロイ スロットの Web アプリに変更を加えて、変更をテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-115">You can make changes to your web app to a **staging** deployment slot and test the changes without impacting users who are accessing the **production** deployment slot.</span></span> <span data-ttu-id="fc746-116">新しい機能を運用環境に移動する準備ができたら、ステージング スロットと運用スロットを**スワップ**するだけです。**ダウンタイムは発生しません**。</span><span class="sxs-lookup"><span data-stu-id="fc746-116">When you are ready to move the new features into production, you can just **swap** the staging and production slots with **no downtime**.</span></span>

<span data-ttu-id="fc746-117">デプロイ スロットを使用するもう 1 つのメリットは、運用スロットにスワップする前に、ステージング スロットでアプリケーションを**ウォーム アップ**できることです。</span><span class="sxs-lookup"><span data-stu-id="fc746-117">Another benefit of using deployment slots is that you can **warm up** your application in a staging slot before swapping it into the production slot.</span></span> <span data-ttu-id="fc746-118">**コールド スタート**の遅延と時間のかかる初期化コードを回避できます。</span><span class="sxs-lookup"><span data-stu-id="fc746-118">You will avoid the delays of a **cold start** and the lengthy initialization code.</span></span>

<span data-ttu-id="fc746-119">最後に、アプリケーションの新しいバージョンが期待どおりに動作しないことが判明したら、前のデプロイ スロットに**戻す**ことができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-119">Finally, you can **swap back** to the previous deployment slot if you realize that the new version of your application is not working as you expected.</span></span>

## <a name="what-is-automated-deployment"></a><span data-ttu-id="fc746-120">自動デプロイとは</span><span class="sxs-lookup"><span data-stu-id="fc746-120">What is automated deployment?</span></span>

<span data-ttu-id="fc746-121">自動デプロイ、または継続的インテグレーションは、エンド ユーザーへの影響を最小限に抑えて、新機能とバグ修正を高速かつ反復的なパターンでプッシュ アウトするために使用されるプロセスです。</span><span class="sxs-lookup"><span data-stu-id="fc746-121">Automated deployment, or continuous integration, is a process used to push out new features and bug fixes in a fast and repetitive pattern with minimal impact on end users.</span></span>

<span data-ttu-id="fc746-122">Azure では、複数のソースから直接、自動デプロイをサポートします。</span><span class="sxs-lookup"><span data-stu-id="fc746-122">Azure supports automated deployment directly from several sources.</span></span> <span data-ttu-id="fc746-123">次のオプションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="fc746-123">The following options are available:</span></span>

- <span data-ttu-id="fc746-124">**Visual Studio Team Services (VSTS)**: VSTS にコードをプッシュし、クラウドでコードをビルドし、テストを実行し、コードからリリースを生成し、最後にコードを Azure Web アプリにプッシュできます。</span><span class="sxs-lookup"><span data-stu-id="fc746-124">**Visual Studio Team Services (VSTS)**: You can push your code to VSTS, build your code in the cloud, run the tests, generate a release from the code, and finally, push your code to an Azure web app.</span></span>

- <span data-ttu-id="fc746-125">**GitHub**: Azure では、GitHub から直接、自動デプロイをサポートします。</span><span class="sxs-lookup"><span data-stu-id="fc746-125">**GitHub**: Azure supports automated deployment directly from GitHub.</span></span> <span data-ttu-id="fc746-126">自動デプロイのために GitHub リポジトリを Azure に接続すると、GitHub 上の運用ブランチにプッシュするすべての変更が自動的にデプロイされるようになります。</span><span class="sxs-lookup"><span data-stu-id="fc746-126">When you connect your GitHub repository to Azure for automated deployment, any changes you push to your production branch on GitHub will be automatically deployed for you.</span></span>

- <span data-ttu-id="fc746-127">**Bitbucket**: GitHub との類似性により、Bitbucket を使用して自動デプロイを構成できます。</span><span class="sxs-lookup"><span data-stu-id="fc746-127">**Bitbucket**: With its similarities to GitHub, you can configure an automated deployment with Bitbucket.</span></span>

- <span data-ttu-id="fc746-128">**OneDrive**: 自分の OneDrive アカウント上の任意のファイルを変更するときに、Azure が自動的にそれを検出して、その Web アプリ ファイルのすべての変更を取り消せるように、自分の OneDrive アカウントと Web アプリを接続することができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-128">**OneDrive**: You can connect your OneDrive account with Web Apps so that when you change any file on your OneDrive account, Azure will automatically detect it and pull back any changes on the web app files.</span></span> <span data-ttu-id="fc746-129">これは、静的な Web サイトに最適なオプションです。</span><span class="sxs-lookup"><span data-stu-id="fc746-129">This is a great option for static websites.</span></span> <span data-ttu-id="fc746-130">Azure でのすべての変更が円滑かつ瞬時に反映されるローカル ファイル システムを扱う感覚が得られます。</span><span class="sxs-lookup"><span data-stu-id="fc746-130">It will give you the feeling that you are dealing with a local file system that reflects any changes on Azure, smoothly and instantly.</span></span>

- <span data-ttu-id="fc746-131">**Dropbox**: OneDrive と同様に、自分の Web アプリ ファイルを Dropbox アカウントでホストして、それらを Azure Web アプリに自動的にプッシュしてデプロイすることができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-131">**Dropbox**: Similar to OneDrive, you can host your web app files in a Dropbox account and have them automatically push and be deployed over an Azure web app.</span></span>

- <span data-ttu-id="fc746-132">**外部リポジトリ**: 外部の Git リポジトリを使用して、自動デプロイを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-132">**External repository**: You can configure automated deployment with any external Git repository.</span></span>

### <a name="non-automated-deployment-to-azure"></a><span data-ttu-id="fc746-133">Azure への非自動デプロイ</span><span class="sxs-lookup"><span data-stu-id="fc746-133">Non-automated deployment to Azure</span></span>

<span data-ttu-id="fc746-134">手動でコードを Azure にプッシュするために使用できるいくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="fc746-134">There are a few options that you can use to manually push your code to Azure:</span></span>

- <span data-ttu-id="fc746-135">**FTP/S**: FTP または FTPS は、コードを任意のホスティング環境にプッシュする従来の方法です。</span><span class="sxs-lookup"><span data-stu-id="fc746-135">**FTP/S**: FTP or FTPS is a traditional way of pushing your code to any hosting environment.</span></span> <span data-ttu-id="fc746-136">現在は推奨されていませんが、引き続き利用することができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-136">Despite the fact that it is not recommended anymore, you can still make use of it.</span></span>

- <span data-ttu-id="fc746-137">**Web 配置/IDE**: Visual Studio IDE を使用して Azure に Web アプリを発行することができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-137">**Web Deploy/IDE**: You can use the Visual Studio IDE to publish your web app to Azure.</span></span> <span data-ttu-id="fc746-138">Visual Studio の発行メカニズムでは、Web 配置テクノロジを利用してコードを Azure にプッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="fc746-138">The Visual Studio publishing mechanism can make use of Web Deploy technology to push your code to Azure.</span></span>

- <span data-ttu-id="fc746-139">**Git**: Azure では、アプリケーション用に**ローカル Git リポジトリ**が維持されます。</span><span class="sxs-lookup"><span data-stu-id="fc746-139">**Git**: Azure maintains a **local Git repository** for your application.</span></span> <span data-ttu-id="fc746-140">単純にコードをそこに直接**コミット**できます。</span><span class="sxs-lookup"><span data-stu-id="fc746-140">You can simply **commit** your code directly to it.</span></span>

> <span data-ttu-id="fc746-141">このモジュールでは、Git を使用して非自動デプロイを実行します。</span><span class="sxs-lookup"><span data-stu-id="fc746-141">In this module, we are going to perform a non-automated deployment using Git.</span></span>

## <a name="summary"></a><span data-ttu-id="fc746-142">まとめ</span><span class="sxs-lookup"><span data-stu-id="fc746-142">Summary</span></span>

<span data-ttu-id="fc746-143">Azure では、現在のワークフローにより適合するように、コードをアップロードする複数の方法が提供されています。</span><span class="sxs-lookup"><span data-stu-id="fc746-143">Azure provides multiple ways to upload your code to help it better fit in with your current workflow.</span></span> <span data-ttu-id="fc746-144">デプロイ スロットを使用して、ユーザーのダウンタイムを防ぐこともできます。</span><span class="sxs-lookup"><span data-stu-id="fc746-144">You can also use deployment slots to help prevent downtime for your users.</span></span>
