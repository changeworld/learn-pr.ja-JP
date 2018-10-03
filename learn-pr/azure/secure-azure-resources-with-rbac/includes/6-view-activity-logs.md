<span data-ttu-id="3994c-101">"First Up Consultants" では、監査とトラブルシューティングの目的で、ロール ベースのアクセス制御 (RBAC) の変更内容を四半期ごとに確認します。</span><span class="sxs-lookup"><span data-stu-id="3994c-101">First Up Consultants reviews role-based access control (RBAC) changes quarterly for auditing and troubleshooting purposes.</span></span> <span data-ttu-id="3994c-102">変更は [Azure アクティビティ ログ](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)に記録されます。</span><span class="sxs-lookup"><span data-stu-id="3994c-102">You know that changes get logged in [Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span></span> <span data-ttu-id="3994c-103">マネージャーから、先月のロールの割り当てとカスタム ロールの変更に関するレポートを生成できるかどうかを確認されたとします。</span><span class="sxs-lookup"><span data-stu-id="3994c-103">Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.</span></span>

## <a name="view-activity-logs"></a><span data-ttu-id="3994c-104">アクティビティ ログの表示</span><span class="sxs-lookup"><span data-stu-id="3994c-104">View activity logs</span></span>

<span data-ttu-id="3994c-105">作業を開始する最も簡単な方法は、Azure portal でアクティビティ ログを表示することです。</span><span class="sxs-lookup"><span data-stu-id="3994c-105">The easiest way to get started is to view the activity logs with the Azure portal.</span></span>

1. <span data-ttu-id="3994c-106">**[すべてのサービス]** をクリックして、**[アクティビティ ログ]** を見つけます。</span><span class="sxs-lookup"><span data-stu-id="3994c-106">Click **All services** and then find **Activity log**.</span></span>

    ![ポータルを使用したアクティビティ ログ](../media/6-all-services-activity-log.png)

1. <span data-ttu-id="3994c-108">**[アクティビティ ログ]** をクリックしてアクティビティ ログを開きます。</span><span class="sxs-lookup"><span data-stu-id="3994c-108">Click **Activity log** to open the activity log.</span></span>

    ![ポータルを使用したアクティビティ ログ](../media/6-activity-log-portal.png)

1. <span data-ttu-id="3994c-110">**[期間]** フィルターに **[先月]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="3994c-110">Set the **Timespan** filter to **Last month**.</span></span>

1. <span data-ttu-id="3994c-111">**[操作]** フィルターを追加し、「**role**」と入力して一覧をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="3994c-111">Add an **Operation** filter and type **role** to filter the list.</span></span>

1. <span data-ttu-id="3994c-112">次の RBAC 操作を選択します。</span><span class="sxs-lookup"><span data-stu-id="3994c-112">Select the following RBAC operations:</span></span>

    - <span data-ttu-id="3994c-113">ロール割り当ての作成 (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="3994c-113">Create role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="3994c-114">ロール割り当ての削除 (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="3994c-114">Delete role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="3994c-115">カスタムのロールの定義を作成または更新 (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="3994c-115">Create or update custom role definition (roleDefinitions)</span></span>
    - <span data-ttu-id="3994c-116">カスタムのロールの定義を削除 (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="3994c-116">Delete custom role definition (roleDefinitions)</span></span>

    ![[操作] フィルター](../media/6-operation-filter.png)

    <span data-ttu-id="3994c-118">数か月後、先月のロール割り当てとロールの定義の操作がすべて表示されます。</span><span class="sxs-lookup"><span data-stu-id="3994c-118">After a few moments, you'll see all the role assignment and role definition operations for the last month.</span></span> <span data-ttu-id="3994c-119">また、アクティビティ ログを CSV ファイルとしてダウンロードするためのリンクも含まれています。</span><span class="sxs-lookup"><span data-stu-id="3994c-119">It also includes a link to download the activity log as a CSV file.</span></span>

<span data-ttu-id="3994c-120">このユニットでは、Azure アクティビティ ログを使用して、ポータルで RBAC の変更を一覧表示し、シンプルなレポートを生成する方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="3994c-120">In this unit, you learned how to use Azure Activity Log to list RBAC changes in the portal and generate a simple report.</span></span>