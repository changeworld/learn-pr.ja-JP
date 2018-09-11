<span data-ttu-id="19486-101">最近、Lamna Healthcare の顧客向けのアプリケーションが停止し、大きな影響があったことがありました。</span><span class="sxs-lookup"><span data-stu-id="19486-101">Lamna Healthcare recently experienced a significant outage to a customer-facing web application.</span></span> <span data-ttu-id="19486-102">あるエンジニアに、運用環境の Web アプリケーションが含まれるリソース グループへのアクセス許可が完全な付与されていました。</span><span class="sxs-lookup"><span data-stu-id="19486-102">An engineer was granted full access to a resource group containing the production web application.</span></span> <span data-ttu-id="19486-103">このユーザーが間違って、顧客のライブ データがホストされているデータベースを含む、リソース グループとその子リソースをすべて削除してしまいました。</span><span class="sxs-lookup"><span data-stu-id="19486-103">This person accidentally deleted the resource group and all child resources, including the database hosting live customer data.</span></span> 

<span data-ttu-id="19486-104">さいわい、アプリケーションのソース コードとリソースはソース コントロールにあり、データベースの定期的なバックアップがスケジュールどおり自動実行されていました。</span><span class="sxs-lookup"><span data-stu-id="19486-104">Fortunately, the application source code and resources were available in source control and regular database backups were running automatically on a schedule.</span></span> <span data-ttu-id="19486-105">したがって、サービスは比較的簡単に元に戻りました。</span><span class="sxs-lookup"><span data-stu-id="19486-105">Therefore the service was reinstated relatively easily.</span></span> <span data-ttu-id="19486-106">ここでは、どうすればこの停止を、インフラストラクチャへのアクセスを保護する Azure の機能を使用して防ぐことができたかを調べます。</span><span class="sxs-lookup"><span data-stu-id="19486-106">Here, we will explore how this outage could have been avoided by utilizing capabilities on Azure to protect the access to infrastructure.</span></span>

## <a name="criticality-of-infrastructure"></a><span data-ttu-id="19486-107">インフラストラクチャの重要性</span><span class="sxs-lookup"><span data-stu-id="19486-107">Criticality of infrastructure</span></span>

<span data-ttu-id="19486-108">多くの企業にとって、クラウド インフラストラクチャは重要な部分となってきています。</span><span class="sxs-lookup"><span data-stu-id="19486-108">Cloud infrastructure is becoming a critical piece of many businesses.</span></span> <span data-ttu-id="19486-109">ユーザーおよびプロセスが、必要なアクセス許可のみで、遂行すべき作業を実行できることが保証される必要があります。</span><span class="sxs-lookup"><span data-stu-id="19486-109">It is critical to ensure people and processes have only the rights they need to get their job done.</span></span> <span data-ttu-id="19486-110">不正なアクセス許可を付与してしまうと、データが失われたり、データが漏えいしたり、サービスを提供できなくなってしまいます。</span><span class="sxs-lookup"><span data-stu-id="19486-110">Assigning incorrect access can result in data loss, data leakage or cause services to become unavailable.</span></span> 

<span data-ttu-id="19486-111">システム管理者は、ユーザー、システムおよびアクセス許可のセットの責任を多数負っている場合があります。</span><span class="sxs-lookup"><span data-stu-id="19486-111">System administrators can be responsible for a large number of users, systems and permission sets.</span></span> <span data-ttu-id="19486-112">したがって、アクセス許可の正しい付与の管理がすぐにできなくなり、対応がワンパターンになってしまう場合があります。</span><span class="sxs-lookup"><span data-stu-id="19486-112">So correctly granting access can quickly become unmanageable and can lead to a 'one size fits all' approach.</span></span> <span data-ttu-id="19486-113">このアプローチでは、管理は単純化できるかもしれませんが、必要以上の制限の少ないアクセスを誤って付与してしまいやすくなります。</span><span class="sxs-lookup"><span data-stu-id="19486-113">This approach can reduce the complexity of administration, but makes it far easier to inadvertently grant more permissive access than required.</span></span>

## <a name="role-based-access-control"></a><span data-ttu-id="19486-114">ロールベースのアクセス制御</span><span class="sxs-lookup"><span data-stu-id="19486-114">Role-based access control</span></span>

<span data-ttu-id="19486-115">ロールベースのアクセス制御 (RBAC) のアプローチは、若干異なります。</span><span class="sxs-lookup"><span data-stu-id="19486-115">Role-based access control (RBAC) offers a slightly different approach.</span></span> <span data-ttu-id="19486-116">ロールとは、アクセス許可の集合であると定義されています。</span><span class="sxs-lookup"><span data-stu-id="19486-116">Roles are defined as collections of access permissions.</span></span> <span data-ttu-id="19486-117">セキュリティ プリンシパルは、ロールに直接マップするか、グループ メンバーシップを介してマップします。</span><span class="sxs-lookup"><span data-stu-id="19486-117">Security principals are mapped to roles directly or through group membership.</span></span> <span data-ttu-id="19486-118">セキュリティ プリンシパル、アクセス許可およびリソースを分離すると、アクセスの管理が単純になり、よりきめ細やかに制御ができるようになります。</span><span class="sxs-lookup"><span data-stu-id="19486-118">Separating security principals, access permissions, and resources provides simplified access management and more fine-grained control.</span></span>

<span data-ttu-id="19486-119">Azure では、ユーザー、グループおよびロールは、すべて Azure Active Directory (Azure AD) に保存されます。</span><span class="sxs-lookup"><span data-stu-id="19486-119">On Azure, users, groups, and roles are all stored in Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="19486-120">Azure Resource Manager の API は、ロールベースのアクセス制御を使用し、Azure 内のすべてのリソースへのアクセス管理を制御しています。</span><span class="sxs-lookup"><span data-stu-id="19486-120">The Azure Resource Manager API uses role-based access control to secure all resource access management within Azure.</span></span>

![ACL ベースのアクセス](../media-draft/ACL_Based_Access.png)

<!-- ![Role-based access control](../media-draft/Role_Based_Access.png)
 -->

### <a name="roles--management-groups"></a><span data-ttu-id="19486-122">ロールおよび管理グループ</span><span class="sxs-lookup"><span data-stu-id="19486-122">Roles & management groups</span></span>

<span data-ttu-id="19486-123">ロールとは、"読み取り専用" または "共同作成者" など、Azure のサービス インスタンスにアクセスするためにユーザーに付与されるアクセス許可のセットです。</span><span class="sxs-lookup"><span data-stu-id="19486-123">Roles are sets of permissions, like "Read-only" or "Contributor", that users can granted to access an Azure service instance.</span></span> <span data-ttu-id="19486-124">ロールは、サービス インスタンス レベルで個々に付与することもできますが、Azure Resource Manager の階層で下位にも伝送されます。</span><span class="sxs-lookup"><span data-stu-id="19486-124">Roles can be granted at the individual service instance level but they also flow down the Azure Resource manager hierarchy.</span></span> <span data-ttu-id="19486-125">サブスクリプション全体など、より高いスコープに割り当てられているロールは、サービス インスタンスなどの子スコープへ継承されます。</span><span class="sxs-lookup"><span data-stu-id="19486-125">Roles assigned at a higher scope, like an entire subscription, are inherited by child scopes, like service instances.</span></span> 

<span data-ttu-id="19486-126">管理グループとは、RBAC モデルに最近導入された、追加の階層レベルです。</span><span class="sxs-lookup"><span data-stu-id="19486-126">Management groups are an additional hierarchical level recently introduced into the RBAC model.</span></span> <span data-ttu-id="19486-127">管理グループには、サブスクリプションをまとめてグループ化する機能や、より高いレベルにポリシーを適用する機能が追加されています。</span><span class="sxs-lookup"><span data-stu-id="19486-127">Management groups add the ability to group subscriptions together and apply policy at an even higher level.</span></span>

<span data-ttu-id="19486-128">任意に定義されたサブスクリプション階層でロールを伝送する機能により、管理者は、認証されたユーザーが環境全体への一時的なアクセス許可を得ることができるようにできます。</span><span class="sxs-lookup"><span data-stu-id="19486-128">The ability to flow roles through an arbitrarily defined subscription hierarchy also allows administrators to grant temporary access to an entire environment for authenticated users.</span></span> <span data-ttu-id="19486-129">たとえば、監査担当者は、すべてのサブスクリプションに対して一時的な読み取り専用のアクセス許可を付与することができます。</span><span class="sxs-lookup"><span data-stu-id="19486-129">For example, an auditor may require temporary read-only access to all subscriptions.</span></span>

![管理グループ](../media-draft/management_groups.png)

### <a name="privileged-identity-management"></a><span data-ttu-id="19486-131">Privileged Identity Management</span><span class="sxs-lookup"><span data-stu-id="19486-131">Privileged Identity Management</span></span>

<span data-ttu-id="19486-132">インフラストラクチャを包括的に管理する場合、Azure のリソースへのアクセスを RBAC を使用して管理することに加え、組織の変化や発展に伴い、ロールのメンバーを継続的に監査することも行われる必要があります。</span><span class="sxs-lookup"><span data-stu-id="19486-132">In addition to managing Azure resource access with RBAC, a comprehensive approach to infrastructure protection should consider including the ongoing auditing of role members as their organization changes and evolves.</span></span> <span data-ttu-id="19486-133">追加の有料のオファリングに、Azure AD Privileged Identity Management (PIM) があります。これでは、ロールの割り当て、セルフサービス、ジャストインタイムでのロールのアクティブ化および Azure AD と Azure のリソースへのアクセス確認を監視できます。</span><span class="sxs-lookup"><span data-stu-id="19486-133">Azure AD Privileged Identity Management (PIM) is an additional paid-for offering that provides oversight of role assignments, self-service, and just-in-time role activation and Azure AD & Azure resource access reviews.</span></span>

![Privileged Identity Management](../media-draft/PIM_Dashboard.PNG)

## <a name="providing-identities-to-services"></a><span data-ttu-id="19486-135">サービスへの ID の提供</span><span class="sxs-lookup"><span data-stu-id="19486-135">Providing identities to services</span></span>

<span data-ttu-id="19486-136">サービスに ID があると役立つことが多々あります。</span><span class="sxs-lookup"><span data-stu-id="19486-136">It's often valuable for services to have identities.</span></span> <span data-ttu-id="19486-137">ベスト プラクティスには反しますが、資格情報はしばしば構成ファイルに埋め込まれています。</span><span class="sxs-lookup"><span data-stu-id="19486-137">Often times, and against best practices, credential information is embedded in configuration files.</span></span> <span data-ttu-id="19486-138">これらの構成ファイルはセキュリティで保護されていないので、システムやリポジトリにアクセスできるすべてのユーザーがこれらの資格情報にアクセスし、公開されてしまうリスクがあります。</span><span class="sxs-lookup"><span data-stu-id="19486-138">With no security around these configuration files, anyone with access to the systems or repositories can access these credentials and risk exposure.</span></span>

<span data-ttu-id="19486-139">Azure AD では、サービス プリンシパルとマネージド サービス ID の 2 つの方法でこの問題を解決しています。</span><span class="sxs-lookup"><span data-stu-id="19486-139">Azure AD addresses this problem through two methods: service principals and managed service identities.</span></span>

### <a name="service-principals"></a><span data-ttu-id="19486-140">サービス プリンシパル</span><span class="sxs-lookup"><span data-stu-id="19486-140">Service principals</span></span>

<span data-ttu-id="19486-141">サービス プリンシパルを理解するには、まず、ID 管理の世界で使用される **ID** と**プリンシパル**という用語を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="19486-141">To understand Service principals it's useful to first understand the words **identity** and **principal** as they are used in Identity management world.</span></span>

<span data-ttu-id="19486-142">**ID** とは、単に認証できるものであるという意味です。</span><span class="sxs-lookup"><span data-stu-id="19486-142">An **identity** is just a thing that can be authenticated.</span></span> <span data-ttu-id="19486-143">これには、当然ユーザー名とパスワードを持つユーザーが含まれますが、シークレット キーや証明書で認証される場合のあるアプリケーションやその他のサーバーも含まれます。</span><span class="sxs-lookup"><span data-stu-id="19486-143">Obviously this includes users with username and password but it can also include applications or other servers, which might authenticate with secret keys or certificates.</span></span> <span data-ttu-id="19486-144">おまけの定義である**アカウント**とは、ID と関連付けられているデータです。</span><span class="sxs-lookup"><span data-stu-id="19486-144">As a bonus definition, an **account** is data associated with an identity.</span></span>

<span data-ttu-id="19486-145">**プリンシパル**とは、特定のロールや要求と動作する ID です。</span><span class="sxs-lookup"><span data-stu-id="19486-145">A **principal** is an identity acting with certain roles or claims.</span></span> <span data-ttu-id="19486-146">多くの場合、ID とプリンシパルを別に考えるのは便利ではありませんが、bash プロンプトで 'sudo' を使用する場合や、"管理者として実行" を Windows で使用する場合を考えてみてください。</span><span class="sxs-lookup"><span data-stu-id="19486-146">Often it is not useful to consider identity and principal separately, but think of using 'sudo' on a bash prompt or on Windows using "run as Administrator".</span></span> <span data-ttu-id="19486-147">それらのいずれの場合も、ユーザーは前と同じ ID でログインしていますが、実行しているロールが変更されています。</span><span class="sxs-lookup"><span data-stu-id="19486-147">In both those case you are still logged in as the same identity as before, but you've changed the role under which you are executing.</span></span>

<span data-ttu-id="19486-148">したがって、**サービス プリンシパル**は、文字どおりの名前です。</span><span class="sxs-lookup"><span data-stu-id="19486-148">So a **Service Principal** is literally named.</span></span> <span data-ttu-id="19486-149">これは、サービスまたはアプリケーションが使用する ID です。</span><span class="sxs-lookup"><span data-stu-id="19486-149">It is an identity that is used by a service or application.</span></span> <span data-ttu-id="19486-150">他の ID 同様、割り当てられるロールにすることも可能です。</span><span class="sxs-lookup"><span data-stu-id="19486-150">Like other identities it can be assigned roles</span></span> 

<span data-ttu-id="19486-151">たとえば、Lamna Healthcare では、サービス プリンシパルとして認証されるように、その配置スクリプトに割り当てて実行することができます。</span><span class="sxs-lookup"><span data-stu-id="19486-151">For example, Lamna Healthcare can assign its deployment scripts to run authenticated as a service principal.</span></span> <span data-ttu-id="19486-152">これが、破壊的なアクションを実行するアクセス許可を持つ唯一の ID であった場合、リソースの偶発的な削除が再度起こらないことを、Lamna が確認するには非常に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="19486-152">If that is the only identity that has permission to perform destructive actions Lamna will have gone a long way toward making sure they don't have a repeat of the accidental resource deletion.</span></span>

### <a name="managed-service-identities"></a><span data-ttu-id="19486-153">マネージド サービス ID</span><span class="sxs-lookup"><span data-stu-id="19486-153">Managed Service Identities</span></span>

<span data-ttu-id="19486-154">サービス プリンシパルを作成するプロセスは退屈であり、それらを維持するのが困難であるタッチ ポイントが多くがあります。</span><span class="sxs-lookup"><span data-stu-id="19486-154">The creation of service principals can be a tedious process and there are a lot of touch points which can make maintaining them difficult.</span></span> <span data-ttu-id="19486-155">マネージド サービス ID (MSI) ははるかに簡単であり、作業のほとんどがユーザーに代わって実行されます。</span><span class="sxs-lookup"><span data-stu-id="19486-155">Manage Service Identities (MSI) are much easier and will do most of the work for you.</span></span> 

<span data-ttu-id="19486-156">MSI は、MSI をサポートするすべての Azure サービス (これをサポートするサービスは増え続けています) で即座に作成することができます。</span><span class="sxs-lookup"><span data-stu-id="19486-156">A MSI can be instantly created for any Azure service that supports it (the list is constantly growing).</span></span> <span data-ttu-id="19486-157">ユーザーがサービス用に MSI を作成する場合、アカウントは Azure Active Directory テナントに作成することになります。</span><span class="sxs-lookup"><span data-stu-id="19486-157">When you create an MSI for a service, you are creating an account on the Azure Active Directory tenant.</span></span> <span data-ttu-id="19486-158">Azure インフラストラクチャは、サービスの認証とアカウントの管理を自動で行ってくれます。</span><span class="sxs-lookup"><span data-stu-id="19486-158">Azure infrastructure will automatically take care of authenticating the service and managing the account.</span></span> <span data-ttu-id="19486-159">そのアカウントは、他のすべての AD アカウントと同様に使用できます。これには、認証されているサービスが安全にその他の Azure リソースにアクセスできるようにすることも含まれます。</span><span class="sxs-lookup"><span data-stu-id="19486-159">You can then use that account like any other AD account including securely letting the authenticated service access other Azure resources.</span></span>

<span data-ttu-id="19486-160">Lamna Healthcare では、同社の ID 管理を一歩進め、インフラストラクチャの管理と配置を実行する機能が必要なサポートされているすべてのサービスに、MSI を使用しています。</span><span class="sxs-lookup"><span data-stu-id="19486-160">Lamna Healthcare takes their identity management a step further and uses MSIs for all supported services that need the ability to perform infrastructure management and deployments.</span></span>

## <a name="infrastructure-protection-at-lamna-healthcare"></a><span data-ttu-id="19486-161">Lamna Healthcare でのインフラストラクチャの保護</span><span class="sxs-lookup"><span data-stu-id="19486-161">Infrastructure protection at Lamna Healthcare</span></span>

<span data-ttu-id="19486-162">インフラストラクチャを誤って削除してしまった Lamna Healthcare が、それをどのように解決したかを説明しました。</span><span class="sxs-lookup"><span data-stu-id="19486-162">We've seen how Lamna Healthcare has addressed issues from their incident where infrastructure was inadvertently deleted.</span></span> <span data-ttu-id="19486-163">同社では、ロールベースのアクセス制御を使用し、インフラストラクチャのセキュリティ管理を向上させ、マネージド サービス ID を使用して、資格情報をコードの外に配置し、サービスに必要な ID の管理を簡単にしました。</span><span class="sxs-lookup"><span data-stu-id="19486-163">They've used role-based access control to better manage the security of their infrastructure, and are using managed service identities to keep their credentials out of code and ease administration of the identities needed for their services.</span></span>

## <a name="summary"></a><span data-ttu-id="19486-164">まとめ</span><span class="sxs-lookup"><span data-stu-id="19486-164">Summary</span></span>

<span data-ttu-id="19486-165">インフラストラクチャの可用性と整合性を確保するには、インフラストラクチャを適切に保護する必要があります。</span><span class="sxs-lookup"><span data-stu-id="19486-165">To ensure the availability and integrity of infrastructure, it's important to properly secure your infrastructure.</span></span> <span data-ttu-id="19486-166">RBAC やマネージド サービス ID などの機能を正しく使用すると、使用している Azure 環境を許可されていないアクセスや意図されていないアクセスから守り、アーキテクチャの ID セキュリティ機能を向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="19486-166">Properly using features such as RBAC and managed service identities will help protect your Azure environment from unauthorized or unintended access, and will enhance the identity security capabilities in your architecture.</span></span>