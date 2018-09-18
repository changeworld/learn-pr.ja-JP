<span data-ttu-id="a8c51-101">販売担当者が使用しているモバイル アプリと、Azure SQL Database インスタンスに各販売に関する詳細情報を格納する、Azure でホストされた Web サービスの間で、Service Bus キューを使用して、個々の販売に関するメッセージを交換することを選択しました。</span><span class="sxs-lookup"><span data-stu-id="a8c51-101">You've chosen to use a Service Bus queue to exchange messages about individual sales between the mobile app that your sales personnel use and the web service, hosted in Azure, that will store details about each sale in an Azure SQL Database instance.</span></span>

<span data-ttu-id="a8c51-102">必要なオブジェクトは Azure サブスクリプションに既に実装してあります。</span><span class="sxs-lookup"><span data-stu-id="a8c51-102">You've already implemented the necessary objects in your Azure subscription.</span></span> <span data-ttu-id="a8c51-103">ここでは、そのキューにメッセージを送信したり、キューからメッセージを取得したりするコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-103">Now, you want to write code that sends messages to that queue and retrieves messages.</span></span>

## <a name="clone-and-open-the-starter-application"></a><span data-ttu-id="a8c51-104">スターター アプリケーションを複製して開く</span><span class="sxs-lookup"><span data-stu-id="a8c51-104">Clone and open the starter application</span></span>

<span data-ttu-id="a8c51-105">このユニットでは、**Visual Studio Code** で 2 つのコンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-105">In this unit, you'll build two console applications in **Visual Studio Code**.</span></span> <span data-ttu-id="a8c51-106">1 つ目のアプリケーションは Service Bus キューにメッセージを格納し、2 つ目はそれらを取得します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-106">The first application places messages into a Service Bus queue and the second retrieves them.</span></span> <span data-ttu-id="a8c51-107">これらのアプリケーションは、1 つの .NET Core ソリューションの一部です。</span><span class="sxs-lookup"><span data-stu-id="a8c51-107">The applications are part of a single .NET Core solution.</span></span> 

<span data-ttu-id="a8c51-108">最初にソリューションを複製します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-108">Start by cloning the solution:</span></span>

1. <span data-ttu-id="a8c51-109">コマンド プロンプトを開始し、アプリケーションのソース コードをホストするディレクトリに変更します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-109">Start a command prompt and change to the directory where you want to host the source code for the application.</span></span>

1. <span data-ttu-id="a8c51-110">次のコマンドを入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-110">Type the following command, and then press **Enter**:</span></span>

    ```powershell
    git clone https:\\ <!-- TODO: (add git URL) -->
    ```

1. <span data-ttu-id="a8c51-111">複製操作が完了したら、スターター フォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-111">When the clone operation is complete, change to the starter folder.</span></span>

1. <span data-ttu-id="a8c51-112">次のコマンドを入力して、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-112">Type the following command, and then press **Enter**.</span></span>

    ```powershell
    code .
    ```

1. <span data-ttu-id="a8c51-113">依存関係を復元するかどうかを確認するメッセージが表示された場合は、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-113">If a message appears asking if you want to restore dependencies, click **Yes**.</span></span>

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a><span data-ttu-id="a8c51-114">Service Bus 名前空間への接続文字列を構成する</span><span class="sxs-lookup"><span data-stu-id="a8c51-114">Configure a connection string to a Service Bus namespace</span></span>

<span data-ttu-id="a8c51-115">Service Bus 名前空間にアクセスしてキューを使用するには、コンソール アプリで次の 2 つの情報を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8c51-115">In order to access a Service Bus namespace and use a queue, you must configure two pieces of information in your console apps:</span></span>

* <span data-ttu-id="a8c51-116">名前空間のエンドポイント</span><span class="sxs-lookup"><span data-stu-id="a8c51-116">The endpoint for your namespace</span></span>
* <span data-ttu-id="a8c51-117">認証のための共有アクセス キー</span><span class="sxs-lookup"><span data-stu-id="a8c51-117">The shared access key for authentication</span></span>

<span data-ttu-id="a8c51-118">どちらの値も、完全な接続文字列の形式で Azure portal から取得できます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-118">Both of these values can be obtained from the Azure portal in the form of a complete connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="a8c51-119">わかりやすくするため、ここでは、両方のコンソール アプリケーションの **Program.cs** ファイルに接続文字列をハード コーディングします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-119">For simplicity, you will hard-code the connection string in the **Program.cs** file of both console applications.</span></span> <span data-ttu-id="a8c51-120">運用アプリケーションでは、構成ファイルまたは Azure Key Vault を使用して接続文字列を格納できます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-120">In a production application, you might use a configuration file or even Azure Key Vault to store the connection string.</span></span>

1. <span data-ttu-id="a8c51-121">Azure portal に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-121">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="a8c51-122">ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-122">In the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="a8c51-123">**[設定]** の **[共有アクセス ポリシー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-123">Under **SETTINGS**, click **Shared Access Policies**.</span></span>

1. <span data-ttu-id="a8c51-124">ポリシーの一覧で、**RootManageSharedAccessKey** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-124">In the list of policies, click **RootManageSharedAccessKey**.</span></span>

1. <span data-ttu-id="a8c51-125">**[プライマリ接続文字列]** テキスト ボックスの右側にある **[クリックしてコピー]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-125">To the right of the **Primary Connection string** text box, click the **Click to copy** button.</span></span>

1. <span data-ttu-id="a8c51-126">**Visual Studio Code** に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-126">Switch to **Visual Studio Code**.</span></span>

1. <span data-ttu-id="a8c51-127">**[エクスプローラー]** ウィンドウで、**privatemessagesender** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-127">In the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="a8c51-128">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-128">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="a8c51-129">引用符の間にカーソルを置き、**Ctrl + V** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-129">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="a8c51-130">**[エクスプローラー]** ウィンドウで、**privatemessagereceiver** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-130">In the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="a8c51-131">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-131">Locate the following line of code:</span></span>

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. <span data-ttu-id="a8c51-132">引用符の間にカーソルを置き、**Ctrl + V** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-132">Place the cursor between the quotation marks, and then press **Ctrl+V**.</span></span>

1. <span data-ttu-id="a8c51-133">**[ファイル]**、**[すべて保存]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-133">Click **File**, and then click **Save All**.</span></span>

1. <span data-ttu-id="a8c51-134">開いているすべてのエディター ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-134">Close all open editor windows.</span></span>

## <a name="write-code-that-sends-a-message-to-the-queue"></a><span data-ttu-id="a8c51-135">キューにメッセージを送信するコードを記述する</span><span class="sxs-lookup"><span data-stu-id="a8c51-135">Write code that sends a message to the queue</span></span>

<span data-ttu-id="a8c51-136">販売に関するメッセージを送信するコンポーネントを完成させるには、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-136">To complete the component that sends messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="a8c51-137">Visual Studio Code の **[エクスプローラー]** ウィンドウで、**privatemessagesender** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-137">In Visual Studio Code, in the **Explorer** pane, in the **privatemessagesender** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="a8c51-138">`SendSalesMessageAsync()` メソッドを探します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-138">Locate the `SendSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="a8c51-139">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-139">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="a8c51-140">キュー クライアントを作成するには、そのコード行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-140">To create a queue client, replace that line of code with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="a8c51-141">`try...catch` ブロック内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-141">Within the `try...catch` block, locate the following line of code:</span></span>

    ```C#
    // Create and send a message here
    ```

1. <span data-ttu-id="a8c51-142">キューに対するメッセージを作成して書式設定するには、そのコード行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-142">To create and format a message for the queue, replace that line of code with the following code:</span></span>

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. <span data-ttu-id="a8c51-143">コンソールにメッセージを表示するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-143">To display the message in the console, on the next line, add the following code:</span></span>

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. <span data-ttu-id="a8c51-144">キューにメッセージを送信するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-144">To send the message to the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.SendAsync(message);
    ```

1. <span data-ttu-id="a8c51-145">次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-145">Locate the following line of code:</span></span>

    ```C#
    // Close the connection to the queue here
    ```

1. <span data-ttu-id="a8c51-146">Service Bus の接続を閉じるには、そのコード行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-146">To close the connection the Service Bus, replace that line of code with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="a8c51-147">Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-147">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="send-a-message-to-the-queue"></a><span data-ttu-id="a8c51-148">メッセージをキューに送信する</span><span class="sxs-lookup"><span data-stu-id="a8c51-148">Send a message to the queue</span></span>

<span data-ttu-id="a8c51-149">販売に関するメッセージを送信するコンポーネントを実行するには、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-149">To run the component that sends a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="a8c51-150">Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-150">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="a8c51-151">**[デバッグ]** ウィンドウのドロップダウン リストで、**[Private Message Sender を起動]** を選択して、**F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-151">In the **Debug** pane, in the drop-down list, select **Launch Private Message Sender**, and then press **F5**.</span></span> <span data-ttu-id="a8c51-152">Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-152">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="a8c51-153">プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-153">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="a8c51-154">Azure portal に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-154">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="a8c51-155">Service Bus の名前空間が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-155">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="a8c51-156">**[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[キュー]** をクリックし、**salesmessages** キューをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-156">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue.</span></span> <span data-ttu-id="a8c51-157">**[アクティブなメッセージ数]** で、1 つのメッセージがキューに追加されたことが示されます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-157">The **ACTIVE MESSAGE COUNT** should indicate that one message has been added to the queue.</span></span>

## <a name="write-code-that-receives-a-message-from-the-queue"></a><span data-ttu-id="a8c51-158">キューからメッセージを受信するコードを記述する</span><span class="sxs-lookup"><span data-stu-id="a8c51-158">Write code that receives a message from the queue</span></span>

<span data-ttu-id="a8c51-159">販売に関するメッセージを取得するコンポーネントを完成させるには、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-159">To complete the component that retrieves messages about sales, follow these steps:</span></span>

1. <span data-ttu-id="a8c51-160">Visual Studio Code の **[エクスプローラー]** ウィンドウで、**privatemessagereceiver** フォルダーの **Program.cs** ファイルをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-160">In Visual Studio Code, in the **Explorer** pane, in the **privatemessagereceiver** folder, click the **Program.cs** file.</span></span>

1. <span data-ttu-id="a8c51-161">`ReceiveSalesMessageAsync()` メソッドを探します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-161">Locate the `ReceiveSalesMessageAsync()` method.</span></span>

1. <span data-ttu-id="a8c51-162">そのメソッド内で、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-162">Within that method, locate the following line of code:</span></span>

    ```C#
    // Create a queue client here
    ```

1. <span data-ttu-id="a8c51-163">キュー クライアントを作成するには、その行を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-163">To create a queue client, replace that line with the following code:</span></span>

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. <span data-ttu-id="a8c51-164">`RegisterMessageHandler()` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-164">Locate the `RegisterMessageHandler()` method.</span></span>

1. <span data-ttu-id="a8c51-165">メッセージ処理オプションを構成するには、そのメソッド内のすべてのコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-165">To configure message handling options, replace all the code within that method with the following code:</span></span>

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. <span data-ttu-id="a8c51-166">メッセージ ハンドラーを登録するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-166">To register the message handler, on the next line, add the following code:</span></span>

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. <span data-ttu-id="a8c51-167">`ProcessMessagesAsync()` メソッドを探します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-167">Locate the `ProcessMessagesAsync()` method.</span></span> <span data-ttu-id="a8c51-168">このメソッドを、受信メッセージを処理するメソッドとして登録しました。</span><span class="sxs-lookup"><span data-stu-id="a8c51-168">You have registered this method as the one that handles incoming messages.</span></span>

1. <span data-ttu-id="a8c51-169">受信メッセージをコンソールに表示するには、そのメソッド内のすべてのコードを次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-169">To display incoming messages in the console, replace all the code within that method with the following code:</span></span>

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. <span data-ttu-id="a8c51-170">受信したメッセージをキューから削除するには、その次の行に、以下のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-170">To remove the received message from the queue, on the next line, add the following code:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. <span data-ttu-id="a8c51-171">`ReceiveSalesMessageAsync()` メソッドに戻り、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-171">Return to the `ReceiveSalesMessageAsync()` method and locate the following line of code:</span></span>

    ```C#
    // Close the queue here
    ```

1. <span data-ttu-id="a8c51-172">Service Bus への接続を終了するには、その行を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-172">To close the connection to Service Bus, replace that line with the following code:</span></span>

    ```C#
    await queueClient.CloseAsync();
    ```

1. <span data-ttu-id="a8c51-173">Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-173">In Visual Studio Code, close all editor windows and save all changed files.</span></span>

## <a name="retrieve-a-message-from-the-queue"></a><span data-ttu-id="a8c51-174">キューからメッセージを取得する</span><span class="sxs-lookup"><span data-stu-id="a8c51-174">Retrieve a message from the queue</span></span>

<span data-ttu-id="a8c51-175">販売に関するメッセージを取得するコンポーネントを実行するには、次の手順のようにします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-175">To run the component that retrieves a message about a sale, follow these steps:</span></span>

1. <span data-ttu-id="a8c51-176">Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-176">In Visual Studio Code, on the **View** menu, click **Debug**.</span></span>

1. <span data-ttu-id="a8c51-177">**[デバッグ]** ウィンドウのドロップダウン リストで、**[Private Message Receiver を起動]** を選択して、**F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="a8c51-177">In the **Debug** pane, in the drop-down list, select **Launch Private Message Receiver**, and then press **F5**.</span></span> <span data-ttu-id="a8c51-178">Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-178">Visual Studio Code builds and runs the console application in debugging mode.</span></span>

1. <span data-ttu-id="a8c51-179">プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-179">As the program executes, examine the messages in the **Debug Console**.</span></span>

1. <span data-ttu-id="a8c51-180">メッセージが受信されてコンソールに表示されたら、**[デバッグ]** メニューの **[デバッグの停止]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-180">When you see that the message has been received and displayed in the console, on the **Debug** menu, click **Stop Debugging**.</span></span>

1. <span data-ttu-id="a8c51-181">Azure portal に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-181">Switch to the Azure portal.</span></span>

1. <span data-ttu-id="a8c51-182">Service Bus の名前空間が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-182">If the Service Bus namespace is not displayed, in the home page, click **All Resources**, and then click the Service Bus namespace you created earlier.</span></span>

1. <span data-ttu-id="a8c51-183">**[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[キュー]** をクリックし、**salesmessages** キューをクリックします。</span><span class="sxs-lookup"><span data-stu-id="a8c51-183">In the **Service Bus Namespace** blade, under **ENTITIES**, click **Queues**, and then click the **salesmessages** queue.</span></span> <span data-ttu-id="a8c51-184">**[アクティブなメッセージ数]** で、メッセージがキューから削除されたことが示されます。</span><span class="sxs-lookup"><span data-stu-id="a8c51-184">The **ACTIVE MESSAGE COUNT** should indicate that the message has been removed from the queue.</span></span>

<span data-ttu-id="a8c51-185">個々の販売に関するメッセージを Service Bus キューに送信するコードを記述しました。</span><span class="sxs-lookup"><span data-stu-id="a8c51-185">You have written code that sends a message about individual sales to a Service Bus queue.</span></span> <span data-ttu-id="a8c51-186">営業部門の分散アプリケーションでは、販売担当者がデバイスで使用するモバイル アプリにこのコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8c51-186">In the sales force distributed application, you should write this code in the mobile app that sales personnel use on devices.</span></span>

<span data-ttu-id="a8c51-187">Service Bus キューからメッセージを受信するコードも記述しました。</span><span class="sxs-lookup"><span data-stu-id="a8c51-187">You have also written code that receives a message from the Service Bus queue.</span></span> <span data-ttu-id="a8c51-188">営業部門の分散アプリケーションでは、Azure で実行されて受信メッセージを処理する Web サービスにこのコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8c51-188">In the sales force distributed application, you should write this code in the web service that runs in Azure and processes received messages.</span></span>
