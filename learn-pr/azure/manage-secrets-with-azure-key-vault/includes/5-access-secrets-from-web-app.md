これまでに、MSI を有効にして、認証に使用できる自社アプリ用の ID を作成する方法を学習しました。次は、その ID を使用してコンテナー内のシークレットにアクセスするアプリを作成してみましょう。

## <a name="reading-secrets-in-an-aspnet-core-app"></a>ASP.NET Core アプリでシークレットを読み取る

Azure Key Vault API は、キーとコンテナーのすべての管理と使用を処理する REST API です。 コンテナー内の各シークレットには一意の URL があり、シークレット値は HTTP GET 要求で取得されます。

.NET Core の公式の Key Vault クライアント ライブラリは、Microsoft.Azure.KeyVault NuGet パッケージの `KeyVaultClient` クラスです。 これを直接使用する必要はありませんが、&mdash; と ASP.NET Core の `AddAzureKeyVault` メソッドを使用すると、起動時にコンテナーのすべてのシークレットを Configuration API に読み込むことができます。 この手法では、残りの構成で使用するものと同じ `IConfiguration` インターフェイスを使用して、すべてのシークレットに名前を指定してアクセスできます。 `AddAzureKeyVault` を使用するアプリには、コンテナーに対する **Get** と **List** の両方のアクセス許可が必要です。

> [!TIP]
> 特別な理由がない限り、アプリのビルドに使用するフレームワークや言語に関係なく、アプリの起動時にシークレット値をローカルにキャッシュするか、メモリに読み込んでください。 必要なときに毎回コンテナーから直接読み取ると、不必要に低速でコストも高くなります。

`AddAzureKeyVault` には入力としてコンテナー名のみが必要です。コンテナー名はローカル アプリの構成から取得します。 また、MSI が有効な Azure App Service にデプロイされたアプリに使用すると、MSI 認証 &mdash; が自動的に処理され、MSI トークン サービスが検出され、認証に使用されます。 これはほとんどのシナリオに適しており、すべてのベスト プラクティスが実装されているので、このユニットの演習に使用します。

## <a name="handling-secrets-in-an-app"></a>アプリでシークレットを処理する

シークレットがアプリに読み込まれた後、シークレットを安全に処理する作業はアプリの担当です。 このモジュールで構築したアプリでは、クライアントの応答に応じてシークレットの値を書き出し、Web ブラウザーに表示して、読み込みが成功したことを示します。 **クライアントにシークレット値を返す処理は、通常の作業とは*異なります*。** 通常は、シークレットを使用してデータベースやリモート API のクライアント ライブラリを初期化するなどの処理を行います。

> [!IMPORTANT]
> 常に注意してコードを見直し、ログ、ストレージ、応答など、どのような種類の出力にもシークレット情報を書き込まないようにしてください。

## <a name="exercise"></a>演習

新しい ASP.NET Core Web API を作成し、`AddAzureKeyVault` を使用してコンテナーからシークレットを読み込みます。

### <a name="create-the-app"></a>アプリケーションの作成

Azure Cloud Shell ターミナルで以下を実行して新しい ASP.NET Core Web API アプリケーションを作成し、エディターで開きます。

```console
dotnet new webapi -o KeyVaultDemoApp
cd KeyVaultDemoApp
code .
```

エディターの読み込みが完了したら、シェルで次のコマンドを実行して、`AddAzureKeyVault` を含む NuGet パッケージを追加し、すべてのアプリの依存関係を復元します。

```console
dotnet add package Microsoft.Extensions.Configuration.AzureKeyVault
dotnet restore
```

### <a name="add-code-to-load-and-use-secrets"></a>シークレットを読み込んで使用するコードを追加する

Key Vault の適切な使用方法を示すため、起動時にコンテナーからシークレットを読み込むようにアプリを変更します。 また、エンドポイントに新しいコントローラーを追加して、**SecretPassword** シークレットをコンテナーから取得します。

最初にアプリのスタートアップ: `Program.cs` を開き、内容を削除して次のコードに置き換えます。

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

> [!NOTE]
> 編集が完了したら、必ず `Ctrl+S` でファイルを保存します。

スターター コードからの唯一の変更は、`ConfigureAppConfiguration` の追加です。 ここでは、構成からコンテナー名を読み込み、それを使用して `AddAzureKeyVault` を呼び出します。

次にコントローラー: `SecretTestController.cs` という `Controllers` フォルダーに新しいファイルを作成し、次のコードを貼り付けます。

> [!NOTE]
> 新しいファイルを作成するには、シェルで `touch` コマンドを使用します。 例では `touch Controllers/SecretTestController.cs`が使用されます。 表示するには、エディターの [ファイル] ウィンドウの [更新] ボタンをクリックする必要があります。

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

シェルで `dotnet build` を実行して、すべてがコンパイルされていることを確認します。 アプリで &mdash; を実行できる準備が整いました。次は Azure に取り込みましょう。