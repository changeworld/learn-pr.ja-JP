販売担当者が使用しているモバイル アプリと、Azure SQL Database インスタンスに各販売に関する詳細情報を格納する、Azure でホストされた Web サービスの間で、Service Bus キューを使用して、個々の販売に関するメッセージを交換することを選択しました。

必要なオブジェクトは Azure サブスクリプションに既に実装してあります。 ここでは、そのキューにメッセージを送信したり、キューからメッセージを取得したりするコードを記述します。

## <a name="clone-and-open-the-starter-application"></a>スターター アプリケーションを複製して開く

このユニットでは、2 つのコンソール アプリケーションをビルドします。 1 つ目のアプリケーションでメッセージを Service Bus キューに入れ、2 つ目のアプリケーションでそれらを取得します。 これらのアプリケーションは、単一の .NET Core ソリューションの一部です。

1. まず、ソリューションを複製します。その場合、Cloud Shell で以下のコマンドを実行します。

```bash
cd ~
git clone https://github.com/MicrosoftDocs/mslearn-connect-services-together.git
```

2. 次に、スターター フォルダーにディレクトリを変更し、Cloud Shell エディターを開きます。

```bash
cd mslearn-connect-services-together/implement-message-workflows-with-service-bus/src/start
code .
```

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Service Bus 名前空間への接続文字列を構成する

Service Bus 名前空間にアクセスしてキューを使用するには、コンソール アプリで次の 2 つの情報を構成する必要があります。

* 名前空間のエンドポイント
* 認証のための共有アクセス キー

どちらの値も、完全な接続文字列の形式で Azure portal から取得できます。

> [!NOTE]
> わかりやすくするため、ここでは、両方のコンソール アプリケーションの **Program.cs** ファイルに接続文字列をハード コーディングします。 運用アプリケーションでは、構成ファイルまたは Azure Key Vault を使用して接続文字列を格納できます。

1. Cloud Shell で、次のコマンドを実行して、ご利用の Service Bus 名前空間用のプライマリ接続文字列を表示します。 `<namespace-name>` を、実際の Service Bus 名前空間の名前に置き換えます。

    ```azurecli
    az servicebus namespace authorization-rule keys list \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --namespace-name <namespace-name> \
        --name RootManageSharedAccessKey \
        --query primaryConnectionString \
        --output tsv
    ```

    この接続文字列はこのモジュールを通して複数回必要になるため、すぐ使用できる場所に貼り付けてください。

1. Cloud Shell からキーをコピーします。 エディターで、**privatemessagesender/Program.cs** を開き、次のコード行を見つけます。

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    接続文字列を引用符の間に貼り付けます。 <kbd>Ctrl + S</kbd> キーを使用して、ファイルを保存します。

1. **privatemessagereceiver/Program.cs** で前の手順を繰り返します。その場合、同じ接続文字列値に貼り付けます。 [...] メニューまたはアクセラレータ キー (Windows と Linux の場合は <kbd>Ctrl + S</kbd>、macOS の場合は <kbd>Cmd + S</kbd>) を使用して、ファイルを保存します。

## <a name="write-code-that-sends-a-message-to-the-queue"></a>キューにメッセージを送信するコードを記述する

販売に関するメッセージを送信するコンポーネントを完成させるには、次の手順に従います。

1. エディターで **privatemessagesender/Program.cs** を開く

1. `SendSalesMessageAsync()` メソッドを検索します。

1. そのメソッド内で、次のコード行を見つけます。

    ```C#
    // Create a queue client here
    ```

1. キュー クライアントを作成するには、そのコード行を次のコードに置き換えます。

    ```C#
    var queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. `try...catch` ブロック内で、次のコード行を見つけます。

    ```C#
    // Create and send a message here
    ```

1. キューに対するメッセージを作成して書式設定するには、そのコード行を次のコードに置き換えます。

    ```C#
    string messageBody = $"$10,000 order for bicycle parts from retailer Adventure Works.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. コンソールにメッセージを表示するには、その次の行に、以下のコードを追加します。

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. キューにメッセージを送信するには、その次の行に、以下のコードを追加します。

    ```C#
    await queueClient.SendAsync(message);
    ```

1. 次のコード行を見つけます。

    ```C#
    // Close the connection to the queue here
    ```

1. Service Bus への接続を閉じるには、そのコード行を次のコードに置き換えます。

    ```C#
    await queueClient.CloseAsync();
    ```

1. ファイルを保存します。

## <a name="send-a-message-to-the-queue"></a>メッセージをキューに送信する

販売に関するメッセージを送信するコンポーネントを実行するには、Cloud Shell で次のコマンドを実行します。

```bash
dotnet run -p privatemessagesender
```

> [!NOTE]
> この演習中に実行するアプリは起動するまでしばらく時間がかかる場合があります。これは、`dotnet` でリモート ソースからパッケージを復元し、アプリを初回の実行時にビルドする必要があるためです。

プログラムを実行すると、メッセージを送信していることを示すメッセージが表示されます。 アプリを実行するたびに、1 つのメッセージがキューに追加されます。

終了したら、次のコマンドを実行して、キュー内のメッセージの数を確認します。

```azurecli
az servicebus queue show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

## <a name="write-code-that-receives-a-message-from-the-queue"></a>キューからメッセージを受信するコードを記述する

1. エディターで **privatemessagereceiver/Program.cs** を開く

1. `ReceiveSalesMessageAsync()` メソッドを検索します。

1. そのメソッド内で、次のコード行を見つけます。

    ```C#
    // Create a queue client here
    ```

1. キュー クライアントを作成するには、その行を次のコードに置き換えます。

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
    ```

1. `RegisterMessageHandler()` メソッドを検索します。

1. メッセージ処理オプションを構成するには、そのメソッド内のすべてのコードを次のコードに置き換えます。

    ```C#
    var messageHandlerOptions = new MessageHandlerOptions(ExceptionReceivedHandler)
    {
        MaxConcurrentCalls = 1,
        AutoComplete = false
    };
    ```

1. メッセージ ハンドラーを登録するには、その次の行に、以下のコードを追加します。

    ```C#
    queueClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. `ProcessMessagesAsync()` メソッドを探します。 このメソッドを、受信メッセージを処理するメソッドとして登録しました。

1. 受信メッセージをコンソールに表示するには、そのメソッド内のすべてのコードを次のコードに置き換えます。

    ```C#
    Console.WriteLine($"Received message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. 受信したメッセージをキューから削除するには、その次の行に、以下のコードを追加します。

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. `ReceiveSalesMessageAsync()` メソッドに戻り、次のコード行を見つけます。

    ```C#
    // Close the queue here
    ```

1. Service Bus への接続を終了するには、その行を次のコードで置き換えます。

    ```C#
    await queueClient.CloseAsync();
    ```

1. ファイルを保存します。

## <a name="retrieve-a-message-from-the-queue"></a>キューからメッセージを取得する

販売に関するメッセージを送信するコンポーネントを実行するには、Cloud Shell で次のコマンドを実行します。

```bash
dotnet run -p privatemessagereceiver
```

メッセージが受信されてコンソールに表示されていることを確認したら、`Enter` キーを押してアプリを停止します。 次に、前と同じコマンドを実行し、キューからすべてのメッセージが削除されたことを確認します。

```azurecli
az servicebus queue show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --name salesmessages \
    --query messageCount
```

すべてのメッセージが削除されている場合は、`0` が表示されます。

個々の販売に関するメッセージを Service Bus キューに送信するコードを記述しました。 営業部門の分散アプリケーションでは、販売担当者がデバイスで使用するモバイル アプリにこのコードを記述する必要があります。

Service Bus キューからメッセージを受信するコードも記述しました。 営業部門の分散アプリケーションでは、Azure で実行されて受信メッセージを処理する Web サービスにこのコードを記述する必要があります。
