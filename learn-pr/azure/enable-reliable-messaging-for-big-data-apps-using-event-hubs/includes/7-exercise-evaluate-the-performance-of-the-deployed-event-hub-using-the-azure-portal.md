<span data-ttu-id="0cf9a-101">このユニットでは、Azure portal を使用して、イベント ハブが想定どおりのパフォーマンスで動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-101">In this unit, you'll use the Azure portal to verify your Event Hub is working and performing according to the desired expectations.</span></span> <span data-ttu-id="0cf9a-102">また、イベント ハブが一時的に使用できなくなった場合にイベント ハブのメッセージングがどのように動作するかをテストし、Event Hubs メトリックを使用してイベント ハブのパフォーマンスを確認します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-102">You'll also test how Event Hub messaging works when it's temporarily unavailable and use Event Hubs metrics to check the performance of your Event Hub.</span></span>

## <a name="view-event-hub-activity"></a><span data-ttu-id="0cf9a-103">イベント ハブのアクティビティの確認</span><span class="sxs-lookup"><span data-stu-id="0cf9a-103">View Event Hub activity</span></span>

1. <span data-ttu-id="0cf9a-104">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="0cf9a-105">検索バーでイベント ハブを検索して開きます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-105">Find your Event Hub, using the Search bar, and open it.</span></span>

1. <span data-ttu-id="0cf9a-106">[概要] ページでメッセージ数を確認します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-106">On the Overview page, view the message counts.</span></span>

    ![イベント ハブ名前空間とメッセージ数が表示されている Azure portal のスクリーン ショット。](../media/6-view-messages.png)

1. <span data-ttu-id="0cf9a-108">SimpleSend アプリケーションと EventProcessorSample アプリケーションは、100 件のメッセージを送信/受信するように構成されています。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-108">The SimpleSend and EventProcessorSample applications are configured to send/receive 100 messages.</span></span> <span data-ttu-id="0cf9a-109">イベント ハブが、SimpleSend アプリケーションからの 100 件のメッセージを処理し、EventProcessorSample アプリケーションに 100 件のメッセージを送信したことがわかります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-109">You'll see that the Event Hub has processed 100 messages from the SimpleSend application and has transmitted 100 messages to the EventProcessorSample application.</span></span>

## <a name="test-event-hub-resilience"></a><span data-ttu-id="0cf9a-110">イベント ハブの回復力のテスト</span><span class="sxs-lookup"><span data-stu-id="0cf9a-110">Test Event Hub resilience</span></span>

<span data-ttu-id="0cf9a-111">次の手順に従って、イベント ハブが一時的に使用できなくなっているときに、アプリケーションがメッセージを送信するとどうなるかを確認します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-111">Use the following steps to see what happens when an application sends messages to an Event Hub while it's temporarily unavailable.</span></span>

1. <span data-ttu-id="0cf9a-112">SimpleSend アプリケーションを使用して、イベント ハブにメッセージを再送信します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-112">Resend messages to the Event Hub using the SimpleSend application.</span></span> <span data-ttu-id="0cf9a-113">次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-113">Use the following command:</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="0cf9a-114">"**Send Complete...**" と表示されたら、<kbd>Enter</kbd> キーを押します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-114">When you see **Send Complete...**, press <kbd>ENTER</kbd>.</span></span>

1. <span data-ttu-id="0cf9a-115">**[概要]** 画面でイベント ハブを選択します。これで、イベント ハブに固有の詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-115">Select your Event Hub in the **Overview** screen - this will show details specific to the Event Hub.</span></span> <span data-ttu-id="0cf9a-116">ネームスペース ページの **[Azure Event Hubs]** エントリからこの画面を表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-116">You can also get to this screen through the **Event Hubs** entry from the namespace page.</span></span>

1. <span data-ttu-id="0cf9a-117">**[設定]** > **[プロパティ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-117">Select **Settings** > **Properties**.</span></span>

1. <span data-ttu-id="0cf9a-118">イベント ハブの状態の下で、**[無効]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-118">Under Event Hub state, click **Disabled**.</span></span> <span data-ttu-id="0cf9a-119">変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-119">Save the changes.</span></span>

    ![イベント ハブの無効化](../media/7-disable-event-hub.png)

    <span data-ttu-id="0cf9a-121">**最低でも 5 分間待ちます。**</span><span class="sxs-lookup"><span data-stu-id="0cf9a-121">**Wait for a minimum of five minutes.**</span></span>

1. <span data-ttu-id="0cf9a-122">[イベント ハブの状態] の **[アクティブ]** をクリックしてイベント ハブを再度有効にし、変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-122">Click **Active** under Event Hub state to re-enable your Event Hub and save your changes.</span></span>

1. <span data-ttu-id="0cf9a-123">EventProcessorSample アプリケーションを再実行して、メッセージを受信します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-123">Rerun the EventProcessorSample application to receive messages.</span></span> <span data-ttu-id="0cf9a-124">次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-124">Use the following command.</span></span>

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. <span data-ttu-id="0cf9a-125">メッセージがコンソールに表示されなくなったら、<kbd>Enter</kbd> キーを押します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-125">When messages stop being displayed to the console, press <kbd>ENTER</kbd>.</span></span>

1. <span data-ttu-id="0cf9a-126">Azure portal で、イベント ハブの名前空間に戻ります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-126">Back in the Azure portal, go back to your Event Hub Namespace.</span></span> <span data-ttu-id="0cf9a-127">[イベント ハブ] ページ上にいる場合は、画面上部のブレッドクラムを使用して後方に移動できます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-127">If you are still on the Event Hub page, you can use the breadcrumb on the top of the screen to go backwards.</span></span> <span data-ttu-id="0cf9a-128">または、名前空間を検索して選択することもできます。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-128">Or you can search for the namespace and select it.</span></span>

1. <span data-ttu-id="0cf9a-129">**[監視]** > **[メトリック (プレビュー)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-129">Click **MONITORING** > **Metrics (preview)**.</span></span>

    ![着信メッセージと発信メッセージの数が表示されたイベント ハブ メトリックを示すスクリーンショット。](../media/7-event-hub-metrics.png)

1. <span data-ttu-id="0cf9a-131">**[メトリック]** の一覧から **[受信メッセージ]** を選択し、**[メトリックの追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-131">From the **Metric** list, select **Incoming Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="0cf9a-132">**[メトリック]** の一覧から **[送信メッセージ]** を選択し、**[メトリックの追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-132">From the **Metric** list, select **Outgoing Messages** and click **Add Metric**.</span></span>

1. <span data-ttu-id="0cf9a-133">グラフの上部にある **[Last 24 hours (Automatic)]\(過去 24 時間 (自動)\)** をクリックして、期間を **[過去 30 分間]** に変更してデータ グラフを拡張します。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-133">At the top of the chart, click **Last 24 hours (Automatic)** to change the time period to **Last 30 minutes** to expand the data graph.</span></span>

<span data-ttu-id="0cf9a-134">メッセージを送信したのはイベント ハブがしばらくオフラインになる前でしたが、100 件のメッセージがすべて正常に送信されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-134">You'll see that though the messages were sent before the Event Hub was taken offline for a period, all 100 messages were successfully transmitted.</span></span>

## <a name="summary"></a><span data-ttu-id="0cf9a-135">まとめ</span><span class="sxs-lookup"><span data-stu-id="0cf9a-135">Summary</span></span>

<span data-ttu-id="0cf9a-136">このユニットでは、Event Hubs メトリックを使用して、イベント ハブがメッセージの送受信を正常に処理していることをテストしました。</span><span class="sxs-lookup"><span data-stu-id="0cf9a-136">In this unit, you used the Event Hubs metrics to test that your Event Hub is successfully processing the sending and receiving messages.</span></span>
