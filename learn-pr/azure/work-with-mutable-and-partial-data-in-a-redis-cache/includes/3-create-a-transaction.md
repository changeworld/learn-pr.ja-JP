<span data-ttu-id="29147-101">まず、Azure で Redis Cache インスタンスを作成してから、キャッシュに 2 つのデータ値を挿入する単純なトランザクションを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="29147-101">Let's start by creating a Redis cache instance in Azure, then create a simple transaction that inserts two data values into the cache.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="29147-102">Azure Redis Cache の作成</span><span class="sxs-lookup"><span data-stu-id="29147-102">Create an Azure Redis Cache</span></span>

<span data-ttu-id="29147-103">Azure CLI を使用して Azure Redis Cache の作成から始めましょう。</span><span class="sxs-lookup"><span data-stu-id="29147-103">Let's start by creating an Azure Redis Cache with the Azure CLI.</span></span> <span data-ttu-id="29147-104">Azure とやりとりするには、ブラウザー ウィンドウの右側にある Cloud Shell を使用します。</span><span class="sxs-lookup"><span data-stu-id="29147-104">Use the Cloud Shell on the right side of the browser window to interact with Azure.</span></span>

<span data-ttu-id="29147-105">`azure rediscache create` コマンドを使用して、新しい Azure Redis Cache を作成します。</span><span class="sxs-lookup"><span data-stu-id="29147-105">We'll use the `azure rediscache create` command to create a new Azure Redis Cache.</span></span> <span data-ttu-id="29147-106">これは、いくつかのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="29147-106">It takes several parameters.</span></span> <span data-ttu-id="29147-107">最も一般的なものを次に示します (完全なリストを取得するには、マニュアルを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="29147-107">Here are the most common (to get a full list, check the documentation).</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="29147-108">パラメーター</span><span class="sxs-lookup"><span data-stu-id="29147-108">Parameter</span></span> | <span data-ttu-id="29147-109">説明</span><span class="sxs-lookup"><span data-stu-id="29147-109">Description</span></span> |
> |-----------|-------------|
> | `--name`    | <span data-ttu-id="29147-110">キャッシュの名前: グローバルに一意の名前にし、文字、数字、ハイフンで構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29147-110">The name of the cache - this must be globally unique and composed of letters, numbers and dashes.</span></span> |
> | `--resource-group` | <span data-ttu-id="29147-111">Azure サンドボックスの一部である、事前に作成したリソース グループ <rgn>[サンドボックス リソース グループ名]</rgn> を使用します。</span><span class="sxs-lookup"><span data-stu-id="29147-111">Use the pre-created Resource Group <rgn>[Sandbox resource group name]</rgn> which is part of the Azure Sandbox.</span></span> |
> | `--location` | <span data-ttu-id="29147-112">キャッシュが配置される場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="29147-112">Specify the location where the cache should be located.</span></span> <span data-ttu-id="29147-113">通常は、データ コンシューマーに近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="29147-113">Normally, you will want to choose a location close to the data consumers.</span></span> <span data-ttu-id="29147-114">この場合は、Azure サンドボックス内で使用できる場所に制限されています。</span><span class="sxs-lookup"><span data-stu-id="29147-114">In this case, you are limited to the locations available in the Azure Sandbox.</span></span> <span data-ttu-id="29147-115">最も近いものを選択します。</span><span class="sxs-lookup"><span data-stu-id="29147-115">Select the closest one to you.</span></span> |
> | `--size` | <span data-ttu-id="29147-116">Redis Cache のサイズです。</span><span class="sxs-lookup"><span data-stu-id="29147-116">Size of the Redis Cache.</span></span> <span data-ttu-id="29147-117">有効な値: [C0、C1、C2、C3、C4、C5、C6、P1、P2、P3、P4]</span><span class="sxs-lookup"><span data-stu-id="29147-117">Valid values are [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span></span> |
> | `--sku` | <span data-ttu-id="29147-118">Redis SKU です。</span><span class="sxs-lookup"><span data-stu-id="29147-118">Redis SKU.</span></span> <span data-ttu-id="29147-119">有効な値: [Basic、Standard、Premium]</span><span class="sxs-lookup"><span data-stu-id="29147-119">Valid values are [Basic, Standard, Premium]</span></span> |
> | `--enable-non-ssl-port` | <span data-ttu-id="29147-120">キャッシュの非 SSL ポートを有効にする場合は、このフラグを追加します。</span><span class="sxs-lookup"><span data-stu-id="29147-120">Add this flag if you want to enable the Non SSL Port for your cache.</span></span> |

### <a name="selecting-a-location"></a><span data-ttu-id="29147-121">場所の選択</span><span class="sxs-lookup"><span data-stu-id="29147-121">Selecting a location</span></span>
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="29147-122">次のオプションを使用してキャッシュを作成します。</span><span class="sxs-lookup"><span data-stu-id="29147-122">Create a cache using the following options:</span></span>
    - <span data-ttu-id="29147-123">サイズ: C0</span><span class="sxs-lookup"><span data-stu-id="29147-123">Size: C0</span></span>
    - <span data-ttu-id="29147-124">SKU: Basic</span><span class="sxs-lookup"><span data-stu-id="29147-124">SKU: Basic</span></span>
    - <span data-ttu-id="29147-125">EnableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="29147-125">EnableNonSslPort</span></span>
    
1. <span data-ttu-id="29147-126">コマンドラインの例を次に示します。**[name]** と **[location]** は、必ず有効な値に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="29147-126">Here's an example command line, make sure to replace the **[name]** and **[location]** with valid values.</span></span>

    ```azurecli
    azure rediscache create \
        --name [name] \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location [location] \
        --size C0 --sku Basic
    ```

1. <span data-ttu-id="29147-127">キャッシュの作成には数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="29147-127">It will take a few minutes to create the cache.</span></span>

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="29147-128">.NET Core コンソール アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="29147-128">Create a .NET Core Console application</span></span>

<span data-ttu-id="29147-129">次に、Azure Redis Cache にデータ値を挿入するために使用される .NET Core C# ベースのコンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="29147-129">Next, create a .NET Core C#-based console application, which will be used to insert data values into our Azure Redis Cache.</span></span>

1. <span data-ttu-id="29147-130">ウィンドウの右側に統合された Cloud Shell を使用して、新しい .NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="29147-130">Create a new .NET Core application using the integrated Cloud Shell on the right hand side of the window.</span></span> <span data-ttu-id="29147-131">これに "RedisData" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="29147-131">Name it "RedisData".</span></span>

    ```bash
    dotnet new console --name RedisData
    ```
    
1. <span data-ttu-id="29147-132">アプリ用に作成した新しいディレクトリに変更します。</span><span class="sxs-lookup"><span data-stu-id="29147-132">Change into the new directory created for your app.</span></span>

    ```bash
    cd RedisData
    ```
    
1. <span data-ttu-id="29147-133">プロジェクト ファイルと、**Program.cs** ソース ファイルが 1 つあるはずです。</span><span class="sxs-lookup"><span data-stu-id="29147-133">You should find a project file, and a single **Program.cs** source file.</span></span>

1. <span data-ttu-id="29147-134">アプリケーションをビルドして実行します。"Hello, World!" と出力されます。</span><span class="sxs-lookup"><span data-stu-id="29147-134">Build and run the application - it should output "Hello, World!".</span></span>

    ```bash
    dotnet run
    ```
    
## <a name="add-the-servicestackredis-nuget-package"></a><span data-ttu-id="29147-135">ServiceStack.Redis NuGet パッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="29147-135">Add the ServiceStack.Redis NuGet package</span></span>

<span data-ttu-id="29147-136">コンソール アプリケーションを作成したので、**ServiceStack.Redis** NuGet パッケージを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29147-136">Now that we have our console application, we need to add the **ServiceStack.Redis** NuGet package.</span></span> <span data-ttu-id="29147-137">これにより、Redis Cache に接続して、C# でコマンドを発行できるようになります。</span><span class="sxs-lookup"><span data-stu-id="29147-137">This will allow us to connect to the Redis Cache and issue commands in C#.</span></span>

1. <span data-ttu-id="29147-138">ターミナル シェルを使用して、NuGet パッケージ **ServiceStack.Redis** を追加します。</span><span class="sxs-lookup"><span data-stu-id="29147-138">Add the NuGet package **ServiceStack.Redis** using the terminal shell.</span></span>

    ```bash
    dotnet add package ServiceStack.Redis
    ```
    
1. <span data-ttu-id="29147-139">すべてコンパイルされていることを確認するため、もう一度アプリケーションをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="29147-139">Build and run the application again to make sure it all compiles.</span></span> <span data-ttu-id="29147-140">ここでも "Hello, World!" が出力されるはずです。</span><span class="sxs-lookup"><span data-stu-id="29147-140">It should still output "Hello, World!"</span></span>

## <a name="get-your-azure-redis-cache-connection-string"></a><span data-ttu-id="29147-141">Azure Redis Cache の接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="29147-141">Get your Azure Redis Cache connection string</span></span>

<span data-ttu-id="29147-142">Azure Redis Cache に接続するには、パスワードと URL を含む接続文字列が必要です。</span><span class="sxs-lookup"><span data-stu-id="29147-142">To connect to your Azure Redis Cache, you need a connection string that contains your password and URL.</span></span> <span data-ttu-id="29147-143">この接続文字列は **ServiceStack.Redis** に固有のもので、`[password]@[host name]:[port]` の形式になります。</span><span class="sxs-lookup"><span data-stu-id="29147-143">This connection string is unique to **ServiceStack.Redis** and is in the form of: `[password]@[host name]:[port]`</span></span>

<span data-ttu-id="29147-144">このキーは、Azure portal またはコマンドラインを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="29147-144">You can retrieve this key with the Azure portal, or with the command line.</span></span> <span data-ttu-id="29147-145">ポータルによるアプローチは「**Optimize your web applications by caching read-only data with Redis**」(Redis で読み取り専用データをキャッシュすることにより Web アプリケーションを最適化する) モジュールで使用したため、ここでは後者を使用しましょう。</span><span class="sxs-lookup"><span data-stu-id="29147-145">Let's use the latter here since we used the portal approach in the **Optimize your web applications by caching read-only data with Redis** module.</span></span>

<span data-ttu-id="29147-146">`azure rediscache list-keys` コマンドを使用して、アクセス キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="29147-146">Use the `azure rediscache list-keys` command to get the access keys.</span></span> <span data-ttu-id="29147-147">次に示すように、名前とリソース グループを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29147-147">You will need to supply the name and resource group as shown below:</span></span>

```azurecli
azure rediscache list-keys \
    --name [name] \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="29147-148">次のような結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="29147-148">This will return something like</span></span>

```output
Primary Key   : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
Secondary Key : XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX=
```

1. <span data-ttu-id="29147-149">ご自分の**主**キーをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="29147-149">Copy your **Primary** key to the clipboard.</span></span>

1. <span data-ttu-id="29147-150">ホスト名は、キャッシュを作成したときにサフィックス `.redis.cache.windows.net` を付けてそのキャッシュに指定した名前になります。</span><span class="sxs-lookup"><span data-stu-id="29147-150">The hostname will be the name you gave to the cache when you created it with the suffix `.redis.cache.windows.net`.</span></span>

1. <span data-ttu-id="29147-151">ポートは **6379** になります。</span><span class="sxs-lookup"><span data-stu-id="29147-151">The port should be **6379**.</span></span>

1. <span data-ttu-id="29147-152">コンソール内のすべての情報を確認するには、アクティブなサブスクリプション (この場合は Azure サンドボックス) のすべての Redis Cache インスタンスを示す `azure rediscache list` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="29147-152">You can verify all of that information in the console with the `azure rediscache list` command which will show you all the Redis cache instances in your active subscription (in this case, the Azure Sandbox).</span></span>

## <a name="add-the-connection-string-to-your-app"></a><span data-ttu-id="29147-153">アプリに接続文字列を追加する</span><span class="sxs-lookup"><span data-stu-id="29147-153">Add the connection string to your app</span></span>

1. <span data-ttu-id="29147-154">アプリ フォルダーにアクセスしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29147-154">Make sure you are in the app folder.</span></span> <span data-ttu-id="29147-155">`ls` または `dir` を入力した場合、**Program.cs** は現在のフォルダーにあるはずです。</span><span class="sxs-lookup"><span data-stu-id="29147-155">The **Program.cs** should be in the current folder if you type `ls` or `dir`.</span></span>

1. <span data-ttu-id="29147-156">アプリ フォルダーに `code .` を入力して、組み込みのエディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="29147-156">Open the built-in editor by typing `code .` in the app folder.</span></span>

1. <span data-ttu-id="29147-157">**Program.cs** ソース ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="29147-157">Select the **Program.cs** source file.</span></span>

1. <span data-ttu-id="29147-158">`Program` クラスで次のフィールドを作成します。</span><span class="sxs-lookup"><span data-stu-id="29147-158">Create the following field in the `Program` class.</span></span>

    ```csharp
    static string redisConnectionString = "";
    ```

1. <span data-ttu-id="29147-159">先ほど作成した **redisConnectionString** フィールドに接続文字列を貼り付け、ポート番号として **6379** を使用します。</span><span class="sxs-lookup"><span data-stu-id="29147-159">Paste in your connection string in the **redisConnectionString** field you just created and use **6379** as your port number.</span></span> <span data-ttu-id="29147-160">接続文字列は `[password]@[host name]:[port]` の形式になっていることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="29147-160">Remember, your connection string is in the form of: `[password]@[host name]:[port]`</span></span>

<span data-ttu-id="29147-161">接続文字列の例は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="29147-161">An example connection string would look something like this:</span></span>

```output
ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6379
```
    
## <a name="insert-two-data-values-into-your-azure-redis-cache"></a><span data-ttu-id="29147-162">Azure Redis Cache に 2 つのデータ値を挿入する</span><span class="sxs-lookup"><span data-stu-id="29147-162">Insert two data values into your Azure Redis Cache</span></span>

<span data-ttu-id="29147-163">最後に、Azure Redis Cache にデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="29147-163">Finally, we're going to add data into your Azure Redis Cache.</span></span>

1. <span data-ttu-id="29147-164">**Program.cs** ファイルの先頭に次の using ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="29147-164">Add the following using statement to the top of the **Program.cs** file.</span></span>

    ```csharp
    using ServiceStack.Redis;
    ```

1. <span data-ttu-id="29147-165">**Main** メソッド内に次のコードのスニペットを追加します。</span><span class="sxs-lookup"><span data-stu-id="29147-165">Add the following snippet of code in your **Main** method.</span></span> <span data-ttu-id="29147-166">これにより、2 つの値が過渡的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="29147-166">This will add two values transitionally.</span></span>

    ```csharp
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        if(transactionResult)
            Console.WriteLine("Transaction committed");
        else
            Console.WriteLine("Transaction failed to commit");
    }
    ```
1. <span data-ttu-id="29147-167">エディター ウィンドウの下部にあるコマンド プロンプトでアプリケーションを実行し、コンソールに「**トランザクションをコミットしました**」と表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29147-167">Run the application through the command prompt at the bottom of the editor window and verify that the console says **Transaction committed**.</span></span> 

    ```bash
    dotnet run
    ```
    
## <a name="verify-your-data"></a><span data-ttu-id="29147-168">データの検証</span><span class="sxs-lookup"><span data-stu-id="29147-168">Verify your data</span></span>

<span data-ttu-id="29147-169">完了するため、追加したデータが Azure Redis Cache にあることを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="29147-169">To finish off, let's verify that the data we added is in our Azure Redis Cache.</span></span>

1. <span data-ttu-id="29147-170">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="29147-170">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="29147-171">左側のサイドバーで **[すべてのリソース]** を選択して Redis Cache を見つけ、左のフィルター ボックスを使用して Redis Cache インスタンスを選択します。</span><span class="sxs-lookup"><span data-stu-id="29147-171">Locate your Redis cache by selecting **All Resources** in the left-hand sidebar and using the filter box on the left to select Redis Cache instances.</span></span> <span data-ttu-id="29147-172">または、上部にある検索ボックスを使用して、キャッシュの名前を入力することもできます。</span><span class="sxs-lookup"><span data-stu-id="29147-172">Alternatively, you can use the search box at the top and type the name of the cache.</span></span>

1. <span data-ttu-id="29147-173">Redis Cache インスタンスを選択します。</span><span class="sxs-lookup"><span data-stu-id="29147-173">Select your Redis cache instance.</span></span>

1. <span data-ttu-id="29147-174">ご自分の Redis Cache の **[概要]** ブレードで、**[コンソール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="29147-174">In the **Overview** blade for your Redis Cache, select **Console**.</span></span> <span data-ttu-id="29147-175">これにより、詳細な Redis コマンドを入力できる Redis コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="29147-175">This will open a Redis console, which allows you to enter low-level Redis commands.</span></span>

1. <span data-ttu-id="29147-176">「**get MyKey1**」を入力します。</span><span class="sxs-lookup"><span data-stu-id="29147-176">Type **get MyKey1**.</span></span> <span data-ttu-id="29147-177">返される値が **MyValue1** であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29147-177">Verify that the value returned is **MyValue1**.</span></span>

1. <span data-ttu-id="29147-178">「**get MyKey2**」を入力します。</span><span class="sxs-lookup"><span data-stu-id="29147-178">Type **get MyKey2**.</span></span> <span data-ttu-id="29147-179">返される値が **MyValue2** であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29147-179">Verify that the value returned is **MyValue2**.</span></span>

    ![MyKey1 と MyKey2 の値を示す Azure Redis コンソールのスクリーンショット。](../media/4-redis-console.png)