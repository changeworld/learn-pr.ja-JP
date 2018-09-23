<span data-ttu-id="efe40-101">柔軟なデータ型および地理空間のサポートによるオンプレミスの PostgreSQL リレーショナル データベースを現在使用しているとします。</span><span class="sxs-lookup"><span data-stu-id="efe40-101">Let's assume that you're currently using an on-premises PostgreSQL relational database using flexible data types and geospatial support.</span></span> <span data-ttu-id="efe40-102">会社は拡張を考えていて、データベースをスケーリングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="efe40-102">Your company is looking at expanding, which requires the database to scale.</span></span> <span data-ttu-id="efe40-103">追加のハードウェアに投資する代わりに、あなたは最適なクラウド ホスト型データベース サービスを探す任務を負いました。</span><span class="sxs-lookup"><span data-stu-id="efe40-103">As an alternative to investing in additional hardware, you're tasked with finding an optimal cloud-hosted database offering.</span></span> <span data-ttu-id="efe40-104">そこで Azure Database for PostgreSQL サーバーを使用することに決めました。</span><span class="sxs-lookup"><span data-stu-id="efe40-104">You've decided to use an Azure Database for PostgreSQL server.</span></span>

## <a name="what-is-an-azure-database-for-postgresql-server"></a><span data-ttu-id="efe40-105">Azure Database for PostgreSQL サーバーとは</span><span class="sxs-lookup"><span data-stu-id="efe40-105">What is an Azure Database for PostgreSQL server?</span></span>

<span data-ttu-id="efe40-106">PostgreSQL サーバーは、1 つ以上のデータベースの中央管理ポイントです。</span><span class="sxs-lookup"><span data-stu-id="efe40-106">The PostgreSQL server is a central administration point for one or more databases.</span></span> <span data-ttu-id="efe40-107">Azure の PostgreSQL サービスは、パフォーマンスを保証する管理対象リソースであり、アクセスと機能をサーバー レベルで提供します。</span><span class="sxs-lookup"><span data-stu-id="efe40-107">The PostgreSQL service in Azure is a managed resource that provides performance guarantees, and provides access and features at the server level.</span></span>

<span data-ttu-id="efe40-108">**Azure Database for PostgreSQL** サーバーは、データベースの親_リソース_です。</span><span class="sxs-lookup"><span data-stu-id="efe40-108">An **Azure Database for PostgreSQL** server is the parent _resource_ for a database.</span></span> <span data-ttu-id="efe40-109">_リソース_は、Azure を通じて管理できる要素です。</span><span class="sxs-lookup"><span data-stu-id="efe40-109">A _resource_ is a manageable item that's available through Azure.</span></span> <span data-ttu-id="efe40-110">このリソースを作成すると、サーバー インスタンスを構成できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-110">Creating this resource allows you to configure your server instance.</span></span>

### <a name="what-is-an-azure-database-for-postgresql-server-resource"></a><span data-ttu-id="efe40-111">Azure Database for PostgreSQL サーバー リソースとは</span><span class="sxs-lookup"><span data-stu-id="efe40-111">What is an Azure Database for PostgreSQL server resource?</span></span>

<span data-ttu-id="efe40-112">Azure Database for PostgreSQL サーバー リソースは、その存続期間を通してサーバーとデータベースに強力な影響を与えるコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="efe40-112">An Azure Database for PostgreSQL server resource is a container with strong lifetime implications for your server and databases.</span></span> <span data-ttu-id="efe40-113">サーバー リソースが削除されると、データベースもすべて削除されます。</span><span class="sxs-lookup"><span data-stu-id="efe40-113">If the server resource is deleted, all databases are also deleted.</span></span> <span data-ttu-id="efe40-114">親に属しているすべてのリソースが同じリージョンでホストされていることに留意してください。</span><span class="sxs-lookup"><span data-stu-id="efe40-114">Keep in mind that all resources belonging to the parent are hosted in the same region.</span></span>

<span data-ttu-id="efe40-115">サーバー リソース名は、サーバー エンドポイント名の定義に使用されます。</span><span class="sxs-lookup"><span data-stu-id="efe40-115">The server resource name is used to define the server endpoint name.</span></span> <span data-ttu-id="efe40-116">たとえば、リソース名が **mypgsqlserver** の場合、サーバー名は **mypgsqlserver.postgres.database.azure.com** になります。</span><span class="sxs-lookup"><span data-stu-id="efe40-116">For example, if the resource name is **mypgsqlserver**, then the server name becomes **mypgsqlserver.postgres.database.azure.com**.</span></span>

<span data-ttu-id="efe40-117">また、サーバー リソースは、データベースに適用される管理ポリシーの__接続スコープ__も提供します。</span><span class="sxs-lookup"><span data-stu-id="efe40-117">The server resource also provides the __connection scope__ for management policies that apply to its database.</span></span> <span data-ttu-id="efe40-118">たとえば、ログイン、ファイアウォール、ユーザー、ロール、構成などです。</span><span class="sxs-lookup"><span data-stu-id="efe40-118">For example: login, firewall, users, roles, and configuration.</span></span>

<span data-ttu-id="efe40-119">PostgreSQL のオープン ソース バージョンと同様に、このサーバーはいくつかのバージョンが用意されていて、拡張機能のインストールが可能です。</span><span class="sxs-lookup"><span data-stu-id="efe40-119">Just like the open-source version of PostgreSQL, the server is available in several versions and allows for the installation of extensions.</span></span> <span data-ttu-id="efe40-120">インストールするサーバー バージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-120">You'll choose which server version to install.</span></span>

> [!NOTE]
> <span data-ttu-id="efe40-121">拡張機能により、複数の SQL オブジェクトを 1 つのパッケージにまとめて、1 つのコマンドで読み込んだり削除したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="efe40-121">Extensions allow for bundling multiple SQL objects together in a single package that can be loaded or removed with a single command.</span></span> <span data-ttu-id="efe40-122">拡張機能の例としては、自動暗号化パスワードのデータ型を提供する `chkpass` があります。</span><span class="sxs-lookup"><span data-stu-id="efe40-122">An example of an extension is `chkpass`, which provides a data type for auto-encrypted passwords.</span></span>

## <a name="pricing-tiers"></a><span data-ttu-id="efe40-123">価格レベル</span><span class="sxs-lookup"><span data-stu-id="efe40-123">Pricing tiers</span></span>

<span data-ttu-id="efe40-124">Azure Database for PostgreSQL では、コンピューティング能力やストレージなどのパラメーターに基づいて 3 つの価格レベルから選択できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-124">Azure Database for PostgreSQL provides you with the option to choose from three pricing tiers based on parameters like compute power and storage.</span></span>

### <a name="basic-tier"></a><span data-ttu-id="efe40-125">Basic レベル</span><span class="sxs-lookup"><span data-stu-id="efe40-125">Basic tier</span></span>

<span data-ttu-id="efe40-126">**Basic** レベルは、低負荷なコンピューティングと I/O パフォーマンスを必要とするワークロードに最適です。</span><span class="sxs-lookup"><span data-stu-id="efe40-126">The **Basic** tier is ideal for workloads that require light compute and I/O performance.</span></span> <span data-ttu-id="efe40-127">このレベルでは、次のハードウェアにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="efe40-127">In this tier, you have access to the following hardware:</span></span>

- <span data-ttu-id="efe40-128">1 つまたは 2 つの仮想コアの構成が可能な 2673-Intel E5 v3 (Haswell) 2.4 GHz プロセッサを基盤とする、Gen 4 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="efe40-128">Compute Gen 4 CPUs base on Intel E5-2673 v3 (Haswell) 2.4-GHz processors available as either a 1 or 2 vCore configuration</span></span>
- <span data-ttu-id="efe40-129">1 つまたは 2 つの仮想コアの構成が可能な Intel E5-2673 v4 (Broadwell) 2.3-GHz プロセッサを基盤とする、Gen 5 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="efe40-129">Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available as either a 1 or 2 vCore configuration</span></span>
- <span data-ttu-id="efe40-130">1 TB までのストレージ</span><span class="sxs-lookup"><span data-stu-id="efe40-130">Storage up to 1 TB</span></span>
- <span data-ttu-id="efe40-131">ローカル冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="efe40-131">Locally redundant backup</span></span>

<span data-ttu-id="efe40-132">後でサンプル サーバーを作成するときに、特定のシナリオのサポートを示すために特定の価格レベル設定を使用します。</span><span class="sxs-lookup"><span data-stu-id="efe40-132">You'll use a specific pricing tier setting to illustrate support of a specific scenario when you create an example server later.</span></span> <span data-ttu-id="efe40-133">ただし、運用サーバーでは環境に合わせて価格レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-133">Keep in mind that for production servers, you'll choose a pricing tier to match your environment.</span></span>

### <a name="general-purpose-tier"></a><span data-ttu-id="efe40-134">汎用レベル</span><span class="sxs-lookup"><span data-stu-id="efe40-134">General Purpose tier</span></span>

<span data-ttu-id="efe40-135">**汎用**レベルは、負荷分散されたコンピューティング、メモリ、スケーラブルな I/O スループットを必要とする大部分のビジネス ワークロードに最適です。</span><span class="sxs-lookup"><span data-stu-id="efe40-135">The **General Purpose** tier is ideal for most business workloads that require balanced compute and memory with scalable I/O throughput.</span></span> <span data-ttu-id="efe40-136">このレベルでは、次のハードウェアにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="efe40-136">In this tier, you have access to the following hardware:</span></span>

- <span data-ttu-id="efe40-137">2、4、8、18、32 個の仮想コアの構成が可能な 2673-Intel E5 v3 (Haswell) 2.4 GHz プロセッサを基盤とする、Gen 4 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="efe40-137">Compute Gen 4 CPUs base on Intel E5-2673 v3 (Haswell) 2.4-GHz processors available in 2, 4, 8, 18, 32 vCore configurations</span></span>
- <span data-ttu-id="efe40-138">2、4、8、18、32 個の仮想コアの構成が可能な Intel E5-2673 v4 (Broadwell) 2.3-GHz プロセッサを基盤とする、Gen 5 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="efe40-138">Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available in 2, 4, 8, 18, 32 vCore configurations</span></span>
- <span data-ttu-id="efe40-139">4 TB までのストレージ</span><span class="sxs-lookup"><span data-stu-id="efe40-139">Storage up to 4 TB</span></span>
- <span data-ttu-id="efe40-140">ローカル冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="efe40-140">Locally redundant backup</span></span>
- <span data-ttu-id="efe40-141">地理冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="efe40-141">Geographically redundant backup</span></span>

### <a name="memory-optimized-tier"></a><span data-ttu-id="efe40-142">メモリ最適化レベル</span><span class="sxs-lookup"><span data-stu-id="efe40-142">Memory Optimized tier</span></span>

<span data-ttu-id="efe40-143">**メモリ最適化**レベルは、高速トランザクション処理と高い同時実行性を実現するためのインメモリ パフォーマンスを必要とする、高パフォーマンス データベース ワークロードに最適です。</span><span class="sxs-lookup"><span data-stu-id="efe40-143">The **Memory Optimized** tier is ideal for high-performance database workloads that require in-memory performance for faster transaction processing and higher concurrency.</span></span> <span data-ttu-id="efe40-144">このレベルでは、次のハードウェアにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="efe40-144">In this tier, you have access to the following hardware:</span></span>

- <span data-ttu-id="efe40-145">2、4、8、16 個の仮想コアの構成が可能な Intel E5-2673 v4 (Broadwell) 2.3-GHz プロセッサを基盤とする、Gen 5 コンピューティング CPU</span><span class="sxs-lookup"><span data-stu-id="efe40-145">Compute Gen 5 CPUs base on Intel E5-2673 v4 (Broadwell) 2.3-GHz processors available in 2, 4, 8, 16 vCore configurations</span></span>
- <span data-ttu-id="efe40-146">4 TB までのストレージ</span><span class="sxs-lookup"><span data-stu-id="efe40-146">Storage up to 4 TB</span></span>
- <span data-ttu-id="efe40-147">ローカル冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="efe40-147">Locally redundant backup</span></span>
- <span data-ttu-id="efe40-148">地理冗長バックアップ</span><span class="sxs-lookup"><span data-stu-id="efe40-148">Geographically redundant backup</span></span>

## <a name="steps-to-create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="efe40-149">Azure Database for PostgreSQL サーバーを作成する手順</span><span class="sxs-lookup"><span data-stu-id="efe40-149">Steps to create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="efe40-150">通常、Azure Database for PostgreSQL サーバーは Azure portal を使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="efe40-150">You'll typically create an Azure Database for PostgreSQL server using the Azure portal.</span></span> <span data-ttu-id="efe40-151">その手順を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="efe40-151">Let's look at the steps you'll take.</span></span>

<span data-ttu-id="efe40-152">まず Azure portal にサインインし、**[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="efe40-152">First, you'll sign in to the Azure portal, and then you'll click **Create a resource**.</span></span>

<span data-ttu-id="efe40-153">**[データベース]** と **[Azure Database for PostgreSQL]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-153">You'll select **Databases** and **Azure Database for PostgreSQL**.</span></span> <span data-ttu-id="efe40-154">**検索**機能を使用してこのカテゴリを検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="efe40-154">You can also use the **Search** functionality to find this category.</span></span>

<span data-ttu-id="efe40-155">ポータルに、ブレードとも呼ばれる PostgreSQL サーバー構成画面が表示されるので、ここで選択を行います。</span><span class="sxs-lookup"><span data-stu-id="efe40-155">The portal will display a PostgreSQL server configuration screen, also called a blade, and you make your selection.</span></span>

![新しい PostgreSQL データベースの作成ブレードを示す Azure portal のスクリーンショット](../media/4-create-blade.png)

<span data-ttu-id="efe40-157">ブレード内のすべての項目に値を指定する必要があります。では、それぞれ見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="efe40-157">You'll need to give a value to all the items in the blade, so let's have a look at each.</span></span>

### <a name="server-name"></a><span data-ttu-id="efe40-158">サーバー名</span><span class="sxs-lookup"><span data-stu-id="efe40-158">Server name</span></span>

<span data-ttu-id="efe40-159">先ほど、サーバー リソースを作成することに言及しました。</span><span class="sxs-lookup"><span data-stu-id="efe40-159">Earlier we mentioned that you'll create a server resource.</span></span> <span data-ttu-id="efe40-160">サーバー名は、このリソースを指定する項目です。</span><span class="sxs-lookup"><span data-stu-id="efe40-160">The server name is the item that specifies this resource.</span></span> <span data-ttu-id="efe40-161">そのため、サーバーには一意の名前を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="efe40-161">As a result, you'll have to choose a unique name for the server.</span></span> <span data-ttu-id="efe40-162">サーバー名はすべて小文字とする必要があり、数字とハイフンを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="efe40-162">The server name must be all lowercase and can have numbers and hyphens.</span></span>

<span data-ttu-id="efe40-163">たとえば、サーバーに _Adventure Works Tracking_ という名前を付けるとします。</span><span class="sxs-lookup"><span data-stu-id="efe40-163">Let's say you want to name the server _Adventure Works Tracking_.</span></span> <span data-ttu-id="efe40-164">その場合は、名前を `adventure-works-tracking` に設定します。</span><span class="sxs-lookup"><span data-stu-id="efe40-164">You would then set up the name as `adventure-works-tracking`.</span></span> <span data-ttu-id="efe40-165">既に存在する名前のサーバーを作成しようとすると、それに対応するエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="efe40-165">If you try to create a server with a name that already exists, you'll get an error to that effect.</span></span>

### <a name="subscription"></a><span data-ttu-id="efe40-166">サブスクリプション</span><span class="sxs-lookup"><span data-stu-id="efe40-166">Subscription</span></span>

<span data-ttu-id="efe40-167">サブスクリプション フィールドは課金に使用します。</span><span class="sxs-lookup"><span data-stu-id="efe40-167">The subscription field is used for billing.</span></span> <span data-ttu-id="efe40-168">使用可能なサブスクリプションが複数ある場合は、特定のサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-168">You'll pick a specific subscription in case you have more than one subscription available.</span></span>

### <a name="resource-group"></a><span data-ttu-id="efe40-169">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="efe40-169">Resource group</span></span>

<span data-ttu-id="efe40-170">リソース グループは、サーバーに関連するすべてのリソースを管理するために使用します。</span><span class="sxs-lookup"><span data-stu-id="efe40-170">You'll use a resource group to manage all the resources related to your server.</span></span> <span data-ttu-id="efe40-171">新しいリソース グループを作成するか、既存のリソース グループを再使用できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-171">You can create a new resource group or reuse an existing resource group.</span></span>

### <a name="source"></a><span data-ttu-id="efe40-172">ソース</span><span class="sxs-lookup"><span data-stu-id="efe40-172">Source</span></span>

<span data-ttu-id="efe40-173">既定の _[空白]_ オプションを選択してサーバーを最初から作成するか、または既存のバックアップから作成することができます。</span><span class="sxs-lookup"><span data-stu-id="efe40-173">You can create a server either from scratch by choosing the _Blank_ default option or from an existing backup.</span></span> <span data-ttu-id="efe40-174">**[バックアップ]** オプションでは、既存の Azure Database for PostgreSQL サーバーの geo バックアップを復元できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-174">The **Backup** option will give you the opportunity restore a geo-backup of an existing Azure Database for PostgreSQL server.</span></span>

### <a name="server-admin-login-name"></a><span data-ttu-id="efe40-175">サーバー管理者ログイン名</span><span class="sxs-lookup"><span data-stu-id="efe40-175">Server admin login name</span></span>

<span data-ttu-id="efe40-176">サーバー管理者ユーザーを作成します。</span><span class="sxs-lookup"><span data-stu-id="efe40-176">You create the server admin user.</span></span> <span data-ttu-id="efe40-177">新しいサーバー用の管理者ログインとして使用するログイン名を選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-177">Choose a login name to use as an administrator login for the new server.</span></span> <span data-ttu-id="efe40-178">管理者のログイン名に azure_superuser、azure_pg_admin、admin、administrator、root、guest、または public は使用できません。</span><span class="sxs-lookup"><span data-stu-id="efe40-178">The admin login name can't be azure_superuser, azure_pg_admin, admin, administrator, root, guest, or public.</span></span> <span data-ttu-id="efe40-179">pg_ で始めることはできません。</span><span class="sxs-lookup"><span data-stu-id="efe40-179">It can't start with pg_.</span></span> <span data-ttu-id="efe40-180">今後の使用に備えてこの名前を覚えておくか、書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="efe40-180">Remember or write down this name for future use.</span></span>

### <a name="password"></a><span data-ttu-id="efe40-181">パスワード</span><span class="sxs-lookup"><span data-stu-id="efe40-181">Password</span></span>

<span data-ttu-id="efe40-182">上記の管理者ログイン名と一緒に使用するパスワードを選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-182">You choose a password to use with the above administrator login name.</span></span> <span data-ttu-id="efe40-183">パスワードには次のカテゴリのうち 3 つの文字が含まれている必要があります。</span><span class="sxs-lookup"><span data-stu-id="efe40-183">Your password must include characters from three of the following categories:</span></span>
- <span data-ttu-id="efe40-184">英大文字</span><span class="sxs-lookup"><span data-stu-id="efe40-184">English uppercase letters</span></span>
- <span data-ttu-id="efe40-185">英小文字</span><span class="sxs-lookup"><span data-stu-id="efe40-185">English lowercase letters</span></span>
- <span data-ttu-id="efe40-186">数字 (0 から 9)</span><span class="sxs-lookup"><span data-stu-id="efe40-186">Numbers (0 through 9)</span></span>
- <span data-ttu-id="efe40-187">英数字以外の文字 (!、$、#、% など)</span><span class="sxs-lookup"><span data-stu-id="efe40-187">Non-alphanumeric characters (!, $, #, %, and so on)</span></span>

<span data-ttu-id="efe40-188">今後の使用に備えてこのパスワードを覚えておくか、書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="efe40-188">Remember or write down this password for future use.</span></span>

### <a name="confirm-password"></a><span data-ttu-id="efe40-189">パスワードの確認</span><span class="sxs-lookup"><span data-stu-id="efe40-189">Confirm password</span></span>

<span data-ttu-id="efe40-190">確認のためにパスワードを再入力します。</span><span class="sxs-lookup"><span data-stu-id="efe40-190">Retype the password to confirmation.</span></span>

### <a name="location"></a><span data-ttu-id="efe40-191">場所</span><span class="sxs-lookup"><span data-stu-id="efe40-191">Location</span></span>

<span data-ttu-id="efe40-192">場所のオプションを使用すると、サーバーが物理的に作成される場所を指定できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-192">The location option allows you to specify where the server is created physically.</span></span> <span data-ttu-id="efe40-193">自分が住んでいる場所に最も近い地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-193">Choose the geographic location closest to you.</span></span> <span data-ttu-id="efe40-194">実際のシナリオでは、大部分のユーザーに最も近い場所にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="efe40-194">In a real-world scenario, the location should be the location closest to the majority of your users.</span></span>

### <a name="version"></a><span data-ttu-id="efe40-195">バージョン</span><span class="sxs-lookup"><span data-stu-id="efe40-195">Version</span></span>

<span data-ttu-id="efe40-196">使用する PostgreSQL のバージョンを指定できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-196">You can specify the version of PostgreSQL to use.</span></span> <span data-ttu-id="efe40-197">Microsoft では、基本的には PostgreSQL の現在のバージョンとそれより前の 2 つのバージョンがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="efe40-197">Microsoft aims to support the current version and two previous versions of PostgreSQL.</span></span> <span data-ttu-id="efe40-198">運用環境に一致するバージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-198">You'll choose a version that matches your production environment.</span></span>

> [!NOTE]
> <span data-ttu-id="efe40-199">詳細については、次の資料を参照してください。</span><span class="sxs-lookup"><span data-stu-id="efe40-199">For more information, see the following sources:</span></span>
> - [<span data-ttu-id="efe40-200">サポートされている PostgreSQL データベース バージョン</span><span class="sxs-lookup"><span data-stu-id="efe40-200">Supported PostgreSQL database versions</span></span>](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions)
> - [<span data-ttu-id="efe40-201">Versioning Policy (バージョン管理ポリシー)</span><span class="sxs-lookup"><span data-stu-id="efe40-201">Versioning policy</span></span>](https://www.postgresql.org/support/versioning/)

### <a name="pricing-tier"></a><span data-ttu-id="efe40-202">価格レベル</span><span class="sxs-lookup"><span data-stu-id="efe40-202">Pricing tier</span></span>

<span data-ttu-id="efe40-203">サーバーのワークロードをサポートする価格レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="efe40-203">You'll choose a pricing tier that will support your server's workload.</span></span> <span data-ttu-id="efe40-204">選択できるレベルは 3 つあります。</span><span class="sxs-lookup"><span data-stu-id="efe40-204">Recall that you have three tiers to choose from.</span></span> <span data-ttu-id="efe40-205">前述のように、これらの各レベルを使用して、コンピューティングとストレージのオプションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-205">As we saw earlier, each of these tiers allows you to configure the compute and storage options.</span></span> <span data-ttu-id="efe40-206">ここでは、Basic 価格レベルが選択されていて、Gen 5 コンピューティングを使用し、バックアップ保有期間が 25 日である例が示されています。</span><span class="sxs-lookup"><span data-stu-id="efe40-206">Here is an example with the Basic price tier selected, a Gen 5 compute, and a 25-day backup retention period.</span></span>

![新しい PostgreSQL データベースのデータベース価格レベルを示す Azure portal のスクリーンショット](../media/4-azure-db-pricing-tier.png)

<span data-ttu-id="efe40-208">あとは、入力した値を確認し、後で必要となる可能性のあるすべての項目を書き留め、**[作成]** をクリックしてサーバーを作成するだけです。</span><span class="sxs-lookup"><span data-stu-id="efe40-208">All that's left now is to review the values that you entered, make any notes you may need for later, and click **Create** to create the server.</span></span>

<span data-ttu-id="efe40-209">サーバーをデプロイするには数分かかります。</span><span class="sxs-lookup"><span data-stu-id="efe40-209">Deploying the server takes a couple of minutes.</span></span> <span data-ttu-id="efe40-210">デプロイが完了すると、通知を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="efe40-210">You'll receive a notification when the deployment is complete.</span></span> <span data-ttu-id="efe40-211">通知から、新しく作成したサーバーに移動できます。</span><span class="sxs-lookup"><span data-stu-id="efe40-211">From the notification, you can navigate to the newly created server.</span></span>

<span data-ttu-id="efe40-212">ここまでは、Azure Database for PostgreSQL サーバーを作成する手順を説明しました。</span><span class="sxs-lookup"><span data-stu-id="efe40-212">You've now seen the steps you take to create an Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="efe40-213">次のユニットでは、独自の Azure Database for PostgreSQL サーバーを作成してみます。</span><span class="sxs-lookup"><span data-stu-id="efe40-213">In the next unit, you'll have the opportunity to create your own Azure Database for PostgreSQL server.</span></span>
