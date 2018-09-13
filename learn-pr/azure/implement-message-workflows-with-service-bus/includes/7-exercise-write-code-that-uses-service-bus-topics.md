Azure Service Bus トピックを使用して、営業部門の分散アプリケーションで販売業績に関するメッセージを配信することにしました。 販売担当者が自分のモバイル デバイスで使用しているアプリが、各エリアと期間ごとの売り上げ高を集計したメッセージを送信します。 これらのメッセージは、南北アメリカ、ヨーロッパなど会社の運用領域内にある web サービスに配信されます。

トピックとサブスクリプションを含む、Azure サブスクリプションで、必要なインフラストラクチャを既に実装しています。 ここでは、トピックにメッセージを送信し、各サブスクリプションからメッセージを収得するコードを書き込みます。

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Service Bus 名前空間への接続文字列を構成する

接続文字列を送信と受信コンポーネントの両方で構成することからはじめます。

1. Azure portal に切り替えます。

1. ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。

1. **[設定]** の **[共有アクセス ポリシー]** をクリックします。

1. ポリシーの一覧で、**RootManageSharedAccessKey** をクリックします。

1. **[プライマリ接続文字列]** テキスト　ボックスの右側にある **[クリックしてコピー]** ボタンをクリックします。

1. **Visual Studio Code** に切り替えます。

1. **[エクスプローラー]** ウィンドウで、**performancemessagesender** フォルダーの **Program.cs** ファイルをクリックします。

1. 次のコード行を見つけます。

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. 引用符の間にカーソルを置き、**Ctrl + V** キーを押します。

1. **[エクスプローラー]** ウィンドウで、**performancemessagereceiver** フォルダーの**Program.cs** ファイルをクリックします。

1. 次のコード行を見つけます。

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. 引用符の間にカーソルを置き、**Ctrl + V** キーを押します。

1. **[ファイル]**、**[すべて保存]** の順にクリックします。

1. 開いているすべてのエディター ウィンドウを閉じます。

## <a name="write-code-that-sends-a-message-to-the-topic"></a>トピックにメッセージを送信するコードを書き込みます

販売業績に関するメッセージを送信するコンポーネントを完成させるには、次の手順に従います。

1. Visual Studio Code の **[エクスプローラー]** ウィンドウで、**performancemessagesender** フォルダーの **Program.cs** ファイルをクリックします。

1. `SendPerformanceMessageAsync()` メソッドを見つけます。

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

1. キューに対するメッセージを作成してフォーマットするには、そのコード行を次のコードで置き換えます。

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

1. Service Bus の接続を閉じるには、そのコード行を次のコードで置き換えます。

    ```C#
    await topicClient.CloseAsync();
    ```

1. Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。

## <a name="send-a-message-to-the-topic"></a>メッセージをトピックに送信する

販売に関するメッセージを送信するコンポーネントを実行するには、次の手順に従います。

1. Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。

1. **[デバッグ]** ウィンドウのドロップダウン リストで、**[Launch Performance Message Sender]** を選択して、**F5** キーを押します。 Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。

1. プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。

1. Azure portal に切り替えます。

1. Service Bus が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。

1. **[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[トピック]** をクリックし、**salesperformancemessages** トピックをクリックします。 サブスクリプションの一覧では、**南北アメリカ**と**ヨーロッパ**サブスクリプションの両方に一つのメッセージが表示されます。

## <a name="write-code-that-receives-a-message-from-a-topic-subscription"></a>トピックサブスクリプションからメッセージを受信するコードを書き込む

販売業績に関するメッセージを取得するコンポーネントを完成させるには、次の手順に従います。

1. Visual Studio Code の **[エクスプローラー]** ウィンドウで、**performancemessagereceiver** フォルダーの **Program.cs** ファイルをクリックします。

1. `MainAsync()` メソッドを見つけます。

1. そのメソッド内で、次のコード行を見つけます。

    ```C#
    // Create a Subscription Client here
    ```

1. サブスクリプション クライアントを作成するには、その行を次のコードで置き換えます。

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, TopicName, SubscriptionName);
    ```

1. `RegisterMessageHandler()` メソッドを見つけます。

1. メッセージ処理オプションを構成するには、そのメソッド内のすべてのコードを次のコードで置き換えます。

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

1. `ProcessMessagesAsync()` メソッドを見つけます。 このメソッドを、受信メッセージを処理するメソッドとして登録しました。

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

1. Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。

1. **[デバッグ]** ウィンドウのドロップダウン リストで、**[Launch Performance Message Receiver]** を選択して、**F5** キーを押します。 Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。

1. プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。

1. メッセージが受信されてコンソールに表示されたら、**[デバッグ]** メニューの **[デバッグの停止]** をクリックします。

1. Azure portal に切り替えます。

1. Service Bus が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。

1. **[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[トピック]** をクリックし、**salesperformancemessages** トピックをクリックします。 サブスクリプションの一覧では、アプリケーションが処理され、唯一のメッセージが削除されるため、**南北アメリカ**サブスクリプションに表示されるメッセージはゼロであるはずです。 **ヨーロッパ**サブスクリプションにはメッセージがあることを確認してください。
