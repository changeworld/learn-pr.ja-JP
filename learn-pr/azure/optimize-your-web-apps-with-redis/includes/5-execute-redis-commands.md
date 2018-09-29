<span data-ttu-id="0f128-101">既に説明したように、Redis は複数のサーバー間でレプリケートできるインメモリ NoSQL データベースです。</span><span class="sxs-lookup"><span data-stu-id="0f128-101">As mentioned earlier, Redis is an in-memory NoSQL database which can be replicated across multiple servers.</span></span> <span data-ttu-id="0f128-102">多くの場合、キャッシュとして使用されますが、正式なデータベースまたはメッセージ ブローカーとして使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="0f128-102">It is often used as a cache, but can be used as a formal database or even message-broker.</span></span> 

<span data-ttu-id="0f128-103">さまざまなデータ型や構造体を格納することができ、キャッシュ データを取得したり、キャッシュ自体に関する情報を照会したりするために発行できる各種コマンドをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="0f128-103">It can store a variety of data types and structures and supports a variety of commands you can issue to retrieve cached data or query information about the cache itself.</span></span> <span data-ttu-id="0f128-104">使用するデータは、常にキーと値のペアとして格納されます。</span><span class="sxs-lookup"><span data-stu-id="0f128-104">The data you work with is always stored as key/value pairs.</span></span>

## <a name="executing-commands-on-the-redis-cache"></a><span data-ttu-id="0f128-105">Redis Cache でコマンドを実行する</span><span class="sxs-lookup"><span data-stu-id="0f128-105">Executing commands on the Redis cache</span></span>

<span data-ttu-id="0f128-106">通常、クライアント アプリケーションは "_クライアント ライブラリ_" を使用して要求を作成し、Redis Cache でコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="0f128-106">Typically, a client application will use a _client library_ to form requests and execute commands on a Redis cache.</span></span> <span data-ttu-id="0f128-107">クライアント ライブラリの一覧は、[Redis クライアント ページ](https://redis.io/clients)から直接取得できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-107">You can get a list of client libraries directly from the [Redis clients page](https://redis.io/clients).</span></span> <span data-ttu-id="0f128-108">.NET 言語用の一般的な高性能 Redis クライアントは **StackExchange.Redis** です。</span><span class="sxs-lookup"><span data-stu-id="0f128-108">A popular high-performance Redis client for the .NET language is **StackExchange.Redis**.</span></span> <span data-ttu-id="0f128-109">このパッケージは NuGet から入手でき、コマンド ラインまたは IDE を使用して .NET コードに追加できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-109">The package is available through NuGet and can be added to your .NET code using the command line or IDE.</span></span>

### <a name="connecting-to-your-redis-cache-with-stackexchangeredis"></a><span data-ttu-id="0f128-110">StackExchange.Redis を使用して Redis Cache に接続する</span><span class="sxs-lookup"><span data-stu-id="0f128-110">Connecting to your Redis cache with StackExchange.Redis</span></span>

<span data-ttu-id="0f128-111">Redis サーバーに接続するには、ホスト アドレス、ポート番号、アクセス キーを使用します。</span><span class="sxs-lookup"><span data-stu-id="0f128-111">Recall that we use the host address, port number, and an access key to connect to a Redis server.</span></span> <span data-ttu-id="0f128-112">Azure では、一部の Redis クライアント用に、このデータを 1 つの文字列にまとめた_接続文字列_も提供されます。</span><span class="sxs-lookup"><span data-stu-id="0f128-112">Azure also offers a _connection string_ for some Redis clients which bundles this data together into a single string.</span></span>

### <a name="what-is-a-connection-string"></a><span data-ttu-id="0f128-113">接続文字列とは</span><span class="sxs-lookup"><span data-stu-id="0f128-113">What is a connection string?</span></span>

<span data-ttu-id="0f128-114">接続文字列とは、Azure の Redis Cache に接続して認証するために必要なすべての情報を含む 1 行のテキストです。</span><span class="sxs-lookup"><span data-stu-id="0f128-114">A connection string is a single line of text that includes all the required pieces of information to connect and authenticate to a Redis cache in Azure.</span></span> <span data-ttu-id="0f128-115">接続文字列は次のようになります (**cache-name** フィールドと **password-here** フィールドには実際の値を入力します)。</span><span class="sxs-lookup"><span data-stu-id="0f128-115">It will look something like the following (with the **cache-name** and **password-here** fields filled in with real values):</span></span>

```
[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False
```

> [!TIP]
> <span data-ttu-id="0f128-116">接続文字列は、アプリケーション内で保護する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f128-116">The connection string should be protected in your application.</span></span> <span data-ttu-id="0f128-117">アプリケーションが Azure でホストされている場合は、Azure Key Vault を使用して値を格納することを検討してください。</span><span class="sxs-lookup"><span data-stu-id="0f128-117">If the application is hosted on Azure, consider using an Azure Key Vault to store the value.</span></span>

<span data-ttu-id="0f128-118">この文字列を **StackExchange.Redis** に渡して、サーバーへの接続を作成できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-118">You can pass this string to **StackExchange.Redis** to create a connection the server.</span></span> 

<span data-ttu-id="0f128-119">末尾に次の 2 つの追加パラメーターがあることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0f128-119">Notice that there are two additional parameters at the end:</span></span> 

- <span data-ttu-id="0f128-120">**ssl** - 通信を暗号化できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-120">**ssl** - ensures that communication is encrypted.</span></span>
- <span data-ttu-id="0f128-121">**abortConnection** - その時にサーバーが使用できなくても接続を作成できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-121">**abortConnection** - allows a connection to be created even if the server is unavailable at that moment.</span></span>

<span data-ttu-id="0f128-122">クライアント ライブラリを構成するために文字列に追加できる他の[省略可能なパラメーター](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options)がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="0f128-122">There are several other [optional parameters](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Configuration.md#configuration-options) you can append to the string to configure the client library.</span></span>

### <a name="creating-a-connection"></a><span data-ttu-id="0f128-123">接続を作成する</span><span class="sxs-lookup"><span data-stu-id="0f128-123">Creating a connection</span></span>

<span data-ttu-id="0f128-124">**StackExchange.Redis** の主な接続オブジェクトは `StackExchange.Redis.ConnectionMultiplexer` クラスです。</span><span class="sxs-lookup"><span data-stu-id="0f128-124">The main connection object in **StackExchange.Redis** is the `StackExchange.Redis.ConnectionMultiplexer` class.</span></span> <span data-ttu-id="0f128-125">このオブジェクトは、Redis サーバー (またはサーバーのグループ) への接続プロセスを抽象化します。</span><span class="sxs-lookup"><span data-stu-id="0f128-125">This object abstracts the process of connecting to a Redis server (or group of servers).</span></span> <span data-ttu-id="0f128-126">接続を効率的に管理するように最適化されており、キャッシュにアクセスする必要がある間は保持されることを目的としています。</span><span class="sxs-lookup"><span data-stu-id="0f128-126">It's optimized to manage connections efficiently and intended to be kept around while you need access to the cache.</span></span>

<span data-ttu-id="0f128-127">静的 `ConnectionMultiplexer.Connect` メソッドまたは `ConnectionMultiplexer.ConnectAsync` メソッドを使用して `ConnectionMultiplexer` インスタンスを作成し、接続文字列または `ConfigurationOptions` オブジェクトを渡します。</span><span class="sxs-lookup"><span data-stu-id="0f128-127">You create a `ConnectionMultiplexer` instance using the static `ConnectionMultiplexer.Connect` or `ConnectionMultiplexer.ConnectAsync` method, passing in either a connection string or a `ConfigurationOptions` object.</span></span> 

<span data-ttu-id="0f128-128">簡単な例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0f128-128">Here's a simple example:</span></span>

```csharp
using StackExchange.Redis;
...
var connectionString = "[cache-name].redis.cache.windows.net:6380,password=[password-here],ssl=True,abortConnect=False";
var redisConnection = ConnectionMultiplexer.Connect(connectionString);
    // ^^^ store and re-use this!!!
```

<span data-ttu-id="0f128-129">`ConnectionMultiplexer` を作成したら、主に次の 3 つのことを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0f128-129">Once you have a `ConnectionMultiplexer`, there are 3 primary things you might want to do:</span></span>

1. <span data-ttu-id="0f128-130">Redis データベースにアクセスする。</span><span class="sxs-lookup"><span data-stu-id="0f128-130">Access a Redis Database.</span></span> <span data-ttu-id="0f128-131">ここではこれについて説明します。</span><span class="sxs-lookup"><span data-stu-id="0f128-131">This is what we will focus on here.</span></span>
2. <span data-ttu-id="0f128-132">Redis のパブリッシャー/サブスクリプト機能を使用する。</span><span class="sxs-lookup"><span data-stu-id="0f128-132">Make use of the publisher/subscript features of Redis.</span></span> <span data-ttu-id="0f128-133">これはこのモジュールの範囲外です。</span><span class="sxs-lookup"><span data-stu-id="0f128-133">This is outside the scope of this module.</span></span>
3. <span data-ttu-id="0f128-134">メンテナンスまたは監視のために個々のサーバーにアクセスする。</span><span class="sxs-lookup"><span data-stu-id="0f128-134">Access an individual server for maintenance or monitoring purposes.</span></span>

### <a name="accessing-a-redis-database"></a><span data-ttu-id="0f128-135">Redis データベースにアクセスする</span><span class="sxs-lookup"><span data-stu-id="0f128-135">Accessing a Redis database</span></span>

<span data-ttu-id="0f128-136">Redis データベースは `IDatabase` 型で表されます。</span><span class="sxs-lookup"><span data-stu-id="0f128-136">The Redis database is represented by the `IDatabase` type.</span></span> <span data-ttu-id="0f128-137">`GetDatabase()` メソッドを使用してこれを取得できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-137">You can retrieve one using the `GetDatabase()` method:</span></span>

```csharp
IDatabase db = redisConnection.GetDatabase();
```

> [!TIP]
> <span data-ttu-id="0f128-138">`GetDatabase` から返されるオブジェクトはライトウェイト オブジェクトであり、保存する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0f128-138">The object returned from `GetDatabase` is a lightweight object, and does not need to be stored.</span></span> <span data-ttu-id="0f128-139">`ConnectionMultiplexer` だけを保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0f128-139">Only the `ConnectionMultiplexer` needs to be kept alive.</span></span>

<span data-ttu-id="0f128-140">`IDatabase` オブジェクトを取得したら、キャッシュを操作するメソッドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-140">Once you have a `IDatabase` object, you can execute methods to interact with the cache.</span></span> <span data-ttu-id="0f128-141">すべてのメソッドには同期バージョンと非同期バージョンがあります。非同期バージョンは、`async` および `await` キーワードに対応できるようにするための `Task` オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="0f128-141">All methods have synchronous and asynchronous versions which return `Task` objects to make them compatible with the `async` and `await` keywords.</span></span>

<span data-ttu-id="0f128-142">キーと値をキャッシュに格納する例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0f128-142">Here is an example of storing a key/value in the cache:</span></span>

```csharp
bool wasSet = db.StringSet("favorite:flavor", "i-love-rocky-road");
```

<span data-ttu-id="0f128-143">`StringSet` メソッドは、値が設定されているか (`true`)、設定されていないか (`false`) を示す `bool` を返します。</span><span class="sxs-lookup"><span data-stu-id="0f128-143">The `StringSet` method returns a `bool` indicating whether the value was set (`true`) or not (`false`).</span></span> <span data-ttu-id="0f128-144">値は `StringGet` メソッドを使用して取得できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-144">We can then retrieve the value with the `StringGet` method:</span></span>

```csharp
string value = db.StringGet("favorite:flavor");
Console.WriteLine(value); // displays: ""i-love-rocky-road""
```

#### <a name="getting-and-setting-binary-values"></a><span data-ttu-id="0f128-145">バイナリ値を取得および設定する</span><span class="sxs-lookup"><span data-stu-id="0f128-145">Getting and Setting binary values</span></span>

<span data-ttu-id="0f128-146">Redis キーと値は "_バイナリ セーフ_" であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="0f128-146">Recall that Redis keys and values are _binary safe_.</span></span> <span data-ttu-id="0f128-147">これらの同じメソッドを使用して、バイナリ データを格納できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-147">These same methods can be used to store binary data.</span></span> <span data-ttu-id="0f128-148">データを自然に操作できるように、`byte[]` 型を処理する暗黙的な変換演算子があります。</span><span class="sxs-lookup"><span data-stu-id="0f128-148">There are implicit conversion operators to work with `byte[]` types so you can work with the data naturally:</span></span>

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
> <span data-ttu-id="0f128-149">**StackExchange.Redis** では、`RedisKey` 型を使用してキーを表します。</span><span class="sxs-lookup"><span data-stu-id="0f128-149">**StackExchange.Redis** represents keys using the `RedisKey` type.</span></span> <span data-ttu-id="0f128-150">このクラスでは、`string` と `byte[]` の両方との間で暗黙的な変換が行われるので、複雑さを伴わずにテキスト キーとバイナリ キーの両方を使用できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-150">This class has implicit conversions to and from both `string` and `byte[]`, allowing both text and binary keys to be used without any complication.</span></span> <span data-ttu-id="0f128-151">値は `RedisValue` 型で表されます。</span><span class="sxs-lookup"><span data-stu-id="0f128-151">Values are represented by the `RedisValue` type.</span></span> <span data-ttu-id="0f128-152">`RedisKey` と同様に、`string` または `byte[]` を渡すことができるように、暗黙的な変換が適切に行われます。</span><span class="sxs-lookup"><span data-stu-id="0f128-152">As with `RedisKey`, there are implicit conversions in place to allow you to pass `string` or `byte[]`.</span></span>

#### <a name="other-common-operations"></a><span data-ttu-id="0f128-153">その他の一般的な操作</span><span class="sxs-lookup"><span data-stu-id="0f128-153">Other common operations</span></span>

<span data-ttu-id="0f128-154">`IDatabase` インターフェイスには、Redis Cache で動作する他の複数のメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="0f128-154">The `IDatabase` interface includes several other methods to work with the Redis cache.</span></span> <span data-ttu-id="0f128-155">ハッシュ、リスト、セット、順序付けされたセットを操作するメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="0f128-155">There are methods to work with hashes, lists, sets, and ordered sets.</span></span>

<span data-ttu-id="0f128-156">ここでは、単一キーで動作するより一般的なものを紹介します。完全なリストについては、このインターフェイスの[ソース コードを参照](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs)してください。</span><span class="sxs-lookup"><span data-stu-id="0f128-156">Here are some of the more common ones that work with single keys, you can [read the source code](https://github.com/StackExchange/StackExchange.Redis/blob/master/src/StackExchange.Redis/Interfaces/IDatabase.cs) for the interface to see the full list.</span></span>

| <span data-ttu-id="0f128-157">メソッド</span><span class="sxs-lookup"><span data-stu-id="0f128-157">Method</span></span> | <span data-ttu-id="0f128-158">説明</span><span class="sxs-lookup"><span data-stu-id="0f128-158">Description</span></span> |
|--------|-------------|
| `CreateBatch` | <span data-ttu-id="0f128-159">単一ユニットとしてサーバーに送信されるが、必ずしもユニットとして処理されるわけではない "_操作のグループ_" を作成します。</span><span class="sxs-lookup"><span data-stu-id="0f128-159">Creates a _group of operations_ that will be sent to the server as a single unit, but not necessarily processed as a unit.</span></span> |
| `CreateTransaction` | <span data-ttu-id="0f128-160">単一ユニットとしてサーバーに送信され、_なおかつ_サーバー上で単一ユニットとして処理される操作のグループを作成します。</span><span class="sxs-lookup"><span data-stu-id="0f128-160">Creates a group of operations that will be sent to the server as a single unit _and_ processed on the server as a single unit.</span></span> |
| `KeyDelete` | <span data-ttu-id="0f128-161">キーと値を削除します。</span><span class="sxs-lookup"><span data-stu-id="0f128-161">Delete the key/value.</span></span> |
| `KeyExists` | <span data-ttu-id="0f128-162">特定のキーがキャッシュに存在するかどうかを返します。</span><span class="sxs-lookup"><span data-stu-id="0f128-162">Returns whether the given key exists in cache.</span></span> |
| `KeyExpire` | <span data-ttu-id="0f128-163">キーの Time to Live (TTL) の有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="0f128-163">Sets a time-to-live (TTL) expiration on a key.</span></span> |
| `KeyRename` | <span data-ttu-id="0f128-164">キーの名前を変更します。</span><span class="sxs-lookup"><span data-stu-id="0f128-164">Renames a key.</span></span> |
| `KeyTimeToLive` | <span data-ttu-id="0f128-165">キーの TTL を返します。</span><span class="sxs-lookup"><span data-stu-id="0f128-165">Returns the TTL for a key.</span></span> |
| `KeyType` | <span data-ttu-id="0f128-166">キーに格納されている値の型の文字列表現を返します。</span><span class="sxs-lookup"><span data-stu-id="0f128-166">Returns the string representation of the type of the value stored at key.</span></span> <span data-ttu-id="0f128-167">返される型は、文字列、list、set、zset、hash です。</span><span class="sxs-lookup"><span data-stu-id="0f128-167">The different types that can be returned are: string, list, set, zset and hash.</span></span> |
       
### <a name="executing-other-commands"></a><span data-ttu-id="0f128-168">他のコマンドを実行する</span><span class="sxs-lookup"><span data-stu-id="0f128-168">Executing other commands</span></span>

<span data-ttu-id="0f128-169">`IDatabase` オブジェクトには、テキスト コマンドを Redis サーバーに渡すときに使用できる `Execute` メソッドと `ExecuteAsync` メソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="0f128-169">The `IDatabase` object has an `Execute` and `ExecuteAsync` method which can be used to pass textual commands to the Redis server.</span></span> <span data-ttu-id="0f128-170">例:</span><span class="sxs-lookup"><span data-stu-id="0f128-170">For example:</span></span>

```csharp
var result = db.Execute("ping");
Console.WriteLine(result.ToString()); // displays: "PONG"
```

<span data-ttu-id="0f128-171">`Execute` メソッドと `ExecuteAsync` メソッドは、`RedisResult` オブジェクトを返します。これは、次の 2 つのプロパティを含むデータ ホルダーです。</span><span class="sxs-lookup"><span data-stu-id="0f128-171">The `Execute` and `ExecuteAsync` methods return a `RedisResult` object which is a data holder that includes two properties:</span></span>

- <span data-ttu-id="0f128-172">`Type` - 結果の型 ("STRING"、"INTEGER" など) を示す `string` を返します。</span><span class="sxs-lookup"><span data-stu-id="0f128-172">`Type` which returns a `string` indicating the type of the result - "STRING", "INTEGER", etc.</span></span>
- <span data-ttu-id="0f128-173">`IsNull` - 結果が `null` かどうかを検出する true/false 値。</span><span class="sxs-lookup"><span data-stu-id="0f128-173">`IsNull` a true/false value to detect when the result is `null`.</span></span>

<span data-ttu-id="0f128-174">`RedisResult` で `ToString()` を使用して、実際の戻り値を取得できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-174">You can then use `ToString()` on the `RedisResult` to get the actual return value.</span></span>

<span data-ttu-id="0f128-175">`Execute` を使用して、サポートされているコマンドを実行できます。たとえば、キャッシュに接続されているすべてのクライアントを取得できます ("CLIENT LIST")。</span><span class="sxs-lookup"><span data-stu-id="0f128-175">You can use `Execute` to perform any supported commands - for example, we can get all the clients connected to the cache ("CLIENT LIST"):</span></span>

```csharp
var result = await db.ExecuteAsync("client", "list");
Console.WriteLine($"Type = {result.Type}\r\nResult = {result}");
```

<span data-ttu-id="0f128-176">この場合、接続されているすべてのクライアントが出力されます。</span><span class="sxs-lookup"><span data-stu-id="0f128-176">This would output all the connected clients:</span></span>

```output
Type = BulkString
Result = id=9469 addr=16.183.122.154:54961 fd=18 name=DESKTOP-AAAAAA age=0 idle=0 flags=N db=0 sub=1 psub=0 multi=-1 qbuf=0 qbuf-free=0 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=subscribe numops=5
id=9470 addr=16.183.122.155:54967 fd=13 name=DESKTOP-BBBBBB age=0 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 ow=0 owmem=0 events=r cmd=client numops=17
```

### <a name="storing-more-complex-values"></a><span data-ttu-id="0f128-177">より複雑な値を格納する</span><span class="sxs-lookup"><span data-stu-id="0f128-177">Storing more complex values</span></span>
<span data-ttu-id="0f128-178">Redis はバイナリ セーフな文字列を中心としていますが、オブジェクト グラフをテキスト形式 (通常は XML または JSON) にシリアル化することでキャッシュすることができます。</span><span class="sxs-lookup"><span data-stu-id="0f128-178">Redis is oriented around binary safe strings, but you can cache off object graphs by serializing them to a textual format - typically XML or JSON.</span></span> <span data-ttu-id="0f128-179">たとえば、演習で使用する統計では、次のような `GameStats` オブジェクトがあります。</span><span class="sxs-lookup"><span data-stu-id="0f128-179">For example, perhaps for our statistics, we have a `GameStats` object which looks like:</span></span>

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

<span data-ttu-id="0f128-180">**Newtonsoft.Json** ライブラリを使用すると、このオブジェクトのインスタンスを文字列に変えることができます。</span><span class="sxs-lookup"><span data-stu-id="0f128-180">We could use the **Newtonsoft.Json** library to turn an instance of this object into a string:</span></span>

```csharp
var stat = new GameStat("Soccer", new DateTime(1950, 7, 16), "FIFA World Cup", 
                new[] { "Uruguay", "Brazil" },
                new[] { ("Uruguay", 2), ("Brazil", 1) });

string serializedValue = Newtonsoft.Json.JsonConvert.SerializeObject(stat);
bool added = db.StringSet("event:1950-world-cup", serializedValue);
```

<span data-ttu-id="0f128-181">この文字列を取得し、逆のプロセスを使用してオブジェクトに戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="0f128-181">We could retrieve it and turn it back into an object using the reverse process:</span></span>

```csharp
var result = db.StringGet("event:1950-world-cup");
var stat = Newtonsoft.Json.JsonConvert.DeserializeObject<GameStat>(result.ToString());
Console.WriteLine(stat.Sport); // displays "Soccer"
```

## <a name="cleaning-up-the-connection"></a><span data-ttu-id="0f128-182">接続をクリーンアップする</span><span class="sxs-lookup"><span data-stu-id="0f128-182">Cleaning up the connection</span></span>
<span data-ttu-id="0f128-183">Redis 接続が完了したら、`ConnectionMultiplexer` を**破棄 (Dispose)** できます。</span><span class="sxs-lookup"><span data-stu-id="0f128-183">Once you are done with the Redis connection, you can **Dispose** the `ConnectionMultiplexer`.</span></span> <span data-ttu-id="0f128-184">これにより、すべての接続が閉じられ、サーバーとの通信がシャットダウンされます。</span><span class="sxs-lookup"><span data-stu-id="0f128-184">This will close all connections and shutdown the communication to the server.</span></span>

```csharp
redisConnection.Dispose();
redisConnection = null;
```

<span data-ttu-id="0f128-185">アプリケーションを作成し、Redis Cache の簡単な操作を実行しましょう。</span><span class="sxs-lookup"><span data-stu-id="0f128-185">Let's create an application and do some simple work with our Redis cache.</span></span>