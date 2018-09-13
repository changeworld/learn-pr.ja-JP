<span data-ttu-id="5f642-101">Azure Blob Storage を使用するアプリの一般的なワークフローを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5f642-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="5f642-102">**構成を取得する**: 起動時に、ストレージ アカウントの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="5f642-102">**Retrieve configuration**: At startup, load the storage account configuration.</span></span> <span data-ttu-id="5f642-103">これは通常、ストレージ アカウント接続文字列です。</span><span class="sxs-lookup"><span data-stu-id="5f642-103">This is typically a storage account connection string.</span></span>

1. <span data-ttu-id="5f642-104">**クライアントを初期化する**: 接続文字列を使用して、Azure Storage クライアント ライブラリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="5f642-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="5f642-105">これにより、BLOB ストレージ API を操作する場合にアプリが使用するオブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5f642-105">This creates the objects the app will use to work with the Blob storage API.</span></span>

1. <span data-ttu-id="5f642-106">**使用する**: コンテナーおよび BLOB に対して動作するクライアント ライブラリを指定して API 呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="5f642-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="5f642-107">接続文字列の構成</span><span class="sxs-lookup"><span data-stu-id="5f642-107">Configure your connection string</span></span>

<span data-ttu-id="5f642-108">コードを記述する前に、使用するストレージ アカウント用の接続文字列が必要になります。</span><span class="sxs-lookup"><span data-stu-id="5f642-108">Before writing any code, you'll need the connection string for the storage account you will use.</span></span>

<span data-ttu-id="5f642-109">ストレージ アカウント接続文字列には、アカウント キーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5f642-109">Storage account connection strings include the account key.</span></span> <span data-ttu-id="5f642-110">アカウント キーはシークレットとみなし、安全に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5f642-110">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="5f642-111">ここで、App Service アプリケーション設定内に接続文字列を格納することにします。</span><span class="sxs-lookup"><span data-stu-id="5f642-111">Here, we will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="5f642-112">App Service アプリケーション設定は、アプリケーション シークレットにとって安全な場所ですが、この設計ではローカル開発をサポートしていないため、単独では堅牢なエンドツーエンド ソリューションとはなりません。</span><span class="sxs-lookup"><span data-stu-id="5f642-112">App Service application settings are a secure place for application secrets, but this design does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="5f642-113">**コード内または保護されていない構成ファイル内には、ストレージ アカウント キーを置かないでください。**</span><span class="sxs-lookup"><span data-stu-id="5f642-113">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="5f642-114">ストレージ アカウント キーにより、ご利用のストレージ アカウントへのフル アクセスを有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="5f642-114">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="5f642-115">キーが漏えいすると、回復不能な損害が発生し、高額の請求を招く恐れがあります。</span><span class="sxs-lookup"><span data-stu-id="5f642-115">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="5f642-116">ストレージのガイダンスおよび漏えいしたキーを回収する方法については、このモジュールの最後に示す「参考資料」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="5f642-116">See the Further Reading section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="5f642-117">BLOB ストレージ オブジェクト モデルを初期化する</span><span class="sxs-lookup"><span data-stu-id="5f642-117">Initialize the Blob storage object model</span></span>

<span data-ttu-id="5f642-118">Azure Storage SDK for .NET Core において、BLOB ストレージを使用するための標準的なパターンは、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="5f642-118">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="5f642-119">接続文字列を使用して `CloudStorageAccount.Parse` (または `TryParse`) を呼び出して、`CloudStorageAccount` を取得します。</span><span class="sxs-lookup"><span data-stu-id="5f642-119">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>

1. <span data-ttu-id="5f642-120">`CloudStorageAccount` 上で `CreateCloudBlobClient` を呼び出して `CloudBlobClient` を取得します。</span><span class="sxs-lookup"><span data-stu-id="5f642-120">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>

1. <span data-ttu-id="5f642-121">`CloudBlobClient` 上で `GetContainerReference` を呼び出して `CloudBlobContainer` を取得します。</span><span class="sxs-lookup"><span data-stu-id="5f642-121">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>

1. <span data-ttu-id="5f642-122">そのコンテナー上のメソッドを使用することで、BLOB の一覧を取得および/または個々の BLOB への参照を取得してデータのアップロードおよびダウンロードを行います。</span><span class="sxs-lookup"><span data-stu-id="5f642-122">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="5f642-123">コード内で、ステップ 1 からステップ 3 は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="5f642-123">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="5f642-124">この初期化コードで、ネットワーク経由の呼び出しが行われることはありません。</span><span class="sxs-lookup"><span data-stu-id="5f642-124">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="5f642-125">これは、間違った情報が原因で発生するいくつかの例外が後になってもスローされないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="5f642-125">This means that some exceptions that occur because of incorrect information won't be thrown until later.</span></span> <span data-ttu-id="5f642-126">たとえば、`CloudStorageAccount.Parse` への呼び出しでは、接続文字列が不正に書式設定されていると、すぐに例外がスローされますが、接続文字列の指すストレージ アカウントが存在しない場合には、例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="5f642-126">For example, the call to `CloudStorageAccount.Parse` will throw an exception immediately if the connection string is formatted incorrectly, but no exception will be thrown if the storage account that a connection string points to doesn't exist.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="5f642-127">起動時にコンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="5f642-127">Create containers at startup</span></span>

<span data-ttu-id="5f642-128">アプリケーションの起動時またはアプリケーションで初めてコンテナーを使用するときにコンテナーを作成するには、`CloudBlobContainer` で `CreateIfNotExistsAsync` を呼び出すのが最善の方法です。</span><span class="sxs-lookup"><span data-stu-id="5f642-128">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to create a container when your application starts or when it first tries to use it.</span></span>

<span data-ttu-id="5f642-129">コンテナーは既に存在するものの、Azure Storage へのネットワーク呼び出しを行った場合には、`CreateIfNotExistsAsync` で例外がスローされません。</span><span class="sxs-lookup"><span data-stu-id="5f642-129">`CreateIfNotExistsAsync` won't throw an exception if the container already exists, but it does make a network call to Azure Storage.</span></span> <span data-ttu-id="5f642-130">呼び出しは、コンテナーを使用するたびではなく、初期化中に 1 回行います。</span><span class="sxs-lookup"><span data-stu-id="5f642-130">Call it once during initialization, not every time you try to use a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="5f642-131">演習</span><span class="sxs-lookup"><span data-stu-id="5f642-131">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="5f642-132">未完了のアプリを複製して詳細を確認する</span><span class="sxs-lookup"><span data-stu-id="5f642-132">Clone and explore the unfinished app</span></span>

<span data-ttu-id="5f642-133">まず、GitHub からスターター アプリを複製してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5f642-133">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="5f642-134">Cloud Shell ターミナルで、次のコマンドを実行してソース コードのコピーを取得し、それをエディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="5f642-134">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

<span data-ttu-id="5f642-135">**TODO 最終的なリポジトリの URL を更新してください**</span><span class="sxs-lookup"><span data-stu-id="5f642-135">**TODO update to final repo URL**</span></span>

```console
git clone https://github.com/nickwalkmsft/FileUploader.git
cd FileUploader
code .
```

<span data-ttu-id="5f642-136">ファイル `Controllers/FilesController.cs` を開きます。</span><span class="sxs-lookup"><span data-stu-id="5f642-136">Open the file `Controllers/FilesController.cs`.</span></span> <span data-ttu-id="5f642-137">ここで作業することはありませんが、アプリで行われることを簡単に見ていきます。</span><span class="sxs-lookup"><span data-stu-id="5f642-137">There's no work to do here, but we're going to have a quick look at what the app does.</span></span>

<span data-ttu-id="5f642-138">このコントローラーでは、次の 3 つのアクションによって API が実装されます。</span><span class="sxs-lookup"><span data-stu-id="5f642-138">This controller implements an API with three actions:</span></span>

- <span data-ttu-id="5f642-139">**Index** (GET/api/Files) では、URL のリストが返されます。アップロードされた各ファイルに対する URL です。</span><span class="sxs-lookup"><span data-stu-id="5f642-139">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="5f642-140">アップロードされたファイルへのハイパーリンクのリストを作成するこのメソッドがアプリ フロント エンドによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5f642-140">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
- <span data-ttu-id="5f642-141">**Upload** (POST/api/Files) では、アップロードされたファイルが受信され、保存されます。</span><span class="sxs-lookup"><span data-stu-id="5f642-141">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
- <span data-ttu-id="5f642-142">**Download** (GET /api/Files/{filename}) では、名前を指定した個々のファイルがダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="5f642-142">**Download** (GET /api/Files/{filename}) downloads an individual file by its name.</span></span>

<span data-ttu-id="5f642-143">各メソッドでは、該当する処理を実行するために `storage` と呼ばれる `IStorage` インスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="5f642-143">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="5f642-144">`Models/BlobStorage.cs` には、`IStorage` の不完全な実装があり、それをこれから埋めていきます。</span><span class="sxs-lookup"><span data-stu-id="5f642-144">There is an incomplete implementation of `IStorage` in `Models/BlobStorage.cs` that we're going to fill in.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="5f642-145">NuGet パッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="5f642-145">Add the NuGet package</span></span>

<span data-ttu-id="5f642-146">最初に、Azure Storage SDK への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="5f642-146">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="5f642-147">ターミナルで、次の内容を実行します。</span><span class="sxs-lookup"><span data-stu-id="5f642-147">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="5f642-148">これにより、確実に BLOB ストレージ クライアント ライブラリの最新バージョンを使用することになります。</span><span class="sxs-lookup"><span data-stu-id="5f642-148">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="5f642-149">構成</span><span class="sxs-lookup"><span data-stu-id="5f642-149">Configure</span></span>

<span data-ttu-id="5f642-150">アプリを実行するために必要な構成値は、ストレージ アカウント接続文字列と、ファイルを格納するためにアプリによって使用されるコンテナーの名前です。</span><span class="sxs-lookup"><span data-stu-id="5f642-150">The configuration values we need to run the app are the storage account connection string and the name of the container the app will use to store files.</span></span> <span data-ttu-id="5f642-151">このユニットでは、Azure App Service でアプリを実行するだけなので、App Service のベスト プラクティスに従って、値を App Service アプリケーション設定に格納します。</span><span class="sxs-lookup"><span data-stu-id="5f642-151">In this unit, we're only going to run the app in Azure App Service, so we'll follow App Service best practice and store the values in App Service application settings.</span></span> <span data-ttu-id="5f642-152">これは、App Service インスタンスを作成するときに行うので、現時点では何もする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="5f642-152">We'll do that when we create the App Service instance, so there's nothing we need to do at the moment.</span></span>

<span data-ttu-id="5f642-153">構成の*使用*に関しては、対象のスターター アプリに必要としているプラミングが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="5f642-153">When it comes to *using* the configuration, our starter app already includes the plumbing we need.</span></span> <span data-ttu-id="5f642-154">`BlobStorage` 内の `IOptions<AzureStorageConfig>` コンストラクター パラメーターには 2 つのプロパティがあります。ストレージ アカウントの接続文字列と、アプリによって BLOB が格納されるコンテナーの名前です。</span><span class="sxs-lookup"><span data-stu-id="5f642-154">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="5f642-155">`Startup.cs` の `ConfigureServices` メソッド内には、アプリの起動時に構成から値を読み込むコードがあります。</span><span class="sxs-lookup"><span data-stu-id="5f642-155">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

### <a name="initialize"></a><span data-ttu-id="5f642-156">初期化</span><span class="sxs-lookup"><span data-stu-id="5f642-156">Initialize</span></span>

<span data-ttu-id="5f642-157">`Models/BlobStorage.cs` を開きます。</span><span class="sxs-lookup"><span data-stu-id="5f642-157">Open `Models/BlobStorage.cs`.</span></span> <span data-ttu-id="5f642-158">ファイルの先頭に次の `using` ステートメントを追加して、演習中に追加するコード用に準備します。</span><span class="sxs-lookup"><span data-stu-id="5f642-158">Add the following `using` statements to the top of the file to prepare it for the code you're going to add during the exercise.</span></span>

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="5f642-159">`Initialize` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="5f642-159">Locate the `Initialize` method.</span></span> <span data-ttu-id="5f642-160">`BlobStorage` が初めて使用されるときに、ご利用のアプリによってこのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="5f642-160">Our app will call this method when `BlobStorage` is used for the first time.</span></span> <span data-ttu-id="5f642-161">関心がある場合は、`Startup.cs` 内にある `ConfigureServices` を調べて、それがどのように行われるかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="5f642-161">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span>

<span data-ttu-id="5f642-162">コンテナーがまだ存在していない場合は、`Initialize` でコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="5f642-162">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="5f642-163">`Initialize` の現在の実装を次のコードで置き換えて、作業内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="5f642-163">Replace the current implementation of `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```