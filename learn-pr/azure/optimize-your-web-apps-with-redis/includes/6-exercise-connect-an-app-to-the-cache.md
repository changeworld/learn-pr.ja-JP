Azure で Redis Cache を作成したので、それを使用するアプリケーションを作成しましょう。 Azure portal から取得した接続文字列情報があることを確認します。

> [!NOTE]
> 統合された Cloud Shell は右側で利用できます。 そのコマンド プロンプトを使用して、ここでビルドするサンプル コードを作成し、実行できます。また、.NET Core 開発環境をセットアップしている場合は、以下の手順をローカルで実行できます。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成する

Redis 実装に集中できるように、簡単なコンソール アプリケーションを使用します。

1. Cloud Shell で、新しい .NET Core コンソール アプリケーションを作成し、"SportsStatsTracker" という名前を付けます。

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. これにより、プロジェクトのフォルダーが作成され、現在のディレクトリが変更されます。

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a>接続文字列を追加する

Azure portal から取得した接続文字列をコードに追加しましょう。 このような資格情報は、ソース コードに保存しないでください。 このサンプルをシンプルにするために、構成ファイルを使用します。 Azure のサーバー側アプリケーションに適した方法は、Azure Key Vault と証明書を使用することです。

1. 新しい **appsettings.json** ファイルを作成してプロジェクトに追加します。

    ```bash
    touch appsettings.json
    ```

1. プロジェクト フォルダーで「`code .`」と入力してコード エディターを開きます。 ローカルで作業している場合は、**Visual Studio Code** を使用することをお勧めします。 ほとんどの場合、ここでの手順は Visual Studio Code でも使用できます。

1. エディターで **appsettings.json** ファイルを選択し、次のテキストを追加します。 接続文字列を設定の**値**に貼り付けます。

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. 変更を保存します。

    > [!IMPORTANT]
    > エディターでファイルにコードを貼り付けたり、コードを変更したりした場合は、その後に必ず [...] メニューまたはアクセス キー (Windows と Linux では <kbd>Ctrl + S</kbd>、macOS では <kbd>Cmd + S</kbd>) を使用して保存してください。

1. エディターで **SportsStatsTracker.csproj** ファイルをクリックして開きます。

1. プロジェクトに新しいファイルを組み入れるために、次の `<ItemGroup>` 構成ブロックをルートの `<Project>` 要素に追加し、それを出力フォルダーにコピーします。 これにより、アプリをコンパイル/ビルドしたときに、アプリ構成ファイルが確実に出力ディレクトリに配置されます。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. ファイルを保存します  (これを必ず行ってください。そうしないと、下記のパッケージを追加したときに変更内容が失われます)。

## <a name="add-support-to-read-a-json-configuration-file"></a>JSON 構成ファイルを読み取るためのサポートを追加する

.NET Core アプリケーションには、JSON 構成ファイルを読み取るための追加の NuGet パッケージが必要です。

1. ウィンドウのコマンド プロンプト セクションで、**Microsoft.Extensions.Configuration.Json** NuGet パッケージへの参照を追加します。

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>構成ファイルを読み取るコードを追加する

構成の読み取りを有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。

1. エディターで **Program.cs** を選択します。

1. ファイルの先頭に `using System` 行があります。 その行の下に次のコード行を追加します。

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. **Main** メソッドの内容を次のコードに置き換えます。 このコードにより、**appsettings.json** ファイルから読み取る構成システムが初期化されます。

    ```csharp
    var config = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json")
        .Build();
    ```

**Program.cs** ファイルは次のようになります。

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace SportsStatsTracker
{
    class Program
    {
        static void Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();
        }
    }
}
```

## <a name="get-the-connection-string-from-configuration"></a>構成から接続文字列を取得する

1. **Program.cs** の **Main** メソッドの末尾で、新しい **config** 変数を使用して接続文字列を取得し、**connectionString** という名前の新しい変数に格納します。
    - **config** 変数には、**appSettings.json** ファイルから取得する文字列を渡すことができるインデクサーが含まれています。

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a>Redis Cache .NET クライアントのサポートを追加する

次に、.NET 用の **StackExchange.Redis** クライアントを使用するようにコンソール アプリケーションを構成しましょう。

1. Cloud Shell エディターの下部にあるコマンド プロンプトを使用して、**StackExchange.Redis** NuGet パッケージをプロジェクトに追加します。

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. エディターで **Program.cs** を選択し、**StackExchange.Redis** 名前空間の `using` を追加します。

    ```csharp
    using StackExchange.Redis;
    ```
    
インストールが完了すると、Redis Cache クライアントをプロジェクトで使用できるようになります。

## <a name="connect-to-the-cache"></a>キャッシュに接続する

キャッシュに接続するコードを追加しましょう。

1. エディターで **Program.cs** を選択します。

1. `ConnectionMultiplexer.Connect` を使用し、これに接続文字列を渡して `ConnectionMultiplexer` を作成します。 戻り値に **cache** という名前を付けます。

1. 作成された接続は "_破棄可能_" であるため、`using` ブロックにラップします。 コードは次のような内容になります。

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> Azure Redis Cache への接続は、`ConnectionMultiplexer` クラスによって管理されます。 このクラスは、クライアント アプリケーション全体で共有して再利用する必要があります。 操作ごとに新しい接続を作成するのは望ましく_ありません_。 代わりに、接続をクラスのフィールドとして保存し、各操作で再利用します。 ここでは、**Main** メソッドでのみ使用しますが、実稼働アプリケーションでは、クラス フィールド (シングルトン) に格納する必要があります。

## <a name="add-a-value-to-the-cache"></a>キャッシュに値を追加する

接続が作成されたので、キャッシュに値を追加しましょう。

1. 接続が作成された後の `using` ブロック内で、`GetDatabase` メソッドを使用して `IDatabase` インスタンスを取得します。

1. `IDatabase` オブジェクトに対して `StringSet` を呼び出して、キー "test:key" を値 "some value" に設定します。
    - `StringSet` からの戻り値は、キーが追加されたかどうかを示す `bool` です。

1. `StringSet` からの戻り値をコンソールに表示します。

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a>キャッシュから値を取得する

1. 次に、`StringGet` を使用して値を取得します。 これはキーを取得して値を返します。

1. 戻り値を出力します。

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. コードは次のようになります。

    ```csharp
    using System;
    using Microsoft.Extensions.Configuration;
    using System.IO;
    using StackExchange.Redis;
    
    namespace SportsStatsTracker
    {
        class Program
        {
            static void Main(string[] args)
            {
                var config = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile("appsettings.json")
                    .Build();
    
                string connectionString = config["CacheConnection"];
    
                using (var cache = ConnectionMultiplexer.Connect(connectionString))
                {
                    var db = cache.GetDatabase();
    
                    bool setValue = db.StringSet("test:key", "some value");
                    Console.WriteLine($"SET: {setValue}");
    
                    string getValue = db.StringGet("test:key");
                    Console.WriteLine($"GET: {getValue}");
                }
            }
        }
    }
    ```
    
1. アプリケーションを実行して結果を確認します。 エディターの下のターミナル ウィンドウに「`dotnet run`」と入力します。 必ずプロジェクト フォルダーに移動してください。そうしないと、ビルドして実行するコードが見つかりません。
    
    ```bash
    dotnet run
    ```
    
> [!TIP] 
> プログラムが期待どおりに動作しないのに、コンパイルされる場合、エディターで変更を保存しなかったことが原因の可能性があります。 ターミナルとエディター ウィンドウを切り替える際に、常に変更を保存することを忘れないでください。 

## <a name="use-the-async-versions-of-the-methods"></a>非同期バージョンのメソッドを使用する

キャッシュから値を取得および設定できましたが、古い同期バージョンを使用しています。 サーバー側アプリケーションでは、同期バージョンを使用すると、スレッドを効率的に使用できません。 代わりに、メソッドの_非同期_バージョンを使用します。 非同期バージョンはすべて末尾が **Async** であるため、簡単に見分けることができます。

これらのメソッドを操作しやすくするために、C# の `async` および `await` キーワードを使用できます。 ただし、これらのキーワードを **Main** メソッドに適用するには、C# 7.1 "_以上_" を使用する必要があります。

### <a name="switch-to-c-71"></a>C# 7.1 に切り替える

C# 7.1 より前では、C# の `async` および `await` キーワードは **Main** メソッドで有効なキーワードではありませんでした。 **.csproj** ファイルでフラグを使用することで、そのコンパイラに簡単に切り替えることができます。

1. エディターで **SportsStatsTracker.csproj** ファイルを開きます。

1. ビルド ファイル内の最初の `PropertyGroup` に `<LangVersion>7.1</LangVersion>` を追加します。 完了すると、次のようになります。
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>async キーワードを適用する

次に、`async` キーワードを **Main** メソッドに適用します。 次の 3 つのことを行う必要があります。

1. `async` キーワードを **Main** メソッド シグネチャに追加します。
1. 戻り値の型を `void` から `Task` に変更します。
1. `System.Threading.Tasks` を含めるための `using` ステートメントを追加します。

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
        ...
```

### <a name="get-and-set-values-asynchronously"></a>値を非同期に取得および設定する

同期メソッドをそのままにして、`StringSetAsync` メソッドと `StringGetAsync` メソッドの呼び出しを追加し、別の値をキャッシュに追加します。 "counter" の値を "100" に設定します。  

1. `StringSetAsync` メソッドと `StringGetAsync` メソッドを使用して、"counter" という名前のキーを設定および取得します。 値を "100" に設定します。

1. `await` キーワードを適用して、各メソッドから結果を取得します。

1. 同期バージョンで行ったように、コンソール ウィンドウに結果を出力します。

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. アプリケーションをもう一度実行します。アプリケーションは引き続き動作し、2 つの値を取得します。

#### <a name="increment-the-value"></a>値を増分する

1. `StringIncrementAsync` メソッドを使用して **counter** 値を増分します。 カウンターに追加する数値 **50** を渡します。
    - このメソッドは、キー_と_ `long` または `double` を取得することに注意してください。
    - 渡されたパラメーターに応じて、`long` または `double` が返されます。

1. メソッドの結果をコンソールに出力します。

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a>その他の操作

最後に、`ExecuteAsync` をサポートするその他のメソッドをいくつか実行してみましょう。

1. サーバー接続をテストするには、"PING" を実行します。 このメソッドは "PONG" で応答します。
1. データベース値をクリアするには、"FLUSHDB" を実行します。 このメソッドは "OK" で応答します。

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

最終コードは次のような内容になります。

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;
using StackExchange.Redis;
using System.Threading.Tasks;

namespace SportsStatsTracker
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var config = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json")
                .Build();

            string connectionString = config["CacheConnection"];

            using (var cache = ConnectionMultiplexer.Connect(connectionString))
            {
                var db = cache.GetDatabase();

                bool setValue = db.StringSet("test:key", "some value");
                Console.WriteLine($"SET: {setValue}");

                string getValue = db.StringGet("test:key");
                Console.WriteLine($"GET: {getValue}");

                setValue = await db.StringSetAsync("test", "100");
                Console.WriteLine($"SET: {setValue}");

                getValue = await db.StringGetAsync("test");
                Console.WriteLine($"GET: {getValue}");

                var result = await db.ExecuteAsync("ping");
                Console.WriteLine($"PING = {result.Type} : {result}");
                
                result = await db.ExecuteAsync("flushdb");
                Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
            }
        }
    }
}
```

## <a name="challenge"></a>チャレンジ

チャレンジとして、オブジェクト型をシリアル化してキャッシュに格納してみてください。 基本的な手順は次のとおりです。

1. いくつかのパブリック プロパティを持つ新しい `class` を作成します。 独自のクラス ("Person" や "Car" が一般的) を作成することも、前のユニットで提供された "GameStats" の例を使用することもできます。

1. `dotnet add package` を使用して、**Newtonsoft.Json** NuGet パッケージのサポートを追加します。

1. `Newtonsoft.Json` 名前空間の `using` を追加します。

1. オブジェクトのいずれかを作成します。

1. `JsonConvert.SerializeObject` を使用してオブジェクトをシリアル化し、`StringSetAsync` を使用してキャッシュにプッシュします。

1. `StringGetAsync` を使用してオブジェクトをキャッシュから取り出し、`JsonConvert.DeserializeObject<T>` を使用して逆シリアル化します。

