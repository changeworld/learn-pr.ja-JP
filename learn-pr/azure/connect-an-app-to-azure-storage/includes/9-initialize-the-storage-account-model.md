Azure Storage クライアント ライブラリでは、Azure ストレージ アカウントとのやりとりに使用されるオブジェクト モデルが提供されます。 これは Azure ストレージ アカウントにすばやく接続して、Azure Storage サービス API を使用するために使用されます。 

## <a name="azure-storage-client-library-object-model"></a>Azure Storage クライアント ライブラリ オブジェクト モデル

::: zone pivot="csharp"

.NET Core のクライアント ライブラリでは、ストレージ アカウント オブジェクト モデルの基礎はクラス`CloudStorageAccount`します。 オブジェクト モデルを初期化するための最もシンプルな方法は、`CloudStorageAccount.Parse` または `CloudStorageAccount.TryParse` を使用して接続文字列を解析することです。

> [!NOTE]
> クライアント ライブラリでは、必要な操作が呼び出されるまで接続が試行されません。 `Parse()` と `TryParse()` は、接続文字列が適切な形式になることを保証するだけで、アカウントが存在することやキーが正しいことの検証は行いません。 

その結果、`CloudStorageAccount`から返されるインスタンス、`Parse()`または`TryParse()`メソッドの呼び出しは、クライアントを Azure Blob、ファイル、キュー、およびテーブル ストレージ サービスにアクセスするオブジェクトを作成するメソッドを公開します。 

次のコード スニペットでは、Blob Storage に使用するクライアントを作成する例を示します。

```csharp
using Microsoft.WindowsAzure.Storage;

CloudStorageAccount storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");

CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` とクライアント オブジェクトは軽量であり、オンデマンドで作成または事前に作成して、アプリケーション内で共有することができます。 標準的なアプローチは、アプリケーションのエントリ ポイントの近くで `CloudStorageAccount.TryParse()` を使用してインスタンスを作成し、それをアプリケーション内でクライアント インスタンスを作成するために使用できるようにすることです。

特定の記憶域の種類をクライアント オブジェクトを作成したら、実際の作業を実行するのにメソッドを使用できます。 ネットワーク呼び出しを行うメソッドは意図的に非同期 - .NET で使用して、`async`と`await`効率的にこれらのメソッドを使用するキーワード。

例として、使用できる、`CloudBlobClient`を作成する、 _blob コンテナー_し、blob storage にファイルをアップロードします。

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

::: zone pivot="javascript"

ストレージ アカウントのオブジェクト モデルでの基盤となる、 **Node.js、JavaScript 用の Microsoft Azure Storage クライアント ライブラリ**は、`azurestorage`オブジェクト。 追加することによってこれが作成される、 **azure storage**モジュールを使用してアプリケーションを`require`ステートメント。

```javascript
const storage = require('azure-storage');
```

このオブジェクトには、一連の_ファクトリ_を Azure storage の各ファセットを使用する特定のオブジェクトを作成するメソッド。 呼び出す`createXXX`各オブジェクトを作成する方法。

| 種類 | 方法 | 戻り値 |
|--------|---------|-------------|
| **BLOB** | `createBlobService` | `BlobService` |
| **テーブル** | `createTableService` | `TableService` |
| **キュー** | `createQueueService` | `QueueService` |
| **ファイル** | `createFileService` | `FileService` |

> [!NOTE]
> クライアント ライブラリでは、必要な操作が呼び出されるまで接続が試行されません。 これらの各`create`メソッドは、ストレージの種類へのアクセスを表す軽量のオブジェクトを返す&mdash;接続または使用されているアクセス キーを検証しません。

特定の記憶域の種類のサービス オブジェクトを作成したら、実際の作業を実行するのにメソッドを使用できます。 ネットワーク呼び出しを行うメソッドでは、意図的に非同期です。 ライブラリは現在サポートされている_コールバック_非同期の結果を返します。 たとえば、blob コンテナーを作成するコードを次に示します。

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

このアプローチは、正常に動作が、大量のコードを管理できなくなる可能性のコールバックに追加する傾向があります。 JavaScript でのより優れたアプローチは、使用することです。 _promise_これらのメソッドを使用します。 Promise コールバック スタイルのメソッドに変換されますが、いくつかの優れたライブラリがある&mdash;必要に応じて 1 つを選択することができます。

ここでは、使用、`util.promisify`ノードと使用から機能、`BlobService`コンテナーを作成し、blob ストレージにファイルをアップロードします。 さらに、使用を`async`と`await`少し当然ながら、promise を使用するキーワード。

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