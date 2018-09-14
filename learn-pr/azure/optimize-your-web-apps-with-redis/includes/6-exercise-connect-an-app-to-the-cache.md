Azure で作成した Redis cache をしたら、それを使用するアプリケーションを作成しましょう。 Azure portal から接続文字列情報があることを確認します。

> [!NOTE]
> 統合された Cloud Shell は、右側に使用できます。 コマンド プロンプトを使用して作成し、ここでは、構築されたサンプル コードを実行したり、開発環境のセットアップがある場合にローカルでは、これらの手順を実行できます。 Cloud Shell を使用する場合は、選択を求められた場合に、Bash シェルに選択してください。

## <a name="create-a-console-application"></a>コンソール アプリケーションを作成します。

Redis の実装に専念できるように、単純なコンソール アプリケーションを使用します。

1. Cloud Shell では、新しい .NET Core コンソール アプリケーションを作成、"SportsStatsTracker"という名前を付けます

    ```bash
    dotnet new console --name SportsStatsTracker
    ```
    
1. プロジェクトのフォルダーを作成が進み、現在のディレクトリを変更され。

    ```bash
    cd SportsStatsTracker
    ```
    
## <a name="add-the-connection-string"></a>接続文字列を追加します。

コードに、Azure ポータルから先ほど接続文字列を追加してみましょう。 ソース コードでは、このような資格情報を保存しないでください。 このサンプルをシンプルにするには、構成ファイルを使用する快適います。 Azure でのサーバー側アプリケーションのより優れたアプローチは、Azure Key Vault を使用して、証明書を使用することです。

1. 新規作成**appsettings.json**プロジェクトに追加するファイル。

    ```bash
    touch appsettings.json
    ```

1. 」と入力して、コード エディターを開きます`code .`プロジェクト フォルダーにします。 ローカルで作業している場合を使用して勧め**Visual Studio Code**します。 ほとんどの場合、次の手順がの使用状況と整列します。

1. 選択、 **appsettings.json**エディターでファイルを開き、次のテキストを追加します。 接続文字列を貼り付け、**値**設定。

    ```json
    {
      "CacheConnection": "[value-goes-here]"
    }
    ```

1. メニューがファイルを保存、-オンライン エディターでは、ページの右上にあるが一般的なファイル操作であります。

1. プロジェクトに新しいファイルを含めるし、出力フォルダーにコピーする次の構成ブロックを追加します。 これにより、アプリがコンパイル/ビルドされたとき、構成ファイルが出力ディレクトリに確実に配置されます。

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

1. ファイルを保存します。 (これを行うか、以下のパッケージを追加すると、変更が失われますことを確認してください。)

## <a name="add-support-to-read-a-json-configuration-file"></a>JSON 構成ファイルを読み込むためのサポートを追加する

.NET Core アプリケーションでは、JSON 構成ファイルの読み取りに NuGet パッケージの追加が必要です。

1. ウィンドウの [コマンド プロンプト] セクションへの参照を追加、 **Microsoft.Extensions.Configuration.Json** NuGet パッケージ。

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## <a name="add-code-to-read-the-configuration-file"></a>構成ファイルを読み取るコードを追加します。

読み取り構成を有効にするために必要なライブラリが追加されたので、コンソール アプリケーション内でその機能を有効にする必要があります。

1. 選択**Program.cs**エディターでします。

1. ファイルの一番上に **using System;** 行があります。 その行の下に次のコード行を追加します。

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. 内容を置き換える、 **Main**メソッドを次のコード。 このコードにより、**appsettings.json** ファイルから読み込む構成システムが初期化されます。

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

## <a name="get-the-connection-string-from-configuration"></a>構成から接続文字列を取得します。

1. **Program.cs**の最後に、 **Main**メソッドを使用して、新しい**config**接続文字列を取得し、という名前の新しい変数に格納する変数**connectionString**します。
    - **Config**変数がインデクサーから取得する文字列で渡すことができます、 **appSettings.json**ファイル。

    ```csharp
    string connectionString = config["CacheConnection"];
    ```
    
## <a name="add-support-for-the-redis-cache-net-client"></a>Redis キャッシュの .NET クライアントのサポートを追加します。

次に、使用するコンソール アプリケーションを構成しましょう、 **StackExchange.Redis** .NET 用のクライアント。

1. 追加、 **StackExchange.Redis** Cloud Shell のエディターの下部にあるコマンド プロンプトを使用してプロジェクトに NuGet パッケージ。

    ```bash
    dotnet add package StackExchange.Redis
    ```

1. 選択**Program.cs**エディターで追加し、`using`名前空間の**StackExchange.Redis**

    ```csharp
    using StackExchange.Redis;
    ```
    
インストールが完了すると、Redis キャッシュ クライアントは、プロジェクトで使用できます。

## <a name="connect-to-the-cache"></a>キャッシュに接続する

キャッシュに接続するコードを追加してみましょう。

1. 選択**Program.cs**エディターでします。

1. 作成、`ConnectionMultiplexer`を使用して`ConnectionMultiplexer.Connect`接続文字列を渡すことによって。 返される値に名前を**キャッシュ**します。

1. 作成された接続があるため_破棄可能な_、ラップすることで、`using`ブロックします。 コードは次のような内容になります。

    ```csharp
    string connectionString = config["CacheConnection"];
    
    using (var cache = ConnectionMultiplexer.Connect(connectionString))
    {
        
    }
    ```

> [!NOTE] 
> Azure Redis Cache への接続は、によって管理される、`ConnectionMultiplexer`クラス。 このクラスは、クライアント アプリ全体で共有して再利用する必要があります。 私たち_いない_操作ごとに新しい接続を作成します。 代わりに、オフ、クラス内のフィールドとして格納し、各操作で再利用します。 ここでしかここでそれを使用して、 **Main**メソッドが、実稼働アプリケーションでクラス フィールド、またはシングルトンに格納する必要がありますが、します。

## <a name="add-a-value-to-the-cache"></a>値をキャッシュに追加します。

接続したら、値をキャッシュに追加してみましょう。

1. 内で、`using`ブロック接続を作成した後、使用、`GetDatabase`を取得するメソッド、`IDatabase`インスタンス。

1. 呼び出す`StringSet`上、 `IDatabase` 「いくつかの値」キー「テスト: キー」を値に設定するオブジェクト。
    - 戻り値`StringSet`は、`bool`キーが追加されたかどうかを示します。

1. 戻り値の表示`StringSet`コンソールにします。

    ```csharp
    bool setValue = db.StringSet("test:key", "some value");
    Console.WriteLine($"SET: {setValue}");
    ```
    
## <a name="get-a-value-from-the-cache"></a>キャッシュから値を取得します。

1. 使用して値を次に、取得`StringGet`します。 取得するキーは、この値を返します。

1. 返される値を出力します。

    ```csharp
    string getValue = db.StringGet("test:key");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. コードについては、次のようになります。

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
    
1. 結果を表示するアプリケーションを実行します。 型`dotnet run`エディターの下でターミナル ウィンドウにします。 必ずプロジェクト フォルダーにいるかをビルドして実行するようにコードを検索しません。
    
    ```bash
    dotnet run
    ```
    
## <a name="use-the-async-versions-of-the-methods"></a>メソッドの非同期バージョンを使用して、

取得およびキャッシュから値を設定することが、以前の同期バージョンを使用していること。 サーバー側アプリケーションでは、これらのない、スレッドを効率的に使用します。 使用する代わりに、_非同期_メソッドのバージョン。 簡単に見つけることができます - これらはすべて末尾**Async**します。

これらのメソッドを使用しやすくするに c# を使用、`async`と`await`キーワード。 使用する必要がありますが、_少なくとも_c# 7.1 をこれらのキーワードを適用できるように、 **Main**メソッド。

### <a name="switch-to-c-71"></a>C# 7.1 に切り替える

# の`async`と`await`キーワードは有効なキーワードでした。 **Main** c# 7.1 までメソッド。 そのコンパイラで、フラグを使って簡単に切り替えることができます、 **.csproj**ファイル。

1. 開く、 **SportsStatsTracker.csproj**エディターでファイル。

1. 追加`<LangVersion>7.1</LangVersion>`最初に`PropertyGroup`ビルド ファイルにします。 完了したら、次のようになりますする必要があります。
    
    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
    
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <LangVersion>7.1</LangVersion> 
      </PropertyGroup>
    ...
    ```
    
### <a name="apply-the-async-keyword"></a>Async キーワードを適用します。

次に、適用、`async`キーワードを**Main**メソッド。 次の 3 つの作業を行う必要があります。

1. 追加、`async`にキーワード、 **Main**メソッド シグネチャ。
1. 戻り値の型を変更する`void`に`Task`します。
1. 追加、`using`ステートメントに含める`System.Threading.Tasks`します。

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

### <a name="get-and-set-values-asynchronously"></a>取得および非同期的に値を設定します。

1. 使用して、`StringSetAsync`と`StringGetAsync`設定およびキーを取得するメソッドは、"counter"という名前です。 「100」に値を設定します。

1. 適用、`await`各メソッドから結果を取得するキーワード。

1. 同期バージョンと同様に、コンソール ウィンドウに結果を出力します。

    ```csharp
    // Simple get and put of integral data types into the cache
    setValue = await db.StringSetAsync("test", "100");
    Console.WriteLine($"SET: {setValue}");
    
    getValue = await db.StringGetAsync("test");
    Console.WriteLine($"GET: {getValue}");
    ```
    
1. -もう一度アプリケーションを実行する必要がありますも動作し、2 つの値があるようになりました。

#### <a name="increment-the-value"></a>増分値

1. 使用して、`StringIncrementAsync`をインクリメントするメソッド、**カウンター**値。 番号を渡して**50**カウンターを追加します。
    - メソッドが、キーを受け取ることに注意してください。_と_いずれかを`long`または`double`します。
    - 渡されたパラメーターに応じてどちらかを返します、`long`または`double`します。

1. コンソールにメソッドの結果を出力します。

    ```csharp
    long newValue = await db.StringIncrementAsync("counter", 50);
    Console.WriteLine($"INCR new value = {newValue}");
    ```
    
## <a name="other-operations"></a>その他の操作

最後に、試してみましょうでいくつかの追加メソッドの実行、`ExecuteAsync`をサポートします。

1. サーバー接続をテストするには、"PING"を実行します。 "PONG"で応答します。
1. "FLUSHDB"を実行するデータベースの値をクリアします。 "OK"で応答する必要があります。

```csharp
var result = await db.ExecuteAsync("ping");
Console.WriteLine($"PING = {result.Type} : {result}");

result = await db.ExecuteAsync("flushdb");
Console.WriteLine($"FLUSHDB = {result.Type} : {result}");
```

## <a name="challenge"></a>課題

チャレンジとしてキャッシュにオブジェクトの種類をシリアル化してみてください。 基本的な手順を次に示します。

1. 新規作成`class`いくつかのパブリック プロパティを使用します。 独自のいずれかの在庫 ("Person"または"Car"は人気のある)、または以前の単位で指定された例では、"GameStats"を使用します。
1. サポートを追加、 **Newtonsoft.Json** NuGet パッケージを使用して`dotnet add package`します。
1. 追加、`using`の`Newtonsoft.Json`名前空間。
1. オブジェクトのいずれかを作成します。
1. シリアル化`JsonConvert.SerializeObject`して`StringSetAsync`キャッシュにプッシュします。
1. キャッシュから戻る`StringGetAsync`でシリアル化し`JsonConvert.DeserializeObject<T>`します。

