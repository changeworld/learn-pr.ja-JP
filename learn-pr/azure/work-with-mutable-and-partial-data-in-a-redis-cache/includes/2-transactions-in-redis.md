<span data-ttu-id="9992b-101">場合によっては、複数の操作が同時に実行されることを保証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9992b-101">There are times where you must guarantee that multiple operations execute together.</span></span> <span data-ttu-id="9992b-102">たとえば、インスタント メッセージング アプリケーションで、ユーザーは個々の画像を送信したり、個々のテキスト メッセージを送信したり、イメージとテキスト メッセージを同時に送信したりできます。</span><span class="sxs-lookup"><span data-stu-id="9992b-102">For example, in your instant messaging application, users can send: an individual picture, an individual text message, or a picture and text message together.</span></span> <span data-ttu-id="9992b-103">ユーザーが画像とテキスト メッセージを同時に送信する場合、開発者はグループの他のメンバーがそれらを同時に受信するようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9992b-103">When the user chooses to send a picture and text message together, we must ensure that other members of the group receive them at the same time.</span></span> <span data-ttu-id="9992b-104">画像とテキスト メッセージが同時に受信されない場合、画像送信とテキスト メッセージ送信の間に別のメッセージが送信される可能性があり、メッセージ交換全体で混乱が生じることが考えられるため、このことは重要です。</span><span class="sxs-lookup"><span data-stu-id="9992b-104">This is important because if a picture and text message are not received together, it’s possible that a separate message could be sent in between the picture and text message, which could make the overall conversation confusing.</span></span>

<span data-ttu-id="9992b-105">ここでは、複数の操作が同時に実行されることを保証するために Redis でトランザクションを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9992b-105">Here, we'll look at how to create a transaction in Redis to guarantee multiple operations are executed together.</span></span>

## <a name="how-to-create-a-transaction"></a><span data-ttu-id="9992b-106">トランザクションを作成する方法</span><span class="sxs-lookup"><span data-stu-id="9992b-106">How to create a transaction</span></span>

<span data-ttu-id="9992b-107">トランザクションを作成するには、トランザクション ブロックが必要です。</span><span class="sxs-lookup"><span data-stu-id="9992b-107">To create a transaction, you need to have a transaction block.</span></span> <span data-ttu-id="9992b-108">これは、同時に実行されるコマンドを含むキューです。</span><span class="sxs-lookup"><span data-stu-id="9992b-108">This is a queue that contains the commands that will be executed together.</span></span> <span data-ttu-id="9992b-109">`MULTI` コマンドを使用してトランザクション ブロックを作成すると、後続のすべてのコマンドがアトミック実行のキューに登録されます。</span><span class="sxs-lookup"><span data-stu-id="9992b-109">The `MULTI` command is used to create a transaction block and all subsequent commands are queued for atomic execution.</span></span>

## <a name="how-to-execute-a-transaction"></a><span data-ttu-id="9992b-110">トランザクションを実行する方法</span><span class="sxs-lookup"><span data-stu-id="9992b-110">How to execute a transaction</span></span>

<span data-ttu-id="9992b-111">トランザクション ブロックでコマンドを実行するには、`EXEC` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="9992b-111">To execute the commands in a transaction block, you use the `EXEC` command.</span></span> <span data-ttu-id="9992b-112">これにより、キューに登録されたすべてのコマンドが実行され、接続状態が正常に回復します。</span><span class="sxs-lookup"><span data-stu-id="9992b-112">This will execute all queued commands and restore the connection state to normal.</span></span> <span data-ttu-id="9992b-113">トランザクションを実行しない場合は、`DISCARD` コマンドを使用して、トランザクション ブロックをクリアし、接続状態を正常に設定することができます。</span><span class="sxs-lookup"><span data-stu-id="9992b-113">If you decide you don't want to execute a transaction, you can use the `DISCARD` command, which will clear out the transaction block and set the connection state to normal.</span></span>

<span data-ttu-id="9992b-114">Redis トランザクションを理解するうえで重要な点は、ロールバックがサポートされないことです。</span><span class="sxs-lookup"><span data-stu-id="9992b-114">One important thing to understand about Redis transactions is that they don't support rollbacks.</span></span> <span data-ttu-id="9992b-115">これは、トランザクション内の 1 つのコマンドが失敗した場合でも、それ以外のコマンドが実行されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="9992b-115">This means that if a command inside a transaction fails, the remaining commands will still be executed.</span></span>

## <a name="what-is-servicestackredis"></a><span data-ttu-id="9992b-116">ServiceStack.Redis とは</span><span class="sxs-lookup"><span data-stu-id="9992b-116">What is ServiceStack.Redis?</span></span>

<span data-ttu-id="9992b-117">前のモジュール「**Optimize your web application by caching read-only data with Redis**」(Redis で読み取り専用データをキャッシュすることにより Web アプリケーションを最適化する) では、**StackExchange.Redis** クライアントを使用しました。</span><span class="sxs-lookup"><span data-stu-id="9992b-117">In the previous module, **Optimize your web application by caching read-only data with Redis** we used the **StackExchange.Redis** client.</span></span> <span data-ttu-id="9992b-118">ここでは、一般的に使用されるもう 1 つのクライアントである **ServiceStack.Redis** を試してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9992b-118">Here, we're going to try out another popular client, **ServiceStack.Redis**.</span></span>

<span data-ttu-id="9992b-119">**ServiceStack.Redis** は、Redis Cache とやりとりするための C# クライアント ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="9992b-119">**ServiceStack.Redis** is a C# client library for interacting with a Redis cache.</span></span> <span data-ttu-id="9992b-120">つまり、低レベルの Redis コマンドを使用する代わりに、C# クラスおよびメソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9992b-120">This means that instead of using low-level Redis commands, we can use C# classes and methods.</span></span>

<span data-ttu-id="9992b-121">たとえば、トランザクションを作成するには、通常は Redis `MULTI` コマンドを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9992b-121">For example, to create a transaction, we normally must use the Redis `MULTI` command.</span></span> <span data-ttu-id="9992b-122">しかし、**ServiceStack.Redis** を使用すれば、`CreateTransaction()` メソッドを使用してトランザクションを作成できます。</span><span class="sxs-lookup"><span data-stu-id="9992b-122">However, using **ServiceStack.Redis** we can create a transaction using the `CreateTransaction()` method.</span></span>

```csharp
var transaction = redisClient.CreateTransaction();
```

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a><span data-ttu-id="9992b-123">C# と ServiceStack.Redis クライアントを使用してトランザクションを作成する</span><span class="sxs-lookup"><span data-stu-id="9992b-123">Create a transaction using C# and the ServiceStack.Redis client</span></span>

<span data-ttu-id="9992b-124">次に、**ServiceStack.Redis** を使用して、画像とテキストを含むメッセージを送信するためのトランザクションを作成する例を示します。</span><span class="sxs-lookup"><span data-stu-id="9992b-124">Here's an example of using **ServiceStack.Redis** to create a transaction that can send a message that includes a picture and text.</span></span>

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        return transactionResult;
    }
}
```
<span data-ttu-id="9992b-125">トランザクションにより、複数の操作が同時に実行されることを保証できます。</span><span class="sxs-lookup"><span data-stu-id="9992b-125">Transactions ensure that multiple operations are executed together.</span></span> <span data-ttu-id="9992b-126">トランザクションの作成は、`MULTI` コマンドを使用できますが、**ServiceStack.Redis** などのクライアント ライブラリを使用することで `CreateTransaction()` メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="9992b-126">You create a transaction using the `MULTI` command, however, using a client library like **ServiceStack.Redis**, you can use the `CreateTransaction()` method.</span></span>