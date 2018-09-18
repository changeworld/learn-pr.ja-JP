<span data-ttu-id="cf5a1-101">ある輸送会社は、他の会社と差別化したいと考えていますが、コストはあまりかけたくありません。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-101">Your transportation company wants to set themselves apart from other companies but without breaking the bank.</span></span> <span data-ttu-id="cf5a1-102">コストを管理しながら、最善のサービスを提供するようにデータベースを設定する方法を理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-102">You must have a good handle on how to set up the database to provide the best service while controlling costs.</span></span>

<span data-ttu-id="cf5a1-103">ここでは、次のことを学習します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-103">Here, you'll learn:</span></span>

- <span data-ttu-id="cf5a1-104">次のような、Azure SQL Database を作成するときに考慮する必要がある内容。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-104">What considerations you need to make when creating an Azure SQL database, including:</span></span>
  - <span data-ttu-id="cf5a1-105">データベースの管理コンテナーとしての論理サーバーの動作。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-105">How a logical server acts as an administrative container for your databases.</span></span>
  - <span data-ttu-id="cf5a1-106">購入するモデルの相違。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-106">The differences between purchasing models.</span></span>
  - <span data-ttu-id="cf5a1-107">エラスティック プールを使用してデータベース間で処理能力を共有する方法。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-107">How elastic pools enable you to share processing power among databases.</span></span>
  - <span data-ttu-id="cf5a1-108">データの比較と並べ替えの方法に対する照合順序規則の影響。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-108">How collation rules affect how data is compared and sorted.</span></span>
- <span data-ttu-id="cf5a1-109">ポータルから Azure SQL Database を起動する方法。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-109">How to bring up Azure SQL Database from the portal.</span></span>
- <span data-ttu-id="cf5a1-110">信頼できるソースからのみデータベースにアクセスできるようにファイアウォール規則を追加する方法。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-110">How to add firewall rules so that your database is accessible from only trusted sources.</span></span>

<span data-ttu-id="cf5a1-111">Azure SQL Database を作成するときに考慮する必要があることを簡単に見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-111">Let's take a quick look at some things you need to consider when you create an Azure SQL database.</span></span>

## <a name="one-server-many-databases"></a><span data-ttu-id="cf5a1-112">1 つのサーバー、多くのデータベース</span><span class="sxs-lookup"><span data-stu-id="cf5a1-112">One server, many databases</span></span>

<span data-ttu-id="cf5a1-113">最初の Azure SQL Database を作成するときは、_Azure SQL 論理サーバー_ も作成します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-113">When you create your first Azure SQL database, you also create an _Azure SQL logical server_.</span></span> <span data-ttu-id="cf5a1-114">論理サーバーはデータベース用の管理コンテナーと考えることができます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-114">Think of a logical server as an administrative container for your databases.</span></span> <span data-ttu-id="cf5a1-115">論理サーバーを使用して、ログイン、ファイアウォール規則、セキュリティ ポリシーを制御できます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-115">You can control logins, firewall rules, and security policies through the logical server.</span></span> <span data-ttu-id="cf5a1-116">また、論理サーバー内の各データベースでこれらのポリシーをオーバーライドすることもできます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-116">You can also override these policies on each database within the logical server.</span></span>

<span data-ttu-id="cf5a1-117">ここで必要なデータベースは 1 つだけです。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-117">For now, you need just one database.</span></span> <span data-ttu-id="cf5a1-118">ただし、論理サーバーを使用すると、後でさらにデータベースを追加し、すべてのデータベース間でパフォーマンスを調整することができます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-118">But a logical server enables you to add more later and tune performance among all your databases.</span></span>

## <a name="choose-performance-dtus-versus-vcores"></a><span data-ttu-id="cf5a1-119">パフォーマンスの選択: DTU か仮想コアか</span><span class="sxs-lookup"><span data-stu-id="cf5a1-119">Choose performance: DTUs versus vCores</span></span>

<span data-ttu-id="cf5a1-120">Azure SQL Database には、DTU と仮想コアの 2 種類の購入モデルがあります。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-120">Azure SQL Database has two purchasing models: DTU and vCore.</span></span>

### <a name="what-are-dtus"></a><span data-ttu-id="cf5a1-121">DTU とは</span><span class="sxs-lookup"><span data-stu-id="cf5a1-121">What are DTUs?</span></span>

<span data-ttu-id="cf5a1-122">DTU はデータベース トランザクション ユニットの略語であり、コンピューティング、ストレージ、IO リソースの結合されたメジャーです。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-122">DTU stands for Database Transaction Unit and is a combined measure of compute, storage, and IO resources.</span></span> <span data-ttu-id="cf5a1-123">DTU モデルは、シンプルな事前構成済みの購入オプションと考えることができます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-123">Think of the DTU model as a simple, preconfigured purchase option.</span></span>

<span data-ttu-id="cf5a1-124">論理サーバーでは複数のデータベースを保持できるため、eDTU、つまり、エラスティック データベース トランザクション ユニットの概念もあります。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-124">Because your logical server can hold more than one database, there's also the idea of eDTUs, or elastic Database Transaction Units.</span></span> <span data-ttu-id="cf5a1-125">このオプションで選択できる価格は 1 つですが、プール内の各データベースは、現在の負荷に応じて消費するリソースを増やしたり減らしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-125">This option enables you to choose one price, but allow each database in the pool to consume fewer or greater resources depending on current load.</span></span>

### <a name="what-are-vcores"></a><span data-ttu-id="cf5a1-126">仮想コアとは</span><span class="sxs-lookup"><span data-stu-id="cf5a1-126">What are vCores?</span></span>

<span data-ttu-id="cf5a1-127">仮想コアでは、作成して支払うコンピューティング リソースとストレージ リソースをきめ細かく制御できます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-127">vCore gives you greater control over what compute and storage resources you create and pay for.</span></span>

<span data-ttu-id="cf5a1-128">DTU モデルではコンピューティング、ストレージ、IO リソースの組み合わせは固定ですが、仮想コア モデルではリソースを個別に構成することができます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-128">While the DTU model provides fixed combinations of compute, storage, and IO resources, the vCore model enables you to configure resources independently.</span></span> <span data-ttu-id="cf5a1-129">たとえば、仮想コア モデルでは、ストレージ容量だけを増やし、コンピューティングと IO のスループットは既存の量のままにすることができます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-129">For example, with the vCore model you can increase storage capacity but keep the existing amount of compute and IO throughput.</span></span>

<span data-ttu-id="cf5a1-130">この輸送と物流のプロトタイプで必要なのは、1 つの Azure SQL Database インスタンスだけです。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-130">Your transportation and logistics prototype only needs one Azure SQL Database instance.</span></span> <span data-ttu-id="cf5a1-131">コンピューティング、ストレージ、IO のパフォーマンスのバランスが取れていて、低コストで開始できるため、DTU オプションを使うことに決定します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-131">You decide on the DTU option because it provides a good balance of compute, storage, and IO performance and is less expensive to get started.</span></span>

## <a name="what-are-sql-elastic-pools"></a><span data-ttu-id="cf5a1-132">SQL エラスティック プールとは</span><span class="sxs-lookup"><span data-stu-id="cf5a1-132">What are SQL elastic pools?</span></span>

<span data-ttu-id="cf5a1-133">Azure SQL Database を作成するときに、_SQL エラスティック プール_ を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-133">When you create your Azure SQL database, you can create a _SQL elastic pool_.</span></span>

<span data-ttu-id="cf5a1-134">SQL エラスティック プールは eDTU と関連があります。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-134">SQL elastic pools relate to eDTUs.</span></span> <span data-ttu-id="cf5a1-135">SQL エラスティック プールでは、プール内のすべてのデータベース間で共有されるコンピューティング リソースとストレージ リソースのセットを購入できます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-135">They enable you to buy a set of compute and storage resources that are shared among all the databases in the pool.</span></span> <span data-ttu-id="cf5a1-136">各データベースでは、現在の負荷に応じて、設定された制限内で必要なリソースを使用できます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-136">Each database can use the resources they need, within the limits you set, depending on current load.</span></span>

<span data-ttu-id="cf5a1-137">このプロトタイプでは、必要な SQL Database は 1 つだけなので、SQL エラスティック プールは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-137">For your prototype, you won't need a SQL elastic pool because you need only one SQL database.</span></span>

## <a name="what-is-collation"></a><span data-ttu-id="cf5a1-138">照合順序とは</span><span class="sxs-lookup"><span data-stu-id="cf5a1-138">What is collation?</span></span>

<span data-ttu-id="cf5a1-139">照合順序とは、データを並べ替えたり比較したりする規則のことです。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-139">Collation refers to the rules that sort and compare data.</span></span> <span data-ttu-id="cf5a1-140">大文字小文字の区別、アクセント記号、その他の言語の特性が重要なときに並べ替え規則を定義する場合に、照合順序が役立ちます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-140">Collation helps you define sorting rules when case sensitivity, accent marks, and other language characteristics are important.</span></span>

<span data-ttu-id="cf5a1-141">既定の照合順序 **SQL_Latin1_General_CP1_CI_AS** が意味することについて考えます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-141">Let's take a moment to consider what the default collation, **SQL_Latin1_General_CP1_CI_AS**, means.</span></span>

- <span data-ttu-id="cf5a1-142">**Latin1_General** は、西ヨーロッパ言語のファミリを示します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-142">**Latin1_General** refers to the family of Western European languages.</span></span>
- <span data-ttu-id="cf5a1-143">**CP1** は、ラテン アルファベットの人気のある文字エンコーディングであるコード ページ 1252 を示します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-143">**CP1** refers to code page 1252, a popular character encoding of the Latin alphabet.</span></span>
- <span data-ttu-id="cf5a1-144">**CI** は、比較で大文字と小文字が区別されないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-144">**CI** means that comparisons are case insensitive.</span></span> <span data-ttu-id="cf5a1-145">たとえば、"HELLO" と "hello" は同じです。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-145">For example, "HELLO" compares equally to "hello".</span></span>
- <span data-ttu-id="cf5a1-146">**AS** は、比較でアクセントが区別されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-146">**AS** means that comparisons are accent sensitive.</span></span> <span data-ttu-id="cf5a1-147">たとえば、"résumé" と "resume" は同じではありません。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-147">For example, "résumé" doesn't compare equally to "resume".</span></span>

<span data-ttu-id="cf5a1-148">データの並べ替えと比較の方法に関して特定の要件はないので、既定の照合順序を選択します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-148">Because you don't have specific requirements around how data is sorted and compared, you choose the default collation.</span></span>

## <a name="create-your-azure-sql-database"></a><span data-ttu-id="cf5a1-149">Azure SQL Database を作成する</span><span class="sxs-lookup"><span data-stu-id="cf5a1-149">Create your Azure SQL database</span></span>

<span data-ttu-id="cf5a1-150">ここではデータベースを設定します。これには論理サーバーの作成が含まれます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-150">Here you'll set up your database, which includes creating your logical server.</span></span> <span data-ttu-id="cf5a1-151">輸送物流アプリケーションをサポートする設定を選択します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-151">You'll choose settings that support your transportation logistics application.</span></span> <span data-ttu-id="cf5a1-152">実際には、構築しているアプリの種類をサポートしている設定を選択します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-152">In practice, you would choose settings that support the kind of app you're building.</span></span>

<span data-ttu-id="cf5a1-153">時間が経って需要に対応するためにコンピューティング能力の増強が必要であるとわかった場合は、パフォーマンス オプションを調整することができ、DTU と仮想コアのパフォーマンス モデルを切り替えることさえできます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-153">Over time if you you realize you need additional compute power to keep up with demand, you can adjust performance options or even switch between the DTU and vCore performance models.</span></span> 

1. <span data-ttu-id="cf5a1-154">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-154">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>


1. <span data-ttu-id="cf5a1-155">ポータルの左上隅にある **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-155">From the portal, click **Create a resource** from the upper left-hand corner.</span></span> <span data-ttu-id="cf5a1-156">**[Databases]** を選択し、**[SQL Database]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-156">Select **Databases**, then select **SQL Database**.</span></span>

   ![[リソースの作成] ブレードと [Databases] セクションが表示され、[リソースの作成]、[Databases]、[SQL Database] ボタンが強調されている、Azure portal の スクリーンショット。](../media-draft/create-db.png)

1. <span data-ttu-id="cf5a1-158">**[サーバー]** の **[必要な設定の構成]** をクリックし、フォームに入力して、**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-158">Under **Server**, click **Configure required settings**, fill out the form, then click **Select**.</span></span> <span data-ttu-id="cf5a1-159">フォームに入力する方法の詳細を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-159">Here's more information on how to fill out the form:</span></span>

    | <span data-ttu-id="cf5a1-160">設定</span><span class="sxs-lookup"><span data-stu-id="cf5a1-160">Setting</span></span>      | <span data-ttu-id="cf5a1-161">値</span><span class="sxs-lookup"><span data-stu-id="cf5a1-161">Value</span></span> |
    | ------------ | ----- |
    | <span data-ttu-id="cf5a1-162">**サーバー名**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-162">**Server name**</span></span> | <span data-ttu-id="cf5a1-163">グローバルに一意の[サーバー名](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)です。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-163">A globally unique [server name](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).</span></span> |
    | <span data-ttu-id="cf5a1-164">**サーバー管理者ログイン**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-164">**Server admin login**</span></span> | <span data-ttu-id="cf5a1-165">プライマリ管理者ログイン名として機能する[データベース識別子](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers)です。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-165">A [database identifier](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) that serves as your primary administrator login name.</span></span> |
    | <span data-ttu-id="cf5a1-166">**パスワード**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-166">**Password**</span></span> | <span data-ttu-id="cf5a1-167">8 文字以上が使用され、大文字、小文字、数字、英数字以外の文字のうち、3 つのカテゴリの文字が含まれている有効なパスワード。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-167">Any valid password that has at least eight characters and contains characters from three of these categories: uppercase characters, lowercase characters, numbers, and non-alphanumeric characters.</span></span> |
    | <span data-ttu-id="cf5a1-168">**場所**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-168">**Location**</span></span> | <span data-ttu-id="cf5a1-169">有効な場所です。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-169">Any valid location.</span></span> |
    > [!IMPORTANT]
    > <span data-ttu-id="cf5a1-170">後で使用するので、サーバー名、管理者ログイン、パスワードを書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-170">Note your server name, admin login, and password for later.</span></span>

1. <span data-ttu-id="cf5a1-171">**[価格レベル]** をクリックして、サービス レベルを指定します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-171">Click **Pricing tier** to specify the service tier.</span></span> <span data-ttu-id="cf5a1-172">**[Basic]** サービス レベルを選択し、**[適用]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-172">Select the **Basic** service tier, then click **Apply**.</span></span>

1. <span data-ttu-id="cf5a1-173">フォームの残りの部分を設定するには、次の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-173">Use these values to fill out the rest of the form.</span></span>

    | <span data-ttu-id="cf5a1-174">設定</span><span class="sxs-lookup"><span data-stu-id="cf5a1-174">Setting</span></span>      | <span data-ttu-id="cf5a1-175">値</span><span class="sxs-lookup"><span data-stu-id="cf5a1-175">Value</span></span> |
    | ------------ | ----- |
    | <span data-ttu-id="cf5a1-176">**データベース名**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-176">**Database name**</span></span> | <span data-ttu-id="cf5a1-177">**Logistics**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-177">**Logistics**</span></span> |
    | <span data-ttu-id="cf5a1-178">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-178">**Subscription**</span></span> | <span data-ttu-id="cf5a1-179">お使いのサブスクリプション</span><span class="sxs-lookup"><span data-stu-id="cf5a1-179">Your subscription</span></span> |
    | <span data-ttu-id="cf5a1-180">**[リソース グループ]**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-180">**Resource group**</span></span> | <span data-ttu-id="cf5a1-181">**logistics-db-rg**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-181">**logistics-db-rg**</span></span> |
    | <span data-ttu-id="cf5a1-182">**ソースの選択**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-182">**Select source**</span></span> | <span data-ttu-id="cf5a1-183">**空のデータベース**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-183">**Blank database**</span></span> |
    | <span data-ttu-id="cf5a1-184">**SQL エラスティック プールを使用しますか?**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-184">**Want to use SQL elastic pool?**</span></span> | <span data-ttu-id="cf5a1-185">**後で**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-185">**Not now**</span></span> |
    | <span data-ttu-id="cf5a1-186">**照合順序**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-186">**Collation**</span></span> | <span data-ttu-id="cf5a1-187">**SQL_Latin1_General_CP1_CI_AS**</span><span class="sxs-lookup"><span data-stu-id="cf5a1-187">**SQL_Latin1_General_CP1_CI_AS**</span></span> |

1. <span data-ttu-id="cf5a1-188">**[作成]** をクリックして Azure SQL Database を作成します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-188">Click **Create** to create your Azure SQL database.</span></span>

1. <span data-ttu-id="cf5a1-189">ツール バーの **[通知]** をクリックして、デプロイ プロセスを監視します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-189">On the toolbar, click **Notifications** to monitor the deployment process.</span></span>

<span data-ttu-id="cf5a1-190">プロセスが完了したら、**[ダッシュボードにピン留めする]** をクリックして、後で必要なときにすばやくアクセスできるように、データベース サーバーをダッシュボードにピン留めします。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-190">When the process completes, click **Pin to dashboard** to pin your database server to the dashboard so that you have quick access when you need it later.</span></span>

   ![強調表示された最近の展開成功メッセージから [通知] メニューと [ダッシュボードにピン留めする] が表示されている Azure portal のスクリーンショット。](../media-draft/notifications-complete.png)

## <a name="set-the-server-firewall"></a><span data-ttu-id="cf5a1-192">サーバーのファイアウォールを設定する</span><span class="sxs-lookup"><span data-stu-id="cf5a1-192">Set the server firewall</span></span>

<span data-ttu-id="cf5a1-193">Azure SQL Database が稼働状態になりました。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-193">Your Azure SQL database is now up and running.</span></span> <span data-ttu-id="cf5a1-194">新しいデータベースをさらに構成、セキュリティ保護、監視、トラブルシューティングするための多くのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-194">You have many options to further configure, secure, monitor, and troubleshoot your new database.</span></span>

<span data-ttu-id="cf5a1-195">また、ファイアウォール経由でデータベースにアクセスできるシステムを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-195">You can also specify which systems can access your database through the firewall.</span></span> <span data-ttu-id="cf5a1-196">初期状態のファイアウォールでは、Azure の外部からデータベース サーバーへのアクセスはすべて禁止されます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-196">Initially, the firewall prevents all access to your database server from outside of Azure.</span></span>

<span data-ttu-id="cf5a1-197">このプロトタイプでは、ユーザーのノート パソコンからのみデータベースにアクセスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-197">For your prototype, you only need to access the database from your laptop.</span></span> <span data-ttu-id="cf5a1-198">後で、モバイル アプリなどのシステムをホワイトリストに追加できます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-198">Later, you can whitelist additional systems, such as your mobile app.</span></span>

<span data-ttu-id="cf5a1-199">それでは、開発用コンピューターがファイアウォール経由でデータベースにアクセスできるようにしましょう。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-199">Let's enable your development computer to access the database through the firewall now.</span></span>

1. <span data-ttu-id="cf5a1-200">Logistics データベースの概要ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-200">Go to the overview blade of the Logistics database.</span></span> <span data-ttu-id="cf5a1-201">前にデータベースをピン留めした場合は、ダッシュボードの **Logistics** タイルをクリックして移動できます。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-201">If you pinned the database earlier, you can click the **Logistics** tile on the dashboard to get there.</span></span>

1. <span data-ttu-id="cf5a1-202">**[サーバー ファイアウォールの設定]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-202">Click **Set server firewall**.</span></span>

    ![[サーバー ファイアウォールの設定] ボタンが強調表示されている SQL データベース概要ブレードを示す Azure portal のスクリーンショット。](../media-draft/set-server-firewall.png)

1. <span data-ttu-id="cf5a1-204">**[クライアント IP の追加]**、**[保存]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-204">Click **Add client IP**, and then click **Save**.</span></span>

    ![[クライアント IP の追加] ボタンが強調表示されている SQL データベース ファイアウォール設定ブレードを示す Azure portal のスクリーンショット。](../media-draft/add-client-ip.png)

<span data-ttu-id="cf5a1-206">次のパートでは、新しいデータベースと Azure Cloud Shell に関する実習をいくつか行います。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-206">In the next part, you'll get some hands-on practice with your new database and with Azure Cloud Shell.</span></span> <span data-ttu-id="cf5a1-207">データベースに接続し、テーブルを作成し、サンプル データをいくつか追加して、いくつかの SQL ステートメントを実行します。</span><span class="sxs-lookup"><span data-stu-id="cf5a1-207">You'll connect to the database, create a table, add some sample data, and execute a few SQL statements.</span></span>