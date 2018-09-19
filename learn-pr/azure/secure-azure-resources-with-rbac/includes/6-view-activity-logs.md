"First Up Consultants" では、監査とトラブルシューティングの目的で、ロール ベースのアクセス制御 (RBAC) の変更内容を四半期ごとに確認します。 変更は [Azure アクティビティ ログ](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)に記録されます。 マネージャーから、先月のロールの割り当てとカスタム ロールの変更に関するレポートを生成できるかどうかを確認されたとします。

## <a name="view-activity-logs"></a>アクティビティ ログの表示

作業を開始する最も簡単な方法は、Azure portal でアクティビティ ログを表示することです。

1. **[すべてのサービス]** をクリックして、**[アクティビティ ログ]** を見つけます。

    ![ポータルを使用したアクティビティ ログ](../media/6-all-services-activity-log.png)

1. **[アクティビティ ログ]** をクリックしてアクティビティ ログを開きます。

    ![ポータルを使用したアクティビティ ログ](../media/6-activity-log-portal.png)

1. **[期間]** フィルターに **[先月]** を設定します。

1. **[イベント カテゴリ]** フィルターに **[管理]** を設定します。

1. **[操作]** フィルターに、「**role**」と入力して一覧をフィルター処理します。

1. 次の RBAC 操作を選択します。

    - ロール割り当ての作成 (roleAssignments)
    - ロール割り当ての削除 (roleAssignments)
    - カスタムのロールの定義を作成または更新 (roleDefinitions)
    - カスタムのロールの定義を削除 (roleDefinitions)

    ![[操作] フィルター](../media/6-operation-filter.png)

1. **[適用]** をクリックしてフィルターを適用します。

    先月のロール割り当てとロールの定義の操作がすべて表示されます。 また、アクティビティ ログを CSV ファイルとしてダウンロードするためのリンクも含まれています。

    ![RBAC のアクティビティ ログ](../media/6-activity-log-portal-filter.png)

## <a name="end-lab"></a>ラボの終了

1. ラボを終了するには、このウィンドウの右上隅にあるハンバーガー メニューをクリックして、**[終了]** をクリックします。

1. **[Yes, end my lab]\(はい、ラボを終了します\)** をクリックします。

## <a name="summary"></a>まとめ

このユニットでは、Azure アクティビティ ログを使用して、ポータルで RBAC の変更を一覧表示し、シンプルなレポートを生成する方法について学習しました。