<span data-ttu-id="9322a-101">クラウド コンピューティングに関しては 3 つの主要なカテゴリがあります。</span><span class="sxs-lookup"><span data-stu-id="9322a-101">When talking about cloud computing, there are three major categories.</span></span> <span data-ttu-id="9322a-102">これらは会話、ドキュメント、トレーニングで使用されるため、理解しておくことが重要です。</span><span class="sxs-lookup"><span data-stu-id="9322a-102">It's important to understand them because they are used in conversation, documentation, and training.</span></span>

<span data-ttu-id="9322a-103">3 つの一般的なクラウド サービス カテゴリは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="9322a-103">Here are the three common cloud service categories:</span></span>

- <span data-ttu-id="9322a-104">サービスとしてのインフラストラクチャ (IaaS)</span><span class="sxs-lookup"><span data-stu-id="9322a-104">Infrastructure as a Service (IaaS)</span></span>
- <span data-ttu-id="9322a-105">サービスとしてのプラットフォーム (PaaS)</span><span class="sxs-lookup"><span data-stu-id="9322a-105">Platform as a Service (PaaS)</span></span>
- <span data-ttu-id="9322a-106">サービスとしてのソフトウェア (SaaS)</span><span class="sxs-lookup"><span data-stu-id="9322a-106">Software as a Service (SaaS)</span></span>

<span data-ttu-id="9322a-107">注意する必要があるのは、これらのカテゴリが相互に階層を成しているということです。</span><span class="sxs-lookup"><span data-stu-id="9322a-107">One thing to pay attention to is that these categories are layers on top of each other.</span></span> <span data-ttu-id="9322a-108">たとえば、PaaS は抽象化のレベルを提供することによって IaaS の上にレイヤーを追加します。</span><span class="sxs-lookup"><span data-stu-id="9322a-108">For example, PaaS adds a layer on top of IaaS by providing a level of abstraction.</span></span> <span data-ttu-id="9322a-109">抽象化には、細部が隠ぺいされるので開発者は気にする必要がなく迅速にコーディングできるというメリットがあります。</span><span class="sxs-lookup"><span data-stu-id="9322a-109">The abstraction has the benefit of hiding the details that you may not care about so that you can get to coding quicker.</span></span> <span data-ttu-id="9322a-110">ただし、その対価の 1 つとして、基になるハードウェアについて開発者が制御できる範囲は狭くなります。</span><span class="sxs-lookup"><span data-stu-id="9322a-110">However, one cost of that is that you have less control over the underlying hardware.</span></span> <span data-ttu-id="9322a-111">次の図では、ご自分で、およびサービス プロバイダーが管理する、それぞれのクラウド サービスのカテゴリのリソース一覧を示しています。</span><span class="sxs-lookup"><span data-stu-id="9322a-111">The following illustration shows a list of resources that you manage and that your service provider manages in each cloud service category.</span></span>

![クラウド サービスの各カテゴリの抽象化レベルを示す図。](../media/5-layer-diagram.png)


### <a name="infrastructure-as-a-service-iaas"></a><span data-ttu-id="9322a-113">サービスとしてのインフラストラクチャ (IaaS)</span><span class="sxs-lookup"><span data-stu-id="9322a-113">Infrastructure as a service (IaaS)</span></span>

<span data-ttu-id="9322a-114">サービスとしてのインフラストラクチャは最も柔軟なクラウド サービスのカテゴリであり、アプリケーションを実行するハードウェアを完全に制御できるようにすることが目的です。</span><span class="sxs-lookup"><span data-stu-id="9322a-114">Infrastructure as a Service is the most flexible category of cloud services and aims to give you complete control over the hardware that will run your application.</span></span> <span data-ttu-id="9322a-115">IaaS では、ハードウェアを購入するのではなくレンタルします。</span><span class="sxs-lookup"><span data-stu-id="9322a-115">Instead of buying hardware, with IaaS, you're renting it.</span></span>

<span data-ttu-id="9322a-116">IaaS の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="9322a-116">Here are some examples of IaaS:</span></span>

- <span data-ttu-id="9322a-117">仮想マシン</span><span class="sxs-lookup"><span data-stu-id="9322a-117">Virtual machines</span></span>
- <span data-ttu-id="9322a-118">ファイアウォール</span><span class="sxs-lookup"><span data-stu-id="9322a-118">Firewalls</span></span>
- <span data-ttu-id="9322a-119">ハード ディスク</span><span class="sxs-lookup"><span data-stu-id="9322a-119">Hard disks</span></span>
- <span data-ttu-id="9322a-120">ロード バランサー</span><span class="sxs-lookup"><span data-stu-id="9322a-120">Load balancers</span></span>

### <a name="platform-as-a-service-paas"></a><span data-ttu-id="9322a-121">サービスとしてのプラットフォーム (PaaS)</span><span class="sxs-lookup"><span data-stu-id="9322a-121">Platform as a service (PaaS)</span></span>

<span data-ttu-id="9322a-122">サービスとしてのプラットフォームでは、ソフトウェア アプリケーションのビルド、テスト、展開のための環境が提供されます。</span><span class="sxs-lookup"><span data-stu-id="9322a-122">Platform as a Service provides an environment for building, testing, and deploying software applications.</span></span> <span data-ttu-id="9322a-123">PaaS の目的は、基になっているインフラストラクチャの管理について心配することなく、できるだけ早くアプリケーションを作成できるようにすることです。</span><span class="sxs-lookup"><span data-stu-id="9322a-123">The goal of PaaS is to help you create an application as quickly as possible without having to worry about managing the underlying infrastructure.</span></span> <span data-ttu-id="9322a-124">たとえば、PaaS を使用して Web アプリケーションを展開するときは、オペレーティング システムや Web サーバー、さらにはシステムの更新プログラムさえインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="9322a-124">For example, when deploying a web application using PaaS, you don't have to install an operating system, web server, or even system updates.</span></span> 

<span data-ttu-id="9322a-125">PaaS の例は Azure App Service です。</span><span class="sxs-lookup"><span data-stu-id="9322a-125">An example of PaaS is Azure App Service.</span></span>

### <a name="software-as-a-service-saas"></a><span data-ttu-id="9322a-126">サービスとしてのソフトウェア (SaaS)</span><span class="sxs-lookup"><span data-stu-id="9322a-126">Software as a service (SaaS)</span></span>

<span data-ttu-id="9322a-127">サービスとしてのソフトウェアでは、インターネット経由でアプリケーションを配布する手段が提供されます。</span><span class="sxs-lookup"><span data-stu-id="9322a-127">Software as a Service provides a way to deliver applications over the internet.</span></span> <span data-ttu-id="9322a-128">SaaS アプリケーションは Web ベースのアプリケーションと呼ぶのが最も一般的ですが、SaaS アプリケーションという用語は、アプリケーションが SaaS プロバイダーのサーバーでホストされているという事実に由来します。</span><span class="sxs-lookup"><span data-stu-id="9322a-128">A SaaS application is most commonly called a web-based application; however, the term SaaS application comes from the fact that the application is hosted on a SaaS provider's server.</span></span> <span data-ttu-id="9322a-129">SaaS では、インストールやセットアップについて心配する必要はなく、Web ブラウザーまたはソフトウェア アプリケーションを通して使用するだけです。</span><span class="sxs-lookup"><span data-stu-id="9322a-129">With SaaS, you don't have to worry about installation or setup, you just use it through a web browser or software application.</span></span> 

<span data-ttu-id="9322a-130">Microsoft Office 365 は SaaS アプリケーションの例です。</span><span class="sxs-lookup"><span data-stu-id="9322a-130">An example of a SaaS application is Microsoft Office 365.</span></span>

## <a name="summary"></a><span data-ttu-id="9322a-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="9322a-131">Summary</span></span>

<span data-ttu-id="9322a-132">IaaS、PaaS、SaaS はすべて相互にレイヤーを形成しています。</span><span class="sxs-lookup"><span data-stu-id="9322a-132">IaaS, PaaS, and SaaS all layer on top of each other.</span></span> <span data-ttu-id="9322a-133">基になるハードウェアを制御できるレベルにより、最適なサービスのカテゴリが決まります。</span><span class="sxs-lookup"><span data-stu-id="9322a-133">The level of control that you want over the underlying hardware will determine what category of services would work best for you.</span></span>
