<span data-ttu-id="d0ed6-101">オンプレミス環境を Azure と統合するには、暗号化された接続を作成できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-101">To integrate your on-premises environment with Azure, you need the ability to create an encrypted connection.</span></span> <span data-ttu-id="d0ed6-102">パブリック インターネット経由で、または専用リンク経由で接続できます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-102">You can connect over the public internet or over a dedicated link.</span></span> <span data-ttu-id="d0ed6-103">ここでは、オンプレミス環境からの受信接続に対するエンドポイントを提供する Azure VPN Gateway について説明します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-103">Here, we'll look at Azure VPN Gateway, which provides an endpoint for incoming connections from on-premises environments.</span></span>

<span data-ttu-id="d0ed6-104">Azure 仮想ネットワークを設定し、Azure からサイトへのデータ転送と Azure 仮想ネットワーク間のデータ転送がすべて暗号化されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-104">You have set up an Azure virtual network and need to ensure that any data transfers from Azure to your site and between Azure virtual networks are encrypted.</span></span> <span data-ttu-id="d0ed6-105">また、リージョンとサブスクリプション間で仮想ネットワークを接続する方法を知っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-105">You also need to know how to connect virtual networks between regions and subscriptions.</span></span>

## <a name="describe-a-vpn-gateway"></a><span data-ttu-id="d0ed6-106">VPN ゲートウェイの説明</span><span class="sxs-lookup"><span data-stu-id="d0ed6-106">Describe a VPN gateway</span></span>

<span data-ttu-id="d0ed6-107">Azure VPN Gateway では、オンプレミスの場所から Azure への、インターネット経由での暗号化された受信接続に対するエンドポイントが提供されます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-107">An Azure VPN gateway provides an endpoint for incoming encrypted connections from on-premises locations to Azure over the internet.</span></span> <span data-ttu-id="d0ed6-108">また、異なるリージョンの Azure データ センターをリンクしている Microsoft の専用ネットワークを経由して、Azure 仮想ネットワーク間で暗号化されたトラフィックを送信することもできます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-108">It can also send encrypted traffic between Azure virtual networks over Microsoft's dedicated network that links Azure datacenters in different regions.</span></span> <span data-ttu-id="d0ed6-109">この構成を使用すると、異なるリージョンの仮想マシンとサービスを安全にリンクできます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-109">This configuration allows you to link virtual machines and services in different regions securely.</span></span>

<span data-ttu-id="d0ed6-110">各仮想ネットワークには VPN ゲートウェイを 1 つだけ作成できます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-110">Each virtual network can have only one VPN gateway.</span></span> <span data-ttu-id="d0ed6-111">その VPN ゲートウェイへのすべての接続は、使用可能なネットワーク帯域幅を共有します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-111">All connections to that VPN gateway share the available network bandwidth.</span></span>

<span data-ttu-id="d0ed6-112">各仮想ネットワーク ゲートウェイの内部には、2 つ以上の仮想マシン (VM) があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-112">Within each virtual network gateway are two or more virtual machines (VMs).</span></span> <span data-ttu-id="d0ed6-113">これらの VM は、ユーザーが指定する "_ゲートウェイ サブネット_" という特別なサブネットにデプロイされています。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-113">These VMs have been deployed to a special subnet that you specify, called the _gateway subnet_.</span></span> <span data-ttu-id="d0ed6-114">これらには、特定のゲートウェイ サービスと共に、他のネットワークに接続するためのルーティング テーブルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-114">They contain routing tables for connections to other networks, along with specific gateway services.</span></span> <span data-ttu-id="d0ed6-115">これらの VM とゲートウェイ サブネットは、強化されたネットワーク デバイスに似ています。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-115">These VMs and the gateway subnet are similar to a hardened network device.</span></span> <span data-ttu-id="d0ed6-116">これらの VM を直接構成する必要はありませんし、ゲートウェイ サブネットに追加リソースをデプロイする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-116">You don't need to configure these VMs directly and should not deploy any additional resources into the gateway subnet.</span></span>

<span data-ttu-id="d0ed6-117">仮想ネットワーク ゲートウェイの作成は完了までに時間がかかる場合があるため、適切な計画を立てることが不可欠です。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-117">Creating a virtual network gateway can take some time to complete, so it's vital that you plan appropriately.</span></span> <span data-ttu-id="d0ed6-118">仮想ネットワーク ゲートウェイを作成するときは、プロビジョニング プロセスでゲートウェイの VM を生成し、それをゲートウェイ サブネットにデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-118">When you create a virtual network gateway, the provisioning process generates the gateway VMs and deploys them to the gateway subnet.</span></span> <span data-ttu-id="d0ed6-119">これらの VM では、ゲートウェイで構成した設定が使用されます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-119">These VMs will have the settings that you configure on the gateway.</span></span>

<span data-ttu-id="d0ed6-120">重要な設定は "**_ゲートウェイの種類_**" です。VPN ゲートウェイの場合、この種類は "vpn" となります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-120">A key setting is the **_gateway type_**, which for a VPN gateway will be of type "vpn".</span></span> <span data-ttu-id="d0ed6-121">VPN ゲートウェイのオプションには以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-121">Options for VPN gateways include:</span></span>

- <span data-ttu-id="d0ed6-122">IPsec/IKE VPN トンネリング経由のネットワーク間接続。VPN ゲートウェイを他の VPN ゲートウェイとリンクします。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-122">Network-to-network connections over IPsec/IKE VPN tunneling, linking VPN gateways to other VPN gateways.</span></span>

- <span data-ttu-id="d0ed6-123">クロスプレミス IPsec/IKE VPN トンネリング。専用の VPN デバイスを通じてオンプレミス ネットワークを Azure と接続し、サイト間接続を作成します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-123">Cross-premises IPsec/IKE VPN tunneling, for connecting on-premises networks to Azure through dedicated VPN devices to create site-to-site connections.</span></span>

- <span data-ttu-id="d0ed6-124">IKEv2 または SSTP 経由のポイント対サイト接続。クライアント コンピューターを Azure のリソースにリンクします。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-124">Point-to-site connections over IKEv2 or SSTP, to link client computers to resources in Azure.</span></span>

<span data-ttu-id="d0ed6-125">ここで、VPN ゲートウェイを計画するために考慮する必要がある要素を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-125">Now, let's look at the factors you need to consider for planning your VPN gateway.</span></span>

## <a name="plan-a-vpn-gateway"></a><span data-ttu-id="d0ed6-126">VPN ゲートウェイの計画</span><span class="sxs-lookup"><span data-stu-id="d0ed6-126">Plan a VPN gateway</span></span>

<span data-ttu-id="d0ed6-127">VPN ゲートウェイを計画するときは、3 つのアーキテクチャを考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-127">When you're planning a VPN gateway, there are three architectures to consider:</span></span>

- <span data-ttu-id="d0ed6-128">インターネット経由でのポイント対サイト</span><span class="sxs-lookup"><span data-stu-id="d0ed6-128">Point to site over the internet</span></span>
- <span data-ttu-id="d0ed6-129">インターネット経由でのサイト間</span><span class="sxs-lookup"><span data-stu-id="d0ed6-129">Site to site over the internet</span></span>
- <span data-ttu-id="d0ed6-130">専用ネットワーク経由でのサイト間 (Azure ExpressRoute など)</span><span class="sxs-lookup"><span data-stu-id="d0ed6-130">Site to site over a dedicated network, such as Azure ExpressRoute</span></span>

### <a name="planning-factors"></a><span data-ttu-id="d0ed6-131">計画における要素</span><span class="sxs-lookup"><span data-stu-id="d0ed6-131">Planning factors</span></span>

<span data-ttu-id="d0ed6-132">計画プロセスの間で検討する必要がある要素には、以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-132">Factors that you need to cover during your planning process include:</span></span>

- <span data-ttu-id="d0ed6-133">スループット - Mbps または Gbps</span><span class="sxs-lookup"><span data-stu-id="d0ed6-133">Throughput - Mbps or Gbps</span></span>
- <span data-ttu-id="d0ed6-134">バックボーン - インターネットかプライベートか</span><span class="sxs-lookup"><span data-stu-id="d0ed6-134">Backbone - internet or private?</span></span>
- <span data-ttu-id="d0ed6-135">パブリック (静的) IP アドレスの可用性</span><span class="sxs-lookup"><span data-stu-id="d0ed6-135">Availability of a public (static) IP address</span></span>
- <span data-ttu-id="d0ed6-136">VPN デバイスの互換性</span><span class="sxs-lookup"><span data-stu-id="d0ed6-136">VPN device compatibility</span></span>
- <span data-ttu-id="d0ed6-137">複数のクライアント接続か、またはサイト間のリンクか</span><span class="sxs-lookup"><span data-stu-id="d0ed6-137">Multiple client connections or a site-to-site link?</span></span>
- <span data-ttu-id="d0ed6-138">VPN ゲートウェイの種類</span><span class="sxs-lookup"><span data-stu-id="d0ed6-138">VPN gateway type</span></span>
- <span data-ttu-id="d0ed6-139">Azure VPN Gateway の SKU</span><span class="sxs-lookup"><span data-stu-id="d0ed6-139">Azure VPN Gateway SKU</span></span>

<span data-ttu-id="d0ed6-140">計画に関するこれらの問題の一部をまとめたものを、次の表に示します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-140">The following table summarizes some of these planning issues.</span></span> <span data-ttu-id="d0ed6-141">残りの部分については後ほど説明します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-141">The remainder are discussed later.</span></span>

|                           |  <span data-ttu-id="d0ed6-142">ポイント対サイト</span><span class="sxs-lookup"><span data-stu-id="d0ed6-142">Point to site</span></span>            | <span data-ttu-id="d0ed6-143">サイト間</span><span class="sxs-lookup"><span data-stu-id="d0ed6-143">Site to site</span></span>                          |  <span data-ttu-id="d0ed6-144">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="d0ed6-144">ExpressRoute</span></span>                 |
| -------------             | -------------             | -------------                         | ---------                     |
| <span data-ttu-id="d0ed6-145">Azure でサポートされるサービス</span><span class="sxs-lookup"><span data-stu-id="d0ed6-145">Azure supported services</span></span>  | <span data-ttu-id="d0ed6-146">クラウド サービスと VM</span><span class="sxs-lookup"><span data-stu-id="d0ed6-146">Cloud services and VMs</span></span>    | <span data-ttu-id="d0ed6-147">クラウド サービスと VM</span><span class="sxs-lookup"><span data-stu-id="d0ed6-147">Cloud services and VMs</span></span>                | <span data-ttu-id="d0ed6-148">サポートされているすべてのサービス</span><span class="sxs-lookup"><span data-stu-id="d0ed6-148">All supported services</span></span>        |
| <span data-ttu-id="d0ed6-149">一般的な帯域幅</span><span class="sxs-lookup"><span data-stu-id="d0ed6-149">Typical bandwidth</span></span>         | <span data-ttu-id="d0ed6-150">VPN Gateway の SKU によって異なります</span><span class="sxs-lookup"><span data-stu-id="d0ed6-150">Depends on VPN Gateway SKU</span></span>    | <span data-ttu-id="d0ed6-151">集計を含む最大 1 Gbps</span><span class="sxs-lookup"><span data-stu-id="d0ed6-151">Up to 1 Gbps with aggregation</span></span>         | <span data-ttu-id="d0ed6-152">50 Mbps から 10 Gbps</span><span class="sxs-lookup"><span data-stu-id="d0ed6-152">From 50 Mbps to 10 Gbps</span></span>       |
| <span data-ttu-id="d0ed6-153">サポート対象プロトコル</span><span class="sxs-lookup"><span data-stu-id="d0ed6-153">Protocols supported</span></span>       | <span data-ttu-id="d0ed6-154">SSTP および IPSec</span><span class="sxs-lookup"><span data-stu-id="d0ed6-154">SSTP and IPsec</span></span>            | <span data-ttu-id="d0ed6-155">IPsec</span><span class="sxs-lookup"><span data-stu-id="d0ed6-155">IPsec</span></span>                                 | <span data-ttu-id="d0ed6-156">直接接続、VLAN</span><span class="sxs-lookup"><span data-stu-id="d0ed6-156">Direct connection, VLANs</span></span>      |
| <span data-ttu-id="d0ed6-157">ルーティング</span><span class="sxs-lookup"><span data-stu-id="d0ed6-157">Routing</span></span>                   | <span data-ttu-id="d0ed6-158">RouteBased (動的)</span><span class="sxs-lookup"><span data-stu-id="d0ed6-158">RouteBased (dynamic)</span></span>      | <span data-ttu-id="d0ed6-159">PolicyBased (静的) および RouteBased</span><span class="sxs-lookup"><span data-stu-id="d0ed6-159">PolicyBased (static) and RouteBased</span></span>   | <span data-ttu-id="d0ed6-160">BGP</span><span class="sxs-lookup"><span data-stu-id="d0ed6-160">BGP</span></span>                           |
| <span data-ttu-id="d0ed6-161">接続の回復性</span><span class="sxs-lookup"><span data-stu-id="d0ed6-161">Connection resiliency</span></span>     | <span data-ttu-id="d0ed6-162">アクティブ/パッシブ</span><span class="sxs-lookup"><span data-stu-id="d0ed6-162">Active-passive</span></span>            | <span data-ttu-id="d0ed6-163">アクティブ/パッシブまたはアクティブ/アクティブ</span><span class="sxs-lookup"><span data-stu-id="d0ed6-163">Active-passive or active-active</span></span>       | <span data-ttu-id="d0ed6-164">アクティブ/アクティブ</span><span class="sxs-lookup"><span data-stu-id="d0ed6-164">Active-active</span></span>                 |
| <span data-ttu-id="d0ed6-165">ユース ケース</span><span class="sxs-lookup"><span data-stu-id="d0ed6-165">Use case</span></span>                  | <span data-ttu-id="d0ed6-166">テストとプロトタイプ作成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-166">Testing and prototyping</span></span>   | <span data-ttu-id="d0ed6-167">開発、テスト、小規模の運用</span><span class="sxs-lookup"><span data-stu-id="d0ed6-167">Dev, test and small-scale production</span></span>  | <span data-ttu-id="d0ed6-168">エンタープライズ/ミッション クリティカル</span><span class="sxs-lookup"><span data-stu-id="d0ed6-168">Enterprise/mission critical</span></span>   |

### <a name="gateway-skus"></a><span data-ttu-id="d0ed6-169">Gateway の SKU</span><span class="sxs-lookup"><span data-stu-id="d0ed6-169">Gateway SKUs</span></span>

<span data-ttu-id="d0ed6-170">Azure には、ゲートウェイ サービス用に次の SKU が用意されています。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-170">Azure offers the following SKUs for gateway services:</span></span>

| <span data-ttu-id="d0ed6-171">SKU</span><span class="sxs-lookup"><span data-stu-id="d0ed6-171">SKU</span></span>              |  <span data-ttu-id="d0ed6-172">S2S/ネットワーク間のトンネル</span><span class="sxs-lookup"><span data-stu-id="d0ed6-172">S2S/network-to-network tunnels</span></span> | <span data-ttu-id="d0ed6-173">P2S 接続</span><span class="sxs-lookup"><span data-stu-id="d0ed6-173">P2S connections</span></span>  |  <span data-ttu-id="d0ed6-174">合計スループット ベンチマーク</span><span class="sxs-lookup"><span data-stu-id="d0ed6-174">Aggregate throughput benchmark</span></span>   | <span data-ttu-id="d0ed6-175">用途</span><span class="sxs-lookup"><span data-stu-id="d0ed6-175">Use for</span></span>                         |
| -------------    | -------------             | -------------    | ---------                         | ---------                       |
| <span data-ttu-id="d0ed6-176">Basic</span><span class="sxs-lookup"><span data-stu-id="d0ed6-176">Basic</span></span>            | <span data-ttu-id="d0ed6-177">最大 10</span><span class="sxs-lookup"><span data-stu-id="d0ed6-177">Max 10</span></span>                    | <span data-ttu-id="d0ed6-178">最大 128</span><span class="sxs-lookup"><span data-stu-id="d0ed6-178">Max 128</span></span>          | <span data-ttu-id="d0ed6-179">100 Mbps</span><span class="sxs-lookup"><span data-stu-id="d0ed6-179">100 Mbps</span></span>                          | <span data-ttu-id="d0ed6-180">開発/テスト/POC</span><span class="sxs-lookup"><span data-stu-id="d0ed6-180">Dev/test/POC</span></span>                    |
| <span data-ttu-id="d0ed6-181">VpnGw1</span><span class="sxs-lookup"><span data-stu-id="d0ed6-181">VpnGw1</span></span>           | <span data-ttu-id="d0ed6-182">最大 30</span><span class="sxs-lookup"><span data-stu-id="d0ed6-182">Max 30</span></span>                    | <span data-ttu-id="d0ed6-183">最大 128</span><span class="sxs-lookup"><span data-stu-id="d0ed6-183">Max 128</span></span>          | <span data-ttu-id="d0ed6-184">650 Mbps</span><span class="sxs-lookup"><span data-stu-id="d0ed6-184">650 Mbps</span></span>                          | <span data-ttu-id="d0ed6-185">運用/重要なワークロード</span><span class="sxs-lookup"><span data-stu-id="d0ed6-185">Production/critical workloads</span></span>   |
| <span data-ttu-id="d0ed6-186">VpnGw2</span><span class="sxs-lookup"><span data-stu-id="d0ed6-186">VpnGw2</span></span>           | <span data-ttu-id="d0ed6-187">最大 30</span><span class="sxs-lookup"><span data-stu-id="d0ed6-187">Max 30</span></span>                    | <span data-ttu-id="d0ed6-188">最大 128</span><span class="sxs-lookup"><span data-stu-id="d0ed6-188">Max 128</span></span>          | <span data-ttu-id="d0ed6-189">1 Gbps</span><span class="sxs-lookup"><span data-stu-id="d0ed6-189">1 Gbps</span></span>                            | <span data-ttu-id="d0ed6-190">運用/重要なワークロード</span><span class="sxs-lookup"><span data-stu-id="d0ed6-190">Production/critical workloads</span></span>   |
| <span data-ttu-id="d0ed6-191">VpnGw3</span><span class="sxs-lookup"><span data-stu-id="d0ed6-191">VpnGw3</span></span>           | <span data-ttu-id="d0ed6-192">最大 30</span><span class="sxs-lookup"><span data-stu-id="d0ed6-192">Max 30</span></span>                    | <span data-ttu-id="d0ed6-193">最大 128</span><span class="sxs-lookup"><span data-stu-id="d0ed6-193">Max 128</span></span>          | <span data-ttu-id="d0ed6-194">1.25 Gbps</span><span class="sxs-lookup"><span data-stu-id="d0ed6-194">1.25 Gbps</span></span>                          | <span data-ttu-id="d0ed6-195">運用/重要なワークロード</span><span class="sxs-lookup"><span data-stu-id="d0ed6-195">Production/critical workloads</span></span>   |

> [!Note]
> <span data-ttu-id="d0ed6-196">正しい SKU を選択することが重要です。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-196">It's important that you choose the right SKU.</span></span> <span data-ttu-id="d0ed6-197">誤った SKU で VPN ゲートウェイを設定した場合、ゲートウェイを停止させて再構築する必要が発生します。これには時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-197">If you have set up your VPN gateway with the wrong one, you'll have to take it down and rebuild the gateway, which can be time consuming.</span></span>

## <a name="workflow"></a><span data-ttu-id="d0ed6-198">ワークフロー</span><span class="sxs-lookup"><span data-stu-id="d0ed6-198">Workflow</span></span>

<span data-ttu-id="d0ed6-199">Azure での仮想プライベート ネットワークを使用するクラウド接続の計画を立てるときは、次のワークフローを適用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-199">When designing a cloud connectivity strategy using virtual private networking on Azure, you should apply the following workflow:</span></span>

1. <span data-ttu-id="d0ed6-200">接続するすべてのネットワークのアドレス空間をリストする、接続トポロジを設計します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-200">Design your connectivity topology, listing the address spaces for all connecting networks.</span></span>

1. <span data-ttu-id="d0ed6-201">Azure 仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-201">Create an Azure virtual network.</span></span>

1. <span data-ttu-id="d0ed6-202">仮想ネットワークの VPN ゲートウェイを作成します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-202">Create a VPN gateway for the virtual network.</span></span>

1. <span data-ttu-id="d0ed6-203">必要に応じて、オンプレミス ネットワークやその他の仮想ネットワークへの接続を作成および構成します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-203">Create and configure connections to on-premises networks or other virtual networks, as required.</span></span>

1. <span data-ttu-id="d0ed6-204">必要に応じて、Azure VPN ゲートウェイのポイント対サイト接続を作成および構成します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-204">If required, create and configure a point-to-site connection for your Azure VPN gateway.</span></span>

### <a name="design-considerations"></a><span data-ttu-id="d0ed6-205">設計上の考慮事項</span><span class="sxs-lookup"><span data-stu-id="d0ed6-205">Design considerations</span></span>

<span data-ttu-id="d0ed6-206">仮想ネットワークに接続する VPN ゲートウェイを設計するときは、次の要素を考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-206">When you design your VPN gateways to connect virtual networks, you must consider the following factors:</span></span>

- <span data-ttu-id="d0ed6-207">サブネットを重複させることはできません</span><span class="sxs-lookup"><span data-stu-id="d0ed6-207">Subnets cannot overlap</span></span>

    <span data-ttu-id="d0ed6-208">1 つの場所にあるサブネットが、別の場所にあるものと同じアドレス空間を含まないことが不可欠です。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-208">It is vital that a subnet in one location does not contain the same address space as in another location.</span></span>

- <span data-ttu-id="d0ed6-209">IP アドレスは一意である必要があります</span><span class="sxs-lookup"><span data-stu-id="d0ed6-209">IP addresses must be unique</span></span>

    <span data-ttu-id="d0ed6-210">異なる場所に同じ IP アドレスを持つホストを 2 つ持つことはできません。この 2 つのホスト間でトラフィックをルーティングすることができなくなり、ネットワーク間接続が失敗します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-210">You cannot have two hosts with the same IP address in different locations, as it will be impossible to route traffic between those two hosts and the network-to-network connection will fail.</span></span>

- <span data-ttu-id="d0ed6-211">VPN ゲートウェイには、**GatewaySubnet** という名前のゲートウェイ サブネットが必要です。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-211">VPN gateways need a gateway subnet called **GatewaySubnet**</span></span>

    <span data-ttu-id="d0ed6-212">ゲートウェイが機能するためには必ずこの名前にする必要があり、その他のリソースを含めることはできません。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-212">It must have this name for the gateway to work, and it should not contain any other resources.</span></span>

### <a name="create-an-azure-virtual-network"></a><span data-ttu-id="d0ed6-213">Azure の仮想ネットワークを作成する</span><span class="sxs-lookup"><span data-stu-id="d0ed6-213">Create an Azure virtual network</span></span>

<span data-ttu-id="d0ed6-214">VPN ゲートウェイを作成する前に、Azure ネットワークを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-214">Before you create a VPN gateway, you need to create the Azure virtual network.</span></span>

### <a name="create-a-vpn-gateway"></a><span data-ttu-id="d0ed6-215">VPN ゲートウェイの作成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-215">Create a VPN gateway</span></span>

<span data-ttu-id="d0ed6-216">作成する VPN ゲートウェイの種類は、アーキテクチャによって異なります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-216">The type of VPN gateway you create will depend on your architecture.</span></span> <span data-ttu-id="d0ed6-217">オプションは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-217">Options are:</span></span>

- <span data-ttu-id="d0ed6-218">RouteBased</span><span class="sxs-lookup"><span data-stu-id="d0ed6-218">RouteBased</span></span>

    <span data-ttu-id="d0ed6-219">ルート ベース VPN デバイスは、任意の環境間 (ワイルドカード) のトラフィック セレクターを使用して、ルーティング/転送テーブルを異なる IPsec トンネルへのトラフィックに転送します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-219">Route-based VPN devices use any-to-any (wildcard) traffic selectors, and let routing/forwarding tables direct traffic to different IPsec tunnels.</span></span> <span data-ttu-id="d0ed6-220">ルート ベースの接続は、通常、それぞれの IPsec トンネルがネットワーク インターフェイスまたは VTI (仮想トンネル インターフェイス) としてモデル化されるルーターのプラットフォームに基づいて構築されます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-220">Route-based connections are typically built on router platforms where each IPsec tunnel is modeled as a network interface or VTI (virtual tunnel interface).</span></span>

- <span data-ttu-id="d0ed6-221">PolicyBased</span><span class="sxs-lookup"><span data-stu-id="d0ed6-221">PolicyBased</span></span>

    <span data-ttu-id="d0ed6-222">ポリシー ベース VPN デバイスは、両方のネットワークからプレフィクスの組み合わせを使用して、トラフィックがどのように IPsec トンネルを介して暗号化/復号化されるかを定義します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-222">Policy-based VPN devices use the combinations of prefixes from both networks to define how traffic is encrypted/decrypted through IPsec tunnels.</span></span> <span data-ttu-id="d0ed6-223">ポリシー ベースの接続は、通常、パケット フィルタリングを実行するファイアウォール デバイスに基づいて構築されます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-223">A policy-based connection is typically built on firewall devices that perform packet filtering.</span></span> <span data-ttu-id="d0ed6-224">IPsec トンネルの暗号化と復号化は、パケット フィルタリングと処理エンジンに追加されます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-224">IPsec tunnel encryption and decryption are added to the packet filtering and processing engine.</span></span>

## <a name="set-up-a-vpn-gateway"></a><span data-ttu-id="d0ed6-225">VPN ゲートウェイの設定</span><span class="sxs-lookup"><span data-stu-id="d0ed6-225">Set up a VPN gateway</span></span>

<span data-ttu-id="d0ed6-226">実行する必要がある手順は、インストールする VPN ゲートウェイの種類によって異なります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-226">The steps you need to take will depend on the type of VPN gateway that you are installing.</span></span> <span data-ttu-id="d0ed6-227">たとえば、Azure portal を使用してポイント対サイト VPN ゲートウェイを作成するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-227">For example, to create a point-to-site VPN gateway by using the Azure portal, you would carry out the following steps:</span></span>

1. <span data-ttu-id="d0ed6-228">仮想ネットワークの作成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-228">Create a virtual network</span></span>

2. <span data-ttu-id="d0ed6-229">ゲートウェイ サブネットの追加</span><span class="sxs-lookup"><span data-stu-id="d0ed6-229">Add a gateway subnet</span></span>

3. <span data-ttu-id="d0ed6-230">DNS サーバーの指定 (省略可能)</span><span class="sxs-lookup"><span data-stu-id="d0ed6-230">Specify a DNS server (optional)</span></span>

4. <span data-ttu-id="d0ed6-231">仮想ネットワーク ゲートウェイの作成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-231">Create a virtual network gateway</span></span>

5. <span data-ttu-id="d0ed6-232">証明書の生成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-232">Generate certificates</span></span>

6. <span data-ttu-id="d0ed6-233">クライアント アドレス プールの追加</span><span class="sxs-lookup"><span data-stu-id="d0ed6-233">Add the client address pool</span></span>

7. <span data-ttu-id="d0ed6-234">トンネルの種類の構成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-234">Configure the tunnel type</span></span>

8. <span data-ttu-id="d0ed6-235">認証の種類の構成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-235">Configure the authentication type</span></span>

9. <span data-ttu-id="d0ed6-236">ルート証明書の公開証明書データのアップロード</span><span class="sxs-lookup"><span data-stu-id="d0ed6-236">Upload the root certificate public certificate data</span></span>

10. <span data-ttu-id="d0ed6-237">エクスポートしたクライアント証明書のインストール</span><span class="sxs-lookup"><span data-stu-id="d0ed6-237">Install an exported client certificate</span></span>

11. <span data-ttu-id="d0ed6-238">VPN クライアント構成パッケージの生成とインストール</span><span class="sxs-lookup"><span data-stu-id="d0ed6-238">Generate and install the VPN client configuration package</span></span>

12. <span data-ttu-id="d0ed6-239">Azure への接続</span><span class="sxs-lookup"><span data-stu-id="d0ed6-239">Connect to Azure</span></span>

<span data-ttu-id="d0ed6-240">Azure VPN ゲートウェイを構成するためにはいくつかの方法があり、そのそれぞれが複数のオプションを含んでいるため、すべての設定をこのコースで網羅することはできません。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-240">As there are several configuration paths with Azure VPN gateways, each with multiple options, it is not possible to cover every setup in this course.</span></span> <span data-ttu-id="d0ed6-241">詳細については、その他のリソースに関するセクションをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-241">For more information, see the Additional Resources section.</span></span>

## <a name="configure-the-gateway"></a><span data-ttu-id="d0ed6-242">ゲートウェイの構成</span><span class="sxs-lookup"><span data-stu-id="d0ed6-242">Configure the gateway</span></span>

<span data-ttu-id="d0ed6-243">ゲートウェイを作成したら、それを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-243">Once your gateway is created, you'll need to configure it.</span></span>  <span data-ttu-id="d0ed6-244">いくつかの構成設定を指定する必要があります (名前、場所、DNS サーバーなど)。演習では、これらについて詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-244">There are several configuration settings you will need to provide, such as the name, location, DNS server, etc. We will go into these in more detail in the exercise.</span></span>

## <a name="summary"></a><span data-ttu-id="d0ed6-245">まとめ</span><span class="sxs-lookup"><span data-stu-id="d0ed6-245">Summary</span></span>

<span data-ttu-id="d0ed6-246">Azure VPN ゲートウェイとは、ポイント対サイト、サイト間、またはネットワーク間の接続を可能にする、Azure ネットワーク内のコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-246">Azure VPN gateways are a component in Azure virtual networks that enable point-to-site, site-to-site, or network-to-network connections.</span></span> <span data-ttu-id="d0ed6-247">Azure VPN ゲートウェイを使用すると、個々のクライアント コンピューターを Azure のリソースに接続したり、オンプレミスのネットワークを Azure に拡張したり、異なるリージョンやサブスクリプション内のネットワーク間での接続を容易にしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="d0ed6-247">Azure VPN gateways enable individual client computers to connect to resources in Azure, extend on-premises networks into Azure, or facilitate connections between virtual networks in different regions and subscriptions.</span></span>
