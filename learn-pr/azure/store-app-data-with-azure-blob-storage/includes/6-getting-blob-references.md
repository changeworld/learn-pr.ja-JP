<span data-ttu-id="83f6e-101">.NET Core 向けの Azure Storage SDK の個々の BLOB の操作には、`ICloudBlob` オブジェクトのインスタンスである *BLOB 参照*が必要です。</span><span class="sxs-lookup"><span data-stu-id="83f6e-101">Working with an individual blob in the Azure Storage SDK for .NET Core requires a *blob reference* &mdash; an instance of an `ICloudBlob` object.</span></span>

<span data-ttu-id="83f6e-102">`ICloudBlob` は、BLOB 名を使用して要求するか、コンテナーの BLOB 一覧から選択して取得することができます。</span><span class="sxs-lookup"><span data-stu-id="83f6e-102">You can get an `ICloudBlob` by requesting it with the blob's name or selecting it from a list of blobs in the container.</span></span> <span data-ttu-id="83f6e-103">いずれにも、最後のユニットで入手方法を確認した、`CloudBlobContainer` が必要です。</span><span class="sxs-lookup"><span data-stu-id="83f6e-103">Both require a `CloudBlobContainer`, which we saw how to get in the last unit.</span></span>

## <a name="getting-blobs-by-name"></a><span data-ttu-id="83f6e-104">名前での BLOB の取得</span><span class="sxs-lookup"><span data-stu-id="83f6e-104">Getting blobs by name</span></span>

<span data-ttu-id="83f6e-105">`CloudBlobContainer` の `GetXXXReference` メソッドの 1 つを呼び出し、名前で `ICloudBlob` を取得します。</span><span class="sxs-lookup"><span data-stu-id="83f6e-105">Call one of the `GetXXXReference` methods on a `CloudBlobContainer` to get an `ICloudBlob` by name.</span></span> <span data-ttu-id="83f6e-106">取得する BLOB の種類がわかっている場合は、固有のメソッド (`GetBlockBlobReference`、`GetAppendBlobReference`、または`GetPageBlobReference`) のいずれかを使用して、その BLOB の種類に対応したメソッドとプロパティを含むオブジェクトを取得します。</span><span class="sxs-lookup"><span data-stu-id="83f6e-106">If you know the type of the blob you are retrieving, use one of the specific methods (`GetBlockBlobReference`, `GetAppendBlobReference`, or `GetPageBlobReference`) to get an object that includes methods and properties tailored for that blob type.</span></span>

<span data-ttu-id="83f6e-107">これらのいずれのメソッドもネットワーク呼び出しを行ったり、対象の BLOB が実際に存在するかどうかを確認したりすることはありません。</span><span class="sxs-lookup"><span data-stu-id="83f6e-107">None of these methods make network calls, nor do they confirm whether or not the targeted blob actually exists.</span></span> <span data-ttu-id="83f6e-108">これらのメソッドは BLOB 参照オブジェクトをローカルで作成するだけです。このオブジェクトを使って、ネットワーク経由で操作を*行い*、ストレージ内の BLOB とやりとりするメソッドを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="83f6e-108">They only create a blob reference object locally, which can then be used to call methods that *do* operate over the network and interact with blobs in storage.</span></span> <span data-ttu-id="83f6e-109">別のメソッドの `GetBlobReferenceFromServerAsync` は BLOB ストレージ API を呼び出し、BLOB がまだ存在しない場合は、例外をスローします。</span><span class="sxs-lookup"><span data-stu-id="83f6e-109">A separate method, `GetBlobReferenceFromServerAsync`, does call the Blob storage API and will throw an exception if the blob doesn't already exist.</span></span>

## <a name="listing-blobs-in-a-container"></a><span data-ttu-id="83f6e-110">コンテナー内の BLOB を列挙する</span><span class="sxs-lookup"><span data-stu-id="83f6e-110">Listing blobs in a container</span></span>

<span data-ttu-id="83f6e-111">`CloudBlobContainer` の `ListBlobsSegmentedAsync` メソッドを使用すると、コンテナーの BLOB の一覧を入手できます。</span><span class="sxs-lookup"><span data-stu-id="83f6e-111">You can get a list of the blobs in a container using `CloudBlobContainer`'s `ListBlobsSegmentedAsync` method.</span></span> <span data-ttu-id="83f6e-112">*Segmented* とは、戻された結果の別のページを指します。`ListBlobsSegmentedAsync` に 1 回呼び出しを行っても、1 ページにすべての結果を返されることは保証されていません。</span><span class="sxs-lookup"><span data-stu-id="83f6e-112">*Segmented* refers to the separate pages of results returned &mdash; a single call to `ListBlobsSegmentedAsync` is never guaranteed to return all the results in a single page.</span></span> <span data-ttu-id="83f6e-113">ページを進めるには、それが戻した `ContinuationToken` を使用して繰り返し呼び出す必要があります。</span><span class="sxs-lookup"><span data-stu-id="83f6e-113">We may need to call it repeatedly using the `ContinuationToken` it returns to work our way through the pages.</span></span> <span data-ttu-id="83f6e-114">これは、アップロードまたはダウンロード用のコードよりも、BLOB を列挙するコードを少し複雑にしますが、コンテナー内のすべての BLOB を取得するために使用できる標準のパターンがあります。</span><span class="sxs-lookup"><span data-stu-id="83f6e-114">This makes the code for listing blobs a little more complex than the code for uploading or downloading, but there's a standard pattern you can use to get every blob in a container:</span></span>

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

<span data-ttu-id="83f6e-115">これは、`continuationToken` が `null` (結果の終わりを示します) になるまで、`ListBlobsSegmentedAsync` を繰り返し呼び出します。</span><span class="sxs-lookup"><span data-stu-id="83f6e-115">This will call `ListBlobsSegmentedAsync` repeatedly until `continuationToken` is `null`, which signals the end of the results.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="83f6e-116">`ListBlobsSegmentedAsync` の結果が 1 ページに収まらない場合もあります。</span><span class="sxs-lookup"><span data-stu-id="83f6e-116">Never assume that `ListBlobsSegmentedAsync` results will arrive in a single page.</span></span> <span data-ttu-id="83f6e-117">常に継続トークンを確認して、存在する場合にはそれを使用します。</span><span class="sxs-lookup"><span data-stu-id="83f6e-117">Always check for a continuation token and use it if it's present.</span></span>

### <a name="processing-list-results"></a><span data-ttu-id="83f6e-118">一覧の結果の処理</span><span class="sxs-lookup"><span data-stu-id="83f6e-118">Processing list results</span></span>

<span data-ttu-id="83f6e-119">`ListBlobsSegmentedAsync` から戻されるオブジェクトには、`IEnumerable<IListBlobItem>` 型の `Results` プロパティが含まれます。</span><span class="sxs-lookup"><span data-stu-id="83f6e-119">The object you'll get back from `ListBlobsSegmentedAsync` contains a `Results` property of type `IEnumerable<IListBlobItem>`.</span></span> <span data-ttu-id="83f6e-120">`IListBlobItem` インターフェイスには、BLOB のコンテナーと URL に関する少数のプロパティが含まれているだけなので、単独ではあまり役に立ちません。</span><span class="sxs-lookup"><span data-stu-id="83f6e-120">The `IListBlobItem` interface includes only a handful of properties about the blob's container and URL, and isn't very useful by itself.</span></span>

<span data-ttu-id="83f6e-121">`Results` から役立つ BLOB オブジェクトを取得するには、`OfType<>` メソッドを使用して、結果をフィルター処理してより具体的な BLOB オブジェクトの種類にキャストします。</span><span class="sxs-lookup"><span data-stu-id="83f6e-121">To get useful blob objects out of `Results`, you can use the `OfType<>` method to filter and cast the results to more specific blob object types.</span></span> <span data-ttu-id="83f6e-122">次に例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="83f6e-122">Here are a few examples:</span></span>

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

> [!NOTE]
> <span data-ttu-id="83f6e-123">`OfType<>` の使用には、`System.Linq` 名前空間への参照が必要です (`using System.Linq;`)。</span><span class="sxs-lookup"><span data-stu-id="83f6e-123">Using `OfType<>` requires a reference to the `System.Linq` namespace (`using System.Linq;`).</span></span>

## <a name="exercise"></a><span data-ttu-id="83f6e-124">演習</span><span class="sxs-lookup"><span data-stu-id="83f6e-124">Exercise</span></span>

<span data-ttu-id="83f6e-125">アプリケーションの機能の 1 つに、API からの BLOB の一覧を取得する必要があるものがあります。</span><span class="sxs-lookup"><span data-stu-id="83f6e-125">One of the features in our app requires getting a list of blobs from the API.</span></span> <span data-ttu-id="83f6e-126">前述のパターンを使用して、コンテナーのすべての BLOB を列挙します。</span><span class="sxs-lookup"><span data-stu-id="83f6e-126">We'll use the pattern shown above to list all the blobs in our container.</span></span> <span data-ttu-id="83f6e-127">一覧を処理してゆくと、各 BLOB の名前を取得できます。</span><span class="sxs-lookup"><span data-stu-id="83f6e-127">As we process the list, we get the name of each blob.</span></span>

<span data-ttu-id="83f6e-128">エディターで `BlobStorage.cs` を開き、`GetNames` を次のコードで置き換えて変更内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="83f6e-128">Open `BlobStorage.cs` in the editor, replace `GetNames` with the following code and save your changes.</span></span>

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

> [!TIP]
> <span data-ttu-id="83f6e-129">ここでメソッド シグネチャで `async` を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="83f6e-129">Note that the method signature now needs to specify `async`.</span></span>

<span data-ttu-id="83f6e-130">このメソッドによって返された名前は、`FilesController` によって URL に変換され処理されます。</span><span class="sxs-lookup"><span data-stu-id="83f6e-130">The names returned by this method are processed by `FilesController` to turn them into URLs.</span></span> <span data-ttu-id="83f6e-131">これがクライアントに返されると、ページでハイパーリンクとしてレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="83f6e-131">When they are returned to the client, they are rendered as hyperlinks on the page.</span></span>