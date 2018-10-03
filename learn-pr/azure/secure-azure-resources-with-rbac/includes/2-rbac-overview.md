<span data-ttu-id="81713-101">開発、エンジニアリング、およびマーケティング チームに対して、Azure 内のリソースへのアクセス許可を管理する必要があるものとします。</span><span class="sxs-lookup"><span data-stu-id="81713-101">Suppose you need to manage access to resources in Azure for the development, engineering, and marketing teams.</span></span> <span data-ttu-id="81713-102">アクセス権の要求を受け取り始めており、Azure リソースに対してアクセス管理がどのように機能するのかを早く学習する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81713-102">You’ve started to receive access requests, and you need to quickly learn how access management works for Azure resources.</span></span>

## <a name="what-is-rbac"></a><span data-ttu-id="81713-103">RBAC とは</span><span class="sxs-lookup"><span data-stu-id="81713-103">What is RBAC?</span></span>

<span data-ttu-id="81713-104">ロールベースのアクセス制御 (RBAC) は、Azure Resource Manager 上に構築された認可システムであり、Azure 内のリソースに対するアクセスをきめ細かく管理できます。</span><span class="sxs-lookup"><span data-stu-id="81713-104">Role-based access control (RBAC) is an authorization system built on Azure Resource Manager that provides fine-grained access management of resources in Azure.</span></span> <span data-ttu-id="81713-105">Azure には多くのリソースがありますが、仮想マシン、Web サイト、ネットワーク、ストレージが含まれる例は多くありません。</span><span class="sxs-lookup"><span data-stu-id="81713-105">Azure has lots of resources, but a few examples include virtual machines, websites, networks, and storage.</span></span>

#### <a name="what-is-role-based-access-control"></a><span data-ttu-id="81713-106">ロールベースのアクセス制御とは</span><span class="sxs-lookup"><span data-stu-id="81713-106">What is role-based access control?</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvk]

## <a name="what-can-i-do-with-rbac"></a><span data-ttu-id="81713-107">RBAC でできることは何ですか?</span><span class="sxs-lookup"><span data-stu-id="81713-107">What can I do with RBAC?</span></span>

<span data-ttu-id="81713-108">RBAC を使用すると、自分が制御する Azure リソースに対するアクセス権を付与できます。</span><span class="sxs-lookup"><span data-stu-id="81713-108">RBAC allows you to grant access to Azure resources that you control.</span></span>

<span data-ttu-id="81713-109">次に例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="81713-109">Here are some examples:</span></span>

- <span data-ttu-id="81713-110">あるユーザーにサブスクリプション内の仮想マシンの管理を許可し、別のユーザーに仮想ネットワークの管理を許可します</span><span class="sxs-lookup"><span data-stu-id="81713-110">Allow one user to manage virtual machines in a subscription and another user to manage virtual networks</span></span>
- <span data-ttu-id="81713-111">データベース管理者グループにサブスクリプション内の SQL データベースの管理を許可します</span><span class="sxs-lookup"><span data-stu-id="81713-111">Allow a database administrator group to manage SQL databases in a subscription</span></span>
- <span data-ttu-id="81713-112">あるユーザーに、仮想マシン、Web サイト、サブネットなど、リソース グループ内のすべてのリソースの管理を許可します</span><span class="sxs-lookup"><span data-stu-id="81713-112">Allow a user to manage all resources in a resource group, such as virtual machines, websites, and subnets</span></span>
- <span data-ttu-id="81713-113">あるアプリケーションに、リソース グループ内のすべてのリソースへのアクセスを許可します</span><span class="sxs-lookup"><span data-stu-id="81713-113">Allow an application to access all resources in a resource group</span></span>

## <a name="rbac-in-the-azure-portal"></a><span data-ttu-id="81713-114">Azure portal での RBAC</span><span class="sxs-lookup"><span data-stu-id="81713-114">RBAC in the Azure portal</span></span>

<span data-ttu-id="81713-115">Azure portal のいくつかの領域に、**[アクセス制御 (IAM)]** という名前のブレードが表示されます。これは、ID やアクセス管理とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="81713-115">In several areas in the Azure portal, you'll see a blade named **Access control (IAM)**, also known as identity and access management.</span></span> <span data-ttu-id="81713-116">このブレードでは、その領域へのアクセス権を持つユーザーとそのロールを確認できます。</span><span class="sxs-lookup"><span data-stu-id="81713-116">On this blade, you can see who has access to that area and their role.</span></span> <span data-ttu-id="81713-117">この同じブレードを使用して、アクセス許可を付与または削除できます。</span><span class="sxs-lookup"><span data-stu-id="81713-117">Using this same blade, you can grant or remove access.</span></span>

<span data-ttu-id="81713-118">リソース グループに対する [アクセス制御 (IAM)] ブレードの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="81713-118">The following shows an example of the Access control (IAM) blade for a resource group.</span></span> <span data-ttu-id="81713-119">この例で、Alain Charon には、このリソース グループに対するバックアップ オペレーター ロールが割り当てられています。</span><span class="sxs-lookup"><span data-stu-id="81713-119">In this example, Alain Charon has been assigned the Backup Operator role for this resource group.</span></span>

![Azure portal の [アクセス制御 (IAM)]](../media/2-resource-group-access-control.png)

## <a name="how-does-rbac-work"></a><span data-ttu-id="81713-121">RBAC のしくみ</span><span class="sxs-lookup"><span data-stu-id="81713-121">How does RBAC work?</span></span>

<span data-ttu-id="81713-122">ロールの割り当てを作成することによって、RBAC を使用してリソースへのアクセスを制御します。これは、アクセス許可を適用する方法を制御します。</span><span class="sxs-lookup"><span data-stu-id="81713-122">You control access to resources using RBAC by creating role assignments, which control how permissions are enforced.</span></span> <span data-ttu-id="81713-123">ロールの割り当てを作成するには、次の 3 つの要素が必要です。セキュリティ プリンシパル、ロールの定義、スコープです。</span><span class="sxs-lookup"><span data-stu-id="81713-123">To create a role assignment, you need three elements: a security principal, a role definition, and a scope.</span></span> <span data-ttu-id="81713-124">これらの要素を "だれが"、"何を"、"どこで" として考えることができます。</span><span class="sxs-lookup"><span data-stu-id="81713-124">You can think of these elements as "who", "what", and "where".</span></span>

### <a name="1-security-principal-who"></a><span data-ttu-id="81713-125">1.セキュリティ プリンシパル (だれが)</span><span class="sxs-lookup"><span data-stu-id="81713-125">1. Security principal (who)</span></span>

<span data-ttu-id="81713-126">*セキュリティ プリンシパル*は、アクセス許可を付与するユーザー、グループ、またはアプリケーションへの単に装飾的な名前です。</span><span class="sxs-lookup"><span data-stu-id="81713-126">A *security principal* is just a fancy name for a user, group, or application that you want to grant access to.</span></span>

![ユーザー、グループ、サービス プリンパス スルーなどのセキュリティ プリンシパルを示す図。](../media/2-rbac-security-principal.png)

### <a name="2-role-definition-what-you-can-do"></a><span data-ttu-id="81713-128">2.ロールの定義 (実行できること)</span><span class="sxs-lookup"><span data-stu-id="81713-128">2. Role definition (what you can do)</span></span>

<span data-ttu-id="81713-129">*ロール定義*は、アクセス許可のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="81713-129">A *role definition* is a collection of permissions.</span></span> <span data-ttu-id="81713-130">単にロールと呼ばれることもあります。</span><span class="sxs-lookup"><span data-stu-id="81713-130">It's sometimes just called a role.</span></span> <span data-ttu-id="81713-131">ロール定義には、実行できるアクセス許可 (読み取り、書き込み、削除など) が一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="81713-131">A role definition lists the permissions that can be performed, such as read, write, and delete.</span></span> <span data-ttu-id="81713-132">ロールは、所有者のように高レベルにすることも、Virtual Machine Contributor のように限定することもできます。</span><span class="sxs-lookup"><span data-stu-id="81713-132">Roles can be high-level, like Owner, or specific, like Virtual Machine Contributor.</span></span>

![さまざまな組み込みロールとカスタム ロールが一覧表示され、共同作成者ロールの定義が拡大表示されている図。](../media/2-rbac-role-definition.png)

<span data-ttu-id="81713-134">Azure には、ユーザーが使用できる複数の組み込みロールがあります。</span><span class="sxs-lookup"><span data-stu-id="81713-134">Azure includes several built-in roles that you can use.</span></span> <span data-ttu-id="81713-135">4 つの基本的な組み込みロールを次に示します。</span><span class="sxs-lookup"><span data-stu-id="81713-135">The following lists four fundamental built-in roles:</span></span>

- <span data-ttu-id="81713-136">所有者 - 他のユーザーにアクセス許可を委任する権限を含め、すべてのリソースへのフル アクセス権を持ちます。</span><span class="sxs-lookup"><span data-stu-id="81713-136">Owner - Has full access to all resources, including the right to delegate access to others.</span></span>
- <span data-ttu-id="81713-137">共同作成者 - Azure リソースのすべてのタイプを作成および管理できますが、他のユーザーにアクセス許可を付与することはできません。</span><span class="sxs-lookup"><span data-stu-id="81713-137">Contributor - Can create and manage all types of Azure resources, but can’t grant access to others.</span></span>
- <span data-ttu-id="81713-138">閲覧者 - 既存の Azure リソースを表示できます。</span><span class="sxs-lookup"><span data-stu-id="81713-138">Reader - Can view existing Azure resources.</span></span>
- <span data-ttu-id="81713-139">ユーザー アクセス管理者 - Azure リソースへのユーザー アクセスを管理できます。</span><span class="sxs-lookup"><span data-stu-id="81713-139">User Access Administrator - Lets you manage user access to Azure resources.</span></span>

<span data-ttu-id="81713-140">組み込みロールが組織の特定のニーズを満たさない場合は、独自のカスタム ロールを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="81713-140">If the built-in roles don't meet the specific needs of your organization, you can create your own custom roles.</span></span>

### <a name="3-scope-where"></a><span data-ttu-id="81713-141">3.スコープ (どこで)</span><span class="sxs-lookup"><span data-stu-id="81713-141">3. Scope (where)</span></span>

<span data-ttu-id="81713-142">*スコープ*は、アクセス許可が適用される場所です。</span><span class="sxs-lookup"><span data-stu-id="81713-142">*Scope* is where the access applies to.</span></span> <span data-ttu-id="81713-143">これは、1 つのリソース グループについてのみ、あるユーザーを Web サイトの共同作成者として指定する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="81713-143">This is helpful if you want to make someone a Website Contributor, but only for one resource group.</span></span>

<span data-ttu-id="81713-144">Azure では、複数のレベル (管理グループ、サブスクリプション、リソース グループ、リソース) でスコープを指定できます。</span><span class="sxs-lookup"><span data-stu-id="81713-144">In Azure, you can specify a scope at multiple levels: management group, subscription, resource group, or resource.</span></span> <span data-ttu-id="81713-145">スコープは親子関係で構造化されています。</span><span class="sxs-lookup"><span data-stu-id="81713-145">Scopes are structured in a parent-child relationship.</span></span> <span data-ttu-id="81713-146">親スコープでアクセス権を付与すると、それらのアクセス許可が子スコープに継承されます。</span><span class="sxs-lookup"><span data-stu-id="81713-146">When you grant access at a parent scope, those permissions are inherited by the child scopes.</span></span> <span data-ttu-id="81713-147">たとえば、サブスクリプション スコープで共同作成者ロールをグループに割り当てる場合、そのロールはサブスクリプション内のすべてのリソース グループとリソースに継承されます。</span><span class="sxs-lookup"><span data-stu-id="81713-147">For example, if you assign the Contributor role to a group at the subscription scope, that role is inherited by all resource groups and resources in the subscription.</span></span>

![スコープを適用するさまざまな Azure レベルの階層表現を示す図。](../media/2-rbac-scope.png)

### <a name="role-assignment"></a><span data-ttu-id="81713-150">ロールの割り当て</span><span class="sxs-lookup"><span data-stu-id="81713-150">Role assignment</span></span>

<span data-ttu-id="81713-151">だれが、何を、どこで、を決定すると、アクセス許可を付与するそれらの要素を結合させることができます。</span><span class="sxs-lookup"><span data-stu-id="81713-151">Once you have determined the who, what, and where, you can combine those elements to grant access.</span></span> <span data-ttu-id="81713-152">*ロールの割り当て*は、アクセスの許可を目的として、特定のスコープでサービス プリンシパルにロールをバインドするプロセスです。</span><span class="sxs-lookup"><span data-stu-id="81713-152">A *role assignment* is the process of binding a role to a security principal at a particular scope, for the purpose of granting access.</span></span> <span data-ttu-id="81713-153">アクセス権を付与するには、ロールの割り当てを作成します。</span><span class="sxs-lookup"><span data-stu-id="81713-153">To grant access, you create a role assignment.</span></span> <span data-ttu-id="81713-154">アクセス権を削除するには、ロールの割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="81713-154">To revoke access, you remove a role assignment.</span></span>

<span data-ttu-id="81713-155">この例では、マーケティング グループが、どのように営業リソース グループ スコープで共同作成者ロールを割り当てられているかを示しています。</span><span class="sxs-lookup"><span data-stu-id="81713-155">The following example shows how the Marketing group has been assigned the Contributor role at the sales resource group scope.</span></span>

![マーケティング グループのサンプル ロール割り当てプロセスを示す図。セキュリティ プリンシパル、ロール定義、スコープが組み合わされています。](../media/2-rbac-overview.png)

## <a name="rbac-is-an-allow-model"></a><span data-ttu-id="81713-158">RBAC は許可モデル</span><span class="sxs-lookup"><span data-stu-id="81713-158">RBAC is an allow model</span></span>

<span data-ttu-id="81713-159">RBAC は許可モデルです。</span><span class="sxs-lookup"><span data-stu-id="81713-159">RBAC is an allow model.</span></span> <span data-ttu-id="81713-160">つまり、これはロールを割り当てられたときに、RBAC では読み取り、書き込み、削除などの特定のアクションを実行できるということです。</span><span class="sxs-lookup"><span data-stu-id="81713-160">What this means is that when you are assigned a role, RBAC allows you to perform certain actions, such as read, write, or delete.</span></span> <span data-ttu-id="81713-161">そのため、1 つのロールの割り当てによってあるリソース グループへの読み取りアクセス許可が付与されていて、別のロールの割り当てによって同じリソース グループへの書き込みアクセス許可が付与されている場合、ユーザーはそのリソース グループで書き込みアクセス許可を持つことになります。</span><span class="sxs-lookup"><span data-stu-id="81713-161">So, if one role assignment grants you read permissions to a resource group and a different role assignment grants you write permissions to the same resource group, you will have write permissions on that resource group.</span></span>

<span data-ttu-id="81713-162">RBAC には、`NotActions` アクセス許可と呼ばれるものがあります。</span><span class="sxs-lookup"><span data-stu-id="81713-162">RBAC has something called `NotActions` permissions.</span></span> <span data-ttu-id="81713-163">`NotActions` は拒否ルールとは異なり、特定のアクセス許可を除外する必要があるときに、許可の対象となる一連のアクセス許可を作成しやすくすることを目的としたものに過ぎません。</span><span class="sxs-lookup"><span data-stu-id="81713-163">`NotActions` is not a deny rule – it is simply a convenient way to create a set of allowed permissions when specific permissions need to be excluded.</span></span>

<span data-ttu-id="81713-164">このユニットでは、RBAC のしくみの基本について学習しました。</span><span class="sxs-lookup"><span data-stu-id="81713-164">In this unit, you learned the basics of how RBAC works.</span></span> <span data-ttu-id="81713-165">これで、RBAC の基礎を理解できたので、RBAC の使用を始めることができます。</span><span class="sxs-lookup"><span data-stu-id="81713-165">Now that you have the RBAC fundamentals out of the way, you can get your hands dirty by starting to use RBAC.</span></span> <span data-ttu-id="81713-166">作業を開始するには、Azure portal を使用するのが最も簡単です。</span><span class="sxs-lookup"><span data-stu-id="81713-166">The easiest way to get started is to use the Azure portal.</span></span> <span data-ttu-id="81713-167">このモジュールの残りで、RBAC に関連する実践的な演習を行えます。</span><span class="sxs-lookup"><span data-stu-id="81713-167">The rest of this module has you perform hands-on exercises related to RBAC.</span></span>