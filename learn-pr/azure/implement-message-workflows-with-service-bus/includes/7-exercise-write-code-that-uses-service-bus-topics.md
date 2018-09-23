<span data-ttu-id="a350f-101">Azure Service Bus トピックを使用して、営業部門の分散アプリケーションで販売業績に関するメッセージを配信することにしました。</span><span class="sxs-lookup"><span data-stu-id="a350f-101">You have chosen to use an Azure Service Bus topic to distribute messages about sales performance in your sales force distributed application.</span></span> <span data-ttu-id="a350f-102">販売担当者が自分のモバイル デバイスで使用しているアプリが、各エリアと期間ごとの売り上げ高を集計したメッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="a350f-102">The app used by sales personnel on their mobile devices will send messages that summarize sales figures for each area and time period.</span></span> <span data-ttu-id="a350f-103">これらのメッセージは、南北アメリカやヨーロッパなど会社の営業地域内にある Web サービスに配信されます。</span><span class="sxs-lookup"><span data-stu-id="a350f-103">Those messages will be distributed to web services located in the company's operational regions, including the Americas and Europe.</span></span>

<span data-ttu-id="a350f-104">トピックやサブスクリプションなど、必要なインフラストラクチャは Azure サブスクリプションに既に実装してあります。</span><span class="sxs-lookup"><span data-stu-id="a350f-104">You have already implemented the necessary infrastructure in your Azure subscription, including the topic and subscriptions.</span></span> <span data-ttu-id="a350f-105">ここでは、トピックにメッセージを送信し、サブスクリプションからメッセージを取得するコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="a350f-105">Now, you want to write the code that sends messages to the topic and retrieves messages from a subscription.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="a350f-106">Service Bus 名前空間への接続文字列を構成する</span><span class="sxs-lookup"><span data-stu-id="a350f-106">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="a350f-107">最初に、送信コンポーネントと受信コンポーネントの両方で接続文字列を構成します。</span><span class="sxs-lookup"><span data-stu-id="a350f-107">Start by configuring connection strings both in the sending and receiving components:</span></span>

1. <span data-ttu-id="a350f-108">エディターで、**performancemessagesender/Program.cs** を開き、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a350f-108">In the editor, open **performancemessagesender/Program.cs** and locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    <span data-ttu-id="a350f-109">引用符の間に接続文字列を貼り付け、"..." メニューまたはショートカット キー (Windows と Linux では <kbd>Ctrl + S</kbd>、macOS では <kbd>Cmd + S</kbd>) を使用して、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a350f-109">Paste the connection string between the quotation marks and save the file either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

1. <span data-ttu-id="a350f-110">**performancemessagereceiver/Program.cs** で前の手順を繰り返します。その場合、同じ接続文字列値に貼り付け、そのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a350f-110">Repeat the previous step in **performancemessagereceiver/Program.cs**, pasting in the same connection string value and save the file.</span></span>

## <a name="write-code-that-sends-a-message-to-the-topic"></a><span data-ttu-id="a350f-111">トピックにメッセージを送信するコードを書き込みます</span><span class="sxs-lookup"><span data-stu-id="a350f-111">Write code that sends a message to the topic</span></span>

<span data-ttu-id="a350f-112">販売業績に関するメッセージを送信するコンポーネントを完成させるには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="a350f-112">To complete the component that sends messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="a350f-113">エディターで **performancemessagesender/Program.cs** を開きます。</span><span class="sxs-lookup"><span data-stu-id="a350f-113">Open **performancemessagesender/Program.cs** in the editor.</span></span>

1. <span data-ttu-id="a350f-114">`SendPerformanceMessageAsync()` メソッドを探します。</span><span class="sxs-lookup"><span data-stu-id="a350f-114">Locate the `SendPerformanceMessageAsync()` method.</span></span>

1. <span data-ttu-id="a350f-115">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a350f-115">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a Topic Client here
    ```

1. <span data-ttu-id="a350f-116">トピック クライアントを作成するには、そのコード行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a350f-116">To create a topic client, replace that line of code with the following code:</span></span>

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. <span data-ttu-id="a350f-117">`try...catch` ブロック内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a350f-117">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="a350f-118">キューに対するメッセージを作成して書式設定するには、そのコード行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a350f-118">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="a350f-119">コンソールにメッセージを表示するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a350f-119">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="a350f-120">キューにメッセージを送信するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a350f-120">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await topicClient.SendAsync(message);
    ```

1. <span data-ttu-id="a350f-121">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a350f-121">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the topic here
    ```

1. <span data-ttu-id="a350f-122">Service Bus への接続を閉じるには、そのコード行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a350f-122">To close the connection to Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await topicClient.CloseAsync();
    ```

1. <span data-ttu-id="a350f-123">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a350f-123">Save the file.</span></span>

## <a name="send-a-message-to-the-topic"></a><span data-ttu-id="a350f-124">メッセージをトピックに送信する</span><span class="sxs-lookup"><span data-stu-id="a350f-124">Send a message to the topic</span></span>

<span data-ttu-id="a350f-125">販売に関するメッセージを送信するコンポーネントを実行するには、Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a350f-125">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p performancemessagesender
```

<span data-ttu-id="a350f-126">プログラムを実行すると、メッセージを送信していることを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a350f-126">As the program executes, you'll see messages printed indicating that it's sending a message.</span></span> <span data-ttu-id="a350f-127">アプリを実行するたびに、1 つのメッセージがトピックに追加され、各サブスクライバーがコピーを受信します。</span><span class="sxs-lookup"><span data-stu-id="a350f-127">Each time you run the app, one additional message will be added to the topic, and each subscriber will receive a copy.</span></span>

<span data-ttu-id="a350f-128">終了したら、次のコマンドを実行して、Americas サブスクリプションのメッセージの数を確認します。</span><span class="sxs-lookup"><span data-stu-id="a350f-128">Once it's finished, run the following command to see how many messages are in the Americas subscription:</span></span>

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

<span data-ttu-id="a350f-129">`Americas` の代わりに `EuropeAndAfrica` を使用している場合は、両方のサブスクリプションに同じ数のメッセージがあることがわかります。</span><span class="sxs-lookup"><span data-stu-id="a350f-129">If you substitute `EuropeAndAfrica` for `Americas`, you should see that both subscriptions have the same number of messages.</span></span>

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a><span data-ttu-id="a350f-130">トピック サブスクリプションからメッセージを受信するコードを書き込む</span><span class="sxs-lookup"><span data-stu-id="a350f-130">Write code that receives a message from a topic subscription</span></span>

<span data-ttu-id="a350f-131">販売業績に関するメッセージを取得するコンポーネントを完成させるには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="a350f-131">To complete the component that retrieves messages about sales performance, follow these steps:</span></span>

1. <span data-ttu-id="a350f-132">エディターで **performancemessagereceiver/Program.cs** を開きます。</span><span class="sxs-lookup"><span data-stu-id="a350f-132">Open **performancemessagereceiver/Program.cs** in the editor.</span></span>

1. <span data-ttu-id="a350f-133">`MainAsync()` メソッドを探します。</span><span class="sxs-lookup"><span data-stu-id="a350f-133">Locate the `MainAsync()` method.</span></span>

1. <span data-ttu-id="a350f-134">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a350f-134">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a subscription client here
    ```

1. <span data-ttu-id="a350f-135">サブスクリプション クライアントを作成するには、その行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a350f-135">To create a subscription client, replace that line with the following code:</span></span>

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. <span data-ttu-id="a350f-136">`RegisterMessageHandler()` メソッドを見つけます。</span><span class="sxs-lookup"><span data-stu-id="a350f-136">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="a350f-137">メッセージ処理オプションを構成するには、そのメソッド内のすべてのコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a350f-137">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="a350f-138">メッセージ ハンドラーを登録するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a350f-138">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="a350f-139">`ProcessMessagesAsync()` メソッドを探します。</span><span class="sxs-lookup"><span data-stu-id="a350f-139">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="a350f-140">このメソッドを、受信メッセージを処理するメソッドとして登録しました。</span><span class="sxs-lookup"><span data-stu-id="a350f-140">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="a350f-141">受信メッセージをコンソールに表示するには、そのメソッド内のすべてのコードを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a350f-141">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="a350f-142">サブスクリプションから受信したメッセージを削除するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a350f-142">To remove the received message from the subscription, on the next line, add the following code:</span></span>

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="a350f-143">`MainAsync()` メソッドに戻り、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a350f-143">Return to the `MainAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the subscription here
    ```

1. <span data-ttu-id="a350f-144">Service Bus への接続を終了するには、そのコードを次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a350f-144">To close the connection to Service Bus, replace that code with the following code:</span></span>

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. <span data-ttu-id="a350f-145">Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a350f-145">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-a-topic-subscription"></a><span data-ttu-id="a350f-146">トピック サブスクリプションからメッセージを取得する</span><span class="sxs-lookup"><span data-stu-id="a350f-146">Retrieve a message from a topic subscription</span></span>

<span data-ttu-id="a350f-147">販売業績に関するメッセージを取得するコンポーネントを実行するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="a350f-147">To run the component that retrieves a message about sales performance, follow these steps:</span></span>

```bash
dotnet run -p performancemessagereceiver
```

<span data-ttu-id="a350f-148">メッセージを受信しているプログラムが通知の出力を停止したら、</span><span class="sxs-lookup"><span data-stu-id="a350f-148">When the program stops printing notifications that it is receiving messages.</span></span> <span data-ttu-id="a350f-149">`Enter` キーを押してアプリを停止します。</span><span class="sxs-lookup"><span data-stu-id="a350f-149">press `Enter` to stop the app.</span></span> <span data-ttu-id="a350f-150">次に、前と同じコマンドを実行して、`Americas` サブスクリプションに残っているメッセージがないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a350f-150">Then, run the same command as before to confirm that there are zero remaining messages in the `Americas` subscription.</span></span>

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

<span data-ttu-id="a350f-151">`Americas` の代わりに `EuropeAndAfrica` を使用している場合は、メッセージの数が変化していないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="a350f-151">If you substitute `EuropeAndAfrica` for `Americas`, you'll see that the message count has not changed.</span></span> <span data-ttu-id="a350f-152">アプリケーションは、`Americas` サブスクリプションからのみメッセージを受信していました。</span><span class="sxs-lookup"><span data-stu-id="a350f-152">The application only received messages from the `Americas` subscription.</span></span>
