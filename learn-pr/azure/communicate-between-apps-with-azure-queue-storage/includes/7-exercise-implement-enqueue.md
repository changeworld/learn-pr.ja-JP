<span data-ttu-id="5e8db-101">これですべての要件が満たされましたので、新しいストレージ キューの作成およびメッセージの追加を行うコードを記述することができます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-101">Now that all the requirements are in place, you can write code that creates a new storage queue and adds a message.</span></span> <span data-ttu-id="5e8db-102">通常、このコードは、データを生成するフロント エンド アプリ内に置きます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-102">We would typically place this code in our front-end apps that generate the data.</span></span>

## <a name="add-the-client-library-for-azure-storage"></a><span data-ttu-id="5e8db-103">Azure Storage 用クライアント ライブラリを追加する</span><span class="sxs-lookup"><span data-stu-id="5e8db-103">Add the client library for Azure Storage</span></span>

<span data-ttu-id="5e8db-104">**.NET 用 Azure Storage クライアント ライブラリ**をアプリに追加することから始めてみましょう。</span><span class="sxs-lookup"><span data-stu-id="5e8db-104">Let's start by adding the **Azure Storage Client Library for .NET** to our app.</span></span> <span data-ttu-id="5e8db-105">これは、NuGet (.NET パッケージ マネージャー) を使用してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-105">It can be installed with NuGet (a .NET package manager).</span></span> 

1. <span data-ttu-id="5e8db-106">`dotnet add package` コマンドを使用してプロジェクトに `WindowsAzure.Storage` パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5e8db-106">Install the `WindowsAzure.Storage` package to the project with the `dotnet add package` command.</span></span> <span data-ttu-id="5e8db-107">これは、Cloud Shell の_下_のターミナルウィンドウで実行するか、ローカル コンピューターで作業している場合は、プロジェクトと同じフォルダーのターミナル/コンソール ウィンドウで実行します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-107">You can do this in the terminal window _below_ the Cloud Shell, or if you are working on your local computer, in a terminal/console window in the same folder as the project.</span></span>

```azurecli
dotnet add package WindowsAzure.Storage
```

## <a name="add-a-method-to-send-a-news-alert"></a><span data-ttu-id="5e8db-108">ニュース速報を送信するメソッドを追加する</span><span class="sxs-lookup"><span data-stu-id="5e8db-108">Add a method to send a news alert</span></span>

<span data-ttu-id="5e8db-109">次に、ニュース記事をキューに送信する新しいメソッドを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="5e8db-109">Next, let's create a new method to send a news story into a queue.</span></span>

1. <span data-ttu-id="5e8db-110">コード エディターで `Program.cs` ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-110">Open the `Program.cs` file in your code editor.</span></span>

1. <span data-ttu-id="5e8db-111">ファイルの先頭に次の名前空間を追加します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-111">At the top of the file, add the following namespaces.</span></span> <span data-ttu-id="5e8db-112">次の両方からの型を使用して Azure Storage に接続して、キューを操作します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-112">We'll be using types from both of these to connect to Azure Storage and then to work with queues.</span></span>

    - `System.Threading.Tasks`
    - `Microsoft.WindowsAzure.Storage`
    - `Microsoft.WindowsAzure.Storage.Queue`

1. <span data-ttu-id="5e8db-113">`string` を取り、`Task` を返す `SendArticleAsync` という名前の `Program` クラス内に静的メソッドを作成します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-113">Create a static method in the `Program` class named `SendArticleAsync` that takes a `string` and returns a `Task`.</span></span> <span data-ttu-id="5e8db-114">このメソッドを使用して、利用しているサービスにニュース記事を送信します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-114">We'll use this method to send a news article in to our service.</span></span> <span data-ttu-id="5e8db-115">次に示すように、入力パラメーターの名前を `newsMessage` とします。</span><span class="sxs-lookup"><span data-stu-id="5e8db-115">Name the input parameter `newsMessage` as shown below.</span></span>

    ```csharp
    ...
    using System.Threading.Tasks;
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
    
1. <span data-ttu-id="5e8db-116">ご自分の新しいメソッド内で、静的メソッド `CloudStorageAccount.Parse` を使用してご利用の接続文字列を解析します (それを定数文字列内に配置したことを思い出してください)。</span><span class="sxs-lookup"><span data-stu-id="5e8db-116">In your new method, use the static `CloudStorageAccount.Parse` method to parse your connection string (recall we placed it into a constant string).</span></span> <span data-ttu-id="5e8db-117">このメソッドからは、変数内に格納する必要がある `CloudStorageAccount` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-117">This method returns a `CloudStorageAccount` object that needs to be stored in a variable.</span></span>

1. <span data-ttu-id="5e8db-118">ストレージ アカウント オブジェクトに対して `CreateCloudQueueClient()` メソッドを呼び出すことで、クライアント オブジェクトを取得し、それを変数内に格納します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-118">Call the `CreateCloudQueueClient()` method on the storage account object to get a client object and store that in a variable.</span></span>

1. <span data-ttu-id="5e8db-119">次に、そのクライアント オブジェクトに対して `GetQueueReference` メソッドを呼び出し、キュー名として文字列 "newsqueue" を渡します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-119">Next, call `GetQueueReference` method on the client object and pass the string "newsqueue" for the queue name.</span></span> <span data-ttu-id="5e8db-120">これにより、キューを操作するのに使用できる `CloudQueue` オブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-120">This returns a `CloudQueue` object that we can use to work with the queue.</span></span> <span data-ttu-id="5e8db-121">キューがまだ存在していなくても問題ありません。</span><span class="sxs-lookup"><span data-stu-id="5e8db-121">It's OK if the queue does not exist yet.</span></span>

1. <span data-ttu-id="5e8db-122">`CloudQueue` オブジェクトに対して `CreateIfNotExistsAsync()` を呼び出して、確実にキューが使用できる状態になるようにします。</span><span class="sxs-lookup"><span data-stu-id="5e8db-122">Call `CreateIfNotExistsAsync()` on the `CloudQueue` object to ensure the queue is ready for use.</span></span> <span data-ttu-id="5e8db-123">この場合は、必要に応じて、キューが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-123">This will create the queue if necessary.</span></span>
    - <span data-ttu-id="5e8db-124">これは非同期メソッドであるため、C# の `await` キーワードを使用して、確実にその適切な操作が行えるようにします。</span><span class="sxs-lookup"><span data-stu-id="5e8db-124">Since this is an asynchronous method, use the C# `await` keyword to ensure we work properly with it.</span></span> <span data-ttu-id="5e8db-125">そのことはまた、_メソッド_を `async` キーワードで修飾する必要があることを意味します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-125">That also means you need to decorate the _method_ with the `async` keyword.</span></span> 
    - <span data-ttu-id="5e8db-126">`CreateIfNotExistsAsync` によって `bool` 値が返されます。その値はキューが作成された場合は `true` となり、キューが既に存在する場合は `false` となります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-126">`CreateIfNotExistsAsync` returns a `bool` value that will be `true` if the queue was created and `false` if it already exists.</span></span> <span data-ttu-id="5e8db-127">キューを作成した場合は、コンソールにメッセージが出力されます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-127">Output a message to the console if we created the queue.</span></span>
    - <span data-ttu-id="5e8db-128">ヘルプが必要な場合は、次の例を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5e8db-128">Here's an example if you need some help.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        // Not Shown here - code from prior steps
        ...
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    }
    ```

1. <span data-ttu-id="5e8db-129">`CloudQueueMessage` のインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-129">Create an instance of a `CloudQueueMessage`.</span></span> 
    - <span data-ttu-id="5e8db-130">これは `string` パラメーターを取り、メソッド パラメーターを渡します (`newsMessage`)。</span><span class="sxs-lookup"><span data-stu-id="5e8db-130">It takes a `string` parameter, pass in the method parameter (`newsMessage`).</span></span> <span data-ttu-id="5e8db-131">これは、メッセージの_本文_となります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-131">This will be the _body_ of the message.</span></span> <span data-ttu-id="5e8db-132">バイナリ メッセージ ペイロードを作成できる  という名前の静的メソッドもあります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-132">There is also a static method named  that can create a binary message payload.</span></span>

1. <span data-ttu-id="5e8db-133">`CloudQueue` オブジェクトに対して `AddMessageAsync` を呼び出して、キューにメッセージを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-133">Call `AddMessageAsync` on the `CloudQueue` object to add the message to the queue.</span></span> <span data-ttu-id="5e8db-134">これも非同期メソッドであるので、確実にその適切な操作が行えるようにするために `await` キーワードを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-134">This is also an asynchronous method and you will need to use the `await` keyword to ensure we properly interact with it.</span></span>

1. <span data-ttu-id="5e8db-135">ファイルを保存し、コマンド ウィンドウに「`dotnet build`」と入力してそれをビルドします。</span><span class="sxs-lookup"><span data-stu-id="5e8db-135">Save the file and build it by typing `dotnet build` into the command window.</span></span> <span data-ttu-id="5e8db-136">エラーがある場合は修正します。次のコードを使用して、ご自分の作業内容をチェックすることができます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-136">Fix any errors, you can use the following code to check your work.</span></span>

    ```csharp
    static async Task SendArticleAsync(string newsMessage)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
        CloudQueue queue = queueClient.GetQueueReference("newsqueue");
        bool createdQueue = await queue.CreateIfNotExistsAsync();
        if (createdQueue)
        {
            Console.WriteLine("The queue of news articles was created.");
        }
    
        CloudQueueMessage articleMessage = new CloudQueueMessage(newsMessage);
        await queue.AddMessageAsync(articleMessage);
    }
    ```

## <a name="add-code-to-send-a-message"></a><span data-ttu-id="5e8db-137">メッセージを送信するためのコードを追加する</span><span class="sxs-lookup"><span data-stu-id="5e8db-137">Add code to send a message</span></span>

<span data-ttu-id="5e8db-138">この新しい送信メソッドをテストするために、受信したパラメーターが新しいメソッドに渡されるように `Main` メソッドを修正してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5e8db-138">Let's modify the `Main` method to pass any parameters received into our new method so we can test our new send method.</span></span>

1. <span data-ttu-id="5e8db-139">`Main` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-139">Locate the `Main` method.</span></span>

1. <span data-ttu-id="5e8db-140">渡された `args` パラメーターを調べて、任意のデータがコマンドラインに渡されたかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-140">Check the passed `args` parameter to see if any data was passed to the command line.</span></span> <span data-ttu-id="5e8db-141">そうである場合は、`String.Join` を使用して、区切り記号としてスペースを使用しているすべての単語から、単一の文字列を作成します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-141">If so, use `String.Join` to create a single string from all the words using a space as the separator.</span></span>

1. <span data-ttu-id="5e8db-142">それを新しい `SendArticleAsync` メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-142">Pass that to the new `SendArticleAsync` method.</span></span> 

1. <span data-ttu-id="5e8db-143">そのメソッドから制御が返されたら、`Console.WriteLine` を使用して、送信してあるメッセージを出力します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-143">Once it returns, use `Console.WriteLine` to output the message we sent.</span></span>

1. <span data-ttu-id="5e8db-144">ファイルを保存し、プログラムをビルドします (`dotnet build`)。次のコードを使用することで、作業内容を確認することができます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-144">Save the file and build the program (`dotnet build`), you can use the code below to check your work.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5e8db-145">このメソッドは技術的には非同期であるため、`await` キーワードを使用したいです。ただし、この機能は C# 7.1 以降を使用していない限り、ご自分の `Main` メソッドでは利用できません。</span><span class="sxs-lookup"><span data-stu-id="5e8db-145">Since our method is technically asynchronous, we will want to use the `await` keyword, however that feature isn't available on your `Main` method unless you are using C# 7.1 or later.</span></span> <span data-ttu-id="5e8db-146">メソッドで単に `Wait()` を呼び出し、メソッドの応答の待機を実際にブロックします。これは 1 分以内に修正します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-146">Just call `Wait()` on the method to actually block waiting for the method to return, we'll fix that in a minute.</span></span>

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

## <a name="execute-the-application"></a><span data-ttu-id="5e8db-147">アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="5e8db-147">Execute the application</span></span>

<span data-ttu-id="5e8db-148">アプリケーションでは、メッセージを送信できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="5e8db-148">The application can now send messages.</span></span> <span data-ttu-id="5e8db-149">それをテストするには、`dotnet run` コマンドを使用してコマンド ラインから実行します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-149">To test it, you can run it from the command line with the `dotnet run` command.</span></span> <span data-ttu-id="5e8db-150">任意の追加の文字列がパラメーターとしてアプリケーションに渡されます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-150">Any additional strings are passed as parameters to the application.</span></span> 

> [!WARNING]
> <span data-ttu-id="5e8db-151">必ず、オンライン エディターですべてのファイルが保存されていることを確認してから、プログラムをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-151">Make sure you have saved all the files in the online editor before you build and run the program.</span></span>

<span data-ttu-id="5e8db-152">たとえば、次のように入力することができます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-152">As an example, you can type:</span></span>

```azurecli
dotnet run Send this message
```

<span data-ttu-id="5e8db-153">これにより、文字列 `"Send this message"` がキューに追加されます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-153">This should add the string `"Send this message"` into the queue.</span></span>

## <a name="check-your-results"></a><span data-ttu-id="5e8db-154">結果を確認する</span><span class="sxs-lookup"><span data-stu-id="5e8db-154">Check your results</span></span>

<span data-ttu-id="5e8db-155">Azure portal 内で **ストレージ エクスプローラー** を使用してキューを確認することができます。その製品を開くと、自分が所有している各ストレージ アカウント内のデータを探索できるようになります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-155">You can check queues in the Azure portal using the **Storage Explorer**, if you open that product it will let you explore the data in each storage account you own.</span></span>

<span data-ttu-id="5e8db-156">別の方法として、Azure CLI または PowerShell を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-156">Alternatively, you can use the Azure CLI or PowerShell.</span></span> <span data-ttu-id="5e8db-157">このコマンドをシェル内で試してみます。それには、`<connection-string>` 値をご自分の特定の接続文字列に置換します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-157">Try this command in the shell, replacing the `<connection-string>` value with your specific connection string:</span></span>

```azurecli
az storage message peek --queue-name newsqueue --connection-string <connection-string> 
```

<span data-ttu-id="5e8db-158">これによりご自分のメッセージに関する情報がダンプされます。それは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-158">This should dump the information for your message, which will look something like this:</span></span>

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

<span data-ttu-id="5e8db-159">上記のツールを使用して試すことができるコマンドが他にもいくつか用意されています。それらを調べるには、`az storage queue --help` と `az storage message --help` の両方をチェックアウトします。</span><span class="sxs-lookup"><span data-stu-id="5e8db-159">There are several other commands available that you can try with the tools - check out both `az storage queue --help` and `az storage message --help` to explore them.</span></span>

> [!TIP]
> <span data-ttu-id="5e8db-160">`AZURE_STORAGE_CONNECTION_STRING` という名前の環境変数にご自分の接続文字列値を格納し、`--connection-string` を毎回入力しなくても済むようにします。</span><span class="sxs-lookup"><span data-stu-id="5e8db-160">Put your connection string value into an environment variable named `AZURE_STORAGE_CONNECTION_STRING` to save yourself from having to type the `--connection-string` parameter every time!</span></span>

## <a name="optional---use-the-async-versions-of-the-methods"></a><span data-ttu-id="5e8db-161">省略可能 - メソッドの非同期バージョンを使用する</span><span class="sxs-lookup"><span data-stu-id="5e8db-161">Optional - Use the async versions of the methods</span></span>

<span data-ttu-id="5e8db-162">上述の送信メソッドでは、`Wait()` を使用しましたが、これはコンピューター リソースを効率的に使用するものではありません。</span><span class="sxs-lookup"><span data-stu-id="5e8db-162">We used `Wait()` on the send method above but that's not an efficient use of our computing resources.</span></span> <span data-ttu-id="5e8db-163">代わりに C# の `async` と `await` のメソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-163">Instead, we want to use the C# `async` and `await` methods.</span></span> <span data-ttu-id="5e8db-164">ただし、これらのキーワードを **Main** メソッドに適用するには、C# 7.1 _以上_を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-164">However, we will need to be using _at least_ C# 7.1 to be able to apply these keywords to our **Main** method.</span></span>

### <a name="switch-to-c-71"></a><span data-ttu-id="5e8db-165">C# 7.1 に切り替える</span><span class="sxs-lookup"><span data-stu-id="5e8db-165">Switch to C# 7.1</span></span>

<span data-ttu-id="5e8db-166">C# 7.1 より前では、C# の `async` および `await` キーワードは **Main** メソッドで有効なキーワードではありませんでした。</span><span class="sxs-lookup"><span data-stu-id="5e8db-166">C#'s `async` and `await` keywords were not valid keywords in **Main** methods until C# 7.1.</span></span> <span data-ttu-id="5e8db-167">**.csproj** ファイルでフラグを使用することで、そのコンパイラに簡単に切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-167">We can easily switch to that compiler through a flag in the **.csproj** file.</span></span>

1. <span data-ttu-id="5e8db-168">エディターで **QueueApp.csproj** ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="5e8db-168">Open the **QueueApp.csproj** file in the editor.</span></span>

1. <span data-ttu-id="5e8db-169">ビルド ファイル内の最初の `PropertyGroup` に `<LangVersion>7.1</LangVersion>` を追加します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-169">Add `<LangVersion>7.1</LangVersion>` into the first `PropertyGroup` in the build file.</span></span> <span data-ttu-id="5e8db-170">完了すると、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-170">It should look like the following when you are finished.</span></span>
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a><span data-ttu-id="5e8db-171">async キーワードを適用する</span><span class="sxs-lookup"><span data-stu-id="5e8db-171">Apply the async keyword</span></span>

<span data-ttu-id="5e8db-172">次に、`async` キーワードを **Main** メソッドに適用します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-172">Next, apply the `async` keyword to the **Main** method.</span></span> <span data-ttu-id="5e8db-173">次の 3 つのことを行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="5e8db-173">We will have to do three things.</span></span>

1. <span data-ttu-id="5e8db-174">`async` キーワードを **Main** メソッド シグネチャに追加します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-174">Add the `async` keyword onto the **Main** method signature.</span></span>
1. <span data-ttu-id="5e8db-175">戻り値の型を `void` から `Task` に変更します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-175">Change the return type from `void` to `Task`.</span></span>
1. <span data-ttu-id="5e8db-176">`SendArticleAsync` への呼び出しから `Wait()` への呼び出しを削除し、`await` に変更します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-176">Remove the call to `Wait()` on the call to `SendArticleAsync` and replace it with `await`.</span></span>

<span data-ttu-id="5e8db-177">アプリを再実行します。アプリは以前と同様に動作するはずですが、メッセージがキューに入るのを待機している間、コードは .NET スレッドプールにスレッドをリリースして戻せるようになっているはずです。</span><span class="sxs-lookup"><span data-stu-id="5e8db-177">Try running the app again - it should still work exactly as before, but now the code is able to release the thread back to the .NET threadpool while we wait for the message to be queued.</span></span>

<span data-ttu-id="5e8db-178">これでメッセージの送信は完了しました。最後の手順では、メッセージを_受信_するためのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="5e8db-178">Now that we have sent a message, the last step is to add support to _receive_ the message.</span></span>