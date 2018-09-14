<span data-ttu-id="4ae75-101">Microsoft Azure では、現在および将来のビジネスの課題に対応する組織に役立つクラウド サービスのセットが継続的に拡張されています。</span><span class="sxs-lookup"><span data-stu-id="4ae75-101">Microsoft Azure is a continually expanding set of cloud services that help your organization meet your current and future business challenges.</span></span> <span data-ttu-id="4ae75-102">Azure では、好みのツールとフレームワークを使用して、大規模なグローバル ネットワーク上でアプリケーションを自由にビルド、管理、デプロイできます。</span><span class="sxs-lookup"><span data-stu-id="4ae75-102">Azure gives you the freedom to build, manage, and deploy applications on a massive global network using your favorite tools and frameworks.</span></span> <span data-ttu-id="4ae75-103">Azure で提供されている高レベルのサービスをざっと見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="4ae75-103">Let's take a quick tour of the high-level services Azure offers.</span></span>

## <a name="azure-services"></a><span data-ttu-id="4ae75-104">Azure サービス</span><span class="sxs-lookup"><span data-stu-id="4ae75-104">Azure services</span></span>

<span data-ttu-id="4ae75-105">Azure では非常に広範なクラウドベースのサービスが提供されており、機能の追加と強化が毎月行われています。</span><span class="sxs-lookup"><span data-stu-id="4ae75-105">Azure provides a vast range of cloud-based services, with features added and enhanced every month.</span></span> 

![Azure サービス](../media-draft/2-image204.png)

<span data-ttu-id="4ae75-107">よく使われる機能のいくつかを詳しく見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="4ae75-107">Let's take a closer look at a few of the more commonly-used features:</span></span> 

- [<span data-ttu-id="4ae75-108">Compute</span><span class="sxs-lookup"><span data-stu-id="4ae75-108">Compute</span></span>](#compute-services)
- [<span data-ttu-id="4ae75-109">ネットワーク</span><span class="sxs-lookup"><span data-stu-id="4ae75-109">Networking </span></span>](#networking-services)
- [<span data-ttu-id="4ae75-110">Storage</span><span class="sxs-lookup"><span data-stu-id="4ae75-110">Storage</span></span>](#storage-services)
- [<span data-ttu-id="4ae75-111">Mobile</span><span class="sxs-lookup"><span data-stu-id="4ae75-111">Mobile</span></span>](#mobil-services)
- [<span data-ttu-id="4ae75-112">データベース</span><span class="sxs-lookup"><span data-stu-id="4ae75-112">Databases</span></span>](#database-services)
- [<span data-ttu-id="4ae75-113">Web</span><span class="sxs-lookup"><span data-stu-id="4ae75-113">Web</span></span>](#web-services)

<a name="compute-services"></a>

### <a name="compute"></a><span data-ttu-id="4ae75-114">コンピューティング</span><span class="sxs-lookup"><span data-stu-id="4ae75-114">Compute</span></span>

<span data-ttu-id="4ae75-115">コンピューティング サービスは、企業が Azure Platform に移行する主な理由の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="4ae75-115">Compute services are one of the primary reasons why companies move to the Azure platform.</span></span> <span data-ttu-id="4ae75-116">Azure では、アプリケーションとサービスをホストするためのさまざまなオプションが提供されています。次はその一部です。</span><span class="sxs-lookup"><span data-stu-id="4ae75-116">Azure provides a range of options for hosting applications and services including:</span></span>

![IaaS、PaaS、FaaS の比較](../media/2-iaas-paas-faas.png)

<span data-ttu-id="4ae75-118">Azure での IaaS、PaaS、FaaS の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="4ae75-118">Here are some examples of IaaS, PaaS, and FaaS in Azure.</span></span>

|  <span data-ttu-id="4ae75-119">種類</span><span class="sxs-lookup"><span data-stu-id="4ae75-119">Type</span></span>  |  <span data-ttu-id="4ae75-120">サービス名</span><span class="sxs-lookup"><span data-stu-id="4ae75-120">Service name</span></span>             | <span data-ttu-id="4ae75-121">サービスの機能</span><span class="sxs-lookup"><span data-stu-id="4ae75-121">Service function</span></span>                                                         |
|--------|---------------------------|--------------------------------------------------------------------------|
| <span data-ttu-id="4ae75-122">IaaS</span><span class="sxs-lookup"><span data-stu-id="4ae75-122">IaaS</span></span>   | <span data-ttu-id="4ae75-123">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="4ae75-123">Azure Virtual Machines</span></span>    | <span data-ttu-id="4ae75-124">Azure でホストされている Windows または Linux の VM です</span><span class="sxs-lookup"><span data-stu-id="4ae75-124">Windows or Linux VMs hosted in Azure</span></span>                                     | 
| <span data-ttu-id="4ae75-125">IaaS</span><span class="sxs-lookup"><span data-stu-id="4ae75-125">IaaS</span></span>   | <span data-ttu-id="4ae75-126">Azure Kubernetes Service</span><span class="sxs-lookup"><span data-stu-id="4ae75-126">Azure Kubernetes Service</span></span>  | <span data-ttu-id="4ae75-127">コンテナー化されたサービスを実行する VM のクラスターを管理できます</span><span class="sxs-lookup"><span data-stu-id="4ae75-127">Enables management of a cluster of VMs that run containerized services</span></span>   |
| <span data-ttu-id="4ae75-128">PaaS</span><span class="sxs-lookup"><span data-stu-id="4ae75-128">PaaS</span></span>   | <span data-ttu-id="4ae75-129">Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="4ae75-129">Azure Service Fabric</span></span>      | <span data-ttu-id="4ae75-130">分散システム プラットフォームです。</span><span class="sxs-lookup"><span data-stu-id="4ae75-130">Distributed systems platform.</span></span> <span data-ttu-id="4ae75-131">Azure またはオンプレミスで実行されます。</span><span class="sxs-lookup"><span data-stu-id="4ae75-131">Runs in Azure or on-premises</span></span>               |
| <span data-ttu-id="4ae75-132">PaaS</span><span class="sxs-lookup"><span data-stu-id="4ae75-132">PaaS</span></span>   | <span data-ttu-id="4ae75-133">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="4ae75-133">Azure Batch</span></span>               | <span data-ttu-id="4ae75-134">並列および高パフォーマンスのコンピューティング アプリケーション用のマネージド サービスです</span><span class="sxs-lookup"><span data-stu-id="4ae75-134">Managed service for parallel and high-performance computing applications</span></span> |
| <span data-ttu-id="4ae75-135">PaaS</span><span class="sxs-lookup"><span data-stu-id="4ae75-135">PaaS</span></span>   | <span data-ttu-id="4ae75-136">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="4ae75-136">Azure Cloud Services</span></span>      | <span data-ttu-id="4ae75-137">クラウド アプリケーションを実行するためのマネージド サービスです</span><span class="sxs-lookup"><span data-stu-id="4ae75-137">Managed service for running cloud applications</span></span>                           |
| <span data-ttu-id="4ae75-138">FaaS</span><span class="sxs-lookup"><span data-stu-id="4ae75-138">FaaS</span></span>   | <span data-ttu-id="4ae75-139">Azure Container Instances</span><span class="sxs-lookup"><span data-stu-id="4ae75-139">Azure Container Instances</span></span> | <span data-ttu-id="4ae75-140">VM のプロビジョニングやより高いサービスを必要としないコンテナーを提供します</span><span class="sxs-lookup"><span data-stu-id="4ae75-140">Provides containers without requiring VM provision or higher services</span></span>    |
| <span data-ttu-id="4ae75-141">FaaS</span><span class="sxs-lookup"><span data-stu-id="4ae75-141">FaaS</span></span>   | <span data-ttu-id="4ae75-142">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="4ae75-142">Azure Functions</span></span>           | <span data-ttu-id="4ae75-143">管理された FaaS サービスです</span><span class="sxs-lookup"><span data-stu-id="4ae75-143">Managed FaaS service</span></span>                                                     |

<a name="network-services"></a>

### <a name="networking"></a><span data-ttu-id="4ae75-144">ネットワーク</span><span class="sxs-lookup"><span data-stu-id="4ae75-144">Networking</span></span>

<span data-ttu-id="4ae75-145">コンピューティング リソースをリンクし、アプリケーションへのアクセスを提供することが、Azure ネットワークの主要な機能です。</span><span class="sxs-lookup"><span data-stu-id="4ae75-145">Linking compute resources and providing access to applications is the key function of Azure networking.</span></span> <span data-ttu-id="4ae75-146">Azure のネットワーク機能には、外部の世界をグローバルな Microsoft Azure データ センター内のサービスと機能に接続するためのさまざまなオプションが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4ae75-146">Networking functionality in Azure includes a range of options to connect the outside world to services and features in the global Microsoft Azure datacenters.</span></span>

<span data-ttu-id="4ae75-147">Azure ネットワーク機能には次のようなものがあります</span><span class="sxs-lookup"><span data-stu-id="4ae75-147">Azure networking facilities have the following features:</span></span>

|  <span data-ttu-id="4ae75-148">サービス名</span><span class="sxs-lookup"><span data-stu-id="4ae75-148">Service name</span></span>             | <span data-ttu-id="4ae75-149">サービスの機能</span><span class="sxs-lookup"><span data-stu-id="4ae75-149">Service function</span></span>                                                                     |
| -------------             | -------------                                                                        |
| <span data-ttu-id="4ae75-150">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="4ae75-150">Azure Virtual Network</span></span>     | <span data-ttu-id="4ae75-151">受信仮想プライベート ネットワーク (VPN) 接続に VM を接続します</span><span class="sxs-lookup"><span data-stu-id="4ae75-151">Connects VMs to incoming Virtual Private Network (VPN) connections</span></span>                   |
| <span data-ttu-id="4ae75-152">Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="4ae75-152">Azure Load Balancer</span></span>       | <span data-ttu-id="4ae75-153">受信および送信接続を複数のアプリケーションまたはサービス エンドポイントに分散させます</span><span class="sxs-lookup"><span data-stu-id="4ae75-153">Balances inbound and outbound connections to applications or service endpoints</span></span>       |
| <span data-ttu-id="4ae75-154">Azure Application Gateway</span><span class="sxs-lookup"><span data-stu-id="4ae75-154">Azure Application Gateway</span></span> | <span data-ttu-id="4ae75-155">アプリケーションのセキュリティを高めながら、アプリ サーバー ファームの配信を最適化します</span><span class="sxs-lookup"><span data-stu-id="4ae75-155">Optimizes app server farm delivery while increasing application security</span></span>             |
| <span data-ttu-id="4ae75-156">Azure VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="4ae75-156">Azure VPN Gateway</span></span>         | <span data-ttu-id="4ae75-157">高パフォーマンスの VPN ゲートウェイを介して Azure Virtual Network にアクセスします</span><span class="sxs-lookup"><span data-stu-id="4ae75-157">Accesses Azure Virtual Networks through high-performance VPN gateways</span></span>                |
| <span data-ttu-id="4ae75-158">Azure DNS</span><span class="sxs-lookup"><span data-stu-id="4ae75-158">Azure DNS</span></span>                 | <span data-ttu-id="4ae75-159">超高速の DNS 応答と非常に高いドメインの可用性を提供します</span><span class="sxs-lookup"><span data-stu-id="4ae75-159">Provides ultra-fast DNS responses and ultra-high domain availability</span></span>                 |
| <span data-ttu-id="4ae75-160">Azure Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="4ae75-160">Azure Content Delivery Network</span></span>  | <span data-ttu-id="4ae75-161">顧客に対して高帯域幅コンテンツをグローバルに提供します</span><span class="sxs-lookup"><span data-stu-id="4ae75-161">Delivers high-bandwidth content to customers globally</span></span>                          |
| <span data-ttu-id="4ae75-162">Azure DDoS Protection</span><span class="sxs-lookup"><span data-stu-id="4ae75-162">Azure DDoS Protection</span></span>     | <span data-ttu-id="4ae75-163">Azure でホストされたアプリケーションを分散型サービス拒否 (DDoS) 攻撃から保護します</span><span class="sxs-lookup"><span data-stu-id="4ae75-163">Protects Azure-hosted applications from distributed denial of service (DDOS) attacks</span></span> |
| <span data-ttu-id="4ae75-164">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="4ae75-164">Azure Traffic Manager</span></span>     | <span data-ttu-id="4ae75-165">世界中の Azure リージョンにネットワーク トラフィックを分散させます</span><span class="sxs-lookup"><span data-stu-id="4ae75-165">Distributes network traffic across Azure regions worldwide</span></span>                           |
| <span data-ttu-id="4ae75-166">Azure ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="4ae75-166">Azure ExpressRoute</span></span>        | <span data-ttu-id="4ae75-167">セキュリティで保護された高帯域幅の専用接続を介して Azure に接続します</span><span class="sxs-lookup"><span data-stu-id="4ae75-167">Connects to Azure over high-bandwidth dedicated secure connections</span></span>                   |
| <span data-ttu-id="4ae75-168">Azure Network Watcher</span><span class="sxs-lookup"><span data-stu-id="4ae75-168">Azure Network Watcher</span></span>     | <span data-ttu-id="4ae75-169">シナリオ ベースの分析を使用してネットワークの問題を監視および診断します</span><span class="sxs-lookup"><span data-stu-id="4ae75-169">Monitors and diagnoses network issues using scenario-based analysis</span></span>                  |
| <span data-ttu-id="4ae75-170">Azure Firewall</span><span class="sxs-lookup"><span data-stu-id="4ae75-170">Azure Firewall</span></span>            | <span data-ttu-id="4ae75-171">無制限の拡張性を備えたセキュリティと可用性が高いファイアウォールを実装します</span><span class="sxs-lookup"><span data-stu-id="4ae75-171">Implements high-security, high-availability firewall with unlimited scalability</span></span>      |
| <span data-ttu-id="4ae75-172">Azure Virtual WAN</span><span class="sxs-lookup"><span data-stu-id="4ae75-172">Azure Virtual WAN</span></span>         | <span data-ttu-id="4ae75-173">ローカルとリモートのサイトを接続する統合されたワイド エリア ネットワーク (WAN) を作成します</span><span class="sxs-lookup"><span data-stu-id="4ae75-173">Creates a unified wide area network (WAN), connecting local and remote sites</span></span>         |

<a name="storage-services"></a>

### <a name="storage"></a><span data-ttu-id="4ae75-174">ストレージ</span><span class="sxs-lookup"><span data-stu-id="4ae75-174">Storage</span></span>

<span data-ttu-id="4ae75-175">Azure では、主に 4 種類のストレージ サービスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="4ae75-175">Azure provides four main types of storage services.</span></span> <span data-ttu-id="4ae75-176">これらのサービスを次に示します。</span><span class="sxs-lookup"><span data-stu-id="4ae75-176">These services are:</span></span>

- <span data-ttu-id="4ae75-177">**Azure Blob Storage** - ビデオ ファイルやビットマップなどの非常に大きなオブジェクト用のストレージを提供します</span><span class="sxs-lookup"><span data-stu-id="4ae75-177">**Azure Blob storage** - provides storage for very large objects, such as video files or bitmaps</span></span>
- <span data-ttu-id="4ae75-178">**Azure File Storage** - ファイル サーバーのようにアクセスおよび管理できるファイル共有を作成します</span><span class="sxs-lookup"><span data-stu-id="4ae75-178">**Azure File storage** - creates file shares that you can access and manage like a file server</span></span>
- <span data-ttu-id="4ae75-179">**Azure Queue Storage** - アプリケーション間のメッセージのキューイングおよび確実な配信のためのストアを実装します</span><span class="sxs-lookup"><span data-stu-id="4ae75-179">**Azure Queue storage** - implements a store for queuing and reliably delivering messages between applications</span></span>
- <span data-ttu-id="4ae75-180">**Azure Table Storage** - どのようなスキーマからも独立した非構造化データをホストする NoSQL ストアで構成されます</span><span class="sxs-lookup"><span data-stu-id="4ae75-180">**Azure Table storage** - consists of a NoSQL store that hosts unstructured data independent of any schema</span></span>

<span data-ttu-id="4ae75-181">これらの各サービスは、次のような共通の特性を備えています。</span><span class="sxs-lookup"><span data-stu-id="4ae75-181">Each of these services shares common characteristics, which are:</span></span>

- <span data-ttu-id="4ae75-182">冗長性とレプリケーションによる永続性と高可用性。</span><span class="sxs-lookup"><span data-stu-id="4ae75-182">Durable and highly available with redundancy and replication.</span></span>
- <span data-ttu-id="4ae75-183">自動的な暗号化とロールベースのアクセス制御による安全性。</span><span class="sxs-lookup"><span data-stu-id="4ae75-183">Secure through automatic encryption and role-based access control.</span></span>
- <span data-ttu-id="4ae75-184">実質的に無制限のストレージによる拡張性。</span><span class="sxs-lookup"><span data-stu-id="4ae75-184">Scalable with virtually unlimited storage.</span></span>
- <span data-ttu-id="4ae75-185">メンテナンスや重大な問題の管理された自動的な処理。</span><span class="sxs-lookup"><span data-stu-id="4ae75-185">Managed, handling maintenance and any critical problems for you.</span></span>
- <span data-ttu-id="4ae75-186">世界中のどこからでも HTTP または HTTPS 経由でアクセス可能。</span><span class="sxs-lookup"><span data-stu-id="4ae75-186">Accessible from anywhere in the world over HTTP or HTTPS.</span></span>

<a name="mobile-services"></a>

### <a name="mobile"></a><span data-ttu-id="4ae75-187">モバイル</span><span class="sxs-lookup"><span data-stu-id="4ae75-187">Mobile</span></span>

<span data-ttu-id="4ae75-188">Azure では、開発者は自分が選択した開発環境を使用して、幅広い言語で iOS、Android、Windows の魅力的なアプリを短時間で簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="4ae75-188">Azure enables developers to create engaging iOS, Android, and Windows apps quickly and easily in a wide range of languages using their choice of development environment.</span></span> <span data-ttu-id="4ae75-189">企業のサインインを追加してから、SAP、Oracle、SQL Server、SharePoint などのオンプレミス リソースに接続するといった、これまでであれば時間がかかってプロジェクトのリスクが高くなるような機能を、シンプルに組み込めるようになりました。</span><span class="sxs-lookup"><span data-stu-id="4ae75-189">Features that used to take time and increase project risks, such as adding corporate sign-in and then connecting to on-premises resources such as SAP, Oracle, SQL Server, and SharePoint, are now simple to include.</span></span>

<span data-ttu-id="4ae75-190">このサービスには他に次のような機能があります。</span><span class="sxs-lookup"><span data-stu-id="4ae75-190">Other features of this service include:</span></span>

- <span data-ttu-id="4ae75-191">オフライン データ同期。</span><span class="sxs-lookup"><span data-stu-id="4ae75-191">Offline data synchronization.</span></span>
- <span data-ttu-id="4ae75-192">オンプレミス データへの接続。</span><span class="sxs-lookup"><span data-stu-id="4ae75-192">Connectivity to on-premises data.</span></span>
- <span data-ttu-id="4ae75-193">プッシュ通知のブロードキャスト。</span><span class="sxs-lookup"><span data-stu-id="4ae75-193">Broadcasting push notifications.</span></span>
- <span data-ttu-id="4ae75-194">ビジネス ニーズに合わせた自動スケーリング。</span><span class="sxs-lookup"><span data-stu-id="4ae75-194">Autoscaling to match business needs.</span></span>

<a name="database-services"></a>

### <a name="databases"></a><span data-ttu-id="4ae75-195">データベース</span><span class="sxs-lookup"><span data-stu-id="4ae75-195">Databases</span></span>

<span data-ttu-id="4ae75-196">Azure では、さまざまなデータの種類とボリュームを格納するために複数のデータベース サービスが提供されています。</span><span class="sxs-lookup"><span data-stu-id="4ae75-196">Azure provides multiple database services to store a wide variety of data types and volumes.</span></span> <span data-ttu-id="4ae75-197">また、グローバル接続により、ユーザーはこのデータをすぐに使用できます。</span><span class="sxs-lookup"><span data-stu-id="4ae75-197">And with global connectivity, this data is available to users instantly.</span></span>

|  <span data-ttu-id="4ae75-198">サービス名</span><span class="sxs-lookup"><span data-stu-id="4ae75-198">Service name</span></span>              | <span data-ttu-id="4ae75-199">サービスの機能</span><span class="sxs-lookup"><span data-stu-id="4ae75-199">Service function</span></span>                                                                                |
| -------------              | -------------                                                                                   |
| <span data-ttu-id="4ae75-200">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="4ae75-200">Azure Cosmos DB</span></span>            | <span data-ttu-id="4ae75-201">NoSQL オプションをサポートするグローバル分散データベース</span><span class="sxs-lookup"><span data-stu-id="4ae75-201">Globally distributed database that supports NoSQL options</span></span>                                       |
| <span data-ttu-id="4ae75-202">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="4ae75-202">Azure SQL Database</span></span>         | <span data-ttu-id="4ae75-203">自動スケーリング、統合インテリジェンス、堅牢なセキュリティを備えた、フル マネージドのリレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="4ae75-203">Fully managed relational database with auto-scale, integral intelligence, and robust security</span></span>    |
| <span data-ttu-id="4ae75-204">Azure Database for MySQL</span><span class="sxs-lookup"><span data-stu-id="4ae75-204">Azure Database for MySQL</span></span>   | <span data-ttu-id="4ae75-205">高可用性とセキュリティを備えた完全に管理されたスケーラブルな MySQL リレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="4ae75-205">Fully managed and scalable MySQL relational database with high availability and security</span></span>        |
| <span data-ttu-id="4ae75-206">Azure Database for PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4ae75-206">Azure Database for PostgreSQL</span></span>   | <span data-ttu-id="4ae75-207">高可用性とセキュリティを備えた完全に管理されたスケーラブルな PostgreSQL リレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="4ae75-207">Fully managed and scalable PostgreSQL relational database with high availability and security</span></span>   |
| <span data-ttu-id="4ae75-208">VM 上の SQL Server</span><span class="sxs-lookup"><span data-stu-id="4ae75-208">SQL Server on VMs</span></span>          | <span data-ttu-id="4ae75-209">エンタープライズ SQL Server アプリをクラウドでホストする</span><span class="sxs-lookup"><span data-stu-id="4ae75-209">Host enterprise SQL Server apps in the cloud</span></span>                                                    |
| <span data-ttu-id="4ae75-210">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="4ae75-210">Azure SQL Data Warehouse</span></span>   | <span data-ttu-id="4ae75-211">すべてのスケール レベルで無料の統合セキュリティを備えた完全に管理されたデータ ウェアハウス</span><span class="sxs-lookup"><span data-stu-id="4ae75-211">Fully managed data warehouse with integral security at every level of scale at no extra cost</span></span>    |
| <span data-ttu-id="4ae75-212">Azure Database Migration Service</span><span class="sxs-lookup"><span data-stu-id="4ae75-212">Azure Database Migration Service</span></span>    | <span data-ttu-id="4ae75-213">アプリケーション コードの変更なしでクラウドにデータベースを移行します</span><span class="sxs-lookup"><span data-stu-id="4ae75-213">Migrates your databases to the cloud with no application code changes</span></span>                  |
| <span data-ttu-id="4ae75-214">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="4ae75-214">Azure Redis Cache</span></span>          | <span data-ttu-id="4ae75-215">頻繁に使用される静的データをキャッシュして、データとアプリケーションの待機時間を短縮します</span><span class="sxs-lookup"><span data-stu-id="4ae75-215">Caches frequently used and static data to reduce data and application latency</span></span>                   |
| <span data-ttu-id="4ae75-216">Azure Database for MariaDB</span><span class="sxs-lookup"><span data-stu-id="4ae75-216">Azure Database for MariaDB</span></span> | <span data-ttu-id="4ae75-217">高可用性とセキュリティを備えた完全に管理されたスケーラブルな MySQL リレーショナル データベース</span><span class="sxs-lookup"><span data-stu-id="4ae75-217">Fully managed and scalable MySQL relational database with high availability and security</span></span>        |

<a name="web-services"></a>

### <a name="web"></a><span data-ttu-id="4ae75-218">Web</span><span class="sxs-lookup"><span data-stu-id="4ae75-218">Web</span></span>

<span data-ttu-id="4ae75-219">Azure の Web サービスには、次の機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="4ae75-219">Web services in Azure include the following facilities:</span></span>

| <span data-ttu-id="4ae75-220">サービス名</span><span class="sxs-lookup"><span data-stu-id="4ae75-220">Service Name</span></span> | <span data-ttu-id="4ae75-221">説明</span><span class="sxs-lookup"><span data-stu-id="4ae75-221">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="4ae75-222">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="4ae75-222">Azure App Service</span></span> | <span data-ttu-id="4ae75-223">Web およびモバイル向けのパワフルなクラウド アプリを短期間で作成します。</span><span class="sxs-lookup"><span data-stu-id="4ae75-223">Quickly create powerful cloud apps for web and mobile.</span></span> |
| <span data-ttu-id="4ae75-224">Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="4ae75-224">Azure Notification Hubs</span></span> |<span data-ttu-id="4ae75-225">どのバック エンドからでもあらゆるプラットフォームへプッシュ通知を送信します。</span><span class="sxs-lookup"><span data-stu-id="4ae75-225">Send push notifications to any platform from any back end.</span></span> |
| <span data-ttu-id="4ae75-226">Azure API Management</span><span class="sxs-lookup"><span data-stu-id="4ae75-226">Azure API Management</span></span> | <span data-ttu-id="4ae75-227">API を開発者、パートナー、従業員に安全かつ大規模に発行します。</span><span class="sxs-lookup"><span data-stu-id="4ae75-227">Publish APIs to developers, partners, and employees securely and at scale.</span></span> |
| <span data-ttu-id="4ae75-228">Azure Search</span><span class="sxs-lookup"><span data-stu-id="4ae75-228">Azure Search</span></span> | <span data-ttu-id="4ae75-229">フル マネージドの、サービスとしての検索です。</span><span class="sxs-lookup"><span data-stu-id="4ae75-229">Fully managed search as a service.</span></span> |
| <span data-ttu-id="4ae75-230">Azure App Service の Web Apps の機能</span><span class="sxs-lookup"><span data-stu-id="4ae75-230">Web Apps feature of Azure App Service</span></span> | <span data-ttu-id="4ae75-231">大規模な基幹業務系 Web アプリを作成してデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4ae75-231">Create and deploy mission-critical web apps at scale.</span></span> |
| <span data-ttu-id="4ae75-232">Azure SignalR Service</span><span class="sxs-lookup"><span data-stu-id="4ae75-232">Azure SignalR Service</span></span> | <span data-ttu-id="4ae75-233">リアルタイム Web 機能を簡単に追加します。</span><span class="sxs-lookup"><span data-stu-id="4ae75-233">Add real-time web functionalities easily.</span></span> |

<span data-ttu-id="4ae75-234">これで、Azure への移行を検討している企業が関心を持つ領域をいくつか特定できました。では、そのサービスと機能を利用するために必要なことを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="4ae75-234">Now that we've identified some of the areas that might interest a company looking to migrate to Azure let's look at what it takes to use the services and features.</span></span>