BLOB への参照を設定したら、データをアップロードおよびダウンロードすることができます。 `ICloudBlob` オブジェクトには、ソースおよびターゲットとしてバイト配列、ストリーム、およびファイルをサポートする `Upload` メソッドと `Download` メソッドが含まれています。 特定の種類には、便宜上、追加のメソッドが含まれています。たとえば、`CloudBlockBlob` では、`UploadTextAsync` および `DownloadTextAsync` を使用した文字列のアップロードとダウンロードがサポートされています。

## <a name="creating-new-blobs"></a>新しい BLOB の作成

新しい BLOB を作成するには、存在していない BLOB に対して `Upload` メソッドのいずれかを呼び出します。 これにより、BLOB の作成とデータのアップロードという 2 つのことが行われます。 

## <a name="moving-data-to-and-from-blobs"></a>BLOB との間でのデータ移動

BLOB との間のデータ移動は、時間がかかるネットワーク操作です。 Azure Storage SDK for .NET Core では、ネットワーク アクティビティを必要とするすべてのメソッドから `Task` が返されます。したがって、必要に応じてご利用のコント ローラー メソッドが `async` であること、さらにメソッド呼び出しを `await` しているのであって、メソッド呼び出しで `Wait` しているのではないことを確認します。

大規模なデータ オブジェクトを操作する場合の一般的な推奨事項は、バイト配列や文字列のようなメモリ内の構造ではなくストリームを使用するということです。 これにより、ターゲットへの送信前に、すべての内容がメモリ内にバッファリングされるのを回避できます。 ASP.NET Core では、要求と応答でのストリームの読み取りおよび書き込みがサポートされています。

## <a name="concurrent-access"></a>同時アクセス

ご利用のアプリによって BLOB が使用されているときにその BLOB が他のプロセスによって追加、変更、または削除される可能性があります。 常に、防衛的なコードを作成すると共に、ダウンロードを試みるとすぐに削除される BLOB や予期していないときに内容が変更される BLOB など同時実行により発生する問題について考えます。 AccessConditions および BLOB リースを使用して、BLOB への同時アクセスを管理する方法については、このモジュールの最後に記載されている「その他のリソース」セクションを参照してください。

## <a name="exercise"></a>演習

アップロードおよびダウンロードのコードを追加することによってアプリを完了してから、テストのためにそれを Azure App Service にデプロイしましょう。

### <a name="upload"></a>アップロード

BLOB をアップロードするには、コンテナーから `CloudBlockBlob` を取得する `GetBlockBlobReference` を使用して `BlobStorage.Save` メソッドを実装します。 `FilesController.Upload` からは `Save` にファイル ストリームが渡されるので、効率が最大になるように `UploadFromStreamAsync` を使用してアップロードを実行することができます。

エディターで `BlobStorage.cs` を開き、`Save` 実装に次のコードを入力します。

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
> ここに示したストリーム ベースのアップロード コードは、バイト配列にファイルを読み込んでから Azure Blob Storage に送信するよりも効率的です。 ただし、クライアントからファイルを取得するのに使用する `IFormFile` 手法は、真のエンドツーエンド ストリーミング実装ではなく、小さいファイルのアップロードを処理する場合にのみ適しています。 完全にストリーミングされたファイル アップロードについては、このモジュールの最後に記載されている「その他のリソース」セクションを参照してください。

### <a name="download"></a>[ダウンロード]

`BlobStorage.Load` からは `Stream` が返されます。すなわち、作成するコードでは BLOB ストレージからバイトを物理的に移動する必要は全くなく、BLOB ストリームへの参照を返す必要があるだけです。 それは、`OpenReadAsync` を使用して行うことができます。 ASP.NET Core では、クライアント応答がビルドされると、ストリームの読み取りおよび終了の処理が行われます。

`Load` に次のコードを入力します。

```csharp
public Task<Stream> Load(string name)
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.GetBlobReference(name).OpenReadAsync();
}
```

### <a name="deploy-and-run-in-azure"></a>Azure 内でデプロイして実行する

アプリは完成しましたので、デプロイして動作を確認してみましょう。 Azure Cloud Shell ターミナルで次のコードを実行して、コードをビルドし、それを新しい App Service インスタンスにデプロイします。 また、ストレージ アカウント接続文字列を構成に追加します。

```console

```

...

いくつかのファイルをアップロードおよびダウンロードすることでアプリをテストし、次に、シェル内で次を実行してアップロードされた BLOB を確認します。

```console

```

**ポータルからの TODO pic**