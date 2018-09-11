<span data-ttu-id="06b46-101">"First Up Consultants" では、監査とトラブルシューティングの目的で、ロール ベースのアクセス制御 (RBAC) の変更内容を四半期ごとに確認します。</span><span class="sxs-lookup"><span data-stu-id="06b46-101">First Up Consultants reviews role-based access control (RBAC) changes quarterly for auditing and troubleshooting purposes.</span></span> <span data-ttu-id="06b46-102">変更は [Azure アクティビティ ログ](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)に記録されます。</span><span class="sxs-lookup"><span data-stu-id="06b46-102">You know that changes get logged in [Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span></span> <span data-ttu-id="06b46-103">マネージャーから、先月のロールの割り当てとカスタム ロールの変更に関するレポートを生成できるかどうかを確認されたとします。</span><span class="sxs-lookup"><span data-stu-id="06b46-103">Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.</span></span>

## <a name="view-activity-logs"></a><span data-ttu-id="06b46-104">アクティビティ ログの表示</span><span class="sxs-lookup"><span data-stu-id="06b46-104">View activity logs</span></span>

<span data-ttu-id="06b46-105">作業を開始する最も簡単な方法は、Azure portal でアクティビティ ログを表示することです。</span><span class="sxs-lookup"><span data-stu-id="06b46-105">The easiest way to get started is to view the activity logs with the Azure portal.</span></span>

1. <span data-ttu-id="06b46-106">**[すべてのサービス]** をクリックして、**[アクティビティ ログ]** を見つけます。</span><span class="sxs-lookup"><span data-stu-id="06b46-106">Click **All services** and then find **Activity log**.</span></span>

    ![ポータルを使用したアクティビティ ログ](../media-draft/7-all-services-activity-log.png)

1. <span data-ttu-id="06b46-108">**[アクティビティ ログ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="06b46-108">Click **Activity log**.</span></span>

    ![ポータルを使用したアクティビティ ログ](../media-draft/7-activity-log-portal.png)

1. <span data-ttu-id="06b46-110">**[期間]** フィルターに **[先月]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="06b46-110">Set the **Timespan** filter to **Last month**.</span></span>

1. <span data-ttu-id="06b46-111">**[イベント カテゴリ]** フィルターに **[管理]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="06b46-111">Set the **Event category** filter to **Administrative**.</span></span>

1. <span data-ttu-id="06b46-112">**[操作]** フィルターに、「**role**」と入力して一覧をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="06b46-112">In the **Operation** filter, type **role** to filter the list.</span></span>

1. <span data-ttu-id="06b46-113">次の RBAC 操作を選択します。</span><span class="sxs-lookup"><span data-stu-id="06b46-113">Select the following RBAC operations:</span></span>

    - <span data-ttu-id="06b46-114">ロール割り当ての作成 (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="06b46-114">Create role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="06b46-115">ロール割り当ての削除 (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="06b46-115">Delete role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="06b46-116">カスタムのロールの定義を作成または更新 (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="06b46-116">Create or update custom role definition (roleDefinitions)</span></span>
    - <span data-ttu-id="06b46-117">カスタムのロールの定義を削除 (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="06b46-117">Delete custom role definition (roleDefinitions)</span></span>

    ![[操作] フィルター](../media-draft/7-operation-filter.png)

1. <span data-ttu-id="06b46-119">**[適用]** をクリックしてフィルターを適用します。</span><span class="sxs-lookup"><span data-stu-id="06b46-119">Click **Apply** to apply your filters.</span></span>

    <span data-ttu-id="06b46-120">先月のロール割り当てとロールの定義の操作がすべて表示されます。</span><span class="sxs-lookup"><span data-stu-id="06b46-120">You'll see all the role assignment and role definition operations for the last month.</span></span> <span data-ttu-id="06b46-121">また、アクティビティ ログを CSV ファイルとしてダウンロードするためのリンクも含まれています。</span><span class="sxs-lookup"><span data-stu-id="06b46-121">It also includes a link to download the activity log as a CSV file.</span></span>

    ![RBAC のアクティビティ ログ](../media-draft/7-activity-log-portal-filter.png)

## <a name="end-lab"></a><span data-ttu-id="06b46-123">ラボの終了</span><span class="sxs-lookup"><span data-stu-id="06b46-123">End lab</span></span>

1. <span data-ttu-id="06b46-124">ラボを終了するには、このウィンドウの右上隅にあるハンバーガー メニューをクリックして、**[終了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="06b46-124">To end the lab, click the hamburger menu in the upper-right corner of this window and then click **End**.</span></span>

1. <span data-ttu-id="06b46-125">**[Yes, end my lab]\(はい、ラボを終了します\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="06b46-125">Click **Yes, end my lab**.</span></span>

## <a name="summary"></a><span data-ttu-id="06b46-126">まとめ</span><span class="sxs-lookup"><span data-stu-id="06b46-126">Summary</span></span>

<span data-ttu-id="06b46-127">このユニットでは、Azure アクティビティ ログを使用して、ポータルで RBAC の変更を一覧表示し、シンプルなレポートを生成する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="06b46-127">In this unit, you learned how to use Azure Activity Log to list RBAC changes in the portal and generate a simple report.</span></span>
