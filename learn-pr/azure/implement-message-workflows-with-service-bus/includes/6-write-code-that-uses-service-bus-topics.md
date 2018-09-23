分散アプリケーションでは、一部のメッセージは 1 つの受信側コンポーネントに対して使用される必要があります。 他のメッセージは、複数の宛先に到達する必要があります。

ユーザーがピザの注文をキャンセルしたときの動作について説明します。 これは、最初に注文するときとは若干異なります。 注文の場合は、支払い処理が済むまで待ってから、他のステップ (最寄りの店での準備と調理など) に注文を送る必要があります。 しかし、キャンセル操作の場合は、店と支払いプロセッサの "*両方*" に同時に通知します。 このようにすることで、材料や配達ドライバーの時間が無駄になる可能性を最小限に抑えます。

複数のコンポーネントが同じメッセージを受信できるように、Azure Service Bus トピックを使用します。

## <a name="code-with-topics-vs-code-with-queues"></a>トピックを含むコードと、キューを含むコードの違い

トピックに送信されたすべてのメッセージを、サブスクライブしているすべてのコンポーネントに配信する場合はトピックを使用します。 トピックを使用するコードの記述は、キューを置き換える方法の 1 つです。 同じ **Microsoft.Azure.ServiceBus** NuGet パッケージを使用し、接続文字列を構成して、非同期プログラミング パターンを使用します。

ただし、メッセージを送信する `QueueClient` クラスおよびメッセージを受信する `SubscriptionClient` クラスの代わりに、`TopicClient` クラスを使用します。

## <a name="setting-filters-on-subscriptions"></a>サブスクリプションでのフィルターの設定

トピックに送信されたどのメッセージをどのサブスクリプションに配信するかを制御したい場合は、トピックの各サブスクリプションにフィルターを配置できます。 たとえば、ピザ アプリケーションでは、店でユニバーサル Windows プラットフォーム (UWP) アプリケーションが実行されています。 各店では、"OrderCancellation" トピックをサブスクライブし、その店の StoreId でフィルター処理します。 離れた場所にある店に不要なメッセージが送信されないので、インターネットの帯域幅が節約されます。 一方、支払い処理コンポーネントは、すべてのキャンセル メッセージをサブスクライブします。

次の 3 種類のフィルターのいずれかを使用できます。

- **ブール フィルター。** `TrueFilter` は、トピックに送信されたすべてのメッセージが現在のサブスクリプションに配信されるようにします。 `FalseFilter` は、現在のサブスクリプションにメッセージがまったく配信されないようにします (これは、サブスクリプションを効果的にブロックまたはオフにします)。
- **SQL フィルター。** SQL フィルターは、SQL クエリの `WHERE` 句と同じ構文を使用して条件を指定します。 このサブスクリプションに対して評価されたときに `True` を返すメッセージのみが、サブスクライバーに配信されます。
- **相関関係フィルター。** 相関関係フィルターは、各メッセージのプロパティと照合される条件のセットを保持します。 フィルターのプロパティとメッセージのプロパティが同じ値である場合、一致するものと見なされます。

この例の StoreId フィルターでは、SQL フィルターを使用することも "*できました*"。 SQL フィルターは最も柔軟性がありますが、計算の負荷は最も大きく、Service Bus のスループットが低下する可能性があります。 そこで、相関関係フィルターを代わりに選択します。 

## <a name="topicclient-example"></a>TopicClient の例

すべての送信または受信コンポーネントで、Service Bus トピックを呼び出すコード ファイルに次の `using` ステートメントを追加する必要があります。

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

メッセージを送信するには、最初に新しい `TopicClient` オブジェクトを作成し、接続文字列とトピックの名前を渡します。

```C#
topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
```

`TopicClient.SendAsync()` メソッドを呼び出してメッセージを渡すことにより、トピックにメッセージを送信できます。 キューの場合と同じように、メッセージは UTF-8 でエンコードされた文字列の形式にする必要があります。

```C#
string message = "Cancel! I can't believe you use canned mushrooms!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await topicClient.SendAsync(encodedMessage);
```

メッセージを受信するには、`TopicClient` オブジェクトではなく `SubscriptionClient` オブジェクトを作成し、それに接続文字列、トピックの名前、サブスクリプションの名前を**すべて**渡す必要があります。

```C#
subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, "GroupMessageTopic", "NorthAmerica");
```

その後、メッセージ ハンドラーを登録します。これは、取得したメッセージを処理するコード内の非同期メソッドです。

```C#
subscriptionClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

メッセージ ハンドラー内では、`SubscriptionClient.CompleteAsync()` メソッドを呼び出してキューからメッセージを削除します。

```C#
await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
```