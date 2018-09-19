分散アプリケーションでは、Service Bus キューなどのキューを、送信先コンポーネントへの配信を待機しているメッセージのための一時保管場所として使用します。 キューを介してメッセージを送受信するには、送信元コンポーネントと送信先コンポーネントでコードを記述する必要があります。

Contoso Slices アプリケーションについて考えます。 顧客は Web サイトまたはモバイル アプリを使用して注文します。 Web サイトとモバイル アプリは顧客のデバイス上で実行されるため、一度に送られてくる注文件数に制限はありません。 モバイル アプリと Web サイトでキューに注文をためることで、バックエンド コンポーネント (Web アプリ) は自分のペースでそのキューから注文を処理することができます。

Contoso Slices アプリケーションは実際には複数のステップで新しい注文を処理します。 ただし、そのすべては最初の支払いの承認に依存するので、キューを使用することに決めます。 受信コンポーネントの最初のジョブは、支払いの処理です。

モバイル アプリと Web サイトでは、キューにメッセージを追加するコードを記述する必要があります。 バックエンドの Web アプリでは、キューからメッセージを取り出すコードを記述します。

ここでは、そのコードを記述する方法を学習します。

## <a name="the-microsoftazureservicebus-nuget-package"></a>Microsoft.Azure.ServiceBus NuGet パッケージ

Service Bus を通してメッセージを送受信するコードを簡単に記述できるように、.NET クラスのライブラリが用意されています。このライブラリを任意の .NET Framework 言語で使用して、Service Bus のキュー、トピック、リレーと対話できます。 **Microsoft.Azure.ServiceBus** NuGet パッケージを追加することで、アプリケーションにこのライブラリを組み込むことができます。

キューに関してこのライブラリで最も重要なクラスは、`QueueClient` クラスです。 最初に、送信と受信両方のコンポーネントで、このクラスをインスタンス化する必要があります。

## <a name="connection-strings-and-keys"></a>接続文字列とキー

Service Bus 名前空間内のキューに接続するには、送信元コンポーネントと送信先コンポーネントの両方に 2 つの情報が必要です。

- Service Bus 名前空間の場所。**エンドポイント**とも呼ばれます。 場所は、**servicebus.windows.net** ドメイン内の完全修飾ドメイン名で指定されます。 例: **pizzaService.servicebus.windows.net**。
- アクセス キー。 Service Bus では、アクセス キーを要求することによってキュー、トピック、リレーへのアクセスが制限されます。

両方の情報は、接続文字列の形式で `QueueClient` オブジェクトに提供されます。 名前空間に対する正しい接続文字列は、Azure portal から取得できます。

## <a name="calling-methods-asynchronously"></a>メソッドの非同期呼び出し

Azure のキューは、送信側と受信側のコンポーネントから何千キロも離れた場所に存在する可能性があります。 物理的に近くても、遅い接続や帯域幅の競合のため、コンポーネントがキューのメソッドを呼び出したときに遅延が発生することがあります。 このため、Service Bus クライアント ライブラリではキューとの対話に `async` メソッドを使用できるようになっています。 ここでは、これらのメソッドを使用して、呼び出しが完了するのを待つ間にスレッドがブロックされるのを防ぎます。

たとえば、キューにメッセージを送信するときは、`await` キーワードを指定して `QueueClient.SendAsync()` メソッドを使用します。

## <a name="write-code-that-sends-to-queues"></a>キューに送信するコードを記述する

すべての送信または受信コンポーネントで、Service Bus キューを呼び出すコード ファイルに次の `using` ステートメントを追加する必要があります。

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

次に、新しい `QueueClient` オブジェクトを作成し、接続文字列とキューの名前を渡します。

```C#
queueClient = new QueueClient(TextAppConnectionString, "PrivateMessageQueue");
```

`QueueClient.SendAsync()` メソッドを呼び出して、UTF-8 エンコード文字列の形式でメッセージを渡すことにより、キューにメッセージを送信できます。

```C#
string message = "Sure would like a large pepperoni!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await queueClient.SendAsync(encodedMessage);
```

## <a name="receive-messages-from-queue"></a>キューからメッセージを受信する

メッセージを受信するには、最初にメッセージ ハンドラーを登録する必要があります。メッセージ ハンドラーはコード内のメソッドであり、キューでメッセージが利用可能になると呼び出されます。

```C#
queueClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

必要な処理を行います。 その後、メッセージ ハンドラー内で、`QueueClient.CompleteAsync()` メソッドを呼び出してキューからメッセージを削除します。

```C#
await queueClient.CompleteAsync(message.SystemProperties.LockToken);
```