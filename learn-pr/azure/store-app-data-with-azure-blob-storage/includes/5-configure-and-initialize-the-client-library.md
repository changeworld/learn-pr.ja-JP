<span data-ttu-id="3933f-101">Azure Blob Storage を使用するアプリの一般的なワークフローを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3933f-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="3933f-102">**構成を取得する**: 起動時に、アカウント キーを含む接続文字列などの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3933f-102">**Retrieve configuration**: At startup, load the configuration, such as the connection string with the account key.</span></span> <span data-ttu-id="3933f-103">これは API 呼び出しを認証するのに必要です。</span><span class="sxs-lookup"><span data-stu-id="3933f-103">This is needed to authenticate API calls.</span></span>
1. <span data-ttu-id="3933f-104">**クライアントを初期化する**: 接続文字列を使用して、Azure Storage クライアント ライブラリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="3933f-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="3933f-105">これにより、BLOB ストレージ API を操作する場合にアプリが使用するオブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-105">This creates the objects the app will use to work with the Blob storage API.</span></span>
1. <span data-ttu-id="3933f-106">**使用する**: コンテナーおよび BLOB に対して動作するクライアント ライブラリを指定して API 呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="3933f-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="3933f-107">接続文字列の構成</span><span class="sxs-lookup"><span data-stu-id="3933f-107">Configure your connection string</span></span>

<span data-ttu-id="3933f-108">コードを記述する前に、使用するストレージ アカウント用の接続文字列が必要になります。</span><span class="sxs-lookup"><span data-stu-id="3933f-108">Before writing any code, you'll need the connection string for the storage account you will use.</span></span> 

<span data-ttu-id="3933f-109">接続文字列には、アカウント キーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3933f-109">The connection string includes your account key.</span></span> <span data-ttu-id="3933f-110">アカウント キーはシークレットとみなし、安全に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3933f-110">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="3933f-111">App Service アプリケーション設定内に接続文字列を格納することにします。</span><span class="sxs-lookup"><span data-stu-id="3933f-111">We will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="3933f-112">アプリケーション設定は、アプリケーション シークレットにとって安全な場所ですが、ローカル開発をサポートしていないため、単独では堅牢なエンド ツーエンド ソリューションとはなりません。</span><span class="sxs-lookup"><span data-stu-id="3933f-112">An application setting is a secure place for application secrets, but it does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="3933f-113">**コード内または保護されていない構成ファイル内には、ストレージ アカウント キーを置かないでください。**</span><span class="sxs-lookup"><span data-stu-id="3933f-113">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="3933f-114">ストレージ アカウント キーにより、ご利用のストレージ アカウントへのフル アクセスを有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="3933f-114">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="3933f-115">キーが漏洩すると、回復不能な損害が発生し、高額の請求を招く恐れがあります。</span><span class="sxs-lookup"><span data-stu-id="3933f-115">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="3933f-116">ストレージのガイダンスおよび漏洩したキーを回収する方法については、このモジュールの最後に示す「その他のリソース」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3933f-116">See the Additional Resources section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="3933f-117">BLOB ストレージ オブジェクト モデルを初期化する</span><span class="sxs-lookup"><span data-stu-id="3933f-117">Initialize the Blob storage object model</span></span>

<span data-ttu-id="3933f-118">Azure Storage SDK for .NET Core において、BLOB ストレージを使用するための標準的なパターンは、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-118">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="3933f-119">接続文字列を使用して `CloudStorageAccount.Parse` (または `TryParse`) を呼び出して、`CloudStorageAccount` を取得します。</span><span class="sxs-lookup"><span data-stu-id="3933f-119">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>
1. <span data-ttu-id="3933f-120">`CloudStorageAccount` 上で `CreateCloudBlobClient` を呼び出して `CloudBlobClient` を取得します。</span><span class="sxs-lookup"><span data-stu-id="3933f-120">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>
1. <span data-ttu-id="3933f-121">`CloudBlobClient` 上で `GetContainerReference` を呼び出して `CloudBlobContainer` を取得します。</span><span class="sxs-lookup"><span data-stu-id="3933f-121">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>
1. <span data-ttu-id="3933f-122">そのコンテナー上のメソッドを使用することで、BLOB の一覧を取得および/または個々の BLOB への参照を取得してデータのアップロードおよびダウンロードを行います。</span><span class="sxs-lookup"><span data-stu-id="3933f-122">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="3933f-123">コード内で、ステップ 1 からステップ 3 は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3933f-123">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="3933f-124">この初期化コードで、ネットワーク経由の呼び出しが行われることはありません。</span><span class="sxs-lookup"><span data-stu-id="3933f-124">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="3933f-125">これは、誤った情報により発生した例外をスローする処理が後回しにされることを意味します。たとえば、`GetContainerReference` の呼び出しは、アカウント内にコンテナーが実際に存在するかどうかに関係なく成功します。</span><span class="sxs-lookup"><span data-stu-id="3933f-125">This means exceptions that occur from incorrect information won't be thrown until later; for example, the call to `GetContainerReference` will succeed whether or not the container actually exists in the account.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="3933f-126">起動時にコンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="3933f-126">Create containers at startup</span></span>

<span data-ttu-id="3933f-127">一般的な方法では、必要なコンテナーが事前にわかっている場合でもそれらのコードはアプリケーションによってコード内で作成されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-127">Common practice is for applications to create needed containers in code, even when we know what those containers will be up-front.</span></span> <span data-ttu-id="3933f-128">`CloudBlobContainer` 上で `CreateIfNotExistsAsync` を呼び出すのが最善の方法であり、それを使用して、使用する必要があることがわかっている各コンテナーをあらかじめ作成しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="3933f-128">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to do this, and we should use it to create each container we know we'll need before we use them.</span></span>

<span data-ttu-id="3933f-129">`CreateIfNotExistsAsync` で、Azure Storage へのネットワーク呼び出しを "*行います*"。</span><span class="sxs-lookup"><span data-stu-id="3933f-129">`CreateIfNotExistsAsync` *does* make a network call to Azure Storage.</span></span> <span data-ttu-id="3933f-130">コンテナーにアクセスするたびにそれを呼び出すのではなく、起動時に一度それを呼び出すのがベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="3933f-130">Best practice is to call it once at startup and not every time we access a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="3933f-131">演習</span><span class="sxs-lookup"><span data-stu-id="3933f-131">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="3933f-132">未完了のアプリを複製して詳細を確認する</span><span class="sxs-lookup"><span data-stu-id="3933f-132">Clone and explore the unfinished app</span></span>

<span data-ttu-id="3933f-133">まず、GitHub からスターター アプリを複製してみましょう。</span><span class="sxs-lookup"><span data-stu-id="3933f-133">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="3933f-134">Cloud Shell ターミナルで、次のコマンドを実行してソース コードのコピーを取得し、それをエディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="3933f-134">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

```console
git clone TODO
cd TODO
code .
```

<span data-ttu-id="3933f-135">ファイル `Controllers/FilesController.cs`を開きます。</span><span class="sxs-lookup"><span data-stu-id="3933f-135">Open the file `Controllers/FilesController.cs`.</span></span>

<span data-ttu-id="3933f-136">このコントローラーでは、次の 3 つのアクションによって API が実装されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-136">This controller implements an API with three actions:</span></span>

* <span data-ttu-id="3933f-137">**Index** (GET/api/Files) では、URL のリストが返されます。アップロードされた各ファイルに対する URL です。</span><span class="sxs-lookup"><span data-stu-id="3933f-137">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="3933f-138">アップロードされたファイルへのハイパーリンクのリストを作成するこのメソッドがアプリ フロント エンドによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-138">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
* <span data-ttu-id="3933f-139">**Upload** (POST/api/Files) では、アップロードされたファイルが受信され、保存されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-139">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
* <span data-ttu-id="3933f-140">**Download** (GET /api/Files/{file-name}) では、名前を指定した個々のファイルがダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="3933f-140">**Download** (GET /api/Files/{file-name}) downloads an individual file by its name.</span></span>

<span data-ttu-id="3933f-141">各メソッドでは、該当する処理を実行するために `storage` と呼ばれる `IStorage` インスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-141">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="3933f-142">`Models/BlobStorage.cs` には、`IStorage` の不完全な実装があります。</span><span class="sxs-lookup"><span data-stu-id="3933f-142">There is an incomplete implementation of `IStorage` in  `Models/BlobStorage.cs`.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="3933f-143">NuGet パッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="3933f-143">Add the NuGet package</span></span>

<span data-ttu-id="3933f-144">最初に、Azure Storage SDK への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="3933f-144">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="3933f-145">ターミナルで、次の内容を実行します。</span><span class="sxs-lookup"><span data-stu-id="3933f-145">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="3933f-146">これにより、確実に BLOB ストレージ クライアント ライブラリの最新バージョンを使用することになります。</span><span class="sxs-lookup"><span data-stu-id="3933f-146">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="3933f-147">構成</span><span class="sxs-lookup"><span data-stu-id="3933f-147">Configure</span></span>

<span data-ttu-id="3933f-148">対象のスターター アプリには、必要としている構成プラミングが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="3933f-148">Our starter app already includes the configuration plumbing we need.</span></span> <span data-ttu-id="3933f-149">`BlobStorage` 内の `IOptions<AzureStorageConfig>` コンストラクター パラメーターには 2 つのプロパティがあります。ストレージ アカウントの接続文字列と、アプリによって BLOB が格納されるコンテナーの名前です。</span><span class="sxs-lookup"><span data-stu-id="3933f-149">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="3933f-150">`Startup.cs` の `ConfigureServices` メソッド内には、アプリの起動時に構成から値を読み込むコードがあります。</span><span class="sxs-lookup"><span data-stu-id="3933f-150">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

<span data-ttu-id="3933f-151">この演習では、Azure App Service 内でアプリを実行し、それで App Service アプリケーション設定に構成値を後で追加します。</span><span class="sxs-lookup"><span data-stu-id="3933f-151">In this exercise, we will run the app in Azure App Service, so we will add the configuration values to the App Service application settings later.</span></span> <span data-ttu-id="3933f-152">ここでは、構成に関連する作業を行う必要はありません。</span><span class="sxs-lookup"><span data-stu-id="3933f-152">For now, we don't need to do any work related to configuration.</span></span>

### <a name="initialize"></a><span data-ttu-id="3933f-153">初期化</span><span class="sxs-lookup"><span data-stu-id="3933f-153">Initialize</span></span>

<span data-ttu-id="3933f-154">`BlobStorage.cs`を開きます。</span><span class="sxs-lookup"><span data-stu-id="3933f-154">Open `BlobStorage.cs`.</span></span>

<span data-ttu-id="3933f-155">`Initialize` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="3933f-155">Location the `Initialize` method.</span></span> <span data-ttu-id="3933f-156">このメソッドは、アプリが初めて使用されたときに、アプリによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="3933f-156">Our app will call this method when it's first used.</span></span> <span data-ttu-id="3933f-157">関心がある場合は、`Startup.cs` 内にある `ConfigureServices` を調べて、それがどのように行われるかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="3933f-157">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span> 

<span data-ttu-id="3933f-158">コンテナーがまだ存在していない場合は、`Initialize` でコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3933f-158">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="3933f-159">`Initialize` と次のコードを入力し、作業内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="3933f-159">Fill in `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```