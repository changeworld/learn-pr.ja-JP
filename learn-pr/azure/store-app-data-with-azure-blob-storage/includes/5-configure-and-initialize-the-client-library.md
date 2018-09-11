Azure Blob Storage を使用するアプリの一般的なワークフローを次に示します。

1. **構成を取得する**: 起動時に、ストレージ アカウントの構成を読み込みます。 これは通常、ストレージ アカウント接続文字列です。
1. **クライアントを初期化する**: 接続文字列を使用して、Azure Storage クライアント ライブラリを初期化します。 これにより、BLOB ストレージ API を操作する場合にアプリが使用するオブジェクトが作成されます。
1. **使用する**: コンテナーおよび BLOB に対して動作するクライアント ライブラリを指定して API 呼び出しを行います。

## <a name="configure-your-connection-string"></a>接続文字列の構成

コードを記述する前に、使用するストレージ アカウント用の接続文字列が必要になります。

ストレージ アカウント接続文字列には、アカウント キーが含まれます。 アカウント キーはシークレットとみなし、安全に格納する必要があります。 ここで、App Service アプリケーション設定内に接続文字列を格納することにします。 App Service アプリケーション設定は、アプリケーション シークレットにとって安全な場所ですが、この設計ではローカル開発をサポートしていないため、単独では堅牢なエンドツーエンド ソリューションとはなりません。

> [!WARNING]
> **コード内または保護されていない構成ファイル内には、ストレージ アカウント キーを置かないでください。** ストレージ アカウント キーにより、ご利用のストレージ アカウントへのフル アクセスを有効にすることができます。 キーが漏えいすると、回復不能な損害が発生し、高額の請求を招く恐れがあります。 ストレージのガイダンスおよび漏えいしたキーを回収する方法については、このモジュールの最後に示す「参考資料」セクションを参照してください。

## <a name="initialize-the-blob-storage-object-model"></a>BLOB ストレージ オブジェクト モデルを初期化する

Azure Storage SDK for .NET Core において、BLOB ストレージを使用するための標準的なパターンは、次の手順で構成されます。

1. 接続文字列を使用して `CloudStorageAccount.Parse` (または `TryParse`) を呼び出して、`CloudStorageAccount` を取得します。
1. `CloudStorageAccount` 上で `CreateCloudBlobClient` を呼び出して `CloudBlobClient` を取得します。
1. `CloudBlobClient` 上で `GetContainerReference` を呼び出して `CloudBlobContainer` を取得します。
1. そのコンテナー上のメソッドを使用することで、BLOB の一覧を取得および/または個々の BLOB への参照を取得してデータのアップロードおよびダウンロードを行います。

コード内で、ステップ 1 からステップ 3 は次のようになります。

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

この初期化コードで、ネットワーク経由の呼び出しが行われることはありません。 これは、間違った情報が原因で発生するいくつかの例外が後になってもスローされないことを意味します。 たとえば、`CloudStorageAccount.Parse` への呼び出しでは、接続文字列が不正に書式設定されていると、すぐに例外がスローされますが、接続文字列の指すストレージ アカウントが存在しない場合には、例外はスローされません。

## <a name="create-containers-at-startup"></a>起動時にコンテナーを作成する

アプリケーションの起動時またはアプリケーションで初めてコンテナーを使用するときにコンテナーを作成するには、`CloudBlobContainer` で `CreateIfNotExistsAsync` を呼び出すのが最善の方法です。

コンテナーは既に存在するものの、Azure Storage へのネットワーク呼び出しを行った場合には、`CreateIfNotExistsAsync` で例外がスローされません。 呼び出しは、コンテナーを使用するたびではなく、初期化中に 1 回行います。

## <a name="exercise"></a>演習

### <a name="clone-and-explore-the-unfinished-app"></a>未完了のアプリを複製して詳細を確認する

まず、GitHub からスターター アプリを複製してみましょう。 Cloud Shell ターミナルで、次のコマンドを実行してソース コードのコピーを取得し、それをエディターで開きます。

**TODO 最終的なリポジトリの URL を更新してください**

```console
git clone https://github.com/nickwalkmsft/FileUploader.git
cd FileUploader
code .
```

ファイル `Controllers/FilesController.cs` を開きます。 ここで作業することはありませんが、アプリで行われることを簡単に見ていきます。

このコントローラーでは、次の 3 つのアクションによって API が実装されます。

* **Index** (GET/api/Files) では、URL のリストが返されます。アップロードされた各ファイルに対する URL です。 アップロードされたファイルへのハイパーリンクのリストを作成するこのメソッドがアプリ フロント エンドによって呼び出されます。
* **Upload** (POST/api/Files) では、アップロードされたファイルが受信され、保存されます。
* **Download** (GET /api/Files/{filename}) では、名前を指定した個々のファイルがダウンロードされます。

各メソッドでは、該当する処理を実行するために `storage` と呼ばれる `IStorage` インスタンスが使用されます。 `Models/BlobStorage.cs` には、`IStorage` の不完全な実装があり、それをこれから埋めていきます。

### <a name="add-the-nuget-package"></a>NuGet パッケージを追加する

最初に、Azure Storage SDK への参照を追加します。 ターミナルで、次の内容を実行します。

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

これにより、確実に BLOB ストレージ クライアント ライブラリの最新バージョンを使用することになります。

### <a name="configure"></a>構成

アプリを実行するために必要な構成値は、ストレージ アカウント接続文字列と、ファイルを格納するためにアプリによって使用されるコンテナーの名前です。 この演習では、Azure App Service でアプリを実行するだけなので、App Service のベスト プラクティスに従って、値を App Service アプリケーション設定に格納します。 これは、App Service インスタンスを作成するときに行うので、現時点では何もする必要はありません。

構成の*使用*に関しては、対象のスターター アプリに必要としているプラミングが既に含まれています。 `BlobStorage` 内の `IOptions<AzureStorageConfig>` コンストラクター パラメーターには 2 つのプロパティがあります。ストレージ アカウントの接続文字列と、アプリによって BLOB が格納されるコンテナーの名前です。 `Startup.cs` の `ConfigureServices` メソッド内には、アプリの起動時に構成から値を読み込むコードがあります。

### <a name="initialize"></a>初期化

`Models/BlobStorage.cs` を開きます。 ファイルの先頭に次の `using` ステートメントを追加して、演習中に追加するコード用に準備します。

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

`Initialize` メソッドを検索します。 `BlobStorage` が初めて使用されるときに、ご利用のアプリによってこのメソッドが呼び出されます。 関心がある場合は、`Startup.cs` 内にある `ConfigureServices` を調べて、それがどのように行われるかを確認することができます。

コンテナーがまだ存在していない場合は、`Initialize` でコンテナーを作成します。 `Initialize` の現在の実装を次のコードで置き換えて、作業内容を保存します。

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```