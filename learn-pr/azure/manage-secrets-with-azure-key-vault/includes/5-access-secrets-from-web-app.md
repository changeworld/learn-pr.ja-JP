<span data-ttu-id="2d1fd-101">これまでに、MSI を有効にして、認証に使用できる自社アプリ用の ID を作成する方法を学習しました。次は、その ID を使用してコンテナー内のシークレットにアクセスするアプリを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-101">Now that you know how enabling MSI creates an identity for our app to use for authentication, we'll create an app that uses that identity to access secrets in the vault.</span></span>

## <a name="reading-secrets-in-an-aspnet-core-app"></a><span data-ttu-id="2d1fd-102">ASP.NET Core アプリでシークレットを読み取る</span><span class="sxs-lookup"><span data-stu-id="2d1fd-102">Reading secrets in an ASP.NET Core app</span></span>

<span data-ttu-id="2d1fd-103">Azure Key Vault API は、キーとコンテナーのすべての管理と使用を処理する REST API です。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-103">The Azure Key Vault API is a REST API that handles all management and usage of keys and vaults.</span></span> <span data-ttu-id="2d1fd-104">コンテナー内の各シークレットには一意の URL があり、シークレット値は HTTP GET 要求で取得されます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-104">Each secret in a vault has a unique URL, and secret values are retrieved with HTTP GET requests.</span></span>

<span data-ttu-id="2d1fd-105">.NET Core の公式の Key Vault クライアント ライブラリは、Microsoft.Azure.KeyVault NuGet パッケージの `KeyVaultClient` クラスです。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-105">The official Key Vault client library for .NET Core is the `KeyVaultClient` class in the Microsoft.Azure.KeyVault NuGet package.</span></span> <span data-ttu-id="2d1fd-106">これを直接使用する必要はありませんが、&mdash; と ASP.NET Core の `AddAzureKeyVault` メソッドを使用すると、起動時にコンテナーのすべてのシークレットを Configuration API に読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-106">You don't need to use it directly, though &mdash; with ASP.NET Core's `AddAzureKeyVault` method, you can load all the secrets from a vault into the Configuration API at startup.</span></span> <span data-ttu-id="2d1fd-107">この手法では、残りの構成で使用するものと同じ `IConfiguration` インターフェイスを使用して、すべてのシークレットに名前を指定してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-107">This technique enables you to access all of your secrets by name using the same `IConfiguration` interface you use for the rest of your configuration.</span></span> <span data-ttu-id="2d1fd-108">`AddAzureKeyVault` を使用するアプリには、コンテナーに対する **Get** と **List** の両方のアクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-108">Apps that use `AddAzureKeyVault` require both **Get** and **List** permissions to the vault.</span></span>

> [!TIP]
> <span data-ttu-id="2d1fd-109">特別な理由がない限り、アプリのビルドに使用するフレームワークや言語に関係なく、アプリの起動時にシークレット値をローカルにキャッシュするか、メモリに読み込んでください。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-109">Regardless of the framework or language you use to build your app, cache secret values locally or load them into memory at app startup unless you have a specific reason not to.</span></span> <span data-ttu-id="2d1fd-110">必要なときに毎回コンテナーから直接読み取ると、不必要に低速でコストも高くなります。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-110">Reading them directly from the vault every time you need them is unnecessarily slow and expensive.</span></span>

<span data-ttu-id="2d1fd-111">`AddAzureKeyVault` には入力としてコンテナー名のみが必要です。コンテナー名はローカル アプリの構成から取得します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-111">`AddAzureKeyVault` only requires the vault name as an input, which we'll get from our local app configuration.</span></span> <span data-ttu-id="2d1fd-112">また、MSI が有効な Azure App Service にデプロイされたアプリに使用すると、MSI 認証 &mdash; が自動的に処理され、MSI トークン サービスが検出され、認証に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-112">It also automatically handles MSI authentication &mdash; when used in an app deployed to Azure App Service with MSI enabled, it will detect the MSI token service and use it to authenticate.</span></span> <span data-ttu-id="2d1fd-113">これはほとんどのシナリオに適しており、すべてのベスト プラクティスが実装されているので、このユニットの演習に使用します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-113">It's a good fit for most scenarios and implements all best practices, and we'll use it in this unit's exercise.</span></span>

## <a name="handling-secrets-in-an-app"></a><span data-ttu-id="2d1fd-114">アプリでシークレットを処理する</span><span class="sxs-lookup"><span data-stu-id="2d1fd-114">Handling secrets in an app</span></span>

<span data-ttu-id="2d1fd-115">シークレットがアプリに読み込まれた後、シークレットを安全に処理する作業はアプリの担当です。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-115">Once a secret is loaded into your app, it's up to your app to handle it securely.</span></span> <span data-ttu-id="2d1fd-116">このモジュールで構築したアプリでは、クライアントの応答に応じてシークレットの値を書き出し、Web ブラウザーに表示して、読み込みが成功したことを示します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-116">In the app we build in this module, we write our secret value out to the client response and view it in a web browser to demonstrate that it has been loaded successfully.</span></span> <span data-ttu-id="2d1fd-117">**クライアントにシークレット値を返す処理は、通常の作業とは*異なります*。**</span><span class="sxs-lookup"><span data-stu-id="2d1fd-117">**Returning a secret value to the client is *not* something you'd normally do!**</span></span> <span data-ttu-id="2d1fd-118">通常は、シークレットを使用してデータベースやリモート API のクライアント ライブラリを初期化するなどの処理を行います。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-118">Usually, you'll use secrets to do things like initialize client libraries for databases or remote APIs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2d1fd-119">常に注意してコードを見直し、ログ、ストレージ、応答など、どのような種類の出力にもシークレット情報を書き込まないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-119">Always carefully review your code to ensure that your app never writes secrets to any kind of output, including logs, storage, and responses.</span></span>

## <a name="exercise"></a><span data-ttu-id="2d1fd-120">演習</span><span class="sxs-lookup"><span data-stu-id="2d1fd-120">Exercise</span></span>

<span data-ttu-id="2d1fd-121">新しい ASP.NET Core Web API を作成し、`AddAzureKeyVault` を使用してコンテナーからシークレットを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-121">We'll create a new ASP.NET Core web API and use `AddAzureKeyVault` to load the secret from our vault.</span></span>

### <a name="create-the-app"></a><span data-ttu-id="2d1fd-122">アプリケーションの作成</span><span class="sxs-lookup"><span data-stu-id="2d1fd-122">Create the app</span></span>

<span data-ttu-id="2d1fd-123">Azure Cloud Shell ターミナルで以下を実行して新しい ASP.NET Core Web API アプリケーションを作成し、エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-123">In the Azure Cloud Shell terminal, run the following to create a new ASP.NET Core web API application and open it in the editor.</span></span>

```console
dotnet new webapi -o KeyVaultDemoApp
cd KeyVaultDemoApp
code .
```

<span data-ttu-id="2d1fd-124">エディターの読み込みが完了したら、シェルで次のコマンドを実行して、`AddAzureKeyVault` を含む NuGet パッケージを追加し、すべてのアプリの依存関係を復元します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-124">After the editor loads, run the following commands in the shell to add the NuGet package containing `AddAzureKeyVault` and restore all of the app's dependencies.</span></span>

```console
dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault
dotnet restore
```

### <a name="add-code-to-load-and-use-secrets"></a><span data-ttu-id="2d1fd-125">シークレットを読み込んで使用するコードを追加する</span><span class="sxs-lookup"><span data-stu-id="2d1fd-125">Add code to load and use secrets</span></span>

<span data-ttu-id="2d1fd-126">Key Vault の適切な使用方法を示すため、起動時にコンテナーからシークレットを読み込むようにアプリを変更します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-126">To demonstrate good usage of Key Vault, we will modify our app to load secrets from the vault at startup.</span></span> <span data-ttu-id="2d1fd-127">また、エンドポイントに新しいコントローラーを追加して、**SecretPassword** シークレットをコンテナーから取得します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-127">We'll also add a new controller with an endpoint that gets our **SecretPassword** secret from the vault.</span></span>

<span data-ttu-id="2d1fd-128">最初にアプリのスタートアップ: `Program.cs` を開き、内容を削除して次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-128">First, the app startup: Open `Program.cs`, delete the contents and replace them with the following code:</span></span>

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((context, config) =>
                {
                    // Build the current set of configuration to load values from
                    // JSON files and environment variables, including VaultName.
                    var builtConfig = config.Build();

                    // Use VaultName from the configuration to create the full vault URL.
                    var vaultUrl = $"https://{builtConfig["VaultName"]}.vault.azure.net/";

                    // Load all secrets from the vault into configuration. This will automatically
                    // authenticate to the vault using MSI. If MSI is not available, it will
                    // check if Visual Studio and/or the Azure CLI are installed locally and
                    // see if they are configured with credentials that can access the vault.
                    config.AddAzureKeyVault(vaultUrl);
                })
                .UseStartup<Startup>();
    }
}
```

> [!TIP]
> <span data-ttu-id="2d1fd-129">編集が完了したら、必ず `Ctrl+S` でファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-129">Make sure to save files with `Ctrl+S` when you're done editing them.</span></span>

<span data-ttu-id="2d1fd-130">スターター コードからの唯一の変更は、`ConfigureAppConfiguration` の追加です。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-130">The only change from the starter code is the addition of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="2d1fd-131">ここでは、構成からコンテナー名を読み込み、それを使用して `AddAzureKeyVault` を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-131">This is where we load the vault name from configuration and call `AddAzureKeyVault` with it.</span></span>

<span data-ttu-id="2d1fd-132">次にコントローラー: `SecretTestController.cs` という `Controllers` フォルダーに新しいファイルを作成し、次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-132">Next, the controller: Create a new file in the `Controllers` folder called `SecretTestController.cs` and paste the following code into it.</span></span>

> [!TIP]
> <span data-ttu-id="2d1fd-133">新しいファイルを作成するには、シェルで `touch` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-133">To create a new file, use the `touch` command in the shell.</span></span> <span data-ttu-id="2d1fd-134">例では `touch Controllers/SecretTestController.cs`が使用されます。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-134">In this case, use `touch Controllers/SecretTestController.cs`.</span></span> <span data-ttu-id="2d1fd-135">表示するには、エディターの [ファイル] ウィンドウの [更新] ボタンをクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-135">You'll need to click the refresh button in the Files pane of the editor to see it there.</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;

namespace KeyVaultDemoApp.Controllers
{
    [Route("api/[controller]")]
    public class SecretTestController : ControllerBase
    {
        private readonly IConfiguration _configuration;

        public SecretTestController(IConfiguration configuration)
        {
            _configuration = configuration;
        }

        [HttpGet]
        public IActionResult Get()
        {
            // Get the secret value from configuration. This can be done anywhere
            // we have access to IConfiguration. This does not call the Key Vault
            // API, because the secrets were loaded at startup.
            var secretName = "SecretPassword";
            var secretValue = _configuration[secretName];

            if (secretValue == null)
            {
                return StatusCode(
                    StatusCodes.Status500InternalServerError,
                    $"Error: No secret named {secretName} was found...");
            }
            else {
                return Content($"Secret value: {secretValue}" +
                    Environment.NewLine + Environment.NewLine +
                    "This is for testing only! Never output a secret " +
                    "to a response or anywhere else in a real app!");
            }
        }
    }
}
```

<span data-ttu-id="2d1fd-136">シェルで `dotnet build` を実行して、すべてがコンパイルされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-136">Run `dotnet build` in the shell to make sure everything compiles.</span></span> <span data-ttu-id="2d1fd-137">アプリで &mdash; を実行できる準備が整いました。次は Azure に取り込みましょう。</span><span class="sxs-lookup"><span data-stu-id="2d1fd-137">The app is ready to run &mdash; now let's get it into Azure!</span></span>