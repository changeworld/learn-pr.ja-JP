<span data-ttu-id="3771e-101">複数の操作を確実にまとめて実行しなければならない場合があります。</span><span class="sxs-lookup"><span data-stu-id="3771e-101">There are times when you must guarantee that multiple operations execute together.</span></span> <span data-ttu-id="3771e-102">たとえば、インスタント メッセージング アプリケーションでは、ユーザーは個々の画像を送信したり、個々のテキスト メッセージを送信したり、イメージとテキスト メッセージを同時に送信したりできます。</span><span class="sxs-lookup"><span data-stu-id="3771e-102">For example, in your instant messaging application, users can send an individual picture, an individual text message, or a picture and text message together.</span></span> <span data-ttu-id="3771e-103">ユーザーが画像とテキスト メッセージを同時に送信することを選択したとき、グループの他のメンバーがそれらを確実に同時に受信するようにしなければなりません。</span><span class="sxs-lookup"><span data-stu-id="3771e-103">When the user chooses to send a picture and text message together, you must ensure that other members of the group receive them at the same time.</span></span> <span data-ttu-id="3771e-104">この同時受信が重要であるのは、画像とテキスト メッセージが同時に受信されない場合、画像とテキスト メッセージの間で別個のメッセージが送信される可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="3771e-104">This is important because if a picture and text message are not received together, it’s possible that a separate message could be sent in between the picture and text message.</span></span> <span data-ttu-id="3771e-105">会話全体が混乱することになりかねません。</span><span class="sxs-lookup"><span data-stu-id="3771e-105">That could make the overall conversation confusing.</span></span>

<span data-ttu-id="3771e-106">ここでは、複数の操作を確実に同時実行するために Azure Redis Cache でトランザクションを作成する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3771e-106">Here, we'll look at how to create a transaction in Azure Redis Cache to guarantee that multiple operations are executed together.</span></span>

## <a name="creating-and-running-transactions"></a><span data-ttu-id="3771e-107">トランザクションの作成と実行</span><span class="sxs-lookup"><span data-stu-id="3771e-107">Creating and running transactions</span></span>

<span data-ttu-id="3771e-108">Redis のトランザクションは、グループとして実行される複数のコマンドをキューに入れることで動作します。</span><span class="sxs-lookup"><span data-stu-id="3771e-108">Transactions in Redis work by queueing multiple commands to be executed as a group.</span></span> <span data-ttu-id="3771e-109">トランザクションが実行されると、その中でキューに入れられたコマンドは、他のクライアントからのその他のコマンドがインターリーブされずに実行されます。</span><span class="sxs-lookup"><span data-stu-id="3771e-109">When a transaction is executed, the commands queued inside of it are guaranteed to execute without any other commands from other clients interleaved between them.</span></span>

<span data-ttu-id="3771e-110">トランザクション ブロックを開始するには、`MULTI` コマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="3771e-110">To begin a transaction block, enter the `MULTI` command.</span></span> <span data-ttu-id="3771e-111">追加のコマンドはキューに入れられ、すぐには実行されません。</span><span class="sxs-lookup"><span data-stu-id="3771e-111">Further commands will be queued and not executed immediately.</span></span> <span data-ttu-id="3771e-112">`EXEC` コマンドを実行すると、トランザクション単位としてキューに入れられているすべてのコマンドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="3771e-112">Running the `EXEC` command will execute all of the queued commands as a transactional unit.</span></span> <span data-ttu-id="3771e-113">コマンドをキューに入れている間にトランザクションを中止する場合、`DISCARD` コマンドを実行すると、キューに入れられた_どの_コマンドも実行せずにトランザクション ブロックが閉じられます。</span><span class="sxs-lookup"><span data-stu-id="3771e-113">If you decide you want to abort an open transaction while queuing commands, running the `DISCARD` command will close the transaction block without running _any_ of the queued commands.</span></span>

<span data-ttu-id="3771e-114">Redis トランザクションは、ロールバックの概念をサポートしていません。</span><span class="sxs-lookup"><span data-stu-id="3771e-114">Redis transactions do not support the concept of rollback.</span></span> <span data-ttu-id="3771e-115">誤った構文を含むコマンドをトランザクション ブロックのキューに入れると、そのブロックは開いたままになりますが、`EXEC` で実行しようとすると自動的に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3771e-115">If you queue a command with incorrect syntax into a transaction block, the block will remain open, but will automatically be discarded if you attempt to execute it with `EXEC`.</span></span> <span data-ttu-id="3771e-116">実行_中_ (`EXEC`が呼び出された後) にトランザクションのコマンドが失敗しても、トランザクションは中止または &mdash; にロールバックされません。Redis は引き続きすべてのコマンドを実行し、 トランザクションが正常に完了したと見なします。</span><span class="sxs-lookup"><span data-stu-id="3771e-116">Commands in a transaction that fail _during_ execution (after `EXEC` is called) do not cause a transaction to be aborted or rolled back &mdash; Redis will still run all of the commands and consider the transaction to have completed successfully.</span></span>

## <a name="redis-transactions-with-servicestackredis"></a><span data-ttu-id="3771e-117">ServiceStack.Redis を使用した Redis トランザクション</span><span class="sxs-lookup"><span data-stu-id="3771e-117">Redis transactions with ServiceStack.Redis</span></span>

<span data-ttu-id="3771e-118">**ServiceStack.Redis** は、Azure Redis Cache とやりとりするための C# クライアント ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="3771e-118">**ServiceStack.Redis** is a C# client library for interacting with Azure Redis Cache.</span></span>

<span data-ttu-id="3771e-119">ServiceStack.Redis でのトランザクションは、`IRedisClient.CreateTransaction()` を呼び出すことで作成されます。</span><span class="sxs-lookup"><span data-stu-id="3771e-119">Transactions in ServiceStack.Redis are created by calling `IRedisClient.CreateTransaction()`.</span></span> <span data-ttu-id="3771e-120">返される `IRedisTransaction` オブジェクトでは、`QueueCommand()` を使用して複数のコマンドをキューに入れることができます。</span><span class="sxs-lookup"><span data-stu-id="3771e-120">The `IRedisTransaction` object that is returned can have multiple commands queued into it with `QueueCommand()`.</span></span> <span data-ttu-id="3771e-121">トランザクションで `Commit()` を呼び出すと、オブジェクトがそれを実行します。</span><span class="sxs-lookup"><span data-stu-id="3771e-121">Calling `Commit()` on the transaction object will execute it.</span></span>

<span data-ttu-id="3771e-122">`IRedisTransaction` オブジェクトは破棄可能であり、`Commit()` を呼び出す前に破棄されている場合、自動的に `DISCARD` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="3771e-122">`IRedisTransaction` objects are disposable, and will automatically issue a `DISCARD` command if disposed before calling `Commit()`.</span></span> <span data-ttu-id="3771e-123">この機能は、C# の `using` ブロックで正常に作動します。何らかの理由でトランザクションをコミットしなかった場合、Redis 接続を引き続き使用できるように、トランザクションは自動的に破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3771e-123">This feature works well with C#'s `using` blocks: if you don't commit a transaction for any reason, you can trust that the transaction will automatically be discarded so that the Redis connection can continue to be used.</span></span>

## <a name="create-a-transaction-using-c-and-the-servicestackredis-client"></a><span data-ttu-id="3771e-124">C# と ServiceStack.Redis クライアントを使用してトランザクションを作成する</span><span class="sxs-lookup"><span data-stu-id="3771e-124">Create a transaction using C# and the ServiceStack.Redis client</span></span>

<span data-ttu-id="3771e-125">次に、ServiceStack.Redis を使用して、画像 URL 含むメッセージとテキスト メッセージのコンテンツを送信するためのトランザクションを作成する例を示します。</span><span class="sxs-lookup"><span data-stu-id="3771e-125">Here's an example of using ServiceStack.Redis to create a transaction that can send a message that includes a picture URL and the contents of a text message.</span></span>

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    return transactionResult;
}
```

<span data-ttu-id="3771e-126">トランザクションにより、複数の操作が、トランザクション間で他のクライアントからの操作なしで一緒に実行されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="3771e-126">Transactions ensure that multiple operations are executed together without operations from other clients in between them.</span></span> <span data-ttu-id="3771e-127">`MULTI` コマンドを使用してトランザクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3771e-127">You create a transaction using the `MULTI` command.</span></span> <span data-ttu-id="3771e-128">ServiceStack.Redis を使用して、`CreateTransaction()` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3771e-128">With ServiceStack.Redis, you use the `CreateTransaction()` method.</span></span>