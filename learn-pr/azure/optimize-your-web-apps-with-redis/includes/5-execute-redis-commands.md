既に説明したように、Redis は複数のサーバー間でレプリケートできるインメモリ NoSQL データベースです。 多くの場合、キャッシュとして使用されますが、正式なデータベースまたはメッセージ ブローカーとして使用することもできます。 

さまざまなデータ型や構造体を格納することができ、キャッシュ データを取得したり、キャッシュ自体に関する情報を照会したりするために発行できる各種コマンドをサポートしています。 使用するデータは、常にキーと値のペアとして格納されます。

## <a name="executing-commands-on-the-redis-cache"></a>Redis Cache でコマンドを実行する

通常、クライアント アプリケーションは "_クライアント ライブラリ_" を使用して要求を作成し、Redis Cache でコマンドを実行します。 クライアント ライブラリの一覧は、[Redis クライアント ページ](https://redis.io/clients)から直接取得できます。 .NET 言語用の一般的な高性能 Redis クライアントは **StackExchange.Redis** です。 このパッケージは NuGet から入手でき、コマンド ラインまたは IDE を使用して .NET コードに追加できます。

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a>StackExchange.Redis を使用して Redis Cache に接続する

Redis サーバーに接続するには、ホスト アドレス、ポート番号、アクセス キーを使用します。 Azure では、一部の Redis クライアント用に、このデータを 1 つの文字列にまとめた_接続文字列_も提供されます。

### <a name="what-is-a-connection-string"></a>接続文字列とは

接続文字列とは、Azure の Redis Cache に接続して認証するために必要なすべての情報を含む 1 行のテキストです。 接続文字列は次のようになります (**cache-name** フィールドと **password-here** フィールドには実際の値を入力します)。

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> 接続文字列は、アプリケーション内で保護する必要があります。 アプリケーションが Azure でホストされている場合は、Azure Key Vault を使用して値を格納することを検討してください。

この文字列を **StackExchange.Redis** に渡して、サーバーへの接続を作成できます。 

末尾に次の 2 つの追加パラメーターがあることに注意してください。 

- **ssl** - 通信を暗号化できます。
- **abortConnection** - その時にサーバーが使用できなくても接続を作成できます。

クライアント ライブラリを構成するために文字列に追加できる他の[省略可能なパラメーター](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options)がいくつかあります。

### <a name="creating-a-connection"></a>接続を作成する

**StackExchange.Redis** の主な接続オブジェクトは `StackExchange.Redis.ConnectionMultiplexer` クラスです。 このオブジェクトは、Redis サーバー (またはサーバーのグループ) への接続プロセスを抽象化します。 接続を効率的に管理するように最適化されており、キャッシュにアクセスする必要がある間は保持されることを目的としています。

静的 `ConnectionMultiplexer.Connect` メソッドまたは `ConnectionMultiplexer.ConnectAsync` メソッドを使用して `ConnectionMultiplexer` インスタンスを作成し、接続文字列または `ConfigurationOptions` オブジェクトを渡します。 

簡単な例を次に示します。

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

`ConnectionMultiplexer` を作成したら、主に次の 3 つのことを行うことができます。

1. Redis データベースにアクセスする。 ここではこれについて説明します。
2. Redis のパブリッシャー/サブスクリプト機能を使用する。 これはこのモジュールの範囲外です。
3. メンテナンスまたは監視のために個々のサーバーにアクセスする。

### <a name="accessing-a-redis-database"></a>Redis データベースにアクセスする

Redis データベースは `IDatabase` 型で表されます。 `GetDatabase()` メソッドを使用してこれを取得できます。

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> `GetDatabase` から返されるオブジェクトはライトウェイト オブジェクトであり、保存する必要はありません。 `ConnectionMultiplexer` だけを保持する必要があります。

`IDatabase` オブジェクトを取得したら、キャッシュを操作するメソッドを実行できます。 すべてのメソッドには同期バージョンと非同期バージョンがあります。非同期バージョンは、`async` および `await` キーワードに対応できるようにするための `Task` オブジェクトを返します。

キーと値をキャッシュに格納する例を次に示します。

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

`StringSet` メソッドは、値が設定されているか (`true`)、設定されていないか (`false`) を示す `bool` を返します。 値は `StringGet` メソッドを使用して取得できます。

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a>バイナリ値を取得および設定する

Redis キーと値は "_バイナリ セーフ_" であることに注意してください。 これらの同じメソッドを使用して、バイナリ データを格納できます。 データを自然に操作できるように、`byte[]` 型を処理する暗黙的な変換演算子があります。

```csharp
byte[] key = ...;
byte[] value = ...;

db.StringSet(key, value);
```

```csharp
byte[] key = ...;
byte[] value = db.StringGet(key);
```

> [!TIP]
> **StackExchange.Redis** では、`RedisKey` 型を使用してキーを表します。 このクラスでは、`string` と `byte[]` の両方との間で暗黙的な変換が行われるので、複雑さを伴わずにテキスト キーとバイナリ キーの両方を使用できます。 値は `RedisValue` 型で表されます。 `RedisKey` と同様に、`string` または `byte[]` を渡すことができるように、暗黙的な変換が適切に行われます。

#### <a name="other-common-operations"></a>その他の一般的な操作

`IDatabase` インターフェイスには、Redis Cache で動作する他の複数のメソッドがあります。 ハッシュ、リスト、セット、順序付けされたセットを操作するメソッドがあります。

ここでは、単一キーで動作するより一般的なものを紹介します。完全なリストについては、このインターフェイスの[ソース コードを参照](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs)してください。

| メソッド | 説明 |
|--------|-------------|
| `CreateBatch` | 単一ユニットとしてサーバーに送信されるが、必ずしもユニットとして処理されるわけではない "_操作のグループ_" を作成します。 |
| `CreateTransaction` | 単一ユニットとしてサーバーに送信され、_なおかつ_サーバー上で単一ユニットとして処理される操作のグループを作成します。 |
| `KeyDelete` | キーと値を削除します。 |
| `KeyExists` | 特定のキーがキャッシュに存在するかどうかを返します。 |
| `KeyExpire` | キーの Time to Live (TTL) の有効期限を設定します。 |
| `KeyRename` | キーの名前を変更します。 |
| `KeyTimeToLive` | キーの TTL を返します。 |
| `KeyType` | キーに格納されている値の型の文字列表現を返します。 返される型は、文字列、list、set、zset、hash です。 |
       
### <a name="executing-other-commands"></a>他のコマンドを実行する

`IDatabase` オブジェクトには、テキスト コマンドを Redis サーバーに渡すときに使用できる `Execute` メソッドと `ExecuteAsync` メソッドがあります。 例:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

`Execute` メソッドと `ExecuteAsync` メソッドは、`RedisResult` オブジェクトを返します。これは、次の 2 つのプロパティを含むデータ ホルダーです。

- `Type` - 結果の型 ("STRING"、"INTEGER" など) を示す `string` を返します。
- `IsNull` - 結果が `null` かどうかを検出する true/false 値。

`RedisResult` で `ToString()` を使用して、実際の戻り値を取得できます。

`Execute` を使用して、サポートされているコマンドを実行できます。たとえば、キャッシュに接続されているすべてのクライアントを取得できます ("CLIENT LIST")。

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

この場合、接続されているすべてのクライアントが出力されます。

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a>より複雑な値を格納する
Redis はバイナリ セーフな文字列を中心としていますが、オブジェクト グラフをテキスト形式 (通常は XML または JSON) にシリアル化することでキャッシュすることができます。 たとえば、演習で使用する統計では、次のような `GameStats` オブジェクトがあります。

```csharp
public class GameStat
{
    public string Id { get; set; }
    public string Sport { get; set; }
    public DateTimeOffset DatePlayed { get; set; }
    public string Game { get; set; }
    public IReadOnlyList<string> Teams { get; set; }
    public IReadOnlyList<(string team, int score)> Results { get; set; }

    public GameStat(string sport, DateTimeOffset datePlayed, string game, string[] teams, IEnumerable<(string team, int score)> results)
    {
        Id = Guid.NewGuid().ToString();
        Sport = sport;
        DatePlayed = datePlayed;
        Game = game;
        Teams = teams.ToList();
        Results = results.ToList();
    }

    public override string ToString()
    {
        return $"{Sport} {Game} played on {DatePlayed.Date.ToShortDateString()} - " +
               $"{String.Join(',', Teams)}\r\n\t" + 
               $"{String.Join('\t', Results.Select(r => $"{r.team } - {r.score}\r\n"))}";
    }
}
```

**Newtonsoft.Json** ライブラリを使用すると、このオブジェクトのインスタンスを文字列に変えることができます。

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

この文字列を取得し、逆のプロセスを使用してオブジェクトに戻すことができます。

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a>接続をクリーンアップする
Redis 接続が完了したら、`ConnectionMultiplexer` を**破棄 (Dispose)** できます。 これにより、すべての接続が閉じられ、サーバーとの通信がシャットダウンされます。

```csharp
redisConnection.Dispose();
redisConnection = null;
```

アプリケーションを作成し、Redis Cache の簡単な操作を実行しましょう。