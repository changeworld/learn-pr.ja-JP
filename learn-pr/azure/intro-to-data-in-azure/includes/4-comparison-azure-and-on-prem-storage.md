<span data-ttu-id="27402-101">Azure データ ストレージの利点と機能を学習したので、次はオンプレミス ストレージとの違いを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="27402-101">Now that you know about the benefits and features of Azure data storage, let's see how it differs from on-premises storage.</span></span>

<span data-ttu-id="27402-102">"オンプレミス" という用語は、ローカルのハードウェアやサーバー上でのデータの保存と管理を指します。</span><span class="sxs-lookup"><span data-stu-id="27402-102">The term "on-premises" refers to the storage and maintenance of data on local hardware and servers.</span></span> <span data-ttu-id="27402-103">オンプレミスと Azure データ ストレージを比較するときに、考慮すべきいくつかの要素があります。</span><span class="sxs-lookup"><span data-stu-id="27402-103">There are several factors to consider when comparing on-premises to Azure data storage.</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="27402-104">![紙の請求書とコスト効率を表すクラウド](../media/4-cost-effectiveness.png)</span><span class="sxs-lookup"><span data-stu-id="27402-104">![Paper bill and a cloud representing cost effectiveness](../media/4-cost-effectiveness.png)</span></span>
  :::column-end:::
    <span data-ttu-id="27402-105">:::column span="3"::: **コスト効率**</span><span class="sxs-lookup"><span data-stu-id="27402-105">:::column span="3"::: **Cost effectiveness**</span></span>

<span data-ttu-id="27402-106">オンプレミス ストレージ ソリューションには、購入、導入、構成、保守が必要な専用ハードウェアが必要です。</span><span class="sxs-lookup"><span data-stu-id="27402-106">An on-premises storage solution requires dedicated hardware that needs to be purchased, installed, configured, and maintained.</span></span> <span data-ttu-id="27402-107">これは、多大な初期投資 (資本コスト) になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="27402-107">This can be a significant up-front expense (or capital cost).</span></span> <span data-ttu-id="27402-108">要件の変更により、新しいハードウェアへの投資が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="27402-108">Change in requirements can require investment in new hardware.</span></span> <span data-ttu-id="27402-109">お使いのハードウェアでピーク時の需要を処理できる必要があります。これは、ピーク時以外はアイドル状態であったり、使用率が低かったりする可能性があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="27402-109">Your hardware needs to be capable of handling peak demand which means it may sit idle or be under-utilized in off-peak times.</span></span>

<span data-ttu-id="27402-110">Azure データ ストレージでは、初期資本コストではなく、運用コストとして企業にとって魅力的であることが多い従量課金制価格モデルが提供されます。</span><span class="sxs-lookup"><span data-stu-id="27402-110">Azure data storage provides a pay-as-you-go pricing model which is often appealing to businesses as an operating expense instead of an upfront capital cost.</span></span> <span data-ttu-id="27402-111">さらに、Azure データ ストレージはスケーラブルであるため、需要の増加に応じてスケールアップまたはスケールアウトし、需要が少ないときにはスケールバックすることができます。</span><span class="sxs-lookup"><span data-stu-id="27402-111">It's also scalable, allowing you to scale up or scale out as demand dictates and scale back when demand is low.</span></span> <span data-ttu-id="27402-112">データ サービスが必要な場合にのみ課金されます。</span><span class="sxs-lookup"><span data-stu-id="27402-112">You are charged for data services only as you need them.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="27402-113">![信頼性を表す証明書](../media/4-reliability.png)</span><span class="sxs-lookup"><span data-stu-id="27402-113">![A certificate representing reliability](../media/4-reliability.png)</span></span>
  :::column-end:::
    <span data-ttu-id="27402-114">:::column span="3"::: **信頼性**</span><span class="sxs-lookup"><span data-stu-id="27402-114">:::column span="3"::: **Reliability**</span></span>

<span data-ttu-id="27402-115">オンプレミス ストレージには、データ バックアップ、負荷分散、ディザスター リカバリーの各戦略が必要です。</span><span class="sxs-lookup"><span data-stu-id="27402-115">On-premises storage requires data backup, load balancing, and disaster recovery strategies.</span></span> <span data-ttu-id="27402-116">多くの場合、これらは、ハードウェアと IT リソースの両方に多大な投資を必要とする専用サーバーが必要であるため、困難でコストがかかる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="27402-116">These can be challenging and expensive as they often each need dedicated servers requiring a significant investment in both hardware and IT resources.</span></span>

<span data-ttu-id="27402-117">Azure データ ストレージでは、データ バックアップ、負荷分散、ディザスター リカバリー、データ レプリケーションがサービスとして提供され、データの安全性と高可用性が確保されます。</span><span class="sxs-lookup"><span data-stu-id="27402-117">Azure data storage provides data backup, load balancing, disaster recovery, and data replication as services to ensure data safety and high availability.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="27402-118">![別のストレージの種類を表す一意に形成されたビルド](../media/4-storage-types.png)</span><span class="sxs-lookup"><span data-stu-id="27402-118">![A uniquely shaped building representing different storage types](../media/4-storage-types.png)</span></span>
  :::column-end:::
    <span data-ttu-id="27402-119">:::column span="3"::: **ストレージの種類**</span><span class="sxs-lookup"><span data-stu-id="27402-119">:::column span="3"::: **Storage types**</span></span>

<span data-ttu-id="27402-120">ソリューションには、ファイル ストレージやデータベース ストレージなど、複数の異なるストレージの種類が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="27402-120">Sometimes multiple different storage types are required for a solution, such as file and database storage.</span></span> <span data-ttu-id="27402-121">多くの場合、オンプレミスのアプローチでは、ストレージの種類ごとに多数のサーバーと管理ツールが必要になります。</span><span class="sxs-lookup"><span data-stu-id="27402-121">An on-premises approach often requires numerous servers and administrative tools for each storage type.</span></span>

<span data-ttu-id="27402-122">Azure データ ストレージには、分散アクセスや階層型ストレージなどのさまざまなストレージ オプションが用意されています。</span><span class="sxs-lookup"><span data-stu-id="27402-122">Azure data storage provides a variety of different storage options including distributed access and tiered storage.</span></span> <span data-ttu-id="27402-123">これにより、ソリューションの各部分に最適なストレージの選択肢を提供するストレージ テクノロジの組み合わせを統合することが可能になります。</span><span class="sxs-lookup"><span data-stu-id="27402-123">This makes it possible to integrate a combination of storage technologies providing the best storage choice for each part of your solution.</span></span>

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="27402-124">![機敏性を表すスポーツ プレイブック](../media/4-agility.png)</span><span class="sxs-lookup"><span data-stu-id="27402-124">![A sports playbook representing agility](../media/4-agility.png)</span></span>
  :::column-end:::
    <span data-ttu-id="27402-125">:::column span="3"::: **俊敏性**</span><span class="sxs-lookup"><span data-stu-id="27402-125">:::column span="3"::: **Agility**</span></span>

<span data-ttu-id="27402-126">要件とテクノロジは変化します。</span><span class="sxs-lookup"><span data-stu-id="27402-126">Requirements and technologies change.</span></span> <span data-ttu-id="27402-127">オンプレミス展開の場合、これは新しいサーバーやインフラストラクチャ要素のプロビジョニングと展開を意味することがありますが、このアクティビティには時間とコストがかかります。</span><span class="sxs-lookup"><span data-stu-id="27402-127">For an on-premises deployment this may mean provisioning and deploying new servers and infrastructure pieces, which is a time consuming and expensive activity.</span></span>

<span data-ttu-id="27402-128">Azure データ ストレージでは、新しいサービスを数分で作成できる柔軟性がもたらされます。</span><span class="sxs-lookup"><span data-stu-id="27402-128">Azure data storage gives you the flexibility to create new services in minutes.</span></span> <span data-ttu-id="27402-129">この柔軟性により、多大なハードウェア投資を必要とせずにストレージ バックエンドを迅速に変更できます。</span><span class="sxs-lookup"><span data-stu-id="27402-129">This flexibility allows you to change storage back-ends quickly without needing a significant hardware investment.</span></span>

<span data-ttu-id="27402-130">次の図では、オンプレミス ストレージと Azure データ ストレージの違いを示します。</span><span class="sxs-lookup"><span data-stu-id="27402-130">The following illustration shows differences between on-premise storage and Azure data storage.</span></span>

![いくつかの一般的なビジネス ニーズに関するオンプレミス ストレージと Azure データ ストレージの比較を示す図。](../media/4-Comparison.png)

  :::column-end:::
:::row-end:::