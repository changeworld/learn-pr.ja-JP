<span data-ttu-id="bb7ab-101">分散アプリケーションでは、Service Bus キューなどのキューを、送信先コンポーネントへの配信を待機しているメッセージのための一時保管場所として使用します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-101">Distributed applications use queues, such as Service Bus queues, as temporary storage locations for messages that are awaiting delivery to a destination component.</span></span> <span data-ttu-id="bb7ab-102">キューを介してメッセージを送受信するには、送信元コンポーネントと送信先コンポーネントでコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-102">To send and receive messages through a queue, you must write code in the source and destination components.</span></span>

<span data-ttu-id="bb7ab-103">Contoso Slices アプリケーションについて考えます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-103">Consider the Contoso Slices application.</span></span> <span data-ttu-id="bb7ab-104">顧客は Web サイトまたはモバイル アプリを使用して注文します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-104">The customer places the order through a website or mobile app.</span></span> <span data-ttu-id="bb7ab-105">これらは顧客のデバイス上で実行されるため、一度に送られてくる注文件数に制限はありません。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-105">Since those run on customer devices, there is really no limit to how many orders could come in at once.</span></span> <span data-ttu-id="bb7ab-106">モバイル アプリと Web サイトでキューに注文をためることで、バックエンド コンポーネント (Web アプリ) は自分のペースでそのキューから注文を処理することができます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-106">By having the mobile app and website deposit the orders in a queue, we can allow the back-end component (a web app) to process orders from that queue at its own pace.</span></span>

<span data-ttu-id="bb7ab-107">Contoso Slices アプリケーションは実際には複数のステップで新しい注文を処理します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-107">The Contoso Slices application actually has several steps to handle a new order.</span></span> <span data-ttu-id="bb7ab-108">ただし、そのすべては最初の支払いの承認に依存するので、キューを使用することに決めます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-108">But all of them are dependent on first authorizing payment, so we decide to use a queue.</span></span> <span data-ttu-id="bb7ab-109">受信コンポーネントの最初のジョブは、支払いの処理です。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-109">Our receiving component's first job will be processing the payment.</span></span>

<span data-ttu-id="bb7ab-110">モバイル アプリと Web サイトでは、キューにメッセージを追加するコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-110">In the mobile app and website, Contoso needs to write code that adds a message to the queue.</span></span> <span data-ttu-id="bb7ab-111">バックエンドの Web アプリでは、キューからメッセージを取り出すコードを記述します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-111">In the back-end web app, they'll write code that picks messages up from the the queue.</span></span>

<span data-ttu-id="bb7ab-112">ここでは、そのコードを記述する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-112">Here, you will learn how to write that code.</span></span>

## <a name="the-microsoftazureservicebus-nuget-package"></a><span data-ttu-id="bb7ab-113">Microsoft.Azure.ServiceBus NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="bb7ab-113">The Microsoft.Azure.ServiceBus NuGet package</span></span>

<span data-ttu-id="bb7ab-114">Service Bus を通してメッセージを送受信するコードを簡単に記述できるように、.NET クラスのライブラリが用意されています。このライブラリを任意の .NET Framework 言語で使用して、Service Bus のキュー、トピック、リレーと対話できます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-114">To make it easy to write code that sends and receives messages through Service Bus, Microsoft provides a library of .NET classes, which you can use in any .NET Framework language to interact with a Service Bus queue, topic, or relay.</span></span> <span data-ttu-id="bb7ab-115">**Microsoft.Azure.ServiceBus** NuGet パッケージを追加することで、アプリケーションにこのライブラリを組み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-115">You can include this library in your application by adding the **Microsoft.Azure.ServiceBus** NuGet package.</span></span>

<span data-ttu-id="bb7ab-116">キューに関してこのライブラリで最も重要なクラスは、`QueueClient` クラスです。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-116">The most important class in this library for queues is the `QueueClient` class.</span></span> <span data-ttu-id="bb7ab-117">最初に、送信と受信両方のコンポーネントで、このクラスをインスタンス化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-117">You must start by instantiating this class both in sending and receiving components.</span></span>

## <a name="connection-strings-and-keys"></a><span data-ttu-id="bb7ab-118">接続文字列とキー</span><span class="sxs-lookup"><span data-stu-id="bb7ab-118">Connection Strings and Keys</span></span>

<span data-ttu-id="bb7ab-119">Service Bus 名前空間内のキューに接続するには、送信元コンポーネントと送信先コンポーネントの両方に 2 つの情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-119">Both source components and destination components need two pieces of information to connect to a queue in a Service Bus namespace:</span></span>

- <span data-ttu-id="bb7ab-120">Service Bus 名前空間の場所。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-120">The location of the Service Bus namespace.</span></span> <span data-ttu-id="bb7ab-121">**エンドポイント**とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-121">Also known as an **endpoint**.</span></span> <span data-ttu-id="bb7ab-122">場所は、**servicebus.windows.net** ドメイン内の完全修飾ドメイン名で指定されます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-122">The location is specified as a fully-qualified domain name within the **servicebus.windows.net** domain.</span></span> <span data-ttu-id="bb7ab-123">例: **pizzaService.servicebus.windows.net**。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-123">For example: **pizzaService.servicebus.windows.net**.</span></span>
- <span data-ttu-id="bb7ab-124">アクセス キー。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-124">An access key.</span></span> <span data-ttu-id="bb7ab-125">Service Bus では、アクセス キーを要求することによってキュー、トピック、リレーへのアクセスが制限されます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-125">Service Bus restricts access to queues, topics, and relays by requiring an access key.</span></span>

<span data-ttu-id="bb7ab-126">両方の情報は、接続文字列の形式で `QueueClient` オブジェクトに提供されます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-126">Both of these pieces of information are provided to the `QueueClient` object in the form of a connection string.</span></span> <span data-ttu-id="bb7ab-127">名前空間に対する正しい接続文字列は、Azure portal から取得できます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-127">You can obtain the correct connection string for your namespace from the Azure portal.</span></span>

## <a name="calling-methods-asynchronously"></a><span data-ttu-id="bb7ab-128">メソッドの非同期呼び出し</span><span class="sxs-lookup"><span data-stu-id="bb7ab-128">Calling methods asynchronously</span></span>

<span data-ttu-id="bb7ab-129">Azure のキューは、送信側と受信側のコンポーネントから何千キロも離れた場所に存在する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-129">The queue in Azure may be located thousands of miles away from sending and receiving components.</span></span> <span data-ttu-id="bb7ab-130">物理的に近くても、遅い接続や帯域幅の競合のため、コンポーネントがキューのメソッドを呼び出したときに遅延が発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-130">Even if it is physically close, slow connections and bandwidth contention may cause delays when a component calls a method on the queue.</span></span> <span data-ttu-id="bb7ab-131">このため、serviceBus クライアント ライブラリではキューとの対話に `async` メソッドを使用できるようになっています。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-131">For this reason, the serviceBus client library makes `async` methods available for interacting with the queues.</span></span> <span data-ttu-id="bb7ab-132">ここでは、これらのメソッドを使用して、呼び出しが完了するのを待つ間にスレッドがブロックされるのを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-132">We'll use these methods to avoid blocking a thread while waiting for calls to complete.</span></span>

<span data-ttu-id="bb7ab-133">たとえば、キューにメッセージを送信するときは、`await` キーワードを指定して `QueueClient.SendAsync()` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-133">When sending a message to a queue, for example, use the `QueueClient.SendAsync()` method with the `await` keyword.</span></span>

## <a name="write-code-that-sends-to-queues"></a><span data-ttu-id="bb7ab-134">キューに送信するコードを記述する</span><span class="sxs-lookup"><span data-stu-id="bb7ab-134">Write code that sends to queues</span></span> 

<span data-ttu-id="bb7ab-135">すべての送信または受信コンポーネントで、Service Bus キューを呼び出すコード ファイルに次の using ステートメントを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-135">In any sending or receiving component, you should add the following using statements to any code file that calls a Service Bus queue:</span></span>

    ```C#
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.ServiceBus;
    ```

<span data-ttu-id="bb7ab-136">次に、新しい `QueueClient` オブジェクトを作成し、接続文字列とキューの名前を渡します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-136">Next, create a new `QueueClient` object and pass it the connection string and the name of the queue:</span></span>

    ```C#
    queueClient = new QueueClient(TextAppConnectionString, "PrivateMessageQueue");
    ```

<span data-ttu-id="bb7ab-137">`QueueClient.SendAsync()` メソッドを呼び出して、UTF8 エンコード文字列の形式でメッセージを渡すことにより、キューにメッセージを送信できます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-137">You can send a message to the queue, by calling the `QueueClient.SendAsync()` method and passing the message in the form of a UTF8 encoded string:</span></span>

    ```C#
    string message = "Sure would like a large pepperoni!";
    var encodedMessage = new Message(Encoding.UTF8.GetBytes(message));
    await queueClient.SendAsync(encodedMessage);
    ```

## <a name="receive-messages-from-queue"></a><span data-ttu-id="bb7ab-138">キューからメッセージを受信する</span><span class="sxs-lookup"><span data-stu-id="bb7ab-138">Receive messages from queue</span></span>

<span data-ttu-id="bb7ab-139">メッセージを受信するには、最初にメッセージ ハンドラーを登録する必要があります。メッセージ ハンドラーはコード内のメソッドであり、キューでメッセージが利用可能になると呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-139">To receive messages, you must first register a message handler - this is the method in your code that will be invoked when a message is available on the queue.</span></span>

    ```C#
    queueClient.RegisterMessageHandler(MessageHandler, messageHandlerOptions);
    ```

<span data-ttu-id="bb7ab-140">必要な処理を行います。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-140">Do your processing work.</span></span> <span data-ttu-id="bb7ab-141">その後、メッセージ ハンドラー内で、`QueueClient.CompleteAsync()` メソッドを呼び出してキューからメッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="bb7ab-141">Then within the message handler, call the `QueueClient.CompleteAsync()` method to remove the message from the queue:</span></span>

    ```C#
    await queueClient.CompleteAsync(message.SystemProperties.LockToken);
    ```
    