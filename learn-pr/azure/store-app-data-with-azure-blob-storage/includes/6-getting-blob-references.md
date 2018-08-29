.NET Core 向けの Azure Storage SDK の個々の BLOB の操作には、`ICloudBlob` オブジェクトのインスタンスである *BLOB 参照*が必要です。

`ICloudBlob` は、BLOB 名を使用して要求するか、コンテナーの BLOB 一覧から選択して取得することができます。 いずれにも、最後のユニットで入手方法を確認した、`CloudBlobContainer` が必要です。

## <a name="getting-blobs-by-name"></a>名前での BLOB の取得

`CloudBlobContainer` の `GetXXXReference` メソッドの 1 つを呼び出し、名前で `ICloudBlob` を取得します。 取得する BLOB の種類がわかっている場合、より具体的なメソッドを選んで使用します (`GetBlockBlobReference`、`GetAppendBlobReference`、または `GetPageBlobReference`)。

これらのいずれのメソッドもネットワーク呼び出しを行ったり、BLOB が実際に存在するか確認しません。 別のメソッドの `GetBlobReferenceFromServerAsync` は BLOB ストレージ API を呼び出し、BLOB がまだ存在しない場合は、例外をスローします。

## <a name="listing-blobs-in-a-container"></a>コンテナー内の BLOB を列挙する

`CloudBlobContainer` の `ListBlobsSegmentedAsync` メソッドを使用すると、コンテナーの BLOB の一覧を入手できます。 *Segmented* とは、戻された結果の別のページを指します。`ListBlobsSegmentedAsync` に 1 回呼び出しを行っても、1 ページにすべての結果を返されることは保証されていません。 ページを進めるには、それが戻した `ContinuationToken` を使用して繰り返し呼び出す必要があります。 これは、アップロードまたはダウンロード用のコードよりも、BLOB を列挙するコードを少し複雑にしますが、コンテナー内のすべての BLOB を取得するために使用できる標準のパターンがあります。

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

これは、`continuationToken` が `null` (結果の終わりを示します) になるまで、`ListBlobsSegmentedAsync` を繰り返し呼び出します。

### <a name="processing-list-results"></a>一覧の結果の処理

`ListBlobsSegmentedAsync` から戻されるオブジェクトには、`IEnumerable<IListBlobItem>` 型の `Results` プロパティが含まれます。 `IListBlobItem` には、BLOB のコンテナーと URL に関するいくつかのプロパティが含まれますが、アップロードまたはダウンロード方法は含まれていません。 これは、一部の結果オブジェクトが、個々の BLOB ではなく仮想ディレクトリを表す `CloudBlobDirectory` オブジェクトである場合があるからです。

個々の BLOB に興味がある場合は、結果のフィルターに `OfType<>` メソッドを使用することができます。 次に例をいくつか示します。

```csharp
// Get all blobs
var allBlobs = resultSegment.Results.OfType<ICloudBlob>();

// Get only block blobs
var blockBlobs = resultSegment.Results.OfType<CloudBlockBlob();
```

`OfType<>` の使用には、`System.Linq` 名前空間への参照が必要です (`using System.Linq;`)。

## <a name="exercise"></a>演習

アプリケーションの機能の 1 つに、API からの BLOB の一覧を取得する必要があるものがあります。 前述のパターンを使用して、コンテナーのすべての BLOB を列挙します。 一覧を処理してゆくと、各 BLOB の名前を取得できます。

エディターで `BlobStorage.cs` を開き、次のコードを `GetNames` に入力します。

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

このメソッドによって返された名前は、`FilesController` によって URL に変換され処理されます。 これがクライアントに返されると、ページでハイパーリンクとしてレンダリングされます。