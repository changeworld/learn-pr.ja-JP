<span data-ttu-id="0970d-101">自社のスポーツ統計情報の開発チームにより、キャッシュが最近要求されたデータのパフォーマンスを大幅に向上させる可能性があると判断されました。</span><span class="sxs-lookup"><span data-stu-id="0970d-101">Your sports statistics development team has decided that caching could dramatically improve performance for recently requested data.</span></span> <span data-ttu-id="0970d-102">次の手順として、Azure Redis Cache インスタンスを作成および構成します。</span><span class="sxs-lookup"><span data-stu-id="0970d-102">Your next step is to create and configure an Azure Redis Cache instance.</span></span>

## <a name="create-and-configure-the-azure-redis-cache-instance"></a><span data-ttu-id="0970d-103">Azure Redis Cache インスタンスの作成および構成</span><span class="sxs-lookup"><span data-stu-id="0970d-103">Create and configure the Azure Redis Cache instance</span></span>

<span data-ttu-id="0970d-104">Redis キャッシュは、Azure portal、Azure CLI、または Azure PowerShell を使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="0970d-104">You can create a Redis cache using the Azure portal, the Azure CLI, or Azure PowerShell.</span></span> <span data-ttu-id="0970d-105">目的に合わせてキャッシュを正しく構成する目的で、いくつかのパラメーターを決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-105">There are several parameters you will need to decide in order to configure the cache properly for your purposes.</span></span>

### <a name="name"></a><span data-ttu-id="0970d-106">名前</span><span class="sxs-lookup"><span data-stu-id="0970d-106">Name</span></span>

<span data-ttu-id="0970d-107">Redis キャッシュには、グローバルで一意の名前が必要になります。</span><span class="sxs-lookup"><span data-stu-id="0970d-107">The Redis cache will need a globally unique name.</span></span> <span data-ttu-id="0970d-108">この名前は Azure 内で一意にする必要があります。サービスに接続し、通信する目的で公開 URL の生成に使用されるためです。</span><span class="sxs-lookup"><span data-stu-id="0970d-108">The name has to be unique within Azure because it is used to generate a public-facing URL to connect and communicate with the service.</span></span>

<span data-ttu-id="0970d-109">この名前は 1 から 63 文字で作成し、数字、英字、"-" 文字のみを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-109">The name must be between 1 and 63 characters, composed of numbers, letters, and the '-' character.</span></span> <span data-ttu-id="0970d-110">キャッシュ名の先頭と末尾には "-" 文字を使用できません。また、連続する "-" 文字は無効です。</span><span class="sxs-lookup"><span data-stu-id="0970d-110">The cache name can't start or end with the '-' character, and consecutive '-' characters aren't valid.</span></span>

### <a name="resource-group"></a><span data-ttu-id="0970d-111">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="0970d-111">Resource Group</span></span>

<span data-ttu-id="0970d-112">Azure Redis Cache はマネージド リソースであり、_リソース グループ_の所有者を必要とします。</span><span class="sxs-lookup"><span data-stu-id="0970d-112">The Azure Redis Cache is a managed resource and needs a _resource group_ owner.</span></span> <span data-ttu-id="0970d-113">新しいリソース グループを作成するか、加入しているサブスクリプションの既存のリソース グループを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0970d-113">You can either create a new resource group, or use an existing one in a subscription you are part of.</span></span>

### <a name="location"></a><span data-ttu-id="0970d-114">場所</span><span class="sxs-lookup"><span data-stu-id="0970d-114">Location</span></span>

<span data-ttu-id="0970d-115">Azure リージョンを選択し、Redis キャッシュが物理的に置かれる場所を決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-115">You will need to decide where the Redis cache will be physically located by selecting an Azure region.</span></span> <span data-ttu-id="0970d-116">キャッシュ インスタンスとアプリケーションは常に同じリージョンに配置してください。</span><span class="sxs-lookup"><span data-stu-id="0970d-116">You should always place your cache instance and your application in the same region.</span></span> <span data-ttu-id="0970d-117">異なるリージョンのキャッシュに接続すると、待ち時間が大幅に増加し、信頼性が低下する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-117">Connecting to a cache in a different region can significantly increase latency and reduce reliability.</span></span> <span data-ttu-id="0970d-118">Azure の外部にあるキャッシュに接続する場合は、データを使用するアプリケーションが実行されている場所に近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="0970d-118">If you are connecting to the cache outside of Azure, then select a location close to where the application consuming the data is running.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0970d-119">Redis キャッシュは、データ _コンシューマー_のできるだけ近くに配置してください。</span><span class="sxs-lookup"><span data-stu-id="0970d-119">Put the Redis cache as close to the data _consumer_ as you can.</span></span>

### <a name="pricing-tier"></a><span data-ttu-id="0970d-120">価格レベル</span><span class="sxs-lookup"><span data-stu-id="0970d-120">Pricing tier</span></span>

<span data-ttu-id="0970d-121">前回のユニットで述べたように、Azure Redis Cache には 3 つの価格レベルがあります。</span><span class="sxs-lookup"><span data-stu-id="0970d-121">As mentioned in the last unit, there are three pricing tiers available for an Azure Redis Cache.</span></span>

- <span data-ttu-id="0970d-122">**Basic**: Basic キャッシュは開発/試験に最適です。</span><span class="sxs-lookup"><span data-stu-id="0970d-122">**Basic**: Basic cache ideal for development/testing.</span></span> <span data-ttu-id="0970d-123">1 台のサーバー、53 GB のメモリ、接続数は 20,000 に限定されます。</span><span class="sxs-lookup"><span data-stu-id="0970d-123">Is limited to a single server, 53 GB of memory, and 20,000 connections.</span></span> <span data-ttu-id="0970d-124">このサービス レベルには SLA がありません。</span><span class="sxs-lookup"><span data-stu-id="0970d-124">There is no SLA for this service tier.</span></span>
- <span data-ttu-id="0970d-125">**Standard**: 複製をサポートし、99.99% SLA が含まれる運用キャッシュ。</span><span class="sxs-lookup"><span data-stu-id="0970d-125">**Standard**: Production cache which supports replication and includes an 99.99% SLA.</span></span> <span data-ttu-id="0970d-126">2 つのサーバー (マスター/スレーブ) をサポートします。メモリ/接続数の制限は Basic レベルと同じになります。</span><span class="sxs-lookup"><span data-stu-id="0970d-126">It supports two servers (master/slave), and has the same memory/connection limits as the Basic tier.</span></span>
- <span data-ttu-id="0970d-127">**Premium**: Standard レベルをベースとする企業向けレベルであり、永続化、クラスタリング、スケールアウトをキャッシュで提供します。</span><span class="sxs-lookup"><span data-stu-id="0970d-127">**Premium**: Enterprise tier which builds on the Standard tier and includes persistence, clustering, and scale-out cache support.</span></span> <span data-ttu-id="0970d-128">メモリが最大 530 GB で同時接続数が 40,000 という最高の性能を誇るレベルです。</span><span class="sxs-lookup"><span data-stu-id="0970d-128">This is the highest performing tier with up to 530 GB of memory and 40,000 simultaneous connections.</span></span>

<span data-ttu-id="0970d-129">各レベルで利用できるキャッシュ メモリの量を制御できます。Basic/Standard の場合は C0-C6 から、Premium の場合は P0-P4 からキャッシュ レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="0970d-129">You can control the amount of cache memory available on each tier - this is selected by choosing a cache level from C0-C6 for Basic/Standard and P0-P4 for Premium.</span></span> <span data-ttu-id="0970d-130">詳細については[価格ページ](https://azure.microsoft.com/pricing/details/cache/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="0970d-130">Check the [pricing page](https://azure.microsoft.com/pricing/details/cache/) for full details.</span></span>

> [!TIP]
> <span data-ttu-id="0970d-131">運用システムでは常に Standard レベルまたは Premium レベルを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0970d-131">Microsoft recommends you always use Standard or Premium Tier for production systems.</span></span> <span data-ttu-id="0970d-132">Basic レベルは単一ノード システムであり、データ レプリケーション機能や SLA がありません。</span><span class="sxs-lookup"><span data-stu-id="0970d-132">The Basic Tier is a single node system with no data replication and no SLA.</span></span> <span data-ttu-id="0970d-133">また、C1 以上のキャッシュを使用してください。</span><span class="sxs-lookup"><span data-stu-id="0970d-133">Also, use at least a C1 cache.</span></span> <span data-ttu-id="0970d-134">C0 キャッシュは CPU コアが共有であり、メモリも非常に少量であるため、単純な開発/テスト シナリオにしか適していません。</span><span class="sxs-lookup"><span data-stu-id="0970d-134">C0 caches are really meant for simple dev/test scenarios since they have a shared CPU core and very little memory.</span></span>

<span data-ttu-id="0970d-135">Premium レベルでは、ディザスター リカバリーを提供するために、次の 2 つの方法でデータを永続化できます。</span><span class="sxs-lookup"><span data-stu-id="0970d-135">The Premium tier allows you to persist data in two ways to provide disaster recovery:</span></span>

1. <span data-ttu-id="0970d-136">RDB 永続化では、定期的にスナップショットが作成されます。エラーが発生した場合には、このスナップショットを使用してキャッシュを再構築できます。</span><span class="sxs-lookup"><span data-stu-id="0970d-136">RDB persistence takes a periodic snapshot and can rebuild the cache using the snapshot in case of failure.</span></span>

    ![新しい Redis Cache インスタンスでの RDB 永続化オプションを示す Azure portal のスクリーンショット。](../media/3-redis-persistence-1.png)

2. <span data-ttu-id="0970d-138">AOF 永続化では、すべての書き込み操作がログに最低 1 秒に一度保存されます。</span><span class="sxs-lookup"><span data-stu-id="0970d-138">AOF persistence saves every write operation to a log that is saved at least once per second.</span></span> <span data-ttu-id="0970d-139">これでは、ファイル サイズが RDB よりも大きくなるものの、データの損失は少なくなります。</span><span class="sxs-lookup"><span data-stu-id="0970d-139">This creates bigger files than RDB but has less data loss.</span></span>

    ![Azure portal のスクリーンショット。新しい Redis Cache インスタンスの AOF 永続化オプションが示されています。](../media/3-redis-persistence-2.png)

<span data-ttu-id="0970d-141">**Premium** レベルでのみ利用できる設定が他にいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="0970d-141">There are several other settings which are only available to the **Premium** tier.</span></span>

### <a name="virtual-network-support"></a><span data-ttu-id="0970d-142">仮想ネットワークのサポート</span><span class="sxs-lookup"><span data-stu-id="0970d-142">Virtual Network support</span></span>

<span data-ttu-id="0970d-143">Premium レベルの Redis キャッシュを作成する場合、クラウドの仮想ネットワークにそれをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="0970d-143">If you create a premium tier Redis cache, you can deploy it to a virtual network in the cloud.</span></span> <span data-ttu-id="0970d-144">キャッシュは同じ仮想ネットワーク内の他の仮想マシンやアプリケーションのみが使用できます。</span><span class="sxs-lookup"><span data-stu-id="0970d-144">Your cache will be available to only other virtual machines and applications in the same virtual network.</span></span> <span data-ttu-id="0970d-145">サービスとキャッシュがいずれも Azure でホストされているか、Azure 仮想ネットワーク VPN 経由で接続されているとき、セキュリティのレベルが上がります。</span><span class="sxs-lookup"><span data-stu-id="0970d-145">This provides a higher level of security when your service and cache are both hosted in Azure, or are connected through an Azure virtual network VPN.</span></span>

### <a name="clustering-support"></a><span data-ttu-id="0970d-146">クラスター化のサポート</span><span class="sxs-lookup"><span data-stu-id="0970d-146">Clustering support</span></span>

<span data-ttu-id="0970d-147">Premium レベルの Redis Cache では、クラスタリングを実装して、複数のノードに自動的にデータセットが分割されるようにできます。</span><span class="sxs-lookup"><span data-stu-id="0970d-147">With a premium tier Redis cache, you can implement clustering to automatically split your dataset among multiple nodes.</span></span> <span data-ttu-id="0970d-148">クラスタリングを実装するには、シャードの数を最大の 10 に指定します。</span><span class="sxs-lookup"><span data-stu-id="0970d-148">To implement clustering, you specify the number of shards to a maximum of 10.</span></span> <span data-ttu-id="0970d-149">発生するコストは、元のノードのコストにシャード数を掛けたものです。</span><span class="sxs-lookup"><span data-stu-id="0970d-149">The cost incurred is the cost of the original node, multiplied by the number of shards.</span></span>

## <a name="accessing-the-redis-instance"></a><span data-ttu-id="0970d-150">Redis インスタンスへのアクセス</span><span class="sxs-lookup"><span data-stu-id="0970d-150">Accessing the Redis instance</span></span>

<span data-ttu-id="0970d-151">Redis は、[既知のコマンド](https://redis.io/commands)のセットをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="0970d-151">Redis supports a set of [known commands](https://redis.io/commands).</span></span> <span data-ttu-id="0970d-152">コマンドは、通常、`COMMAND parameter1 parameter2 parameter3` として発行されます。</span><span class="sxs-lookup"><span data-stu-id="0970d-152">A command is typically issued as `COMMAND parameter1 parameter2 parameter3`.</span></span>

<span data-ttu-id="0970d-153">使用できるいくつかの一般的なコマンドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="0970d-153">Here are some common commands you can use:</span></span>

| <span data-ttu-id="0970d-154">コマンド</span><span class="sxs-lookup"><span data-stu-id="0970d-154">Command</span></span> | <span data-ttu-id="0970d-155">説明</span><span class="sxs-lookup"><span data-stu-id="0970d-155">Description</span></span> |
|---------|-------------|
| `ping` | <span data-ttu-id="0970d-156">サーバーへの ping を実行します。</span><span class="sxs-lookup"><span data-stu-id="0970d-156">Ping the server.</span></span> <span data-ttu-id="0970d-157">"PONG" を返します。</span><span class="sxs-lookup"><span data-stu-id="0970d-157">Returns "PONG".</span></span> |
| `set [key] [value]` | <span data-ttu-id="0970d-158">キャッシュのキー/値を設定します。</span><span class="sxs-lookup"><span data-stu-id="0970d-158">Sets a key/value in the cache.</span></span> <span data-ttu-id="0970d-159">成功したら "OK" を返します。</span><span class="sxs-lookup"><span data-stu-id="0970d-159">Returns "OK" on success.</span></span> |
| `get [key]` | <span data-ttu-id="0970d-160">キャッシュから値を取得します。</span><span class="sxs-lookup"><span data-stu-id="0970d-160">Gets a value from the cache.</span></span> |
| `exists [key]` | <span data-ttu-id="0970d-161">**キー**がキャッシュに存在する場合は "1"、存在しない場合は "0" を返します。</span><span class="sxs-lookup"><span data-stu-id="0970d-161">Returns '1' if the **key** exists in the cache, '0' if it doesn't.</span></span> |
| `type [key]` | <span data-ttu-id="0970d-162">指定された**キー**の値に関連付けられている型を返します。</span><span class="sxs-lookup"><span data-stu-id="0970d-162">Returns the type associated to the value for the given **key**.</span></span> |
| `incr [key]` | <span data-ttu-id="0970d-163">**キー**に関連付けられている指定された値を "1" だけ増分します。</span><span class="sxs-lookup"><span data-stu-id="0970d-163">Increment the given value associated with **key** by '1'.</span></span> <span data-ttu-id="0970d-164">値は整数または倍精度値にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-164">The value must be an integer or double value.</span></span> <span data-ttu-id="0970d-165">新しい値を返します。</span><span class="sxs-lookup"><span data-stu-id="0970d-165">This returns the new value.</span></span> |
| `incrby [key] [amount]` | <span data-ttu-id="0970d-166">**キー**に関連付けられている指定された値を、指定された量だけ増分します。</span><span class="sxs-lookup"><span data-stu-id="0970d-166">Increment the given value associated with **key** by the specified amount.</span></span> <span data-ttu-id="0970d-167">値は整数または倍精度値にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-167">The value must be an integer or double value.</span></span> <span data-ttu-id="0970d-168">新しい値を返します。</span><span class="sxs-lookup"><span data-stu-id="0970d-168">This returns the new value.</span></span> |
| `del [key]` | <span data-ttu-id="0970d-169">**キー**に関連付けられている値を削除します。</span><span class="sxs-lookup"><span data-stu-id="0970d-169">Deletes the value associated with the **key**.</span></span> |
| `flushdb` | <span data-ttu-id="0970d-170">データベース内の "_すべての_" キーと値を削除します。</span><span class="sxs-lookup"><span data-stu-id="0970d-170">Delete _all_ keys and values in the database.</span></span> |

<span data-ttu-id="0970d-171">Redis には、これらのコマンドを直接試すために使用できるコマンドライン ツール (**redis-cli**) が用意されています。</span><span class="sxs-lookup"><span data-stu-id="0970d-171">Redis has a command-line tool (**redis-cli**) you can use to experiment directly with these commands.</span></span> <span data-ttu-id="0970d-172">次に例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="0970d-172">Here are some examples.</span></span>

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

<span data-ttu-id="0970d-173">以下に、`INCR` コマンドの操作例を示します。</span><span class="sxs-lookup"><span data-stu-id="0970d-173">Here's an example of working with the `INCR` commands.</span></span> <span data-ttu-id="0970d-174">これらは、キャッシュを使用している "_複数のアプリケーションにわたる_" アトミック増分を提供するので便利です。</span><span class="sxs-lookup"><span data-stu-id="0970d-174">These are convenient because they provide atomic increments _across multiple applications_ that are using the cache.</span></span>

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

### <a name="adding-an-expiration-time-to-values"></a><span data-ttu-id="0970d-175">値への有効期限の追加</span><span class="sxs-lookup"><span data-stu-id="0970d-175">Adding an expiration time to values</span></span>

<span data-ttu-id="0970d-176">キャッシュは、よく使用される値をメモリに格納することができるため、重要です。</span><span class="sxs-lookup"><span data-stu-id="0970d-176">Caching is important because it allows us to store commonly used values in memory.</span></span> <span data-ttu-id="0970d-177">ただし、値が古くなったときに期限切れにする手段も必要です。</span><span class="sxs-lookup"><span data-stu-id="0970d-177">However, we also need a way to expire values when they are stale.</span></span> <span data-ttu-id="0970d-178">Redis では、キーに _Time to Live_ (TTL) を適用することによって、それが行われます。</span><span class="sxs-lookup"><span data-stu-id="0970d-178">In Redis this is done by applying a _time to live_ (TTL) to a key.</span></span>

<span data-ttu-id="0970d-179">TTL が経過すると、DEL コマンドが発行されたかのように、キーが自動的に削除されます。</span><span class="sxs-lookup"><span data-stu-id="0970d-179">When the TTL elapses, the key is automatically deleted, exactly as if the DEL command were issued.</span></span> <span data-ttu-id="0970d-180">TTL の有効期限には、以下の注意事項があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-180">Here are some notes on TTL expirations.</span></span>

- <span data-ttu-id="0970d-181">有効期限は、秒またはミリ秒の精度で設定することができます。</span><span class="sxs-lookup"><span data-stu-id="0970d-181">Expirations can be set using seconds or milliseconds precision.</span></span>
- <span data-ttu-id="0970d-182">有効期限切れの時刻の精度は、常に 1 ミリ秒です。</span><span class="sxs-lookup"><span data-stu-id="0970d-182">The expire time resolution is always 1 millisecond.</span></span>
- <span data-ttu-id="0970d-183">期限切れに関する情報は、ディスクに複製され、永続的に保管されます。Redis サーバーが停止しているときは、時間は仮想的に経過します (つまり、キーが期限切れになる日付が Redis によって保存されます)。</span><span class="sxs-lookup"><span data-stu-id="0970d-183">Information about expires are replicated and persisted on disk, the time virtually passes when your Redis server remains stopped (this means that Redis saves the date at which a key will expire).</span></span>

<span data-ttu-id="0970d-184">次に、有効期間の例を示します。</span><span class="sxs-lookup"><span data-stu-id="0970d-184">Here is an example of an expiration:</span></span>

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

## <a name="accessing-a-redis-cache-from-a-client"></a><span data-ttu-id="0970d-185">クライアントから Redis キャッシュへのアクセス</span><span class="sxs-lookup"><span data-stu-id="0970d-185">Accessing a Redis cache from a client</span></span>

<span data-ttu-id="0970d-186">Azure Redis Cache インスタンスに接続するには、いくつかの情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="0970d-186">To connect to an Azure Redis Cache instance, you'll need several pieces of information.</span></span> <span data-ttu-id="0970d-187">クライアントでホスト名、ポート、キャッシュのアクセス キーが必要になります。</span><span class="sxs-lookup"><span data-stu-id="0970d-187">Clients need the host name, port, and an access key for the cache.</span></span> <span data-ttu-id="0970d-188">この情報は、Azure portal で **[設定]、[アクセス キー]** ページの順に進み、取得できます。</span><span class="sxs-lookup"><span data-stu-id="0970d-188">You can retrieve this information in the Azure portal through the **Settings > Access Keys** page.</span></span> 

- <span data-ttu-id="0970d-189">ホスト名は、キャッシュのパブリック インターネット アドレスです。このアドレスは、キャッシュの名前を使用して作成されたものです。</span><span class="sxs-lookup"><span data-stu-id="0970d-189">The host name is the public Internet address of your cache, which was created using the name of the cache.</span></span> <span data-ttu-id="0970d-190">たとえば、`sportsresults.redis.cache.windows.net` のようなものです。</span><span class="sxs-lookup"><span data-stu-id="0970d-190">For example `sportsresults.redis.cache.windows.net`.</span></span>

- <span data-ttu-id="0970d-191">アクセス キーは、キャッシュのパスワードとして動作します。</span><span class="sxs-lookup"><span data-stu-id="0970d-191">The access key acts as a password for your cache.</span></span> <span data-ttu-id="0970d-192">プライマリとセカンダリの 2 つのキーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0970d-192">There are two keys created: primary and secondary.</span></span> <span data-ttu-id="0970d-193">どちらのキーも使用できますが、2 つ用意されているのは、プライマリ キーを変更する必要があるときのためです。</span><span class="sxs-lookup"><span data-stu-id="0970d-193">You can use either key, two are provided in case you need to change the primary key.</span></span> <span data-ttu-id="0970d-194">すべてのクライアントをセカンダリ キーに切り替えて、プライマリ キーを再生成することができます。</span><span class="sxs-lookup"><span data-stu-id="0970d-194">You can switch all of your clients to the secondary key, and regenerate the primary key.</span></span> <span data-ttu-id="0970d-195">こうすると、元のプライマリ キーを使用しているすべてのアプリケーションがブロックされます。</span><span class="sxs-lookup"><span data-stu-id="0970d-195">This would block any applications using the original primary key.</span></span> <span data-ttu-id="0970d-196">Microsoft では、個人用のパスワードと同じように、定期的にキーを再生成することを推奨しています。</span><span class="sxs-lookup"><span data-stu-id="0970d-196">Microsoft recommends periodically regenerating the keys - much like you would your personal passwords.</span></span>

> [!WARNING]
> <span data-ttu-id="0970d-197">アクセス キーは機密情報であると見なして、パスワードと同じように取り扱う必要があります。</span><span class="sxs-lookup"><span data-stu-id="0970d-197">Your access keys should be considered confidential information, treat them like you would a password.</span></span> <span data-ttu-id="0970d-198">アクセス キーを入手した人は、キャッシュに対して任意の操作を実行できてしまいます。</span><span class="sxs-lookup"><span data-stu-id="0970d-198">Anyone who has an access key can perform any operation on your cache!</span></span>
