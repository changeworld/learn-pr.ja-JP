<span data-ttu-id="81768-101">Azure Service Bus トピックを使用して、営業部門の分散アプリケーションで販売業績に関するメッセージを配信することにしました。</span><span class="sxs-lookup"><span data-stu-id="81768-101">You have chosen to use an Azure Service Bus topic to distribute messages about sales performance in your sales force distributed application.</span></span> <span data-ttu-id="81768-102">販売担当者が自分のモバイル デバイスで使用しているアプリが、各エリアと期間ごとの売り上げ高を集計したメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="81768-102">The app used by sales personnel on their mobile devices will send messages that summarize sales figures for each area and time period.</span></span> <span data-ttu-id="81768-103">これらのメッセージは、南北アメリカやヨーロッパなど会社の営業地域内にある Web サービスに配信されます。</span><span class="sxs-lookup"><span data-stu-id="81768-103">Those messages will be distributed to web services located in the company's operational regions, including the Americas and Europe.</span></span>

<span data-ttu-id="81768-104">トピックやサブスクリプションなど、必要なインフラストラクチャは Azure サブスクリプションに既に実装してあります。</span><span class="sxs-lookup"><span data-stu-id="81768-104">You have already implemented the necessary infrastructure in your Azure subscription, including the topic and subscriptions.</span></span> <span data-ttu-id="81768-105">ここでは、トピックにメッセージを送信し、各サブスクリプションからメッセージを取得するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="81768-105">Now, you want to write the code that sends messages to the topic and retrieves messages from each subscription.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="81768-106">Service Bus 名前空間への接続文字列を構成する</span><span class="sxs-lookup"><span data-stu-id="81768-106">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="81768-107">最初に、送信コンポーネントと受信コンポーネントの両方で接続文字列を構成します。</span><span class="sxs-lookup"><span data-stu-id="81768-107">Start by configuring connection strings both in the sending and receiving components:</span></span>

1. <span data-ttu-id="81768-108">Azure portal に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="81768-108">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="81768-109">ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-109">In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="81768-110">**[設定]** の **[共有アクセス ポリシー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-110">Under **SETTINGS**, click **Shared Access Policies**.</span></span>

1. <span data-ttu-id="81768-111">ポリシーの一覧で、**RootManageSharedAccessKey** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-111">In the list of policies, click **RootManageSharedAccessKey**.</span></span>

1. <span data-ttu-id="81768-112">**[プライマリ接続文字列]** テキスト　ボックスの右側にある **[クリックしてコピー]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-112">To the right of the **Primary Connection string** text box, click the **Click to copy** button.</span></span>

1. <span data-ttu-id="81768-113">**Visual Studio Code** に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="81768-113">Switch to **Visual Studio Code**.</span></span>

1. <span data-ttu-id="81768-114">**[エクスプローラー]** ウィンドウで、**performancemessagesender** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-114">In the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="81768-115">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-115">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="81768-116">引用符の間にカーソルを置き、**Ctrl + V** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="81768-116">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="81768-117">**[エクスプローラー]** ウィンドウで、**performancemessagereceiver** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-117">In the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="81768-118">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-118">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="81768-119">引用符の間にカーソルを置き、**Ctrl + V** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="81768-119">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="81768-120">**[ファイル]**、**[すべて保存]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-120">Click **File**, and then click **Save All**.</span></span>

1. <span data-ttu-id="81768-121">開いているすべてのエディター ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="81768-121">Close all open editor windows.</span></span>

## <a name="write-code-that-sends-a-message-to-the-topic"></a><span data-ttu-id="81768-122">トピックにメッセージを送信するコードを書き込みます</span><span class="sxs-lookup"><span data-stu-id="81768-122">Write code that sends a message to the topic</span></span>

<span data-ttu-id="81768-123">販売業績に関するメッセージを送信するコンポーネントを完成させるには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="81768-123">To complete the component that sends messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="81768-124">Visual Studio Code の **[エクスプローラー]** ウィンドウで、**performancemessagesender** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-124">In Visual Studio Code, in the **Explorer** pane, in the **performancemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="81768-125">`SendPerformanceMessageAsync()` メソッドを見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-125">Locate the `SendPerformanceMessageAsync()` method.</span></span>

1. <span data-ttu-id="81768-126">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-126">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a Topic Client here
    ```

1. <span data-ttu-id="81768-127">トピック クライアントを作成するには、そのコード行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81768-127">To create a topic client, replace that line of code with the following code:</span></span>

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. <span data-ttu-id="81768-128">`try...catch` ブロック内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-128">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="81768-129">キューに対するメッセージを作成してフォーマットするには、そのコード行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81768-129">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="81768-130">コンソールにメッセージを表示するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="81768-130">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="81768-131">キューにメッセージを送信するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="81768-131">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await topicClient.SendAsync(message);
    ```

1. <span data-ttu-id="81768-132">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-132">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the topic here
    ```

1. <span data-ttu-id="81768-133">Service Bus への接続を閉じるには、そのコード行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81768-133">To close the connection to Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await topicClient.CloseAsync();
    ```

1. <span data-ttu-id="81768-134">Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="81768-134">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="send-a-message-to-the-topic"></a><span data-ttu-id="81768-135">メッセージをトピックに送信する</span><span class="sxs-lookup"><span data-stu-id="81768-135">Send a message to the topic</span></span>

<span data-ttu-id="81768-136">販売に関するメッセージを送信するコンポーネントを実行するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="81768-136">To run the component that sends a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="81768-137">Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-137">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="81768-138">**[デバッグ]** ウィンドウのドロップダウン リストで、**[Performance Message Sender を起動]** を選択して、**F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="81768-138">In the **Debug** pane, in the drop-down list, select **Launch Performance Message Sender**, and then press **F5**.</span></span> <span data-ttu-id="81768-139">Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。</span><span class="sxs-lookup"><span data-stu-id="81768-139">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="81768-140">プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。</span><span class="sxs-lookup"><span data-stu-id="81768-140">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="81768-141">Azure portal に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="81768-141">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="81768-142">Service Bus の名前空間が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-142">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="81768-143">**[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[トピック]** をクリックし、**salesperformancemessages** トピックをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-143">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic.</span></span> <span data-ttu-id="81768-144">サブスクリプションの一覧では、**南北アメリカ**と**ヨーロッパ**サブスクリプションの両方に一つのメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="81768-144">In the list of subscriptions, there should be one message displayed in both the **Americas** and **Europe** subscriptions.</span></span>

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a><span data-ttu-id="81768-145">トピックサブスクリプションからメッセージを受信するコードを書き込む</span><span class="sxs-lookup"><span data-stu-id="81768-145">Write code that receives a message from a topic subscription</span></span>

<span data-ttu-id="81768-146">販売業績に関するメッセージを取得するコンポーネントを完成させるには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="81768-146">To complete the component that retrieves messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="81768-147">Visual Studio Code の **[エクスプローラー]** ウィンドウで、**performancemessagereceiver** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-147">In Visual Studio Code, in the **Explorer** pane, in the **performancemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="81768-148">`MainAsync()` メソッドを見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-148">Locate the `MainAsync()` method.</span></span>

1. <span data-ttu-id="81768-149">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-149">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a subscription client here
    ```

1. <span data-ttu-id="81768-150">サブスクリプション クライアントを作成するには、その行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81768-150">To create a subscription client, replace that line with the following code:</span></span>

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. <span data-ttu-id="81768-151">`RegisterMessageHandler()` メソッドを見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-151">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="81768-152">メッセージ処理オプションを構成するには、そのメソッド内のすべてのコードを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81768-152">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="81768-153">メッセージ ハンドラーを登録するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="81768-153">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="81768-154">`ProcessMessagesAsync()` メソッドを見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-154">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="81768-155">このメソッドを、受信メッセージを処理するメソッドとして登録しました。</span><span class="sxs-lookup"><span data-stu-id="81768-155">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="81768-156">受信メッセージをコンソールに表示するには、そのメソッド内のすべてのコードを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81768-156">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="81768-157">サブスクリプションから受信したメッセージを削除するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="81768-157">To remove the received message from the subscription, on the next line, add the following code:</span></span>

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="81768-158">`MainAsync()` メソッドに戻り、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="81768-158">Return to the `MainAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the subscription here
    ```

1. <span data-ttu-id="81768-159">Service Bus への接続を終了するには、そのコードを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81768-159">To close the connection to Service Bus, replace that code with the following code:</span></span>

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. <span data-ttu-id="81768-160">Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="81768-160">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-a-topic-subscription"></a><span data-ttu-id="81768-161">トピック サブスクリプションからメッセージを取得する</span><span class="sxs-lookup"><span data-stu-id="81768-161">Retrieve a message from a topic subscription</span></span>

<span data-ttu-id="81768-162">販売業績に関するメッセージを取得するコンポーネントを実行するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="81768-162">To run the component that retrieves a message about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="81768-163">Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-163">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="81768-164">**[デバッグ]** ウィンドウのドロップダウン リストで、**[Performance Message Receiver を起動]** を選択して、**F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="81768-164">In the **Debug** pane, in the drop-down list, select **Launch Performance Message Receiver**, and then press **F5**.</span></span> <span data-ttu-id="81768-165">Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。</span><span class="sxs-lookup"><span data-stu-id="81768-165">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="81768-166">プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。</span><span class="sxs-lookup"><span data-stu-id="81768-166">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="81768-167">メッセージが受信されてコンソールに表示されたら、**[デバッグ]** メニューの **[デバッグの停止]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-167">When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.</span></span>

1. <span data-ttu-id="81768-168">Azure portal に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="81768-168">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="81768-169">Service Bus の名前空間が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-169">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="81768-170">**[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[トピック]** をクリックし、**salesperformancemessages** トピックをクリックします。</span><span class="sxs-lookup"><span data-stu-id="81768-170">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Topics**, and then click the **salesperformancemessages** topic.</span></span> <span data-ttu-id="81768-171">サブスクリプションの一覧では、アプリケーションが処理され、唯一のメッセージが削除されるため、**南北アメリカ**サブスクリプションに表示されるメッセージはゼロであるはずです。</span><span class="sxs-lookup"><span data-stu-id="81768-171">In the list of subscriptions, there should be zero messages displayed in the **Americas** subscription because your application has processed and removed the only message.</span></span> <span data-ttu-id="81768-172">**ヨーロッパ**サブスクリプションにはメッセージがあることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="81768-172">Notice that the message is still present in the **Europe** subscription.</span></span>
