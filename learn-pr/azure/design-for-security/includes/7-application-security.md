<span data-ttu-id="b5be5-101">クラウド プラットフォーム上でアプリケーションをホストすることで、従来のオンプレミス デプロイと比べて、多くの利点が得られます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-101">Hosting applications on a cloud platform provides a number of advantages when compared to traditional on-premises deployments.</span></span> <span data-ttu-id="b5be5-102">クラウドの共同責任モデルでは、クラウド プロバイダーの管理下にある物理ネットワーク、ビルドおよびホスト レベルでセキュリティを移行します。</span><span class="sxs-lookup"><span data-stu-id="b5be5-102">The cloud's shared-responsibility model moves security at the physical network, building and host levels under the control of the cloud provider.</span></span> <span data-ttu-id="b5be5-103">このレベルでプラットフォームを侵害しようとする攻撃者は、収穫逓減と、プロバイダーによるインフラストラクチャの保護と監視に関する多額の投資と分析情報を比較します。</span><span class="sxs-lookup"><span data-stu-id="b5be5-103">An attacker trying to compromise the platform at this level would see diminishing returns versus the considerable investment and insight providers make in securing and monitoring their infrastructure.</span></span>

<span data-ttu-id="b5be5-104">したがって、クラウド プラットフォームのお客様が発生させたアプリケーション レベルでの脆弱性を追求する攻撃者にとってははるかに効率的です。</span><span class="sxs-lookup"><span data-stu-id="b5be5-104">It's therefore far more effective for attackers to pursue vulnerabilities introduced at the application level by cloud-platform customers.</span></span> <span data-ttu-id="b5be5-105">さらに、アプリケーションをホストするサービスとしてのプラットフォーム (PaaS) を導入することで、お客様はオペレーティング システム セキュリティの管理からリソースを解放し、リソースをデプロイしてアプリケーション コードを強化し、アプリケーションの周囲の ID 境界を監視できます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-105">Furthermore, by adopting Platform as a Service (PaaS) to host their applications, customers are able to free resources from managing operating system security and deploy them to harden application code and monitor the identity perimeter around the application.</span></span> <span data-ttu-id="b5be5-106">このユニットでは、設計によりアプリケーション セキュリティを強化できる方法をいくつか説明します。</span><span class="sxs-lookup"><span data-stu-id="b5be5-106">In this unit we will discuss some of the ways application security can be improved through design.</span></span>

## <a name="scenario"></a><span data-ttu-id="b5be5-107">シナリオ</span><span class="sxs-lookup"><span data-stu-id="b5be5-107">Scenario</span></span>

<span data-ttu-id="b5be5-108">Lamna Healthcare のお客様は、オンライン Web ポータルから個人の医療記録にアクセスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-108">Lamna Healthcare customers require access to their personal medical records through an online web portal.</span></span> <span data-ttu-id="b5be5-109">Health Insurance Portability and Accountability Act (HIPAA) の準拠は必須です。個人データの漏えいが発生した場合、会社に罰金が課せられる危険性が高いため、アプリケーションと、アプリケーションでやりとりされる個人データをセキュリティで保護することが重要です。</span><span class="sxs-lookup"><span data-stu-id="b5be5-109">Compliance with the Health Insurance Portability and Accountability Act (HIPAA) is mandatory and puts the company at significant risk of financial penalties if a breach of personal data occurs, therefore securing the application and personal data it interacts with is paramount.</span></span>

<span data-ttu-id="b5be5-110">お客様のアプリケーションに関連する主な領域は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b5be5-110">The primary areas that concern customer applications are:</span></span>

- <span data-ttu-id="b5be5-111">セキュリティで保護されたアプリケーションの設計</span><span class="sxs-lookup"><span data-stu-id="b5be5-111">Secure application design</span></span>
- <span data-ttu-id="b5be5-112">データのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="b5be5-112">Data security</span></span>
- <span data-ttu-id="b5be5-113">ID およびアクセス管理</span><span class="sxs-lookup"><span data-stu-id="b5be5-113">Identity & access management</span></span>
- <span data-ttu-id="b5be5-114">エンドポイントのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="b5be5-114">Endpoint security</span></span>

## <a name="security-development-lifecycle"></a><span data-ttu-id="b5be5-115">セキュリティ開発ライフサイクル</span><span class="sxs-lookup"><span data-stu-id="b5be5-115">Security Development Lifecycle</span></span>

<span data-ttu-id="b5be5-116">Microsoft の[セキュリティ開発ライフサイクル](https://www.microsoft.com/en-us/sdl) (SDL) プロセスをアプリケーションの設計段階で使用することで、確実にソフトウェア開発ライフサイクルにセキュリティに関する懸念事項を組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-116">Microsoft's [Security Development Lifecycle](https://www.microsoft.com/en-us/sdl) (SDL) process can be used during the application design stage to ensure security concerns are incorporated in the software development lifecycle.</span></span> <span data-ttu-id="b5be5-117">セキュリティとコンプライアンスの問題は、アプリケーションの設計時に対処するほうがはるかに簡単であり、最終製品のセキュリティの欠陥につながる可能性のある多くの一般的なエラーを減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-117">Security and compliance issues are far easier to address when designing an application and can mitigate many common errors that can lead to security flaws in the final product.</span></span> <span data-ttu-id="b5be5-118">ソフトウェア開発の早い段階で問題を解決することで、コストははるかに低くなります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-118">Fixing issues early in the software development journey is also far less costly.</span></span> <span data-ttu-id="b5be5-119">ソフトウェア プロジェクトで使用できる SDL の一般的な一連の手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b5be5-119">The typical sequence of SDL steps a software project can use are as follows:</span></span>

1. <span data-ttu-id="b5be5-120">トレーニング</span><span class="sxs-lookup"><span data-stu-id="b5be5-120">Training</span></span>

    - <span data-ttu-id="b5be5-121">コア セキュリティのトレーニング</span><span class="sxs-lookup"><span data-stu-id="b5be5-121">Core security training</span></span>

1. <span data-ttu-id="b5be5-122">要件</span><span class="sxs-lookup"><span data-stu-id="b5be5-122">Requirements</span></span>

    - <span data-ttu-id="b5be5-123">要件と品質ゲートを定義する</span><span class="sxs-lookup"><span data-stu-id="b5be5-123">Define requirements and quality gates</span></span>
    - <span data-ttu-id="b5be5-124">セキュリティおよびプライバシー リスクを分析する</span><span class="sxs-lookup"><span data-stu-id="b5be5-124">Analyze security & privacy risks</span></span>
 
1. <span data-ttu-id="b5be5-125">設計</span><span class="sxs-lookup"><span data-stu-id="b5be5-125">Design</span></span>

    - <span data-ttu-id="b5be5-126">攻撃対象領域の分析</span><span class="sxs-lookup"><span data-stu-id="b5be5-126">Attack surface analysis</span></span>
    - <span data-ttu-id="b5be5-127">脅威モデリング</span><span class="sxs-lookup"><span data-stu-id="b5be5-127">Threat modelling</span></span>
 
1. <span data-ttu-id="b5be5-128">実装</span><span class="sxs-lookup"><span data-stu-id="b5be5-128">Implementation</span></span>

    - <span data-ttu-id="b5be5-129">コードの品質を測定できることを確認するためのツールを指定する</span><span class="sxs-lookup"><span data-stu-id="b5be5-129">Specify tools to ensure code quality can be measured</span></span>
    - <span data-ttu-id="b5be5-130">禁止されている API と関数を適用する</span><span class="sxs-lookup"><span data-stu-id="b5be5-130">Enforce banned APIs & functions</span></span>
    - <span data-ttu-id="b5be5-131">スタティック コード分析を実行する</span><span class="sxs-lookup"><span data-stu-id="b5be5-131">Perform Static code analysis</span></span>
    - <span data-ttu-id="b5be5-132">リポジトリをスキャンして格納されたシークレットを調べる</span><span class="sxs-lookup"><span data-stu-id="b5be5-132">Scan repositories for stored secrets</span></span>
 
1. <span data-ttu-id="b5be5-133">検証</span><span class="sxs-lookup"><span data-stu-id="b5be5-133">Verification</span></span>

    - <span data-ttu-id="b5be5-134">動的/ファジー テスト</span><span class="sxs-lookup"><span data-stu-id="b5be5-134">Dynamic/Fuzz testing</span></span>
    - <span data-ttu-id="b5be5-135">脅威モデル/攻撃対象領域を検証する</span><span class="sxs-lookup"><span data-stu-id="b5be5-135">Verify threat models/attack surface</span></span>
 
1. <span data-ttu-id="b5be5-136">解放</span><span class="sxs-lookup"><span data-stu-id="b5be5-136">Release</span></span>

    - <span data-ttu-id="b5be5-137">セキュリティ対応計画を立てる</span><span class="sxs-lookup"><span data-stu-id="b5be5-137">Form security response plan</span></span>
    - <span data-ttu-id="b5be5-138">最終的なセキュリティ レビューを行う</span><span class="sxs-lookup"><span data-stu-id="b5be5-138">Perform a final security review</span></span>
    - <span data-ttu-id="b5be5-139">アーカイブの解放</span><span class="sxs-lookup"><span data-stu-id="b5be5-139">Release archive</span></span>
 
1. <span data-ttu-id="b5be5-140">対応</span><span class="sxs-lookup"><span data-stu-id="b5be5-140">Response</span></span> 

    - <span data-ttu-id="b5be5-141">脅威に対応する</span><span class="sxs-lookup"><span data-stu-id="b5be5-141">Execute threat response execution</span></span>

![セキュリティ開発ライフサイクル](../media/sdl.png)

<span data-ttu-id="b5be5-143">SDL は、ツールのセットやプロセスであると同時に文化的側面でもあります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-143">The SDL is as much a cultural aspect as it is a process or set of tools.</span></span> <span data-ttu-id="b5be5-144">セキュリティがアプリケーション開発の主な焦点および要件であるという文化を築くことで、セキュリティに関する組織の能力を高めることに大きく前進できます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-144">Building a culture where security is a primary focus and requirement of any application development can make great strides in evolving an organizations capabilities around security.</span></span>

<!-- Bear in mind that the migration of un-modified applications (especially COTS procured software systems) will not be able to perform many of the steps listed above.
 -->

## <a name="operational-security-assessment"></a><span data-ttu-id="b5be5-145">運用上のセキュリティの評価</span><span class="sxs-lookup"><span data-stu-id="b5be5-145">Operational security assessment</span></span>

<span data-ttu-id="b5be5-146">アプリケーションがデプロイされた後、そのセキュリティに対する姿勢を継続的に評価し、検出された問題を軽減する方法を決定し、知識をソフトウェア開発サイクルにフィードバックすることが重要です。</span><span class="sxs-lookup"><span data-stu-id="b5be5-146">Once an application has been deployed, it's essential to continually to evaluate its security posture, determine how to mitigate any issues that are discovered and feed the knowledge back into the software development cycle.</span></span> <span data-ttu-id="b5be5-147">これが実行される深さは、データのプライバシー要件と、ソフトウェア開発および運用チームの成熟度レベルの要因となります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-147">The depth to which this is performed is a factor of the maturity level of the software development and operational teams as well as the data privacy requirements.</span></span>

<span data-ttu-id="b5be5-148">セキュリティの脆弱性をスキャンするソフトウェア サービスは、このプロセスを自動化し、侵入テストなど、コストのかかる手動プロセスでチームに負担をかけることなく、一定のリズムでセキュリティに関する懸念事項を評価するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-148">Security vulnerability scanning software services are available to help automate this process and assess security concerns on a regular cadence, without burdening teams with costly manual processes, such as penetration testing.</span></span>

<span data-ttu-id="b5be5-149">Azure Security Center は無料のサービスであり、すべての Azure サブスクリプションに対して既定で有効になるようになりました。Azure Application Gateway や Azure Web アプリケーション ファイアウォールなどの他の Azure アプリケーション レベルのサービスと緊密に統合されています。</span><span class="sxs-lookup"><span data-stu-id="b5be5-149">Azure Security Center is a free service, now enabled by default for all Azure subscriptions, that is tightly integrated with other Azure application level services, such as Azure Application Gateway and Azure Web Application Firewall.</span></span> <span data-ttu-id="b5be5-150">これらのサービスからのログを分析することで、ASC ではリアルタイムで既知の脆弱性をレポートし、脆弱性を減らすための対応を推奨でき、攻撃の対応でプレイブックを自動的に実行するように構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-150">By analyzing logs from these services ASC can report on known vulnerabilities in real-time, recommend responses to mitigate them and even be configured to automatically execute playbooks in response to attacks.</span></span>

<!-- SDL culture
Key Vault / MSI
CSE = App  -> DB & App Storage
Mention approach of code scanning & SDL
Scanning for passwords - Git
 -->

## <a name="identity-as-the-perimeter"></a><span data-ttu-id="b5be5-151">境界としての ID</span><span class="sxs-lookup"><span data-stu-id="b5be5-151">Identity as the perimeter</span></span>

<span data-ttu-id="b5be5-152">ID 検証は、アプリケーションに関する防御の最前線になりつつあります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-152">Identity validation is becoming the first line in defense for applications.</span></span> <span data-ttu-id="b5be5-153">セッションを認証して承認し、Web アプリケーションへのアクセスを制限することで、攻撃対象領域を大幅に減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-153">Restricting access to a web application by authenticating and authorizing sessions can drastically reduce the attack surface area.</span></span> <span data-ttu-id="b5be5-154">Azure AD と Azure AD B2C では、ID の責任を軽減し、フル マネージドのサービスにアクセスする効果的な方法が提供されます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-154">Azure AD and Azure AD B2C offer an effective way to offload the responsibility of identity and access to a fully managed service.</span></span> <span data-ttu-id="b5be5-155">Azure AD の条件付きアクセス ポリシー、Privileged Identity Management および Identity Protection 制御により、お客様はさらに効率的に未承認のアクセスを防ぎ、変更を監視できるようになります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-155">Azure AD conditional access policies, privileged identity management and Identity Protection controls further enhance a customer's ability to prevent unauthorized access and audit changes.</span></span>

## <a name="data-protection"></a><span data-ttu-id="b5be5-156">データ保護</span><span class="sxs-lookup"><span data-stu-id="b5be5-156">Data protection</span></span>

<span data-ttu-id="b5be5-157">顧客データは、Web アプリケーションに対するほぼすべての攻撃の対象となります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-157">Customer data is the target for most, if not all attacks against web applications.</span></span> <span data-ttu-id="b5be5-158">アプリケーションとそのデータ ストレージ層の間のデータの保管と転送をセキュリティで保護することが重要です。</span><span class="sxs-lookup"><span data-stu-id="b5be5-158">The secure storage and transport of data between an application and its data storage layer is paramount.</span></span>

<span data-ttu-id="b5be5-159">Lamna Healthcare では、特に機密性の高い患者の医療記録データを格納し、このデータにアクセスしています。</span><span class="sxs-lookup"><span data-stu-id="b5be5-159">Lamna Healthcare stores and accesses particularly sensitive patient medical record data.</span></span> <span data-ttu-id="b5be5-160">特に、1996 年に米国議会で制定された HIPAA では、医療機関や雇用主による電子医療記録のトランザクションに関する国家規格が定義されています。</span><span class="sxs-lookup"><span data-stu-id="b5be5-160">HIPAA, enacted by the United States Congress in 1996, among other controls, defines the national standards for electronic healthcare transactions by healthcare providers and employers.</span></span> <span data-ttu-id="b5be5-161">Lamna では、患者と承認された関係者 (医師など) が医療データに安全にアクセスできるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b5be5-161">Lamna must ensure patients and authorized parties, such as their physicians have secure access to medical data.</span></span>

<span data-ttu-id="b5be5-162">これらの要件を満たすために、Lamna Healthcare ではアプリケーションを変更し、保存時および転送中のすべての患者のデータを暗号化するようにしました。</span><span class="sxs-lookup"><span data-stu-id="b5be5-162">To comply with these requirements, Lamna Healthcare have modified their applications to encrypt all patient data at rest and in transit.</span></span> <span data-ttu-id="b5be5-163">たとえば、Web アプリケーションとバックエンドの SQL データベースの間で交換されるデータを暗号化するためにトランスポート層セキュリティ (TLS) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-163">For example, Transport Layer Security (TLS) is used to encrypt data exchanged between the web application and back-end SQL databases.</span></span> <span data-ttu-id="b5be5-164">SQL Server に保存されているデータも透過的なデータ暗号化 (TDE) を使用して暗号化されます。そのため、環境が侵害された場合でも、正しい復号化キーがないと誰もデータを効果的に使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="b5be5-164">Data is also encrypted at rest in SQL Server using Transparent Data Encryption (TDE) ensuring that even if the environment is compromised, data is effectively useless to anyone without the correct decryption keys.</span></span>

<span data-ttu-id="b5be5-165">BLOB ストレージに格納されているデータを暗号化する場合、クライアント側の暗号化を使用してメモリ内のデータを暗号化してから、ストレージ サービスに書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-165">To encrypt data stored in blob storage, client side encryption can be used to encrypt the data in memory before it's written to the storage service.</span></span> <span data-ttu-id="b5be5-166">この暗号化がサポートされているライブラリを .NET、Java、および Python で利用でき、データの暗号化をアプリケーションに直接統合してデータの整合性を高めることができます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-166">Libraries supporting this encryption are available for .NET, Java, and Python, and enable the integration of data encryption directly into applications to enhance data integrity.</span></span>

### <a name="secure-key-and-secret-storage"></a><span data-ttu-id="b5be5-167">キーとシークレットのストレージをセキュリティで保護する</span><span class="sxs-lookup"><span data-stu-id="b5be5-167">Secure key and secret storage</span></span>

<span data-ttu-id="b5be5-168">アプリケーション シークレット (接続文字列やパスワードなど) と暗号化キーを、データにアクセスするために使用するアプリケーションから分離させることが重要です。</span><span class="sxs-lookup"><span data-stu-id="b5be5-168">Separating application secrets (connection strings, passwords, etc.) and encryption keys from the application used to access data is vital.</span></span> <span data-ttu-id="b5be5-169">暗号化キーとアプリケーション シークレットを構成ファイルのアプリケーション コードに格納しないでください。</span><span class="sxs-lookup"><span data-stu-id="b5be5-169">Encryption keys and application secrets should never be stored in the application code of configuration files.</span></span> <span data-ttu-id="b5be5-170">代わりに、Azure Key Vault などの安全なストアを使用してください。</span><span class="sxs-lookup"><span data-stu-id="b5be5-170">Instead, a secure store such as Azure Key Vault should be used.</span></span> <span data-ttu-id="b5be5-171">そうすれば、マネージド サービス ID を使用してこの機密データへのアクセスをアプリケーション ID に制限することができ、キーを定期的に交換し、暗号化キーが漏えいした場合に露出を制限することができます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-171">Access to this sensitive data can then be limited to application identities through Managed Service Identities and keys can be rotated on a regular basis to limit exposure in the case of encryption key leakage.</span></span> <span data-ttu-id="b5be5-172">お客様はオンプレミスのハードウェア セキュリティ モジュール (HSM) によって生成された独自の暗号化キーを使用し、Azure Key Vault インスタンスが必ず単一テナントの個別の HSM に実装されるようにすることもできます。</span><span class="sxs-lookup"><span data-stu-id="b5be5-172">Customers can also choose to use their own encryption keys generated by on-premies Hardware Security Modules (HSM) and even mandate that Azure Key Vault instances are implemented in single-tenant, discrete HSMs.</span></span>

<!-- ### Secure and immutable file storage

All Azure storage accounts are encrypted by default using Microsoft managed keys. Azure customers also have the ability to use their own encryption keys (BYOK) to encrypt blob, file and queue data so that even the hosting provider has no access to unencrypted data. Data immutability is often required for auditing purposes or when legal disputes call for data to be effectively frozen for a determined amount of time. Azure has recently introduced an [immutable data storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-immutable-storage) option known as Write-Once, Read many (WORM) for this scenario. -->

## <a name="application-security-at-lamna-healthcare"></a><span data-ttu-id="b5be5-173">Lamna Healthcare でのアプリケーションのセキュリティ</span><span class="sxs-lookup"><span data-stu-id="b5be5-173">Application security at Lamna Healthcare</span></span>

## <a name="summary"></a><span data-ttu-id="b5be5-174">まとめ</span><span class="sxs-lookup"><span data-stu-id="b5be5-174">Summary</span></span>