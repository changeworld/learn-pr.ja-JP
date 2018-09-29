<span data-ttu-id="ed028-101">::: zone pivot="csharp" コードを追加して構成から接続文字列を取得し、その接続文字列を使用して Azure ストレージ アカウントに接続してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ed028-101">::: zone pivot="csharp" Let's add code to retrieve the connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="ed028-102">接続文字列を取得する</span><span class="sxs-lookup"><span data-stu-id="ed028-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="ed028-103">コード エディターで **Program.cs** を開きます。</span><span class="sxs-lookup"><span data-stu-id="ed028-103">Open **Program.cs** in the code editor.</span></span>

1. <span data-ttu-id="ed028-104">ファイルの上部に `using` ステートメントを追加して、`Microsoft.WindowsAzure.Storage` 名前空間を参照します。</span><span class="sxs-lookup"><span data-stu-id="ed028-104">Add a `using` statement at the top of the file to reference the `Microsoft.WindowsAzure.Storage` namespace:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. <span data-ttu-id="ed028-105">`Main` メソッドの末尾に、次の行を追加して、構成ファイルから Azure ストレージ アカウントの接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="ed028-105">At the end of the `Main` method, add the following line to retrieve the Azure storage account connection string from the configuration file.</span></span> <span data-ttu-id="ed028-106">渡された_キー_は、**appsettings.json** ファイルで使用される名前と一致する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed028-106">The passed _key_ must match the name used in your **appsettings.json** file.</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="ed028-107">BLOB クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="ed028-107">Create a blob client</span></span>

1. <span data-ttu-id="ed028-108">静的な `CloudStorageAccount.TryParse` メソッドを使用して `CloudStorageAccount` オブジェクトを作成します。作成されたオブジェクトを返すには、接続文字列と `out` パラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="ed028-108">Use the static `CloudStorageAccount.TryParse` method to create a `CloudStorageAccount` object - it takes the connection string and an `out` parameter to return the created object.</span></span> <span data-ttu-id="ed028-109">文字列が正常に分析されたかどうかを示す `bool` 値が返されます。</span><span class="sxs-lookup"><span data-stu-id="ed028-109">It returns a `bool` value indicating whether it successfully parsed the string.</span></span>
    - <span data-ttu-id="ed028-110">正常に分析されなかった場合は、メッセージをコンソールに出力して、メソッドから返します。</span><span class="sxs-lookup"><span data-stu-id="ed028-110">If it fails, output a message to the console and return from the method.</span></span>

    ```csharp
    if (!CloudStorageAccount.TryParse(connectionString,
            out CloudStorageAccount storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    ```

1. <span data-ttu-id="ed028-111">返された `CloudStorageAccount` オブジェクトを使用して、BLOB クライアントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed028-111">Use the returned `CloudStorageAccount` object to create a blob client.</span></span>

    ```csharp
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="ed028-112">次に、BLOB クライアントを使用して、"photoblobs" という名前のコンテナーへの参照を取得します。</span><span class="sxs-lookup"><span data-stu-id="ed028-112">Next, use the blob client to retrieve a reference to a container named "photoblobs".</span></span> <span data-ttu-id="ed028-113">アカウント名と同じように、BLOB コンテナー名は小文字にして、文字と数字で構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed028-113">Much like the account names, the Blob container names must be lowercase and composed of letters and numbers.</span></span>

    ```csharp
    var blobContainer = blobClient.GetContainerReference("photoblobs");
    ```

1. <span data-ttu-id="ed028-114">`CreateIfNotExistsAsync` メソッドを使用してコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ed028-114">Use the `CreateIfNotExistsAsync` method to create the container.</span></span> <span data-ttu-id="ed028-115">これにより、コンテナーが作成されたかどうかを示す `bool` が返されます。</span><span class="sxs-lookup"><span data-stu-id="ed028-115">This returns a `bool` indicating whether the container was created.</span></span> <span data-ttu-id="ed028-116">これを `created` という名前の変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="ed028-116">Store this in a variable named `created`.</span></span>
    - <span data-ttu-id="ed028-117">これは **async** メソッドで、つまり、これにより実際のネットワーク呼び出しが行われることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="ed028-117">Notice that this is an **async** method - that means it will perform an actual network call.</span></span>
    - <span data-ttu-id="ed028-118">`await` キーワードを使用して、`bool` の結果を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ed028-118">You will need to use the `await` keywords to get the `bool` result.</span></span>

    ```csharp
    bool created = await blobContainer.CreateIfNotExistsAsync();
    ```

1. <span data-ttu-id="ed028-119">`await` キーワードを使用するので、先に進んで `Main` メソッドの署名を `async` になるように変更して `Task` を返します。</span><span class="sxs-lookup"><span data-stu-id="ed028-119">Because we are using the `await` keyword, go ahead and change the signature for the `Main` method to be `async` and return a `Task`.</span></span>

    ```csharp
    static async Task Main(string[] args)
    {
        ...
    }
    ```

    <span data-ttu-id="ed028-120">先頭に新しい `using` ステートメントを追加する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="ed028-120">You'll also need to add a new `using` statement at the top:</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="ed028-121">最後に、BLOB コンテナーを作成したかどうかを出力します。</span><span class="sxs-lookup"><span data-stu-id="ed028-121">Finally, output whether we created the Blob container.</span></span>

    ```csharp
    Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
    ```

1. <span data-ttu-id="ed028-122">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="ed028-122">Save the file.</span></span>

<span data-ttu-id="ed028-123">作業内容を確認する場合の最終的なファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ed028-123">The final file should look like this if you'd like to check your work.</span></span>

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using Microsoft.WindowsAzure.Storage;
using System.Threading.Tasks;

namespace PhotoSharingApp
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();
            var connectionString = configuration["StorageAccountConnectionString"];

            if (!CloudStorageAccount.TryParse(connectionString, out CloudStorageAccount storageAccount))
            {
                Console.WriteLine("Unable to parse connection string");
                return;
            }

            var blobClient = storageAccount.CreateCloudBlobClient();
            var blobContainer = blobClient.GetContainerReference("photoblobs");
            bool created = await blobContainer.CreateIfNotExistsAsync();

            Console.WriteLine(created ? "Created the Blob container" : "Blob container already exists.");
        }
    }
}
```

## <a name="use-c-71-to-build-our-app"></a><span data-ttu-id="ed028-124">C# 7.1 を使用してアプリをビルドする</span><span class="sxs-lookup"><span data-stu-id="ed028-124">Use C# 7.1 to build our app</span></span>

<span data-ttu-id="ed028-125">C# 7.1 に、`Main` メソッドでの `async` と `await` のサポートが追加されました。</span><span class="sxs-lookup"><span data-stu-id="ed028-125">Support for `async` and `await` on `Main` methods was added to C# 7.1.</span></span> <span data-ttu-id="ed028-126">これが、使用しているコンパイラの既定のバージョンではない可能性があります。</span><span class="sxs-lookup"><span data-stu-id="ed028-126">This might not be the default version of the compiler you are using.</span></span> <span data-ttu-id="ed028-127">構成ファイルで設定して、そのバージョンのコンパイラを使用していることを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="ed028-127">Let's make sure we use that version of the compiler by setting it in our configuration file.</span></span>

1. <span data-ttu-id="ed028-128">`PhotoSharingApp.csproj` を開き、`TargetFramework` を指定する `PropertyGroup` に `<LangVersion>7.1</LangVersion>` を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed028-128">Open the `PhotoSharingApp.csproj` and add `<LangVersion>7.1</LangVersion>` to the `PropertyGroup` that specifies the `TargetFramework`.</span></span> <span data-ttu-id="ed028-129">完了すると、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ed028-129">It should look like this when you're finished:</span></span>

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <LangVersion>7.1</LangVersion>
        <TargetFramework>netcoreapp2.0</TargetFramework>
      </PropertyGroup>
      ...
    </Project>
    ```

1. <span data-ttu-id="ed028-130">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="ed028-130">Save the file.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="ed028-131">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="ed028-131">Run the app</span></span>

1. <span data-ttu-id="ed028-132">アプリケーションをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="ed028-132">Build and run the application.</span></span> <span data-ttu-id="ed028-133">**注:** 正しい作業ディレクトリで操作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="ed028-133">**Note:** make sure you're in the correct working directory.</span></span>

    ```bash
    dotnet run
    ```

<span data-ttu-id="ed028-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="ed028-134">::: zone-end</span></span>

<span data-ttu-id="ed028-135">::: zone-pivot="javascript" 格納された接続文字列を使用して Azure ストレージ アカウントに接続するコードを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ed028-135">::: zone pivot="javascript" Let's add code to connect to the Azure storage account using our stored connection string.</span></span> <span data-ttu-id="ed028-136">Azure クライアント ライブラリでは、自動的に **AZURE_STORAGE_CONNECTION_STRING** 環境変数を使用して接続文字列が取得されます。</span><span class="sxs-lookup"><span data-stu-id="ed028-136">The Azure client library will automatically use the **AZURE_STORAGE_CONNECTION_STRING** environment variable to get the connection string.</span></span>

## <a name="create-a-blob-client"></a><span data-ttu-id="ed028-137">BLOB クライアントを作成する</span><span class="sxs-lookup"><span data-stu-id="ed028-137">Create a blob client</span></span>

1. <span data-ttu-id="ed028-138">コード エディターで **index.js** を開きます。</span><span class="sxs-lookup"><span data-stu-id="ed028-138">Open **index.js** in the code editor.</span></span>

1. <span data-ttu-id="ed028-139">まず、**azure-storage** モジュールを含めます。</span><span class="sxs-lookup"><span data-stu-id="ed028-139">Start by including the **azure-storage** module.</span></span> <span data-ttu-id="ed028-140">モジュールを **storage** という名前の変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="ed028-140">Store the module in a variable named **storage**.</span></span>

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    const storage = require('azure-storage');
    ```

1. <span data-ttu-id="ed028-141">次に、その直後に、**storage** オブジェクトを使用して `BlobService` オブジェクトを作成し、グローバルな名前の **blobService** に格納します。</span><span class="sxs-lookup"><span data-stu-id="ed028-141">Next, right after that, use the **storage** object to create the `BlobService` object and store it in a global named **blobService**.</span></span> <span data-ttu-id="ed028-142">これらは、ストレージ アカウントへのアクセスを表す軽量なオブジェクトであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="ed028-142">Remember, these are lightweight objects representing access to the storage account.</span></span>

    ```javascript
    const blobService = storage.createBlobService();
    ```

1. <span data-ttu-id="ed028-143">作成するコンテナーを表す定数を追加します。</span><span class="sxs-lookup"><span data-stu-id="ed028-143">Add a constant to represent the container we want to create.</span></span> <span data-ttu-id="ed028-144">コンテナーに "photoblobs" という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="ed028-144">We'll name the container "photoblobs".</span></span>

    ```javascript
    const containerName = 'photoblobs';
    ```

## <a name="create-a-container"></a><span data-ttu-id="ed028-145">コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="ed028-145">Create a container</span></span>

<span data-ttu-id="ed028-146">`BlobService` オブジェクトを使用して、Azure Storage で BLOB API を操作することができます。</span><span class="sxs-lookup"><span data-stu-id="ed028-146">We can use the `BlobService` object to work with blob APIs in Azure storage.</span></span> <span data-ttu-id="ed028-147">前述のとおり、ネットワーク呼び出しを行うすべての API は、アプリの応答性を維持するために非同期になっています。</span><span class="sxs-lookup"><span data-stu-id="ed028-147">As mentioned before, all the APIs that make network calls are asynchronous to keep the app responsive.</span></span> <span data-ttu-id="ed028-148">`createContainerIfNotExists` メソッドは、そのようなメソッドの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="ed028-148">The `createContainerIfNotExists` method is one such method.</span></span> <span data-ttu-id="ed028-149">_promise_ を使用して、応答を含むコールバックを処理します。</span><span class="sxs-lookup"><span data-stu-id="ed028-149">We'll use _promises_ to handle the callback that contains the response.</span></span>

1. <span data-ttu-id="ed028-150">`util.promisify` を使用してコールバック バージョンの `createContainerIfNotExists` を取得し、promise-returning メソッドに変換します。</span><span class="sxs-lookup"><span data-stu-id="ed028-150">Use `util.promisify` to take the callback version of `createContainerIfNotExists` and turn it into a promise-returning method.</span></span>
    - <span data-ttu-id="ed028-151">コールバック メソッドはオブジェクトにあるため、必ず、末尾に `bind` 呼び出しを追加して、そのコンテキストに接続するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="ed028-151">Since the callback method is on an object, make sure to add a `bind` call at the end to connect it to that context.</span></span>
    - <span data-ttu-id="ed028-152">以下のように、`createContainerAsync` という名前のファイルの先頭にある定数に戻り値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="ed028-152">Assign the return value to a constant at the top of the file named `createContainerAsync`, as shown below.</span></span>
    - <span data-ttu-id="ed028-153">ファイルの先頭近くに `util` モジュール用の `require` も必要になります。</span><span class="sxs-lookup"><span data-stu-id="ed028-153">You'll also need a `require` for the `util` module near the top of the file.</span></span>

    ```javascript
    const util = require('util');
    const storage = require('azure-storage');
    const blobService = storage.createBlobService();

    const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

    const containerName = 'photoblobs';
    ```

2. <span data-ttu-id="ed028-154">"Hello, World!" という出力を </span><span class="sxs-lookup"><span data-stu-id="ed028-154">Remove the "Hello, World!"</span></span> <span data-ttu-id="ed028-155">`main()` から削除します。</span><span class="sxs-lookup"><span data-stu-id="ed028-155">output from `main()`.</span></span>

3. <span data-ttu-id="ed028-156">新しい `createContainerAsync` promise を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ed028-156">Call your new `createContainerAsync` promise.</span></span>
    - <span data-ttu-id="ed028-157">それに **containerName** 定数を渡します。</span><span class="sxs-lookup"><span data-stu-id="ed028-157">Pass it the **containerName** constant.</span></span>
    - <span data-ttu-id="ed028-158">呼び出しに `await` キーワードを適用します。</span><span class="sxs-lookup"><span data-stu-id="ed028-158">Apply the `await` keyword to the call.</span></span>
    - <span data-ttu-id="ed028-159">`try` / `catch` コンストラクトに呼び出しをラップして、すべてのエラーを出力します。</span><span class="sxs-lookup"><span data-stu-id="ed028-159">Wrap the call in a `try` / `catch` construct and output any error.</span></span>

    ```javascript
    try {
        await createContainerAsync(containerName);
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. <span data-ttu-id="ed028-160">`createContainerAsync` promise によって、基になる `createContainerIfNotExists` から最初の値が返されます。これはコンテナーの結果です。</span><span class="sxs-lookup"><span data-stu-id="ed028-160">The `createContainerAsync` promise returns the first value from the underlying `createContainerIfNotExists`, which is the container result.</span></span> <span data-ttu-id="ed028-161">これには、コンテナーが作成されたかどうかに関する情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="ed028-161">This includes information on whether the container was created or not.</span></span> <span data-ttu-id="ed028-162">結果を変数にキャプチャし、`result.created` プロパティに基づいてコンテナーが作成されたかどうかを出力します。</span><span class="sxs-lookup"><span data-stu-id="ed028-162">Capture the result in a variable, and output whether the container was created based on the `result.created` property.</span></span>

    ```javascript
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
    ```

1. <span data-ttu-id="ed028-163">最後に、`main` 関数を `async` キーワードで修飾します。</span><span class="sxs-lookup"><span data-stu-id="ed028-163">Finally, decorate the `main` function with the `async` keyword.</span></span>

1. <span data-ttu-id="ed028-164">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="ed028-164">Save the file.</span></span>

<span data-ttu-id="ed028-165">作業内容を確認する場合、最終的なファイルは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="ed028-165">The final file should look like this, if you'd like to check your work.</span></span>

```javascript
#!/usr/bin/env node

require('dotenv').load();

const util = require('util');

const storage = require('azure-storage');
const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);

const containerName = 'photoblobs';

async function main() {
    try {
        var result = await createContainerAsync(containerName);
        if (result.created) {
            console.log(`Blob container ${containerName} created`);
        }
        else {
            console.log(`Blob container ${containerName} already exists.`);
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

main();
```

## <a name="run-the-app"></a><span data-ttu-id="ed028-166">アプリを実行する</span><span class="sxs-lookup"><span data-stu-id="ed028-166">Run the app</span></span>

1. <span data-ttu-id="ed028-167">アプリケーションをビルドして実行します。</span><span class="sxs-lookup"><span data-stu-id="ed028-167">Build and run the application.</span></span> <span data-ttu-id="ed028-168">**注:** PhotoSharingApp ディレクトリで操作していることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="ed028-168">**Note:** make sure you're in the PhotoSharingApp directory.</span></span>

    ```bash
    node index.js
    ```

<span data-ttu-id="ed028-169">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="ed028-169">::: zone-end</span></span>

<span data-ttu-id="ed028-170">BLOB コンテナーが作成されたことが報告されます。</span><span class="sxs-lookup"><span data-stu-id="ed028-170">It should report that the blob container was created.</span></span> <span data-ttu-id="ed028-171">2 回目の実行時には、既に存在することが示されます。</span><span class="sxs-lookup"><span data-stu-id="ed028-171">If you run it a second time, it should tell you it already exists.</span></span>

<span data-ttu-id="ed028-172">コンテナーを確認するには:</span><span class="sxs-lookup"><span data-stu-id="ed028-172">To verify the container:</span></span>

1. <span data-ttu-id="ed028-173">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="ed028-173">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="ed028-174">ストレージ アカウントに移動します。</span><span class="sxs-lookup"><span data-stu-id="ed028-174">Navigate to your storage account.</span></span> <span data-ttu-id="ed028-175">**[すべてのリソース]** セクションを使用して、ストレージ アカウントを検索できます。また、ポータル ウィンドウの上部にある_検索ボックス_から名前で検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="ed028-175">You can use the **All Resources** section to find the storage account or search by name from the _search box_ at the top of the portal window.</span></span>

1. <span data-ttu-id="ed028-176">**[Blob service]** セクションで、ストレージ アカウントの **[BLOB]** エントリを選択します。</span><span class="sxs-lookup"><span data-stu-id="ed028-176">Select the **Blobs** entry of the storage account in the **Blob services** section.</span></span>

1. <span data-ttu-id="ed028-177">[BLOB] パネルに **photoblobs** コンテナーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ed028-177">You should see your **photoblobs** container in the Blobs panel.</span></span> <span data-ttu-id="ed028-178">エントリの右側にある [...] メニューを使用してコンテナーを削除し、アプリで再作成を試すことができます。</span><span class="sxs-lookup"><span data-stu-id="ed028-178">You can delete the container through the "..." menu on the right-hand side of the entry to try recreating it with your app.</span></span>

> [!NOTE]
> <span data-ttu-id="ed028-179">ポータルではコンテナーはすぐに表示されなくなりますが、実際に削除されるまでは数分かかります。</span><span class="sxs-lookup"><span data-stu-id="ed028-179">The container will disappear from the portal very quickly, but it takes a few minutes to actually delete.</span></span> <span data-ttu-id="ed028-180">再作成を試す場合、削除されている間に、Azure からエラー応答を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ed028-180">You will get an error response from Azure while it is being deleted if you attempt to recreate it.</span></span>

## <a name="delete-the-app"></a><span data-ttu-id="ed028-181">アプリを削除する</span><span class="sxs-lookup"><span data-stu-id="ed028-181">Delete the app</span></span>
<span data-ttu-id="ed028-182">ご利用の Cloud Shell 環境でアプリケーションのソース コードを保持しないようにした場合は、次のコマンドを使用して、フォルダーとすべての内容を削除することができます。</span><span class="sxs-lookup"><span data-stu-id="ed028-182">If you decide you don't want to keep the application source code in your Cloud Shell environment, you can use the following command to remove the folder and all the contents.</span></span>

```bash
rm -r PhotoSharingApp/
```
