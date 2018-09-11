<span data-ttu-id="c214a-101">企業では機密性の高いデータを扱い、Azure に格納される情報が大量にあるため、パブリック インターネット上での接続のセキュリティと信頼性についていくつかの考慮事項があります。</span><span class="sxs-lookup"><span data-stu-id="c214a-101">As your company deals with highly-sensitive data and has large amounts of information it will store in Azure, there are some concerns about the security and reliability of connections over the public internet.</span></span> <span data-ttu-id="c214a-102">企業は、より高いレベルの接続性、セキュリティ、信頼性を実証できるまで、Azure への大規模な移行を望みません。</span><span class="sxs-lookup"><span data-stu-id="c214a-102">The company is not willing to migrate wholesale to Azure unless it can demonstrate higher levels of connectivity, security, and reliability.</span></span>

<span data-ttu-id="c214a-103">ここでは、Azure データ センターに専用回線で直接接続されたパブリック インターネット上で実行される接続の先に進みます。</span><span class="sxs-lookup"><span data-stu-id="c214a-103">Here, we will go beyond connections that run over the public internet to dedicated lines direct into the Azure data centers.</span></span>

## <a name="azure-expressroute"></a><span data-ttu-id="c214a-104">Azure ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="c214a-104">Azure ExpressRoute</span></span>

<span data-ttu-id="c214a-105">Microsoft Azure ExpressRoute を利用すれば、組織は接続プロバイダーが実装するプライベート接続上で、オンプレミスのネットワークを Microsoft Cloud に拡張できます。</span><span class="sxs-lookup"><span data-stu-id="c214a-105">Microsoft Azure ExpressRoute enables organizations to extend their on-premises networks into the Microsoft cloud over a private connection implemented by a connectivity provider.</span></span> <span data-ttu-id="c214a-106">この配置は、Azure データ センターへの接続が、インターネット経由ではなく専用リンク経由であることを意味します。</span><span class="sxs-lookup"><span data-stu-id="c214a-106">This arrangement means that the connectivity to the Azure data centers does not go over the internet but across a dedicated link.</span></span> <span data-ttu-id="c214a-107">ExpressRoute は、Office 365 や Dynamics 365 などの他の Microsoft Cloud ベース サービスとの効率的な接続も促進します。</span><span class="sxs-lookup"><span data-stu-id="c214a-107">ExpressRoute also facilitates efficient connections with other Microsoft cloud-based services, such as Office 365 and Dynamics 365.</span></span>

<span data-ttu-id="c214a-108">ExpressRoute が提供する利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c214a-108">Advantages that ExpressRoute provides include:</span></span>

- <span data-ttu-id="c214a-109">帯域幅の動的スケーリングがある 50 Mbps から 10 Gbps のより速い速度</span><span class="sxs-lookup"><span data-stu-id="c214a-109">Faster speeds, from 50 Mbps to 10 Gbps, with dynamic bandwidth scaling</span></span>

- <span data-ttu-id="c214a-110">より短い待機時間</span><span class="sxs-lookup"><span data-stu-id="c214a-110">Lower latency</span></span>

- <span data-ttu-id="c214a-111">組み込みピアリングを通じたより高い信頼性</span><span class="sxs-lookup"><span data-stu-id="c214a-111">Greater reliability though built-in peering</span></span>

- <span data-ttu-id="c214a-112">より高度なセキュリティ</span><span class="sxs-lookup"><span data-stu-id="c214a-112">Highly secure</span></span>

<span data-ttu-id="c214a-113">ExpressRoute には、さらに次のような利点もあります。</span><span class="sxs-lookup"><span data-stu-id="c214a-113">ExpressRoute brings a number of further benefits, such as:</span></span>

- <span data-ttu-id="c214a-114">サポートされているすべての Azure サービスへの接続性</span><span class="sxs-lookup"><span data-stu-id="c214a-114">Connectivity to all supported Azure services</span></span>

- <span data-ttu-id="c214a-115">すべてのリージョンへのグローバル接続 (Premium アドオンが必要)</span><span class="sxs-lookup"><span data-stu-id="c214a-115">Global connectivity to all regions (requires premium add-on)</span></span>

- <span data-ttu-id="c214a-116">Border Gateway Protocol 経由での動的ルーティング</span><span class="sxs-lookup"><span data-stu-id="c214a-116">Dynamic routing over Border Gateway Protocol</span></span>

- <span data-ttu-id="c214a-117">接続アップタイムに関するサービス レベル アグリーメント (SLA)</span><span class="sxs-lookup"><span data-stu-id="c214a-117">Service Level Agreements (SLA) for connection uptime</span></span>

- <span data-ttu-id="c214a-118">Skype for Business のサービスの品質 (QoS)</span><span class="sxs-lookup"><span data-stu-id="c214a-118">Quality of Service (QoS) for Skype for Business</span></span>

<span data-ttu-id="c214a-119">さらに、ルート制限の増加、グローバル サービスの接続、回線ごとの vNet リンクの増加などのメリットを提供する ExpressRoute Premium アドオンがあります。</span><span class="sxs-lookup"><span data-stu-id="c214a-119">Additionally, there is the ExpressRoute premium add-on, which offers benefits such as increased route limits, global service connectivity, and increased vNet links per circuit.</span></span>

## <a name="expressroute-connectivity-models"></a><span data-ttu-id="c214a-120">ExpressRoute 接続モデル</span><span class="sxs-lookup"><span data-stu-id="c214a-120">ExpressRoute Connectivity Models</span></span>

<span data-ttu-id="c214a-121">ExpressRoute への接続には、次のメカニズムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="c214a-121">Connections into ExpressRoute can be through the following mechanisms:</span></span>

- <span data-ttu-id="c214a-122">IP VPN ネットワーク (任意の環境間)</span><span class="sxs-lookup"><span data-stu-id="c214a-122">IP VPN network (any-to-any)</span></span>

- <span data-ttu-id="c214a-123">イーサネット交換による仮想交差接続</span><span class="sxs-lookup"><span data-stu-id="c214a-123">Virtual cross-connection through an Ethernet exchange</span></span>

- <span data-ttu-id="c214a-124">ポイント ツー ポイントのイーサネット接続</span><span class="sxs-lookup"><span data-stu-id="c214a-124">Point-to-point Ethernet connection</span></span>

 <span data-ttu-id="c214a-125">ExpressRoute の機能は上記の接続モデルのすべてに共通しています。</span><span class="sxs-lookup"><span data-stu-id="c214a-125">ExpressRoute capabilities and features are all identical across all of the above connectivity models.</span></span>

### <a name="what-is-layer-3-connectivity"></a><span data-ttu-id="c214a-126">レイヤー 3 接続とは</span><span class="sxs-lookup"><span data-stu-id="c214a-126">What is layer 3 connectivity?</span></span>

<span data-ttu-id="c214a-127">Microsoft は業界標準の動的ルーティング プロトコル (BGP) を利用し、オンプレミス ネットワーク、Azure のインスタンス、および Microsoft パブリック アドレスの間のルートを交換します。</span><span class="sxs-lookup"><span data-stu-id="c214a-127">Microsoft uses industry standard dynamic routing protocol (BGP) to exchange routes between your on-premises network, your instances in Azure, and Microsoft public addresses.</span></span> <span data-ttu-id="c214a-128">さまざまなトラフィック プロファイルに合わせ、ネットワークとさまざまな BGP セッションを確立します。</span><span class="sxs-lookup"><span data-stu-id="c214a-128">We establish multiple BGP sessions with your network for different traffic profiles.</span></span>
### <a name="any-to-any-ipvpn-networks"></a><span data-ttu-id="c214a-129">任意の環境間 (IPVPN) ネットワーク</span><span class="sxs-lookup"><span data-stu-id="c214a-129">Any-to-any (IPVPN) networks</span></span>

<span data-ttu-id="c214a-130">通常、IPVPN プロバイダーは、マネージド レイヤー 3 接続経由でブランチ オフィスと会社のデータ センター間の接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="c214a-130">IPVPN providers typically provide connectivity between branch offices and your corporate data center over managed layer 3 connections.</span></span> <span data-ttu-id="c214a-131">ExpressRoute では、Azure データ センターは別のブランチ オフィスであるかのように見えます。</span><span class="sxs-lookup"><span data-stu-id="c214a-131">With ExpressRoute, the Azure data centers appear as if they were another branch office.</span></span>

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a><span data-ttu-id="c214a-132">イーサネット交換による仮想交差接続</span><span class="sxs-lookup"><span data-stu-id="c214a-132">Virtual Cross-connection through an Ethernet Exchange</span></span>

<span data-ttu-id="c214a-133">組織がクラウド エクスチェンジ施設に併置されている場合は、プロバイダーのイーサネット交換を通じて Microsoft Cloud への交差接続を要求します。</span><span class="sxs-lookup"><span data-stu-id="c214a-133">If your organization is co-located with a cloud exchange facility, you request cross-connections to the Microsoft cloud though your provider's Ethernet exchange.</span></span> <span data-ttu-id="c214a-134">Microsoft Cloud へのこれら交差接続は、ネットワーク OSI モデルの場合と同様に、レイヤー 2 接続またはレイヤー 3 マネージド接続で動作できます。</span><span class="sxs-lookup"><span data-stu-id="c214a-134">These cross-connections to the Microsoft cloud can operate either at Layer 2 or Layer 3 managed connections, as in the networking OSI model.</span></span>

### <a name="point-to-point-ethernet-connection"></a><span data-ttu-id="c214a-135">ポイント ツー ポイントのイーサネット接続</span><span class="sxs-lookup"><span data-stu-id="c214a-135">Point-to-point Ethernet Connection</span></span>

<span data-ttu-id="c214a-136">ポイント ツー ポイントのイーサネット リンクでは、Microsoft Cloud にオンプレミス データ センターまたはオフィス間のレイヤー 2 接続またはマネージド レイヤー 3 接続を提供できます。</span><span class="sxs-lookup"><span data-stu-id="c214a-136">Point-to-point Ethernet links can provide Layer 2 or managed Layer 3 connections between your on-premises data centers or offices to the Microsoft Cloud.</span></span>

## <a name="how-expressroute-works"></a><span data-ttu-id="c214a-137">ExpressRoute のしくみ</span><span class="sxs-lookup"><span data-stu-id="c214a-137">How ExpressRoute Works</span></span>

<span data-ttu-id="c214a-138">Azure ExpressRoute は、ExpressRoute 回線とルーティング ドメインの組み合わせを使用して Microsoft Cloud への高帯域幅接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="c214a-138">Azure ExpressRoute uses a combination of ExpressRoute circuits and routing domains to provide high-bandwidth connectivity to the Microsoft cloud.</span></span>

### <a name="what-are-expressroute-circuits"></a><span data-ttu-id="c214a-139">ExpressRoute 回線とは</span><span class="sxs-lookup"><span data-stu-id="c214a-139">What are ExpressRoute Circuits</span></span>

<span data-ttu-id="c214a-140">ExpressRoute 回線は、オンプレミス インフラストラクチャと Microsoft Cloud 間の論理接続です。</span><span class="sxs-lookup"><span data-stu-id="c214a-140">An ExpressRoute circuit is the logical connection between your on-premises infrastructure and the Microsoft cloud.</span></span> <span data-ttu-id="c214a-141">接続プロバイダーがその接続を実装しますが、組織によっては冗長性を確保するために複数の接続プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="c214a-141">A connectivity provider implements that connection, although some organizations use multiple connectivity providers for redundancy reasons.</span></span> <span data-ttu-id="c214a-142">各回線には 50、100、200、500 Mbps、1 または 10 Gbps の固定の帯域幅があり、これらの回線それぞれが接続プロバイダーとピアリングの場所にマップされます。</span><span class="sxs-lookup"><span data-stu-id="c214a-142">Each circuit has a fixed bandwidth of either 50, 100, 200 or 500 Mbps, or 1 or 10 Gbps, and each of those circuits map to a connectivity provider and a peering location.</span></span> <span data-ttu-id="c214a-143">また、各 ExpressRoute 回線には既定のクォータと制限があります。</span><span class="sxs-lookup"><span data-stu-id="c214a-143">In addition, each ExpressRoute circuit has default quotas and limits.</span></span>

<span data-ttu-id="c214a-144">ExpressRoute 回線は、ネットワーク接続またはネットワーク デバイスと同じではありません。</span><span class="sxs-lookup"><span data-stu-id="c214a-144">An ExpressRoute circuit is not equivalent to a network connection or a network device.</span></span> <span data-ttu-id="c214a-145">各回線は "_サービス_" または "_s キー_" という GUID で定義されます。</span><span class="sxs-lookup"><span data-stu-id="c214a-145">Each circuit is defined by a GUID, called a _service_ or _s- key_.</span></span> <span data-ttu-id="c214a-146">この s キーは、Microsoft、接続プロバイダー、および組織間の接続リンクを提供します。暗号化シークレットではありません。</span><span class="sxs-lookup"><span data-stu-id="c214a-146">This s-key provides the connectivity link between Microsoft, your connectivity provider, and your organization - it is not a cryptographic secret.</span></span> <span data-ttu-id="c214a-147">各 s キーには、Azure ExpressRoute 回線への 1 対 1 マッピングがあります。</span><span class="sxs-lookup"><span data-stu-id="c214a-147">Each s-key has a one-to-one mapping to an Azure ExpressRoute circuit.</span></span>

<span data-ttu-id="c214a-148">各回線は、冗長性を確保するために構成されている BGP セッションのペアであるピアリングを最大 3 つ持つことができます。これらは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="c214a-148">Each circuit can have up to three peerings, which are a pair of BGP sessions that are configured for redundancy, they are:</span></span>

- <span data-ttu-id="c214a-149">Azure プライベート</span><span class="sxs-lookup"><span data-stu-id="c214a-149">Azure Private</span></span>
- <span data-ttu-id="c214a-150">Azure パブリック</span><span class="sxs-lookup"><span data-stu-id="c214a-150">Azure Public</span></span>
- <span data-ttu-id="c214a-151">Microsoft</span><span class="sxs-lookup"><span data-stu-id="c214a-151">Microsoft</span></span>

### <a name="routing-domains"></a><span data-ttu-id="c214a-152">ルーティング ドメイン</span><span class="sxs-lookup"><span data-stu-id="c214a-152">Routing Domains</span></span>

<span data-ttu-id="c214a-153">ExpressRoute 回線はルーティング ドメインにマップされ、各 ExpressRoute 回線に複数のルーティング ドメインがあります。これらのドメインは上に示した 3 つのピアリングと同じです。</span><span class="sxs-lookup"><span data-stu-id="c214a-153">ExpressRoute circuits then map to routing domains, with each ExpressRoute circuit having multiple routing domains, these domains being the same as the three peerings listed above.</span></span> <span data-ttu-id="c214a-154">アクティブ/アクティブ構成では、ルーターの各ペアの各ルーティング ドメインの構成が同じであるため、高可用性が実現されます。</span><span class="sxs-lookup"><span data-stu-id="c214a-154">In an active-active configuration, each pair of routers would have each routing domain configured identically, thus providing high availability.</span></span> <span data-ttu-id="c214a-155">Azure パブリックおよび Azure プライベート ピアリング名は IP アドレス指定スキームを表します。</span><span class="sxs-lookup"><span data-stu-id="c214a-155">The Azure Public and Azure Private peering names represent the IP addressing schemes.</span></span>

#### <a name="azure-private-peering"></a><span data-ttu-id="c214a-156">Azure プライベート ピアリング</span><span class="sxs-lookup"><span data-stu-id="c214a-156">Azure Private Peering</span></span>

<span data-ttu-id="c214a-157">Azure プライベート ピアリングは、仮想ネットワークでデプロイされている仮想マシンやクラウド サービスなどの Azure コンピューティング サービスに接続します。</span><span class="sxs-lookup"><span data-stu-id="c214a-157">Azure Private Peering connects to Azure compute services such as virtual machines and cloud services that are deployed with a virtual network.</span></span> <span data-ttu-id="c214a-158">セキュリティに関しては、プライベート ピアリング ドメインは単にオンプレミス ネットワークを Azure に拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="c214a-158">As far as security goes, the private peering domain is simply an extension of your on-premises network into Azure.</span></span> <span data-ttu-id="c214a-159">そのネットワークと Azure の仮想ネットワーク間の双方向接続を有効にし、内部ネットワーク内で Azure VM の IP アドレスを公開します。</span><span class="sxs-lookup"><span data-stu-id="c214a-159">You then enable bidirectional connectivity between that network and any Azure virtual networks, making the Azure VM IP addresses visible within your internal network.</span></span>

> [!NOTE]
> <span data-ttu-id="c214a-160">プライベート ピアリング ドメインには 1 つの仮想ネットワークのみを接続できます</span><span class="sxs-lookup"><span data-stu-id="c214a-160">You can only connect ONE virtual network to the private peering domain</span></span>

#### <a name="azure-public-peering"></a><span data-ttu-id="c214a-161">Azure パブリック ピアリング</span><span class="sxs-lookup"><span data-stu-id="c214a-161">Azure Public Peering</span></span>

<span data-ttu-id="c214a-162">Azure パブリック ピアリングは、Azure Storage、Azure SQL データベース、Azure Web サービスなど、パブリック IP アドレスで使用可能なサービスへのプライベート接続を有効にします。</span><span class="sxs-lookup"><span data-stu-id="c214a-162">Azure Public Peering enables private connections to services that are available on public IP addresses, such as Azure Storage, Azure SQL databases, and Azure Web services.</span></span> <span data-ttu-id="c214a-163">パブリック ピアリングでは、トラフィックをインターネット経由でルーティングせずに、それらのサービスのパブリック IP アドレスに接続できます。</span><span class="sxs-lookup"><span data-stu-id="c214a-163">With public peering, you can connect to those service public IP addresses without your traffic being routed over the internet.</span></span> <span data-ttu-id="c214a-164">接続は常に WAN から Azure であり、他の方法では行われません。</span><span class="sxs-lookup"><span data-stu-id="c214a-164">Connectivity is always from your WAN to Azure, not the other way around.</span></span> <span data-ttu-id="c214a-165">また、パブリック ピアリングを有効にするサービスを選択することはできないので、これはオールオアナッシングのアプローチです。</span><span class="sxs-lookup"><span data-stu-id="c214a-165">This is also an all-or-nothing approach, as you cannot select which services for which you want public peering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="c214a-166">Azure PaaS サービスでは、パブリック ピアリングではなく Microsoft ピアリングを使用することをお勧めします</span><span class="sxs-lookup"><span data-stu-id="c214a-166">For Azure PaaS services, it's recommended to use Microsoft peering rather than public peering</span></span>

#### <a name="microsoft-peering"></a><span data-ttu-id="c214a-167">Microsoft ピアリング</span><span class="sxs-lookup"><span data-stu-id="c214a-167">Microsoft Peering</span></span>

<span data-ttu-id="c214a-168">Microsoft ピアリングでは、Office 365 や Dynamics 365 などのクラウド ベースの SaaS サービスへの接続がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="c214a-168">Microsoft Peering supports connections to cloud-based SaaS offerings, such as Office 365 and Dynamics 365.</span></span> <span data-ttu-id="c214a-169">このピアリング オプションでは、会社の WAN と Microsoft クラウド サービス間の双方向接続が提供されます。</span><span class="sxs-lookup"><span data-stu-id="c214a-169">This peering option provides bi-directional connectivity between your company's WAN and Microsoft cloud services.</span></span>

### <a name="expressroute-health"></a><span data-ttu-id="c214a-170">ExpressRoute の正常性</span><span class="sxs-lookup"><span data-stu-id="c214a-170">ExpressRoute Health</span></span>

<span data-ttu-id="c214a-171">Microsoft Azure のほとんどの機能と同様に、ExpressRoute 接続を監視して、正常に実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="c214a-171">As with most features in Microsoft Azure, you can monitor ExpressRoute connections to ensure that they are performing satisfactorily.</span></span> <span data-ttu-id="c214a-172">監視には、次の領域の対象範囲が含まれます。</span><span class="sxs-lookup"><span data-stu-id="c214a-172">Monitoring includes coverage of the following areas:</span></span>

- <span data-ttu-id="c214a-173">可用性</span><span class="sxs-lookup"><span data-stu-id="c214a-173">Availability</span></span>
- <span data-ttu-id="c214a-174">VNet への接続</span><span class="sxs-lookup"><span data-stu-id="c214a-174">Connectivity to VNets</span></span>
- <span data-ttu-id="c214a-175">帯域幅の使用率</span><span class="sxs-lookup"><span data-stu-id="c214a-175">Bandwidth utilization</span></span>

<span data-ttu-id="c214a-176">この監視アクティビティの主要ツールは Network Performance Monitor (特に ExpressRoute 用の NPM) です。</span><span class="sxs-lookup"><span data-stu-id="c214a-176">The key tool for this monitoring activity is Network Performance Monitor, particularly NPM for ExpressRoute.</span></span>

## <a name="summary"></a><span data-ttu-id="c214a-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="c214a-177">Summary</span></span>

<span data-ttu-id="c214a-178">Azure ExpressRoute は、Azure のデータセンターとオンプレミスや共用環境にあるインフラストラクチャの間でプライベート接続を作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="c214a-178">Azure ExpressRoute is used to create private connections between Azure datacenters and infrastructure on your premises or in a colocation environment.</span></span> <span data-ttu-id="c214a-179">ExpressRoute 接続はパブリック インターネットを経由しないので、一般的なインターネット接続と比べて信頼性が高く、高速で、待ち時間も短くなります。</span><span class="sxs-lookup"><span data-stu-id="c214a-179">ExpressRoute connections don't go over the public internet, and they offer more reliability, faster speeds, and lower latencies than typical internet connections.</span></span>
