<span data-ttu-id="79f58-101">まず、Azure Redis Cache のインスタンスを作成してから、キャッシュに 2 つのデータ値を挿入する単純なトランザクションを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="79f58-101">Let's start by creating an instance of Azure Redis Cache, and then create a simple transaction that inserts two data values into the cache.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="79f58-102">Azure Redis Cache の作成</span><span class="sxs-lookup"><span data-stu-id="79f58-102">Create an Azure Redis Cache</span></span>

<span data-ttu-id="79f58-103">Azure CLI を使用して Azure Redis Cache の作成から始めましょう。</span><span class="sxs-lookup"><span data-stu-id="79f58-103">Let's start by creating an Azure Redis Cache with the Azure CLI.</span></span> <span data-ttu-id="79f58-104">Azure とやりとりするには、ブラウザー ウィンドウの右側にある Cloud Shell を使用します。</span><span class="sxs-lookup"><span data-stu-id="79f58-104">Use the Cloud Shell on the right side of the browser window to interact with Azure.</span></span>

<span data-ttu-id="79f58-105">`az redis create` コマンドを使用して、新しい Azure Redis Cache を作成します。</span><span class="sxs-lookup"><span data-stu-id="79f58-105">We'll use the `az redis create` command to create a new Azure Redis Cache.</span></span> <span data-ttu-id="79f58-106">これは、いくつかのパラメーターを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="79f58-106">It takes several parameters.</span></span> <span data-ttu-id="79f58-107">最も一般的なものを次に示します (完全なリストを取得するには、マニュアルを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="79f58-107">Here are the most common (to get a full list, check the documentation).</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="79f58-108">パラメーター</span><span class="sxs-lookup"><span data-stu-id="79f58-108">Parameter</span></span> | <span data-ttu-id="79f58-109">説明</span><span class="sxs-lookup"><span data-stu-id="79f58-109">Description</span></span> |
> |-----------|-------------|
> | `--name`    | <span data-ttu-id="79f58-110">キャッシュの名前: グローバルに一意の名前にし、文字、数字、ハイフンで構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79f58-110">The name of the cache - this must be globally unique and composed of letters, numbers, and dashes.</span></span> |
> | `--resource-group` | <span data-ttu-id="79f58-111">Azure サンドボックスの一部である、事前に作成したリソース グループ **<rgn>[サンドボックス リソース グループ名]</rgn>** を使用します。</span><span class="sxs-lookup"><span data-stu-id="79f58-111">Use the pre-created Resource Group **<rgn>[sandbox resource group name]</rgn>**, which is part of the Azure sandbox.</span></span> |
> | `--location` | <span data-ttu-id="79f58-112">キャッシュが配置される場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="79f58-112">Specify the location where the cache should be located.</span></span> <span data-ttu-id="79f58-113">通常は、データ コンシューマーに近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="79f58-113">Normally, you will want to choose a location close to the data consumers.</span></span> <span data-ttu-id="79f58-114">この場合は、Azure サンドボックス内で使用できる場所に制限されています。</span><span class="sxs-lookup"><span data-stu-id="79f58-114">In this case, you are limited to the locations available in the Azure sandbox.</span></span> <span data-ttu-id="79f58-115">最も近いものを選択します。</span><span class="sxs-lookup"><span data-stu-id="79f58-115">Select the closest one to you.</span></span> |
> | `--size` | <span data-ttu-id="79f58-116">Azure Redis Cache のサイズ。</span><span class="sxs-lookup"><span data-stu-id="79f58-116">The size of the Azure Redis Cache.</span></span> <span data-ttu-id="79f58-117">有効な値: [C0、C1、C2、C3、C4、C5、C6、P1、P2、P3、P4]。</span><span class="sxs-lookup"><span data-stu-id="79f58-117">Valid values are [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4].</span></span> |
> | `--sku` | <span data-ttu-id="79f58-118">Azure Redis Cache SKU。</span><span class="sxs-lookup"><span data-stu-id="79f58-118">The Azure Redis Cache SKU.</span></span> <span data-ttu-id="79f58-119">有効な値: [Basic、Standard、Premium]。</span><span class="sxs-lookup"><span data-stu-id="79f58-119">Valid values are [Basic, Standard, Premium].</span></span> |

### <a name="select-a-location"></a><span data-ttu-id="79f58-120">場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="79f58-120">Select a location</span></span>
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="79f58-121">次のオプションを使用してキャッシュを作成します。</span><span class="sxs-lookup"><span data-stu-id="79f58-121">Create a cache using the following options:</span></span>
    - <span data-ttu-id="79f58-122">サイズ: C0</span><span class="sxs-lookup"><span data-stu-id="79f58-122">Size: C0</span></span>
    - <span data-ttu-id="79f58-123">SKU: Basic</span><span class="sxs-lookup"><span data-stu-id="79f58-123">SKU: Basic</span></span>

1. <span data-ttu-id="79f58-124">コマンド ラインの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="79f58-124">Here's an example command line.</span></span> <span data-ttu-id="79f58-125">`[name]` パラメーターを一意の名前に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="79f58-125">Make sure to replace the `[name]` parameter with a unique name.</span></span> <span data-ttu-id="79f58-126">米国東部以外のリージョンを使用する場合は場所を置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="79f58-126">You can replace the location if you want a different region than East US.</span></span>

    ```azurecli
    REDIS_NAME=[name]

    az redis create \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location eastus \
        --vm-size C0 \
        --sku Basic \
    ```

## <a name="create-a-net-core-console-application"></a><span data-ttu-id="79f58-127">.NET Core コンソール アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="79f58-127">Create a .NET Core console application</span></span>

<span data-ttu-id="79f58-128">次に、Azure Redis Cache にデータ値を挿入するために使用される .NET Core コンソール アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="79f58-128">Next, create a .NET Core console application, which will be used to insert data values into our Azure Redis Cache.</span></span>

1. <span data-ttu-id="79f58-129">ウィンドウの右側に統合された Cloud Shell を使用して、新しい .NET Core アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="79f58-129">Create a new .NET Core application using the integrated Cloud Shell on the right hand side of the window.</span></span> <span data-ttu-id="79f58-130">これに "RedisData" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="79f58-130">Name it "RedisData".</span></span>

    ```bash
    cd ~
    dotnet new console --name RedisData
    ```

1. <span data-ttu-id="79f58-131">アプリ用に作成した新しいディレクトリに変更します。</span><span class="sxs-lookup"><span data-stu-id="79f58-131">Change into the new directory created for your app.</span></span>

    ```bash
    cd RedisData
    ```

1. <span data-ttu-id="79f58-132">アプリケーションをビルドして実行します。"Hello World!" と出力されます。</span><span class="sxs-lookup"><span data-stu-id="79f58-132">Build and run the application - it should output "Hello World!"</span></span>

    ```bash
    dotnet run
    ```

## <a name="add-the-servicestackredis-nuget-package"></a><span data-ttu-id="79f58-133">ServiceStack.Redis NuGet パッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="79f58-133">Add the ServiceStack.Redis NuGet package</span></span>

<span data-ttu-id="79f58-134">コンソール アプリケーションを作成したので、**ServiceStack.Redis** NuGet パッケージを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="79f58-134">Now that we have our console application, we need to add the **ServiceStack.Redis** NuGet package.</span></span> <span data-ttu-id="79f58-135">これにより、Azure Redis Cache に接続して、C# でコマンドを発行できるようになります。</span><span class="sxs-lookup"><span data-stu-id="79f58-135">This will allow us to connect to the Azure Redis Cache and issue commands in C#.</span></span>

1. <span data-ttu-id="79f58-136">ターミナル シェルを使用して、NuGet パッケージ **ServiceStack.Redis** を追加します。</span><span class="sxs-lookup"><span data-stu-id="79f58-136">Add the NuGet package **ServiceStack.Redis** using the terminal shell.</span></span>

    ```bash
    dotnet add package ServiceStack.Redis
    ```

1. <span data-ttu-id="79f58-137">すべてコンパイルされていることを確認するため、もう一度アプリケーションをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="79f58-137">Build and run the application again to make sure it all compiles.</span></span> <span data-ttu-id="79f58-138">ここでも "Hello World!" が出力されるはずです。</span><span class="sxs-lookup"><span data-stu-id="79f58-138">It should still output "Hello World!"</span></span>

## <a name="get-your-azure-redis-cache-connection-string"></a><span data-ttu-id="79f58-139">Azure Redis Cache の接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="79f58-139">Get your Azure Redis Cache connection string</span></span>

<span data-ttu-id="79f58-140">Azure Redis Cache に接続するには、パスワードと URL を含む接続文字列が必要です。</span><span class="sxs-lookup"><span data-stu-id="79f58-140">To connect to your Azure Redis Cache, you need a connection string that contains your password and URL.</span></span> <span data-ttu-id="79f58-141">**ServiceStack.Redis** には独自の接続文字列の形式 `[password]@[hostname]:[sslport]?ssl=true` があります。</span><span class="sxs-lookup"><span data-stu-id="79f58-141">**ServiceStack.Redis** has its own connection string format: `[password]@[hostname]:[sslport]?ssl=true`.</span></span>

<span data-ttu-id="79f58-142">このパスワードは、Azure portal またはコマンドラインを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="79f58-142">You can retrieve your password with the Azure portal, or with the command line.</span></span> <span data-ttu-id="79f58-143">ポータルによるアプローチは「Redis で読み取り専用データをキャッシュすることにより Web アプリケーションを最適化する」モジュールで使用したため、ここでは後者を使用しましょう。</span><span class="sxs-lookup"><span data-stu-id="79f58-143">Let's use the latter here, since we used the portal approach in the "Optimize your web applications by caching read-only data with Redis" module.</span></span>

<span data-ttu-id="79f58-144">`az redis list-keys` コマンドを使用して、アクセス キーを取得します。</span><span class="sxs-lookup"><span data-stu-id="79f58-144">Use the `az redis list-keys` command to get the access keys.</span></span> <span data-ttu-id="79f58-145">これらのコマンドを実行してプライマリ キーを取得し、それを `REDIS_KEY` という変数に保管して表示します。</span><span class="sxs-lookup"><span data-stu-id="79f58-145">Run these commands to get the primary key, store it in a variable called `REDIS_KEY`, and display it:</span></span>

```azurecli
REDIS_KEY=$(az redis list-keys \
    --name "$REDIS_NAME" \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query primaryKey \
    --output tsv)

echo $REDIS_KEY
```

<span data-ttu-id="79f58-146">次に、このコマンドを実行して、接続文字列をまとめて、コマンド ライン上に表示します。</span><span class="sxs-lookup"><span data-stu-id="79f58-146">Next, run this command to put the connection string together and display it on the command line.</span></span> <span data-ttu-id="79f58-147">ホスト名は `.redis.cache.windows.net` の前に来るキャッシュの名前で、ポートはデフォルトの Redis SSL ポートである **6380** です。</span><span class="sxs-lookup"><span data-stu-id="79f58-147">Note that the hostname is the name of the cache followed by `.redis.cache.windows.net`, and the port is **6380**, the default Redis SSL port.</span></span>

```bash
echo "$REDIS_KEY"@"$REDIS_NAME".redis.cache.windows.net:6380?ssl=true
```

<span data-ttu-id="79f58-148">次のステップで使用するクリップボード &mdash; に接続文字列をコピーします。</span><span class="sxs-lookup"><span data-stu-id="79f58-148">Copy the connection string to the clipboard &mdash; you'll be using it in the next step.</span></span>

## <a name="add-the-connection-string-to-your-app"></a><span data-ttu-id="79f58-149">アプリに接続文字列を追加する</span><span class="sxs-lookup"><span data-stu-id="79f58-149">Add the connection string to your app</span></span>

1. <span data-ttu-id="79f58-150">アプリケーション フォルダーから Cloud Shell エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="79f58-150">Open the Cloud Shell editor from the application folder.</span></span>

    ```bash
    cd ~/RedisData
    code .
    ```

1. <span data-ttu-id="79f58-151">**Program.cs** ソース ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="79f58-151">Select the **Program.cs** source file.</span></span>

1. <span data-ttu-id="79f58-152">`Program` クラスに次のフィールドを作成し、接続文字列を値として貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="79f58-152">Create the following field in the `Program` class and paste in your connection string as the value.</span></span>

    ```csharp
    static string redisConnectionString = "<connection string>";
    ```

    <span data-ttu-id="79f58-153">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="79f58-153">Here's an example:</span></span>

    ```csharp
    static string redisConnectionString = "ToOosHLZw9Gwyr46ZlxcNeCCIzS35IBkEtwsCt1Xu4c=@myname.redis.cache.windows.net:6380?ssl=true";
    ```

## <a name="insert-two-data-values-into-your-azure-redis-cache"></a><span data-ttu-id="79f58-154">Azure Redis Cache に 2 つのデータ値を挿入する</span><span class="sxs-lookup"><span data-stu-id="79f58-154">Insert two data values into your Azure Redis Cache</span></span>

<span data-ttu-id="79f58-155">最後に、Azure Redis Cache にデータを追加します。</span><span class="sxs-lookup"><span data-stu-id="79f58-155">Finally, we're going to add data into your Azure Redis Cache.</span></span>

1. <span data-ttu-id="79f58-156">**Program.cs** ファイルの先頭に次の `using` ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="79f58-156">Add the following `using` statement to the top of the **Program.cs** file.</span></span>

    ```csharp
    using ServiceStack.Redis;
    ```

1. <span data-ttu-id="79f58-157">`Main` メソッドの内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="79f58-157">Replace the contents of the `Main` method with the following code.</span></span> <span data-ttu-id="79f58-158">これにより、トランザクションを使用して 2 つの値が追加されます。</span><span class="sxs-lookup"><span data-stu-id="79f58-158">This will use a transaction to add two values.</span></span>

    ```csharp
    bool transactionResult = false;

    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    using (var transaction = redisClient.CreateTransaction())
    {
        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
        transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

        //Commit and get result of transaction
        transactionResult = transaction.Commit();
    }

    if (transactionResult)
    {
        Console.WriteLine("Transaction committed");
    }
    else
    {
        Console.WriteLine("Transaction failed to commit");
    }
    ```

1. <span data-ttu-id="79f58-159">アプリケーションをビルドして実行する前に、Redis キャッシュが完全にプロビジョニングされていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="79f58-159">Before building and running the application, check to make sure that the Redis cache has been fully provisioned.</span></span> <span data-ttu-id="79f58-160">`az redis create` が完了するまでに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="79f58-160">It can sometimes take a few minutes after `az redis create` completes.</span></span> <span data-ttu-id="79f58-161">次のコマンドを実行して、状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="79f58-161">Run the following command to check the status.</span></span>

    ```azcli
    az redis show \
        --name "$REDIS_NAME" \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --query provisioningState
    ```

    <span data-ttu-id="79f58-162">状態が `Creating` の場合は、数分後にもう一度確認してください。</span><span class="sxs-lookup"><span data-stu-id="79f58-162">If the status is `Creating`, try checking again in a couple of minutes.</span></span> <span data-ttu-id="79f58-163">完了すると、`Succeeded` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="79f58-163">It will show `Succeeded` when finished.</span></span>

1. <span data-ttu-id="79f58-164">アプリケーションを実行し、**[トランザクションがコミットされました]** とコンソールに表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79f58-164">Run the application and verify that the console says **Transaction committed**.</span></span>

    ```bash
    dotnet run
    ```

## <a name="verify-your-data"></a><span data-ttu-id="79f58-165">データの検証</span><span class="sxs-lookup"><span data-stu-id="79f58-165">Verify your data</span></span>

<span data-ttu-id="79f58-166">完了するため、追加したデータが Azure Redis Cache にあることを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="79f58-166">To finish off, let's verify that the data we added is in our Azure Redis Cache.</span></span>

1. <span data-ttu-id="79f58-167">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="79f58-167">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="79f58-168">左側のサイドバーで **[すべてのリソース]** を選択して Azure Redis Cache を見つけ、左のフィルター ボックスを使用して Azure Redis Cache インスタンスを選択します。</span><span class="sxs-lookup"><span data-stu-id="79f58-168">Locate your Azure Redis Cache by selecting **All Resources** in the left-hand sidebar, and using the filter box on the left to select Azure Redis Cache instances.</span></span> <span data-ttu-id="79f58-169">または、上部にある検索ボックスを使用して、キャッシュの名前を入力することもできます。</span><span class="sxs-lookup"><span data-stu-id="79f58-169">Alternatively, you can use the search box at the top, and type the name of the cache.</span></span>

1. <span data-ttu-id="79f58-170">Azure Redis Cache インスタンスを選択します。</span><span class="sxs-lookup"><span data-stu-id="79f58-170">Select your Azure Redis Cache instance.</span></span>

1. <span data-ttu-id="79f58-171">Azure Redis Cache の **[概要]** ブレードで、**[コンソール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="79f58-171">In the **Overview** blade for your Azure Redis Cache, select **Console**.</span></span> <span data-ttu-id="79f58-172">これにより Azure Redis Cache コンソールが開き、低レベルの Azure Redis Cache コマンドを入力できます。</span><span class="sxs-lookup"><span data-stu-id="79f58-172">This will open an Azure Redis Cache console, which allows you to enter low-level Azure Redis Cache commands.</span></span>

1. <span data-ttu-id="79f58-173">「**get MyKey1**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="79f58-173">Type **get MyKey1**.</span></span> <span data-ttu-id="79f58-174">返される値が **MyValue1** であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79f58-174">Verify that the value returned is **MyValue1**.</span></span>

1. <span data-ttu-id="79f58-175">「**get MyKey2**」を入力します。</span><span class="sxs-lookup"><span data-stu-id="79f58-175">Type **get MyKey2**.</span></span> <span data-ttu-id="79f58-176">返される値が **MyValue2** であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="79f58-176">Verify that the value returned is **MyValue2**.</span></span>

    ![MyKey1 と MyKey2 の値を示す Azure Redis Cache コンソールのスクリーンショット。](../media/4-redis-console.png)