<span data-ttu-id="2b8b0-101">Azure Storage クライアント ライブラリでは、Azure ストレージ アカウントとのやりとりに使用されるオブジェクト モデルが提供されます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-101">The Azure Storage client library provides an object model that is used to interact with Azure storage accounts.</span></span> <span data-ttu-id="2b8b0-102">これは Azure ストレージ アカウントにすばやく接続して、Azure Storage サービス API を使用するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-102">It's used to quickly connect to an Azure storage account and use the Azure Storage service APIs.</span></span> 

## <a name="azure-storage-client-library-object-model"></a><span data-ttu-id="2b8b0-103">Azure Storage クライアント ライブラリ オブジェクト モデル</span><span class="sxs-lookup"><span data-stu-id="2b8b0-103">Azure Storage client library object model</span></span>

<span data-ttu-id="2b8b0-104">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="2b8b0-104">::: zone pivot="csharp"</span></span>

<span data-ttu-id="2b8b0-105">.NET Core クライアント ライブラリ内のストレージ アカウント オブジェクト モデルの基盤は、クラス `CloudStorageAccount` です。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-105">The foundation of the storage account object model in the .NET Core client library is the class `CloudStorageAccount`.</span></span> <span data-ttu-id="2b8b0-106">オブジェクト モデルを初期化するための最もシンプルな方法は、`CloudStorageAccount.Parse` または `CloudStorageAccount.TryParse` を使用して接続文字列を解析することです。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-106">The simplest way to initialize the object model is to use `CloudStorageAccount.Parse` or `CloudStorageAccount.TryParse` to parse the connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="2b8b0-107">クライアント ライブラリは、必要な操作が呼び出されるまで、接続を試みません。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-107">The client library will not attempt to connect until an operation is invoked that requires it.</span></span> <span data-ttu-id="2b8b0-108">`Parse()` と `TryParse()` は、接続文字列が適切な形式になることを保証するだけで、アカウントが存在することやキーが正しいことの検証は行いません。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-108">`Parse()` and `TryParse()` only guarantee that the connection string is well-formatted; they don't verify that the account exists or that the key is correct.</span></span> 

<span data-ttu-id="2b8b0-109">`Parse()` または `TryParse()` メソッドの呼び出しによって返される結果の `CloudStorageAccount` インスタンスでは、Azure Blob、Azure Files、Azure Queue、および Azure Table Storage のサービスにアクセスするためのクライアントを作成するメソッドが公開されます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-109">The resulting `CloudStorageAccount` instance returned from the `Parse()` or `TryParse()` method call exposes methods to create a client objects to access the Azure Blob, Files, Queue and Table storage services.</span></span> 

<span data-ttu-id="2b8b0-110">次のコード スニペットでは、BLOB ストレージに使用するクライアントを作成する例を示します。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-110">The code snippet below shows an example of creating a client to use for blob storage:</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

<span data-ttu-id="2b8b0-111">`CloudStorageAccount` とクライアント オブジェクトは軽量であり、オンデマンドで作成または事前に作成して、アプリケーション内で共有することができます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-111">`CloudStorageAccount` and the client objects are lightweight and can be created on demand or created up front to be shared within your application.</span></span> <span data-ttu-id="2b8b0-112">標準的なアプローチは、アプリケーションのエントリ ポイントの近くで `CloudStorageAccount.TryParse()` を使用してインスタンスを作成し、それをアプリケーション内でクライアント インスタンスを作成するために使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-112">A standard approach is to use `CloudStorageAccount.TryParse()` near the entry point of your application to create an instance, and make it available within your application for creating client instances.</span></span>

<span data-ttu-id="2b8b0-113">特定のストレージの種類のクライアント オブジェクトを作成したら、メソッドを使用して実際の作業を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-113">Once you have a client object to a specific storage type, you can use methods to perform actual work.</span></span> <span data-ttu-id="2b8b0-114">ネットワーク呼び出しを行うメソッドは意図的に非同期になっています。 .NET では、これらのメソッドを効率的に使用するため、`async` と `await` キーワードを使用します。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-114">Methods which make network calls are intentionally asynchronous &mdash; in .NET we use the `async` and `await` keywords to work with these methods efficiently.</span></span>

<span data-ttu-id="2b8b0-115">例として、`CloudBlobClient` を使用して、_BLOB コンテナー_を作成し、ファイルを BLOB ストレージにアップロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-115">As an example, we can use the `CloudBlobClient` to create a _blob container_ and upload a file to blob storage.</span></span>

```csharp
// Create a local CloudBlobContainer object. No network call.
string containerName = "myblobcontainer";
CloudBlobContainer blobContainer = blobClient.GetContainerReference(containerName);

// This makes an actual service call to the Azure Storage service. 
// Unless this call fails, the container will have been created.
await blobContainer.CreateAsync();

// Create a local object to represent our blob. No network call.
string blobName = "myphoto";
CloudBlockBlob blob = blobContainer.GetBlockBlobReference(blobName);

// This transfers data in the file to the blob on the service.
string filename = "photo.png";
await blob.UploadFromFileAsync(fileName);
```

<span data-ttu-id="2b8b0-116">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2b8b0-116">::: zone-end</span></span>

<span data-ttu-id="2b8b0-117">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="2b8b0-117">::: zone pivot="javascript"</span></span>

<span data-ttu-id="2b8b0-118">**Node.js と JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**におけるストレージ アカウント オブジェクト モデルの基盤は、`azurestorage` オブジェクトです。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-118">The foundation of the storage account object model in the **Microsoft Azure Storage Client Library for Node.js and JavaScript** is the `azurestorage` object.</span></span> <span data-ttu-id="2b8b0-119">これは、`require` ステートメントを使用して **azure-storage** モジュールをご自分のアプリに追加することによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-119">This is created by adding the **azure-storage** module to your app through a `require` statement.</span></span>

```javascript
const storage = require('azure-storage');
```

<span data-ttu-id="2b8b0-120">このオブジェクトにより、Azure Storage の各ファセットを使用するための固有のオブジェクトを作成する一連の_ファクトリ_ メソッドが提供されます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-120">This object provides a series of _factory_ methods that create specific objects to work with each facet of Azure storage.</span></span> <span data-ttu-id="2b8b0-121">`createXXX` メソッドを呼び出して、各オブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-121">You call `createXXX` methods to create each object.</span></span>

| <span data-ttu-id="2b8b0-122">種類</span><span class="sxs-lookup"><span data-stu-id="2b8b0-122">Type</span></span> | <span data-ttu-id="2b8b0-123">メソッド</span><span class="sxs-lookup"><span data-stu-id="2b8b0-123">Method</span></span> | <span data-ttu-id="2b8b0-124">戻り値</span><span class="sxs-lookup"><span data-stu-id="2b8b0-124">Returns</span></span> |
|--------|---------|-------------|
| <span data-ttu-id="2b8b0-125">**Blob**</span><span class="sxs-lookup"><span data-stu-id="2b8b0-125">**Blob**</span></span> | `createBlobService` | `BlobService` |
| <span data-ttu-id="2b8b0-126">**Table**</span><span class="sxs-lookup"><span data-stu-id="2b8b0-126">**Table**</span></span> | `createTableService` | `TableService` |
| <span data-ttu-id="2b8b0-127">**Queue**</span><span class="sxs-lookup"><span data-stu-id="2b8b0-127">**Queue**</span></span> | `createQueueService` | `QueueService` |
| <span data-ttu-id="2b8b0-128">**File**</span><span class="sxs-lookup"><span data-stu-id="2b8b0-128">**File**</span></span> | `createFileService` | `FileService` |

> [!NOTE]
> <span data-ttu-id="2b8b0-129">クライアント ライブラリは、必要な操作が呼び出されるまで、接続を試みません。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-129">The client library will not attempt to connect until an operation is invoked that requires it.</span></span> <span data-ttu-id="2b8b0-130">これらの `create` メソッドはそれぞれ、ストレージの種類へのアクセスを表す軽量のオブジェクトを返します。接続または使用されているアクセス キーの検証は行いません。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-130">Each of these `create` methods return a lightweight object representing access to the storage type&mdash;it does not validate the connection or the access key being used.</span></span>

<span data-ttu-id="2b8b0-131">特定のストレージの種類のサービス オブジェクトを作成したら、メソッドを使用して実際の作業を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-131">Once you have a service object to a specific storage type, you can use methods to perform actual work.</span></span> <span data-ttu-id="2b8b0-132">ネットワーク呼び出しを行うメソッドは、意図的に非同期になっています。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-132">Methods that make network calls are intentionally asynchronous.</span></span> <span data-ttu-id="2b8b0-133">ライブラリでは現在、非同期の結果を返す_コールバック_がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-133">The library currently supports _callbacks_ to return asynchronous results.</span></span> <span data-ttu-id="2b8b0-134">例として、BLOB コンテナーを作成するコードを次に示します。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-134">For example, here is code that creates a blob container.</span></span>

```javascript
var azure = require('azure-storage');
var blobService = azure.createBlobService();

blobService.createContainerIfNotExists('myblobcontainer', function(err, result, response) {
  if (!err) {
    // if result.created = true, container was created.
    // if result.created = false, container already existed.
  }
});
```

<span data-ttu-id="2b8b0-135">この方法でも問題ありませんが、コールバックに多くのコードが追加される傾向があるため、管理されなくなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-135">This approach works fine, but it tends to lead to a lot of code being added into the callbacks, which can get unmanageable.</span></span> <span data-ttu-id="2b8b0-136">JavaScript でのより優れたアプローチは、_promise_ を使用してこれらのメソッドを使用することです。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-136">A better approach in JavaScript is to use _promises_ to work with these methods.</span></span> <span data-ttu-id="2b8b0-137">callback-style メソッドを promise に変換する優れたライブラリがいくつかあり、必要に応じて選択できます。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-137">There are several great libraries which will convert callback-style methods into promises&mdash;you can pick the one you prefer.</span></span>

<span data-ttu-id="2b8b0-138">ここでは、Node の `util.promisify` 機能と `BlobService` を使用して、コンテナーを作成し、ファイルを BLOB ストレージにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-138">Here, we'll use the `util.promisify` feature from Node and use the `BlobService` to create the container and upload a file to blob storage.</span></span> <span data-ttu-id="2b8b0-139">さらに、`async` と `await` のキーワードを使用して、promise をもう少し自然に使用します。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-139">In addition, we'll use the `async` and `await` keywords to work with the promises a bit more naturally.</span></span>

```javascript
const util = require('util');
const storage = require('azure-storage');

const blobService = storage.createBlobService();
const createContainerAsync = util.promisify(blobService.createContainerIfNotExists).bind(blobService);
const uploadBlobAsync = util.promisify(blobService.createBlockBlobFromLocalFile).bind(blobService);

async function main() {
    try {
        // This makes an actual service call to the Azure Storage service. 
        // Unless this call fails, the container will have been created.
        await createContainerAsync(containerName);

        // This transfers data in the file to the blob on the service.
        var uploadResult = await uploadBlobAsync(containerName, "myphoto", "photo.png");
        if (uploadResult) {
            console.log("blob uploaded");
        }
    }
    catch (err) {
        console.log(err.message);
    }
}

main();
```
<span data-ttu-id="2b8b0-140">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="2b8b0-140">::: zone-end</span></span>

<span data-ttu-id="2b8b0-141">アプリでこれを試してみましょう。</span><span class="sxs-lookup"><span data-stu-id="2b8b0-141">Let's try this in our app.</span></span>