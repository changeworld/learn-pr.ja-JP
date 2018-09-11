販売担当者が使用しているモバイル アプリと、Azure SQL Database に各販売に関する詳細情報を格納する、Azure でホストされた Web サービスの間で、Service Bus キューを使用して、個々の販売に関するメッセージを交換することを選択しました。

必要なオブジェクトは Azure サブスクリプションに既に実装してあります。 ここでは、そのキューにメッセージを送信したり、キューからメッセージを取得したりするコードを記述します。

## <a name="clone-and-open-the-starter-application"></a>スターター アプリケーションを複製して開く

このユニットでは、**Visual Studio Code** で 2 つのコンソール アプリケーションを作成します。 1 つ目のアプリケーションは Service Bus キューにメッセージを格納し、2 つ目はそれらを取得します。 これらのアプリケーションは、1 つの .NET Core ソリューションの一部です。 

最初にソリューションを複製します。

1. コマンド プロンプトを開始し、アプリケーションのソース コードをホストするディレクトリに変更します。

1. 次のコマンドを入力して、**Enter** キーを押します。

    ```powershell
    git clone https:\\ <!-- TODO: (add git URL) -->
    ```

1. 複製操作が完了したら、スターター フォルダーに変更します。

1. 次のコマンドを入力して、**Enter** キーを押します。

    ```powershell
    code .
    ```

1. 依存関係を復元するかどうかを確認するメッセージが表示された場合は、**[はい]** をクリックします。

## <a name="configure-a-connection-string-to-a-service-bus-namespace"></a>Service Bus 名前空間への接続文字列を構成する

Service Bus 名前空間にアクセスしてキューを使用するには、コンソール アプリで次の 2 つの情報を構成する必要があります。

* 名前空間のエンドポイント
* 認証のための共有アクセス キー

どちらの値も、完全な接続文字列の形式で Azure portal から取得できます。

> [!NOTE]
> わかりやすくするため、ここでは、両方のコンソール アプリケーションの **Program.cs** に接続文字列をハード コーディングします。 運用アプリケーションでは、構成ファイルまたは Azure Key Vault を使用して接続文字列を格納できます。

1. Azure portal に切り替えます。

1. ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。

1. **[設定]** の **[共有アクセス ポリシー]** をクリックします。

1. ポリシーの一覧で、**RootManageSharedAccessKey** をクリックします。

1. **[プライマリ接続文字列]** ボックスの右側にある **[クリックしてコピー]** ボタンをクリックします。

1. **Visual Studio Code** に切り替えます。

1. **[エクスプローラー]** ウィンドウで、**privatemessagesender** フォルダーの **Program.cs** ファイルをクリックします。

1. 次のコード行を見つけます。

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. 引用符の間にカーソルを置き、**Ctrl + V** キーを押します。

1. **[エクスプローラー]** ウィンドウで、**privatemessagereceiver** フォルダーの **Program.cs** ファイルをクリックします。

1. 次のコード行を見つけます。

    ```C#
    const string ServiceBusConnectionString = "";
    ```

1. 引用符の間にカーソルを置き、**Ctrl + V** キーを押します。

1. **[ファイル]**、**[すべて保存]** の順にクリックします。

1. 開いているすべてのエディター ウィンドウを閉じます。

## <a name="write-code-that-sends-a-message-to-the-queue"></a>キューにメッセージを送信するコードを記述する

販売に関するメッセージを送信するコンポーネントを完成させるには、次の手順のようにします。

1. Visual Studio Code の **[エクスプローラー]** ウィンドウで、**privatemessagesender** フォルダーの **Program.cs** ファイルをクリックします。

1. `SendSalesMessageAsync()` メソッドを探します。

1. そのメソッド内で、次のコード行を見つけます。

    ```C#
    // Create a Queue Client here
    ```

1. キュー クライアントを作成するには、そのコード行を次のコードに置き換えます。

    ```C#
    queueClient = new QueueClient(ServiceBusConnectionString, QueueName);
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

1. Service Bus の接続を閉じるには、そのコード行を次のコードに置き換えます。

    ```C#
    await queueClient.CloseAsync();
    ```

1. Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。

## <a name="send-a-message-to-the-queue"></a>メッセージをキューに送信する

販売に関するメッセージを送信するコンポーネントを実行するには、次の手順のようにします。

1. Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。

1. **[デバッグ]** ウィンドウのドロップダウン リストで、**[Launch Private Message Sender]\(プライベート メッセージ送信側の起動\)** を選択して、**F5** キーを押します。 Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。

1. プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。

1. Azure portal に切り替えます。

1. Service Bus が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。

1. **[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[キュー]** をクリックし、**salesmessages** キューをクリックします。 **[アクティブなメッセージ数]** で、1 つのメッセージがキューに追加されたことが示されます。

## <a name="write-code-that-receives-a-message-from-the-queue"></a>キューからメッセージを受信するコードを記述する

販売に関するメッセージを取得するコンポーネントを完成させるには、次の手順のようにします。

1. Visual Studio Code の **[エクスプローラー]** ウィンドウで、**privatemessagereceiver** フォルダーの **Program.cs** ファイルをクリックします。

1. `ReceiveSalesMessageAsync()` メソッドを探します。

1. そのメソッド内で、次のコード行を見つけます。

    ```C#
    // Create a Queue Client here
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

1. Service Bus への接続を閉じるには、そのコードを次のコードに置き換えます。

    ```C#
    await queueClient.CloseAsync();
    ```

1. Visual Studio Code で、すべてのエディター ウィンドウを閉じ、変更したすべてのファイルを保存します。

## <a name="retrieve-a-message-from-the-queue"></a>キューからメッセージを取得する

販売に関するメッセージを取得するコンポーネントを実行するには、次の手順のようにします。

1. Visual Studio Code の **[表示]** メニューで、**[デバッグ]** をクリックします。

1. **[デバッグ]** ウィンドウのドロップダウン リストで、**[Launch Private Message Receiver]\(プライベート メッセージ受信側の起動\)** を選択して、**F5** キーを押します。 Visual Studio Code でデバッグ モードのコンソール アプリケーションがビルドされて実行されます。

1. プログラムが実行されたら、**デバッグ コンソール**でメッセージを調べます。

1. メッセージが受信されてコンソールに表示されたら、**[デバッグ]** メニューの **[デバッグの停止]** をクリックします。

1. Azure portal に切り替えます。

1. Service Bus が表示されない場合は、ホーム ページで **[すべてのリソース]** をクリックし、先ほど作成した Service Bus 名前空間をクリックします。

1. **[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[キュー]** をクリックし、**salesmessages** キューをクリックします。 **[アクティブなメッセージ数]** で、メッセージがキューから削除されたことが示されます。

個々の販売に関するメッセージを Service Bus キューに送信するコードを記述しました。 営業部門の分散アプリケーションでは、販売担当者がデバイスで使用するモバイル アプリにこのコードを記述する必要があります。

Service Bus キューからメッセージを受信するコードも記述しました。 営業部門の分散アプリケーションでは、Azure で実行されて受信メッセージを処理する Web サービスにこのコードを記述する必要があります。
