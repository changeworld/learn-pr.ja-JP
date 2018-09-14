<span data-ttu-id="76ce6-101">キューには、送信者および受信者が把握している形状のデータ パケットのメッセージが保持されます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-101">Queues hold messages - packets of data whose shape is known to the sender and receiver.</span></span> <span data-ttu-id="76ce6-102">キューは送信者によって作成され、メッセージが追加されます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-102">The sender creates the queue and adds a message.</span></span> <span data-ttu-id="76ce6-103">受信者はメッセージを受け取り、それを処理して、キューからそのメッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-103">The receiver retrieves a message, processes it, and then deletes the message from the queue.</span></span> <span data-ttu-id="76ce6-104">次の図は、このプロセスの典型的なフローを示しています。</span><span class="sxs-lookup"><span data-stu-id="76ce6-104">The following illustration shows a typical flow of this process.</span></span>

![Azure Queue におけるメッセージの典型的なフロー図。](../media/6-message-flow.png)

<span data-ttu-id="76ce6-106">`get` と `delete` は別の操作であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="76ce6-106">Notice that `get` and `delete` are separate operations.</span></span> <span data-ttu-id="76ce6-107">この取り合わせは、受信側で可能性のあるエラーを処理し、_at-least-once delivery_ と呼ばれる概念を実装します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-107">This arrangement handles potential failures in the receiver and implements a concept called _at-least-once delivery_.</span></span> <span data-ttu-id="76ce6-108">受信者がメッセージを受け取ると、メッセージはキューに残りますが、30 秒間は表示されません。</span><span class="sxs-lookup"><span data-stu-id="76ce6-108">After the receiver gets a message, that message remains in the queue but is invisible for 30 seconds.</span></span> <span data-ttu-id="76ce6-109">受信者に処理時のクラッシュや電源障害があった場合は、キューからメッセージは削除されません。</span><span class="sxs-lookup"><span data-stu-id="76ce6-109">If the receiver crashes or experiences a power failure during processing, then it will never delete the message from the queue.</span></span> <span data-ttu-id="76ce6-110">このメッセージは、30 秒後にキューに再表示され、受信者の別のインスタンスがそれを完了まで処理します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-110">After 30 seconds, the message will reappear in the queue and another instance of the receiver can process it to completion.</span></span>

## <a name="the-azure-storage-client-library-for-net"></a><span data-ttu-id="76ce6-111">.NET 用 Azure Storage クライアント ライブラリ</span><span class="sxs-lookup"><span data-stu-id="76ce6-111">The Azure Storage Client Library for .NET</span></span>

<span data-ttu-id="76ce6-112">**.NET 用 Azure Storage クライアント ライブラリ**には、やりとりをする必要がある各オブジェクトを表す型があります。</span><span class="sxs-lookup"><span data-stu-id="76ce6-112">The **Azure Storage Client Library for .NET** provides types to represent each of the objects you need to interact with:</span></span>

- <span data-ttu-id="76ce6-113">`CloudStorageAccount` は、ご使用の Azure ストレージ アカウントを表します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-113">`CloudStorageAccount` represents your Azure storage account.</span></span>
- <span data-ttu-id="76ce6-114">`CloudQueueClient` は、Azure Queue Storage を表します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-114">`CloudQueueClient` represents Azure Queue storage.</span></span>
- <span data-ttu-id="76ce6-115">`CloudQueue` は、ご使用のキュー インスタンスの 1 つを表します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-115">`CloudQueue` represents one of your queue instances.</span></span>
- <span data-ttu-id="76ce6-116">`CloudQueueMessage` は、メッセージを表します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-116">`CloudQueueMessage` represents a message.</span></span>

<span data-ttu-id="76ce6-117">これらのクラスを使用すると、キューにプログラムを使用してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-117">You will use these classes to get programmatic access to your queue.</span></span> <span data-ttu-id="76ce6-118">このライブラリには、同期メソッドと非同期メソッドの両方があります。クライアント アプリがブロックされるのを防ぐには、非同期バージョンを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="76ce6-118">The library has both synchronous and asynchronous methods; you should prefer to use the asynchronous versions to avoid blocking the client app.</span></span>

> [!NOTE]
> <span data-ttu-id="76ce6-119">.NET 用 Azure Storage クライアント ライブラリは、**WindowsAzure.Storage** NuGet パッケージにあります。</span><span class="sxs-lookup"><span data-stu-id="76ce6-119">The Azure Storage Client Library for .NET is available in the **WindowsAzure.Storage** NuGet package.</span></span> <span data-ttu-id="76ce6-120">これは、IDE、Azure CLI または PowerShell `Install-Package WindowsAzure.Storage` を使用してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-120">You can install it through an IDE, Azure CLI, or through PowerShell `Install-Package WindowsAzure.Storage`.</span></span>

## <a name="how-to-connect-to-a-queue"></a><span data-ttu-id="76ce6-121">キューへの接続方法</span><span class="sxs-lookup"><span data-stu-id="76ce6-121">How to connect to a queue</span></span>

<span data-ttu-id="76ce6-122">キューに接続するには、まず接続文字列を使用して `CloudStorageAccount` を作成します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-122">To connect to a queue, you first create a `CloudStorageAccount` with your connection string.</span></span> <span data-ttu-id="76ce6-123">結果のオブジェクトにより、`CloudQueueClient` が作成され、代わりに `CloudQueue` インスタンスを開くことができるようになります。</span><span class="sxs-lookup"><span data-stu-id="76ce6-123">The resulting object can then create a `CloudQueueClient`, which in turn can open a `CloudQueue` instance.</span></span> <span data-ttu-id="76ce6-124">基本のコード フローは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="76ce6-124">The basic code flow is shown below.</span></span>

```csharp
CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);

CloudQueueClient client = account.CreateCloudQueueClient();

CloudQueue queue = client.GetQueueReference("myqueue");
```

<span data-ttu-id="76ce6-125">`CloudQueue` を作成しても、必ずしも_実際_にストレージ キューが存在することは意味しません。</span><span class="sxs-lookup"><span data-stu-id="76ce6-125">Creating a `CloudQueue` doesn't necessarily mean the _actual_ storage queue exists.</span></span> <span data-ttu-id="76ce6-126">ただし、このオブジェクトを使用すると、既存のキューを作成、削除、チェックすることができます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-126">However, you can use this object to create, delete, and check for an existing queue.</span></span> <span data-ttu-id="76ce6-127">前述のとおり、すべてのメソッドで同期バージョンと非同期バージョンがサポートされていますが、ここでは、`Task` ベースの非同期バージョンのみを使用します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-127">As mentioned above, all methods support both synchronous and asynchronous versions, but we will only be using the `Task`-based asynchronous versions.</span></span>

## <a name="how-to-create-a-queue"></a><span data-ttu-id="76ce6-128">キューの作成方法</span><span class="sxs-lookup"><span data-stu-id="76ce6-128">How to create a queue</span></span>

<span data-ttu-id="76ce6-129">キューは、送信者のアプリケーションが常にキューを作成する責任を負う、一般的な方法で作成します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-129">You will use a common pattern for queue creation: the sender application should always be responsible for creating the queue.</span></span> <span data-ttu-id="76ce6-130">これにより、アプリケーションはより自己完結的となり、管理セットアップへの依存性が低くなります。</span><span class="sxs-lookup"><span data-stu-id="76ce6-130">This keeps your application more self-contained and less dependent on administrative set-up.</span></span> 

<span data-ttu-id="76ce6-131">クライアント ライブラリでは、作成がシンプルになるように、必要に応じてキューが作成されたり、またはキューが既に存在する場合に `false` が返されたりする `CreateIfNotExistsAsync` メソッドが公開されています。</span><span class="sxs-lookup"><span data-stu-id="76ce6-131">To make the creation simple, the client library exposes a `CreateIfNotExistsAsync` method that will create the queue if necessary, or return `false` if the queue already exists.</span></span> 

<span data-ttu-id="76ce6-132">典型的なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="76ce6-132">Typical code is shown below.</span></span>

```csharp
CloudQueue queue;
//...

await queue.CreateIfNotExistsAsync();
```

> [!NOTE]
> <span data-ttu-id="76ce6-133">この API を使用するには、ストレージ アカウントに対する `Write` または `Create` のアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="76ce6-133">You must have `Write` or `Create` permissions for the storage account to use this API.</span></span> <span data-ttu-id="76ce6-134">**アクセス キー** セキュリティ モデルを使用していても、常に、キューに対する読み取り操作のみを許可するその他のアプローチを持つアカウントへのアクセス許可をロックダウンすることができます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-134">This is always true if you use the **Access Key** security model, but you can lock down permissions to the account with other approaches that will only allow read operations against the queue.</span></span>

## <a name="how-to-send-a-message"></a><span data-ttu-id="76ce6-135">メッセージの送信方法</span><span class="sxs-lookup"><span data-stu-id="76ce6-135">How to send a message</span></span>

<span data-ttu-id="76ce6-136">メッセージを送信するには、`CloudQueueMessage` オブジェクトをインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-136">To send a message, you instantiate a `CloudQueueMessage` object.</span></span> <span data-ttu-id="76ce6-137">このクラスには、メッセージにデータを読み込むいくつかのオーバーロードされたコンストラクターがあります。</span><span class="sxs-lookup"><span data-stu-id="76ce6-137">The class has a few overloaded constructors that load your data into the message.</span></span> <span data-ttu-id="76ce6-138">ここでは、`string` を受け取るコンストラクターを使用します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-138">We will use the constructor that takes a `string`.</span></span> <span data-ttu-id="76ce6-139">メッセージを作成したら、それの送信に `CloudQueue` オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-139">After creating the message, you use a `CloudQueue` object to send it.</span></span>

<span data-ttu-id="76ce6-140">典型的な例を次に表します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-140">Here's a typical example:</span></span>

```csharp
var message = new CloudQueueMessage("your message here");

CloudQueue queue;
//...

await queue.AddMessageAsync(message);
```

> [!NOTE]
> <span data-ttu-id="76ce6-141">キューの合計サイズとしては、最大 500 TB が可能ですが、その中の個々のメッセージの最大サイズは 64 KB です (Base64 のエンコードを使用する場合は 48 KB)。</span><span class="sxs-lookup"><span data-stu-id="76ce6-141">While the total queue size can be up to 500 TB, the individual messages in it can only be up to 64 KB in size (48 KB when using Base64 encoding).</span></span> <span data-ttu-id="76ce6-142">大きなペイロードが必要な場合、URL をメッセージ内の実際のデータ (Blob として格納) に渡し、キューと blob を結合できます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-142">If you need a larger payload you can combine queues and blobs – passing the URL to the actual data (stored as a Blob) in the message.</span></span> <span data-ttu-id="76ce6-143">このアプローチによりは、最大 200 GB の 1 つの項目をキューに追加できます。</span><span class="sxs-lookup"><span data-stu-id="76ce6-143">This approach would allow you to enqueue up to 200 GB for a single item.</span></span>

## <a name="how-to-receive-and-delete-a-message"></a><span data-ttu-id="76ce6-144">メッセージの受信および削除方法</span><span class="sxs-lookup"><span data-stu-id="76ce6-144">How to receive and delete a message</span></span>

<span data-ttu-id="76ce6-145">受信側で、次のメッセージを受け取り、処理し、処理の完了後それを削除します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-145">In the receiver, you get the next message, process it, and then delete it after processing succeeds.</span></span> <span data-ttu-id="76ce6-146">シンプルな例を次に表します。</span><span class="sxs-lookup"><span data-stu-id="76ce6-146">Here's a simple example:</span></span>

```C#
CloudQueue queue;
//...

CloudQueueMessage message = await queue.GetMessageAsync();

if (message != null)
{
    // Process the message
    //...

    await queue.DeleteMessageAsync(message);
}
```

<span data-ttu-id="76ce6-147">では、この知識をアプリケーションに応用してみましょう。</span><span class="sxs-lookup"><span data-stu-id="76ce6-147">Let's now apply this new knowledge to our application!</span></span>