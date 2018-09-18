<span data-ttu-id="f2a34-101">"First Up Consultants" で、マーケティング チームに対して Azure サブスクリプションへのアクセス許可を付与しました。</span><span class="sxs-lookup"><span data-stu-id="f2a34-101">At First Up Consultants, you've been granted access to the Azure subscription for the marketing team.</span></span> <span data-ttu-id="f2a34-102">Azure portal をよく理解し、現在割り当てられているロールを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2a34-102">You want to familiarize yourself with the Azure portal and see what roles are currently assigned.</span></span>

## <a name="list-role-assignments-for-yourself"></a><span data-ttu-id="f2a34-103">自分のロールの割り当ての一覧表示</span><span class="sxs-lookup"><span data-stu-id="f2a34-103">List role assignments for yourself</span></span>

<span data-ttu-id="f2a34-104">現在、ご自分に割り当てられているロールを確認するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="f2a34-104">Follow these steps to see what roles are currently assigned to you.</span></span>

1. <span data-ttu-id="f2a34-105">[Azure portal](https://portal.azure.com/?azure-portal=true) に移動します。</span><span class="sxs-lookup"><span data-stu-id="f2a34-105">Navigate to the [Azure portal](https://portal.azure.com/?azure-portal=true).</span></span>

1. <span data-ttu-id="f2a34-106">ナビゲーション リストで、**[Azure Active Directory]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2a34-106">In the navigation list, click **Azure Active Directory**.</span></span>

1. <span data-ttu-id="f2a34-107">**[ユーザー]** をクリックして、**[すべてのユーザー]** を開きます。</span><span class="sxs-lookup"><span data-stu-id="f2a34-107">Click **Users** to open **All users**.</span></span>

    ![Azure Active Directory のユーザー](../media-draft/4-aad-all-users.png)

1. <span data-ttu-id="f2a34-109">**[LabAdmin-_XXXXXXX_]** というユーザー名を見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2a34-109">Find and click the **LabAdmin-_XXXXXXX_** user name.</span></span>

    ![Azure Active Directory ラボのユーザー](../media-draft/4-aad-all-users-lab.png)

1. <span data-ttu-id="f2a34-111">**[管理]** セクションで、**[Azure リソース]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2a34-111">In the **Manage** section, click **Azure resources**.</span></span>

    ![Azure リソース](../media-draft/4-aad-user-azure-resources.png)

    <span data-ttu-id="f2a34-113">[Azure リソース] ブレードで、お客様がアクセス権を持つリソースとロールを確認できます。</span><span class="sxs-lookup"><span data-stu-id="f2a34-113">On the Azure resources blade, you can see the resources and roles you have access to.</span></span> <span data-ttu-id="f2a34-114">お客様の一覧の外観は異なります。</span><span class="sxs-lookup"><span data-stu-id="f2a34-114">Your list will look different.</span></span>

## <a name="list-role-assignments-for-a-resource-group"></a><span data-ttu-id="f2a34-115">リソース グループに対するロールの割り当ての一覧表示</span><span class="sxs-lookup"><span data-stu-id="f2a34-115">List role assignments for a resource group</span></span>

<span data-ttu-id="f2a34-116">リソース グループ スコープで割り当てられているロールを表示するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="f2a34-116">Follow these steps to see what roles are assigned at the resource group scope.</span></span>

1. <span data-ttu-id="f2a34-117">ナビゲーション リストで、**[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2a34-117">In the navigation list, click **Resource groups**.</span></span>

   ![リソース グループ](../media-draft/4-resource-groups.png)

1. <span data-ttu-id="f2a34-119">**FirstUpConsultantsRG1-_XXXXXXX_** という名前のリソース グループをクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2a34-119">Click the resource group named **FirstUpConsultantsRG1-_XXXXXXX_**.</span></span>

1. <span data-ttu-id="f2a34-120">**[アクセス制御 (IAM)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2a34-120">Click **Access control (IAM)**.</span></span>

   <span data-ttu-id="f2a34-121">[アクセス制御 (IAM)] ブレードで、このリソース グループにアクセスできるユーザーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="f2a34-121">On the Access control (IAM) blade, you can see who has access to this resource group.</span></span> <span data-ttu-id="f2a34-122">スコープが **[このリソース]** のロールもあれば、親スコープからスコープを **[(継承)]** しているロールもあることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="f2a34-122">Notice that some roles are scoped to **This resource** while others are **(Inherited)** from a parent scope.</span></span>

   ![リソース グループに対する [アクセス制御 (IAM)]](../media-draft/4-resource-group-access-control.png)

## <a name="list-roles"></a><span data-ttu-id="f2a34-124">ロールの一覧表示</span><span class="sxs-lookup"><span data-stu-id="f2a34-124">List roles</span></span>

<span data-ttu-id="f2a34-125">前のユニットで学習したように、ロールはアクセス許可のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="f2a34-125">As you learned in the previous unit, a role is a collection of permissions.</span></span> <span data-ttu-id="f2a34-126">Azure には、ロールの割り当てに使用できる組み込みロールが 70 種類以上あります。</span><span class="sxs-lookup"><span data-stu-id="f2a34-126">Azure has over 70 built-in roles that you can use in your role assignments.</span></span> <span data-ttu-id="f2a34-127">ロールを一覧表示するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="f2a34-127">Follow these steps to list the roles.</span></span>

- <span data-ttu-id="f2a34-128">[アクセス制御 (IAM)] ブレードの上部で、**[ロール]** をクリックして、すべての組み込みロールとカスタム ロールの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="f2a34-128">At the top of the Access control (IAM) blade, click **Roles** to see a list of all the built-in and custom roles.</span></span>

   ![ロールのオプション](../media-draft/4-roles-option.png)

   <span data-ttu-id="f2a34-130">各ロールに割り当てられているユーザーとグループの数を確認できます。</span><span class="sxs-lookup"><span data-stu-id="f2a34-130">You can see the number of users and groups that are assigned to each role.</span></span>

   ![ロールの一覧](../media-draft/4-roles-list.png)

## <a name="summary"></a><span data-ttu-id="f2a34-132">まとめ</span><span class="sxs-lookup"><span data-stu-id="f2a34-132">Summary</span></span>

<span data-ttu-id="f2a34-133">このユニットでは、Azure portal でご自分のロールの割り当てを一覧表示する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="f2a34-133">In this unit, you learned how to list the role assignments for yourself in the Azure portal.</span></span> <span data-ttu-id="f2a34-134">また、さまざまなスコープでロールの割り当てを一覧表示する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="f2a34-134">You also learned how to list the role assignments at different scopes.</span></span> <span data-ttu-id="f2a34-135">次のユニットでは、次の手順に進み、RBAC を使用してユーザーにアクセス許可を付与します。</span><span class="sxs-lookup"><span data-stu-id="f2a34-135">In the next unit, you take the next step and use RBAC to grant access to a user.</span></span>
