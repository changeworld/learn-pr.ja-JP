<span data-ttu-id="c3ebc-101">これまで Azure portal の **[アクセス制御 (IAM)]** ブレードを使用することで十分に対応できていましたが、毎日複数のアクセス許可要求を受け取るようになってきました。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-101">Using the **Access control (IAM)** blade in the Azure portal has been working fine, but you are getting several permission requests each day.</span></span> <span data-ttu-id="c3ebc-102">アクセス管理タスクをこなすため、あなたは PowerShell を使用して一部の手順を自動化することにしました。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-102">To keep up with the access management tasks, you decide to use PowerShell to help automate some of the steps.</span></span>

## <a name="open-cloud-shell-powershell"></a><span data-ttu-id="c3ebc-103">Cloud Shell PowerShell を開く</span><span class="sxs-lookup"><span data-stu-id="c3ebc-103">Open Cloud Shell PowerShell</span></span>

1. <span data-ttu-id="c3ebc-104">Azure portal に **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com** としてサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-104">Make sure you are still signed in to the Azure portal as **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**.</span></span> <span data-ttu-id="c3ebc-105">ユーザー名とパスワードは、このウィンドウの上部にある **[リソース]** タブで見つかります。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-105">You can find the username and password on the **Resources** tab at the top of this window.</span></span>

1. <span data-ttu-id="c3ebc-106">ポータルの上部で、**[Cloud Shell]** をクリックして、[Cloud Shell] ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-106">At the top of the portal, click **Cloud Shell** to open the Cloud Shell pane.</span></span>

    ![[Cloud Shell] ボタン](../media-draft/6-cloud-shell-button.png)

1. <span data-ttu-id="c3ebc-108">左上の [Cloud Shell] ウィンドウで、Cloud Shell が **[PowerShell]** に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-108">In the upper left of the Cloud Shell pane, make sure it is set to **PowerShell**.</span></span> <span data-ttu-id="c3ebc-109">**[Bash]** に設定されている場合は、**[PowerShell]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-109">If it is set to **Bash**, change it to **PowerShell**.</span></span>

    <span data-ttu-id="c3ebc-110">読み込みにしばらく時間がかかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-110">It might take a few moments to load.</span></span> <span data-ttu-id="c3ebc-111">完了すると、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-111">When finished, it will look similar to the following:</span></span>

    ![Cloud Shell PowerShell](../media-draft/6-cloud-shell-powershell.png)

## <a name="grant-access"></a><span data-ttu-id="c3ebc-113">アクセス権を付与する</span><span class="sxs-lookup"><span data-stu-id="c3ebc-113">Grant access</span></span>

<span data-ttu-id="c3ebc-114">Azure PowerShell を使用してユーザーにアクセス権を付与するには、[New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-114">To grant access to a user using Azure PowerShell, you use the [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) command.</span></span> <span data-ttu-id="c3ebc-115">セキュリティ プリンシパル、ロールの定義、およびスコープを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-115">You must specify the security principal, role definition, and scope.</span></span>

<span data-ttu-id="c3ebc-116">次の手順に従って、Virtual Machine Contributor ロールをリソース グループ スコープで **LabUser-_XXXXXXX_** ユーザーに割り当てます。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-116">Follow these steps to assign the Virtual Machine Contributor role to the **LabUser-_XXXXXXX_** user at the resource group scope.</span></span>

1. <span data-ttu-id="c3ebc-117">このウィンドウの上部の **[リソース]** タブで、**Grant access PowerShell** コマンドをコピーします。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-117">On the **Resources** tab at the top of this window, copy the **Grant access PowerShell** command.</span></span>

1. <span data-ttu-id="c3ebc-118">コマンドを PowerShell ウィンドウに貼り付け、Enter キーを押して実行します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-118">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="c3ebc-119">コマンドと出力の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-119">The following shows an example command and the output:</span></span>

    ```Example
    PS Azure:\> New-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    <span data-ttu-id="c3ebc-120">出力には、Virtual Machine Contributor ロールが FirstUpConsultantsRG1-_XXXXXXX_ スコープで LabUser-_XXXXXXX_ に割り当てられたことが示されます。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-120">The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.</span></span>

## <a name="list-access"></a><span data-ttu-id="c3ebc-121">アクセス権を一覧表示する</span><span class="sxs-lookup"><span data-stu-id="c3ebc-121">List access</span></span>

<span data-ttu-id="c3ebc-122">リソース グループのアクセス権を確認するには、[Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) コマンドを使用して、ロールの割り当てを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-122">To verify the access for the resource group, you use the [Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) command to list the role assignments.</span></span>

<span data-ttu-id="c3ebc-123">次の手順に従って、リソース グループ スコープで **LabUser-XXXXXXX** ユーザーに対するすべてのロールの割り当てを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-123">Follow these steps to list all the role assignments for the **LabUser-XXXXXXX** user at the resource group scope.</span></span>

1. <span data-ttu-id="c3ebc-124">このウィンドウの上部の **[リソース]** タブで、**List access PowerShell** コマンドをコピーします。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-124">On the **Resources** tab at the top of this window, copy the **List access PowerShell** command.</span></span>

1. <span data-ttu-id="c3ebc-125">コマンドを PowerShell ウィンドウに貼り付け、Enter キーを押して実行します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-125">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="c3ebc-126">コマンドと出力の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-126">The following shows an example command and the output.</span></span>

    ```Example
    PS Azure:\> Get-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"

    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/FirstUpConsultantsRG1-XXXXXXX
    DisplayName        : LabUser-XXXXXXX
    SignInName         : LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com 
    RoleDefinitionName : Virtual Machine Contributor
    RoleDefinitionId   : 9980e02c-c2be-4d73-94e8-173b1dc7cf3c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : User
    CanDelegate        : False
    ```

    <span data-ttu-id="c3ebc-127">出力には、Virtual Machine Contributor ロールが FirstUpConsultantsRG1-_XXXXXXX_ スコープで LabUser-_XXXXXXX_ に割り当てられたことが示されます。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-127">The output shows that the Virtual Machine Contributor role has been assigned to LabUser-_XXXXXXX_ at the FirstUpConsultantsRG1-_XXXXXXX_ scope.</span></span>

    <span data-ttu-id="c3ebc-128">Azure portal でリソース グループの **[アクセス制御 (IAM)]** ブレードを更新すると、ロールの割り当てはこのようになります。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-128">If you refresh the **Access control (IAM)** blade for the resource group in the Azure portal, this is how the role assignment looks:</span></span>

    ![リソース グループ スコープでのユーザーのロールの割り当て](../media-draft/6-cloud-shell-access-control.png)

## <a name="remove-access"></a><span data-ttu-id="c3ebc-130">アクセス権を削除する</span><span class="sxs-lookup"><span data-stu-id="c3ebc-130">Remove access</span></span>

<span data-ttu-id="c3ebc-131">ユーザー、グループ、アプリケーションのアクセス権を削除するには、[Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) を使用して、ロールの割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-131">To remove access for users, groups, and applications, you use [Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) to remove a role assignment.</span></span>

<span data-ttu-id="c3ebc-132">次の手順に従ってリソース グループ スコープでの **LabUser-_XXXXXX_** ユーザーへの Virtual Machine Contributor ロールの割り当てを削除します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-132">Follow these steps to remove the Virtual Machine Contributor role assignment for the **LabUser-_XXXXXX_** user at the resource group scope.</span></span>

1. <span data-ttu-id="c3ebc-133">このウィンドウの上部の **[リソース]** タブで、**Remove access PowerShell** コマンドをコピーします。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-133">On the **Resources** tab at the top of this window, copy the **Remove access PowerShell** command.</span></span>

1. <span data-ttu-id="c3ebc-134">コマンドを PowerShell ウィンドウに貼り付け、Enter キーを押して実行します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-134">Paste the command into the PowerShell pane and press the Enter key to run it.</span></span> <span data-ttu-id="c3ebc-135">コマンドの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-135">The following shows an example command.</span></span>

    ```Example
    PS Azure:\> Remove-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"
    ```

1. <span data-ttu-id="c3ebc-136">PowerShell ウィンドウで閉じる (**[X]**) ボタンをクリックして、ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-136">In the PowerShell pane, click the close (**X**) button to close the pane.</span></span>

    ![Cloud Shell の閉じるボタン](../media-draft/6-cloud-shell-close.png)


## <a name="summary"></a><span data-ttu-id="c3ebc-138">まとめ</span><span class="sxs-lookup"><span data-stu-id="c3ebc-138">Summary</span></span>

<span data-ttu-id="c3ebc-139">このユニットでは、Azure PowerShell を使用してリソース グループ内に仮想マシンを作成して管理するために、ユーザーにアクセス権を付与する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-139">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using Azure PowerShell.</span></span> <span data-ttu-id="c3ebc-140">次のユニットでは、時間の経過と共に、RBAC の変更を表示する方法について学習します。</span><span class="sxs-lookup"><span data-stu-id="c3ebc-140">In the next unit, you learn how to view the RBAC changes over time.</span></span>
