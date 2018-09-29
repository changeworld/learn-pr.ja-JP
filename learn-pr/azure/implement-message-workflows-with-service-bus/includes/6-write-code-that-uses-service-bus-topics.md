<span data-ttu-id="fd8ce-101">分散アプリケーションでは、一部のメッセージは 1 つの受信側コンポーネントに対して使用される必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-101">In a distributed application, some messages need to be sent to a single recipient component.</span></span> <span data-ttu-id="fd8ce-102">他のメッセージは、複数の宛先に到達する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-102">Other messages need to reach more than one destination.</span></span>

<span data-ttu-id="fd8ce-103">ユーザーがピザの注文をキャンセルしたときの動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-103">Let's discuss what happens when a user cancels a pizza order.</span></span> <span data-ttu-id="fd8ce-104">これは、最初に注文するときとは若干異なります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-104">This is a little different than placing the initial order.</span></span> <span data-ttu-id="fd8ce-105">注文の場合は、支払い処理が済むまで待ってから、他のステップ (最寄りの店での準備と調理など) に注文を送る必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-105">In that case, we wanted to wait until the order cleared payment processing before sending the order on to other steps (like having it prepared and cooked at the local storefront).</span></span> <span data-ttu-id="fd8ce-106">しかし、キャンセル操作の場合は、店と支払いプロセッサの "*両方*" に同時に通知します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-106">But for the cancel operation, we are going to notify both the storefront *and* the payment processor at the same time.</span></span> <span data-ttu-id="fd8ce-107">このようにすることで、材料や配達ドライバーの時間が無駄になる可能性を最小限に抑えます。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-107">This approach minimizes the chances that we waste ingredients or delivery driver time.</span></span>

<span data-ttu-id="fd8ce-108">複数のコンポーネントが同じメッセージを受信できるように、Azure Service Bus トピックを使用します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-108">To allow multiple components to receive the same message, we'll use an Azure Service Bus topic.</span></span>

## <a name="code-with-topics-vs-code-with-queues"></a><span data-ttu-id="fd8ce-109">トピックを含むコードと、キューを含むコードの違い</span><span class="sxs-lookup"><span data-stu-id="fd8ce-109">Code with topics vs. code with queues</span></span>

<span data-ttu-id="fd8ce-110">トピックに送信されたすべてのメッセージを、サブスクライブしているすべてのコンポーネントに配信する場合はトピックを使用します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-110">If you want every message sent to the topic to be delivered to all subscribing components, you'll use topics.</span></span> <span data-ttu-id="fd8ce-111">トピックを使用するコードの記述は、キューを置き換える方法の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-111">Writing code that uses topics is a way to replace queues.</span></span> <span data-ttu-id="fd8ce-112">同じ **Microsoft.Azure.ServiceBus** NuGet パッケージを使用し、接続文字列を構成して、非同期プログラミング パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-112">You use the same **Microsoft.Azure.ServiceBus** NuGet package, configure connection strings, and use asynchronous programming patterns.</span></span>

<span data-ttu-id="fd8ce-113">ただし、メッセージを送信する `QueueClient` クラスおよびメッセージを受信する `SubscriptionClient` クラスの代わりに、`TopicClient` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-113">However, you use the `TopicClient` class instead of the `QueueClient` class to send messages and the `SubscriptionClient` class to receive messages.</span></span>

## <a name="setting-filters-on-subscriptions"></a><span data-ttu-id="fd8ce-114">サブスクリプションでのフィルターの設定</span><span class="sxs-lookup"><span data-stu-id="fd8ce-114">Setting filters on subscriptions</span></span>

<span data-ttu-id="fd8ce-115">トピックに送信されたどのメッセージをどのサブスクリプションに配信するかを制御したい場合は、トピックの各サブスクリプションにフィルターを配置できます。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-115">If you want to control which messages sent to the topic are delivered to which subscriptions, you can place filters on each subscription in the topic.</span></span> <span data-ttu-id="fd8ce-116">たとえば、ピザ アプリケーションでは、店でユニバーサル Windows プラットフォーム (UWP) アプリケーションが実行されています。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-116">In the pizza application, for instance, our storefronts are running Universal Windows Platform (UWP) applications.</span></span> <span data-ttu-id="fd8ce-117">各店では、"OrderCancellation" トピックをサブスクライブし、その店の StoreId でフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-117">Each store can subscribe to the "OrderCancellation" topic but filter for its own StoreId.</span></span> <span data-ttu-id="fd8ce-118">離れた場所にある店に不要なメッセージが送信されないので、インターネットの帯域幅が節約されます。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-118">We save internet bandwidth because we are not sending unnecessary messages to distant store locations.</span></span> <span data-ttu-id="fd8ce-119">一方、支払い処理コンポーネントは、すべてのキャンセル メッセージをサブスクライブします。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-119">Meanwhile, the payment processing component subscribes to all our cancellation messages.</span></span>

<span data-ttu-id="fd8ce-120">次の 3 種類のフィルターのいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-120">Filters can be one of three types:</span></span>

- <span data-ttu-id="fd8ce-121">**ブール フィルター。**</span><span class="sxs-lookup"><span data-stu-id="fd8ce-121">**Boolean Filters.**</span></span> <span data-ttu-id="fd8ce-122">`TrueFilter` は、トピックに送信されたすべてのメッセージが現在のサブスクリプションに配信されるようにします。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-122">The `TrueFilter` ensures that all messages sent to the topic are delivered to the current subscription.</span></span> <span data-ttu-id="fd8ce-123">`FalseFilter` は、現在のサブスクリプションにメッセージがまったく配信されないようにします</span><span class="sxs-lookup"><span data-stu-id="fd8ce-123">The `FalseFilter` ensures that none of the messages are delivered to the current subscription.</span></span> <span data-ttu-id="fd8ce-124">(これは、サブスクリプションを効果的にブロックまたはオフにします)。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-124">(This effectively blocks or switches off the subscription.)</span></span>
- <span data-ttu-id="fd8ce-125">**SQL フィルター。**</span><span class="sxs-lookup"><span data-stu-id="fd8ce-125">**SQL Filters.**</span></span> <span data-ttu-id="fd8ce-126">SQL フィルターは、SQL クエリの `WHERE` 句と同じ構文を使用して条件を指定します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-126">A SQL filter specifies a condition by using the same syntax as a `WHERE` clause in a SQL query.</span></span> <span data-ttu-id="fd8ce-127">このサブスクリプションに対して評価されたときに `True` を返すメッセージのみが、サブスクライバーに配信されます。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-127">Only messages that return `True` when evaluated against this subscription will be delivered to the subscribers.</span></span>
- <span data-ttu-id="fd8ce-128">**相関関係フィルター。**</span><span class="sxs-lookup"><span data-stu-id="fd8ce-128">**Correlation Filters.**</span></span> <span data-ttu-id="fd8ce-129">相関関係フィルターは、各メッセージのプロパティと照合される条件のセットを保持します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-129">A correlation filter holds a set of conditions that are matched against the properties of each message.</span></span> <span data-ttu-id="fd8ce-130">フィルターのプロパティとメッセージのプロパティが同じ値である場合、一致するものと見なされます。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-130">If the property in the filter and the property on the message have the same value, it is considered a match.</span></span>

<span data-ttu-id="fd8ce-131">この例の StoreId フィルターでは、SQL フィルターを使用することも "*できました*"。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-131">For our StoreId filter, we *could* use a SQL filter.</span></span> <span data-ttu-id="fd8ce-132">SQL フィルターは最も柔軟性がありますが、計算の負荷は最も大きく、Service Bus のスループットが低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-132">Those filters are the most flexible, but they're also the most computationally expensive and could slow down our Service Bus throughput.</span></span> <span data-ttu-id="fd8ce-133">そこで、相関関係フィルターを代わりに選択します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-133">In this case, we choose a correlation filter instead.</span></span> 

## <a name="topicclient-example"></a><span data-ttu-id="fd8ce-134">TopicClient の例</span><span class="sxs-lookup"><span data-stu-id="fd8ce-134">TopicClient example</span></span>

<span data-ttu-id="fd8ce-135">すべての送信または受信コンポーネントで、Service Bus トピックを呼び出すコード ファイルに次の `using` ステートメントを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-135">In any sending or receiving component, you should add the following `using` statements to any code file that calls a Service Bus topic:</span></span>

```C#
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.ServiceBus;
```

<span data-ttu-id="fd8ce-136">メッセージを送信するには、最初に新しい `TopicClient` オブジェクトを作成し、接続文字列とトピックの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-136">To send a message, start by creating a new `TopicClient` object and pass it the connection string and the name of the topic:</span></span>

```C#
topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
```

<span data-ttu-id="fd8ce-137">`TopicClient.SendAsync()` メソッドを呼び出してメッセージを渡すことにより、トピックにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-137">You can send a message to the topic by calling the `TopicClient.SendAsync()` method and passing the message.</span></span> <span data-ttu-id="fd8ce-138">キューの場合と同じように、メッセージは UTF-8 でエンコードされた文字列の形式にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-138">As with queues, the message must be in the form of a UTF-8 encoded string:</span></span>

```C#
string message = "Cancel! I can't believe you use canned mushrooms!";
var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
await topicClient.SendAsync(encodedMessage);
```

<span data-ttu-id="fd8ce-139">メッセージを受信するには、`TopicClient` オブジェクトではなく `SubscriptionClient` オブジェクトを作成し、それに接続文字列、トピックの名前、サブスクリプションの名前を**すべて**渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-139">To receive messages, you must create a `SubscriptionClient` object, not a `TopicClient` object, and pass it the connection string, the name of the topic, **and** the name of the subscription:</span></span>

```C#
subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, "GroupMessageTopic", "NorthAmerica");
```

<span data-ttu-id="fd8ce-140">その後、メッセージ ハンドラーを登録します。これは、取得したメッセージを処理するコード内の非同期メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-140">Then register a message handler - this is the asynchronous method in your code that processes the retrieved message.</span></span>

```C#
subscriptionClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
```

<span data-ttu-id="fd8ce-141">メッセージ ハンドラー内では、`SubscriptionClient.CompleteAsync()` メソッドを呼び出してキューからメッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="fd8ce-141">Within the message handler, call the `SubscriptionClient.CompleteAsync()` method to remove the message from the queue:</span></span>

```C#
await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
```