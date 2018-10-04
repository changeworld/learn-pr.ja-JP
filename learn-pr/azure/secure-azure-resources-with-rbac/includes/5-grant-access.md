Alain は、First Up Consultants でのあなたの同僚です。彼は自分が取り組んでいるプロジェクトに仮想マシンを作成して管理する能力を必要としています。 あなたは上司から、この要求に対処するように指示されています。 あなたは、ユーザーが作業を完了するために必要な最小限の特権を付与するベスト プラクティスを使用して、Alain にリソース グループの Virtual Machine Contributor ロールを割り当てることにしました。

## <a name="grant-access"></a>アクセス権を付与する

次の手順に従って、Virtual Machine Contributor ロールをリソース グループ スコープでユーザーに割り当てます。

1. ナビゲーション リストで、**[リソース グループ]** をクリックします。

1. **FirstUpConsultantsRG1-_XXXXXXX_** リソース グループを見つけてクリックします。

1. **[アクセス制御 (IAM)]** をクリックすると、ロールの割り当ての現在の一覧が表示されます。

   ![アクセス制御 - リソース グループのロールの割り当て](../media/5-resource-group-role-assignment.png)

1. 上部で、**[追加]** をクリックして **[アクセス許可の追加]** ウィンドウを開きます。

   ![[アクセス許可の追加] ウィンドウ](../media/5-add-permissions.png)

1. **[ロール]** ドロップダウン リストで、**[Virtual Machine Contributor]** を選択します。

1. **[選択]** リストで、**LabUser-_XXXXXXX_** を選択します。

    ユーザー名は、指示の横にある **[リソース]** タブで見つけることができます。

   ![完成した [アクセス許可の追加] ウィンドウ](../media/5-add-permissions-save.png)

1. **[保存]** をクリックして、ロールの割り当てを作成します。

   しばらくすると、**LabUser-_XXXXXXX_** ユーザーに、Virtual Machine Contributor ロールが **FirstUpConsultantsRG1-_XXXXXXX_** リソース グループ スコープで割り当てられます。 これでそのユーザーは、このリソース グループ内だけで仮想マシンの作成と管理ができるようになりました。

   ![Virtual Machine Contributor ロールの割り当て](../media/5-vm-contributor-assignment.png)

## <a name="remove-access"></a>アクセス権を削除する

RBAC では、アクセス権を削除するにはロールの割り当てを削除する必要があります。

1. ロールの割り当ての一覧で、Virtual Machine Contributor ロールを持つ **LabUser-_XXXXXXX_** ユーザーを選択します。

1. **[削除]** をクリックします。

   ![ロールの割り当ての削除メッセージ](../media/5-remove-role-assignment.png)

1. **ロールの割り当ての削除**メッセージが表示されたら、**[はい]** をクリックします。

このユニットでは、Azure portal を使用してリソース グループ内に仮想マシンを作成して管理するために、ユーザーにアクセス権を付与する方法について学習しました。