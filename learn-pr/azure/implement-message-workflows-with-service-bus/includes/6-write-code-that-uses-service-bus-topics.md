<span data-ttu-id="ca9a9-101">分散アプリケーションでは、一部のメッセージは 1 つの受信側コンポーネントに対して使用される必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-101">In a distributed application, some messages need to be spent to a single recipient component.</span></span> <span data-ttu-id="ca9a9-102">他のメッセージは、複数の宛先に到達する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-102">Other messages need to reach more than one destination.</span></span>

<span data-ttu-id="ca9a9-103">ユーザーがピザの注文をキャンセルしたときの動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-103">Let's discuss what happens when a user cancels a pizza order.</span></span> <span data-ttu-id="ca9a9-104">これは、最初に注文するときとは若干異なります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-104">This is a little different than placing the initial order.</span></span> <span data-ttu-id="ca9a9-105">注文の場合は、支払い処理が済むまで待ってから、他のステップ (最寄りの店での準備と調理など) に注文を送る必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-105">In that case, we wanted to wait until the order cleared payment processing before sending the order on to other steps (like having it prepared and cooked at the local storefront).</span></span> <span data-ttu-id="ca9a9-106">しかし、キャンセル操作の場合は、店と支払いプロセッサの "*両方*" に同時に通知します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-106">But for the cancel operation, we are going to notify both the storefront *and* the payment processor at the same time.</span></span> <span data-ttu-id="ca9a9-107">このようにすることで、材料や配達ドライバーの時間が無駄になる可能性を最小限に抑えます。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-107">This approach minimizes the chances that we waste ingredients or delivery driver time.</span></span>

<span data-ttu-id="ca9a9-108">複数のコンポーネントが同じメッセージを受信できるように、Azure Service Bus トピックを使用します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-108">To allow multiple components to receive the same message, we'll use an Azure Service Bus topic.</span></span>

## <a name="how-code-that-uses-topics-differs-from-queues"></a><span data-ttu-id="ca9a9-109">トピックを使用するコードとキューを使用するコードの相違点</span><span class="sxs-lookup"><span data-stu-id="ca9a9-109">How code that uses topics differs from queues</span></span>

<span data-ttu-id="ca9a9-110">トピックに送信されたすべてのメッセージをサブスクライブしているすべてのコンポーネントに配信したい場合は、トピックを使用します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-110">If you want every message sent to the topic to be delivered to all subscribing components, you'll use topics.</span></span> <span data-ttu-id="ca9a9-111">トピックを使用するコードの記述は、キューにも当てはまります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-111">Writing code that uses topics is way to  queues.</span></span> <span data-ttu-id="ca9a9-112">同じ **Microsoft.Azure.ServiceBus** NuGet パッケージを使用し、接続文字列を構成して、非同期プログラミング パターンを使用します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-112">You use the same **Microsoft.Azure.ServiceBus** NuGet package, configure connection strings, and use asynchronous programming patterns.</span></span>

<span data-ttu-id="ca9a9-113">ただし、メッセージを送信する `QueueClient` クラスおよびメッセージを受信する `SubscriptionClient` クラスの代わりに、`TopicClient` クラスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-113">However, you use the `TopicClient` class instead of the `QueueClient` class to send messages and the `SubscriptionClient` class to receive messages.</span></span>

## <a name="setting-filters-on-subscriptions"></a><span data-ttu-id="ca9a9-114">サブスクリプションでのフィルターの設定</span><span class="sxs-lookup"><span data-stu-id="ca9a9-114">Setting filters on subscriptions</span></span>

<span data-ttu-id="ca9a9-115">トピックに送信されたどのメッセージをどのサブスクリプションに配信するかを制御したい場合は、トピックの各サブスクリプションにフィルターを配置できます。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-115">If you want to control which messages sent to the topic are delivered to which subscriptions, you can place filters on each subscription in the topic.</span></span> <span data-ttu-id="ca9a9-116">たとえば、ピザ アプリケーションでは、店で UWP アプリケーションが実行されています。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-116">In the pizza application, for instance, our storefronts are running UWP applications.</span></span> <span data-ttu-id="ca9a9-117">各店では、"OrderCancellation" トピックをサブスクライブし、その店の StoreId でフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-117">Each store can subscribe to the "OrderCancellation" topic but filter for its own StoreId.</span></span> <span data-ttu-id="ca9a9-118">離れた場所にある店に不要なメッセージが送信されないので、インターネットの帯域幅が節約されます。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-118">We save internet bandwidth because we are not sending unnecessary messages to distant store locations.</span></span> <span data-ttu-id="ca9a9-119">一方、支払い処理コンポーネントは、すべてのキャンセル メッセージをサブスクライブします。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-119">Meanwhile, the payment processing component subscribes to all our cancellation messages.</span></span>

<span data-ttu-id="ca9a9-120">次の 3 種類のフィルターのいずれかを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-120">Filters can be one of three types:</span></span>

- <span data-ttu-id="ca9a9-121">**ブール フィルター。**</span><span class="sxs-lookup"><span data-stu-id="ca9a9-121">**Boolean Filters.**</span></span> <span data-ttu-id="ca9a9-122">`TrueFilter` は、トピックに送信されたすべてのメッセージが、現在のサブスクリプションに配信されるようにします。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-122">The `TrueFilter` ensures that all messages sent to the topic are delivered to the current subscription.</span></span> <span data-ttu-id="ca9a9-123">`FalseFilter` は、現在のサブスクリプションにメッセージがまったく配信されないようにします (これは、サブスクリプションを効果的にブロックまたはオフにします)。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-123">The `FalseFilter` ensures that none of the messages are delivered to the current subscription (This effectively blocks or switches off the subscription).</span></span>
- <span data-ttu-id="ca9a9-124">**SQL フィルター。**</span><span class="sxs-lookup"><span data-stu-id="ca9a9-124">**SQL Filters.**</span></span> <span data-ttu-id="ca9a9-125">SQL フィルターは、SQL クエリの `WHERE` 句と同じ構文を使用して条件を指定します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-125">A SQL filter specifies a condition by using the same syntax as a `WHERE` clause in a SQL query.</span></span> <span data-ttu-id="ca9a9-126">このサブスクリプションに対して評価されたときに `True` を返すメッセージのみが、サブスクライバーに配信されます。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-126">Only messages that return `True`, when evaluated against this subscription, will be delivered to the subscribers.</span></span>
- <span data-ttu-id="ca9a9-127">**相関関係フィルター。**</span><span class="sxs-lookup"><span data-stu-id="ca9a9-127">**Correlation Filters.**</span></span> <span data-ttu-id="ca9a9-128">相関関係フィルターは、各メッセージのプロパティと照合される条件のセットを保持します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-128">A correlation filter holds a set of conditions that are matched against the properties of each message.</span></span> <span data-ttu-id="ca9a9-129">フィルターのプロパティとメッセージのプロパティが同じ値である場合、一致するものと見なされます。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-129">If the property in the filter and the property on the message have the same value, it is considered a match.</span></span>

<span data-ttu-id="ca9a9-130">この例の StoreId フィルターでは、SQL フィルターを使用することも "*できました*"。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-130">For our StoreId filter, we *could* use a SQL filter.</span></span> <span data-ttu-id="ca9a9-131">SQL フィルターは最も柔軟性がありますが、計算の負荷は最も大きく、サービス バスのスループットが低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-131">Those are the most flexible, but also the most computationally expensive and could slow down our service bus throughput.</span></span> <span data-ttu-id="ca9a9-132">そこで、相関関係フィルターを代わりに選択します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-132">In this case we choose a correlation filter instead.</span></span> 

## <a name="topicclient-example"></a><span data-ttu-id="ca9a9-133">TopicClient の例</span><span class="sxs-lookup"><span data-stu-id="ca9a9-133">TopicClient example</span></span>

<span data-ttu-id="ca9a9-134">すべての送信または受信コンポーネントで、Service Bus トピックを呼び出すコード ファイルに次の using ステートメントを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-134">In any sending or receiving component, you should add the following using statements to any code file that calls a Service Bus topic:</span></span>

    ```C#
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

<span data-ttu-id="ca9a9-135">メッセージを送信するには、最初に新しい `TopicClient` オブジェクトを作成し、接続文字列とトピックの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-135">To send a message, start by creating a new `TopicClient` object and pass it the connection string and the name of the topic:</span></span>

    ```C#
    topicClient = new TopicClient(TextAppConnectionString, "GroupMessageTopic");
    ```

<span data-ttu-id="ca9a9-136">`TopicClient.SendAsync()` メソッドを呼び出してメッセージを渡すことにより、トピックにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-136">You can send a message to the topic, by calling the `TopicClient.SendAsync()` method and passing the message.</span></span> <span data-ttu-id="ca9a9-137">キューの場合と同じように、メッセージは UTF8 でエンコードされた文字列の形式にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-137">As for queues, the message must be in the form of a UTF8 encoded string:</span></span>

    ```C#
    string message = "Cancel! I can't believe you use canned mushrooms!";
    var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
    await topicClient.SendAsync(encodedMessage);
    ```

<span data-ttu-id="ca9a9-138">メッセージを受信するには、`TopicClient` オブジェクトではなく `SubscriptionClient` オブジェクトを作成し、それに接続文字列、トピックの名前、サブスクリプションの名前を "**すべて**" 渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-138">To receive messages, you must create a `SubscriptionClient` object, not a `TopicClient` object, and pass it the connection string, the name of the topic, **and** the name of the subscription:</span></span>

    ```C#
    subscriptionClient = new SubscriptionClient(ServiceBusConnectionString, "GroupMessageTopic", "NorthAmerica");
    ```

<span data-ttu-id="ca9a9-139">その後、メッセージ ハンドラーを登録します。これは、取得したメッセージを処理するコード内の非同期メソッドです。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-139">Then register a message handler - this is the asynchronous method in your code that processes the retrieved message.</span></span>

    ```C#
    subscriptionClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
    ```

<span data-ttu-id="ca9a9-140">メッセージ ハンドラー内では、`SubscriptionClient.CompleteAsync()` メソッドを呼び出してキューからメッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="ca9a9-140">Within the message handler, call the `SubscriptionClient.CompleteAsync()` method to remove the message from the queue:</span></span>

    ```C#
    await subscriptionClient.CompleteAsync(message.SystemProperties.LockToken);
    ```