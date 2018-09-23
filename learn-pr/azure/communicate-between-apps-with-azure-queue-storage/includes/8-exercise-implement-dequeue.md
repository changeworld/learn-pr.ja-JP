<span data-ttu-id="76e42-101">ここでは、キュー内の次のメッセージを読み取って処理した後にそのメッセージをキューから削除するコードを記述して、アプリケーションを完成させます。</span><span class="sxs-lookup"><span data-stu-id="76e42-101">Now we want to complete the application by writing code to read the next message in the queue, process it, and delete it from the queue.</span></span> 

<span data-ttu-id="76e42-102">パラメーターが渡されなかった場合は、このコードを前と同じアプリケーションに配置して実行しますが、ニュース サービス シナリオでは、実際にこのコードを中間層サーバーに配置してストーリーを処理します。</span><span class="sxs-lookup"><span data-stu-id="76e42-102">We're going to place this code into the same application and execute it when you don't pass any parameters, however in our news service scenario, we would really place this code into our middle-tier servers to process the stories.</span></span>

## <a name="dequeue-a-message"></a><span data-ttu-id="76e42-103">メッセージをデキューする</span><span class="sxs-lookup"><span data-stu-id="76e42-103">Dequeue a message</span></span>

<span data-ttu-id="76e42-104">次のメッセージをキューから取得する新しいメソッドを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="76e42-104">Let's add a new method that retrieves the next message from the queue.</span></span>

1. <span data-ttu-id="76e42-105">エディターで `Program.cs` ソース ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="76e42-105">Open the `Program.cs` source file in your editor.</span></span>

1. <span data-ttu-id="76e42-106">パラメーターを取らず、`Task<string>` を返す `ReceiveArticleAsync` という名前の `Program` クラス内で静的メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="76e42-106">Create a static method in the `Program` class named `ReceiveArticleAsync` that takes no parameters and returns a `Task<string>`.</span></span> <span data-ttu-id="76e42-107">このメソッドを使用して、キューからニュース記事をプルし、それを返します。</span><span class="sxs-lookup"><span data-stu-id="76e42-107">We'll use this method to pull a news article from the queue and return it.</span></span>
    - <span data-ttu-id="76e42-108">さらに、メソッドに `async` キーワードを追加します。これは非同期の `Task` ベースのメソッドを使用するためです。</span><span class="sxs-lookup"><span data-stu-id="76e42-108">Go ahead and add the `async` keyword to the method since we'll be using some asynchronous `Task`-based methods.</span></span>

    ```csharp
    static async Task<string> ReceiveArticleAsync()
    {
    }

1. All of the setup code to get a `CloudQueue` will be identical to what we did in the last exercise. Code duplication is a bad habit, even in samples so go ahead and, refactor the code that obtains the `CloudQueue` to a new method named `GetQueue` and change the `SendArticleAsync` to use your new method.
     - Make sure to leave the code that _creates_ the queue in the `SendArticleAsync` method; remember only the **publisher** should create the queue.

    ```csharp
    const string ConnectionString = ...;
    // ...

    static CloudQueue GetQueue()
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        return queueClient.GetQueueReference("newsqueue");
    }
    ```
    
1. <span data-ttu-id="76e42-109">ご自分の `ReceiveArticleAsync` メソッド内で新しい`GetQueue` メソッドを呼び出して、キュー参照を取得し、それを変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="76e42-109">In your `ReceiveArticleAsync` method, call the new `GetQueue` method to retrieve your queue reference and assign it to a variable.</span></span>

1. <span data-ttu-id="76e42-110">次に、`CloudQueue` オブジェクトに対して `ExistsAsync` メソッドを呼び出します。これにより、キューが作成されているかどうかが返されます。</span><span class="sxs-lookup"><span data-stu-id="76e42-110">Next, call the `ExistsAsync` method on the `CloudQueue` object; this will return whether the queue has been created.</span></span> <span data-ttu-id="76e42-111">存在しないキューからメッセージを取得しようとすると、API から例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="76e42-111">If we attempt to retrieve a message from a non-existent queue, the API will throw an exception.</span></span>
    - <span data-ttu-id="76e42-112">このメソッドは非同期であるため、`await` を使用して戻り値を取得します。</span><span class="sxs-lookup"><span data-stu-id="76e42-112">This method is asynchronous so use `await` to get the return value.</span></span>
    - <span data-ttu-id="76e42-113">`ReceiveArticleAsync` メソッドには `async` キーワードが既に含まれているはずです。そうでない場合はここで追加します。</span><span class="sxs-lookup"><span data-stu-id="76e42-113">You should already have the `async` keyword on the `ReceiveArticleAsync` method, but if not add it now.</span></span>


1. <span data-ttu-id="76e42-114">`ExistsAsync` からの戻り値を使用する `if` ブロックを追加します。</span><span class="sxs-lookup"><span data-stu-id="76e42-114">Add an `if` block that uses the return value from `ExistsAsync`.</span></span> <span data-ttu-id="76e42-115">キューから値を読み取ってブロックに入れるためのコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="76e42-115">We'll add our code to read a value from the queue into the block.</span></span> <span data-ttu-id="76e42-116">値が読み取られなかったことを示すメソッドに最終的な戻り値の文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="76e42-116">Add a final return string to the method that indicates no value was read.</span></span> <span data-ttu-id="76e42-117">メソッドは次のようになっているはずです。</span><span class="sxs-lookup"><span data-stu-id="76e42-117">Your method should be looking something like this:</span></span>

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
    }

    return "<queue empty or not created>";
}
```

1. <span data-ttu-id="76e42-118">`CloudQueue` オブジェクトに対して `GetMessageAsync` を呼び出して、キューから最初の `CloudQueueMessage` を取得します。</span><span class="sxs-lookup"><span data-stu-id="76e42-118">Call `GetMessageAsync` on the `CloudQueue` object to get the first `CloudQueueMessage` from the queue.</span></span> <span data-ttu-id="76e42-119">キューが空の場合、戻り値は `null` になります。</span><span class="sxs-lookup"><span data-stu-id="76e42-119">The return value will be `null` if the queue is empty.</span></span>

1. <span data-ttu-id="76e42-120">値が null 以外の場合は、`CloudQueueMessage` オブジェクト上で `AsString` プロパティを使用して、メッセージの内容を取得します。</span><span class="sxs-lookup"><span data-stu-id="76e42-120">If it's non-null, use the `AsString` property on the `CloudQueueMessage` object to get the contents of the message.</span></span>

1. <span data-ttu-id="76e42-121">`CloudQueue` オブジェクトに対して `DeleteMessageAsync` を呼び出して、キューからメッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="76e42-121">Call `DeleteMessageAsync` on the `CloudQueue` object to delete the message from the queue.</span></span>

<span data-ttu-id="76e42-122">最終的なメソッドの実装は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="76e42-122">The final method implementation should resemble:</span></span>

```csharp
static async Task<string> ReceiveArticleAsync()
{
    CloudQueue queue = GetQueue();
    bool exists = await queue.ExistsAsync();
    if (exists)
    {
        CloudQueueMessage retrievedArticle = await queue.GetMessageAsync();
        if (retrievedArticle != null)
        {
            string newsMessage = retrievedArticle.AsString;
            await queue.DeleteMessageAsync(retrievedArticle);
            return newsMessage;
        }
    }

    return "<queue empty or not created>";
}
```

## <a name="call-the-receivearticleasync-method"></a><span data-ttu-id="76e42-123">ReceiveArticleAsync メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="76e42-123">Call the ReceiveArticleAsync method</span></span>

<span data-ttu-id="76e42-124">最後に、この新しいメソッドを呼び出すためのサポートを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="76e42-124">Finally, let's add support to invoke our new method.</span></span> <span data-ttu-id="76e42-125">プログラムにパラメーターを渡さない場合に、これを行います。</span><span class="sxs-lookup"><span data-stu-id="76e42-125">We'll do this when we don't pass any parameters into the program.</span></span>

1. <span data-ttu-id="76e42-126">`Main` メソッドを検索します。具体的には、前にパラメーターを検索するために追加した `if` ブロックを検索します。</span><span class="sxs-lookup"><span data-stu-id="76e42-126">Locate the `Main` method and specifically the `if` block you added earlier to look for parameters.</span></span>

1. <span data-ttu-id="76e42-127">`else` 条件を追加して、`ReceiveArticleAsync` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="76e42-127">Add an `else` condition and call the `ReceiveArticleAsync` method.</span></span> 

1. <span data-ttu-id="76e42-128">非同期なので、`await` キーワードを使用して結果を取得し、コンソール ウィンドウに出力します。</span><span class="sxs-lookup"><span data-stu-id="76e42-128">Since it's asynchronous, use the `await` keyword to retrieve the result and print it to the console window.</span></span> <span data-ttu-id="76e42-129">アプリを C# 7.1 に変換しなかった場合、返すタスクから `Result` プロパティを使用して値を取得できます。</span><span class="sxs-lookup"><span data-stu-id="76e42-129">If you didn't convert your app to C# 7.1, you can get the value using the `Result` property from the returning task.</span></span>

<span data-ttu-id="76e42-130">コードは次のような内容になります。</span><span class="sxs-lookup"><span data-stu-id="76e42-130">Your code should look something like:</span></span>

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = await ReceiveArticleAsync();
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a><span data-ttu-id="76e42-131">アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="76e42-131">Execute the application</span></span>

<span data-ttu-id="76e42-132">これでコードは完成しました。</span><span class="sxs-lookup"><span data-stu-id="76e42-132">The code is now complete.</span></span> <span data-ttu-id="76e42-133">このコードでは、メッセージの送信および取得を行えるようになりました。</span><span class="sxs-lookup"><span data-stu-id="76e42-133">It can now send and retrieve messages.</span></span> 

> [!WARNING]
> <span data-ttu-id="76e42-134">必ず、オンライン エディターですべてのファイルが保存されていることを確認してから、プログラムをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="76e42-134">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="76e42-135">これをテストするには、`dotnet run` を使用します。メッセージを送信するにはパラメーターを渡し、1 個のメッセージを読み取るにはパラメーターをオフのままとします。</span><span class="sxs-lookup"><span data-stu-id="76e42-135">To test it, use `dotnet run` and pass parameters to send messages, and leave off parameters to read a single message.</span></span>

<span data-ttu-id="76e42-136">キューが存在しないときのテストを行う場合は、Azure CLI を使用してキュー (およびすべてのデータ) を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="76e42-136">If you want to test when a queue doesn't exist, you can delete the queue (and all the data) with the Azure CLI.</span></span> <span data-ttu-id="76e42-137">必ず、`<connection-string>` パラメーターを置換 (または環境変数を設定) します。</span><span class="sxs-lookup"><span data-stu-id="76e42-137">Make sure to replace the `<connection-string>` parameter (or set the environment variable).</span></span>

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="76e42-138">次にメッセージを追加すると、キューが再度作成されます。</span><span class="sxs-lookup"><span data-stu-id="76e42-138">The next time you add a message, the queue should be re-created.</span></span>

> [!NOTE]
> <span data-ttu-id="76e42-139">実際には、削除操作は非同期で実行されます。</span><span class="sxs-lookup"><span data-stu-id="76e42-139">The delete operation actually occurs asynchronously.</span></span> <span data-ttu-id="76e42-140">完了していない場合、キューを再作成しようとすると例外が発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="76e42-140">If it has not completed you may get an exception when you attempt to re-create the queue.</span></span>