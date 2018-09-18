<span data-ttu-id="34f67-101">柔軟なデータ型および地理空間のサポートによるオンプレミスの PostgreSQL リレーショナル データベースを現在使用しているとします。</span><span class="sxs-lookup"><span data-stu-id="34f67-101">Let's assume that you're currently using an on-premise PostgreSQL relational database using flexible data types and geospatial support.</span></span> <span data-ttu-id="34f67-102">会社はデータベースをスケーリングする必要があり、拡張を考えています。</span><span class="sxs-lookup"><span data-stu-id="34f67-102">Your company is looking at expanding, requiring the database to scale.</span></span> <span data-ttu-id="34f67-103">追加のハードウェアに投資する代わりに、あなたは最適なクラウド ホスト型データベース サービスを探す任務を負い、Azure Database for PostgreSQL サーバーを使用することに決めました。</span><span class="sxs-lookup"><span data-stu-id="34f67-103">As an alternative to investing in additional hardware, you are tasked with finding an optimal cloud-hosted database offering and you have decided to use an Azure Database for PostgreSQL server.</span></span>

## <a name="what-is-an-azure-database-for-postgresql-server"></a><span data-ttu-id="34f67-104">Azure Database for PostgreSQL サーバーとは</span><span class="sxs-lookup"><span data-stu-id="34f67-104">What is an Azure Database for PostgreSQL server?</span></span>

<span data-ttu-id="34f67-105">PostgreSQL サーバーは、1 つ以上のデータベースの中央管理ポイントです。</span><span class="sxs-lookup"><span data-stu-id="34f67-105">The PostgreSQL server is a central administration point for one or more databases.</span></span> <span data-ttu-id="34f67-106">Azure の PostgreSQL サービスは、パフォーマンスを保証する管理対象リソースであり、アクセスと機能をサーバー レベルで提供します。</span><span class="sxs-lookup"><span data-stu-id="34f67-106">The PostgreSQL service in Azure is a managed resource that provides performance guarantees, and provides access and features at the server level.</span></span>

<span data-ttu-id="34f67-107">**Azure Database for PostgreSQL** サーバーは、データベースの親_リソース_です。</span><span class="sxs-lookup"><span data-stu-id="34f67-107">An **Azure Database for PostgreSQL** server is the parent _resource_ for a database.</span></span> <span data-ttu-id="34f67-108">_リソース_は、Azure を通じて管理できる要素です。</span><span class="sxs-lookup"><span data-stu-id="34f67-108">A _resource_ is a manageable item that is available through Azure.</span></span> <span data-ttu-id="34f67-109">このリソースを作成すると、サーバー インスタンスを構成できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-109">Creating this resource allows you to configure your server instance.</span></span>

### <a name="what-is-an-azure-database-for-postgresql-server-resource"></a><span data-ttu-id="34f67-110">Azure Database for PostgreSQL サーバー リソースとは</span><span class="sxs-lookup"><span data-stu-id="34f67-110">What is an Azure Database for PostgreSQL server resource?</span></span>

<span data-ttu-id="34f67-111">Azure Database for PostgreSQL サーバー リソースは、その存続期間を通してサーバーとデータベースに強力な影響を与えるコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="34f67-111">An Azure Database for PostgreSQL server resource is a container with strong lifetime implications for your server and databases.</span></span> <span data-ttu-id="34f67-112">サーバー リソースが削除されると、データベースもすべて削除されます。</span><span class="sxs-lookup"><span data-stu-id="34f67-112">If the server resource is deleted, all databases are also deleted.</span></span> <span data-ttu-id="34f67-113">親に属しているすべてのリソースが同じリージョンでホストされていることに留意してください。</span><span class="sxs-lookup"><span data-stu-id="34f67-113">Keep in mind that all resources belonging to the parent are hosted in the same region.</span></span>

<span data-ttu-id="34f67-114">サーバー リソース名は、サーバー エンドポイント名の定義に使用されます。</span><span class="sxs-lookup"><span data-stu-id="34f67-114">The server resource name is used to define the server endpoint name.</span></span> <span data-ttu-id="34f67-115">たとえば、リソース名が **mypgsqlserver** の場合、サーバー名は **mypgsqlserver.postgres.database.azure.com** になります。</span><span class="sxs-lookup"><span data-stu-id="34f67-115">For example, if the resource name is **mypgsqlserver** then the server name becomes **mypgsqlserver.postgres.database.azure.com**</span></span>

<span data-ttu-id="34f67-116">また、サーバー リソースは、データベースに適用される管理ポリシーの__接続スコープ__も提供します。</span><span class="sxs-lookup"><span data-stu-id="34f67-116">The server resource also provides the __connection scope__ for management policies that apply to its database.</span></span> <span data-ttu-id="34f67-117">たとえば、ログイン、ファイアウォール、ユーザー、ロール、構成などです。</span><span class="sxs-lookup"><span data-stu-id="34f67-117">For example, login, firewall, users, roles, and configuration.</span></span>

<span data-ttu-id="34f67-118">PostgreSQL のオープン ソース バージョンと同様に、このサーバーはいくつかのバージョンが用意されていて、拡張機能のインストールが可能です。</span><span class="sxs-lookup"><span data-stu-id="34f67-118">Just like the open-source version of PostgreSQL, the server is available in several versions and allows for the installation of extensions.</span></span> <span data-ttu-id="34f67-119">インストールするサーバー バージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-119">You'll choose which server version to install.</span></span>

> [!NOTE]
> <span data-ttu-id="34f67-120">拡張機能により、複数の SQL オブジェクトを 1 つのパッケージにまとめて、1 つのコマンドで読み込んだり削除したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="34f67-120">Extensions allow for bundling multiple SQL objects together in a single package that can be loaded or removed with a single command.</span></span> <span data-ttu-id="34f67-121">拡張機能の例としては、自動暗号化パスワードのデータ型を提供する ```chkpass``` があります。</span><span class="sxs-lookup"><span data-stu-id="34f67-121">An example of >an extension is ```chkpass```, which provides a data type for auto-encrypted passwords.</span></span>

## <a name="pricing-tiers"></a><span data-ttu-id="34f67-122">価格レベル</span><span class="sxs-lookup"><span data-stu-id="34f67-122">Pricing tiers</span></span>

<span data-ttu-id="34f67-123">Azure Database for PostgreSQL では、コンピューティング能力やストレージなどのパラメーターに基づいて 3 つの価格レベルから選択できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-123">The Azure Database for PostgreSQL provides you with the option to choose from three pricing tiers based on parameters such as compute power and storage.</span></span>

### <a name="basic"></a><span data-ttu-id="34f67-124">Basic</span><span class="sxs-lookup"><span data-stu-id="34f67-124">Basic</span></span>

<span data-ttu-id="34f67-125">**Basic** レベルは、低負荷なコンピューティングと I/O パフォーマンスを必要とするワークロードに最適です。</span><span class="sxs-lookup"><span data-stu-id="34f67-125">The **Basic** tier is ideal for workloads that requires light compute and I/O performance.</span></span> <span data-ttu-id="34f67-126">このレベルでは、次のハードウェアにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="34f67-126">In this tier, you have access to the following hardware:</span></span>

- <span data-ttu-id="34f67-127">1 つまたは 2 つの仮想コアの構成が可能な 2673-Intel E5 v3 (Haswell) 2.4 GHz プロセッサを基盤とする、Gen 4 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="34f67-127">Compute Gen 4 CPUs base on Intel E5-2673 v3 (Haswell) 2.4-GHz processors available as either a 1 or 2 vCore configuration</span></span>
- <span data-ttu-id="34f67-128">1 つまたは 2 つの仮想コアの構成が可能な Intel E5-2673 v4 (Broadwell) 2.3-GHz プロセッサを基盤とする、Gen 5 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="34f67-128">Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available as either a 1 or 2 vCore configuration</span></span>
- <span data-ttu-id="34f67-129">1 TB までのストレージ</span><span class="sxs-lookup"><span data-stu-id="34f67-129">Storage up to 1 TB</span></span>
- <span data-ttu-id="34f67-130">ローカル冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="34f67-130">Locally redundant backup</span></span>

<span data-ttu-id="34f67-131">後でサンプル サーバーを作成するときに、特定のシナリオのサポートを示すために特定の価格レベル設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="34f67-131">You'll use a specific pricing tier setting to illustrate support of a specific scenario when you create an example server later.</span></span> <span data-ttu-id="34f67-132">ただし、運用サーバーでは環境に合わせて価格レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-132">Keep in mind that for production servers, you'll choose a pricing tier to match your environment.</span></span>

### <a name="general-purpose"></a><span data-ttu-id="34f67-133">汎用</span><span class="sxs-lookup"><span data-stu-id="34f67-133">General Purpose</span></span>

<span data-ttu-id="34f67-134">**汎用**レベルは、負荷分散されたコンピューティング、メモリ、スケーラブルな I/O スループットを必要とする大部分のビジネス ワークロードに最適です。</span><span class="sxs-lookup"><span data-stu-id="34f67-134">The **General Purpose** tier is ideal for most business workloads requiring balanced compute and memory with scalable I/O throughput.</span></span> <span data-ttu-id="34f67-135">このレベルでは、次のハードウェアにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="34f67-135">In this tier, you have access to the following hardware:</span></span>

- <span data-ttu-id="34f67-136">2、4、8、18、32 個の仮想コアの構成が可能な 2673-Intel E5 v3 (Haswell) 2.4 GHz プロセッサを基盤とする、Gen 4 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="34f67-136">Compute Gen 4 CPUs base on Intel E5-2673 v3 (Haswell) 2.4-GHz processors available in 2, 4, 8, 18, 32 vCore configurations</span></span>
- <span data-ttu-id="34f67-137">2、4、8、18、32 個の仮想コアの構成が可能な Intel E5-2673 v4 (Broadwell) 2.3-GHz プロセッサを基盤とする、Gen 5 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="34f67-137">Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available in 2, 4, 8, 18, 32 vCore configurations</span></span>
- <span data-ttu-id="34f67-138">4 TB までのストレージ</span><span class="sxs-lookup"><span data-stu-id="34f67-138">Storage up to 4 TB</span></span>
- <span data-ttu-id="34f67-139">ローカル冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="34f67-139">Locally redundant backup</span></span>
- <span data-ttu-id="34f67-140">地理冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="34f67-140">Geographically redundant backup</span></span>

### <a name="memory-optimized"></a><span data-ttu-id="34f67-141">メモリ最適化</span><span class="sxs-lookup"><span data-stu-id="34f67-141">Memory Optimized</span></span>

<span data-ttu-id="34f67-142">**メモリ最適化**レベルは、高速トランザクション処理と高い同時実行性を実現するためのインメモリ パフォーマンスを必要とする、高パフォーマンス データベース ワークロードに最適です。</span><span class="sxs-lookup"><span data-stu-id="34f67-142">The **Memory Optimized** tier is ideal for high-performance database workloads requiring in-memory performance for faster transaction processing and higher concurrency.</span></span> <span data-ttu-id="34f67-143">このレベルでは、次のハードウェアにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="34f67-143">In this tier, you have access to the following hardware:</span></span>

- <span data-ttu-id="34f67-144">2、4、8、16 個の仮想コアの構成が可能な Intel E5-2673 v4 (Broadwell) 2.3-GHz プロセッサを基盤とする、Gen 5 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="34f67-144">Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available in 2, 4, 8, 16 vCore configurations</span></span>
- <span data-ttu-id="34f67-145">4 TB までのストレージ</span><span class="sxs-lookup"><span data-stu-id="34f67-145">Storage up to 4 TB</span></span>
- <span data-ttu-id="34f67-146">ローカル冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="34f67-146">Locally redundant backup</span></span>
- <span data-ttu-id="34f67-147">地理冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="34f67-147">Geographically redundant backup</span></span>

## <a name="steps-to-create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="34f67-148">Azure Database for PostgreSQL サーバーを作成する手順</span><span class="sxs-lookup"><span data-stu-id="34f67-148">Steps to create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="34f67-149">通常、Azure Database for PostgreSQL サーバーは Azure portal を使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="34f67-149">You'll typically create an Azure Database for PostgreSQL server using the Azure portal.</span></span> <span data-ttu-id="34f67-150">それぞれの手順を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="34f67-150">Let's look at the steps you'll take.</span></span>

<span data-ttu-id="34f67-151">まず Azure portal にサインインし、**[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="34f67-151">First you'll sign into the Azure portal, and then you'll click **Create a resource**.</span></span>

<span data-ttu-id="34f67-152">**[データベース]** と **[Azure Database for PostgreSQL]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-152">You'll select **Databases** and **Azure Database for PostgreSQL**.</span></span> <span data-ttu-id="34f67-153">検索機能を使用してこのカテゴリを検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="34f67-153">You can also use the Search functionality to find this category.</span></span>

<span data-ttu-id="34f67-154">ポータルに、ブレードとも呼ばれる PostgreSQL サーバー構成画面が表示されるので、ここで選択を行います。</span><span class="sxs-lookup"><span data-stu-id="34f67-154">The portal will display a PostgreSQL server configuration screen, also called a blade, you make your selection.</span></span>

![Azure portal での PostgreSQL サーバー作成用のユーザー インターフェイス ブレード](../media-draft/4-create-blade.png)

<span data-ttu-id="34f67-156">ブレード内のすべての項目に値を指定する必要があります。では、それぞれ見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="34f67-156">You'll need to give a value to all the items in the blade, so let's have a look at each.</span></span>

### <a name="server-name"></a><span data-ttu-id="34f67-157">サーバー名</span><span class="sxs-lookup"><span data-stu-id="34f67-157">Server name</span></span>

<span data-ttu-id="34f67-158">先ほど、サーバー リソースを作成することに言及しました。</span><span class="sxs-lookup"><span data-stu-id="34f67-158">Recall, earlier we mentioned that you'll create a server resource.</span></span> <span data-ttu-id="34f67-159">サーバー名は、このリソースを指定する項目です。</span><span class="sxs-lookup"><span data-stu-id="34f67-159">The server name is the item that specifies this resource.</span></span> <span data-ttu-id="34f67-160">そのため、サーバーには一意の名前を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="34f67-160">As a result, you'll have to choose a unique name for the server.</span></span> <span data-ttu-id="34f67-161">サーバー名はすべて小文字とする必要があり、数字とハイフンを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="34f67-161">The server name must be all lowercase and can have numbers and hyphens.</span></span>

<span data-ttu-id="34f67-162">たとえば、サーバーに _Adventure Works Tracking_ という名前を付けるとします。</span><span class="sxs-lookup"><span data-stu-id="34f67-162">Let's say you want to name the server _Adventure Works Tracking_.</span></span> <span data-ttu-id="34f67-163">その場合は、名前を ```adventure-works-tracking``` に設定します。</span><span class="sxs-lookup"><span data-stu-id="34f67-163">You would then set up the name as ```adventure-works-tracking```.</span></span> <span data-ttu-id="34f67-164">既に存在する名前と同じ名前のサーバーを作成しようとすると、それに対応するエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="34f67-164">If you try to create a server with the same name that already exists, you'll get an error to that effect.</span></span>

### <a name="subscription"></a><span data-ttu-id="34f67-165">サブスクリプション</span><span class="sxs-lookup"><span data-stu-id="34f67-165">Subscription</span></span>

<span data-ttu-id="34f67-166">サブスクリプション フィールドは課金に使用します。</span><span class="sxs-lookup"><span data-stu-id="34f67-166">The subscription field is used for billing.</span></span> <span data-ttu-id="34f67-167">使用可能なサブスクリプションが複数ある場合は、特定のサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-167">You'll pick a specific subscription in case you have more than one subscription available.</span></span>

### <a name="resource-group"></a><span data-ttu-id="34f67-168">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="34f67-168">Resource group</span></span>

<span data-ttu-id="34f67-169">リソース グループは、サーバーに関連するすべてのリソースを管理するために使用します。</span><span class="sxs-lookup"><span data-stu-id="34f67-169">You'll use a resource group to manage all the resources related to your server.</span></span> <span data-ttu-id="34f67-170">新しいリソース グループを作成するか、既存のリソース グループを再使用できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-170">You can create a new resource group or reuse an existing resource group.</span></span>

### <a name="source"></a><span data-ttu-id="34f67-171">ソース</span><span class="sxs-lookup"><span data-stu-id="34f67-171">Source</span></span>

<span data-ttu-id="34f67-172">既定の _[空白]_ オプションを選択してサーバーを最初から作成するか、または既存のバックアップから作成することができます。</span><span class="sxs-lookup"><span data-stu-id="34f67-172">You can create a server either from scratch by choosing the _Blank_ default option or from an existing backup.</span></span> <span data-ttu-id="34f67-173">[バックアップ] オプションでは、既存の Azure Database for PostgreSQL サーバーの geo バックアップを復元できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-173">The Backup option will give the opportunity restore a geo-backup of an existing Azure Database for PostgreSQL server.</span></span>

### <a name="server-admin-login-name"></a><span data-ttu-id="34f67-174">サーバー管理者ログイン名</span><span class="sxs-lookup"><span data-stu-id="34f67-174">Server admin login name</span></span>

<span data-ttu-id="34f67-175">サーバー管理者ユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="34f67-175">You create the server admin user.</span></span> <span data-ttu-id="34f67-176">新しいサーバー用の管理者ログインとして使用するログイン名を選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-176">Choose a login name to use as an administrator login for the new server.</span></span> <span data-ttu-id="34f67-177">管理者のログイン名に azure_superuser、azure_pg_admin、admin、administrator、root、guest、または public は使用できません。</span><span class="sxs-lookup"><span data-stu-id="34f67-177">The admin login name can't be azure_superuser, azure_pg_admin, admin, administrator, root, guest, or public.</span></span> <span data-ttu-id="34f67-178">pg_ で始めることはできません。</span><span class="sxs-lookup"><span data-stu-id="34f67-178">It can't start with pg_.</span></span> <span data-ttu-id="34f67-179">今後の使用に備えてこの名前を覚えておくか、書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="34f67-179">Remember or write down this name for future use.</span></span>

### <a name="password"></a><span data-ttu-id="34f67-180">パスワード</span><span class="sxs-lookup"><span data-stu-id="34f67-180">Password</span></span>

<span data-ttu-id="34f67-181">上記の管理者ログイン名と一緒に使用するパスワードを選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-181">You choose a password to use with the above administrator login name.</span></span> <span data-ttu-id="34f67-182">パスワードには、英大文字、英小文字、数字 (0 から 9)、英数字以外の文字 (!、$、#、% など) のうち、3 種類の文字が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="34f67-182">Your password must include characters from three of the following categories: English uppercase letters, English lowercase letters, numbers (0 through 9), and non-alphanumeric characters (!, $, #, %, etc.).</span></span> <span data-ttu-id="34f67-183">今後の使用に備えてこのパスワードを覚えておくか、書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="34f67-183">Remember or write down this password for future use.</span></span>

### <a name="confirm-password"></a><span data-ttu-id="34f67-184">パスワードの確認</span><span class="sxs-lookup"><span data-stu-id="34f67-184">Confirm password</span></span>

<span data-ttu-id="34f67-185">確認のためにパスワードを再入力します。</span><span class="sxs-lookup"><span data-stu-id="34f67-185">Retype the password as confirmation.</span></span>

### <a name="location"></a><span data-ttu-id="34f67-186">場所</span><span class="sxs-lookup"><span data-stu-id="34f67-186">Location</span></span>

<span data-ttu-id="34f67-187">場所のオプションを使用すると、サーバーが物理的に作成される場所を指定できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-187">The location option allows you to specify where the server is created physically.</span></span> <span data-ttu-id="34f67-188">自分が住んでいる場所に最も近い地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-188">Choose the geographical location closest to you.</span></span> <span data-ttu-id="34f67-189">実際のシナリオでは、大部分のユーザーに最も近い場所にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="34f67-189">In a real-world scenario, the location should be the location closest to the majority of your users.</span></span>

### <a name="version"></a><span data-ttu-id="34f67-190">バージョン</span><span class="sxs-lookup"><span data-stu-id="34f67-190">Version</span></span>

<span data-ttu-id="34f67-191">使用する PostgreSQL のバージョンを指定できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-191">You can specify the version of PostgreSQL to use.</span></span> <span data-ttu-id="34f67-192">Microsoft では、基本的には PostgreSQL の現在のバージョンとそれより前の 2 つのバージョンがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="34f67-192">Microsoft aims to support the current version and two previous versions of PostgreSQL.</span></span> <span data-ttu-id="34f67-193">運用環境に一致するバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-193">You'll choose a version that matches your production environment.</span></span>

> [!NOTE]
> <span data-ttu-id="34f67-194">詳細については、次の資料を参照してください。</span><span class="sxs-lookup"><span data-stu-id="34f67-194">For more information, see the following sources:</span></span>
> - [<span data-ttu-id="34f67-195">サポートされている PostgreSQL Database バージョン</span><span class="sxs-lookup"><span data-stu-id="34f67-195">Supported PostgreSQL Database Versions</span></span>](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)
> - [<span data-ttu-id="34f67-196">Versioning Policy (バージョン管理ポリシー)</span><span class="sxs-lookup"><span data-stu-id="34f67-196">Versioning Policy</span></span>](https://www.postgresql.org/support/versioning/)

### <a name="pricing-tier"></a><span data-ttu-id="34f67-197">価格レベル</span><span class="sxs-lookup"><span data-stu-id="34f67-197">Pricing tier</span></span>

<span data-ttu-id="34f67-198">サーバーのワークロードをサポートする価格レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="34f67-198">You'll choose a pricing tier that will support your server's workload.</span></span> <span data-ttu-id="34f67-199">選択できるレベルは 3 つあります。</span><span class="sxs-lookup"><span data-stu-id="34f67-199">Recall, you have three tiers to choose from.</span></span> <span data-ttu-id="34f67-200">前述のように、これらの各レベルを使用して、コンピューティングとストレージのオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-200">As we saw earlier, each of these tiers allows you to configure the compute and storage options.</span></span> <span data-ttu-id="34f67-201">ここでは、Basic 価格レベルが選択されていて、Gen 5 コンピューティングを使用し、バックアップ保有期間が 25 日である例が示されています。</span><span class="sxs-lookup"><span data-stu-id="34f67-201">For example, here is an example with the Basic price tier selected, a Gen 5 compute and a 25-day backup retention period.</span></span>

![価格レベル](../media-draft/4-azure-db-pricing-tier.png)

<span data-ttu-id="34f67-203">あとは、入力した値を確認し、後で必要となる可能性のあるすべての項目を書き留め、作成ボタンをクリックしてサーバーを作成するだけです。</span><span class="sxs-lookup"><span data-stu-id="34f67-203">All that is left now is to review the values you entered, make any notes you may need for later, and click the create button to create the server.</span></span>

<span data-ttu-id="34f67-204">サーバーをデプロイするには数分かかります。</span><span class="sxs-lookup"><span data-stu-id="34f67-204">Deploying the server takes a couple of minutes.</span></span> <span data-ttu-id="34f67-205">デプロイが完了すると、通知を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="34f67-205">You'll receive a notification when the deployment is complete.</span></span> <span data-ttu-id="34f67-206">通知から、新しく作成したサーバーに移動できます。</span><span class="sxs-lookup"><span data-stu-id="34f67-206">From the notification, you can navigate to the newly created server.</span></span>

<span data-ttu-id="34f67-207">ここまでは、Azure Database for PostgreSQL を作成する手順を説明しました。</span><span class="sxs-lookup"><span data-stu-id="34f67-207">You've now seen the steps you take to create an Azure Database for PostgreSQL.</span></span> <span data-ttu-id="34f67-208">次の演習では、独自の Azure Database for PostgreSQL サーバーを作成してみます。</span><span class="sxs-lookup"><span data-stu-id="34f67-208">In the next unit, you'll have to opportunity to create your own Azure Database for PostgreSQL server.</span></span>
