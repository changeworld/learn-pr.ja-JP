<span data-ttu-id="62ebe-101">攻撃や未承認のアクセスからネットワークを保護することは、すべてのアーキテクチャの重要な部分です。</span><span class="sxs-lookup"><span data-stu-id="62ebe-101">Securing your network from attacks and unauthorized access is an important part of any architecture.</span></span> <span data-ttu-id="62ebe-102">環境が大きくなりすぎる前に、Lamna Healthcare はネットワーク インフラストラクチャを計画しました。</span><span class="sxs-lookup"><span data-stu-id="62ebe-102">Before their environment became too large, Lamna Healthcare took the time to plan out their network infrastructure.</span></span> <span data-ttu-id="62ebe-103">ここでは、ネットワーク セキュリティとはどのようなものか、階層型アプローチをアーキテクチャに統合する方法、環境にネットワーク セキュリティを提供するときに Azure がどのように役立つかについて見ていきます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-103">Here, we'll take a look at what network security looks like, how to integrate a layered approach into your architecture, and how Azure can help you provide network security for your environment.</span></span>

## <a name="what-is-network-security"></a><span data-ttu-id="62ebe-104">ネットワークのセキュリティとは</span><span class="sxs-lookup"><span data-stu-id="62ebe-104">What is network security</span></span>

<span data-ttu-id="62ebe-105">ネットワーク セキュリティは、ネットワーク内およびネットワーク外にあるリソースの通信を保護します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-105">Network security is protecting the communication of resources within and outside of your network.</span></span>  <span data-ttu-id="62ebe-106">目標は、サービスおよびシステム全体のネットワーク層での露出を制限することです。</span><span class="sxs-lookup"><span data-stu-id="62ebe-106">The goal is to limit exposure at the network layer across your services and systems.</span></span> <span data-ttu-id="62ebe-107">この露出を制限することで、リソースが攻撃される可能性が減ります。</span><span class="sxs-lookup"><span data-stu-id="62ebe-107">By limiting this exposure, you decrease the likelihood that your resources can be attacked.</span></span> <span data-ttu-id="62ebe-108">ネットワーク セキュリティに注目すると、以下の部分に集中して作業を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-108">In the focus on network security, efforts can be focused on the following areas:</span></span>

- <span data-ttu-id="62ebe-109">アプリケーションとインターネット間のトラフィック フローのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="62ebe-109">Securing traffic flow between applications and the internet</span></span>
- <span data-ttu-id="62ebe-110">アプリケーション間のトラフィック フローのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="62ebe-110">Securing traffic flow amongst applications</span></span>
- <span data-ttu-id="62ebe-111">ユーザーとアプリケーション間のトラフィック フローのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="62ebe-111">Securing traffic flow between users and the application</span></span>

<span data-ttu-id="62ebe-112">アプリケーションとインターネット間のトラフィック フローのセキュリティ保護では、自分のネットワークの外部への露出を制限することに注目します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-112">Securing traffic flow between applications and the internet focuses on limiting exposure outside your network.</span></span> <span data-ttu-id="62ebe-113">ネットワークへの攻撃はネットワークの外部から始まることが最も多いので、インターネットへの露出を制限して、境界をセキュリティで保護することにより、攻撃されるリスクを減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-113">Network attacks will most frequently start outside your network, so by limiting the internet exposure and securing the perimeter, the risk of being attacked can be reduced.</span></span>

<span data-ttu-id="62ebe-114">アプリケーション間のトラフィック フローのセキュリティ保護では、アプリケーション間、階層間、異なる環境間、ネットワーク内の他のサービス間のデータに注目します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-114">Securing traffic flow amongst applications focuses on  data between applications, their tiers, between different environments, and other services within your network.</span></span> <span data-ttu-id="62ebe-115">これらのリソース間の露出を制限することにより、侵害されたリソースが及ぼす影響を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-115">By limiting exposure between these resources, you reduce the effect a compromised resource can have.</span></span> <span data-ttu-id="62ebe-116">これにより、ネットワーク内で侵害が広がるのを抑えることができます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-116">This can help reduce further propagation within a network.</span></span>

<span data-ttu-id="62ebe-117">ユーザーとアプリケーション間のトラフィック フローのセキュリティ保護では、エンド ユーザーに対するネットワーク フローのセキュリティ保護に注目します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-117">Securing traffic flow between users and the application focuses on securing the network flow for your end users.</span></span> <span data-ttu-id="62ebe-118">これにより、リソースが外部の攻撃にさらされる可能性が制限され、ユーザーがリソースを利用するためのセキュリティで保護されたメカニズムが提供されます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-118">This limits the exposure your resources have to outside attacks, and provides a secure mechanism for users to utilize your resources.</span></span> 

## <a name="a-layered-approach-to-network-security"></a><span data-ttu-id="62ebe-119">ネットワーク セキュリティに対する階層型アプローチ</span><span class="sxs-lookup"><span data-stu-id="62ebe-119">A layered approach to network security</span></span>

<span data-ttu-id="62ebe-120">このモジュールの共通スレッドでは、セキュリティに対して階層型アプローチが採用されており、このアプローチはネットワーク層でも違いません。</span><span class="sxs-lookup"><span data-stu-id="62ebe-120">A common thread throughout this module has been taking a layered approach to security, and this approach is no different at the network layer.</span></span> <span data-ttu-id="62ebe-121">ネットワーク境界のセキュリティ保護、またはネットワーク内のサービス間のネットワーク セキュリティに注目するだけでは、十分ではありません。</span><span class="sxs-lookup"><span data-stu-id="62ebe-121">It's not enough to just focus on securing the network perimeter, or focusing on the network security between services inside a network.</span></span> <span data-ttu-id="62ebe-122">階層型アプローチでは、攻撃者が 1 つの階層を突破した場合でも、それ以上の攻撃を制限するための保護がさらに存在するように、複数レベルの保護が提供されています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-122">A layered approach provides multiple levels of protection, so that if an attacker gets through one layer, there's further protections in place to limit further attack.</span></span>

<span data-ttu-id="62ebe-123">Azure においてネットワーク フットプリントをセキュリティ保護するために階層型アプローチ向けのツールがどのように提供されるのかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="62ebe-123">Let's take a look at how Azure can provide the tools for a layered approach to securing your network footprint.</span></span>

### <a name="internet-protection"></a><span data-ttu-id="62ebe-124">インターネットからの保護</span><span class="sxs-lookup"><span data-stu-id="62ebe-124">Internet protection</span></span>

<span data-ttu-id="62ebe-125">まず、ネットワークの境界の場合は、インターネットからの攻撃を制限および排除することに注目します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-125">If we start on the perimeter of the network, we're focused on limiting and eliminating attacks from the internet.</span></span> <span data-ttu-id="62ebe-126">出発点として最も適しているのは、インターネットに接続しているリソースを評価し、必要な場合にのみ受信および送信の通信を許可することです。</span><span class="sxs-lookup"><span data-stu-id="62ebe-126">A great first place to start is to assess the resources that are internet facing, and only allow inbound and outbound communication where necessary.</span></span> <span data-ttu-id="62ebe-127">任意の種類の受信ネットワーク トラフィックを許可しているすべてのリソースを明らかにし、それらが必要であること、そして必要なポート/プロトコルのみに制限されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-127">Identify all resources that are allowing inbound network traffic of any type, and ensure they are necessary and restricted to only the ports/protocols required.</span></span> <span data-ttu-id="62ebe-128">Azure Security Center はこの情報を探すのに最適な場所です。ネットワーク セキュリティ グループ (NSG) が関連付けられていないインターネット接続リソース、およびファイアウォールの背後でセキュリティ保護されていないリソースがわかります。</span><span class="sxs-lookup"><span data-stu-id="62ebe-128">Azure Security Center is a great place to look for this information, as it will identify internet facing resources that don't have network security groups (NSG) associated with them, as well as resources that are not secured behind a firewall.</span></span>

<span data-ttu-id="62ebe-129">境界で受信の保護を行うには 2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="62ebe-129">To provide inbound protection at the perimeter, there are a couple of ways to do this.</span></span> <span data-ttu-id="62ebe-130">Application Gateway はレイヤー 7 のロード バランサーであり、HTTP ベースのサービスに対して高度なセキュリティを提供する Web アプリケーション ファイアウォール (WAF) も含まれています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-130">Application Gateway is a Layer 7 load balancer that also includes a web application firewall (WAF) to provide advanced security for your HTTP-based services.</span></span> <span data-ttu-id="62ebe-131">WAF は、OWASP 3.0 または 2.2.9 コア ルール セットのルールに基づき、クロスサイト スクリプティングや SQL インジェクションなどのよく知られた脆弱性からの保護を提供します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-131">The WAF is based on rules from the OWASP 3.0 or 2.2.9 core rule sets, and provides protection from common-known vulnerabilities such as cross-site scripting and SQL injection.</span></span>

![WAF を使用する Application Gateway](../media-draft/appgw-waf.png)

<span data-ttu-id="62ebe-133">非 HTTP ベースのサービスの保護の場合、またはさらにカスタマイズする場合は、ネットワーク仮想アプライアンス (NVA) を使用してネットワーク リソースをセキュリティ保護できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-133">For protection of non-HTTP-based services or for increased customization, network virtual appliances (NVA) can be used to secure your network resources.</span></span> <span data-ttu-id="62ebe-134">NVA は、オンプレミス ネットワークで使用されていることがあるファイアウォール アプライアンスに似ており、最も一般的な多くのネットワーク セキュリティ ベンダーから入手できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-134">NVAs are similar to firewall appliances you might find in on-premises networks, and are available from many of the most popular network security vendors.</span></span> <span data-ttu-id="62ebe-135">NVA は、それを必要とするアプリケーション用にセキュリティを詳細にカスタマイズできますが、複雑さが増すことがあるため、要件を慎重に検討することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="62ebe-135">NVAs can provide greater customization of security for those applications that require it, but can come with increased complexity, so careful consideration of requirements is advised.</span></span>

<span data-ttu-id="62ebe-136">インターネットに公開されているすべてのリソースには、サービス拒否攻撃を受けるリスクがあります。</span><span class="sxs-lookup"><span data-stu-id="62ebe-136">Any resource exposed to the internet is at risk of being attacked by a denial-of-service attack.</span></span> <span data-ttu-id="62ebe-137">この種の攻撃は、大量の要求を送信することによってネットワーク リソースを低速または応答不能にして、ネットワーク リソースを圧倒しようとします。</span><span class="sxs-lookup"><span data-stu-id="62ebe-137">These types of attacks attempt to overwhelm a network resource by sending so many requests that the resource becomes slow or unresponsive.</span></span> <span data-ttu-id="62ebe-138">これらの攻撃を軽減するため、Azure DDoS では、すべての Azure サービスに対する基本的な保護と、ユーザーのリソースに合わせてさらにカスタマイズできる拡張保護が提供されています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-138">To mitigate these attacks, Azure DDoS provides basic protection across all Azure services and enhanced protection for further customization for your resources.</span></span> <span data-ttu-id="62ebe-139">DDoS 保護は、攻撃トラフィックをブロックし、残りのトラフィックをその目的の宛先に転送します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-139">DDoS protection blocks attack traffic and forwards the remaining traffic to its intended destination.</span></span> <span data-ttu-id="62ebe-140">攻撃の検出から数分以内に、Azure Monitor メトリックを使用してユーザーに通知します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-140">Within a few minutes of attack detection, you are notified using Azure Monitor metrics.</span></span>

![DDoS](../media-draft/ddos.png)

### <a name="virtual-network-security"></a><span data-ttu-id="62ebe-142">仮想ネットワークのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="62ebe-142">Virtual network security</span></span>

<span data-ttu-id="62ebe-143">仮想ネットワーク (VNet) の内部の場合は、リソース間の通信を必要なものだけに制限することが重要です。</span><span class="sxs-lookup"><span data-stu-id="62ebe-143">Once inside a virtual network (VNet), it's important to limit communication between resources to only what is required.</span></span>

<span data-ttu-id="62ebe-144">仮想マシン間の通信では、ネットワーク セキュリティ グループ (NSG) が不要な通信を制限するための重要な部分です。</span><span class="sxs-lookup"><span data-stu-id="62ebe-144">For communication between virtual machines, network security groups (NSG) are a critical piece to restrict unnecessary communication.</span></span> <span data-ttu-id="62ebe-145">NSG はレイヤー 3 と 4 で動作し、ネットワーク インターフェイスおよびサブネットとの間の双方向の通信について、許可および拒否するものの一覧を提供します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-145">NSGs operate at layers 3 & 4, and provide a list of allowed and denied communication to and from network interfaces and subnets.</span></span> <span data-ttu-id="62ebe-146">NSG は完全にカスタマイズ可能であり、仮想マシンとやり取りされるネットワーク通信を完全にロックダウンすることができます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-146">NSGs are fully customizable, and give you the ability to fully lock down network communication to and from your virtual machines.</span></span> <span data-ttu-id="62ebe-147">NSG を使用すると、環境間、階層間、およびサービス間でアプリケーションを分離できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-147">By using NSGs, you can isolate applications between environments, tiers, and services.</span></span>

![Azure ネットワーク セキュリティ グループ](../media-draft/azure-network-security.png)

<span data-ttu-id="62ebe-149">Azure サービスを分離して仮想ネットワークからの通信のみを許可するには、VNet サービス エンドポイントを使用します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-149">To isolate Azure services to only allow communication from virtual networks, use VNet service endpoints.</span></span> <span data-ttu-id="62ebe-150">サービス エンドポイントを使用すると、Azure サービス リソースへのアクセスを仮想ネットワークに限定することができます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-150">With service endpoints, Azure service resources can be secured to your virtual network.</span></span> <span data-ttu-id="62ebe-151">サービス リソースを仮想ネットワークに限定することで、リソースへのパブリック インターネット アクセスを完全に排除し、ご利用の仮想ネットワークからのトラフィックのみを許可することにより、セキュリティが向上します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-151">Securing service resources to a virtual network provides improved security by fully removing public internet access to resources, and allowing traffic only from your virtual network.</span></span> <span data-ttu-id="62ebe-152">これにより、環境の攻撃対象領域が減り、VNet と Azure サービスの間の通信を制限するために必要な管理が減り、この通信に最適なルーティングが提供されます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-152">This reduces the attack surface for your environment, reduces the administration required to limit communication between your VNet and Azure services, and provides optimal routing for this communication.</span></span>

### <a name="network-integration"></a><span data-ttu-id="62ebe-153">ネットワーク統合</span><span class="sxs-lookup"><span data-stu-id="62ebe-153">Network integration</span></span>

<span data-ttu-id="62ebe-154">一般に、オンプレミス ネットワークからの通信を提供したり、Azure 内のサービス間の通信をよりよくするには、既存のネットワーク インフラストラクチャを統合する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62ebe-154">It's common to have existing network infrastructure that needs to be integrated to provide communication from on-premises networks, or to provide improved communication between services in Azure.</span></span> <span data-ttu-id="62ebe-155">この統合を処理してネットワークのセキュリティを強化するには、いくつかの主要な方法があります。</span><span class="sxs-lookup"><span data-stu-id="62ebe-155">There are a few key ways to handle this integration and improve the security of your network.</span></span>

<span data-ttu-id="62ebe-156">仮想プライベート ネットワーク (VPN) 接続は、ネットワーク間に安全な通信チャネルを確立する一般的な方法であり、Azure で仮想ネットワークを使用する場合も同じです。</span><span class="sxs-lookup"><span data-stu-id="62ebe-156">Virtual private network (VPN) connections are a common way of establishing secure communication channels between networks, and this is no different when working with virtual networking on Azure.</span></span> <span data-ttu-id="62ebe-157">Azure VNet とオンプレミスの VPN デバイス間の接続は、ユーザーのネットワークと Azure 上のユーザーの仮想マシンの間にセキュリティ保護された通信を提供する優れた方法です。</span><span class="sxs-lookup"><span data-stu-id="62ebe-157">Connection between Azure VNets and an on-premises VPN device is a great way to provide secure communication between your network and your virtual machines on Azure.</span></span>

<span data-ttu-id="62ebe-158">ネットワークと Azure の間に専用のプライベート接続を提供するには、ExpressRoute を使用できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-158">To provide a dedicated, private connection between your network and Azure, you can use ExpressRoute.</span></span> <span data-ttu-id="62ebe-159">ExpressRoute を利用すると、接続プロバイダーが提供するプライベート接続を介して、オンプレミスのネットワークを Microsoft クラウドに拡張できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-159">ExpressRoute lets you extend your on-premises networks into the Microsoft cloud over a private connection facilitated by a connectivity provider.</span></span> <span data-ttu-id="62ebe-160">ExpressRoute では、Microsoft Azure、Office 365、Dynamics 365 などの Microsoft クラウド サービスへの接続を確立できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-160">With ExpressRoute, you can establish connections to Microsoft cloud services, such as Microsoft Azure, Office 365, and Dynamics 365.</span></span> <span data-ttu-id="62ebe-161">このようにすると、インターネット経由の代わりにプライベート回線でこのトラフィックを送信することによって、オンプレミスの通信のセキュリティが向上します。</span><span class="sxs-lookup"><span data-stu-id="62ebe-161">This improves the security of your on-premises communication by sending this traffic over the private circuit instead of over the internet.</span></span> <span data-ttu-id="62ebe-162">エンド ユーザーにインターネットを介したこれらのサービスへのアクセスを許可する必要はなく、アプライアンス経由でこのトラフィックを送信することによりトラフィックをさらに検査できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-162">You don't need to allow access to these services for your end users over the internet, and can send this traffic through appliances for further traffic inspection.</span></span>

![ExpressRoute](../media-draft/expressroute-connection-overview.png)

<span data-ttu-id="62ebe-164">Azure 内の複数の VNet を簡単に統合するため、VNet ピアリングでは指定した VNet 間に直接接続が確立されます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-164">To easily integrate multiple VNets in Azure, VNet peering establishes a direct connection between designated VNets.</span></span> <span data-ttu-id="62ebe-165">確立された後は、VNet 内のリソースを保護するのと同じ方法で、NSG を使用してリソース間の分離を提供できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-165">Once established, you can use NSGs to provide isolation between resources in the same way you secure resources within a VNet.</span></span> <span data-ttu-id="62ebe-166">この統合では、ピアリングされた VNet 間に同じ基本的なセキュリティ レイヤーを提供できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-166">This integration gives you the ability to provide the same fundamental layer of security across any peered VNets.</span></span> <span data-ttu-id="62ebe-167">直接接続された VNet 間の通信だけが許可されます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-167">Communication is only allowed between directly connected VNets.</span></span>

## <a name="network-security-at-lamna-healthcare"></a><span data-ttu-id="62ebe-168">Lamna Healthcare でのネットワークのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="62ebe-168">Network security at Lamna Healthcare</span></span>

<span data-ttu-id="62ebe-169">Lamna Healthcare は、これらのサービスの多くを利用して、セキュリティ保護されたネットワーク インフラストラクチャを構築しています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-169">Lamna Healthcare has taken advantage of many of these services to build out a secure network infrastructure.</span></span> <span data-ttu-id="62ebe-170">リソース間の通信は既定では拒否され、必要な場合にのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-170">Communication between resources is denied by default, and allowed only when required.</span></span> <span data-ttu-id="62ebe-171">インターネットからの受信接続は、それを必要とするサービスに対してのみ有効になります。RDP と SSH はインターネット エンドポイントからは許可されず、信頼されている内部リソースからのみ許可されます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-171">Inbound connectivity from the internet is enabled only for services that require it; RDP and SSH are not permitted from internet endpoints, only from trusted internal resources.</span></span>

<span data-ttu-id="62ebe-172">インターネットに接続する Web サービスをセキュリティで保護するため、それらは WAF を有効にした Application Gateway の背後に配置されています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-172">To secure their internet facing web services, they place them behind Application Gateways with WAF enabled.</span></span> <span data-ttu-id="62ebe-173">仮想マシンで実行されているサービスと App Service の両方についてそのようになっています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-173">This is true both for services running on virtual machines as well as on App Service.</span></span> <span data-ttu-id="62ebe-174">Application Gateway を使用することにより、多くの一般的な脆弱性から保護しています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-174">By using Application Gateways, they have protection from many of the common vulnerabilities.</span></span>

<span data-ttu-id="62ebe-175">DDoS 標準を有効にして、インターネットに接続するエンドポイントをサービス拒否攻撃から保護しています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-175">They have DDoS standard enabled, to provide protection for their internet facing endpoints from denial-of-service attacks.</span></span>

<span data-ttu-id="62ebe-176">NSG を使用することで、アプリケーション サービス間および環境間の通信を完全に分離できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-176">Through the use of NSGs, they are able to fully isolate communication between application services and between environments.</span></span> <span data-ttu-id="62ebe-177">環境内のサービス間では必要な通信のみを許可しており、運用環境と非運用環境の間のアクセスは許可していません。</span><span class="sxs-lookup"><span data-stu-id="62ebe-177">They only allow the necessary communication between services within an environment, and no access is allowed between production and non-production environments.</span></span>

<span data-ttu-id="62ebe-178">エンド ユーザーと Azure のアプリケーションの間に専用接続を提供するため、オンプレミスのネットワークと接続する ExpressRoute 回線をプロビジョニングしています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-178">To provide dedicated connectivity between their end users and applications in Azure, they have provisioned an ExpressRoute circuit with connectivity to their on-premises network.</span></span> <span data-ttu-id="62ebe-179">これにより、Azure へのトラフィックはインターネットを経由せず、Azure 内のサービスがシステムと通信するためのプライベート接続はオンプレミスに留まっています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-179">This keeps their traffic to Azure off the internet and a private connection for their services in Azure to communicate with systems remaining on-premises.</span></span>

<span data-ttu-id="62ebe-180">この方法により、Lamna Healthcare は Azure のサービスを利用して、ネットワーク インフラストラクチャの複数のレイヤーでセキュリティを提供しています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-180">With this approach, Lamna Healthcare has leveraged Azure services to provide security at multiple layers of their network infrastructure.</span></span>

## <a name="summary"></a><span data-ttu-id="62ebe-181">まとめ</span><span class="sxs-lookup"><span data-stu-id="62ebe-181">Summary</span></span>

<span data-ttu-id="62ebe-182">ネットワーク セキュリティに対する階層型アプローチは、ネットワーク ベースの攻撃による露出のリスクを減らすのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-182">A layered approach to network security helps reduce your risk of exposure through network-based attacks.</span></span> <span data-ttu-id="62ebe-183">Azure では、インターネットに接続されたリソース、内部リソース、およびオンプレミス ネットワーク間の通信をセキュリティ保護するための、複数のサービスと機能が提供されています。</span><span class="sxs-lookup"><span data-stu-id="62ebe-183">Azure provides several services and capabilities to secure your internet facing resource, internal resources, and communication between on-premises networks.</span></span> <span data-ttu-id="62ebe-184">これらの機能を使用すれば、Azure 上でセキュリティ保護されたソリューションを作成できます。</span><span class="sxs-lookup"><span data-stu-id="62ebe-184">These features make it possible to create secure solutions on Azure.</span></span>