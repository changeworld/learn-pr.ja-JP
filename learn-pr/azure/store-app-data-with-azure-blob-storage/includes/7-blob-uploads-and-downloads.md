<span data-ttu-id="399f6-101">BLOB への参照を設定したら、データをアップロードおよびダウンロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="399f6-101">Once we have a reference to a blob, we can upload and download data.</span></span> <span data-ttu-id="399f6-102">`ICloudBlob` オブジェクトには、ソースおよびターゲットとしてバイト配列、ストリーム、およびファイルをサポートする `Upload` メソッドと `Download` メソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="399f6-102">`ICloudBlob` objects have `Upload` and `Download` methods that support byte arrays, streams, and files as sources and targets.</span></span> <span data-ttu-id="399f6-103">特定の種類には、便宜上、追加のメソッドが含まれています。たとえば、`CloudBlockBlob` では、`UploadTextAsync` および `DownloadTextAsync` を使用した文字列のアップロードとダウンロードがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="399f6-103">Specific types have additional methods for convenience &mdash; for example, `CloudBlockBlob` supports uploading and downloading strings with `UploadTextAsync` and `DownloadTextAsync`.</span></span>

## <a name="creating-new-blobs"></a><span data-ttu-id="399f6-104">新しい BLOB の作成</span><span class="sxs-lookup"><span data-stu-id="399f6-104">Creating new blobs</span></span>

<span data-ttu-id="399f6-105">新しい BLOB を作成するには、存在していない BLOB に対して `Upload` メソッドのいずれかを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="399f6-105">To create a new blob, you call one of the `Upload` methods on a blob that doesn't exist.</span></span> <span data-ttu-id="399f6-106">これにより、BLOB の作成とデータのアップロードという 2 つのことが行われます。</span><span class="sxs-lookup"><span data-stu-id="399f6-106">This does two things: creates the blob and uploads the data.</span></span> 

## <a name="moving-data-to-and-from-blobs"></a><span data-ttu-id="399f6-107">BLOB との間でのデータ移動</span><span class="sxs-lookup"><span data-stu-id="399f6-107">Moving data to and from blobs</span></span>

<span data-ttu-id="399f6-108">BLOB との間のデータ移動は、時間がかかるネットワーク操作です。</span><span class="sxs-lookup"><span data-stu-id="399f6-108">Moving data to and from a blob is a network operation that takes time.</span></span> <span data-ttu-id="399f6-109">Azure Storage SDK for .NET Core では、ネットワーク アクティビティを必要とするすべてのメソッドから `Task` が返されます。したがって、必要に応じてご利用のコント ローラー メソッドが `async` であること、さらにメソッド呼び出しを `await` しているのであって、メソッド呼び出しで `Wait` しているのではないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="399f6-109">In the Azure Storage SDK for .NET Core, all methods that require network activity return `Task`s, so make sure your controller methods are `async` as appropriate and that you are `await`ing method calls and not `Wait`ing on them.</span></span>

<span data-ttu-id="399f6-110">大規模なデータ オブジェクトを操作する場合の一般的な推奨事項は、バイト配列や文字列のようなメモリ内の構造ではなくストリームを使用するということです。</span><span class="sxs-lookup"><span data-stu-id="399f6-110">A common recommendation when working with large data objects is to use streams instead of in-memory structures like byte arrays or strings.</span></span> <span data-ttu-id="399f6-111">これにより、ターゲットへの送信前に、すべての内容がメモリ内にバッファリングされるのを回避できます。</span><span class="sxs-lookup"><span data-stu-id="399f6-111">This avoids buffering the full content in memory before sending it to the target.</span></span> <span data-ttu-id="399f6-112">ASP.NET Core では、要求と応答でのストリームの読み取りおよび書き込みがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="399f6-112">ASP.NET Core supports reading and writing streams from requests and responses.</span></span>

## <a name="concurrent-access"></a><span data-ttu-id="399f6-113">同時アクセス</span><span class="sxs-lookup"><span data-stu-id="399f6-113">Concurrent access</span></span>

<span data-ttu-id="399f6-114">ご利用のアプリによって BLOB が使用されているときにその BLOB が他のプロセスによって追加、変更、または削除される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="399f6-114">It is possible that other processes may be adding, changing, or deleting blobs as your app is using them.</span></span> <span data-ttu-id="399f6-115">常に、防衛的なコードを作成すると共に、ダウンロードを試みるとすぐに削除される BLOB や予期していないときに内容が変更される BLOB など同時実行により発生する問題について考えます。</span><span class="sxs-lookup"><span data-stu-id="399f6-115">Always code defensively and think about problems caused by concurrency, such as blobs that are deleted right as you try to download from them, or blobs whose contents change when you don't expect them to.</span></span> <span data-ttu-id="399f6-116">AccessConditions および BLOB リースを使用して、BLOB への同時アクセスを管理する方法については、このモジュールの最後に記載されている「その他のリソース」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="399f6-116">See the Additional Resources section at the end of this module for information about using AccessConditions and blob leases to manage concurrent blob access.</span></span>

## <a name="exercise"></a><span data-ttu-id="399f6-117">演習</span><span class="sxs-lookup"><span data-stu-id="399f6-117">Exercise</span></span>

<span data-ttu-id="399f6-118">アップロードおよびダウンロードのコードを追加することによってアプリを完了してから、テストのためにそれを Azure App Service にデプロイしましょう。</span><span class="sxs-lookup"><span data-stu-id="399f6-118">Let's finish our app by adding upload and download code, then deploy it to Azure App Service for testing.</span></span>

### <a name="upload"></a><span data-ttu-id="399f6-119">アップロード</span><span class="sxs-lookup"><span data-stu-id="399f6-119">Upload</span></span>

<span data-ttu-id="399f6-120">BLOB をアップロードするには、コンテナーから `CloudBlockBlob` を取得する `GetBlockBlobReference` を使用して `BlobStorage.Save` メソッドを実装します。</span><span class="sxs-lookup"><span data-stu-id="399f6-120">To upload a blob, we'll implement the `BlobStorage.Save` method using `GetBlockBlobReference` to get a `CloudBlockBlob` from the container.</span></span> <span data-ttu-id="399f6-121">`FilesController.Upload` からは `Save` にファイル ストリームが渡されるので、効率が最大になるように `UploadFromStreamAsync` を使用してアップロードを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="399f6-121">`FilesController.Upload` passes the file stream to `Save`, so we can use `UploadFromStreamAsync` to perform the upload for maximum efficiency.</span></span>

<span data-ttu-id="399f6-122">エディターで `BlobStorage.cs` を開き、`Save` 実装に次のコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="399f6-122">Open `BlobStorage.cs` in the editor and fill in the `Save` implementation with the following code:</span></span>

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
> <span data-ttu-id="399f6-123">ここに示したストリーム ベースのアップロード コードは、バイト配列にファイルを読み込んでから Azure Blob Storage に送信するよりも効率的です。</span><span class="sxs-lookup"><span data-stu-id="399f6-123">The stream-based upload code shown here is more efficient than reading the file into a byte array before sending it to Azure Blob storage.</span></span> <span data-ttu-id="399f6-124">ただし、クライアントからファイルを取得するのに使用する `IFormFile` 手法は、真のエンドツーエンド ストリーミング実装ではなく、小さいファイルのアップロードを処理する場合にのみ適しています。</span><span class="sxs-lookup"><span data-stu-id="399f6-124">However, the `IFormFile` technique we use to get the file from the client is not a true end-to-end streaming implementation and is only appropriate for handling uploads of small files.</span></span> <span data-ttu-id="399f6-125">完全にストリーミングされたファイル アップロードについては、このモジュールの最後に記載されている「その他のリソース」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="399f6-125">See the Additional Resources section at the end of this module for information about fully streamed file uploads.</span></span>

### <a name="download"></a><span data-ttu-id="399f6-126">[ダウンロード]</span><span class="sxs-lookup"><span data-stu-id="399f6-126">Download</span></span>

<span data-ttu-id="399f6-127">`BlobStorage.Load` からは `Stream` が返されます。すなわち、作成するコードでは BLOB ストレージからバイトを物理的に移動する必要は全くなく、BLOB ストリームへの参照を返す必要があるだけです。</span><span class="sxs-lookup"><span data-stu-id="399f6-127">`BlobStorage.Load` returns a `Stream`, meaning that our code doesn't need to physically move the bytes from Blob storage at all &mdash; we just need to return a reference to the blob stream.</span></span> <span data-ttu-id="399f6-128">それは、`OpenReadAsync` を使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="399f6-128">We can do that with `OpenReadAsync`.</span></span> <span data-ttu-id="399f6-129">ASP.NET Core では、クライアント応答がビルドされると、ストリームの読み取りおよび終了の処理が行われます。</span><span class="sxs-lookup"><span data-stu-id="399f6-129">ASP.NET Core will handle reading and closing the stream when it builds the client response.</span></span>

<span data-ttu-id="399f6-130">`Load` に次のコードを入力します。</span><span class="sxs-lookup"><span data-stu-id="399f6-130">Fill in `Load` with this code:</span></span>

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a><span data-ttu-id="399f6-131">Azure 内でデプロイして実行する</span><span class="sxs-lookup"><span data-stu-id="399f6-131">Deploy and run in Azure</span></span>

<span data-ttu-id="399f6-132">アプリは完成しましたので、デプロイして動作を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="399f6-132">Our app is finished &mdash; let's deploy it and see it work.</span></span> <span data-ttu-id="399f6-133">Azure Cloud Shell ターミナルで次のコードを実行して、コードをビルドし、それを新しい App Service インスタンスにデプロイします。</span><span class="sxs-lookup"><span data-stu-id="399f6-133">Run the following code in the Azure Cloud Shell terminal to build our code and deploy it to a new App Service instance.</span></span> <span data-ttu-id="399f6-134">また、ストレージ アカウント接続文字列を構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="399f6-134">We also add our storage account connection string to configuration.</span></span>

```console

```

<span data-ttu-id="399f6-135">...</span><span class="sxs-lookup"><span data-stu-id="399f6-135">...</span></span>

<span data-ttu-id="399f6-136">いくつかのファイルをアップロードおよびダウンロードすることでアプリをテストし、次に、シェル内で次を実行してアップロードされた BLOB を確認します。</span><span class="sxs-lookup"><span data-stu-id="399f6-136">Upload and download some files to test the app, then run the following in the shell to see the blobs that have been uploaded:</span></span>

```console

```

<span data-ttu-id="399f6-137">**ポータルからの TODO pic**</span><span class="sxs-lookup"><span data-stu-id="399f6-137">**TODO pic from portal**</span></span>