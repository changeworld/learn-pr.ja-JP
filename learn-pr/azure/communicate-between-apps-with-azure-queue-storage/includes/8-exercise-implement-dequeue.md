<span data-ttu-id="5e0c0-101">ここでは、キュー内の次のメッセージを読み取って処理した後にそのメッセージをキューから削除するコードを記述して、アプリケーションを完成させます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-101">Now we want to complete the application by writing code to read the next message in the queue, process it, and delete it from the queue.</span></span> 

<span data-ttu-id="5e0c0-102">パラメーターが渡されなかった場合は、このコードを前と同じアプリケーションに配置して実行しますが、ニュース サービス シナリオでは、実際にこのコードを中間層サーバーに配置してストーリーを処理します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-102">We're going to place this code into the same application and execute it when you don't pass any parameters, however in our news service scenario, we would really place this code into our middle-tier servers to process the stories.</span></span>

## <a name="dequeue-a-message"></a><span data-ttu-id="5e0c0-103">メッセージをデキューする</span><span class="sxs-lookup"><span data-stu-id="5e0c0-103">Dequeue a message</span></span>

<span data-ttu-id="5e0c0-104">次のメッセージをキューから取得する新しいメソッドを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-104">Let's add a new method that retrieves the next message from the queue.</span></span>

1. <span data-ttu-id="5e0c0-105">Cloud Shell で `QueueApp` フォルダーに切り替えて、「`code .`」と入力してオンライン エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-105">Switch to the `QueueApp` folder in the Cloud Shell and type `code .` to open the online editor.</span></span>
 
1. <span data-ttu-id="5e0c0-106">`Program.cs` ソース ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-106">Open the `Program.cs` source file.</span></span>

1. <span data-ttu-id="5e0c0-107">パラメーターを取らず、`Task<string>` を返す `ReceiveArticleAsync` という名前の `Program` クラス内で静的メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-107">Create a static method in the `Program` class named `ReceiveArticleAsync` that takes no parameters and returns a `Task<string>`.</span></span> <span data-ttu-id="5e0c0-108">このメソッドを使用して、キューからニュース記事をプルし、それを返します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-108">We'll use this method to pull a news article from the queue and return it.</span></span>
    - <span data-ttu-id="5e0c0-109">さらに、メソッドに `async` キーワードを追加します。これは非同期の `Task` ベースのメソッドを使用するためです。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-109">Go ahead and add the `async` keyword to the method since we'll be using some asynchronous `Task`-based methods.</span></span>

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
    
1. <span data-ttu-id="5e0c0-110">ご自分の `ReceiveArticleAsync` メソッド内で新しい`GetQueue` メソッドを呼び出して、キュー参照を取得し、それを変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-110">In your `ReceiveArticleAsync` method, call the new `GetQueue` method to retrieve your queue reference and assign it to a variable.</span></span>

1. <span data-ttu-id="5e0c0-111">次に、`CloudQueue` オブジェクトに対して `ExistsAsync` メソッドを呼び出します。これにより、キューが作成されているかどうかが返されます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-111">Next, call the `ExistsAsync` method on the `CloudQueue` object; this will return whether the queue has been created.</span></span> <span data-ttu-id="5e0c0-112">存在しないキューからメッセージを取得しようとすると、API から例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-112">If we attempt to retrieve a message from a non-existent queue, the API will throw an exception.</span></span>
    - <span data-ttu-id="5e0c0-113">このメソッドは非同期であるため、`await` を使用して戻り値を取得します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-113">This method is asynchronous so use `await` to get the return value.</span></span>
    - <span data-ttu-id="5e0c0-114">`ReceiveArticleAsync` メソッドには `async` キーワードが既に含まれているはずです。そうでない場合はここで追加します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-114">You should already have the `async` keyword on the `ReceiveArticleAsync` method, but if not add it now.</span></span>


1. <span data-ttu-id="5e0c0-115">`ExistsAsync` からの戻り値を使用する `if` ブロックを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-115">Add an `if` block that uses the return value from `ExistsAsync`.</span></span> <span data-ttu-id="5e0c0-116">キューから値を読み取ってブロックに入れるためのコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-116">We'll add our code to read a value from the queue into the block.</span></span> <span data-ttu-id="5e0c0-117">値が読み取られなかったことを示すメソッドに最終的な戻り値の文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-117">Add a final return string to the method that indicates no value was read.</span></span> <span data-ttu-id="5e0c0-118">メソッドは次のようになっているはずです。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-118">Your method should be looking something like this:</span></span>

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

1. <span data-ttu-id="5e0c0-119">`CloudQueue` オブジェクトに対して `GetMessageAsync` を呼び出して、キューから最初の `CloudQueueMessage` を取得します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-119">Call `GetMessageAsync` on the `CloudQueue` object to get the first `CloudQueueMessage` from the queue.</span></span> <span data-ttu-id="5e0c0-120">キューが空の場合、戻り値は `null` になります。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-120">The return value will be `null` if the queue is empty.</span></span>

1. <span data-ttu-id="5e0c0-121">値が null 以外の場合は、`CloudQueueMessage` オブジェクト上で `AsString` プロパティを使用して、メッセージの内容を取得します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-121">If it's non-null, use the `AsString` property on the `CloudQueueMessage` object to get the contents of the message.</span></span>

1. <span data-ttu-id="5e0c0-122">`CloudQueue` オブジェクトに対して `DeleteMessageAsync` を呼び出して、キューからメッセージを削除します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-122">Call `DeleteMessageAsync` on the `CloudQueue` object to delete the message from the queue.</span></span>

<span data-ttu-id="5e0c0-123">最終的なメソッドの実装は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-123">The final method implementation should resemble:</span></span>

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

## <a name="call-the-receivearticleasync-method"></a><span data-ttu-id="5e0c0-124">ReceiveArticleAsync メソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="5e0c0-124">Call the ReceiveArticleAsync method</span></span>

<span data-ttu-id="5e0c0-125">最後に、この新しいメソッドを呼び出すためのサポートを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-125">Finally, let's add support to invoke our new method.</span></span> <span data-ttu-id="5e0c0-126">プログラムにパラメーターを渡さない場合に、これを行います。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-126">We'll do this when we don't pass any parameters into the program.</span></span>

1. <span data-ttu-id="5e0c0-127">`Main` メソッドを検索します。具体的には、前にパラメーターを検索するために追加した `if` ブロックを検索します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-127">Locate the `Main` method and specifically the `if` block you added earlier to look for parameters.</span></span>

1. <span data-ttu-id="5e0c0-128">`else` 条件を追加して、`ReceiveArticleAsync` メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-128">Add an `else` condition and call the `ReceiveArticleAsync` method.</span></span> 

1. <span data-ttu-id="5e0c0-129">これは非同期であるため、通常は `await` を使用しますが、前述したように C# のすべてのバージョンでそれが動作するわけではありません。そのため、`Result` プロパティのみを使用して戻り値を取得し、それをコンソール ウィンドウに出力します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-129">Since it's asynchronous, we would normally want to use `await`, however as explained earlier that doesn't work in all versions of C# - so just use the `Result` property to get the return value and print it to the console window.</span></span>

<span data-ttu-id="5e0c0-130">コードは次のような内容になります。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-130">Your code should look something like:</span></span>

```csharp
if (args.Length > 0)
{
    // ...
}
else
{
    string value = ReceiveArticleAsync().Result;
    Console.WriteLine($"Received {value}");
}
```

## <a name="execute-the-application"></a><span data-ttu-id="5e0c0-131">アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="5e0c0-131">Execute the application</span></span>

<span data-ttu-id="5e0c0-132">これでコードは完成しました。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-132">The code is now complete.</span></span> <span data-ttu-id="5e0c0-133">このコードでは、メッセージの送信および取得を行えるようになりました。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-133">It can now send and retrieve messages.</span></span> <span data-ttu-id="5e0c0-134">これをテストするには、`dotnet run` を使用します。メッセージを送信するにはパラメーターを渡し、1 個のメッセージを読み取るにはパラメーターをオフのままとします。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-134">To test it, use `dotnet run` and pass parameters to send messages, and leave off parameters to read a single message.</span></span>

<span data-ttu-id="5e0c0-135">キューが存在しないときのテストを行う場合は、Azure CLI を使用してキュー (およびすべてのデータ) を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-135">If you want to test when a queue doesn't exist, you can delete the queue (and all the data) with the Azure CLI.</span></span> <span data-ttu-id="5e0c0-136">必ず、`<connection-string>` パラメーターを置換 (または環境変数を設定) します。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-136">Make sure to replace the `<connection-string>` parameter (or set the environment variable).</span></span>

```azurecli
az storage queue delete --name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="5e0c0-137">次にメッセージを追加すると、キューが再度作成されます。</span><span class="sxs-lookup"><span data-stu-id="5e0c0-137">The next time you add a message, the queue should be re-created.</span></span>