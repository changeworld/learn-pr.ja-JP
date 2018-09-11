<span data-ttu-id="c8234-101">BLOB への参照を設定したら、データをアップロードおよびダウンロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="c8234-101">Once we have a reference to a blob, we can upload and download data.</span></span> <span data-ttu-id="c8234-102">`ICloudBlob` オブジェクトには、ソースおよびターゲットとしてバイト配列、ストリーム、およびファイルをサポートする `Upload` メソッドと `Download` メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c8234-102">`ICloudBlob` objects have `Upload` and `Download` methods that support byte arrays, streams, and files as sources and targets.</span></span> <span data-ttu-id="c8234-103">特定の種類には、便宜上、追加のメソッドが含まれています。たとえば、`CloudBlockBlob` では、`UploadTextAsync` および `DownloadTextAsync` を使用した文字列のアップロードとダウンロードがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c8234-103">Specific types have additional methods for convenience &mdash; for example, `CloudBlockBlob` supports uploading and downloading strings with `UploadTextAsync` and `DownloadTextAsync`.</span></span>

## <a name="creating-new-blobs"></a><span data-ttu-id="c8234-104">新しい BLOB を作成する</span><span class="sxs-lookup"><span data-stu-id="c8234-104">Creating new blobs</span></span>

<span data-ttu-id="c8234-105">新しい BLOB を作成するには、ストレージ内に存在していない BLOB に対して、参照にある `Upload` メソッドのいずれかを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="c8234-105">To create a new blob, you call one of the `Upload` methods on a reference to a blob that doesn't exist in storage.</span></span> <span data-ttu-id="c8234-106">これにより、ストレージ内での BLOB の作成とデータのアップロードという 2 つのことが行われます。</span><span class="sxs-lookup"><span data-stu-id="c8234-106">This does two things: creates the blob in storage and uploads the data.</span></span>

## <a name="moving-data-to-and-from-blobs"></a><span data-ttu-id="c8234-107">BLOB 間のデータ移動</span><span class="sxs-lookup"><span data-stu-id="c8234-107">Moving data to and from blobs</span></span>

<span data-ttu-id="c8234-108">BLOB 間のデータ移動は、時間がかかるネットワーク操作です。</span><span class="sxs-lookup"><span data-stu-id="c8234-108">Moving data to and from a blob is a network operation that takes time.</span></span> <span data-ttu-id="c8234-109">Azure Storage SDK for .NET Core では、ネットワーク アクティビティを必要とするすべてのメソッドから `Task` が返されるため、コントローラー メソッドで `await` を正しく使用していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c8234-109">In the Azure Storage SDK for .NET Core, all methods that require network activity return `Task`s, so make sure you use `await` in your controller methods appropriately.</span></span>

<span data-ttu-id="c8234-110">大規模なデータ オブジェクトを操作する場合の一般的な推奨事項は、バイト配列や文字列のようなメモリ内の構造ではなくストリームを使用することです。</span><span class="sxs-lookup"><span data-stu-id="c8234-110">A common recommendation when working with large data objects is to use streams instead of in-memory structures like byte arrays or strings.</span></span> <span data-ttu-id="c8234-111">これにより、ターゲットへの送信前に、すべての内容がメモリ内にバッファリングされるのを回避できます。</span><span class="sxs-lookup"><span data-stu-id="c8234-111">This avoids buffering the full content in memory before sending it to the target.</span></span> <span data-ttu-id="c8234-112">ASP.NET Core では、要求と応答でのストリームの読み取りおよび書き込みがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c8234-112">ASP.NET Core supports reading and writing streams from requests and responses.</span></span>

## <a name="concurrent-access"></a><span data-ttu-id="c8234-113">同時アクセス</span><span class="sxs-lookup"><span data-stu-id="c8234-113">Concurrent access</span></span>

<span data-ttu-id="c8234-114">ご利用のアプリによって BLOB が使用されているときにその BLOB が他のプロセスによって追加、変更、または削除される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c8234-114">Other processes may be adding, changing, or deleting blobs as your app is using them.</span></span> <span data-ttu-id="c8234-115">常に防衛的なコードを作成すると共に、ダウンロードを試みるとすぐに削除される BLOB や予期していないときに内容が変更される BLOB など同時実行により発生する問題について考えます。</span><span class="sxs-lookup"><span data-stu-id="c8234-115">Always code defensively and think about problems caused by concurrency, such as blobs that are deleted right as you try to download from them, or blobs whose contents change when you don't expect them to.</span></span> <span data-ttu-id="c8234-116">AccessConditions および BLOB リースを使用して、BLOB への同時アクセスを管理する方法については、このモジュールの最後に記載されている「参考資料」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c8234-116">See the Further Reading section at the end of this module for information about using AccessConditions and blob leases to manage concurrent blob access.</span></span>

## <a name="exercise"></a><span data-ttu-id="c8234-117">演習</span><span class="sxs-lookup"><span data-stu-id="c8234-117">Exercise</span></span>

<span data-ttu-id="c8234-118">アップロードおよびダウンロードのコードを追加することによってアプリを完了してから、テストのためにそれを Azure App Service にデプロイしましょう。</span><span class="sxs-lookup"><span data-stu-id="c8234-118">Let's finish our app by adding upload and download code, then deploy it to Azure App Service for testing.</span></span>

### <a name="upload"></a><span data-ttu-id="c8234-119">アップロード</span><span class="sxs-lookup"><span data-stu-id="c8234-119">Upload</span></span>

<span data-ttu-id="c8234-120">BLOB をアップロードするには、コンテナーから `CloudBlockBlob` を取得する `GetBlockBlobReference` を使用して `BlobStorage.Save` メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="c8234-120">To upload a blob, we'll implement the `BlobStorage.Save` method using `GetBlockBlobReference` to get a `CloudBlockBlob` from the container.</span></span> <span data-ttu-id="c8234-121">`FilesController.Upload` からは `Save` にファイル ストリームが渡されるので、効率を最大限に高めるために `UploadFromStreamAsync` を使用してアップロードを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="c8234-121">`FilesController.Upload` passes the file stream to `Save`, so we can use `UploadFromStreamAsync` to perform the upload for maximum efficiency.</span></span>

<span data-ttu-id="c8234-122">エディターで `BlobStorage.cs` を開き、`Save` を次のコードで置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c8234-122">Open `BlobStorage.cs` in the editor and replace `Save` with the following code:</span></span>

```csharp
public Task Save(Stream fileStream, string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(name);
    return blockBlob.UploadFromStreamAsync(fileStream);
}
```

> [!NOTE]
> <span data-ttu-id="c8234-123">ここに示したストリーム ベースのアップロード コードは、バイト配列にファイルを読み込んでから Azure Blob Storage に送信するよりも効率的です。</span><span class="sxs-lookup"><span data-stu-id="c8234-123">The stream-based upload code shown here is more efficient than reading the file into a byte array before sending it to Azure Blob storage.</span></span> <span data-ttu-id="c8234-124">ただし、クライアントからファイルを取得するのに使用する ASP.NET Core `IFormFile` 手法は、真のエンドツーエンド ストリーミング実装ではなく、小さいファイルのアップロードを処理する場合にのみ適しています。</span><span class="sxs-lookup"><span data-stu-id="c8234-124">However, the ASP.NET Core `IFormFile` technique we use to get the file from the client is not a true end-to-end streaming implementation and is only appropriate for handling uploads of small files.</span></span> <span data-ttu-id="c8234-125">完全にストリーミングされたファイル アップロードについては、このモジュールの最後に記載されている「参考資料」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c8234-125">See the Further Reading section at the end of this module for information about fully streamed file uploads.</span></span>

### <a name="download"></a><span data-ttu-id="c8234-126">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="c8234-126">Download</span></span>

<span data-ttu-id="c8234-127">`BlobStorage.Load` からは `Stream` が返されます。すなわち、作成するコードでは BLOB ストレージからバイトを物理的に移動する必要は全くなく、BLOB ストリームへの参照を返す必要があるだけです。</span><span class="sxs-lookup"><span data-stu-id="c8234-127">`BlobStorage.Load` returns a `Stream`, meaning that our code doesn't need to physically move the bytes from Blob storage at all &mdash; we just need to return a reference to the blob stream.</span></span> <span data-ttu-id="c8234-128">それは、`OpenReadAsync` を使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="c8234-128">We can do that with `OpenReadAsync`.</span></span> <span data-ttu-id="c8234-129">ASP.NET Core では、クライアント応答がビルドされると、ストリームの読み取りおよび終了の処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="c8234-129">ASP.NET Core will handle reading and closing the stream when it builds the client response.</span></span>

<span data-ttu-id="c8234-130">`Load` をこのコードで置き換えて、作業内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="c8234-130">Replace `Load` with this code and save your work:</span></span>

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a><span data-ttu-id="c8234-131">Azure 内でデプロイして実行する</span><span class="sxs-lookup"><span data-stu-id="c8234-131">Deploy and run in Azure</span></span>

<span data-ttu-id="c8234-132">アプリが完成したので、デプロイして動作を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="c8234-132">Our app is finished &mdash; let's deploy it and see it work.</span></span> <span data-ttu-id="c8234-133">App Service アプリを作成し、そのアプリをアプリケーション設定を使用して、自分のストレージ アカウント接続文字列とコンテナー名用に構成します。</span><span class="sxs-lookup"><span data-stu-id="c8234-133">Create an App Service app and configure it with application settings for our storage account connection string and container name.</span></span> <span data-ttu-id="c8234-134">`az storage account show-connection-string` を使用してストレージ アカウントの接続文字列を取得し、コンテナーの名前が `files` になるように設定します。</span><span class="sxs-lookup"><span data-stu-id="c8234-134">Get the storage account's connection string with `az storage account show-connection-string` and set the name of the container to be `files`.</span></span>

<span data-ttu-id="c8234-135">アプリ名はグローバルに一意である必要があるため、独自の名前を選んで `<your-unique-app-name>` に入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c8234-135">The app name needs to be globally unique, so you'll need to choose your own name to fill in `<your-unique-app-name>`.</span></span>

```azurecli
az appservice plan create --name blob-exercise-plan --resource-group blob-exercise-group
az webapp create --name <your-unique-app-name> --plan blob-exercise-plan --resource-group blob-exercise-group
CONNECTIONSTRING=$(az storage account show-connection-string --name <your-unique-storage-account-name> --output tsv)
az webapp config appsettings set --name <your-unique-app-name> --resource-group blob-exercise-group --settings AzureStorageConfig:ConnectionString=$CONNECTIONSTRING AzureStorageConfig:FileContainerName=files
```

<span data-ttu-id="c8234-136">ここでアプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="c8234-136">Now we'll deploy our app.</span></span> <span data-ttu-id="c8234-137">以下のコマンドによってサイトを `pub` フォルダーに発行し、`site.zip` として圧縮した後、その zip を App Service にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="c8234-137">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="c8234-138">シェルが次のコマンドの `FileUploader` ディレクトリにあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c8234-138">Make sure your shell is in the `FileUploader` directory for the following commands.</span></span>

```azurecli
dotnet publish -o pub
cd pub
zip -r ../site.zip *
az webapp deployment source config-zip --src ../site.zip --name <your-unique-app-name> --resource-group blob-exercise-group
```

<span data-ttu-id="c8234-139">ブラウザーで `https://<your-unique-app-name>.azurewebsites.net` を開いて、実行中のアプリを表示します。</span><span class="sxs-lookup"><span data-stu-id="c8234-139">Open `https://<your-unique-app-name>.azurewebsites.net` in a browser to see the running app.</span></span> <span data-ttu-id="c8234-140">次の図のようになります。</span><span class="sxs-lookup"><span data-stu-id="c8234-140">It should look like the image below.</span></span>

![FileUploader Web アプリのスクリーンショット](../media-drafts/fileuploader-empty.PNG)

<span data-ttu-id="c8234-142">アプリをテストするため、ファイルをいくつかアップロードしてダウンロードしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="c8234-142">Try uploading and downloading some files to test the app.</span></span> <span data-ttu-id="c8234-143">ファイルをいくつかアップロードしたら、シェル内で次を実行して、コンテナーにアップロードされた BLOB を確認します。</span><span class="sxs-lookup"><span data-stu-id="c8234-143">After you've uploaded a few files, run the following in the shell to see the blobs that have been uploaded to the container:</span></span>

```console
az storage blob list --account-name <your-unique-storage-account-name> --container-name files --query [].{Name:name} --output table
```