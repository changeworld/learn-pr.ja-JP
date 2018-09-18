<span data-ttu-id="567b4-101">このユニットでは、Azure portal を使用して、イベント ハブが想定どおりに動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="567b4-101">In this unit, you'll use the Azure portal to verify your event hub is working and performing according to the desired expectations.</span></span> <span data-ttu-id="567b4-102">また、一時的に使用できなくなった場合にイベント ハブのメッセージングがどのように動作するかをテストし、Event Hubs メトリックを使用してイベント ハブのパフォーマンスを確認します。</span><span class="sxs-lookup"><span data-stu-id="567b4-102">You'll also test how event hub messaging works when it's temporarily unavailable and use Event Hubs metrics to check the performance of your event hub.</span></span>

## <a name="view-event-hub-activity"></a><span data-ttu-id="567b4-103">イベント ハブのアクティビティの確認</span><span class="sxs-lookup"><span data-stu-id="567b4-103">View event hub activity</span></span>

1. <span data-ttu-id="567b4-104">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="567b4-104">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="567b4-105">検索バーを使用してイベント ハブを検索し、それを開きます。</span><span class="sxs-lookup"><span data-stu-id="567b4-105">Find your event hub, using the Search bar, and open it.</span></span>

1. <span data-ttu-id="567b4-106">[概要] ページでメッセージ数を確認します。</span><span class="sxs-lookup"><span data-stu-id="567b4-106">On the Overview page, view the message counts.</span></span>

    ![イベント ハブのメッセージの確認](../media-draft/6-view-messages.png)

1. <span data-ttu-id="567b4-108">SimpleSend アプリケーションと EventProcessorSample アプリケーションは、100 件のメッセージを送信/受信するように構成されています。</span><span class="sxs-lookup"><span data-stu-id="567b4-108">The SimpleSend and EventProcessorSample applications are configured to send/receive 100 messages.</span></span> <span data-ttu-id="567b4-109">イベント ハブが、SimpleSend アプリケーションからの 100 件のメッセージを処理し、EventProcessorSample アプリケーションに 100 件のメッセージを送信したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="567b4-109">You'll see that the event hub has processed 100 messages from the SimpleSend application and has transmitted 100 messages to the EventProcessorSample application.</span></span>

## <a name="test-event-hub-resilience"></a><span data-ttu-id="567b4-110">イベント ハブの回復力のテスト</span><span class="sxs-lookup"><span data-stu-id="567b4-110">Test event hub resilience</span></span>

<span data-ttu-id="567b4-111">次の手順に従って、イベント ハブが一時的に使用不可になっている間にアプリケーションがメッセージを送信するとどうなるかを確認します。</span><span class="sxs-lookup"><span data-stu-id="567b4-111">Use the following steps to see what happens when an application sends messages to an event hub while it's temporarily unavailable.</span></span>

1. <span data-ttu-id="567b4-112">SimpleSend アプリケーションを使用して、イベント ハブにメッセージを再送信します。</span><span class="sxs-lookup"><span data-stu-id="567b4-112">Resend messages to the event hub using the SimpleSend application.</span></span> <span data-ttu-id="567b4-113">次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="567b4-113">Use the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="567b4-114">"**Send Complete...**" と表示されたら、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="567b4-114">When you see **Send Complete...**, press ENTER.</span></span>

1. <span data-ttu-id="567b4-115">Azure Portal で、**[Event Hubs のインスタンス]** > **[設定]** > **[プロパティ]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="567b4-115">In the Azure portal, click **Event Hubs Instance** > **SETTINGS** > **Properties**.</span></span>

1. <span data-ttu-id="567b4-116">イベント ハブの状態の下で、**[無効]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="567b4-116">Under Event Hub state, click **Disabled**.</span></span>

    ![イベント ハブの無効化](../media-draft/7-disable-event-hub.png)

<span data-ttu-id="567b4-118">最低でも 5 分間待機します。</span><span class="sxs-lookup"><span data-stu-id="567b4-118">Wait for a minimum of five minutes.</span></span>

1. <span data-ttu-id="567b4-119">イベント ハブの状態の下で **[アクティブ]** をクリックしてイベント ハブを再び有効にし、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="567b4-119">Click **Active** under Event Hub state to re-enable your event hub and save your changes.</span></span>

1. <span data-ttu-id="567b4-120">EventProcessorSample アプリケーションを再実行して、メッセージを受信します。</span><span class="sxs-lookup"><span data-stu-id="567b4-120">Rerun the EventProcessorSample application to receive messages.</span></span> <span data-ttu-id="567b4-121">次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="567b4-121">Use the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="567b4-122">メッセージがコンソールに表示されなくなったら、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="567b4-122">When messages stop being displayed to the console, press ENTER.</span></span>

1. <span data-ttu-id="567b4-123">Azure Portal でイベント ハブの **_namespace_** を検索し、それを開きます。</span><span class="sxs-lookup"><span data-stu-id="567b4-123">In the Azure portal, find your event hub **_namespace_** and open it.</span></span> 

1. <span data-ttu-id="567b4-124">**[Event Hubs 名前空間]** > **[監視]** > **[メトリック (プレビュー)]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="567b4-124">Click **Event Hubs Namespace** > **MONITORING** > **Metrics (preview)**.</span></span>

    ![イベント ハブのメトリックの使用](../media-draft/7-event-hub-metrics.png)

1. <span data-ttu-id="567b4-126">**[メトリック]** の一覧から **[受信メッセージ]** を選択し、**[メトリックの追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="567b4-126">From the **Metric** list, select **Incoming Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="567b4-127">**[メトリック]** の一覧から **[送信メッセージ]** を選択し、**[メトリックの追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="567b4-127">From the **Metric** list, select **Outgoing Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="567b4-128">グラフの上部にある **[Last 24 hours (Automatic)]\(過去 24 時間 (自動)\)** をクリックして、期間を **[過去 30 分間]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="567b4-128">At the top of the chart, click **Last 24 hours (Automatic)** to change the time period to **Last 30 minutes**.</span></span>

1. <span data-ttu-id="567b4-129">**[適用]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="567b4-129">Click **Apply**.</span></span>

<span data-ttu-id="567b4-130">メッセージを送信したのはイベント ハブがしばらくオフラインになる前でしたが、100 件のメッセージがすべて正常に送信されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="567b4-130">You'll see that though the messages were sent before the event hub was taken offline for a period, all 100 messages were successfully transmitted.</span></span>

## <a name="summary"></a><span data-ttu-id="567b4-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="567b4-131">Summary</span></span>

<span data-ttu-id="567b4-132">この演習では、Event Hubs メトリックを使用して、イベント ハブがメッセージの送受信を正常に処理していることをテストしました。</span><span class="sxs-lookup"><span data-stu-id="567b4-132">In this unit, you used the Event Hubs metrics to test that your event hub is successfully processing the sending and receiving messages.</span></span>
