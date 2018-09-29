<span data-ttu-id="0feb3-101">お客様によって管理されているデータセンターからクラウド データセンターへのコンピューティング環境の移行時に、セキュリティの責任も移行されます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-101">As computing environments move from customer-controlled data centers to cloud data centers, the responsibility of security also shifts.</span></span> <span data-ttu-id="0feb3-102">現在、セキュリティはクラウド プロバイダーとお客様の両方に共通の懸念事項です。</span><span class="sxs-lookup"><span data-stu-id="0feb3-102">Security is now a concern shared both by cloud providers and customers.</span></span> <span data-ttu-id="0feb3-103">すべてのアプリケーションとソリューションにおいて、お客様が負う責任と Azure 側が負う責任を理解しておくことが重要です。</span><span class="sxs-lookup"><span data-stu-id="0feb3-103">For every application and solution, it's important to understand what's your responsibility and what's Azure's responsibility.</span></span>

#### <a name="understand-security-threats"></a><span data-ttu-id="0feb3-104">セキュリティの脅威について</span><span class="sxs-lookup"><span data-stu-id="0feb3-104">Understand security threats</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWkotg]

#### <a name="azure-security-you-versus-the-cloud"></a><span data-ttu-id="0feb3-105">Azure のセキュリティ: お客様とクラウド</span><span class="sxs-lookup"><span data-stu-id="0feb3-105">Azure security: you versus the cloud</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvj]

## <a name="share-security-responsibility-with-azure"></a><span data-ttu-id="0feb3-106">セキュリティの責任を Azure と共有する</span><span class="sxs-lookup"><span data-stu-id="0feb3-106">Share security responsibility with Azure</span></span>

<span data-ttu-id="0feb3-107">最初に行う移行は、オンプレミスのデータ センターからサービスとしてのインフラストラクチャ (IaaS) への移行です。</span><span class="sxs-lookup"><span data-stu-id="0feb3-107">The first shift you’ll make is from on-premises data centers to infrastructure as a service (IaaS).</span></span> <span data-ttu-id="0feb3-108">IaaS の場合は、最下位レベルのサービスを利用します。そして Azure で仮想マシン (VM) と仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-108">With IaaS, you are leveraging the lowest-level service and asking Azure to create virtual machines (VMs) and virtual networks.</span></span> <span data-ttu-id="0feb3-109">このレベルでは、オペレーティング システムおよびソフトウェアに対してパッチを適用すること、これらをセキュリティで保護すること、さらにセキュリティで保護されるようにネットワークを構成することはお客様側の責任です。</span><span class="sxs-lookup"><span data-stu-id="0feb3-109">At this level, it's still your responsibility to patch and secure your operating systems and software, as well as configure your network to be secure.</span></span> <span data-ttu-id="0feb3-110">Contoso Shipping では、自社のオンプレミスの物理サーバーに代えて Azure VM の使用を開始するときから、IaaS を活用しています。</span><span class="sxs-lookup"><span data-stu-id="0feb3-110">At Contoso Shipping, you are taking advantage of IaaS when you start using Azure VMs instead of your on-premises physical servers.</span></span> <span data-ttu-id="0feb3-111">運用上の利点に加えて、ネットワークの物理的な部分の保護に対する懸念を外部委託できるというセキュリティ上の利点があります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-111">In addition to the operational advantages, you receive the security advantage of having outsourced concern over protecting the physical parts of the network.</span></span>

<span data-ttu-id="0feb3-112">サービスとしてのプラットフォーム (PaaS) への移行によって、セキュリティ上の懸念事項の多くが外部に委託されます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-112">Moving to platform as a service (PaaS) outsources a lot of security concerns.</span></span> <span data-ttu-id="0feb3-113">このレベルでは、オペレーティング システムおよび最も基本的なソフトウェア (データベース管理システムなど) に対応します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-113">At this level, Azure is taking care of the operating system and of most foundational software like database management systems.</span></span> <span data-ttu-id="0feb3-114">すべてが最新のセキュリティ パッチで更新されます。また、すべてを Azure Active Directory に統合してアクセス制御を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-114">Everything is updated with the latest security patches and can be integrated with Azure Active Directory for access controls.</span></span> <span data-ttu-id="0feb3-115">PaaS には、数多くの運用上の利点があります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-115">PaaS also comes with a lot of operational advantages.</span></span> <span data-ttu-id="0feb3-116">ご利用の環境に合わせてインフラストラクチャおよびサブネット全体を手動で構築するのではなく、Azure portal 内で "ポイントしてクリック" するか、または自動化されたスクリプトを実行することで、セキュリティで保護された複雑なシステムをアップまたはダウン状態にしたり、必要に応じてそれらのシステムをスケーリングしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-116">Rather than building whole infrastructures and subnets for your environments by hand, you can "point and click" within the Azure portal or run automated scripts to bring complex, secured systems up and down, and scale them as needed.</span></span> <span data-ttu-id="0feb3-117">Contoso Shipping では、ドローンおよびトラック &mdash; からの製品利用統計情報データを取り込むために Azure Event Hubs を使用します。また、モバイル アプリ &mdash; で Azure Cosmos DB バック エンドを使用した Web アプリも使用します。これらはすべて PaaS の例です。</span><span class="sxs-lookup"><span data-stu-id="0feb3-117">Contoso Shipping uses Azure Event Hubs for ingesting telemetry data from drones and trucks &mdash; as well as a web app with an Azure Cosmos DB back end with their mobile apps &mdash; which are all examples of PaaS.</span></span>

<span data-ttu-id="0feb3-118">サービスとしてのソフトウェア (SaaS) の場合は、ほぼすべてを外部委託できます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-118">With software as a service (SaaS), you outsource almost everything.</span></span> <span data-ttu-id="0feb3-119">SaaS は、インターネット インフラストラクチャで実行されるソフトウェアです。</span><span class="sxs-lookup"><span data-stu-id="0feb3-119">SaaS is software that runs with an internet infrastructure.</span></span> <span data-ttu-id="0feb3-120">そのコードはベンダーによって管理されますが、お客様が使用できるように構成されます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-120">The code is controlled by the vendor but configured to be used by the customer.</span></span> <span data-ttu-id="0feb3-121">多くの企業と同様に Contoso Shipping でも、SaaS の良い例である Office 365 を使用しています。</span><span class="sxs-lookup"><span data-stu-id="0feb3-121">Like so many companies, Contoso Shipping uses Office 365, which is a great example of SaaS!</span></span>

![shared_responsibility.png](../media/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a><span data-ttu-id="0feb3-123">セキュリティへの階層型アプローチ</span><span class="sxs-lookup"><span data-stu-id="0feb3-123">A layered approach to security</span></span>

<span data-ttu-id="0feb3-124">"*多層防御*" は、情報への不正なアクセスを目的とする攻撃の進行を遅らせる一連のメカニズムを採用する戦略です。</span><span class="sxs-lookup"><span data-stu-id="0feb3-124">*Defense in depth* is a strategy that employs a series of mechanisms to slow the advance of an attack aimed at acquiring unauthorized access to information.</span></span> <span data-ttu-id="0feb3-125">各層で保護が提供されるため、1 つの層が破られた場合、後続の層は既に備えができており、さらなる露出を防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-125">Each layer provides protection so that if one layer is breached, a subsequent layer is already in place to prevent further exposure.</span></span> <span data-ttu-id="0feb3-126">Microsoft では、自社の物理データセンター内と Azure サービス間の両方でのセキュリティに階層型アプローチを適用しています。</span><span class="sxs-lookup"><span data-stu-id="0feb3-126">Microsoft applies a layered approach to security, both in physical data centers and across Azure services.</span></span> <span data-ttu-id="0feb3-127">多層防御の目的は、情報を保護し、情報へのアクセスが許可されていない個人から盗まれないようにすることです。</span><span class="sxs-lookup"><span data-stu-id="0feb3-127">The objective of defense in depth is to protect and prevent information from being stolen by individuals who are not authorized to access it.</span></span>

<span data-ttu-id="0feb3-128">多層防御は、同心円状の輪のセットとして視覚化でき、データは中央でセキュリティ保護されます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-128">Defense in depth can be visualized as a set of concentric rings, with the data to be secured at the center.</span></span> <span data-ttu-id="0feb3-129">各輪では、データに関するセキュリティ層が追加されます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-129">Each ring adds an additional layer of security around the data.</span></span> <span data-ttu-id="0feb3-130">このアプローチでは、単一の保護層に依存する必要がなくなり、攻撃速度を落とすことができます。また、自動または手動によるアラート テレメトリが提供されます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-130">This approach removes reliance on any single layer of protection and acts to slow down an attack and provide alert telemetry that can be acted upon, either automatically or manually.</span></span> <span data-ttu-id="0feb3-131">それでは各層を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="0feb3-131">Let's take a look at each of the layers.</span></span>

![多層防御](../media/defense_in_depth_layers_small.PNG)

:::row:::
  :::column:::
    <span data-ttu-id="0feb3-133">![データを表す画像](../media/2-data.png)</span><span class="sxs-lookup"><span data-stu-id="0feb3-133">![Image representing data](../media/2-data.png)</span></span>
  :::column-end:::
    <span data-ttu-id="0feb3-134">:::column span="3"::: **データ**</span><span class="sxs-lookup"><span data-stu-id="0feb3-134">:::column span="3"::: **Data**</span></span>

<span data-ttu-id="0feb3-135">ほとんどすべての場合、攻撃者は次のようなデータを狙っています。</span><span class="sxs-lookup"><span data-stu-id="0feb3-135">In almost all cases, attackers are after data:</span></span>

- <span data-ttu-id="0feb3-136">データベースに格納されたデータ</span><span class="sxs-lookup"><span data-stu-id="0feb3-136">Data stored in a database</span></span>
- <span data-ttu-id="0feb3-137">仮想マシン内のディスク上に格納されたデータ</span><span class="sxs-lookup"><span data-stu-id="0feb3-137">Data stored on disk inside virtual machines</span></span>
- <span data-ttu-id="0feb3-138">Office 365 などの SaaS アプリケーションに格納されたデータ</span><span class="sxs-lookup"><span data-stu-id="0feb3-138">Data stored on a SaaS application such as Office 365</span></span>
- <span data-ttu-id="0feb3-139">クラウド ストレージに格納されたデータ</span><span class="sxs-lookup"><span data-stu-id="0feb3-139">Data stored in cloud storage</span></span>

<span data-ttu-id="0feb3-140">データが確実に適切なセキュリティで保護されるように格納し、データへのアクセスを制御する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-140">It's the responsibility of those storing and controlling access to data to ensure that it's properly secured.</span></span> <span data-ttu-id="0feb3-141">多くの場合、データの機密性、整合性、可用性を確保するために適用する必要がある制御とプロセスを示す規制上の要件があります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-141">Often, there are regulatory requirements that dictate the controls and processes that must be in place to ensure the confidentiality, integrity, and availability of the data.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="0feb3-142">![ネットワーク上のファイルの画像](../media/2-application.png)</span><span class="sxs-lookup"><span data-stu-id="0feb3-142">![Image of a file on the network](../media/2-application.png)</span></span>
  :::column-end:::
    <span data-ttu-id="0feb3-143">:::column span="3"::: **アプリケーション**</span><span class="sxs-lookup"><span data-stu-id="0feb3-143">:::column span="3"::: **Application**</span></span>

- <span data-ttu-id="0feb3-144">アプリケーションが確実にセキュリティで保護された脆弱性のないものにします。</span><span class="sxs-lookup"><span data-stu-id="0feb3-144">Ensure applications are secure and free of vulnerabilities.</span></span>
- <span data-ttu-id="0feb3-145">機密性の高いアプリケーション シークレットをセキュリティで保護されたストレージ メディアに格納します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-145">Store sensitive application secrets in a secure storage medium.</span></span>
- <span data-ttu-id="0feb3-146">すべてのアプリケーション開発に関する設計要件を安全なものにします。</span><span class="sxs-lookup"><span data-stu-id="0feb3-146">Make security a design requirement for all application development.</span></span>

<span data-ttu-id="0feb3-147">アプリケーション開発のライフ サイクルにセキュリティを統合することは、コードにおける脆弱性の数を減らすのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-147">Integrating security into the application development life cycle will help reduce the number of vulnerabilities introduced in code.</span></span> <span data-ttu-id="0feb3-148">すべての開発チームが確実に既定でアプリケーションをセキュリティで保護するようにし、セキュリティ要件を交渉の余地のないものにします。</span><span class="sxs-lookup"><span data-stu-id="0feb3-148">We encourage all development teams to ensure their applications are secure by default, and that they're making security requirements non-negotiable.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="0feb3-149">![コンピューティングを表すターミナル](../media/2-compute.png)</span><span class="sxs-lookup"><span data-stu-id="0feb3-149">![A terminal representing compute](../media/2-compute.png)</span></span>
  :::column-end:::
    <span data-ttu-id="0feb3-150">:::column span="3"::: **コンピューティング**</span><span class="sxs-lookup"><span data-stu-id="0feb3-150">:::column span="3"::: **Compute**</span></span>

- <span data-ttu-id="0feb3-151">仮想マシンへのアクセスをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-151">Secure access to virtual machines.</span></span>
- <span data-ttu-id="0feb3-152">エンドポイント保護を実装し、システムにパッチを適用して最新の状態に保ちます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-152">Implement endpoint protection and keep systems patched and current.</span></span>

<span data-ttu-id="0feb3-153">マルウェア、パッチが適用されていないシステム、適切にセキュリティ保護されていないシステムのために、環境が攻撃を受けやすくなってしまいます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-153">Malware, unpatched systems, and improperly secured systems open your environment to attacks.</span></span> <span data-ttu-id="0feb3-154">この層では、コンピューティング リソースをセキュリティで保護すること、セキュリティ問題を最小限に抑えるべく、適切な管理体制を敷くことに重点的に取り組みます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-154">The focus in this layer is on making sure your compute resources are secure, and that you have the proper controls in place to minimize security issues.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="0feb3-155">![ネットワークを表す 3 つの接続されたシステム](../media/2-networking.png)</span><span class="sxs-lookup"><span data-stu-id="0feb3-155">![Three connected systems representing networking](../media/2-networking.png)</span></span>
  :::column-end:::
    <span data-ttu-id="0feb3-156">:::column span="3"::: **ネットワーク**</span><span class="sxs-lookup"><span data-stu-id="0feb3-156">:::column span="3"::: **Networking**</span></span>

- <span data-ttu-id="0feb3-157">リソース間の通信を制限します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-157">Limit communication between resources.</span></span>
- <span data-ttu-id="0feb3-158">既定で拒否します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-158">Deny by default.</span></span>
- <span data-ttu-id="0feb3-159">必要に応じて、インバウンド インターネット アクセスを限定し、アウトバウンドを制限します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-159">Restrict inbound internet access and limit outbound, where appropriate.</span></span>
- <span data-ttu-id="0feb3-160">オンプレミス ネットワークへのセキュリティで保護された接続を実装します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-160">Implement secure connectivity to on-premises networks.</span></span>

<span data-ttu-id="0feb3-161">この層では、リソース全体でのネットワーク接続を制限し、必要なもののみを許可することに重点を置きます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-161">At this layer, the focus is on limiting the network connectivity across all your resources to allow only what is required.</span></span> <span data-ttu-id="0feb3-162">この通信を制限することで、ネットワーク全体の侵入拡大のリスクを軽減することができます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-162">By limiting this communication, you reduce the risk of lateral movement throughout your network.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="0feb3-163">![ネットワーク境界を表す物理バリア](../media/2-perimeter.png)</span><span class="sxs-lookup"><span data-stu-id="0feb3-163">![A physical barrier representing the network perimeter](../media/2-perimeter.png)</span></span>
  :::column-end:::
    <span data-ttu-id="0feb3-164">:::column span="3"::: **境界**</span><span class="sxs-lookup"><span data-stu-id="0feb3-164">:::column span="3"::: **Perimeter**</span></span>

- <span data-ttu-id="0feb3-165">分散型サービス拒否 (DDoS) 保護を使用して、エンド ユーザーに対するサービス拒否が発生する前に大規模な攻撃をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-165">Use distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for end users.</span></span>
- <span data-ttu-id="0feb3-166">境界ファイアウォールを使用して、ネットワークに対する悪意のある攻撃を識別し、警告します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-166">Use perimeter firewalls to identify and alert on malicious attacks against your network.</span></span>

<span data-ttu-id="0feb3-167">ネットワーク境界で、リソースに対するネットワークベースの攻撃から保護する目的があります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-167">At the network perimeter, it's about protecting from network-based attacks against your resources.</span></span> <span data-ttu-id="0feb3-168">これらの攻撃を識別し、影響を排除し、これらの発生時に警告することは、ネットワークを安全に保つために重要なこと方法です。</span><span class="sxs-lookup"><span data-stu-id="0feb3-168">Identifying these attacks, eliminating their impact, and alerting you when they happen are important ways to keep your network secure.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="0feb3-169">![セキュリティで保護されたアクセスを表すバッジ](../media/2-policies-and-access.png)</span><span class="sxs-lookup"><span data-stu-id="0feb3-169">![A badge representing a secure access](../media/2-policies-and-access.png)</span></span>
  :::column-end:::
    <span data-ttu-id="0feb3-170">:::column span="3"::: **ポリシーとアクセス**</span><span class="sxs-lookup"><span data-stu-id="0feb3-170">:::column span="3"::: **Policies and access**</span></span>

- <span data-ttu-id="0feb3-171">インフラストラクチャへのアクセスを制御し、変更を制御します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-171">Control access to infrastructure and change control.</span></span>
- <span data-ttu-id="0feb3-172">シングル サインオンと多要素認証を使用します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-172">Use single sign-on and multi-factor authentication.</span></span>
- <span data-ttu-id="0feb3-173">イベントと変更を監査します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-173">Audit events and changes.</span></span>

<span data-ttu-id="0feb3-174">ポリシーとアクセス層では、ID がセキュリティで保護されていることと、付与されたアクセス権のみが必要であることと、変更がログに記録されていることを確認することに集中します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-174">The policy and access layer is all about ensuring identities are secure, access granted is only what is needed, and changes are logged.</span></span>
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    <span data-ttu-id="0feb3-175">![物理的なセキュリティを表すセキュリティ カメラ](../media/2-physical-security.png)</span><span class="sxs-lookup"><span data-stu-id="0feb3-175">![A security camera representing physical security](../media/2-physical-security.png)</span></span>
  :::column-end:::
    <span data-ttu-id="0feb3-176">:::column span="3"::: **物理的なセキュリティ**</span><span class="sxs-lookup"><span data-stu-id="0feb3-176">:::column span="3"::: **Physical security**</span></span>

- <span data-ttu-id="0feb3-177">物理的なセキュリティを構築し、データ センター内のコンピューティング ハードウェアへのアクセスを制御することが、防御の最前線となります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-177">Physical building security and controlling access to computing hardware within the data center is the first line of defense.</span></span>

<span data-ttu-id="0feb3-178">物理的なセキュリティの目的は、資産へのアクセスに対する物理的な保護措置を講じることです。</span><span class="sxs-lookup"><span data-stu-id="0feb3-178">With physical security, the intent is to provide physical safeguards against access to assets.</span></span> <span data-ttu-id="0feb3-179">これにより、他の層をバイパスできなくなり、損失や盗難の適切な処理が確保されます。</span><span class="sxs-lookup"><span data-stu-id="0feb3-179">This ensures that other layers can't be bypassed, and loss or theft is handled appropriately.</span></span>
  :::column-end:::
:::row-end:::

## <a name="summary"></a><span data-ttu-id="0feb3-180">まとめ</span><span class="sxs-lookup"><span data-stu-id="0feb3-180">Summary</span></span>

<span data-ttu-id="0feb3-181">ここでは、セキュリティに関するお客様の懸念事項の解消に Azure が大いに役立っていることを見てきました。</span><span class="sxs-lookup"><span data-stu-id="0feb3-181">We've seen here that Azure helps a lot with your security concerns.</span></span> <span data-ttu-id="0feb3-182">しかし、セキュリティは引き続き、**共有する責任**となります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-182">But security is still a **shared responsibility**.</span></span> <span data-ttu-id="0feb3-183">背負う必要がある責任の程度は、Azure で使用するモデルの種類によって異なります。</span><span class="sxs-lookup"><span data-stu-id="0feb3-183">How much of that responsibility falls on us depends on which model we use with Azure.</span></span>

<span data-ttu-id="0feb3-184">データや環境に適切な保護を検討するためのガイドラインとして、*多層防御*リングを使用します。</span><span class="sxs-lookup"><span data-stu-id="0feb3-184">We use the *defense in depth* rings as a guideline for considering what protections are adequate for our data and environments.</span></span>