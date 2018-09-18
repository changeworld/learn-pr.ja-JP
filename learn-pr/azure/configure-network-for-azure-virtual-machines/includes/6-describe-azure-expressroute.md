<span data-ttu-id="e21c5-101">企業では機密性の高いデータを扱い、Azure に格納される情報が大量にあるため、パブリック インターネット上での接続のセキュリティと信頼性についていくつかの考慮事項があります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-101">As your company deals with highly sensitive data and has large amounts of information it will store in Azure, there are some concerns about the security and reliability of connections over the public internet.</span></span> <span data-ttu-id="e21c5-102">企業は、より高いレベルの接続性、セキュリティ、信頼性を実証できるまで、Azure への大規模な移行を望みません。</span><span class="sxs-lookup"><span data-stu-id="e21c5-102">The company is not willing to migrate wholesale to Azure unless it can demonstrate higher levels of connectivity, security, and reliability.</span></span>

<span data-ttu-id="e21c5-103">ここでは、パブリック インターネット経由ではなく、専用回線で Azure データセンターに直接接続します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-103">Here, we will go beyond connections that run over the public internet to dedicated lines direct into the Azure datacenters.</span></span>

## <a name="azure-expressroute"></a><span data-ttu-id="e21c5-104">Azure ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="e21c5-104">Azure ExpressRoute</span></span>

<span data-ttu-id="e21c5-105">Microsoft Azure ExpressRoute を利用すれば、組織では接続プロバイダーによって実装されるプライベート接続で、オンプレミスのネットワークを Microsoft Cloud に拡張できます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-105">Microsoft Azure ExpressRoute enables organizations to extend their on-premises networks into the Microsoft Cloud over a private connection implemented by a connectivity provider.</span></span> <span data-ttu-id="e21c5-106">この配置は、Azure データセンターへの接続がインターネット経由ではなく、専用リンク経由であることを意味します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-106">This arrangement means that the connectivity to the Azure datacenters does not go over the internet but across a dedicated link.</span></span> <span data-ttu-id="e21c5-107">ExpressRoute により、Office 365 や Dynamics 365 などの他の Microsoft のクラウドベース サービスとの効率的な接続も容易になります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-107">ExpressRoute also facilitates efficient connections with other Microsoft cloud-based services, such as Office 365 and Dynamics 365.</span></span>

<span data-ttu-id="e21c5-108">ExpressRoute が提供する利点は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e21c5-108">Advantages that ExpressRoute provides include:</span></span>

- <span data-ttu-id="e21c5-109">帯域幅の動的スケーリングがある 50 Mbps から 10 Gbps のより速い速度</span><span class="sxs-lookup"><span data-stu-id="e21c5-109">Faster speeds, from 50 Mbps to 10 Gbps, with dynamic bandwidth scaling</span></span>

- <span data-ttu-id="e21c5-110">より短い待機時間</span><span class="sxs-lookup"><span data-stu-id="e21c5-110">Lower latency</span></span>

- <span data-ttu-id="e21c5-111">組み込みピアリングを通じたより高い信頼性</span><span class="sxs-lookup"><span data-stu-id="e21c5-111">Greater reliability though built-in peering</span></span>

- <span data-ttu-id="e21c5-112">より高度なセキュリティ</span><span class="sxs-lookup"><span data-stu-id="e21c5-112">Highly secure</span></span>

<span data-ttu-id="e21c5-113">ExpressRoute には、さらに次のような利点もあります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-113">ExpressRoute brings a number of further benefits, such as:</span></span>

- <span data-ttu-id="e21c5-114">サポートされているすべての Azure サービスへの接続性</span><span class="sxs-lookup"><span data-stu-id="e21c5-114">Connectivity to all supported Azure services</span></span>

- <span data-ttu-id="e21c5-115">すべてのリージョンへのグローバル接続 (Premium アドオンが必要)</span><span class="sxs-lookup"><span data-stu-id="e21c5-115">Global connectivity to all regions (requires premium add-on)</span></span>

- <span data-ttu-id="e21c5-116">Border Gateway Protocol 経由での動的ルーティング</span><span class="sxs-lookup"><span data-stu-id="e21c5-116">Dynamic routing over Border Gateway Protocol</span></span>

- <span data-ttu-id="e21c5-117">接続アップタイムに関するサービス レベル アグリーメント (SLA)</span><span class="sxs-lookup"><span data-stu-id="e21c5-117">Service-level agreements (SLAs) for connection uptime</span></span>

- <span data-ttu-id="e21c5-118">Skype for Business のサービスの品質 (QoS)</span><span class="sxs-lookup"><span data-stu-id="e21c5-118">Quality of Service (QoS) for Skype for Business</span></span>

<span data-ttu-id="e21c5-119">さらに、ルート制限の増加、グローバル サービスの接続、回線ごとの仮想ネットワーク リンクの増加などのメリットを提供する ExpressRoute Premium アドオンがあります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-119">Additionally, there is the ExpressRoute premium add-on, which offers benefits such as increased route limits, global service connectivity, and increased virtual network links per circuit.</span></span>

## <a name="expressroute-connectivity-models"></a><span data-ttu-id="e21c5-120">ExpressRoute 接続モデル</span><span class="sxs-lookup"><span data-stu-id="e21c5-120">ExpressRoute connectivity models</span></span>

<span data-ttu-id="e21c5-121">ExpressRoute への接続には、次のメカニズムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-121">Connections into ExpressRoute can be through the following mechanisms:</span></span>

- <span data-ttu-id="e21c5-122">IP VPN ネットワーク (任意の環境間)</span><span class="sxs-lookup"><span data-stu-id="e21c5-122">IP VPN network (any-to-any)</span></span>

- <span data-ttu-id="e21c5-123">イーサネット交換による仮想交差接続</span><span class="sxs-lookup"><span data-stu-id="e21c5-123">Virtual cross-connection through an Ethernet exchange</span></span>

- <span data-ttu-id="e21c5-124">ポイント ツー ポイントのイーサネット接続</span><span class="sxs-lookup"><span data-stu-id="e21c5-124">Point-to-point Ethernet connection</span></span>

 <span data-ttu-id="e21c5-125">ExpressRoute の機能は上記の接続モデルのすべてに共通しています。</span><span class="sxs-lookup"><span data-stu-id="e21c5-125">ExpressRoute capabilities and features are all identical across all of the above connectivity models.</span></span>

### <a name="what-is-layer-3-connectivity"></a><span data-ttu-id="e21c5-126">レイヤー 3 接続とは</span><span class="sxs-lookup"><span data-stu-id="e21c5-126">What is layer 3 connectivity?</span></span>

<span data-ttu-id="e21c5-127">Microsoft では業界標準の動的ルーティング プロトコル (BGP) を利用して、オンプレミス ネットワーク、Azure のインスタンス、および Microsoft パブリック アドレスの間のルートを交換します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-127">Microsoft uses an industry-standard dynamic routing protocol (BGP) to exchange routes between your on-premises network, your instances in Azure, and Microsoft public addresses.</span></span> <span data-ttu-id="e21c5-128">さまざまなトラフィック プロファイルに合わせ、ネットワークとの複数の BGP セッションを確立します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-128">We establish multiple BGP sessions with your network for different traffic profiles.</span></span>

### <a name="any-to-any-ipvpn-networks"></a><span data-ttu-id="e21c5-129">任意の環境間 (IPVPN) ネットワーク</span><span class="sxs-lookup"><span data-stu-id="e21c5-129">Any-to-any (IPVPN) networks</span></span>

<span data-ttu-id="e21c5-130">通常、IPVPN プロバイダーは、マネージド レイヤー 3 接続経由でブランチ オフィスと会社のデータセンターの間の接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-130">IPVPN providers typically provide connectivity between branch offices and your corporate datacenter over managed layer 3 connections.</span></span> <span data-ttu-id="e21c5-131">ExpressRoute では、Azure データセンターは別のブランチ オフィスであるかのように見えます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-131">With ExpressRoute, the Azure datacenters appear as if they were another branch office.</span></span>

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a><span data-ttu-id="e21c5-132">イーサネット交換による仮想交差接続</span><span class="sxs-lookup"><span data-stu-id="e21c5-132">Virtual cross-connection through an Ethernet Exchange</span></span>

<span data-ttu-id="e21c5-133">組織がクラウド エクスチェンジ施設に併置されている場合は、プロバイダーのイーサネット交換を通じて Microsoft Cloud への交差接続を要求します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-133">If your organization is co-located with a cloud exchange facility, you request cross-connections to the Microsoft Cloud though your provider's Ethernet exchange.</span></span> <span data-ttu-id="e21c5-134">Microsoft Cloud へのこれらの交差接続は、ネットワーク OSI モデルの場合と同様に、レイヤー 2 またはレイヤー 3 マネージド接続で動作できます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-134">These cross-connections to the Microsoft Cloud can operate at either layer 2 or layer 3 managed connections, as in the networking OSI model.</span></span>

### <a name="point-to-point-ethernet-connection"></a><span data-ttu-id="e21c5-135">ポイント ツー ポイントのイーサネット接続</span><span class="sxs-lookup"><span data-stu-id="e21c5-135">Point-to-point Ethernet connection</span></span>

<span data-ttu-id="e21c5-136">ポイント ツー ポイントのイーサネット リンクでは、Microsoft Cloud にオンプレミス データセンターまたはオフィス間のレイヤー 2 またはマネージド レイヤー 3 接続を提供できます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-136">Point-to-point Ethernet links can provide layer 2 or managed layer 3 connections between your on-premises datacenters or offices to the Microsoft Cloud.</span></span>

## <a name="how-expressroute-works"></a><span data-ttu-id="e21c5-137">ExpressRoute のしくみ</span><span class="sxs-lookup"><span data-stu-id="e21c5-137">How ExpressRoute works</span></span>

<span data-ttu-id="e21c5-138">Azure ExpressRoute では、ExpressRoute 回線とルーティング ドメインの組み合わせを使用して Microsoft Cloud への高帯域幅接続が提供されます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-138">Azure ExpressRoute uses a combination of ExpressRoute circuits and routing domains to provide high-bandwidth connectivity to the Microsoft Cloud.</span></span>

### <a name="what-are-expressroute-circuits"></a><span data-ttu-id="e21c5-139">ExpressRoute 回線とは</span><span class="sxs-lookup"><span data-stu-id="e21c5-139">What are ExpressRoute circuits</span></span>

<span data-ttu-id="e21c5-140">ExpressRoute 回線は、オンプレミス インフラストラクチャと Microsoft Cloud 間の論理接続です。</span><span class="sxs-lookup"><span data-stu-id="e21c5-140">An ExpressRoute circuit is the logical connection between your on-premises infrastructure and the Microsoft Cloud.</span></span> <span data-ttu-id="e21c5-141">接続プロバイダーがその接続を実装しますが、組織によっては冗長性を確保するために複数の接続プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-141">A connectivity provider implements that connection, although some organizations use multiple connectivity providers for redundancy reasons.</span></span> <span data-ttu-id="e21c5-142">各回線には 50、100、200、500 Mbps、1 または 10 Gbps の固定の帯域幅があり、これらの回線それぞれが接続プロバイダーとピアリングの場所にマップされます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-142">Each circuit has a fixed bandwidth of either 50, 100, 200 or 500 Mbps, or 1 or 10 Gbps, and each of those circuits map to a connectivity provider and a peering location.</span></span> <span data-ttu-id="e21c5-143">また、各 ExpressRoute 回線には既定のクォータと制限があります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-143">In addition, each ExpressRoute circuit has default quotas and limits.</span></span>

<span data-ttu-id="e21c5-144">ExpressRoute 回線は、ネットワーク接続やネットワーク デバイスと同じではありません。</span><span class="sxs-lookup"><span data-stu-id="e21c5-144">An ExpressRoute circuit is not equivalent to a network connection or a network device.</span></span> <span data-ttu-id="e21c5-145">各回線は_サービス_または _s キー_という GUID で定義されます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-145">Each circuit is defined by a GUID, called a _service_ or _s-key_.</span></span> <span data-ttu-id="e21c5-146">この s キーでは、Microsoft、接続プロバイダー、および組織間の接続リンクが提供されます。これは暗号化シークレットではありません。</span><span class="sxs-lookup"><span data-stu-id="e21c5-146">This s-key provides the connectivity link between Microsoft, your connectivity provider, and your organization - it is not a cryptographic secret.</span></span> <span data-ttu-id="e21c5-147">各 s キーには、Azure ExpressRoute 回線への 1 対 1 のマッピングがあります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-147">Each s-key has a one-to-one mapping to an Azure ExpressRoute circuit.</span></span>

<span data-ttu-id="e21c5-148">各回線は、冗長性を確保するために構成されている BGP セッションのペアであるピアリングを最大 3 つ持つことができます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-148">Each circuit can have up to three peerings, which are a pair of BGP sessions that are configured for redundancy.</span></span> <span data-ttu-id="e21c5-149">そのペアリングは以下のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e21c5-149">They are:</span></span>

- <span data-ttu-id="e21c5-150">Azure プライベート</span><span class="sxs-lookup"><span data-stu-id="e21c5-150">Azure private</span></span>
- <span data-ttu-id="e21c5-151">Azure パブリック</span><span class="sxs-lookup"><span data-stu-id="e21c5-151">Azure public</span></span>
- <span data-ttu-id="e21c5-152">Microsoft</span><span class="sxs-lookup"><span data-stu-id="e21c5-152">Microsoft</span></span>

### <a name="routing-domains"></a><span data-ttu-id="e21c5-153">ルーティング ドメイン</span><span class="sxs-lookup"><span data-stu-id="e21c5-153">Routing domains</span></span>

<span data-ttu-id="e21c5-154">その後、ExpressRoute 回線はルーティング ドメインにマップされます。各 ExpressRoute 回線には複数のルーティング ドメインがあります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-154">ExpressRoute circuits then map to routing domains, with each ExpressRoute circuit having multiple routing domains.</span></span> <span data-ttu-id="e21c5-155">これらのドメインは上にリストした 3 つのピアリングと同じです。</span><span class="sxs-lookup"><span data-stu-id="e21c5-155">These domains are the same as the three peerings listed above.</span></span> <span data-ttu-id="e21c5-156">アクティブ/アクティブ構成では、ルーターの各ペアの各ルーティング ドメインの構成が同じであるため、高可用性が実現されます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-156">In an active-active configuration, each pair of routers would have each routing domain configured identically, thus providing high availability.</span></span> <span data-ttu-id="e21c5-157">Azure パブリックおよび Azure プライベートのピアリング名は、IP アドレス指定スキームを表します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-157">The Azure public and Azure private peering names represent the IP addressing schemes.</span></span>

#### <a name="azure-private-peering"></a><span data-ttu-id="e21c5-158">Azure プライベート ピアリング</span><span class="sxs-lookup"><span data-stu-id="e21c5-158">Azure private peering</span></span>

<span data-ttu-id="e21c5-159">Azure プライベート ピアリングでは、仮想ネットワークでデプロイされている仮想マシンやクラウド サービスなどの Azure コンピューティング サービスに接続します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-159">Azure private peering connects to Azure compute services such as virtual machines and cloud services that are deployed with a virtual network.</span></span> <span data-ttu-id="e21c5-160">セキュリティに関しては、プライベート ピアリング ドメインは単にオンプレミス ネットワークを Azure に拡張したものです。</span><span class="sxs-lookup"><span data-stu-id="e21c5-160">As far as security goes, the private peering domain is simply an extension of your on-premises network into Azure.</span></span> <span data-ttu-id="e21c5-161">その後、そのネットワークと Azure 仮想ネットワーク間の双方向接続を有効にし、内部ネットワーク内で Azure VM の IP アドレスを公開します。</span><span class="sxs-lookup"><span data-stu-id="e21c5-161">You then enable bidirectional connectivity between that network and any Azure virtual networks, making the Azure VM IP addresses visible within your internal network.</span></span>

> [!NOTE]
> <span data-ttu-id="e21c5-162">プライベート ピアリング ドメインには 1 つの仮想ネットワークのみを接続できます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-162">You can connect only one virtual network to the private peering domain.</span></span>

#### <a name="azure-public-peering"></a><span data-ttu-id="e21c5-163">Azure パブリック ピアリング</span><span class="sxs-lookup"><span data-stu-id="e21c5-163">Azure public peering</span></span>

<span data-ttu-id="e21c5-164">Azure パブリック ピアリングでは、Azure Storage、Azure SQL データベース、Azure Web サービスなど、パブリック IP アドレスで使用可能なサービスへのプライベート接続を有効にします。</span><span class="sxs-lookup"><span data-stu-id="e21c5-164">Azure public peering enables private connections to services that are available on public IP addresses, such as Azure Storage, Azure SQL databases, and Azure web services.</span></span> <span data-ttu-id="e21c5-165">パブリック ピアリングでは、トラフィックをインターネット経由でルーティングせずに、それらのサービスのパブリック IP アドレスに接続できます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-165">With public peering, you can connect to those service public IP addresses without your traffic being routed over the internet.</span></span> <span data-ttu-id="e21c5-166">接続は常に WAN から Azure であり、他の方法では行われません。</span><span class="sxs-lookup"><span data-stu-id="e21c5-166">Connectivity is always from your WAN to Azure, not the other way around.</span></span> <span data-ttu-id="e21c5-167">また、パブリック ピアリングを有効にするサービスを選択することはできないので、これはオールオアナッシングのアプローチです。</span><span class="sxs-lookup"><span data-stu-id="e21c5-167">This is also an all-or-nothing approach, as you cannot select the services for which you want public peering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="e21c5-168">Azure PaaS サービスでは、パブリック ピアリングではなく、Microsoft ピアリングを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="e21c5-168">For Azure PaaS services, it's recommended to use Microsoft peering rather than public peering.</span></span>

#### <a name="microsoft-peering"></a><span data-ttu-id="e21c5-169">Microsoft ピアリング</span><span class="sxs-lookup"><span data-stu-id="e21c5-169">Microsoft peering</span></span>

<span data-ttu-id="e21c5-170">Microsoft ピアリングでは、Office 365 や Dynamics 365 などのクラウド ベースの SaaS サービスへの接続がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-170">Microsoft peering supports connections to cloud-based SaaS offerings, such as Office 365 and Dynamics 365.</span></span> <span data-ttu-id="e21c5-171">このピアリング オプションでは、会社の WAN と Microsoft クラウド サービス間の双方向接続が提供されます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-171">This peering option provides bi-directional connectivity between your company's WAN and Microsoft cloud services.</span></span>

### <a name="expressroute-health"></a><span data-ttu-id="e21c5-172">ExpressRoute の正常性</span><span class="sxs-lookup"><span data-stu-id="e21c5-172">ExpressRoute health</span></span>

<span data-ttu-id="e21c5-173">Microsoft Azure のほとんどの機能と同様に、ExpressRoute 接続を監視して、正常に実行されていることを確認できます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-173">As with most features in Microsoft Azure, you can monitor ExpressRoute connections to ensure that they are performing satisfactorily.</span></span> <span data-ttu-id="e21c5-174">監視には、次の領域の対象範囲が含まれます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-174">Monitoring includes coverage of the following areas:</span></span>

- <span data-ttu-id="e21c5-175">可用性</span><span class="sxs-lookup"><span data-stu-id="e21c5-175">Availability</span></span>
- <span data-ttu-id="e21c5-176">仮想ネットワークへの接続</span><span class="sxs-lookup"><span data-stu-id="e21c5-176">Connectivity to virtual networks</span></span>
- <span data-ttu-id="e21c5-177">帯域幅の使用率</span><span class="sxs-lookup"><span data-stu-id="e21c5-177">Bandwidth utilization</span></span>

<span data-ttu-id="e21c5-178">この監視アクティビティの主要ツールは Network Performance Monitor (特に ExpressRoute 用の NPM) です。</span><span class="sxs-lookup"><span data-stu-id="e21c5-178">The key tool for this monitoring activity is Network Performance Monitor, particularly NPM for ExpressRoute.</span></span>

## <a name="summary"></a><span data-ttu-id="e21c5-179">まとめ</span><span class="sxs-lookup"><span data-stu-id="e21c5-179">Summary</span></span>

<span data-ttu-id="e21c5-180">Azure ExpressRoute は、Azure のデータセンターとオンプレミスや共用環境にあるインフラストラクチャの間でプライベート接続を作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="e21c5-180">Azure ExpressRoute is used to create private connections between Azure datacenters and infrastructure on your premises or in a colocation environment.</span></span> <span data-ttu-id="e21c5-181">ExpressRoute 接続はパブリック インターネットを経由しないので、一般的なインターネット接続と比べて信頼性が高く、高速で、待ち時間も短くなります。</span><span class="sxs-lookup"><span data-stu-id="e21c5-181">ExpressRoute connections don't go over the public internet, and they offer more reliability, faster speeds, and lower latencies than typical internet connections.</span></span>
