Azure Storage クライアント ライブラリでは、Azure ストレージ アカウントとのやりとりに使用されるオブジェクト モデルが提供されます。 これは Azure ストレージ アカウントにすばやく接続して、Azure Storage サービス API を使用するために使用されます。

ここでは、コードでアカウント オブジェクト モデルを使用して、自分の Azure ストレージ アカウントに接続します。

## <a name="azure-storage-client-library-object-model"></a>Azure Storage クライアント ライブラリ オブジェクト モデル

Azure Storage クライアント ライブラリ オブジェクト モデルの使用方法を理解することが、Azure ストレージ アカウントに接続するときに実装を分かりやすくするための鍵です。

.NET Core クライアント ライブラリ内のストレージ アカウント オブジェクト モデルの基盤は、**CloudStorageAccount** です。 オブジェクト モデルを初期化するための最もシンプルな方法は、`CloudStorageAccount.Parse` または `CloudStorageAccount.TryParse` を使用して接続文字列を解析することです。

クライアント ライブラリは、必要な操作が呼び出されるまで、接続を試みません。 `Parse()` と `TryParse()` は、接続文字列が適切な形式になることを保証するだけで、アカウントが存在することやキーが正しいことの検証は行いません。 `Parse()` または `TryParse()` メソッドの呼び出しによって返される結果の `CloudStorageAccount` インスタンスでは、BLOB、ファイル、テーブル、またはキューのストレージの種類用にクライアントを作成するメソッドが公開されます。 このメソッドは、Azure Blob、Files、Queue、および Table Storage サービス用のサービス クライアント オブジェクトのインスタンスを作成するために使用されます。 次のコード スニペットでは、Blob Storage に使用するクライアントを作成する例を示します。

```c#
using Microsoft.WindowsAzure.Storage;

var storageAccount = CloudStorageAccount.Parse("your-storage-key-connection-string");
var blobClient = storageAccount.CreateCloudBlobClient()
```

`CloudStorageAccount` とクライアント オブジェクトは軽量であり、オンデマンドで作成または事前に作成して、アプリケーション内で共有することができます。 標準的なアプローチは、アプリケーションのエントリ ポイントの近くで `CloudStorageAccount.TryParse()` を使用してインスタンスを作成し、それをアプリケーション内でクライアント インスタンスを作成するために使用できるようにすることです。

## <a name="summary"></a>まとめ

Azure Storage クライアント ライブラリ オブジェクト モデルでは、自分の Azure ストレージ アカウントに接続するシンプルな方法が用意されています。 接続文字列を指定するだけで、オブジェクト モデルによってその形式がチェックされ、アカウントが存在することが確認されます。 `CloudStorageAccount` インスタンスを作成すると、BLOB、ファイル、テーブル、またはキューのストレージの種類用にクライアントを作成できるようになります。
