<span data-ttu-id="25024-101">ご自分のデータベースを評価し、報告された任意のエラーを修正して、データベースを移行する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="25024-101">Now that you have assessed your database and fixed any errors reported, you're ready to migrate your database.</span></span> <span data-ttu-id="25024-102">この演習では、ご使用の SQL データベースを Azure SQL に移行します。</span><span class="sxs-lookup"><span data-stu-id="25024-102">In this exercise, you will migrate your SQL database to Azure SQL.</span></span>

## <a name="setup"></a><span data-ttu-id="25024-103">セットアップ</span><span class="sxs-lookup"><span data-stu-id="25024-103">Setup</span></span>

<span data-ttu-id="25024-104">まだダウンロードしていない場合は、サンプル データをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="25024-104">If you haven't already, download the sample data.</span></span>

1. <span data-ttu-id="25024-105">インターネット ブラウザーを起動して、 https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017 に移動します。</span><span class="sxs-lookup"><span data-stu-id="25024-105">Start an internet browser and navigate to https://docs.microsoft.com/en-us/sql/samples/adventureworks-install-configure?view=sql-server-2017.</span></span>

2. <span data-ttu-id="25024-106">「**OLTP のダウンロード**」で、**AdventureWorks2008R2.bak** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-106">In **OLTP downloads**, click **AdventureWorks2008R2.bak**.</span></span>

3. <span data-ttu-id="25024-107">Management Studio で、*AdventureWorks 2008R2* をご使用の既定のインスタンスに復元します。</span><span class="sxs-lookup"><span data-stu-id="25024-107">In Management Studio, restore *AdventureWorks 2008R2* to your default instance.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="25024-108">Azure SQL Database の作成</span><span class="sxs-lookup"><span data-stu-id="25024-108">Create an Azure SQL Database</span></span>

1. <span data-ttu-id="25024-109">ご自分の Azure portal アカウントにサインインします。</span><span class="sxs-lookup"><span data-stu-id="25024-109">Sign in to your Azure portal account.</span></span>

2. <span data-ttu-id="25024-110">ポータルの左上隅にある **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-110">Click **Create a resource** in the upper left-hand corner of the portal.</span></span>

3. <span data-ttu-id="25024-111">**[データベース]** > **[SQL Database]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-111">Click **Databases** > **SQL Database**.</span></span>

4. <span data-ttu-id="25024-112">SQL Database フォームで、次のフィールドを入力します。</span><span class="sxs-lookup"><span data-stu-id="25024-112">Enter the following fields on the SQL Database form:</span></span>

    |<span data-ttu-id="25024-113">フィールド</span><span class="sxs-lookup"><span data-stu-id="25024-113">Field</span></span>|<span data-ttu-id="25024-114">説明</span><span class="sxs-lookup"><span data-stu-id="25024-114">Description</span></span>|
    |-----|---|
    |<span data-ttu-id="25024-115">データベース名</span><span class="sxs-lookup"><span data-stu-id="25024-115">Database name</span></span>|<span data-ttu-id="25024-116">データベースの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="25024-116">Enter a name for your database.</span></span> <span data-ttu-id="25024-117">これは、移行する既存のデータベースの名前、または選択した新しい名前になります</span><span class="sxs-lookup"><span data-stu-id="25024-117">This can be the name of your existing database that you're migrating, or any new name of your choice</span></span>|
    |<span data-ttu-id="25024-118">リソース グループ</span><span class="sxs-lookup"><span data-stu-id="25024-118">Resource group</span></span>|<span data-ttu-id="25024-119">リソース グループを選択するか、新しいリソース グループ名を入力します</span><span class="sxs-lookup"><span data-stu-id="25024-119">Select a resource group or enter a new resource group name</span></span>|
    |<span data-ttu-id="25024-120">ソースの選択</span><span class="sxs-lookup"><span data-stu-id="25024-120">Select source</span></span>|<span data-ttu-id="25024-121">*[空のデータベース]* を選択します</span><span class="sxs-lookup"><span data-stu-id="25024-121">Select *Blank database*</span></span>|

5. <span data-ttu-id="25024-122">**[サーバー]** の下で、既存のサーバーを選択するか、グローバルに一意の**サーバー名**、**サーバー管理者ログイン**、**パスワード** (確認する必要があります)、**場所**を入力します。</span><span class="sxs-lookup"><span data-stu-id="25024-122">Under **Server**, either select an existing server or, enter a globally unique **Server name**, a **Server admin login**, a **Password** (which you should confirm), and a **Location**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="25024-123">サーバー名、ログイン名、パスワードを使用して、ご利用のデータベースに接続します。</span><span class="sxs-lookup"><span data-stu-id="25024-123">You will use the server name, login name, and password to connect to your database.</span></span>

6. <span data-ttu-id="25024-124">**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-124">Click **Select**.</span></span>

7. <span data-ttu-id="25024-125">**[価格レベル]** で、パフォーマンスの高いデータベースを選択できますが、このチュートリアルでは、既定値を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-125">In **Pricing tier** you can select a database with higher performance, but for this tutorial, select default.</span></span>

8. <span data-ttu-id="25024-126">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-126">Click **Create**.</span></span> <span data-ttu-id="25024-127">チュートリアルを続行する前に、データベースがプロビジョニングされるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="25024-127">Wait until the database has been provisioned before continuing the tutorial.</span></span>

    <span data-ttu-id="25024-128">次のスクリーンショットは、新しい SQL データベースへの可能性がある構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="25024-128">The following screenshot shows a potential configuration for you new SQL database.</span></span>

    ![サンプルの構成を含む、Azure portal の [新しい SQL Database] ブレードのスクリーンショット。](../media-draft/5-create-azure-sql-db.png)

## <a name="create-a-new-migration-project"></a><span data-ttu-id="25024-130">新しい移行プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="25024-130">Create a new migration project</span></span>

1. <span data-ttu-id="25024-131">**Data Migration Assistant** を起動します。</span><span class="sxs-lookup"><span data-stu-id="25024-131">Start **Data Migration Assistant**.</span></span>

2. <span data-ttu-id="25024-132">**新規**アイコンをクリックして、次のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="25024-132">Click the **New** icon, and specify the following options:</span></span>
    - <span data-ttu-id="25024-133">**プロジェクトの種類** - *[移行]* オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-133">**Project type** - Select  the *Migration* option.</span></span>
    - <span data-ttu-id="25024-134">**プロジェクト名** - プロジェクトに使いやすい名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="25024-134">**Project name** - Enter a memorable name for your project.</span></span>
    - <span data-ttu-id="25024-135">**ソース サーバーの種類** - *[SQL Server]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-135">**Source server type** - Select *SQL Server*.</span></span>
    - <span data-ttu-id="25024-136">**対象サーバーの種類** - *[Azure SQL Database]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-136">**Target server type** - Select *Azure SQL Database*.</span></span>
    - <span data-ttu-id="25024-137">**移行スコープ** - *[スキーマとデータ]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-137">**Migration scope** - Select *Schema and data*.</span></span>

3. <span data-ttu-id="25024-138">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-138">Click **Create**.</span></span>

## <a name="specify-the-source-server-and-database"></a><span data-ttu-id="25024-139">ソース サーバーとデータベースの指定</span><span class="sxs-lookup"><span data-stu-id="25024-139">Specify the source server and database</span></span>

1. <span data-ttu-id="25024-140">**[ソース サーバーに接続します]** の下で、**[サーバー名]** フィールドにソース SQL Server インスタンスの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="25024-140">Under **Connect to source server**, in the **Server name** field, enter the name of the source SQL Server instance.</span></span>

2. <span data-ttu-id="25024-141">ソース SQL Server インスタンスによってサポートされる **[認証の種類]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-141">Select the **Authentication type** supported by the source SQL Server instance.</span></span>
    > [!TIP]
    > <span data-ttu-id="25024-142">接続プロパティの下にある **[暗号化接続]** チェック ボックスをオンにして、接続を暗号化することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="25024-142">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

3. <span data-ttu-id="25024-143">**[接続]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-143">Select **Connect**.</span></span>

4. <span data-ttu-id="25024-144">Azure SQL Database に移行する単一のソース データベースを選択し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-144">Select a single source database to migrate to Azure SQL Database and click **Next**.</span></span>

## <a name="specify-the-target-server-and-database"></a><span data-ttu-id="25024-145">対象サーバーとデータベースの指定</span><span class="sxs-lookup"><span data-stu-id="25024-145">Specify the target server and database</span></span>

1. <span data-ttu-id="25024-146">**[対象サーバーに接続します]** の下で、**[サーバー名]** フィールドに Azure SQL Database インスタンスの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="25024-146">Under **Connect to target server**, in the **Server name** field, enter the name of the Azure SQL Database instance.</span></span> <span data-ttu-id="25024-147">**database.windows.net** をデータベース インスタンスに追加します。</span><span class="sxs-lookup"><span data-stu-id="25024-147">Add **database.windows.net** to the database instance.</span></span>

2. <span data-ttu-id="25024-148">対象になる Azure SQL Database インスタンスによってサポートされる認証の種類を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-148">Select the Authentication type supported by the target Azure SQL Database instance.</span></span> <span data-ttu-id="25024-149">このチュートリアルでは、**[SQL Server 認証]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-149">For this tutorial, select **SQL Server Authentication**.</span></span>
    > [!TIP]
    > <span data-ttu-id="25024-150">接続プロパティの下にある **[暗号化接続]** チェック ボックスをオンにして、接続を暗号化することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="25024-150">It is recommended that you select the **Encrypt connection** check box under Connection properties to encrypt the connection.</span></span>

3. <span data-ttu-id="25024-151">Azure SQL Database の作成時に定義した**ユーザー名**と**パスワード**を入力します。</span><span class="sxs-lookup"><span data-stu-id="25024-151">Enter the **Username** and **Password** that you defined when you created the Azure SQL Database.</span></span>

4. <span data-ttu-id="25024-152">**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-152">Click **Connect**.</span></span>

5. <span data-ttu-id="25024-153">移行先の単一の対象になるデータベースを選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-153">Select a single target database to which to migrate.</span></span>
    > <span data-ttu-id="25024-154">[メモ] Windows ユーザーを移行する場合は、ターゲット外部ユーザーのドメイン名のテキスト ボックスに、ターゲット外部ユーザーのドメイン名が正しく指定されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="25024-154">[NOTE] If you intend to migrate Windows users, in the Target external user domain name text box, make sure that the target external user domain name is specified correctly.</span></span>

6. <span data-ttu-id="25024-155">**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-155">Click **Next**.</span></span>

## <a name="select-schema-objects"></a><span data-ttu-id="25024-156">スキーマ オブジェクトの選択</span><span class="sxs-lookup"><span data-stu-id="25024-156">Select schema objects</span></span>

1. <span data-ttu-id="25024-157">Azure SQL Database に移行するソース データベースからスキーマ オブジェクトを選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-157">Select the schema objects from the source database that you want to migrate to Azure SQL Database.</span></span>

    > [!NOTE]
    > <span data-ttu-id="25024-158">そのまま変換できないオブジェクトの一部が、自動修正の可能性と共に表示されます。</span><span class="sxs-lookup"><span data-stu-id="25024-158">Some of the objects that cannot be converted as-is are presented with automatic fix opportunities.</span></span> <span data-ttu-id="25024-159">左側のウィンドウでこれらのオブジェクトをクリックすると、右側のウィンドウに提案された修正が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25024-159">Clicking these objects on the left pane displays the suggested fixes on the right pane.</span></span> <span data-ttu-id="25024-160">修正を確認して、オブジェクトごとにすべての変更を適用するか、無視するかを選びます。</span><span class="sxs-lookup"><span data-stu-id="25024-160">Review the fixes and choose to either apply or ignore all changes, object by object.</span></span> <span data-ttu-id="25024-161">1 つのオブジェクトに対してすべての変更を適用するか、無視しても、他のデータベース オブジェクトには変更が適用されないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="25024-161">Note that applying or ignoring all changes for one object does not affect changes to other database objects.</span></span> <span data-ttu-id="25024-162">変換できない、または自動修正できないというステートメントが、対象になるデータベースに対して再現され、コメントされます。</span><span class="sxs-lookup"><span data-stu-id="25024-162">Statements that cannot be converted or automatically fixed are reproduced to the target database and commented.</span></span>

    !["ストアド プロシージャ: dbo.uspSearchCandidateResumes" の問題が選択され、その問題の詳細が示された、AdventureWorks SQL Server データに影響している移行の問題の一覧を示す Data Migration Assistant のスクリーンショット。](../media-draft/5-suggested-fix.png)

2. <span data-ttu-id="25024-164">**[SQL スクリプトの生成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-164">Click **Generate SQL script**.</span></span>

## <a name="deploy-schema"></a><span data-ttu-id="25024-165">スキーマの配置</span><span class="sxs-lookup"><span data-stu-id="25024-165">Deploy schema</span></span>

1. <span data-ttu-id="25024-166">**[スキーマの配置]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-166">Click **Deploy schema**.</span></span>

2. <span data-ttu-id="25024-167">スキーマの配置の結果を確認します。</span><span class="sxs-lookup"><span data-stu-id="25024-167">Review the results of the schema deployment.</span></span>

## <a name="migrate-data"></a><span data-ttu-id="25024-168">データの移行</span><span class="sxs-lookup"><span data-stu-id="25024-168">Migrate data</span></span>

1. <span data-ttu-id="25024-169">**[データの移行]** をクリックして、データの移行プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="25024-169">Click **Migrate data** to start the data migration process.</span></span>

2. <span data-ttu-id="25024-170">移行するデータを含むテーブルを選択します。</span><span class="sxs-lookup"><span data-stu-id="25024-170">Select the tables with the data you want to migrate.</span></span>

3. <span data-ttu-id="25024-171">**[Start data migration]\(データ移行の開始\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="25024-171">Click **Start data migration**.</span></span>

<span data-ttu-id="25024-172">最後の画面に全体的な状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="25024-172">The final screen shows the overall status.</span></span>

## <a name="summary"></a><span data-ttu-id="25024-173">まとめ</span><span class="sxs-lookup"><span data-stu-id="25024-173">Summary</span></span>

<span data-ttu-id="25024-174">この演習では、空の Azure SQL Database を作成して、ローカルの SQL Server データベースをこの新しいデータベースに移行しました。</span><span class="sxs-lookup"><span data-stu-id="25024-174">In this exercise, you created an empty Azure SQL Database and migrated a local SQL Server database to this new database.</span></span>