これまで Azure portal の **[アクセス制御 (IAM)]** ブレードを使用することで十分に対応できていましたが、毎日複数のアクセス許可要求を受け取るようになってきました。 アクセス管理タスクをこなすため、あなたは PowerShell を使用して一部の手順を自動化することにしました。

## <a name="open-cloud-shell-powershell"></a>Cloud Shell PowerShell を開く

1. Azure portal に **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com** としてサインインしていることを確認します。 ユーザー名とパスワードは、このウィンドウの上部にある **[リソース]** タブで見つかります。

1. ポータルの上部で、**[Cloud Shell]** をクリックして、[Cloud Shell] ウィンドウを開きます。

    ![[Cloud Shell] ボタン](../media-draft/6-cloud-shell-button.png)

1. 左上の [Cloud Shell] ウィンドウで、Cloud Shell が **[PowerShell]** に設定されていることを確認します。 **[Bash]** に設定されている場合は、**[PowerShell]** に変更します。

    読み込みにしばらく時間がかかる場合があります。 完了すると、次のようになります。

    ![Cloud Shell PowerShell](../media-draft/6-cloud-shell-powershell.png)

## <a name="grant-access"></a>アクセス権を付与する

Azure PowerShell を使用してユーザーにアクセス権を付与するには、[New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) コマンドを使用します。 セキュリティ プリンシパル、ロールの定義、およびスコープを指定する必要があります。

次の手順に従って、Virtual Machine Contributor ロールをリソース グループ スコープで **LabUser-_XXXXXXX_** ユーザーに割り当てます。

1. このウィンドウの上部の **[リソース]** タブで、**Grant access PowerShell** コマンドをコピーします。

1. コマンドを PowerShell ウィンドウに貼り付け、Enter キーを押して実行します。 コマンドと出力の例を次に示します。

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

    出力には、Virtual Machine Contributor ロールが FirstUpConsultantsRG1-_XXXXXXX_ スコープで LabUser-_XXXXXXX_ に割り当てられたことが示されます。

## <a name="list-access"></a>アクセス権を一覧表示する

リソース グループのアクセス権を確認するには、[Get-AzureRmRoleAssignment](/powershell/module/azurerm.resources/get-azurermroleassignment) コマンドを使用して、ロールの割り当てを一覧表示します。

次の手順に従って、リソース グループ スコープで **LabUser-XXXXXXX** ユーザーに対するすべてのロールの割り当てを一覧表示します。

1. このウィンドウの上部の **[リソース]** タブで、**List access PowerShell** コマンドをコピーします。

1. コマンドを PowerShell ウィンドウに貼り付け、Enter キーを押して実行します。 コマンドと出力の例を次に示します。

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

    出力には、Virtual Machine Contributor ロールが FirstUpConsultantsRG1-_XXXXXXX_ スコープで LabUser-_XXXXXXX_ に割り当てられたことが示されます。

    Azure portal でリソース グループの **[アクセス制御 (IAM)]** ブレードを更新すると、ロールの割り当てはこのようになります。

    ![リソース グループ スコープでのユーザーのロールの割り当て](../media-draft/6-cloud-shell-access-control.png)

## <a name="remove-access"></a>アクセス権を削除する

ユーザー、グループ、アプリケーションのアクセス権を削除するには、[Remove-AzureRmRoleAssignment](/powershell/module/azurerm.resources/remove-azurermroleassignment) を使用して、ロールの割り当てを削除します。

次の手順に従ってリソース グループ スコープでの **LabUser-_XXXXXX_** ユーザーへの Virtual Machine Contributor ロールの割り当てを削除します。

1. このウィンドウの上部の **[リソース]** タブで、**Remove access PowerShell** コマンドをコピーします。

1. コマンドを PowerShell ウィンドウに貼り付け、Enter キーを押して実行します。 コマンドの例を次に示します。

    ```Example
    PS Azure:\> Remove-AzureRmRoleAssignment -SignInName LabUser-XXXXXXX@xxxxxxxxxxxx.onmicrosoft.com `
      -RoleDefinitionName "Virtual Machine Contributor" `
      -ResourceGroupName "FirstUpConsultantsRG1-XXXXXXX"
    ```

1. PowerShell ウィンドウで閉じる (**[X]**) ボタンをクリックして、ウィンドウを閉じます。

    ![Cloud Shell の閉じるボタン](../media-draft/6-cloud-shell-close.png)


## <a name="summary"></a>まとめ

このユニットでは、Azure PowerShell を使用してリソース グループ内に仮想マシンを作成して管理するために、ユーザーにアクセス権を付与する方法について学習しました。 次のユニットでは、時間の経過と共に、RBAC の変更を表示する方法について学習します。
