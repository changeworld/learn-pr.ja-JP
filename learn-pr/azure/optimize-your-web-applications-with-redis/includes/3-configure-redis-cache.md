<span data-ttu-id="13708-101">スポーツ統計情報の開発チームにより、キャッシュが最近要求されたデータのパフォーマンスを大幅に向上させる可能性があると判断されました。</span><span class="sxs-lookup"><span data-stu-id="13708-101">Your sports statistics development team has decided that caching could dramatically improve performance for recently requested data.</span></span> <span data-ttu-id="13708-102">次の手順として、Azure Redis Cache インスタンスを作成および構成します。</span><span class="sxs-lookup"><span data-stu-id="13708-102">Your next step is to create and configure an Azure Redis Cache instance.</span></span>

### <a name="create-and-configure-the-azure-redis-cache-instance"></a><span data-ttu-id="13708-103">Azure Redis Cache インスタンスの作成および構成</span><span class="sxs-lookup"><span data-stu-id="13708-103">Create and configure the Azure Redis Cache instance</span></span>

1. <span data-ttu-id="13708-104">Azure Redis Cache インスタンスは、Azure portal のデータベース リソースから追加することできます。</span><span class="sxs-lookup"><span data-stu-id="13708-104">You can add an Azure Redis Cache instance from the database resources in the Azure portal.</span></span>

1. <span data-ttu-id="13708-105">グローバルに一意な名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="13708-105">Provide a globally unique name.</span></span> <span data-ttu-id="13708-106">この名前は、1 から 63 文字の文字列で、数字、英字、'-' 文字のみを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="13708-106">The name must be a string between 1 and 63 characters and include only numbers, letters, and the '-' character.</span></span> <span data-ttu-id="13708-107">キャッシュ名の先頭と末尾には '-' 文字を使用できません。また、連続する '-' 文字は無効です。</span><span class="sxs-lookup"><span data-stu-id="13708-107">The cache name can't start or end with the '-' character, and consecutive '-' characters aren't valid.</span></span>

1. <span data-ttu-id="13708-108">この新しい Azure Redis Cache インスタンスが作成されるサブスクリプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="13708-108">Provide the subscription under which this new Azure Redis Cache instance is created.</span></span>

1. <span data-ttu-id="13708-109">キャッシュを作成するリソース グループに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="13708-109">Name the resource group in which to create your cache.</span></span> <span data-ttu-id="13708-110">アプリのすべてのリソースを 1 つのグループ内に配置すると、それらを一緒に管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="13708-110">By putting all the resources for an app in a group, you can manage them together.</span></span>

1. <span data-ttu-id="13708-111">データのコンシューマーに近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="13708-111">Choose a location that is close to the consumer of the data.</span></span>

    > [!NOTE]
    > <span data-ttu-id="13708-112">キャッシュは、データ ストアよりもデータ コンシューマーに近い方がはるかに重要です。</span><span class="sxs-lookup"><span data-stu-id="13708-112">It's far more important that the cache is close to the data consumer than the data store.</span></span>

1. <span data-ttu-id="13708-113">価格レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="13708-113">Select the pricing tier.</span></span> <span data-ttu-id="13708-114">データの永続化を活用するには、Premium レベルを選択する必要があることを念頭に置いておいてください。</span><span class="sxs-lookup"><span data-stu-id="13708-114">Remember, to take advantage of data persistence, you'll need to choose a premium tier.</span></span>

    - <span data-ttu-id="13708-115">Premium レベルでは、ディザスター リカバリーの提供のために、次の 2 つのうちの 1 つの方法でデータを永続化できます。</span><span class="sxs-lookup"><span data-stu-id="13708-115">The premium tier allows you to persist data in one of two ways to provide disaster recovery:</span></span>

        - <span data-ttu-id="13708-116">RDB 永続化では、定期的にスナップショットが撮られます。エラーが発生した場合には、このスナップショットを使用してキャッシュを再構築できます。</span><span class="sxs-lookup"><span data-stu-id="13708-116">RDB persistence takes a periodic snapshot and can rebuild the cache using the snapshot in case of failure.</span></span>

            ![新しい Redis Cache インスタンスの RDB 永続化オプションを示す Azure portal のスクリーンショット。](../media/3-redis-persistence-1.png)

        - <span data-ttu-id="13708-118">AOF 永続化では、すべての書き込み操作がログに最低 1 秒に一度保存されます。</span><span class="sxs-lookup"><span data-stu-id="13708-118">AOF persistence saves every write operation to a log that is saved at least once per second.</span></span> <span data-ttu-id="13708-119">これでは、ファイル サイズが RDB よりも大きくなりますが、データの損失は少ないです。</span><span class="sxs-lookup"><span data-stu-id="13708-119">This creates bigger files than RDB but has less data loss.</span></span>

            ![新しい Redis Cache インスタンスの AOF 永続化オプションを示す Azure portal のスクリーンショット。](../media/3-redis-persistence-2.png)

    - <span data-ttu-id="13708-121">仮想ネットワークを使用して、キャッシュをセキュリティ保護します。</span><span class="sxs-lookup"><span data-stu-id="13708-121">Secure your cache with a virtual network.</span></span>
      <span data-ttu-id="13708-122">Premium レベルの Redis Cache がある場合、クラウドの仮想ネットワークにそれをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="13708-122">If you have a premium tier Redis cache, you can deploy it to a virtual network in the cloud.</span></span> <span data-ttu-id="13708-123">キャッシュは同じ仮想ネットワーク内の他の仮想マシンやアプリケーションのみが使用できます。</span><span class="sxs-lookup"><span data-stu-id="13708-123">Your cache will be available to only other virtual machines and applications in the same virtual network.</span></span>

    - <span data-ttu-id="13708-124">クラスタリングでキャッシュを分散します。</span><span class="sxs-lookup"><span data-stu-id="13708-124">Distribute your cache with clustering.</span></span>
      <span data-ttu-id="13708-125">Premium レベルの Redis Cache では、クラスタリングを実装して、複数のノードに自動的にデータセットが分割されるようにできます。</span><span class="sxs-lookup"><span data-stu-id="13708-125">With a premium tier Redis cache, you can implement clustering to automatically split your dataset among multiple nodes.</span></span> <span data-ttu-id="13708-126">クラスタリングを実装するには、最大を 10 とし、シャードの数値を指定します。</span><span class="sxs-lookup"><span data-stu-id="13708-126">To implement clustering, you specify the number of shards to a maximum of 10.</span></span> <span data-ttu-id="13708-127">これのコストは、元のノードにシャード数を掛けたものです。</span><span class="sxs-lookup"><span data-stu-id="13708-127">The cost is the cost of the original node multiplied by the number of shards.</span></span>

    - <span data-ttu-id="13708-128">Azure Managed Cache Service から移行します。</span><span class="sxs-lookup"><span data-stu-id="13708-128">Migrate from Azure Managed Cache Service.</span></span>
      <span data-ttu-id="13708-129">Azure Managed Cache Service を使用するアプリケーションの Azure Redis Cache への移行は、使用されている Managed Cache Service の機能によっては、最小限の変更をアプリケーションに対して行うだけで実現できます。</span><span class="sxs-lookup"><span data-stu-id="13708-129">Depending on the Managed Cache Service features you use, migrating applications that use Azure Managed Cache Service to Azure Redis Cache can be accomplished with minimal changes to your application.</span></span> <span data-ttu-id="13708-130">API は正確には同じではありませんが、類似しています。</span><span class="sxs-lookup"><span data-stu-id="13708-130">While the APIs aren't exactly the same, they're similar.</span></span> <span data-ttu-id="13708-131">Managed Cache Service を使用してキャッシュにアクセスする既存のコードの多くは、最小限の変更で再利用できます。</span><span class="sxs-lookup"><span data-stu-id="13708-131">Much of your existing code that accesses the cache with Managed Cache Service can be reused with minimal changes.</span></span>

### <a name="configure-your-client"></a><span data-ttu-id="13708-132">クライアントの構成</span><span class="sxs-lookup"><span data-stu-id="13708-132">Configure your client</span></span>

<span data-ttu-id="13708-133">クライアント アプリケーションを構成して、Redis Cache を使用できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="13708-133">Client applications need to be configured to use the Redis cache.</span></span> <span data-ttu-id="13708-134">.NET 言語用に人気のある高性能な Redis クライアントは **StackExchange.Redis** です。</span><span class="sxs-lookup"><span data-stu-id="13708-134">A popular high-performance Redis client for the .NET language is **StackExchange.Redis**.</span></span> <span data-ttu-id="13708-135">Visual Studio で、**NuGet Package Manager** で、**Install-Package** コマンドを使用して Redis クライアントを追加します。</span><span class="sxs-lookup"><span data-stu-id="13708-135">You add the Redis client using the **NuGet Package Manager** in Visual Studio using the **Install-Package** command.</span></span>

<span data-ttu-id="13708-136">次いで、キャッシュに接続するようにクライアントを構成します。</span><span class="sxs-lookup"><span data-stu-id="13708-136">You then need to configure your client to connect to your cache.</span></span> <span data-ttu-id="13708-137">ホスト名とプライマリまたはセカンダリ キーの設定を含む、ファイル拡張子が **.config** のテキスト ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="13708-137">Create a text file with a **.config** file extension that contains the settings for the host name and primary, or secondary, key.</span></span> <span data-ttu-id="13708-138">ファイルの構造は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="13708-138">The file should have the following structure:</span></span>

```XML
    <appSettings>
        <add key="CacheConnection" value="HOST-NAME.redis.cache.windows.net,abortConnect=false,ssl=true,password=PRIMARY-KEY"/>
    </appSettings>
```

<span data-ttu-id="13708-139">次いで、**App.config** ファイルを編集して更新し、作成した config ファイルを参照する **appSettings** ファイル属性が含まれるようにします。</span><span class="sxs-lookup"><span data-stu-id="13708-139">You will then edit your **App.config** file and update it to include an **appSettings** file attribute that references the config file that you just created.</span></span>

### <a name="what-are-access-keys"></a><span data-ttu-id="13708-140">アクセス キーとは</span><span class="sxs-lookup"><span data-stu-id="13708-140">What are access keys?</span></span>

<span data-ttu-id="13708-141">Azure Redis Cache のインスタンスに接続するときには、キャッシュ クライアントにキャッシュのホスト名、ポート、およびキーが必要です。</span><span class="sxs-lookup"><span data-stu-id="13708-141">When connecting to an Azure Redis Cache instance, cache clients need the host name, ports, and a key for the cache.</span></span> <span data-ttu-id="13708-142">この情報は、Azure portal で取得できます。</span><span class="sxs-lookup"><span data-stu-id="13708-142">You can retrieve this information in the Azure portal.</span></span>

### <a name="connection-settings"></a><span data-ttu-id="13708-143">接続の設定</span><span class="sxs-lookup"><span data-stu-id="13708-143">Connection settings</span></span>

<span data-ttu-id="13708-144">Azure Redis Cache に接続するには、ホスト名とアクセス キーが必要です。</span><span class="sxs-lookup"><span data-stu-id="13708-144">To connect to your Azure Redis Cache, you'll need the host name and access key.</span></span> <span data-ttu-id="13708-145">ホスト名とは、*sportsresults.redis.cache.windows.net* など、ご自分のキャッシュのインターネット アドレスです。</span><span class="sxs-lookup"><span data-stu-id="13708-145">The host name is the internet address of your cache, for example *sportsresults.redis.cache.windows.net*.</span></span> <span data-ttu-id="13708-146">アクセス キーは、ご自分のキャッシュのパスワードとして動作します。</span><span class="sxs-lookup"><span data-stu-id="13708-146">The access key acts as a password for your cache.</span></span> <span data-ttu-id="13708-147">アクセス キーには、プライマリとセカンダリがあります。</span><span class="sxs-lookup"><span data-stu-id="13708-147">There are primary and secondary access keys.</span></span> <span data-ttu-id="13708-148">いずれのキーも使用できますが、2 つ用意されているのは、プライマリ キーを変更する必要があるときのためです。</span><span class="sxs-lookup"><span data-stu-id="13708-148">You can use either key, but two are provided in case you need to change the primary key.</span></span> <span data-ttu-id="13708-149">プライマリ キーを変更するときは、サービスが失われることがないよう、すべてのクライアントをセカンダリ キーに切り替えてから行います。</span><span class="sxs-lookup"><span data-stu-id="13708-149">You can switch all of your clients to the secondary key and then make the change to the primary key, which would prevent any loss of service.</span></span>
