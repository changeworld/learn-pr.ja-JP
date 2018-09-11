<span data-ttu-id="ab5fb-101">.NET Core 向けの Azure Storage SDK の個々の BLOB の操作には、`ICloudBlob` オブジェクトのインスタンスである *BLOB 参照*が必要です。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-101">Working with an individual blob in the Azure Storage SDK for .NET Core requires a *blob reference* &mdash; an instance of an `ICloudBlob` object.</span></span>

<span data-ttu-id="ab5fb-102">`ICloudBlob` は、BLOB 名を使用して要求するか、コンテナーの BLOB 一覧から選択して取得することができます。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-102">You can get an `ICloudBlob` by requesting it with the blob's name or selecting it from a list of blobs in the container.</span></span> <span data-ttu-id="ab5fb-103">いずれにも、最後のユニットで入手方法を確認した、`CloudBlobContainer` が必要です。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-103">Both require a `CloudBlobContainer`, which we saw how to get in the last unit.</span></span>

## <a name="getting-blobs-by-name"></a><span data-ttu-id="ab5fb-104">名前での BLOB の取得</span><span class="sxs-lookup"><span data-stu-id="ab5fb-104">Getting blobs by name</span></span>

<span data-ttu-id="ab5fb-105">`CloudBlobContainer` の `GetXXXReference` メソッドの 1 つを呼び出し、名前で `ICloudBlob` を取得します。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-105">Call one of the `GetXXXReference` methods on a `CloudBlobContainer` to get an `ICloudBlob` by name.</span></span> <span data-ttu-id="ab5fb-106">取得する BLOB の種類がわかっている場合、より具体的なメソッドを選んで使用します (`GetBlockBlobReference`、`GetAppendBlobReference`、または `GetPageBlobReference`)。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-106">If you know the type of the blob you are retrieving, prefer using one of the more specific methods (`GetBlockBlobReference`, `GetAppendBlobReference`, or `GetPageBlobReference`).</span></span>

<span data-ttu-id="ab5fb-107">これらのいずれのメソッドもネットワーク呼び出しを行ったり、BLOB が実際に存在するか確認しません。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-107">None of these methods make a network call, nor do they confirm whether or not the blob actually exists.</span></span> <span data-ttu-id="ab5fb-108">別のメソッドの `GetBlobReferenceFromServerAsync` は BLOB ストレージ API を呼び出し、BLOB がまだ存在しない場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-108">A separate method, `GetBlobReferenceFromServerAsync`, does call the Blob storage API and will throw an exception if the blob doesn't already exist.</span></span>

## <a name="listing-blobs-in-a-container"></a><span data-ttu-id="ab5fb-109">コンテナー内の BLOB を列挙する</span><span class="sxs-lookup"><span data-stu-id="ab5fb-109">Listing blobs in a container</span></span>

<span data-ttu-id="ab5fb-110">`CloudBlobContainer` の `ListBlobsSegmentedAsync` メソッドを使用すると、コンテナーの BLOB の一覧を入手できます。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-110">You can get a list of the blobs in a container using `CloudBlobContainer`'s `ListBlobsSegmentedAsync` method.</span></span> <span data-ttu-id="ab5fb-111">*Segmented* とは、戻された結果の別のページを指します。`ListBlobsSegmentedAsync` に 1 回呼び出しを行っても、1 ページにすべての結果を返されることは保証されていません。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-111">*Segmented* refers to the separate pages of results returned &mdash; a single call to `ListBlobsSegmentedAsync` is never guaranteed to return all the results in a single page.</span></span> <span data-ttu-id="ab5fb-112">ページを進めるには、それが戻した `ContinuationToken` を使用して繰り返し呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-112">We may need to call it repeatedly using the `ContinuationToken` it returns to work our way through the pages.</span></span> <span data-ttu-id="ab5fb-113">これは、アップロードまたはダウンロード用のコードよりも、BLOB を列挙するコードを少し複雑にしますが、コンテナー内のすべての BLOB を取得するために使用できる標準のパターンがあります。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-113">This makes the code for listing blobs a little more complex than the code for uploading or downloading, but there's a standard pattern you can use to get every blob in a container:</span></span>

```csharp
BlobContinuationToken continuationToken = null;
BlobResultSegment resultSegment = null; 

do
{
    resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

    // Do work here on resultSegment.Results

    continuationToken = resultSegment.ContinuationToken;
} while (continuationToken != null);
```

<span data-ttu-id="ab5fb-114">これは、`continuationToken` が `null` (結果の終わりを示します) になるまで、`ListBlobsSegmentedAsync` を繰り返し呼び出します。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-114">This will call `ListBlobsSegmentedAsync` repeatedly until `continuationToken` is `null`, which signals the end of the results.</span></span>

### <a name="processing-list-results"></a><span data-ttu-id="ab5fb-115">一覧の結果の処理</span><span class="sxs-lookup"><span data-stu-id="ab5fb-115">Processing list results</span></span>

<span data-ttu-id="ab5fb-116">`ListBlobsSegmentedAsync` から戻されるオブジェクトには、`IEnumerable<IListBlobItem>` 型の `Results` プロパティが含まれます。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-116">The object you'll get back from `ListBlobsSegmentedAsync` contains a `Results` property of type `IEnumerable<IListBlobItem>`.</span></span> <span data-ttu-id="ab5fb-117">`IListBlobItem` には、BLOB のコンテナーと URL に関するいくつかのプロパティが含まれますが、アップロードまたはダウンロード方法は含まれていません。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-117">`IListBlobItem`s contain a handful of properties about the blob's container and URL, but no upload or download methods.</span></span> <span data-ttu-id="ab5fb-118">これは、一部の結果オブジェクトが、個々の BLOB ではなく仮想ディレクトリを表す `CloudBlobDirectory` オブジェクトである場合があるからです。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-118">This is because some of the result objects may be `CloudBlobDirectory` objects that represent virtual directories rather than individual blobs.</span></span>

<span data-ttu-id="ab5fb-119">個々の BLOB に興味がある場合は、結果のフィルターに `OfType<>` メソッドを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-119">If you are only interested in individual blobs, you can use the `OfType<>` method to filter the results.</span></span> <span data-ttu-id="ab5fb-120">次に例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-120">Here are a few examples:</span></span>

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

<span data-ttu-id="ab5fb-121">`OfType<>` の使用には、`System.Linq` 名前空間への参照が必要です (`using System.Linq;`)。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-121">Using `OfType<>` will require a reference to the `System.Linq` namespace (`using System.Linq;`).</span></span>

## <a name="exercise"></a><span data-ttu-id="ab5fb-122">演習</span><span class="sxs-lookup"><span data-stu-id="ab5fb-122">Exercise</span></span>

<span data-ttu-id="ab5fb-123">アプリケーションの機能の 1 つに、API からの BLOB の一覧を取得する必要があるものがあります。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-123">One of the features in our app requires getting a list of blobs from the API.</span></span> <span data-ttu-id="ab5fb-124">前述のパターンを使用して、コンテナーのすべての BLOB を列挙します。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-124">We'll use the pattern shown above to list all the blobs in our container.</span></span> <span data-ttu-id="ab5fb-125">一覧を処理してゆくと、各 BLOB の名前を取得できます。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-125">As we process the list, we get the name of each blob.</span></span>

<span data-ttu-id="ab5fb-126">エディターで `BlobStorage.cs` を開き、次のコードを `GetNames` に入力します。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-126">Open `BlobStorage.cs` in the editor and fill in `GetNames` with the following code:</span></span>

```csharp
public async Task<IEnumerable<string>> GetNames()
{
    List<string> names = new List<string>();

    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);

    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    do
    {
        resultSegment = await container.ListBlobsSegmentedAsync(continuationToken);

        // Get the name of each blob.
        names.AddRange(resultSegment.Results.OfType<ICloudBlob>().Select(b => b.Name));

        continuationToken = resultSegment.ContinuationToken;
    } while (continuationToken != null);

    return names;
}
```

<span data-ttu-id="ab5fb-127">このメソッドによって返された名前は、`FilesController` によって URL に変換され処理されます。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-127">The names returned by this method are processed by `FilesController` to turn them into URLs.</span></span> <span data-ttu-id="ab5fb-128">これがクライアントに返されると、ページでハイパーリンクとしてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="ab5fb-128">When they are returned to the client, they are rendered as hyperlinks on the page.</span></span>