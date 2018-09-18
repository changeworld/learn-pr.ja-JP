<span data-ttu-id="28d11-101">クラウドには 3 種類のデプロイ モデルがあります。</span><span class="sxs-lookup"><span data-stu-id="28d11-101">There are three different cloud deployment models.</span></span> <span data-ttu-id="28d11-102">クラウドのデプロイ モデルでは、データが格納される場所、顧客がデータとやり取りする方法、顧客がデータを取得する方法、アプリケーションが実行される場所が定義されています。</span><span class="sxs-lookup"><span data-stu-id="28d11-102">A cloud deployment model defines where your data is stored and how your customers interact with it – how do they get to it, and where do the applications run?</span></span> <span data-ttu-id="28d11-103">また、モデルは、管理する、または管理する必要がある独自のインフラストラクチャの量によっても異なります。</span><span class="sxs-lookup"><span data-stu-id="28d11-103">It also depends on how much of your own infrastructure you want or need to manage.</span></span>

<span data-ttu-id="28d11-104">ここでは、クラウド コンピューティング リソースに対するさまざまなタイプのデプロイ方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="28d11-104">Here, you'll explore the different types of deployment methods for your cloud computing resources</span></span>

<!-- TODO: Verify video -->
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2yEv7]

:::row:::
    :::column:::
        <span data-ttu-id="28d11-105">![パブリック クラウド アイコン](../media/4-public-cloud.png)</span><span class="sxs-lookup"><span data-stu-id="28d11-105">![Public Cloud Icon](../media/4-public-cloud.png)</span></span>
    :::column-end:::
    <span data-ttu-id="28d11-106">:::column span="3"::: **パブリック クラウド**</span><span class="sxs-lookup"><span data-stu-id="28d11-106">:::column span="3"::: **Public cloud**</span></span>

<span data-ttu-id="28d11-107">これは、最も一般的なデプロイ モデルです。</span><span class="sxs-lookup"><span data-stu-id="28d11-107">This is the most common deployment model.</span></span> <span data-ttu-id="28d11-108">この場合は、管理したり最新状態に維持するローカル ハードウェアはありません。すべてのものはクラウド プロバイダーのハードウェア上で実行されます。</span><span class="sxs-lookup"><span data-stu-id="28d11-108">In this case, you have no local hardware to manage or keep up-to-date – everything runs on your cloud provider’s hardware.</span></span> <span data-ttu-id="28d11-109">場合によっては、他のクラウド ユーザーとコンピューティング リソースを共有することによって、コストをさらに節約できます。</span><span class="sxs-lookup"><span data-stu-id="28d11-109">In some cases, you can save additional costs by sharing computing resources with other cloud users.</span></span>

<span data-ttu-id="28d11-110">パブリック クラウドの利点には次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="28d11-110">Some advantages of the public cloud are:</span></span>

- <span data-ttu-id="28d11-111">高い拡張性 – スケーリングのために新しいサーバーを購入する必要はありません</span><span class="sxs-lookup"><span data-stu-id="28d11-111">High scalability – you don’t have to buy a new server in order to scale</span></span>
- <span data-ttu-id="28d11-112">従量課金制の価格 – 使用量に対してのみ料金を支払います</span><span class="sxs-lookup"><span data-stu-id="28d11-112">Pay-as-you-go pricing – you pay only for what you use</span></span>
- <span data-ttu-id="28d11-113">ユーザーに、ハードウェアのメンテナンスや更新に関する責任はありません</span><span class="sxs-lookup"><span data-stu-id="28d11-113">You’re not responsible for maintenance or updates of the hardware</span></span> :::column-end:::
  :::row-end:::
:::row:::
   :::column:::
        <span data-ttu-id="28d11-114">![プライベート クラウド アイコン](../media/4-private-cloud.png)</span><span class="sxs-lookup"><span data-stu-id="28d11-114">![Private Cloud Icon](../media/4-private-cloud.png)</span></span>
    :::column-end:::
    <span data-ttu-id="28d11-115">:::column span="3"::: **プライベート クラウド**</span><span class="sxs-lookup"><span data-stu-id="28d11-115">:::column span="3"::: **Private cloud**</span></span>

<span data-ttu-id="28d11-116">プライベート クラウドの場合、独自のデータ センターでクラウド環境を構築し、組織のユーザーにコンピューティング リソースへのセルフサービス アクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="28d11-116">In a private cloud, you create a cloud environment in your own data center and provide self-service access to compute resources to users in your organization.</span></span> <span data-ttu-id="28d11-117">ユーザーにパブリック クラウドのシミュレーションを提供しますが、自分が提供するハードウェア/ソフトウェア サービスの購入と保守管理はすべて自分の責任になります。</span><span class="sxs-lookup"><span data-stu-id="28d11-117">This offers a simulation of a public cloud to your users, but you remain completely responsible for the purchase and maintenance of the hardware and software services you provide.</span></span>

<span data-ttu-id="28d11-118">チームがプライベート クラウドから離れる理由には次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="28d11-118">Some reasons teams move away from the private cloud are:</span></span>

- <span data-ttu-id="28d11-119">初期構築時とメンテナンス時にハードウェアを購入する必要があります</span><span class="sxs-lookup"><span data-stu-id="28d11-119">You have to purchase the hardware for startup and maintenance</span></span>
- <span data-ttu-id="28d11-120">プライベート クラウドには IT スキルと専門知識が必要ですが、それは簡単に得られるものではありません</span><span class="sxs-lookup"><span data-stu-id="28d11-120">Private clouds require IT skills and expertise that's hard to come by</span></span>
:::column-end:::
:::row-end:::
 :::row:::
    :::column:::
        <span data-ttu-id="28d11-121">![ハイブリッド クラウド アイコン](../media/4-hybrid-cloud.png)</span><span class="sxs-lookup"><span data-stu-id="28d11-121">![Hybrid Cloud Icon](../media/4-hybrid-cloud.png)</span></span>
    :::column-end:::
    <span data-ttu-id="28d11-122">:::column span="3"::: **ハイブリッド クラウド**</span><span class="sxs-lookup"><span data-stu-id="28d11-122">:::column span="3"::: **Hybrid cloud**</span></span>

<span data-ttu-id="28d11-123">ハイブリッド クラウドはパブリック クラウドとプライベート クラウドを組み合わせたものであり、最適な場所でアプリケーションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="28d11-123">A hybrid cloud combines public and private clouds, allowing you to run your applications in the most appropriate location.</span></span> <span data-ttu-id="28d11-124">たとえば、パブリック クラウドで Web サイトをホストし、プライベート クラウド (またはオンプレミス データ センター) でホストしている高度なセキュリティで保護されたデータベースにそのサイトをリンクします。</span><span class="sxs-lookup"><span data-stu-id="28d11-124">For example, you could host a web site in the public cloud and link it to a highly secure database hosted in your private cloud (or on-premises data center).</span></span>

<span data-ttu-id="28d11-125">これは、法的な理由などのために、クラウドに置くことができないものがある場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="28d11-125">This is helpful when you have some things that cannot be put in the cloud, maybe for legal reasons.</span></span> <span data-ttu-id="28d11-126">たとえば、パブリックに公開できないデータが存在することがあります (医療データなど)。</span><span class="sxs-lookup"><span data-stu-id="28d11-126">For example, you may have data that cannot be exposed publicly (such as medical data).</span></span> <span data-ttu-id="28d11-127">あるいは、更新できない古いハードウェア上でアプリケーションが実行されている場合もあります。</span><span class="sxs-lookup"><span data-stu-id="28d11-127">Another example is one or more applications that run on old hardware that can’t be updated.</span></span> <span data-ttu-id="28d11-128">この場合は、古いシステムはローカルで実行したままにして、承認やストレージのためにパブリック クラウドにそれを接続できます。</span><span class="sxs-lookup"><span data-stu-id="28d11-128">In this case, you can keep the old system running locally, and connect it to the public cloud for authorization or storage.</span></span>

<span data-ttu-id="28d11-129">プライベート クラウドと比較した場合、ハイブリッド クラウドには次のような利点があります。</span><span class="sxs-lookup"><span data-stu-id="28d11-129">Some advantages of a hybrid cloud versus a private cloud are:</span></span>

- <span data-ttu-id="28d11-130">古くなったハードウェアやオペレーティング システムを使用しているシステムを実行させ、アクセスできるようにしておくことができます</span><span class="sxs-lookup"><span data-stu-id="28d11-130">You can keep any systems running and accessible that use out-of-date hardware or an out-of-date operating system</span></span>
- <span data-ttu-id="28d11-131">ローカルまたはクラウドのどちらか適した方で実行できる柔軟性が与えられます。</span><span class="sxs-lookup"><span data-stu-id="28d11-131">You have flexibility with what you run locally versus in the cloud</span></span>

<span data-ttu-id="28d11-132">次のような問題を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28d11-132">Some concerns you'll need to watch out for are:</span></span>

- <span data-ttu-id="28d11-133">1 つのデプロイ モデルを選択することよりコストがかかる場合があります</span><span class="sxs-lookup"><span data-stu-id="28d11-133">It can be more expensive than selecting one deployment model</span></span>
- <span data-ttu-id="28d11-134">セットアップと管理が一層複雑になる可能性があります</span><span class="sxs-lookup"><span data-stu-id="28d11-134">It can be more complicated to set up and manage</span></span> :::column-end:::
  :::row-end:::

## <a name="summary"></a><span data-ttu-id="28d11-135">まとめ</span><span class="sxs-lookup"><span data-stu-id="28d11-135">Summary</span></span>

<span data-ttu-id="28d11-136">クラウド コンピューティングには柔軟性があり、デプロイする方法を選択できます。</span><span class="sxs-lookup"><span data-stu-id="28d11-136">Cloud computing is flexible and gives you the ability to choose how you want to deploy it.</span></span> <span data-ttu-id="28d11-137">クラウド デプロイ モデルは、予算のほか、セキュリティ、スケーラビリティ、メンテナンスのニーズに基づいて選択します。</span><span class="sxs-lookup"><span data-stu-id="28d11-137">The cloud deployment model you choose depends on your budget, and on your security, scalability, and maintenance needs.</span></span>