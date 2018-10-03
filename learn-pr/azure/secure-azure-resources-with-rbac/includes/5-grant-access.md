<span data-ttu-id="3e05b-101">Alain は、First Up Consultants でのあなたの同僚です。彼は自分が取り組んでいるプロジェクトに仮想マシンを作成して管理する能力を必要としています。</span><span class="sxs-lookup"><span data-stu-id="3e05b-101">A co-worker named Alain at First Up Consultants needs the ability to create and manage virtual machines for a project he is working on.</span></span> <span data-ttu-id="3e05b-102">あなたは上司から、この要求に対処するように指示されています。</span><span class="sxs-lookup"><span data-stu-id="3e05b-102">Your manager has asked that you handle this request.</span></span> <span data-ttu-id="3e05b-103">あなたは、ユーザーが作業を完了するために必要な最小限の特権を付与するベスト プラクティスを使用して、Alain にリソース グループの Virtual Machine Contributor ロールを割り当てることにしました。</span><span class="sxs-lookup"><span data-stu-id="3e05b-103">Using the best practice to grant users the least privileges to get their work done, you decide to assign Alain the Virtual Machine Contributor role for a resource group.</span></span>

## <a name="grant-access"></a><span data-ttu-id="3e05b-104">アクセス権を付与する</span><span class="sxs-lookup"><span data-stu-id="3e05b-104">Grant access</span></span>

<span data-ttu-id="3e05b-105">次の手順に従って、Virtual Machine Contributor ロールをリソース グループ スコープでユーザーに割り当てます。</span><span class="sxs-lookup"><span data-stu-id="3e05b-105">Follow these steps to assign the Virtual Machine Contributor role to a user at the resource group scope.</span></span>

1. <span data-ttu-id="3e05b-106">ナビゲーション リストで、**[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e05b-106">In the navigation list, click **Resource groups**.</span></span>

1. <span data-ttu-id="3e05b-107">**FirstUpConsultantsRG1-_XXXXXXX_** リソース グループを見つけてクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e05b-107">Find and click the **FirstUpConsultantsRG1-_XXXXXXX_** resource group.</span></span>

1. <span data-ttu-id="3e05b-108">**[アクセス制御 (IAM)]** をクリックすると、ロールの割り当ての現在の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3e05b-108">Click **Access control (IAM)** to see the current list of role assignments.</span></span>

   ![アクセス制御 - リソース グループのロールの割り当て](../media/5-resource-group-role-assignment.png)

1. <span data-ttu-id="3e05b-110">上部で、**[追加]** をクリックして **[アクセス許可の追加]** ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="3e05b-110">At the top, click **Add** to open the **Add permissions** pane.</span></span>

   ![[アクセス許可の追加] ウィンドウ](../media/5-add-permissions.png)

1. <span data-ttu-id="3e05b-112">**[ロール]** ドロップダウン リストで、**[Virtual Machine Contributor]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3e05b-112">In the **Role** drop-down list, select **Virtual Machine Contributor**.</span></span>

1. <span data-ttu-id="3e05b-113">**[選択]** リストで、**LabUser-_XXXXXXX_** を選択します。</span><span class="sxs-lookup"><span data-stu-id="3e05b-113">In the **Select** list, select **LabUser-_XXXXXXX_**.</span></span>

    <span data-ttu-id="3e05b-114">ユーザー名は、指示の横にある **[リソース]** タブで見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="3e05b-114">You can find the username on the **Resources** tab next to the instructions.</span></span>

   ![完成した [アクセス許可の追加] ウィンドウ](../media/5-add-permissions-save.png)

1. <span data-ttu-id="3e05b-116">**[保存]** をクリックして、ロールの割り当てを作成します。</span><span class="sxs-lookup"><span data-stu-id="3e05b-116">Click **Save** to create the role assignment.</span></span>

   <span data-ttu-id="3e05b-117">しばらくすると、**LabUser-_XXXXXXX_** ユーザーに、Virtual Machine Contributor ロールが **FirstUpConsultantsRG1-_XXXXXXX_** リソース グループ スコープで割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="3e05b-117">After a few moments, the **LabUser-_XXXXXXX_** user is assigned the Virtual Machine Contributor role at the **FirstUpConsultantsRG1-_XXXXXXX_** resource group scope.</span></span> <span data-ttu-id="3e05b-118">これでそのユーザーは、このリソース グループ内だけで仮想マシンの作成と管理ができるようになりました。</span><span class="sxs-lookup"><span data-stu-id="3e05b-118">The user can now create and manage virtual machines just within this resource group.</span></span>

   ![Virtual Machine Contributor ロールの割り当て](../media/5-vm-contributor-assignment.png)

## <a name="remove-access"></a><span data-ttu-id="3e05b-120">アクセス権を削除する</span><span class="sxs-lookup"><span data-stu-id="3e05b-120">Remove access</span></span>

<span data-ttu-id="3e05b-121">RBAC では、アクセス権を削除するにはロールの割り当てを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3e05b-121">In RBAC, to remove access, you remove a role assignment.</span></span>

1. <span data-ttu-id="3e05b-122">ロールの割り当ての一覧で、Virtual Machine Contributor ロールを持つ **LabUser-_XXXXXXX_** ユーザーを選択します。</span><span class="sxs-lookup"><span data-stu-id="3e05b-122">In the list of role assignments, select the **LabUser-_XXXXXXX_** user with the Virtual Machine Contributor role.</span></span>

1. <span data-ttu-id="3e05b-123">**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e05b-123">Click **Remove**.</span></span>

   ![ロールの割り当ての削除メッセージ](../media/5-remove-role-assignment.png)

1. <span data-ttu-id="3e05b-125">**ロールの割り当ての削除**メッセージが表示されたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="3e05b-125">In the **Remove role assignments** message that appears, click **Yes**.</span></span>

<span data-ttu-id="3e05b-126">このユニットでは、Azure portal を使用してリソース グループ内に仮想マシンを作成して管理するために、ユーザーにアクセス権を付与する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="3e05b-126">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using the Azure portal.</span></span>