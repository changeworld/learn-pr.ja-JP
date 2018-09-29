<span data-ttu-id="372c7-101">販売担当者が使用しているモバイル アプリと、Azure SQL Database インスタンスに各販売に関する詳細情報を格納する、Azure でホストされた Web サービスの間で、Service Bus キューを使用して、個々の販売に関するメッセージを交換することを選択しました。</span><span class="sxs-lookup"><span data-stu-id="372c7-101">You've chosen to use a Service Bus queue to exchange messages about individual sales between the mobile app that your sales personnel use and the web service, hosted in Azure, that will store details about each sale in an Azure SQL Database instance.</span></span>

<span data-ttu-id="372c7-102">必要なオブジェクトは Azure サブスクリプションに既に実装してあります。</span><span class="sxs-lookup"><span data-stu-id="372c7-102">You've already implemented the necessary objects in your Azure subscription.</span></span> <span data-ttu-id="372c7-103">ここでは、そのキューにメッセージを送信したり、キューからメッセージを取得したりするコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="372c7-103">Now, you want to write code that sends messages to that queue and retrieves messages.</span></span>

## <a name="clone-and-open-the-starter-application"></a><span data-ttu-id="372c7-104">スターター アプリケーションを複製して開く</span><span class="sxs-lookup"><span data-stu-id="372c7-104">Clone and open the starter application</span></span>

<span data-ttu-id="372c7-105">このユニットでは、2 つのコンソール アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="372c7-105">In this unit, you'll build two console applications.</span></span> <span data-ttu-id="372c7-106">1 つ目のアプリケーションでメッセージを Service Bus キューに入れ、2 つ目のアプリケーションでそれらを取得します。</span><span class="sxs-lookup"><span data-stu-id="372c7-106">The first application places messages into a Service Bus queue and the second retrieves them.</span></span> <span data-ttu-id="372c7-107">これらのアプリケーションは、単一の .NET Core ソリューションの一部です。</span><span class="sxs-lookup"><span data-stu-id="372c7-107">The applications are part of a single .NET Core solution.</span></span>

1. <span data-ttu-id="372c7-108">まず、ソリューションを複製します。その場合、Cloud Shell で以下のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="372c7-108">Start by cloning the solution: run the following commands in the Cloud Shell:</span></span>

```bash
cd ~
git clone https://github.com/MicrosoftDocs/mslearn-connect-services-together.git
```

2. <span data-ttu-id="372c7-109">次に、スターター フォルダーにディレクトリを変更し、Cloud Shell エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="372c7-109">Next, change directories into the starter folder and open the Cloud Shell editor.</span></span>

```bash
cd mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
code .
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="372c7-110">Service Bus 名前空間への接続文字列を構成する</span><span class="sxs-lookup"><span data-stu-id="372c7-110">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="372c7-111">Service Bus 名前空間にアクセスしてキューを使用するには、コンソール アプリで次の 2 つの情報を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="372c7-111">In order to access a Service Bus namespace and use a queue, you must configure two pieces of information in your console apps:</span></span>

* <span data-ttu-id="372c7-112">名前空間のエンドポイント</span><span class="sxs-lookup"><span data-stu-id="372c7-112">The endpoint for your namespace</span></span>
* <span data-ttu-id="372c7-113">認証のための共有アクセス キー</span><span class="sxs-lookup"><span data-stu-id="372c7-113">The shared access key for authentication</span></span>

<span data-ttu-id="372c7-114">どちらの値も、完全な接続文字列の形式で Azure portal から取得できます。</span><span class="sxs-lookup"><span data-stu-id="372c7-114">Both of these values can be obtained from the Azure portal in the form of a complete connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="372c7-115">わかりやすくするため、ここでは、両方のコンソール アプリケーションの **Program.cs** ファイルに接続文字列をハード コーディングします。</span><span class="sxs-lookup"><span data-stu-id="372c7-115">For simplicity, you will hard-code the connection string in the **Program.cs** file of both console applications.</span></span> <span data-ttu-id="372c7-116">運用アプリケーションでは、構成ファイルまたは Azure Key Vault を使用して接続文字列を格納できます。</span><span class="sxs-lookup"><span data-stu-id="372c7-116">In a production application, you might use a configuration file or even Azure Key Vault to store the connection string.</span></span>

1. <span data-ttu-id="372c7-117">Cloud Shell で、次のコマンドを実行して、ご利用の Service Bus 名前空間用のプライマリ接続文字列を表示します。</span><span class="sxs-lookup"><span data-stu-id="372c7-117">In the Cloud Shell, run the following command to display the primary connection string for your Service Bus namespace.</span></span> <span data-ttu-id="372c7-118">`<namespace-name>` を、実際の Service Bus 名前空間の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-118">Replace `<namespace-name>` with the name of your Service Bus namespace.</span></span>

    ```azurecli
    az servicebus namespace authorization-rule keys list \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --namespace-name <namespace-name> \
        --name RootManageSharedAccessKey \
        --query primaryConnectionString \
        --output tsv
    ```

    <span data-ttu-id="372c7-119">この接続文字列はこのモジュールを通して複数回必要になるため、すぐ使用できる場所に貼り付けてください。</span><span class="sxs-lookup"><span data-stu-id="372c7-119">You'll be needing this connection string multiple times throughout this module, so you might want to paste it somewhere handy.</span></span>

1. <span data-ttu-id="372c7-120">Cloud Shell からキーをコピーします。</span><span class="sxs-lookup"><span data-stu-id="372c7-120">Copy the key from Cloud Shell.</span></span> <span data-ttu-id="372c7-121">エディターで、**privatemessagesender/Program.cs** を開き、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="372c7-121">In the editor, open **privatemessagesender/Program.cs** and locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    <span data-ttu-id="372c7-122">接続文字列を引用符の間に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="372c7-122">Paste the connection string between the quotation marks.</span></span> <span data-ttu-id="372c7-123"><kbd>Ctrl + S</kbd> キーを使用して、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="372c7-123">Save the file with <kbd>Ctrl+S</kbd>.</span></span>

1. <span data-ttu-id="372c7-124">**privatemessagereceiver/Program.cs** で前の手順を繰り返します。その場合、同じ接続文字列値に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="372c7-124">Repeat the previous step in **privatemessagereceiver/Program.cs**, pasting in the same connection string value.</span></span> <span data-ttu-id="372c7-125">[...] メニューまたはアクセラレータ キー (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Cmd + S</kbd>) を使用して、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="372c7-125">Save the file either through the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

## <a name="write-code-that-sends-a-message-to-the-queue"></a><span data-ttu-id="372c7-126">キューにメッセージを送信するコードを記述する</span><span class="sxs-lookup"><span data-stu-id="372c7-126">Write code that sends a message to the queue</span></span>

<span data-ttu-id="372c7-127">販売に関するメッセージを送信するコンポーネントを完成させるには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="372c7-127">To complete the component that sends messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="372c7-128">エディターで **privatemessagesender/Program.cs** を開く</span><span class="sxs-lookup"><span data-stu-id="372c7-128">Open **privatemessagesender/Program.cs** in the editor</span></span>

1. <span data-ttu-id="372c7-129">`SendSalesMessageAsync()` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="372c7-129">Locate the `SendSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="372c7-130">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="372c7-130">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="372c7-131">キュー クライアントを作成するには、そのコード行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-131">To create a queue client, replace that line of code with the following code:</span></span>

    ```C#
    var queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="372c7-132">`try...catch` ブロック内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="372c7-132">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="372c7-133">キューに対するメッセージを作成して書式設定するには、そのコード行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-133">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="372c7-134">コンソールにメッセージを表示するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="372c7-134">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="372c7-135">キューにメッセージを送信するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="372c7-135">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.SendAsync(message);
    ```

1. <span data-ttu-id="372c7-136">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="372c7-136">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the queue here
    ```

1. <span data-ttu-id="372c7-137">Service Bus への接続を閉じるには、そのコード行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-137">To close the connection the Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="372c7-138">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="372c7-138">Save the file.</span></span>

## <a name="send-a-message-to-the-queue"></a><span data-ttu-id="372c7-139">メッセージをキューに送信する</span><span class="sxs-lookup"><span data-stu-id="372c7-139">Send a message to the queue</span></span>

<span data-ttu-id="372c7-140">販売に関するメッセージを送信するコンポーネントを実行するには、Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="372c7-140">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p privatemessagesender
```

> [!NOTE]
> <span data-ttu-id="372c7-141">この演習中に実行するアプリは起動するまでしばらく時間がかかる場合があります。これは、`dotnet` でリモート ソースからパッケージを復元し、アプリを初回の実行時にビルドする必要があるためです。</span><span class="sxs-lookup"><span data-stu-id="372c7-141">The apps you run during this exercise may take a moment to start up, as `dotnet` has to restore packages from remote sources and build the apps the first time they are run.</span></span>

<span data-ttu-id="372c7-142">プログラムを実行すると、メッセージを送信していることを示すメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="372c7-142">As the program executes, you'll see messages printed indicating that it's sending a message.</span></span> <span data-ttu-id="372c7-143">アプリを実行するたびに、1 つのメッセージがキューに追加されます。</span><span class="sxs-lookup"><span data-stu-id="372c7-143">Each time you run the app, one additional message will be added to the queue.</span></span>

<span data-ttu-id="372c7-144">終了したら、次のコマンドを実行して、キュー内のメッセージの数を確認します。</span><span class="sxs-lookup"><span data-stu-id="372c7-144">Once it's finished, run the following command to see how many messages are in the queue:</span></span>

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

## <a name="write-code-that-receives-a-message-from-the-queue"></a><span data-ttu-id="372c7-145">キューからメッセージを受信するコードを記述する</span><span class="sxs-lookup"><span data-stu-id="372c7-145">Write code that receives a message from the queue</span></span>

1. <span data-ttu-id="372c7-146">エディターで **privatemessagereceiver/Program.cs** を開く</span><span class="sxs-lookup"><span data-stu-id="372c7-146">Open **privatemessagereceiver/Program.cs** in the editor</span></span>

1. <span data-ttu-id="372c7-147">`ReceiveSalesMessageAsync()` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="372c7-147">Locate the `ReceiveSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="372c7-148">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="372c7-148">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="372c7-149">キュー クライアントを作成するには、その行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-149">To create a queue client, replace that line with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="372c7-150">`RegisterMessageHandler()` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="372c7-150">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="372c7-151">メッセージ処理オプションを構成するには、そのメソッド内のすべてのコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-151">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="372c7-152">メッセージ ハンドラーを登録するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="372c7-152">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="372c7-153">`ProcessMessagesAsync()` メソッドを探します。</span><span class="sxs-lookup"><span data-stu-id="372c7-153">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="372c7-154">このメソッドを、受信メッセージを処理するメソッドとして登録しました。</span><span class="sxs-lookup"><span data-stu-id="372c7-154">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="372c7-155">受信メッセージをコンソールに表示するには、そのメソッド内のすべてのコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-155">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="372c7-156">受信したメッセージをキューから削除するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="372c7-156">To remove the received message from the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="372c7-157">`ReceiveSalesMessageAsync()` メソッドに戻り、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="372c7-157">Return to the `ReceiveSalesMessageAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the queue here
    ```

1. <span data-ttu-id="372c7-158">Service Bus への接続を終了するには、その行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="372c7-158">To close the connection to Service Bus, replace that line with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="372c7-159">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="372c7-159">Save the file.</span></span>

## <a name="retrieve-a-message-from-the-queue"></a><span data-ttu-id="372c7-160">キューからメッセージを取得する</span><span class="sxs-lookup"><span data-stu-id="372c7-160">Retrieve a message from the queue</span></span>

<span data-ttu-id="372c7-161">販売に関するメッセージを送信するコンポーネントを実行するには、Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="372c7-161">To run the component that sends a message about a sale, run this command in the Cloud Shell:</span></span>

```bash
dotnet run -p privatemessagereceiver
```

<span data-ttu-id="372c7-162">メッセージが受信されてコンソールに表示されていることを確認したら、`Enter` キーを押してアプリを停止します。</span><span class="sxs-lookup"><span data-stu-id="372c7-162">When you see that the message has been received and displayed in the console, press `Enter` to stop the app.</span></span> <span data-ttu-id="372c7-163">次に、前と同じコマンドを実行し、キューからすべてのメッセージが削除されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="372c7-163">Then, run the same command as before to confirm that all of the messages have been removed from the queue:</span></span>

```azurecli
az servicebus queue show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

<span data-ttu-id="372c7-164">すべてのメッセージが削除されている場合は、`0` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="372c7-164">This will show `0` if all the messages have been removed.</span></span>

<span data-ttu-id="372c7-165">個々の販売に関するメッセージを Service Bus キューに送信するコードを記述しました。</span><span class="sxs-lookup"><span data-stu-id="372c7-165">You have written code that sends a message about individual sales to a Service Bus queue.</span></span> <span data-ttu-id="372c7-166">営業部門の分散アプリケーションでは、販売担当者がデバイスで使用するモバイル アプリにこのコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="372c7-166">In the sales force distributed application, you should write this code in the mobile app that sales personnel use on devices.</span></span>

<span data-ttu-id="372c7-167">Service Bus キューからメッセージを受信するコードも記述しました。</span><span class="sxs-lookup"><span data-stu-id="372c7-167">You have also written code that receives a message from the Service Bus queue.</span></span> <span data-ttu-id="372c7-168">営業部門の分散アプリケーションでは、Azure で実行されて受信メッセージを処理する Web サービスにこのコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="372c7-168">In the sales force distributed application, you should write this code in the web service that runs in Azure and processes received messages.</span></span>
