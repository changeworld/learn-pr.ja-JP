Azure Blob Storage を使用するアプリの一般的なワークフローを次に示します。

1. **構成を取得する**: 起動時に、アカウント キーを含む接続文字列などの構成を読み込みます。 これは API 呼び出しを認証するのに必要です。
1. **クライアントを初期化する**: 接続文字列を使用して、Azure Storage クライアント ライブラリを初期化します。 これにより、BLOB ストレージ API を操作する場合にアプリが使用するオブジェクトが作成されます。
1. **使用する**: コンテナーおよび BLOB に対して動作するクライアント ライブラリを指定して API 呼び出しを行います。

## <a name="configure-your-connection-string"></a>接続文字列の構成

コードを記述する前に、使用するストレージ アカウント用の接続文字列が必要になります。 

接続文字列には、アカウント キーが含まれています。 アカウント キーはシークレットとみなし、安全に格納する必要があります。 App Service アプリケーション設定内に接続文字列を格納することにします。 アプリケーション設定は、アプリケーション シークレットにとって安全な場所ですが、ローカル開発をサポートしていないため、単独では堅牢なエンド ツーエンド ソリューションとはなりません。

> [!WARNING]
> **コード内または保護されていない構成ファイル内には、ストレージ アカウント キーを置かないでください。** ストレージ アカウント キーにより、ご利用のストレージ アカウントへのフル アクセスを有効にすることができます。 キーが漏洩すると、回復不能な損害が発生し、高額の請求を招く恐れがあります。 ストレージのガイダンスおよび漏洩したキーを回収する方法については、このモジュールの最後に示す「その他のリソース」セクションを参照してください。

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

この初期化コードで、ネットワーク経由の呼び出しが行われることはありません。 これは、誤った情報により発生した例外をスローする処理が後回しにされることを意味します。たとえば、`GetContainerReference` の呼び出しは、アカウント内にコンテナーが実際に存在するかどうかに関係なく成功します。

## <a name="create-containers-at-startup"></a>起動時にコンテナーを作成する

一般的な方法では、必要なコンテナーが事前にわかっている場合でもそれらのコードはアプリケーションによってコード内で作成されます。 `CloudBlobContainer` 上で `CreateIfNotExistsAsync` を呼び出すのが最善の方法であり、それを使用して、使用する必要があることがわかっている各コンテナーをあらかじめ作成しておく必要があります。

`CreateIfNotExistsAsync` で、Azure Storage へのネットワーク呼び出しを "*行います*"。 コンテナーにアクセスするたびにそれを呼び出すのではなく、起動時に一度それを呼び出すのがベスト プラクティスです。

## <a name="exercise"></a>演習

### <a name="clone-and-explore-the-unfinished-app"></a>未完了のアプリを複製して詳細を確認する

まず、GitHub からスターター アプリを複製してみましょう。 Cloud Shell ターミナルで、次のコマンドを実行してソース コードのコピーを取得し、それをエディターで開きます。

```console
git clone TODO
cd TODO
code .
```

ファイル `Controllers/FilesController.cs`を開きます。

このコントローラーでは、次の 3 つのアクションによって API が実装されます。

* **Index** (GET/api/Files) では、URL のリストが返されます。アップロードされた各ファイルに対する URL です。 アップロードされたファイルへのハイパーリンクのリストを作成するこのメソッドがアプリ フロント エンドによって呼び出されます。
* **Upload** (POST/api/Files) では、アップロードされたファイルが受信され、保存されます。
* **Download** (GET /api/Files/{file-name}) では、名前を指定した個々のファイルがダウンロードされます。

各メソッドでは、該当する処理を実行するために `storage` と呼ばれる `IStorage` インスタンスが使用されます。 `Models/BlobStorage.cs` には、`IStorage` の不完全な実装があります。

### <a name="add-the-nuget-package"></a>NuGet パッケージを追加する

最初に、Azure Storage SDK への参照を追加します。 ターミナルで、次の内容を実行します。

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

これにより、確実に BLOB ストレージ クライアント ライブラリの最新バージョンを使用することになります。

### <a name="configure"></a>構成

対象のスターター アプリには、必要としている構成プラミングが既に含まれています。 `BlobStorage` 内の `IOptions<AzureStorageConfig>` コンストラクター パラメーターには 2 つのプロパティがあります。ストレージ アカウントの接続文字列と、アプリによって BLOB が格納されるコンテナーの名前です。 `Startup.cs` の `ConfigureServices` メソッド内には、アプリの起動時に構成から値を読み込むコードがあります。

この演習では、Azure App Service 内でアプリを実行し、それで App Service アプリケーション設定に構成値を後で追加します。 ここでは、構成に関連する作業を行う必要はありません。

### <a name="initialize"></a>初期化

`BlobStorage.cs`を開きます。

`Initialize` メソッドを検索します。 このメソッドは、アプリが初めて使用されたときに、アプリによって呼び出されます。 関心がある場合は、`Startup.cs` 内にある `ConfigureServices` を調べて、それがどのように行われるかを確認することができます。 

コンテナーがまだ存在していない場合は、`Initialize` でコンテナーを作成します。 `Initialize` と次のコードを入力し、作業内容を保存します。

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```