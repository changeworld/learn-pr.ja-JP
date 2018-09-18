<span data-ttu-id="94249-101">自社のスポーツ統計情報の開発チームにより、キャッシュが最近要求されたデータのパフォーマンスを大幅に向上させる可能性があると判断されました。</span><span class="sxs-lookup"><span data-stu-id="94249-101">Your sports statistics development team has decided that caching could dramatically improve performance for recently requested data.</span></span> <span data-ttu-id="94249-102">次の手順として、Azure Redis Cache インスタンスを作成および構成します。</span><span class="sxs-lookup"><span data-stu-id="94249-102">Your next step is to create and configure an Azure Redis Cache instance.</span></span>

## <a name="create-and-configure-the-azure-redis-cache-instance"></a><span data-ttu-id="94249-103">Azure Redis Cache インスタンスの作成および構成</span><span class="sxs-lookup"><span data-stu-id="94249-103">Create and configure the Azure Redis Cache instance</span></span>

1. <span data-ttu-id="94249-104">ブラウザーで [Azure portal](https://portal.azure.com/?azure-portal=true) を開きます。</span><span class="sxs-lookup"><span data-stu-id="94249-104">Open the [Azure Portal](https://portal.azure.com/?azure-portal=true) in your browser.</span></span>

1. <span data-ttu-id="94249-105">**[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="94249-105">Click on **Create a resource**.</span></span>

1. <span data-ttu-id="94249-106">**[Redis Cache]** を検索します。</span><span class="sxs-lookup"><span data-stu-id="94249-106">Search for **Redis Cache**.</span></span>

1. <span data-ttu-id="94249-107">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="94249-107">Click **Create**.</span></span>

1. <span data-ttu-id="94249-108">Azure Redis Cache インスタンスは、Azure portal でデータベース リソースから追加することができます。</span><span class="sxs-lookup"><span data-stu-id="94249-108">You can add an Azure Redis Cache instance from the database resources in the Azure portal.</span></span>

1. <span data-ttu-id="94249-109">グローバルに一意な名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="94249-109">Provide a globally unique name.</span></span> <span data-ttu-id="94249-110">この名前は、1 から 63 文字の文字列で、数字、英字、"-" 文字のみを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94249-110">The name must be a string between 1 and 63 characters and include only numbers, letters, and the '-' character.</span></span> <span data-ttu-id="94249-111">キャッシュ名の先頭と末尾には "-" 文字を使用できません。また、連続する "-" 文字は無効です。</span><span class="sxs-lookup"><span data-stu-id="94249-111">The cache name can't start or end with the '-' character, and consecutive '-' characters aren't valid.</span></span>

1. <span data-ttu-id="94249-112">この新しい Azure Redis Cache インスタンスが作成されるサブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="94249-112">Provide the subscription under which this new Azure Redis Cache instance is created.</span></span>

1. <span data-ttu-id="94249-113">キャッシュを作成するリソース グループに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="94249-113">Name the resource group in which to create your cache.</span></span> <span data-ttu-id="94249-114">アプリのすべてのリソースを 1 つのグループ内に配置すると、それらをまとめて管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="94249-114">By putting all the resources for an app in a group, you can manage them together.</span></span>

1. <span data-ttu-id="94249-115">キャッシュ インスタンスとアプリケーションは常に同じリージョン/場所に配置するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="94249-115">Always place your cache instance and your application in the same region / location.</span></span> <span data-ttu-id="94249-116">異なるリージョンのキャッシュに接続すると、待ち時間が大幅に増加し、信頼性が低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="94249-116">Connecting to a cache in a different region can significantly increase latency and reduce reliability.</span></span> <span data-ttu-id="94249-117">Azure の外部にあるキャッシュに接続する場合は、データを使用するアプリケーションが実行されている場所に近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="94249-117">If you are connecting to the cache outside of Azure, then select a location close to where the application consuming the data is running.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="94249-118">キャッシュがデータ ストアよりもデータ コンシューマーに近いことが、はるかに重要です。</span><span class="sxs-lookup"><span data-stu-id="94249-118">It's far more important that the cache is close to the data consumer than the data store.</span></span>

1. <span data-ttu-id="94249-119">価格レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="94249-119">Select the pricing tier.</span></span> 
    - <span data-ttu-id="94249-120">運用システムでは、常に Standard レベルまたは Premium レベルを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="94249-120">We recommend you always use Standard or Premium Tier for production systems.</span></span> <span data-ttu-id="94249-121">Basic レベルは単一ノード システムであり、データ レプリケーション機能や SLA がありません。</span><span class="sxs-lookup"><span data-stu-id="94249-121">The Basic Tier is a single node system with no data replication and no SLA.</span></span> <span data-ttu-id="94249-122">また、C1 以上のキャッシュを使用してください。</span><span class="sxs-lookup"><span data-stu-id="94249-122">Also, use at least a C1 cache.</span></span> <span data-ttu-id="94249-123">C0 キャッシュは CPU コアが共有であり、メモリも非常に少量であるため、単純な開発/テスト シナリオにしか適していません。</span><span class="sxs-lookup"><span data-stu-id="94249-123">C0 caches are really meant for simple dev/test scenarios since they have a shared CPU core and very little memory.</span></span>

    - <span data-ttu-id="94249-124">Premium レベルでは、ディザスター リカバリーを提供するために、次の 2 つの方法でデータを永続化できます。</span><span class="sxs-lookup"><span data-stu-id="94249-124">The Premium tier allows you to persist data in two ways to provide disaster recovery:</span></span>

        - <span data-ttu-id="94249-125">RDB 永続化では、定期的にスナップショットが作成されます。エラーが発生した場合には、このスナップショットを使用してキャッシュを再構築できます。</span><span class="sxs-lookup"><span data-stu-id="94249-125">RDB persistence takes a periodic snapshot and can rebuild the cache using the snapshot in case of failure.</span></span>

            ![新しい Redis Cache インスタンスでの RDB 永続化オプションを示す Azure portal のスクリーンショット。](../media/3-redis-persistence-1.png)

        - <span data-ttu-id="94249-127">AOF 永続化では、すべての書き込み操作がログに最低 1 秒に一度保存されます。</span><span class="sxs-lookup"><span data-stu-id="94249-127">AOF persistence saves every write operation to a log that is saved at least once per second.</span></span> <span data-ttu-id="94249-128">これでは、ファイル サイズが RDB よりも大きくなるものの、データの損失は少なくなります。</span><span class="sxs-lookup"><span data-stu-id="94249-128">This creates bigger files than RDB but has less data loss.</span></span>

            ![新しい Redis Cache インスタンスでの AOF 永続化オプションを示す Azure portal のスクリーンショット。](../media/3-redis-persistence-2.png)

    - <span data-ttu-id="94249-130">仮想ネットワークを使用して、キャッシュをセキュリティ保護します。</span><span class="sxs-lookup"><span data-stu-id="94249-130">Secure your cache with a virtual network.</span></span>
      <span data-ttu-id="94249-131">Premium レベルの Redis Cache がある場合、クラウドの仮想ネットワークにそれをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="94249-131">If you have a premium tier Redis cache, you can deploy it to a virtual network in the cloud.</span></span> <span data-ttu-id="94249-132">キャッシュは同じ仮想ネットワーク内の他の仮想マシンやアプリケーションのみが使用できます。</span><span class="sxs-lookup"><span data-stu-id="94249-132">Your cache will be available to only other virtual machines and applications in the same virtual network.</span></span>

    - <span data-ttu-id="94249-133">クラスタリングでキャッシュを分散します。</span><span class="sxs-lookup"><span data-stu-id="94249-133">Distribute your cache with clustering.</span></span>
      <span data-ttu-id="94249-134">Premium レベルの Redis Cache では、クラスタリングを実装して、複数のノードに自動的にデータセットが分割されるようにできます。</span><span class="sxs-lookup"><span data-stu-id="94249-134">With a premium tier Redis cache, you can implement clustering to automatically split your dataset among multiple nodes.</span></span> <span data-ttu-id="94249-135">クラスタリングを実装するには、シャードの数を最大の 10 に指定します。</span><span class="sxs-lookup"><span data-stu-id="94249-135">To implement clustering, you specify the number of shards to a maximum of 10.</span></span> <span data-ttu-id="94249-136">発生するコストは、元のノードのコストにシャード数を掛けたものです。</span><span class="sxs-lookup"><span data-stu-id="94249-136">The cost incurred is the cost of the original node, multiplied by the number of shards.</span></span>

    - <span data-ttu-id="94249-137">Azure Managed Cache Service から移行します。</span><span class="sxs-lookup"><span data-stu-id="94249-137">Migrate from Azure Managed Cache Service.</span></span>
      <span data-ttu-id="94249-138">Azure Managed Cache Service を使用するアプリケーションの Azure Redis Cache への移行は、使用されている Managed Cache Service の機能によっては、最小限の変更をアプリケーションに対して行うだけで実現できます。</span><span class="sxs-lookup"><span data-stu-id="94249-138">Depending on the Managed Cache Service features you use, migrating applications that use Azure Managed Cache Service to Azure Redis Cache can be accomplished with minimal changes to your application.</span></span> <span data-ttu-id="94249-139">API は正確には同じではありませんが、類似しています。</span><span class="sxs-lookup"><span data-stu-id="94249-139">While the APIs aren't exactly the same, they're similar.</span></span> <span data-ttu-id="94249-140">Managed Cache Service を使用してキャッシュにアクセスする既存のコードの多くは、最小限の変更で再利用することができます。</span><span class="sxs-lookup"><span data-stu-id="94249-140">Much of your existing code that accesses the cache with Managed Cache Service can be reused with minimal changes.</span></span>

## <a name="accessing-the-redis-instance"></a><span data-ttu-id="94249-141">Redis インスタンスへのアクセス</span><span class="sxs-lookup"><span data-stu-id="94249-141">Accessing the Redis instance</span></span>

<span data-ttu-id="94249-142">Redis は、[既知のコマンド](https://redis.io/commands)のセットをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="94249-142">Redis supports a set of [known commands](https://redis.io/commands).</span></span> <span data-ttu-id="94249-143">コマンドは、通常、`COMMAND parameter1 parameter2 parameter3` として発行されます。</span><span class="sxs-lookup"><span data-stu-id="94249-143">A command is typically issued as `COMMAND parameter1 parameter2 parameter3`.</span></span>

<span data-ttu-id="94249-144">使用できるいくつかの一般的なコマンドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="94249-144">Here are some common commands you can use:</span></span>

| <span data-ttu-id="94249-145">コマンド</span><span class="sxs-lookup"><span data-stu-id="94249-145">Command</span></span> | <span data-ttu-id="94249-146">説明</span><span class="sxs-lookup"><span data-stu-id="94249-146">Description</span></span> |
|---------|-------------|
| `ping` | <span data-ttu-id="94249-147">サーバーへの ping を実行します。</span><span class="sxs-lookup"><span data-stu-id="94249-147">Ping the server.</span></span> <span data-ttu-id="94249-148">"PONG" を返します。</span><span class="sxs-lookup"><span data-stu-id="94249-148">Returns "PONG".</span></span> |
| `set [key] [value]` | <span data-ttu-id="94249-149">キャッシュのキー/値を設定します。</span><span class="sxs-lookup"><span data-stu-id="94249-149">Sets a key/value in the cache.</span></span> <span data-ttu-id="94249-150">成功したら "OK" を返します。</span><span class="sxs-lookup"><span data-stu-id="94249-150">Returns "OK" on success.</span></span> |
| `get [key]` | <span data-ttu-id="94249-151">キャッシュから値を取得します。</span><span class="sxs-lookup"><span data-stu-id="94249-151">Gets a value from the cache.</span></span> |
| `exists [key]` | <span data-ttu-id="94249-152">**キー**がキャッシュに存在する場合は "1"、存在しない場合は "0" を返します。</span><span class="sxs-lookup"><span data-stu-id="94249-152">Returns '1' if the **key** exists in the cache, '0' if it doesn't.</span></span> |
| `type [key]` | <span data-ttu-id="94249-153">指定された**キー**の値に関連付けられている型を返します。</span><span class="sxs-lookup"><span data-stu-id="94249-153">Returns the type associated to the value for the given **key**.</span></span> |
| `incr [key]` | <span data-ttu-id="94249-154">**キー**に関連付けられている指定された値を "1" だけ増分します。</span><span class="sxs-lookup"><span data-stu-id="94249-154">Increment the given value associated with **key** by \`1'.</span></span> <span data-ttu-id="94249-155">値は整数または倍精度値にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="94249-155">The value must be an integer or double value.</span></span> <span data-ttu-id="94249-156">新しい値を返します。</span><span class="sxs-lookup"><span data-stu-id="94249-156">This returns the new value.</span></span> |
| `incrby [key] [amount]` | <span data-ttu-id="94249-157">**キー**に関連付けられている指定された値を、指定された量だけ増分します。</span><span class="sxs-lookup"><span data-stu-id="94249-157">Increment the given value associated with **key** by the specified amount.</span></span> <span data-ttu-id="94249-158">値は整数または倍精度値にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="94249-158">The value must be an integer or double value.</span></span> <span data-ttu-id="94249-159">新しい値を返します。</span><span class="sxs-lookup"><span data-stu-id="94249-159">This returns the new value.</span></span> |
| `del [key]` | <span data-ttu-id="94249-160">**キー**に関連付けられている値を削除します。</span><span class="sxs-lookup"><span data-stu-id="94249-160">Deletes the value associated with the **key**.</span></span> |
| `flushdb` | <span data-ttu-id="94249-161">データベース内の "_すべての_" キーと値を削除します。</span><span class="sxs-lookup"><span data-stu-id="94249-161">Delete _all_ keys and values in the database.</span></span> |

<span data-ttu-id="94249-162">Redis には、これらのコマンドを直接試すために使用できるコマンドライン ツール (**redis-cli**) が用意されています。</span><span class="sxs-lookup"><span data-stu-id="94249-162">Redis has a command-line tool (**redis-cli**) you can use to experiment directly with these commands.</span></span> <span data-ttu-id="94249-163">次に例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="94249-163">Here are some examples.</span></span>

```output
> set somekey somevalue
OK
> get somekey
"somevalue"
> exists somekey
(string) 1
> del somekey
(string) 1
> exists somekey
(string) 0
```

<span data-ttu-id="94249-164">以下に、`INCR` コマンドの操作例を示します。</span><span class="sxs-lookup"><span data-stu-id="94249-164">Here's an example of working with the `INCR` commands.</span></span> <span data-ttu-id="94249-165">これらは、キャッシュを使用している "_複数のアプリケーションにわたる_" アトミック増分を提供するので便利です。</span><span class="sxs-lookup"><span data-stu-id="94249-165">These are convenient because they provide atomic increments _across multiple applications_ that are using the cache.</span></span>

```output
> set counter 100
OK
> incr counter
(integer) 101
> incrby counter 50
(integer) 151
> type counter
(integer)
```

### <a name="adding-an-expiration-time-to-values"></a><span data-ttu-id="94249-166">値への有効期限の追加</span><span class="sxs-lookup"><span data-stu-id="94249-166">Adding an expiration time to values</span></span>

<span data-ttu-id="94249-167">キャッシュは、よく使用される値をメモリに格納することができるため、重要です。</span><span class="sxs-lookup"><span data-stu-id="94249-167">Caching is important because it allows us to store commonly used values in memory.</span></span> <span data-ttu-id="94249-168">ただし、値が古くなったときに期限切れにする手段も必要です。</span><span class="sxs-lookup"><span data-stu-id="94249-168">However, we also need a way to expire values when they are stale.</span></span> <span data-ttu-id="94249-169">Redis では、キーに _Time to Live_ (TTL) を適用することによって、それが行われます。</span><span class="sxs-lookup"><span data-stu-id="94249-169">In Redis this is done by applying a _time to live_ (TTL) to a key.</span></span>

<span data-ttu-id="94249-170">TTL が経過すると、DEL コマンドが発行されたかのように、キーが自動的に削除されます。</span><span class="sxs-lookup"><span data-stu-id="94249-170">When the TTL elapses, the key is automatically deleted, exactly as if the DEL command were issued.</span></span> <span data-ttu-id="94249-171">TTL の有効期限には、以下の注意事項があります。</span><span class="sxs-lookup"><span data-stu-id="94249-171">Here are some notes on TTL expirations.</span></span>

- <span data-ttu-id="94249-172">有効期限は、秒またはミリ秒の精度で設定することができます。</span><span class="sxs-lookup"><span data-stu-id="94249-172">Expirations can be set using seconds or milliseconds precision.</span></span>
- <span data-ttu-id="94249-173">有効期限切れの時刻の精度は、常に 1 ミリ秒です。</span><span class="sxs-lookup"><span data-stu-id="94249-173">The expire time resolution is always 1 millisecond.</span></span>
- <span data-ttu-id="94249-174">期限切れに関する情報は、ディスクに複製され、永続的に保管されます。Redis サーバーが停止しているときは、時間は仮想的に経過します (つまり、キーが期限切れになる日付が Redis によって保存されます)。</span><span class="sxs-lookup"><span data-stu-id="94249-174">Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).</span></span>

<span data-ttu-id="94249-175">次に、有効期間の例を示します。</span><span class="sxs-lookup"><span data-stu-id="94249-175">Here is an example of an expiration:</span></span>

```output
> set counter 100
OK
> expire counter 5
(integer) 1
> get counter
100
... wait ...
> get counter
(nil)
```

## <a name="accessing-a-redis-cache-from-a-client"></a><span data-ttu-id="94249-176">クライアントから Redis キャッシュへのアクセス</span><span class="sxs-lookup"><span data-stu-id="94249-176">Accessing a Redis cache from a client</span></span>

<span data-ttu-id="94249-177">Azure Redis Cache のインスタンスに接続するとき、クライアントにはキャッシュのホスト名、ポート、およびアクセス キーが必要です。</span><span class="sxs-lookup"><span data-stu-id="94249-177">When connecting to an Azure Redis Cache instance, clients need the host name, port, and an access key for the cache.</span></span> <span data-ttu-id="94249-178">この情報は、Azure portal の **[設定] > [アクセス キー]** ページで取得することができます。</span><span class="sxs-lookup"><span data-stu-id="94249-178">You can retrieve this information in the Azure portal through the **Settings > Access Keys** page.</span></span> 

- <span data-ttu-id="94249-179">ホスト名は、キャッシュのパブリック インターネット アドレスです。このアドレスは、キャッシュの名前を使用して作成されたものです。</span><span class="sxs-lookup"><span data-stu-id="94249-179">The host name is the public Internet address of your cache, which was created using the name of the cache.</span></span> <span data-ttu-id="94249-180">たとえば、`sportsresults.redis.cache.windows.net` のようなものです。</span><span class="sxs-lookup"><span data-stu-id="94249-180">For example `sportsresults.redis.cache.windows.net`.</span></span>

- <span data-ttu-id="94249-181">アクセス キーは、キャッシュのパスワードとして動作します。</span><span class="sxs-lookup"><span data-stu-id="94249-181">The access key acts as a password for your cache.</span></span> <span data-ttu-id="94249-182">プライマリとセカンダリの 2 つのキーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="94249-182">There are two keys created: primary and secondary.</span></span> <span data-ttu-id="94249-183">どちらのキーも使用できますが、2 つ用意されているのは、プライマリ キーを変更する必要があるときのためです。</span><span class="sxs-lookup"><span data-stu-id="94249-183">You can use either key, two are provided in case you need to change the primary key.</span></span> <span data-ttu-id="94249-184">すべてのクライアントをセカンダリ キーに切り替えて、プライマリ キーを再生成することができます。</span><span class="sxs-lookup"><span data-stu-id="94249-184">You can switch all of your clients to the secondary key, and regenerate the primary key.</span></span> <span data-ttu-id="94249-185">こうすると、元のプライマリ キーを使用しているすべてのアプリケーションがブロックされます。</span><span class="sxs-lookup"><span data-stu-id="94249-185">This would block any applications using the original primary key.</span></span> <span data-ttu-id="94249-186">Microsoft では、個人用のパスワードと同じように、定期的にキーを再生成することを推奨しています。</span><span class="sxs-lookup"><span data-stu-id="94249-186">Microsoft recommends periodically regenerating the keys - much like you would your personal passwords.</span></span>

> [!WARNING]
> <span data-ttu-id="94249-187">アクセス キーは機密情報であると見なして、パスワードと同じように取り扱う必要があります。</span><span class="sxs-lookup"><span data-stu-id="94249-187">Your access keys should be considered confidential information, treat them like you would a password.</span></span> <span data-ttu-id="94249-188">アクセス キーを入手した人は、キャッシュに対して任意の操作を実行できてしまいます。</span><span class="sxs-lookup"><span data-stu-id="94249-188">Anyone who has an access key can perform any operation on your cache!</span></span>
