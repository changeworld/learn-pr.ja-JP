<span data-ttu-id="975da-101">これですべての要件が満たされましたので、新しいストレージ キューの作成およびメッセージの追加を行うコードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="975da-101">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span> <span data-ttu-id="975da-102">通常、このコードは、データを生成するフロント エンド アプリ内に置きます。</span><span class="sxs-lookup"><span data-stu-id="975da-102">We would typically place this code in our front-end apps that generate the data.</span></span>

## <a name="add-the-client-library-for-azure-storage"></a><span data-ttu-id="975da-103">Azure Storage 用クライアント ライブラリを追加する</span><span class="sxs-lookup"><span data-stu-id="975da-103">Add the client library for Azure Storage</span></span>

<span data-ttu-id="975da-104">**.NET 用 Azure Storage クライアント ライブラリ**をアプリに追加することから始めてみましょう。</span><span class="sxs-lookup"><span data-stu-id="975da-104">Let's start by adding the **Azure Storage Client Library for .NET** to our app.</span></span> <span data-ttu-id="975da-105">これは、NuGet (.NET パッケージ マネージャー) を使用してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="975da-105">It can be installed with NuGet (a .NET package manager).</span></span> 

> [!NOTE]
> <span data-ttu-id="975da-106">新しい Cloud Shell が作成されましたが、ご利用のファイルはまだすべて存在しています。</span><span class="sxs-lookup"><span data-stu-id="975da-106">Even though a new Cloud Shell was created, your files are all still present.</span></span> <span data-ttu-id="975da-107">ただし、今度はご利用のストレージ領域の root が表示されるので、`QueueApp` フォルダーに切り替える必要があります。</span><span class="sxs-lookup"><span data-stu-id="975da-107">However, you will need to switch into the `QueueApp` folder since you will now be at the root of your storage area.</span></span> <span data-ttu-id="975da-108">「`cd QueueApp`」と入力して、フォルダーを切り替えます。</span><span class="sxs-lookup"><span data-stu-id="975da-108">Just type `cd QueueApp` to switch folders.</span></span> <span data-ttu-id="975da-109">.NET アプリはそのフォルダー内にあるので、パッケージの追加を試みる前に必ずその操作を行います。</span><span class="sxs-lookup"><span data-stu-id="975da-109">Make sure to do this before attempting to add the package since the .NET app is in that folder.</span></span>

1. <span data-ttu-id="975da-110">現在のディレクトリを `QueueApp` フォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="975da-110">Change the current directory to the `QueueApp` folder.</span></span>

1. <span data-ttu-id="975da-111">`dotnet add package` コマンドを使用してプロジェクトに `WindowsAzure.Storage` パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="975da-111">Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.</span></span>

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a><span data-ttu-id="975da-112">ニュース速報を送信するメソッドを追加する</span><span class="sxs-lookup"><span data-stu-id="975da-112">Add a method to send a news alert</span></span>

<span data-ttu-id="975da-113">次に、ニュース記事をキューに送信する新しいメソッドを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="975da-113">Next, let's create a new method to send a news story into a queue.</span></span>

1. <span data-ttu-id="975da-114">「`code .`」と入力して、コード エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="975da-114">Open the code editor by typing `code .`</span></span>

1. <span data-ttu-id="975da-115">`Program.cs` ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="975da-115">Open the `Program.cs` file.</span></span>

1. <span data-ttu-id="975da-116">ファイルの先頭に次の名前空間を追加します。</span><span class="sxs-lookup"><span data-stu-id="975da-116">At the top of the file, add the following namespaces.</span></span> <span data-ttu-id="975da-117">次の両方からの型を使用して Azure Storage に接続して、キューを操作します。</span><span class="sxs-lookup"><span data-stu-id="975da-117">We'll be using types from both of these to connect to Azure Storage and then to work with queues.</span></span>
    - <span data-ttu-id="975da-118">Microsoft.WindowsAzure.Storage</span><span class="sxs-lookup"><span data-stu-id="975da-118">Microsoft.WindowsAzure.Storage</span></span>
    - <span data-ttu-id="975da-119">Microsoft.WindowsAzure.Storage.Queue</span><span class="sxs-lookup"><span data-stu-id="975da-119">Microsoft.WindowsAzure.Storage.Queue</span></span>

1. <span data-ttu-id="975da-120">`string` を取り、`Task` を返す `SendArticleAsync` という名前の `Program` クラス内に静的メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="975da-120">Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`.</span></span> <span data-ttu-id="975da-121">このメソッドを使用して、利用しているサービスにニュース記事を送信します。</span><span class="sxs-lookup"><span data-stu-id="975da-121">We'll use this method to send a news article in to our service.</span></span> <span data-ttu-id="975da-122">次に示すように、入力パラメーターの名前を `newsMesasage` とします。</span><span class="sxs-lookup"><span data-stu-id="975da-122">Name the input parameter `newsMesasage` as shown below.</span></span>

    ```csharp
    ...
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Queue; 
    
    class Program
    {
        ...
    
        static async Task SendArticleAsync(string newsMessage)
        {
        }
    }
    ```
    
1. <span data-ttu-id="975da-123">ご自分の新しいメソッド内で、静的メソッド `CloudStorageAccount.Parse` を使用してご利用の接続文字列を解析します (それを定数文字列内に配置したことを思い出してください)。</span><span class="sxs-lookup"><span data-stu-id="975da-123">In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string).</span></span> <span data-ttu-id="975da-124">このメソッドからは、変数内に格納する必要がある `CloudStorageAccount` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="975da-124">This method returns a `CloudStorageAccount` object that needs to be stored in a variable.</span></span>

1. <span data-ttu-id="975da-125">ストレージ アカウント オブジェクトに対して `CreateCloudQueueClient()` メソッドを呼び出すことで、クライアント オブジェクトを取得し、それを変数内に格納します。</span><span class="sxs-lookup"><span data-stu-id="975da-125">Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.</span></span>

1. <span data-ttu-id="975da-126">次に、そのクライアント オブジェクトに対して `GetQueueReference` メソッドを呼び出し、キュー名として文字列 `"newsqueue"` を渡します。</span><span class="sxs-lookup"><span data-stu-id="975da-126">Next, call `GetQueueReference` method on the client object and pass the string `"newsqueue"` for the queue name.</span></span> <span data-ttu-id="975da-127">これにより、キューを操作するのに使用できる `CloudQueue` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="975da-127">This returns a `CloudQueue` object that we can use to work with the queue.</span></span> <span data-ttu-id="975da-128">キューがまだ存在していなくても問題ありません。</span><span class="sxs-lookup"><span data-stu-id="975da-128">It's OK if the queue does not exist yet.</span></span>

1. <span data-ttu-id="975da-129">`CloudQueue` オブジェクトに対して `CreateIfNotExistsAsync()` を呼び出して、確実にキューが使用できる状態になるようにします。</span><span class="sxs-lookup"><span data-stu-id="975da-129">Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use.</span></span> <span data-ttu-id="975da-130">この場合は、必要に応じて、キューが作成されます。</span><span class="sxs-lookup"><span data-stu-id="975da-130">This will create the queue if necessary.</span></span>
    - <span data-ttu-id="975da-131">これは非同期メソッドであるため、C# の `await` キーワードを使用して、確実にその適切な操作が行えるようにします。</span><span class="sxs-lookup"><span data-stu-id="975da-131">Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it.</span></span> <span data-ttu-id="975da-132">そのことはまた、_メソッド_を `async` キーワードで修飾する必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="975da-132">That also means you need to decorate the _method_ with the `async` keyword.</span></span> 
    - <span data-ttu-id="975da-133">`CreateIfNotExistsAsync` によって `bool` 値が返されます。その値はキューが作成された場合は `true` となり、キューが既に存在する場合は `false` となります。</span><span class="sxs-lookup"><span data-stu-id="975da-133">`CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists.</span></span> <span data-ttu-id="975da-134">キューを作成した場合は、コンソールにメッセージが出力されます。</span><span class="sxs-lookup"><span data-stu-id="975da-134">Output a message to the console if we created the queue.</span></span>
    - <span data-ttu-id="975da-135">ヘルプが必要な場合は、次の例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="975da-135">Here's an example if you need some help.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. <span data-ttu-id="975da-136">`CloudQueueMessage` のインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="975da-136">Create an instance of a `CloudQueueMessage`.</span></span> 
    - <span data-ttu-id="975da-137">これは `string` パラメーターを取り、メソッド パラメーターを渡します (`newsMessage`)。</span><span class="sxs-lookup"><span data-stu-id="975da-137">It takes a `string` parameter, pass in the method parameter (`newsMessage`).</span></span> <span data-ttu-id="975da-138">これは、メッセージの_本文_となります。</span><span class="sxs-lookup"><span data-stu-id="975da-138">This will be the _body_ of the message.</span></span> <span data-ttu-id="975da-139">バイナリ メッセージ ペイロードを作成できる  という名前の静的メソッドもあります。</span><span class="sxs-lookup"><span data-stu-id="975da-139">There is also a static method named  that can create a binary message payload.</span></span>
    

1. <span data-ttu-id="975da-140">`CloudQueue` オブジェクトに対して `AddMessageAsync` を呼び出して、キューにメッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="975da-140">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span> <span data-ttu-id="975da-141">これも非同期メソッドであるので、確実にその適切な操作が行えるようにするために `await` キーワードを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="975da-141">This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.</span></span>

1. <span data-ttu-id="975da-142">ファイルを保存し、コマンド ウィンドウに「`dotnet build`」と入力してそれをビルドします。</span><span class="sxs-lookup"><span data-stu-id="975da-142">Save the file and build it by typing `dotnet build` into the command window.</span></span> <span data-ttu-id="975da-143">エラーがある場合は修正します。次のコードを使用して、ご自分の作業内容をチェックすることができます。</span><span class="sxs-lookup"><span data-stu-id="975da-143">Fix any errors, you can use the following code to check your work.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference(QueueName);
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a><span data-ttu-id="975da-144">メッセージを送信するためのコードを追加する</span><span class="sxs-lookup"><span data-stu-id="975da-144">Add code to send a message</span></span>

<span data-ttu-id="975da-145">この新しい送信メソッドをテストするために、受信したパラメーターが新しいメソッドに渡されるように `Main` メソッドを修正してみましょう。</span><span class="sxs-lookup"><span data-stu-id="975da-145">Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.</span></span>

1. <span data-ttu-id="975da-146">`Main` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="975da-146">Locate the `Main` method.</span></span>

1. <span data-ttu-id="975da-147">渡された `args` パラメーターを調べて、任意のデータがコマンドラインに渡されたかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="975da-147">Check the passed `args` parameter to see if any data was passed to the command line.</span></span> <span data-ttu-id="975da-148">そうである場合は、`String.Join` を使用して、区切り記号としてスペースを使用しているすべての単語から、単一の文字列を作成します。</span><span class="sxs-lookup"><span data-stu-id="975da-148">If so, use `String.Join` to create a single string from all the words using a space as the separator.</span></span>

1. <span data-ttu-id="975da-149">それを新しい `SendArticleAsync` メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="975da-149">Pass that to the new `SendArticleAsync` method.</span></span> 

1. <span data-ttu-id="975da-150">そのメソッドから制御が返されたら、`Console.WriteLine` を使用して、送信してあるメッセージを出力します。</span><span class="sxs-lookup"><span data-stu-id="975da-150">Once it returns, use `Console.WriteLine` to output the message we sent.</span></span>

1. <span data-ttu-id="975da-151">ファイルを保存し、プログラムをビルドします (`dotnet build`)。次のコードを使用することで、作業内容を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="975da-151">Save the file and build the program (`dotnet build`), you can use the code below to check your work.</span></span>

> [!NOTE]
> <span data-ttu-id="975da-152">このメソッドは技術的には非同期であるため、通常は、`await` キーワードを使用します。ただし、その機能は C# 7.2 を使用していない限り、ご自分の `Main` では利用できません (それは新しい機能です)。</span><span class="sxs-lookup"><span data-stu-id="975da-152">Since our method is technically asynchronous, we normally would want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.2 (it's a new feature).</span></span> <span data-ttu-id="975da-153">これを言語の以前のバージョンで確実に使用できるようにするためには、メソッド上で `Wait()` を呼び出すだけとして、そのメソッドから制御が戻されるのを実際にブロック状態で待機します。</span><span class="sxs-lookup"><span data-stu-id="975da-153">To ensure we can use this on older versions of the language, just call `Wait()` on the method to actually block waiting for the method to return.</span></span>

    ```csharp
    static void Main(string[] args)
    {
        if (args.Length > 0)
        {
            string value = String.Join(" ", args);
            SendArticleAsync(value).Wait();
            Console.WriteLine($"Sent: {value}");
        }
    }
    ```

## <a name="execute-the-application"></a><span data-ttu-id="975da-154">アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="975da-154">Execute the application</span></span>

<span data-ttu-id="975da-155">アプリケーションでは、メッセージを送信できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="975da-155">The application can now send messages.</span></span> <span data-ttu-id="975da-156">それをテストするには、`dotnet run` コマンドを使用してコマンド ラインから実行します。</span><span class="sxs-lookup"><span data-stu-id="975da-156">To test it, you can run it from the command line with the `dotnet run` command.</span></span> <span data-ttu-id="975da-157">任意の追加の文字列がパラメーターとしてアプリケーションに渡されます。</span><span class="sxs-lookup"><span data-stu-id="975da-157">Any additional strings are passed as parameters to the application.</span></span> <span data-ttu-id="975da-158">たとえば、次のように入力することができます。</span><span class="sxs-lookup"><span data-stu-id="975da-158">As an example, you can type:</span></span>

    ```azurecli
    dotnet run Send this message
    ```

> [!WARNING]
> <span data-ttu-id="975da-159">必ず、オンライン エディターですべてのファイルが保存されていることを確認してから、プログラムをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="975da-159">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="975da-160">これにより、文字列 `"Send this message"` がキューに追加されます。</span><span class="sxs-lookup"><span data-stu-id="975da-160">This should add the string `"Send this message"` into the queue.</span></span>

## <a name="check-your-results"></a><span data-ttu-id="975da-161">結果を確認する</span><span class="sxs-lookup"><span data-stu-id="975da-161">Check your results</span></span>

<span data-ttu-id="975da-162">Azure portal 内で **ストレージ エクスプローラー** を使用してキューを確認することができます。その製品を開くと、自分が所有している各ストレージ アカウント内のデータを探索できるようになります。</span><span class="sxs-lookup"><span data-stu-id="975da-162">You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.</span></span>

<span data-ttu-id="975da-163">別の方法として、Azure CLI または PowerShell を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="975da-163">Alternatively, you can use the Azure CLI or PowerShell.</span></span> <span data-ttu-id="975da-164">このコマンドをシェル内で試してみます。それには、`<connection-string>` 値をご自分の特定の接続文字列に置換します。</span><span class="sxs-lookup"><span data-stu-id="975da-164">Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:</span></span>

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="975da-165">これによりご自分のメッセージに関する情報がダンプされます。それは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="975da-165">This should dump the information for your message, which will look something like this:</span></span>

```json
[
  {
    "content": "U2VuZCB0aGlzIG1lc3NhZ2U=",
    "dequeueCount": 0,
    "expirationTime": "2018-09-05T02:43:40+00:00",
    "id": "b47cbe9f-a246-4a81-86ae-fa02ea8d98bc",
    "insertionTime": "2018-09-24T02:43:40+00:00",
    "popReceipt": null,
    "timeNextVisible": null
  }
]
```

<span data-ttu-id="975da-166">上記のツールを使用して試すことができるコマンドが他にもいくつか用意されています。それらを調べるには、`az storage queue --help` と `az storage message --help` の両方をチェックアウトします。</span><span class="sxs-lookup"><span data-stu-id="975da-166">There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.</span></span>

> [!TIP]
> <span data-ttu-id="975da-167">`AZURE_STORAGE_CONNECTION_STRING` という名前の環境変数にご自分の接続文字列値を格納し、`--connection-string` を毎回入力しなくても済むようにします。</span><span class="sxs-lookup"><span data-stu-id="975da-167">Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!</span></span>

<span data-ttu-id="975da-168">これでメッセージの送信は完了しました。最後の手順は、メッセージを_受信_するためのサポートを追加することです。</span><span class="sxs-lookup"><span data-stu-id="975da-168">Now that we have sent a message, the last step is to add support to _receive_ the message.</span></span>