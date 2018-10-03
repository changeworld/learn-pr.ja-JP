> [!NOTE]
> <span data-ttu-id="ffc01-101">ラボの起動後、必要なユーザー名とパスワードは、手順の横にある **[リソース]** タブに配置されます。</span><span class="sxs-lookup"><span data-stu-id="ffc01-101">After launching the lab, the username and password you need is located on the **Resources** tab next to the instructions.</span></span>

<span data-ttu-id="ffc01-102">"First Up Consultants" で、マーケティング チームに対してリソース グループへのアクセス許可を付与しました。</span><span class="sxs-lookup"><span data-stu-id="ffc01-102">At First Up Consultants, you've been granted access to a resource group for the marketing team.</span></span> <span data-ttu-id="ffc01-103">Azure portal をよく理解し、現在割り当てられているロールを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ffc01-103">You want to familiarize yourself with the Azure portal and see what roles are currently assigned.</span></span>

## <a name="list-role-assignments-for-yourself"></a><span data-ttu-id="ffc01-104">自分のロールの割り当ての一覧表示</span><span class="sxs-lookup"><span data-stu-id="ffc01-104">List role assignments for yourself</span></span>

<span data-ttu-id="ffc01-105">現在、ご自分に割り当てられているロールを確認するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="ffc01-105">Follow these steps to see what roles are currently assigned to you.</span></span>

1. <span data-ttu-id="ffc01-106">Azure portal の右上隅で、ご自分のユーザー名をクリックしてメニューを開きます。</span><span class="sxs-lookup"><span data-stu-id="ffc01-106">In the upper-right corner of the Azure portal, click your user name to open the menu.</span></span>

    ![[アクセス許可] メニュー](../media/4-my-permissions-menu.png)

1. <span data-ttu-id="ffc01-108">**[アクセス許可]** メニューをクリックして、[アクセス許可] ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="ffc01-108">Click **My permissions** to open the My permissions pane.</span></span>

    ![[アクセス許可] ウィンドウ](../media/4-my-permissions-pane.png)

    <span data-ttu-id="ffc01-110">[アクセス許可] ウィンドウでは、割り当てたロールの一覧と、そのスコープを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ffc01-110">On the My permissions pane, you can see a list of the roles that you have been assigned and the scope.</span></span> <span data-ttu-id="ffc01-111">ご自分の一覧の外観は異なるものとなります。</span><span class="sxs-lookup"><span data-stu-id="ffc01-111">Your list will look different.</span></span>

## <a name="list-role-assignments-for-a-resource-group"></a><span data-ttu-id="ffc01-112">リソース グループに対するロールの割り当ての一覧表示</span><span class="sxs-lookup"><span data-stu-id="ffc01-112">List role assignments for a resource group</span></span>

<span data-ttu-id="ffc01-113">リソース グループ スコープで割り当てられているロールを表示するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="ffc01-113">Follow these steps to see what roles are assigned at the resource group scope.</span></span>

1. <span data-ttu-id="ffc01-114">ナビゲーション リストで、**[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ffc01-114">In the navigation list, click **Resource groups**.</span></span>

   ![リソース グループ](../media/4-resource-groups.png)

1. <span data-ttu-id="ffc01-116">**FirstUpConsultantsRG1-_XXXXXXX_** という名前のリソース グループを見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="ffc01-116">Find and click the resource group named **FirstUpConsultantsRG1-_XXXXXXX_**.</span></span>

1. <span data-ttu-id="ffc01-117">**[アクセス制御 (IAM)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ffc01-117">Click **Access control (IAM)**.</span></span>

   ![リソース グループに対するアクセス制御](../media/4-resource-group-access-control.png)

    <span data-ttu-id="ffc01-119">このリソース グループにアクセスできるユーザーを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ffc01-119">You can see who has access to this resource group.</span></span> <span data-ttu-id="ffc01-120">スコープが **[このリソース]** のロールもあれば、親スコープからスコープを **[(継承)]** しているロールもあることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="ffc01-120">Notice that some roles are scoped to **This resource** while others are **(Inherited)** from a parent scope.</span></span>

   ![アクセス制御 - リソース グループのロールの割り当て](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a><span data-ttu-id="ffc01-122">ロールの一覧表示</span><span class="sxs-lookup"><span data-stu-id="ffc01-122">List roles</span></span>

<span data-ttu-id="ffc01-123">前のユニットで学習したように、ロールはアクセス許可のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="ffc01-123">As you learned in the previous unit, a role is a collection of permissions.</span></span> <span data-ttu-id="ffc01-124">Azure には、ロールの割り当てに使用できる組み込みロールが 70 種類以上あります。</span><span class="sxs-lookup"><span data-stu-id="ffc01-124">Azure has over 70 built-in roles that you can use in your role assignments.</span></span> <span data-ttu-id="ffc01-125">ロールを一覧表示するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="ffc01-125">Follow this step to list the roles.</span></span>

- <span data-ttu-id="ffc01-126">ウィンドウの上部で、**[ロール]** をクリックして、すべての組み込みロールとカスタム ロールの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="ffc01-126">At the top of the pane, click **Roles** to see a list of all the built-in and custom roles.</span></span>

   <span data-ttu-id="ffc01-127">各ロールに割り当てられているユーザーとグループの数を確認できます。</span><span class="sxs-lookup"><span data-stu-id="ffc01-127">You can see the number of users and groups that are assigned to each role.</span></span>

   ![ロールの一覧](../media/4-roles-list.png)

<span data-ttu-id="ffc01-129">このユニットでは、Azure portal でご自分のロールの割り当てを一覧表示する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="ffc01-129">In this unit, you learned how to list the role assignments for yourself in the Azure portal.</span></span> <span data-ttu-id="ffc01-130">また、リソース グループに対するロールの割り当てを一覧表示する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="ffc01-130">You also learned how to list the role assignments for a resource group.</span></span>