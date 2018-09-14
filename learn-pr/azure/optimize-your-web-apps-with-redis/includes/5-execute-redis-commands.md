前述のように、Redis は、複数のサーバー間でレプリケートすることができるインメモリ NoSQL データベースです。 キャッシュとして使用されますが、仮のデータベースまたは偶数のメッセージ ブローカーとして使用できます。 

さまざまなデータ型と構造体を格納することができ、さまざまなキャッシュ自体についてキャッシュされたデータまたはクエリの情報を取得するを実行できるコマンドをサポートしています。 使用するデータは常に、キー/値ペアとして格納します。

## <a name="executing-commands-on-the-redis-cache"></a>Redis cache にコマンドを実行します。

通常、クライアント アプリケーションを使用する_クライアント ライブラリ_を要求を形成し、Redis cache のコマンドを実行します。 直接クライアント ライブラリの一覧を取得することができます、 [Redis クライアント ページ](https://redis.io/clients)します。 .NET 言語用に人気のある高性能な Redis クライアントは **StackExchange.Redis** です。 パッケージは NuGet から入手できますであり、コマンドラインまたは IDE を使用して .NET コードに追加できます。

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a>StackExchange.Redis で Redis cache に接続します。

ホスト アドレス、ポート番号、およびアクセス キーを使用して Redis サーバーに接続することを思い出してください。 Azure も提供する_接続文字列_1 つの文字列にこのデータをまとめてバンドルが一部の Redis クライアント。

### <a name="what-is-a-connection-string"></a>接続文字列とは何ですか。

接続文字列は、1 行の情報を azure Redis cache に接続して認証の必要なすべての部分を含むテキストです。 次のような内容が表示されます (使用、**キャッシュ名**と**パスワードここ**実際の値フィールドに入力)。

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> 接続文字列は、アプリケーションで保護する必要があります。 アプリケーションは、Azure でホストされる、Azure Key Vault を使用して値を格納することを検討してください。

この文字列を渡すことができます**StackExchange.Redis**サーバー接続を作成します。 

最後に 2 つのパラメーターがあることに注意してください。 

- **ssl** -により通信を暗号化するようになります。
- **abortConnection** -場合でも、その時点で、サーバーがご利用いただけません作成への接続を許可します。

あるいくつかその他の[省略可能なパラメーター](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options)クライアント ライブラリを構成する文字列を追加することができます。

### <a name="creating-a-connection"></a>接続を作成します。

メイン接続オブジェクト**StackExchange.Redis**は、`StackExchange.Redis.ConnectionMultiplexer`クラス。 このオブジェクトは、Redis サーバー (またはサーバーのグループ) に接続するプロセスを抽象化します。 接続を効率的に管理するように最適化は、キャッシュへのアクセスを必要がありますが、保持するためのもの。

作成する、`ConnectionMultiplexer`インスタンス、静的なを使用して`ConnectionMultiplexer.Connect`または`ConnectionMultiplexer.ConnectAsync`いずれかで接続文字列を渡して、メソッド、または`ConfigurationOptions`オブジェクト。 

シンプルな例を次に表します。

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

作成したら、`ConnectionMultiplexer`を行いたい場合があります、3 つの主な点があります。

1. Redis データベースにアクセスします。 これは、ここで説明されます。
2. 作成の Redis のパブリッシャー/添字機能を使用します。 これは、このモジュールの範囲外です。
3. メンテナンスや監視のための個々 のサーバーにアクセスします。

### <a name="accessing-a-redis-database"></a>Redis データベースへのアクセス

Redis データベースがによって表される、`IDatabase`型。 1 つを使用して取得できます、`GetDatabase()`メソッド。

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> 返されるオブジェクト`GetDatabase`軽量のオブジェクトは、格納する必要はありません。 のみ、`ConnectionMultiplexer`維持される必要があります。

作成したら、`IDatabase`オブジェクトをキャッシュを操作するメソッドを実行することができます。 すべてのメソッドを返す同期および非同期のバージョンがある`Task`オブジェクトに対応できるようにする、`async`と`await`キーワード。

キャッシュ内のキー/値を格納する例を次に示します。

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

`StringSet`メソッドを返します。 を`bool`値が設定されたかどうかを示す (`true`) かどうか (`false`)。 値を取得できますし、`StringGet`メソッド。

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a>取得およびバイナリ値の設定

Redis のキーと値があることを思い出してください_セーフ バイナリ_します。 これらの同じメソッドは、バイナリ データの格納に使用できます。 使用する暗黙的な変換演算子がある`byte[]`当然ながら、データで動作するための型します。

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
> **StackExchange.Redis**を使用してキーを表す、`RedisKey`型。 このクラスは暗黙的な変換と両方の間`string`と`byte[]`テキストとバイナリの両方のキーすべてコンプリケーションせずに使用することができます。 値がによって表される、`RedisValue`型。 同様`RedisKey`に渡すことを許可する暗黙的な変換がある`string`または`byte[]`します。

#### <a name="other-common-operations"></a>その他の一般的な操作

`IDatabase`インターフェイスには、Redis cache を使用するその他のいくつかのメソッドが含まれています。 ハッシュ、リスト、セット、および順序付けされたセットを使用する方法はあります。

1 つのキーを使用する最も一般的な問題をいくつか紹介[ソース コードを読み取る](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs)の完全な一覧を表示するインターフェイス。

| 方法 | 説明 |
|--------|-------------|
| `CreateBatch` | 作成、_操作のグループ_を 1 つの単位としてサーバーに送信されるが必ずしもを単位として処理されます。 |
| `CreateTransaction` | 1 つの単位としてサーバーに送信される操作のグループを作成します。_と_単一ユニットとして、サーバーで処理します。 |
| `KeyDelete` | キー/値を削除します。 |
| `KeyExists` | 指定したキーがキャッシュに存在するかどうかを返します。 |
| `KeyExpire` | キーの有効期限 (TTL) 有効期限を設定します。 |
| `KeyRename` | キーの名前を変更します。 |
| `KeyTimeToLive` | キーの TTL を返します。 |
| `KeyType` | キーに格納されている値の型の文字列表現を返します。 返されるさまざまな種類: 文字列、list、zset とハッシュを設定します。 |
       
### <a name="executing-other-commands"></a>その他のコマンドを実行します。

`IDatabase`オブジェクトには、`Execute`と`ExecuteAsync`Redis サーバーに、テキスト コマンドを渡すために使用できるメソッド。 例:

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

`Execute`と`ExecuteAsync`メソッドを返す、 `RedisResult` 2 つのプロパティが含まれるデータの所有者であるオブジェクト。

- `Type` 返された、 `string` "STRING"、"INTEGER"などの結果の種類を示します。
- `IsNull` 検出結果が true または false 値`null`します。

使用することができますし、`ToString()`で、`RedisResult`を取得する実際の値を返します。

使用することができます`Execute`サポートされているコマンドを実行する - たとえば、キャッシュ (「クライアント リスト」) に接続されているすべてのクライアントを得られます。

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

これは接続されているすべてのクライアントを出力します。

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a>複雑な値を格納します。
Redis はバイナリ文字列を安全な中心が、キャッシュできますオブジェクト グラフをシリアル - 通常、テキスト形式を XML または JSON。 たとえば、おそらく、統計がある、`GameStats`のようなオブジェクト。

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

使用して、 **Newtonsoft.Json**を文字列にこのオブジェクトのインスタンスを有効にするライブラリ。

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

これを取得し、逆のプロセスを使用して、オブジェクトに戻ったでした。

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a>接続のクリーンアップ
Redis の接続が完了したらが完了したら**Dispose** 、`ConnectionMultiplexer`します。 これは、すべての接続と、サーバーへの通信をシャット ダウンに閉じられます。

```csharp
redisConnection.Dispose();
redisConnection = null;
```

アプリケーションを作成し、Redis cache でのいくつかの簡単な作業を実行してみましょう。