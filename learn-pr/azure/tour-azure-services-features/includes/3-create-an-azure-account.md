<span data-ttu-id="d7ec8-101">無料の Azure アカウントを使用して、エンタープライズ アプリケーションの構築、テスト、デプロイや、カスタムの Web およびモバイル エクスペリエンスの作成を行うことができます。また、機械学習や強力な分析によってデータから分析情報を得ることもできます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-101">With a free Azure account, you can build, test, and deploy enterprise applications, create custom web and mobile experiences, and gain insights from your data through machine learning and powerful analytics.</span></span>

## <a name="what-is-an-azure-account"></a><span data-ttu-id="d7ec8-102">Azure アカウントとは</span><span class="sxs-lookup"><span data-stu-id="d7ec8-102">What is an Azure account?</span></span>

<span data-ttu-id="d7ec8-103">_Azure アカウント_は特定の ID に関連付けられており、次のような情報が保持されています。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-103">An _Azure account_ is tied to a specific identity and holds information like:</span></span>

- <span data-ttu-id="d7ec8-104">名前、電子メール、連絡先の設定</span><span class="sxs-lookup"><span data-stu-id="d7ec8-104">Name, email, and contact preferences</span></span>
- <span data-ttu-id="d7ec8-105">クレジット カードなどの課金情報</span><span class="sxs-lookup"><span data-stu-id="d7ec8-105">Billing information such as a credit card</span></span>

<span data-ttu-id="d7ec8-106">Azure アカウントは、1 つ以上の_サブスクリプション_に関連付けられています。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-106">An Azure account is associated with one more  _subscriptions_.</span></span>

## <a name="what-is-an-azure-subscription"></a><span data-ttu-id="d7ec8-107">Azure サブスクリプションとは</span><span class="sxs-lookup"><span data-stu-id="d7ec8-107">What is an Azure subscription?</span></span>

<span data-ttu-id="d7ec8-108">_Azure サブスクリプション_は、Microsoft Azure にリソースをプロビジョニングするために使用される論理コンテナーです。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-108">An _Azure subscription_ is a logical container used to provision resources in Microsoft Azure.</span></span> <span data-ttu-id="d7ec8-109">仮想マシンやデータベースなどのすべてのリソースの詳細が保持されています。また、サブスクリプションには、Azure AD _テナント_との信頼関係があります。この信頼関係は、サブスクリプションに保持されているリソースに対してユーザーとロールを認証する際に使用されます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-109">It holds the details of all your resources like virtual machines, databases, etc. It also has a trust relationship to a single Azure AD _tenant_ which is used to authenticate users and roles for the resources held in the subscription.</span></span>

<span data-ttu-id="d7ec8-110">課金はサブスクリプション レベルで発生します。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-110">Billing occurs at the subscription level.</span></span> <span data-ttu-id="d7ec8-111">月末に驚かないように、サブスクリプションごとに使用制限を設定できます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-111">You can set spending limits on each subscription to ensure you aren't surprised at the end of the month.</span></span> 

## <a name="what-is-an-azure-ad-tenant"></a><span data-ttu-id="d7ec8-112">Azure AD テナントとは</span><span class="sxs-lookup"><span data-stu-id="d7ec8-112">What is an Azure AD tenant?</span></span>

<span data-ttu-id="d7ec8-113">Azure AD (Azure Active Directory) は、複数の認証プロトコルをサポートして、クラウドのアプリケーションとサービスをセキュリティで保護する最新の ID プロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-113">Azure AD (Azure Active Directory) is a modern identity provider that supports multiple authentication protocols to secure applications and services in the cloud.</span></span> <span data-ttu-id="d7ec8-114">Windows デスクトップおよびサーバーのセキュリティ保護を重視した Windows Active Directory とは_異なります_。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-114">It's _not_ the same as Windows Active Directory, which is focused on securing Windows desktops and servers.</span></span> <span data-ttu-id="d7ec8-115">Azure AD は、OpenID や OAuth などの Web ベースの認証標準に関するものです。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-115">Instead, Azure AD is all about web-based authentication standards such as OpenID and OAuth.</span></span>

<span data-ttu-id="d7ec8-116">1 つのテナントは論理的な組織を表しており、そのテナントによって保護されているリソースに複数の ID がアクセスして利用することができます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-116">A single tenant represents a logical organization and allows multiple identities to access and utilize resources protected by that tenant.</span></span> <span data-ttu-id="d7ec8-117">Azure サブスクリプションには、常に "_1 つの_" Azure AD テナントとの信頼関係がありますが、"_複数の_" サブスクリプションで 1 つのテナントを共有できます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-117">An Azure subscription always has a trust relationship with a _single_ Azure AD tenant, but _multiple_ subscriptions can share a single tenant.</span></span> <span data-ttu-id="d7ec8-118">この構造により、組織は複数のサブスクリプションを管理し、それらに含まれるすべてのリソースのセキュリティ規則を設定できます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-118">This structure allows the organization to manage multiple subscriptions and set security rules across all the resources contained within them.</span></span>

<span data-ttu-id="d7ec8-119">ここでは、アカウント、サブスクリプション、テナント、リソースを簡単に表しています。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-119">Here's a simple representation of accounts, subscriptions, tenants, and resources.</span></span>

![アカウント、テナント、サブスクリプション、リソースの連携の図](../media/3-azure-ad-tenant.png)

<span data-ttu-id="d7ec8-121">Azure AD テナントごとに "_アカウント オーナー_" がいることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-121">Notice that each Azure AD tenant has an _account owner_.</span></span> <span data-ttu-id="d7ec8-122">これは、課金に対して責任を負う元の Azure アカウントです。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-122">This is the original Azure account that is responsible for billing.</span></span> <span data-ttu-id="d7ec8-123">ユーザーをテナントに追加したり、サブスクリプション内のリソースにアクセスするために、他の Azure AD テナントからゲストを招待したりできます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-123">You can add additional users to the tenant, and even invite guests from other Azure AD tenants to access resources in subscriptions.</span></span>

## <a name="azure-account-types"></a><span data-ttu-id="d7ec8-124">Azure アカウントの種類</span><span class="sxs-lookup"><span data-stu-id="d7ec8-124">Azure account types</span></span>

<span data-ttu-id="d7ec8-125">Azure には、さまざまな顧客タイプに対応する複数のアカウントの種類があります。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-125">Azure has several account types that cater to different customer types.</span></span> <span data-ttu-id="d7ec8-126">最もよく使用されるアカウントは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-126">The most commonly used accounts are:</span></span>

- <span data-ttu-id="d7ec8-127">無料</span><span class="sxs-lookup"><span data-stu-id="d7ec8-127">Free</span></span>
- <span data-ttu-id="d7ec8-128">従量課金制</span><span class="sxs-lookup"><span data-stu-id="d7ec8-128">Pay-As-You-Go</span></span>
- <span data-ttu-id="d7ec8-129">Enterprise Agreement</span><span class="sxs-lookup"><span data-stu-id="d7ec8-129">Enterprise Agreement</span></span>

### <a name="azure-free-account"></a><span data-ttu-id="d7ec8-130">Azure 無料アカウント</span><span class="sxs-lookup"><span data-stu-id="d7ec8-130">Azure free account</span></span>

<span data-ttu-id="d7ec8-131">Azure 無料アカウントには、最初の 30 日間に使用できる **200 ドルのクレジット**、最も人気のある Azure 製品への 12 か月間の無料アクセス、常に無料の 25 以上の製品へのアクセスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-131">An Azure free account includes a **$200 credit** to spend for the first 30 days, free access to the most popular Azure products for 12 months, and access to more than 25 products that are always free.</span></span> <span data-ttu-id="d7ec8-132">これは、新しいユーザーが使い始めるための優れた方法です。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-132">This is an excellent way for new users to get started.</span></span> <span data-ttu-id="d7ec8-133">無料アカウントを設定するには、電話番号、クレジット カード、および Microsoft アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-133">To set up a free account, you need a phone number, a credit card, and a Microsoft account.</span></span>

> [!NOTE]
> <span data-ttu-id="d7ec8-134">クレジットカード情報は本人確認にのみ使用されます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-134">Credit card information is used for identity verification only.</span></span> <span data-ttu-id="d7ec8-135">アップグレードするまで、どのサービスにも課金されません。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-135">You won’t be charged for any services until you upgrade.</span></span>

### <a name="azure-pay-as-you-go-account"></a><span data-ttu-id="d7ec8-136">Azure 従量課金制アカウント</span><span class="sxs-lookup"><span data-stu-id="d7ec8-136">Azure Pay-As-You-Go account</span></span>

<span data-ttu-id="d7ec8-137">従量課金制 (PAYG) アカウントでは、ユーザーが 1 か月間に使用したサービスに対して請求されます。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-137">A Pay-As-You-Go (PAYG) account bills you monthly for the services you used.</span></span> <span data-ttu-id="d7ec8-138">このアカウントの種類は、個人から小規模企業や多くの大規模組織まで、広範なユーザーに適しています。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-138">This account type is appropriate for a wide range of users, from individuals to small businesses and many large organizations as well.</span></span>

### <a name="azure-enterprise-agreement"></a><span data-ttu-id="d7ec8-139">Azure Enterprise Agreement</span><span class="sxs-lookup"><span data-stu-id="d7ec8-139">Azure Enterprise Agreement</span></span>

<span data-ttu-id="d7ec8-140">Enterprise Agreement では 1 つの契約でクラウド サービスやソフトウェア ライセンスを柔軟に購入でき、新しいライセンスとソフトウェア アシュアランスについては割引があります。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-140">An Enterprise Agreement provides flexibility to buy cloud services and software licenses under one agreement, with discounts for new licenses and Software Assurance.</span></span> <span data-ttu-id="d7ec8-141">エンタープライズ規模の組織が対象になります。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-141">It is targeted at enterprise-scale organizations.</span></span>

## <a name="summary"></a><span data-ttu-id="d7ec8-142">まとめ</span><span class="sxs-lookup"><span data-stu-id="d7ec8-142">Summary</span></span>

<span data-ttu-id="d7ec8-143">個人、スモール ビジネス、エンタープライズのいずれであっても、Azure サービスを使用するにはアカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-143">Whether you are an individual, a small business, or an enterprise, you need an account to use Azure services.</span></span> <span data-ttu-id="d7ec8-144">一般的な順序としては、Azure サービスを評価できるように無料アカウントで開始します。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-144">The typical sequence is to start with a free account so that you can evaluate Azure services.</span></span> <span data-ttu-id="d7ec8-145">試用期間が経過した後、無料アカウントから従量課金制に変換します。</span><span class="sxs-lookup"><span data-stu-id="d7ec8-145">When your trial period expires, you will convert from the free account to Pay-As-You-Go.</span></span>