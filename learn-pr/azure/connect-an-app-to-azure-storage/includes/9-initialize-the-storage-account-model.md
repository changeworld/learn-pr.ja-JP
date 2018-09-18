Azure Storage クライアント ライブラリでは、Azure ストレージ アカウントとのやりとりに使用されるオブジェクト モデルが提供されます。 これは Azure ストレージ アカウントにすばやく接続して、Azure Storage サービス API を使用するために使用されます。 

## <a name="azure-storage-client-library-object-model"></a>Azure Storage クライアント ライブラリ オブジェクト モデル

::: zone pivot="csharp"

.NET Core クライアント ライブラリ内のストレージ アカウント オブジェクト モデルの基盤は、クラス `CloudStorageAccount` です。 オブジェクト モデルを初期化するための最もシンプルな方法は、`CloudStorageAccount.Parse` または `CloudStorageAccount.TryParse` を使用して接続文字列を解析することです。

> [!NOTE]
> クライアント ライブラリは、必要な操作が呼び出されるまで、接続を試みません。 `Parse()` と `TryParse()` は、接続文字列が適切な形式になることを保証するだけで、アカウントが存在することやキーが正しいことの検証は行いません。 

`Parse()` または `TryParse()` メソッドの呼び出しによって返される結果の `CloudStorageAccount` インスタンスでは、Azure Blob、Azure Files、Azure Queue、および Azure Table Storage のサービスにアクセスするためのクライアントを作成するメソッドが公開されます。 

次のコード スニペットでは、BLOB ストレージに使用するクライアントを作成する例を示します。

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` とクライアント オブジェクトは軽量であり、オンデマンドで作成または事前に作成して、アプリケーション内で共有することができます。 標準的なアプローチは、アプリケーションのエントリ ポイントの近くで `CloudStorageAccount.TryParse()` を使用してインスタンスを作成し、それをアプリケーション内でクライアント インスタンスを作成するために使用できるようにします。

特定のストレージの種類のクライアント オブジェクトを作成したら、メソッドを使用して実際の作業を行うことができます。 ネットワーク呼び出しを行うメソッドは意図的に非同期になっています。 .NET では、これらのメソッドを効率的に使用するため、`async` と `await` キーワードを使用します。

例として、`CloudBlobClient` を使用して、_BLOB コンテナー_を作成し、ファイルを BLOB ストレージにアップロードすることができます。

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

::: zone-end

::: zone-pivot="javascript"

**Node.js と JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**におけるストレージ アカウント オブジェクト モデルの基盤は、`azurestorage` オブジェクトです。 これは、`require` ステートメントを使用して **azure-storage** モジュールをご自分のアプリに追加することによって作成されます。

```javascript
const storage = require('azure-storage');
```

このオブジェクトにより、Azure Storage の各ファセットを使用するための固有のオブジェクトを作成する一連の_ファクトリ_ メソッドが提供されます。 `createXXX` メソッドを呼び出して、各オブジェクトを作成します。

| 種類 | メソッド | 戻り値 |
|--------|---------|-------------|
| **Blob** | `createBlobService` | `BlobService` |
| **Table** | `createTableService` | `TableService` |
| **Queue** | `createQueueService` | `QueueService` |
| **File** | `createFileService` | `FileService` |

> [!NOTE]
> クライアント ライブラリは、必要な操作が呼び出されるまで、接続を試みません。 これらの `create` メソッドはそれぞれ、ストレージの種類へのアクセスを表す軽量のオブジェクトを返します。接続または使用されているアクセス キーの検証は行いません。 

特定のストレージの種類のサービス オブジェクトを作成したら、メソッドを使用して実際の作業を行うことができます。 ネットワーク呼び出しを行うメソッドは、意図的に非同期になっています。 ライブラリでは現在、非同期の結果を返す_コールバック_がサポートされています。 例として、BLOB コンテナーを作成するコードを次に示します。

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

この方法でも問題ありませんが、コールバックに多くのコードが追加される傾向があるため、管理されなくなる可能性があります。 JavaScript でのより優れたアプローチは、_promise_ を使用してこれらのメソッドを使用することです。 callback-style メソッドを promise に変換する優れたライブラリがいくつかあり、必要に応じて選択できます。

ここでは、Node の `util.promisify` 機能と `BlobService` を使用して、コンテナーを作成し、ファイルを BLOB ストレージにアップロードします。 さらに、`async` と `await` のキーワードを使用して、promise をもう少し自然に使用します。

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
::: zone-end

アプリでこれを試してみましょう。