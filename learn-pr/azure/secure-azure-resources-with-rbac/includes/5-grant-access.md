<span data-ttu-id="dac38-101">Alain は、First Up Consultants でのあなたの同僚です。彼は自分が取り組んでいるプロジェクトに仮想マシンを作成して管理する能力を必要としています。</span><span class="sxs-lookup"><span data-stu-id="dac38-101">A co-worker named Alain at First Up Consultants needs the ability to create and manage virtual machines for a project he is working on.</span></span> <span data-ttu-id="dac38-102">あなたは上司から、この要求に対処するように指示されています。</span><span class="sxs-lookup"><span data-stu-id="dac38-102">Your manager has asked that you handle this request.</span></span> <span data-ttu-id="dac38-103">あなたは、ユーザーが作業を完了するために必要な最小限の特権を付与するベスト プラクティスを使用して、Alain にリソース グループの Virtual Machine Contributor ロールを割り当てることにしました。</span><span class="sxs-lookup"><span data-stu-id="dac38-103">Using the best practice to grant users the least privileges to get their work done, you decide to assign Alain the Virtual Machine Contributor role for a resource group.</span></span>

## <a name="sign-in-to-the-azure-portal"></a><span data-ttu-id="dac38-104">Azure portal にサインインする</span><span class="sxs-lookup"><span data-stu-id="dac38-104">Sign in to the Azure portal</span></span>

- <span data-ttu-id="dac38-105">Azure portal に **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com** としてサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="dac38-105">Make sure you are still signed in to the Azure portal as **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span></span> <span data-ttu-id="dac38-106">ユーザー名とパスワードは、このウィンドウの上部にある **[リソース]** タブで見つかります。</span><span class="sxs-lookup"><span data-stu-id="dac38-106">You can find the username and password on the **Resources** tab at the top of this window.</span></span>

## <a name="grant-access"></a><span data-ttu-id="dac38-107">アクセス権を付与する</span><span class="sxs-lookup"><span data-stu-id="dac38-107">Grant access</span></span>

<span data-ttu-id="dac38-108">次の手順に従って、Virtual Machine Contributor ロールをリソース グループ スコープでユーザーに割り当てます。</span><span class="sxs-lookup"><span data-stu-id="dac38-108">Follow these steps to assign the Virtual Machine Contributor role to a user at the resource group scope.</span></span>

1. <span data-ttu-id="dac38-109">ナビゲーション リストで、**[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dac38-109">In the navigation list, click **Resource groups**.</span></span>

1. <span data-ttu-id="dac38-110">**FirstUpConsultantsRG1-_XXXXXXX_** リソース グループを見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="dac38-110">Find and click the **FirstUpConsultantsRG1-_XXXXXXX_** resource group.</span></span>

1. <span data-ttu-id="dac38-111">**[アクセス制御 (IAM)]** をクリックすると、ロールの割り当ての現在の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="dac38-111">Click **Access control (IAM)** to see the current list of role assignments.</span></span>

   ![アクセス制御 - リソース グループのロールの割り当て](../media/5-resource-group-role-assignment.png)

1. <span data-ttu-id="dac38-113">上部で、**[追加]** をクリックして **[アクセス許可の追加]** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="dac38-113">At the top, click **Add** to open the **Add permissions** pane.</span></span>

   ![[アクセス許可の追加] ウィンドウ](../media/5-add-permissions.png)

1. <span data-ttu-id="dac38-115">**[ロール]** ドロップダウン リストで、**[Virtual Machine Contributor]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dac38-115">In the **Role** drop-down list, select **Virtual Machine Contributor**.</span></span>

1. <span data-ttu-id="dac38-116">**[選択]** リストで、**LabUser-_XXXXXXX_** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dac38-116">In the **Select** list, select **LabUser-_XXXXXXX_**.</span></span>

   ![完成した [アクセス許可の追加] ウィンドウ](../media/5-add-permissions-save.png)

1. <span data-ttu-id="dac38-118">**[保存]** をクリックして、ロールの割り当てを作成します。</span><span class="sxs-lookup"><span data-stu-id="dac38-118">Click **Save** to create the role assignment.</span></span>

   <span data-ttu-id="dac38-119">しばらくすると、**LabUser-_XXXXXXX_** ユーザーに、Virtual Machine Contributor ロールが **FirstUpConsultantsRG1-_XXXXXXX_** リソース グループ スコープで割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="dac38-119">After a few moments, the **LabUser-_XXXXXXX_** user is assigned the Virtual Machine Contributor role at the **FirstUpConsultantsRG1-_XXXXXXX_** resource group scope.</span></span> <span data-ttu-id="dac38-120">これでそのユーザーは、このリソース グループ内だけで仮想マシンの作成と管理ができるようになりました。</span><span class="sxs-lookup"><span data-stu-id="dac38-120">The user can now create and manage virtual machines just within this resource group.</span></span>

   ![Virtual Machine Contributor ロールの割り当て](../media/5-vm-contributor-assignment.png)

## <a name="remove-access"></a><span data-ttu-id="dac38-122">アクセス権を削除する</span><span class="sxs-lookup"><span data-stu-id="dac38-122">Remove access</span></span>

<span data-ttu-id="dac38-123">RBAC では、アクセス権を削除するにはロールの割り当てを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dac38-123">In RBAC, to remove access, you remove a role assignment.</span></span>

1. <span data-ttu-id="dac38-124">ロールの割り当ての一覧で、Virtual Machine Contributor ロールを持つ **LabUser-_XXXXXXX_** ユーザーを選択します。</span><span class="sxs-lookup"><span data-stu-id="dac38-124">In the list of role assignments, select the **LabUser-_XXXXXXX_** user with the Virtual Machine Contributor role.</span></span>

1. <span data-ttu-id="dac38-125">**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dac38-125">Click **Remove**.</span></span>

   ![ロールの割り当ての削除メッセージ](../media/5-remove-role-assignment.png)

1. <span data-ttu-id="dac38-127">**ロールの割り当ての削除**メッセージが表示されたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dac38-127">In the **Remove role assignments** message that appears, click **Yes**.</span></span>

## <a name="summary"></a><span data-ttu-id="dac38-128">まとめ</span><span class="sxs-lookup"><span data-stu-id="dac38-128">Summary</span></span>

<span data-ttu-id="dac38-129">このユニットでは、Azure portal を使用してリソース グループ内に仮想マシンを作成して管理するために、ユーザーにアクセス権を付与する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="dac38-129">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using the Azure portal.</span></span> <span data-ttu-id="dac38-130">次のユニットでは、時間の経過と共に、RBAC の変更を表示する方法について学習します。</span><span class="sxs-lookup"><span data-stu-id="dac38-130">In the next unit, you learn how to view the RBAC changes over time.</span></span>
