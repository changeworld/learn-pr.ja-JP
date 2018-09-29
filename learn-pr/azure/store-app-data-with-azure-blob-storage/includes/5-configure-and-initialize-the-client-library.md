<span data-ttu-id="f709e-101">Azure Blob Storage を使用するアプリの一般的なワークフローを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f709e-101">The following is the typical workflow for apps that use Azure Blob storage:</span></span>

1. <span data-ttu-id="f709e-102">**構成を取得する**: 起動時に、ストレージ アカウントの構成を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="f709e-102">**Retrieve configuration**: At startup, load the storage account configuration.</span></span> <span data-ttu-id="f709e-103">これは通常、ストレージ アカウント接続文字列です。</span><span class="sxs-lookup"><span data-stu-id="f709e-103">This is typically a storage account connection string.</span></span>

1. <span data-ttu-id="f709e-104">**クライアントを初期化する**: 接続文字列を使用して、Azure Storage クライアント ライブラリを初期化します。</span><span class="sxs-lookup"><span data-stu-id="f709e-104">**Initialize client**: Use the connection string to initialize the Azure Storage client library.</span></span> <span data-ttu-id="f709e-105">これにより、BLOB ストレージ API を操作する場合にアプリが使用するオブジェクトが作成されます。</span><span class="sxs-lookup"><span data-stu-id="f709e-105">This creates the objects the app will use to work with the Blob storage API.</span></span>

1. <span data-ttu-id="f709e-106">**使用する**: コンテナーおよび BLOB に対して動作するクライアント ライブラリを指定して API 呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="f709e-106">**Use**: Make API calls with the client library to operate on containers and blobs.</span></span>

## <a name="configure-your-connection-string"></a><span data-ttu-id="f709e-107">接続文字列の構成</span><span class="sxs-lookup"><span data-stu-id="f709e-107">Configure your connection string</span></span>

<span data-ttu-id="f709e-108">アプリを実行する前に、使用するストレージ アカウント用の接続文字列が必要になります。</span><span class="sxs-lookup"><span data-stu-id="f709e-108">Before running your app, you'll need the connection string for the storage account you will use.</span></span> <span data-ttu-id="f709e-109">Azure portal、Azure CLI、Azure PowerShell などの Azure 管理インターフェイスを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="f709e-109">You can use any Azure management interface to get it, including the Azure portal, the Azure CLI or Azure PowerShell.</span></span> <span data-ttu-id="f709e-110">このモジュールの最後近くにあるコードを実行するように Web アプリを設定するときは、Azure CLI を使用して、前に作成したストレージ アカウントに対する接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="f709e-110">When we set up the web app to run our code near the end of this module, we'll use the Azure CLI to get the connection string for the storage account you created earlier.</span></span>

<span data-ttu-id="f709e-111">ストレージ アカウント接続文字列には、アカウント キーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="f709e-111">Storage account connection strings include the account key.</span></span> <span data-ttu-id="f709e-112">アカウント キーはシークレットとみなし、安全に格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f709e-112">The account key is considered a secret and should be stored securely.</span></span> <span data-ttu-id="f709e-113">ここで、App Service アプリケーション設定内に接続文字列を格納することにします。</span><span class="sxs-lookup"><span data-stu-id="f709e-113">Here, we will store the connection string in an App Service application setting.</span></span> <span data-ttu-id="f709e-114">App Service アプリケーション設定は、アプリケーション シークレットにとって安全な場所ですが、この設計ではローカル開発をサポートしていないため、単独では堅牢なエンドツーエンド ソリューションとはなりません。</span><span class="sxs-lookup"><span data-stu-id="f709e-114">App Service application settings are a secure place for application secrets, but this design does not support local development and is not a robust, end-to-end solution on its own.</span></span>

> [!WARNING]
> <span data-ttu-id="f709e-115">**コード内または保護されていない構成ファイル内には、ストレージ アカウント キーを置かないでください。**</span><span class="sxs-lookup"><span data-stu-id="f709e-115">**Do not place storage account keys in code or in unprotected configuration files.**</span></span> <span data-ttu-id="f709e-116">ストレージ アカウント キーにより、ご利用のストレージ アカウントへのフル アクセスを有効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="f709e-116">Storage account keys enable full access to your storage account.</span></span> <span data-ttu-id="f709e-117">キーが漏えいすると、回復不能な損害が発生し、高額の請求を招く恐れがあります。</span><span class="sxs-lookup"><span data-stu-id="f709e-117">Leaking a key can result in unrecoverable damage and large bills.</span></span> <span data-ttu-id="f709e-118">ストレージのガイダンスおよび漏えいしたキーを回収する方法については、このモジュールの最後に示す「参考資料」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f709e-118">See the Further Reading section at the end of this module for storage guidance and advice about how to recover from a leaked key.</span></span>

## <a name="initialize-the-blob-storage-object-model"></a><span data-ttu-id="f709e-119">BLOB ストレージ オブジェクト モデルを初期化する</span><span class="sxs-lookup"><span data-stu-id="f709e-119">Initialize the Blob storage object model</span></span>

<span data-ttu-id="f709e-120">Azure Storage SDK for .NET Core において、BLOB ストレージを使用するための標準的なパターンは、次の手順で構成されます。</span><span class="sxs-lookup"><span data-stu-id="f709e-120">In the Azure Storage SDK for .NET Core, the standard pattern for using Blob storage consists of the following steps:</span></span>

1. <span data-ttu-id="f709e-121">接続文字列を使用して `CloudStorageAccount.Parse` (または `TryParse`) を呼び出して、`CloudStorageAccount` を取得します。</span><span class="sxs-lookup"><span data-stu-id="f709e-121">Call `CloudStorageAccount.Parse` (or `TryParse`) with your connection string to get a `CloudStorageAccount`.</span></span>

1. <span data-ttu-id="f709e-122">`CloudStorageAccount` 上で `CreateCloudBlobClient` を呼び出して `CloudBlobClient` を取得します。</span><span class="sxs-lookup"><span data-stu-id="f709e-122">Call `CreateCloudBlobClient` on the `CloudStorageAccount` to get a `CloudBlobClient`.</span></span>

1. <span data-ttu-id="f709e-123">`CloudBlobClient` 上で `GetContainerReference` を呼び出して `CloudBlobContainer` を取得します。</span><span class="sxs-lookup"><span data-stu-id="f709e-123">Call `GetContainerReference` on the `CloudBlobClient` to get a `CloudBlobContainer`.</span></span>

1. <span data-ttu-id="f709e-124">そのコンテナー上のメソッドを使用することで、BLOB の一覧を取得および/または個々の BLOB への参照を取得してデータのアップロードおよびダウンロードを行います。</span><span class="sxs-lookup"><span data-stu-id="f709e-124">Use methods on the container to get a list of blobs and/or get references to individual blobs to upload and download data.</span></span>

<span data-ttu-id="f709e-125">コード内で、ステップ 1 からステップ 3 は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f709e-125">In code, steps 1&ndash;3 look like this:</span></span>

```csharp
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); // or TryParse()
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
CloudBlobContainer container = blobClient.GetContainerReference(containerName);
```

<span data-ttu-id="f709e-126">この初期化コードで、ネットワーク経由の呼び出しが行われることはありません。</span><span class="sxs-lookup"><span data-stu-id="f709e-126">None of this initialization code makes calls over the network.</span></span> <span data-ttu-id="f709e-127">これは、間違った情報が原因で発生するいくつかの例外が後になってもスローされないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="f709e-127">This means that some exceptions that occur because of incorrect information won't be thrown until later.</span></span> <span data-ttu-id="f709e-128">たとえば、`CloudStorageAccount.Parse` への呼び出しでは、接続文字列が不正に書式設定されていると、すぐに例外がスローされますが、接続文字列の指すストレージ アカウントが存在しない場合には、例外はスローされません。</span><span class="sxs-lookup"><span data-stu-id="f709e-128">For example, the call to `CloudStorageAccount.Parse` will throw an exception immediately if the connection string is formatted incorrectly, but no exception will be thrown if the storage account that a connection string points to doesn't exist.</span></span>

## <a name="create-containers-at-startup"></a><span data-ttu-id="f709e-129">起動時にコンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="f709e-129">Create containers at startup</span></span>

<span data-ttu-id="f709e-130">アプリケーションの起動時またはアプリケーションで初めてコンテナーを使用するときにコンテナーを作成するには、`CloudBlobContainer` で `CreateIfNotExistsAsync` を呼び出すのが最善の方法です。</span><span class="sxs-lookup"><span data-stu-id="f709e-130">Calling `CreateIfNotExistsAsync` on a `CloudBlobContainer` is the best way to create a container when your application starts or when it first tries to use it.</span></span>

<span data-ttu-id="f709e-131">コンテナーは既に存在するものの、Azure Storage へのネットワーク呼び出しを行った場合には、`CreateIfNotExistsAsync` で例外がスローされません。</span><span class="sxs-lookup"><span data-stu-id="f709e-131">`CreateIfNotExistsAsync` won't throw an exception if the container already exists, but it does make a network call to Azure Storage.</span></span> <span data-ttu-id="f709e-132">呼び出しは、コンテナーを使用するたびではなく、初期化中に 1 回行います。</span><span class="sxs-lookup"><span data-stu-id="f709e-132">Call it once during initialization, not every time you try to use a container.</span></span>

## <a name="exercise"></a><span data-ttu-id="f709e-133">演習</span><span class="sxs-lookup"><span data-stu-id="f709e-133">Exercise</span></span>

### <a name="clone-and-explore-the-unfinished-app"></a><span data-ttu-id="f709e-134">未完了のアプリを複製して詳細を確認する</span><span class="sxs-lookup"><span data-stu-id="f709e-134">Clone and explore the unfinished app</span></span>

<span data-ttu-id="f709e-135">まず、GitHub からスターター アプリを複製してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f709e-135">First, let's clone the starter app from GitHub.</span></span> <span data-ttu-id="f709e-136">Cloud Shell ターミナルで、次のコマンドを実行してソース コードのコピーを取得し、それをエディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="f709e-136">In the Cloud Shell terminal, run the following command to get a copy of the source code and open it in the editor:</span></span>

```console
git clone https://github.com/MicrosoftDocs/mslearn-store-data-in-azure.git
cd mslearn-store-data-in-azure/store-app-data-with-azure-blob-storage/src/start
code .
```

<span data-ttu-id="f709e-137">エディターで `Controllers/FilesController.cs` ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="f709e-137">Open the file `Controllers/FilesController.cs` in the editor.</span></span> <span data-ttu-id="f709e-138">ここで作業することはありませんが、アプリで行われることを簡単に見ていきます。</span><span class="sxs-lookup"><span data-stu-id="f709e-138">There's no work to do here, but we're going to have a quick look at what the app does.</span></span>

<span data-ttu-id="f709e-139">このコントローラーでは、次の 3 つのアクションによって API が実装されます。</span><span class="sxs-lookup"><span data-stu-id="f709e-139">This controller implements an API with three actions:</span></span>

- <span data-ttu-id="f709e-140">**Index** (GET/api/Files) では、URL のリストが返されます。アップロードされた各ファイルに対する URL です。</span><span class="sxs-lookup"><span data-stu-id="f709e-140">**Index** (GET /api/Files) returns a list of URLs, one for each file that's been uploaded.</span></span> <span data-ttu-id="f709e-141">アップロードされたファイルへのハイパーリンクのリストを作成するこのメソッドがアプリ フロント エンドによって呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f709e-141">The app front end calls this method to build a list of hyperlinks to the uploaded files.</span></span>
- <span data-ttu-id="f709e-142">**Upload** (POST/api/Files) では、アップロードされたファイルが受信され、保存されます。</span><span class="sxs-lookup"><span data-stu-id="f709e-142">**Upload** (POST /api/Files) receives an uploaded file and saves it.</span></span>
- <span data-ttu-id="f709e-143">**Download** (GET /api/Files/{filename}) では、名前を指定した個々のファイルがダウンロードされます。</span><span class="sxs-lookup"><span data-stu-id="f709e-143">**Download** (GET /api/Files/{filename}) downloads an individual file by its name.</span></span>

<span data-ttu-id="f709e-144">各メソッドでは、該当する処理を実行するために `storage` と呼ばれる `IStorage` インスタンスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f709e-144">Each method uses an `IStorage` instance called `storage` to do its work.</span></span> <span data-ttu-id="f709e-145">`Models/BlobStorage.cs` には、`IStorage` の不完全な実装があり、それをこれから埋めていきます。</span><span class="sxs-lookup"><span data-stu-id="f709e-145">There is an incomplete implementation of `IStorage` in `Models/BlobStorage.cs` that we're going to fill in.</span></span>

### <a name="add-the-nuget-package"></a><span data-ttu-id="f709e-146">NuGet パッケージを追加する</span><span class="sxs-lookup"><span data-stu-id="f709e-146">Add the NuGet package</span></span>

<span data-ttu-id="f709e-147">最初に、Azure Storage SDK への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="f709e-147">First, add a reference to the Azure Storage SDK.</span></span> <span data-ttu-id="f709e-148">ターミナルで、次の内容を実行します。</span><span class="sxs-lookup"><span data-stu-id="f709e-148">In the terminal, run the following:</span></span>

```console
dotnet add package WindowsAzure.Storage
dotnet restore
```

<span data-ttu-id="f709e-149">これにより、確実に BLOB ストレージ クライアント ライブラリの最新バージョンを使用することになります。</span><span class="sxs-lookup"><span data-stu-id="f709e-149">This will make sure we're using the newest version of the Blob storage client library.</span></span>

### <a name="configure"></a><span data-ttu-id="f709e-150">構成する</span><span class="sxs-lookup"><span data-stu-id="f709e-150">Configure</span></span>

<span data-ttu-id="f709e-151">必要な構成値は、ストレージ アカウントの接続文字列と、ファイルを格納するためにアプリによって使用されるコンテナーの名前です。</span><span class="sxs-lookup"><span data-stu-id="f709e-151">The configuration values we need are the storage account connection string and the name of the container the app will use to store files.</span></span> <span data-ttu-id="f709e-152">このモジュールでは、Azure App Service でアプリを実行するだけなので、App Service のベスト プラクティスに従い、値を App Service アプリケーションの設定に格納します。</span><span class="sxs-lookup"><span data-stu-id="f709e-152">In this module, we're only going to run the app in Azure App Service, so we'll follow App Service best practice and store the values in App Service application settings.</span></span> <span data-ttu-id="f709e-153">これは、App Service インスタンスを作成するときに行うので、現時点では何もする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="f709e-153">We'll do that when we create the App Service instance, so there's nothing we need to do at the moment.</span></span>

<span data-ttu-id="f709e-154">構成の*使用*に関しては、対象のスターター アプリに必要としているプラミングが既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="f709e-154">When it comes to *using* the configuration, our starter app already includes the plumbing we need.</span></span> <span data-ttu-id="f709e-155">`BlobStorage` 内の `IOptions<AzureStorageConfig>` コンストラクター パラメーターには 2 つのプロパティがあります。ストレージ アカウントの接続文字列と、アプリによって BLOB が格納されるコンテナーの名前です。</span><span class="sxs-lookup"><span data-stu-id="f709e-155">The `IOptions<AzureStorageConfig>` constructor parameter in `BlobStorage` has two properties: the storage account connection string and the name of the container our app will store blobs in.</span></span> <span data-ttu-id="f709e-156">`Startup.cs` の `ConfigureServices` メソッド内には、アプリの起動時に構成から値を読み込むコードがあります。</span><span class="sxs-lookup"><span data-stu-id="f709e-156">There is code in the `ConfigureServices` method of `Startup.cs` that loads the values from configuration when the app starts.</span></span>

### <a name="initialize"></a><span data-ttu-id="f709e-157">初期化する</span><span class="sxs-lookup"><span data-stu-id="f709e-157">Initialize</span></span>

<span data-ttu-id="f709e-158">エディターで `Models/BlobStorage.cs` を開きます。</span><span class="sxs-lookup"><span data-stu-id="f709e-158">Open `Models/BlobStorage.cs` in the editor.</span></span> <span data-ttu-id="f709e-159">ファイルの先頭に次の `using` ステートメントを追加して、演習中に追加するコード用に準備します。</span><span class="sxs-lookup"><span data-stu-id="f709e-159">Add the following `using` statements to the top of the file to prepare it for the code you're going to add during the exercise.</span></span>

```csharp
using System.Linq;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
```

<span data-ttu-id="f709e-160">`Initialize` メソッドを検索します。</span><span class="sxs-lookup"><span data-stu-id="f709e-160">Locate the `Initialize` method.</span></span> <span data-ttu-id="f709e-161">`BlobStorage` が初めて使用されるときに、ご利用のアプリによってこのメソッドが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="f709e-161">Our app will call this method when `BlobStorage` is used for the first time.</span></span> <span data-ttu-id="f709e-162">関心がある場合は、`Startup.cs` 内にある `ConfigureServices` を調べて、それがどのように行われるかを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="f709e-162">If you're curious, you can look at `ConfigureServices` in `Startup.cs` to see how this is done.</span></span>

<span data-ttu-id="f709e-163">コンテナーがまだ存在していない場合は、`Initialize` でコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="f709e-163">`Initialize` is where we want to create our container if it doesn't already exist.</span></span> <span data-ttu-id="f709e-164">`Initialize` の現在の実装を次のコードで置き換えて、作業内容を保存します。</span><span class="sxs-lookup"><span data-stu-id="f709e-164">Replace the current implementation of `Initialize` with the following code and save your work:</span></span>

```csharp
public Task Initialize()
{
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageConfig.ConnectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    CloudBlobContainer container = blobClient.GetContainerReference(storageConfig.FileContainerName);
    return container.CreateIfNotExistsAsync();
}
```