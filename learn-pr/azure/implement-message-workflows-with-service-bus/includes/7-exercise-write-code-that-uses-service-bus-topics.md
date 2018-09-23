Azure Service Bus トピックを使用して、営業部門の分散アプリケーションで販売業績に関するメッセージを配信することにしました。 販売担当者が自分のモバイル デバイスで使用しているアプリが、各エリアと期間ごとの売り上げ高を集計したメッセージを送信します。 これらのメッセージは、南北アメリカやヨーロッパなど会社の営業地域内にある Web サービスに配信されます。

トピックやサブスクリプションなど、必要なインフラストラクチャは Azure サブスクリプションに既に実装してあります。 ここでは、トピックにメッセージを送信し、サブスクリプションからメッセージを取得するコードを記述します。

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Service Bus 名前空間への接続文字列を構成する

最初に、送信コンポーネントと受信コンポーネントの両方で接続文字列を構成します。

1. エディターで、**performancemessagesender/Program.cs** を開き、次のコード行を見つけます。

    ```C#
    const string ServiceBusConnectionString = "";
    ```

    引用符の間に接続文字列を貼り付け、"..." メニューまたはショートカット キー (Windows と Linux では <kbd>Ctrl + S</kbd>、macOS では <kbd>Cmd + S</kbd>) を使用して、ファイルを保存します。

1. **performancemessagereceiver/Program.cs** で前の手順を繰り返します。その場合、同じ接続文字列値に貼り付け、そのファイルを保存します。

## <a name="write-code-that-sends-a-message-to-the-topic"></a>トピックにメッセージを送信するコードを書き込みます

販売業績に関するメッセージを送信するコンポーネントを完成させるには、次の手順に従います。

1. エディターで **performancemessagesender/Program.cs** を開きます。

1. `SendPerformanceMessageAsync()` メソッドを探します。

1. そのメソッド内で、次のコード行を見つけます。

    ```C#
    // Create a Topic Client here
    ```

1. トピック クライアントを作成するには、そのコード行を次のコードで置き換えます。

    ```C#
    topicClient = new TopicClient(ServiceBusConnectionString, TopicName);
    ```

1. `try...catch` ブロック内で、次のコード行を見つけます。

    ```C#
    // Create and send a message here
    ```

1. キューに対するメッセージを作成して書式設定するには、そのコード行を次のコードに置き換えます。

    ```C#
    string messageBody = $"Total sales for Brazil in August: $13m.";
    var message = new Message(Encoding.UTF8.GetBytes(messageBody));
    ```

1. コンソールにメッセージを表示するには、その次の行に、以下のコードを追加します。

    ```C#
    Console.WriteLine($"Sending message: {messageBody}");
    ```

1. キューにメッセージを送信するには、その次の行に、以下のコードを追加します。

    ```C#
    await topicClient.SendAsync(message);
    ```

1. 次のコード行を見つけます。

    ```C#
    // Close the connection to the topic here
    ```

1. Service Bus への接続を閉じるには、そのコード行を次のコードで置き換えます。

    ```C#
    await topicClient.CloseAsync();
    ```

1. ファイルを保存します。

## <a name="send-a-message-to-the-topic"></a>メッセージをトピックに送信する

販売に関するメッセージを送信するコンポーネントを実行するには、Cloud Shell で次のコマンドを実行します。

```bash
dotnet run -p performancemessagesender
```

プログラムを実行すると、メッセージを送信していることを示すメッセージが表示されます。 アプリを実行するたびに、1 つのメッセージがトピックに追加され、各サブスクライバーがコピーを受信します。

終了したら、次のコマンドを実行して、Americas サブスクリプションのメッセージの数を確認します。

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

`Americas` の代わりに `EuropeAndAfrica` を使用している場合は、両方のサブスクリプションに同じ数のメッセージがあることがわかります。

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a>トピック サブスクリプションからメッセージを受信するコードを書き込む

販売業績に関するメッセージを取得するコンポーネントを完成させるには、次の手順に従います。

1. エディターで **performancemessagereceiver/Program.cs** を開きます。

1. `MainAsync()` メソッドを探します。

1. そのメソッド内で、次のコード行を見つけます。

    ```C#
    // Create a subscription client here
    ```

1. サブスクリプション クライアントを作成するには、その行を次のコードで置き換えます。

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. `RegisterMessageHandler()` メソッドを見つけます。

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
    subscriptionClient.RegisterMessageHandler(ProcessMessagesAsync, messageHandlerOptions);
    ```

1. `ProcessMessagesAsync()` メソッドを探します。 このメソッドを、受信メッセージを処理するメソッドとして登録しました。

1. 受信メッセージをコンソールに表示するには、そのメソッド内のすべてのコードを次のコードで置き換えます。

    ```C#
    Console.WriteLine($"Received sale performance message: SequenceNumber:{message.SystemProperties.SequenceNumber} Body:{Encoding.UTF8.GetString(message.Body)}");
    ```

1. サブスクリプションから受信したメッセージを削除するには、その次の行に、以下のコードを追加します。

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```

1. `MainAsync()` メソッドに戻り、次のコード行を見つけます。

    ```C#
    // Close the subscription here
    ```

1. Service Bus への接続を終了するには、そのコードを次のコードで置き換えます。

    ```C#
    await subscriptionClient.CloseAsync();
    ```

1. Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。

## <a name="retrieve-a-message-from-a-topic-subscription"></a>トピック サブスクリプションからメッセージを取得する

販売業績に関するメッセージを取得するコンポーネントを実行するには、次の手順に従います。

```bash
dotnet run -p performancemessagereceiver
```

メッセージを受信しているプログラムが通知の出力を停止したら、 `Enter` キーを押してアプリを停止します。 次に、前と同じコマンドを実行して、`Americas` サブスクリプションに残っているメッセージがないことを確認します。

```azurecli
az servicebus topic subscription show \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --namespace-name <namespace-name> \
    --topic-name salesperformancemessages \
    --name Americas \
    --query messageCount
```

`Americas` の代わりに `EuropeAndAfrica` を使用している場合は、メッセージの数が変化していないことがわかります。 アプリケーションは、`Americas` サブスクリプションからのみメッセージを受信していました。
