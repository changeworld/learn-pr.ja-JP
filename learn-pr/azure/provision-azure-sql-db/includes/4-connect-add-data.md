<span data-ttu-id="5175c-101">データベースをアプリに接続する前に、データベースに接続し、基本的なテーブルを追加し、サンプル データを操作できることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5175c-101">Before you connect the database to your app, you want to check that you can connect to it, add a basic table, and work with sample data.</span></span>

<span data-ttu-id="5175c-102">Azure SQL Database のインフラストラクチャ、ソフトウェア更新プログラム、修正プログラムは Microsoft で保持しています。</span><span class="sxs-lookup"><span data-stu-id="5175c-102">We maintain the infrastructure, software updates, and patches for your Azure SQL database.</span></span> <span data-ttu-id="5175c-103">Azure SQL Database のそれ以外の部分については、他の SQL Server インストールと同様に扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="5175c-103">Beyond that, you can treat your Azure SQL database like you would any other SQL Server installation.</span></span> <span data-ttu-id="5175c-104">たとえば、Visual Studio、SQL Server Management Studio、SQL Server Operations Studio、またはその他のツールを使用して、Azure SQL Database を管理することができます。</span><span class="sxs-lookup"><span data-stu-id="5175c-104">For example, you can use Visual Studio, SQL Server Management Studio, SQL Server Operations Studio, or other tools to manage your Azure SQL database.</span></span>

<span data-ttu-id="5175c-105">データベースにアクセスしてアプリに接続する方法は任意です。</span><span class="sxs-lookup"><span data-stu-id="5175c-105">How you access your database and connect it to your app is up to you.</span></span> <span data-ttu-id="5175c-106">しかし、データベースの操作経験をある程度積むために、ここではポータルから直接接続し、テーブルを作成し、基本的な CRUD 操作をいくつか実行します。</span><span class="sxs-lookup"><span data-stu-id="5175c-106">But to get some experience working with your database, here you'll connect to it directly from the portal, create a table, and run a few basic CRUD operations.</span></span> <span data-ttu-id="5175c-107">学習内容:</span><span class="sxs-lookup"><span data-stu-id="5175c-107">You'll learn:</span></span>

- <span data-ttu-id="5175c-108">Cloud Shell の概要と、ポータルからアクセスする方法。</span><span class="sxs-lookup"><span data-stu-id="5175c-108">What Cloud Shell is and how to access it from the portal.</span></span>
- <span data-ttu-id="5175c-109">接続文字列などのデータベースに関する情報に Azure CLI からアクセスする方法。</span><span class="sxs-lookup"><span data-stu-id="5175c-109">How to access information about your database from the Azure CLI, including connection strings.</span></span>
- <span data-ttu-id="5175c-110">`sqlcmd` を使用してデータベースに接続する方法。</span><span class="sxs-lookup"><span data-stu-id="5175c-110">How to connect to your database using `sqlcmd`.</span></span>
- <span data-ttu-id="5175c-111">基本的なテーブルといくつかのサンプル データを使用してデータベースを初期化する方法。</span><span class="sxs-lookup"><span data-stu-id="5175c-111">How to initialize your database with a basic table and some sample data.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="5175c-112">Azure Cloud Shell とは</span><span class="sxs-lookup"><span data-stu-id="5175c-112">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="5175c-113">Azure Cloud Shell は、Azure リソースを開発および管理するための、ブラウザー ベースのシェル環境です。</span><span class="sxs-lookup"><span data-stu-id="5175c-113">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span> <span data-ttu-id="5175c-114">Cloud Shell は、クラウド上で動作する対話型コンソールと考えてください。</span><span class="sxs-lookup"><span data-stu-id="5175c-114">Think of Cloud Shell as an interactive console that runs in the cloud.</span></span>

<span data-ttu-id="5175c-115">バックグラウンドでは Cloud Shell は Linux 上で動作しています。</span><span class="sxs-lookup"><span data-stu-id="5175c-115">Behind the scenes, Cloud Shell runs on Linux.</span></span> <span data-ttu-id="5175c-116">しかし、Linux 環境と Windows 環境のどちらを好むかによって、Bash と PowerShell のいずれかから選択できます。</span><span class="sxs-lookup"><span data-stu-id="5175c-116">But depending on whether you prefer a Linux or Windows environment, you have two experiences to choose from: Bash and PowerShell.</span></span>

<span data-ttu-id="5175c-117">Cloud Shell は任意の場所からアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5175c-117">Cloud Shell is accessible from anywhere.</span></span> <span data-ttu-id="5175c-118">ポータル以外にも、[shell.azure.com](https://shell.azure.com/)、Azure mobile app、または Visual Studio Code からも Cloud Shell にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5175c-118">Besides the portal, you can also access Cloud Shell from [shell.azure.com](https://shell.azure.com/), the Azure mobile app, or from Visual Studio Code.</span></span>

<span data-ttu-id="5175c-119">Cloud Shell には、一般的なツールとテキスト エディターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5175c-119">Cloud Shell includes popular tools and text editors.</span></span> <span data-ttu-id="5175c-120">次に、このドキュメントのタスクに使用する 3 つのツール `az`、`jq`、`sqlcmd` について簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="5175c-120">Here's a brief look at `az`, `jq`, and `sqlcmd`, three tools you'll use for our current task.</span></span>

- <span data-ttu-id="5175c-121">`az` は Azure CLI とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="5175c-121">`az` is also known as the Azure CLI.</span></span> <span data-ttu-id="5175c-122">Azure のリソースを使用するためのコマンド ライン インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="5175c-122">It's the command-line interface for working with Azure resources.</span></span> <span data-ttu-id="5175c-123">接続文字列など、データベースに関する情報を取得するために使用します。</span><span class="sxs-lookup"><span data-stu-id="5175c-123">You'll use this to get information about your database, including the connection string.</span></span>
- <span data-ttu-id="5175c-124">`jq` はコマンドライン JSON パーサーです。</span><span class="sxs-lookup"><span data-stu-id="5175c-124">`jq` is a command-line JSON parser.</span></span> <span data-ttu-id="5175c-125">`az` コマンドの出力をこのツールにパイプして、JSON 出力から重要なフィールドを抽出します。</span><span class="sxs-lookup"><span data-stu-id="5175c-125">You'll pipe output from `az` commands to this tool to extract important fields from JSON output.</span></span>
- <span data-ttu-id="5175c-126">`sqlcmd` を使用すると、SQL Server 上でステートメントを実行できます。</span><span class="sxs-lookup"><span data-stu-id="5175c-126">`sqlcmd` enables you to execute statements on SQL Server.</span></span> <span data-ttu-id="5175c-127">Azure SQL Database との対話型セッションを作成するために `sqlcmd` を使用します。</span><span class="sxs-lookup"><span data-stu-id="5175c-127">You'll use `sqlcmd` to create an interactive session with your Azure SQL database.</span></span>

## <a name="get-information-about-your-azure-sql-database"></a><span data-ttu-id="5175c-128">Azure SQL Database に関する情報を入手する</span><span class="sxs-lookup"><span data-stu-id="5175c-128">Get information about your Azure SQL database</span></span>

<span data-ttu-id="5175c-129">データベースに接続する前に、データベースが存在していて、オンラインであることを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5175c-129">Before you connect to your database, it's a good idea to verify it exists and is online.</span></span>

<span data-ttu-id="5175c-130">ここでは、Cloud Shell を起動し、`az` ユーティリティを使用してデータベースを一覧表示し、最大サイズと状態などの **Logistics** データベースに関する一部の情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="5175c-130">Here, you bring up Cloud Shell and use the `az` utility to list your databases and show some information about the **Logistics** database, including its maximum size and status.</span></span>

1. <span data-ttu-id="5175c-131">ポータルの上部にある **[Cloud Shell]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5175c-131">From the portal, at the top, click **Cloud Shell**.</span></span>

1. <span data-ttu-id="5175c-132">`az` コマンドを実行する場合、リソース グループの名前と Azure SQL 論理サーバーの名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="5175c-132">The `az` commands you'll run require the name of your resource group and the name of your Azure SQL logical server.</span></span> <span data-ttu-id="5175c-133">入力を省略できるように、この `azure configure` コマンドを実行して既定値として指定します。</span><span class="sxs-lookup"><span data-stu-id="5175c-133">To save typing, run this `azure configure` command to specify them as default values.</span></span>
    <span data-ttu-id="5175c-134">`contoso-logistics` を Azure SQL 論理サーバーの名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5175c-134">Replace `contoso-logistics` with the name of your Azure SQL logical server.</span></span>

    ```azurecli
    az configure --defaults group=logistics-db-rg sql-server=contoso-logistics
    ```

1. <span data-ttu-id="5175c-135">`az sql db list` を実行して、Azure SQL 論理サーバー上のすべてのデータベースを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="5175c-135">Run `az sql db list` to list all databases on your Azure SQL logical server.</span></span>

    ```azurecli
    az sql db list
    ```
    <span data-ttu-id="5175c-136">出力として大きなブロックの JSON が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-136">You see a large block of JSON as output.</span></span>

1. <span data-ttu-id="5175c-137">データベース名のみが必要なので、このコマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="5175c-137">Since we want just the database names, run the command a second time.</span></span> <span data-ttu-id="5175c-138">今回は、出力を `jq` にパイプして、名前フィールドのみを出力します。</span><span class="sxs-lookup"><span data-stu-id="5175c-138">This time, pipe the output to `jq` to print out only the name fields.</span></span>
    ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    <span data-ttu-id="5175c-139">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-139">You see this.</span></span>
    ```console
    [
      {
        "name": "Logistics"
      },
      {
        "name": "master"
      }
    ]
    ```
    <span data-ttu-id="5175c-140">**Logistics** はデータベースです。</span><span class="sxs-lookup"><span data-stu-id="5175c-140">**Logistics** is your database.</span></span> <span data-ttu-id="5175c-141">SQL Server と同様に、**master** には、サインイン アカウントやシステム構成設定などのサーバー メタデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5175c-141">Like SQL Server, **master** includes server metadata, such as sign-in accounts and system configuration settings.</span></span>

1. <span data-ttu-id="5175c-142">この `az sql db show` コマンドを実行すると、**Logistics** データベースに関する詳細情報が得られます。</span><span class="sxs-lookup"><span data-stu-id="5175c-142">Run this `az sql db show` command to get details about the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics
    ```
    <span data-ttu-id="5175c-143">前回と同様に、出力として大きなブロックの JSON が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-143">As before, you see a large block of JSON as output.</span></span>

1. <span data-ttu-id="5175c-144">このコマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="5175c-144">Run the command a second time.</span></span> <span data-ttu-id="5175c-145">今回は、出力を `jq` にパイプして、**Logistics** データベースの名前、最大サイズ、状態のみに出力を制限します。</span><span class="sxs-lookup"><span data-stu-id="5175c-145">This time, pipe the output to `jq` to limit output to only the name, maximum size, and status of the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```
    <span data-ttu-id="5175c-146">データベースがオンラインであり、約 2 GB のデータを保持できることがわかります。</span><span class="sxs-lookup"><span data-stu-id="5175c-146">You see that the database is online and can hold around 2 GB of data.</span></span>
    ```console
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a><span data-ttu-id="5175c-147">データベースに接続する</span><span class="sxs-lookup"><span data-stu-id="5175c-147">Connect to your database</span></span>

<span data-ttu-id="5175c-148">データベースの概要を少し理解したところで、`sqlcmd` を使用してデータベースに接続し、輸送ドライバーに関する情報を保持するテーブルを作成し、いくつかの基本的な CRUD 操作を実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="5175c-148">Now that you understand a bit about your database, let's connect to it using `sqlcmd`, create a table that holds information about transportation drivers, and perform a few basic CRUD operations.</span></span>

<span data-ttu-id="5175c-149">CRUD は、**作成** (create)、**読み取り** (read)、**更新** (update)、**削除** (delete) の略であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5175c-149">Remember that CRUD stands for **create**, **read**, **update**, and **delete**.</span></span> <span data-ttu-id="5175c-150">これらの用語はテーブル データに対して実行する操作を指します。また、アプリに必要な 4 つの基本操作です。</span><span class="sxs-lookup"><span data-stu-id="5175c-150">These terms refer to operations you perform on table data and are the four basic operations you need for your app.</span></span> <span data-ttu-id="5175c-151">今が各操作を実行できることを確認するよい機会です。</span><span class="sxs-lookup"><span data-stu-id="5175c-151">Now's a good time to verify you can perform each of them.</span></span>

1. <span data-ttu-id="5175c-152">この `az sql db show-connection-string` コマンドを実行すると、**Logistics** データベースに対する接続文字列を `sqlcmd` に使用できる形式で取得できます。</span><span class="sxs-lookup"><span data-stu-id="5175c-152">Run this `az sql db show-connection-string` command to get the connection string to the **Logistics** database in a format that `sqlcmd` can use.</span></span>

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```
    <span data-ttu-id="5175c-153">次のような内容が出力されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-153">Your output resembles this.</span></span>
    ```console
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```

1. <span data-ttu-id="5175c-154">前の手順で `sqlcmd` ステートメントを実行して、対話型セッションを作成します。</span><span class="sxs-lookup"><span data-stu-id="5175c-154">Run the `sqlcmd` statement from the previous step to create an interactive session.</span></span>
    <span data-ttu-id="5175c-155">囲んでいる引用符を削除し、`<username>` と `<password>` をデータベースの作成時に指定したユーザー名とパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="5175c-155">Remove the surrounding quotes and replace `<username>` and `<password>` with the username and password you specified when you created your database.</span></span> <span data-ttu-id="5175c-156">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5175c-156">Here's an example.</span></span>

    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```

    > [!TIP]
    > <span data-ttu-id="5175c-157">"&" と他の特殊文字が処理命令として解釈されないように、パスワードを引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="5175c-157">Place your password in quotes so that "&" and other special characters aren't interpreted as processing instructions.</span></span>

1. <span data-ttu-id="5175c-158">`sqlcmd` セッションから、`Drivers` という名前のテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="5175c-158">From your `sqlcmd` session, create a table named `Drivers`.</span></span>

    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255) );
    GO
    ```

    <span data-ttu-id="5175c-159">このテーブルには、一意の識別子、ドライバーの姓、名、ドライバーの出発地 (市区町村) という 4 つの列があります。</span><span class="sxs-lookup"><span data-stu-id="5175c-159">The table contains four columns: a unique identifier, the driver's last and first name, and the driver's city of origin.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5175c-160">ここに表示されている言語は、Transact-SQL または T-SQL です。</span><span class="sxs-lookup"><span data-stu-id="5175c-160">The language you see here is Transact-SQL, or T-SQL.</span></span>

1. <span data-ttu-id="5175c-161">`Drivers` テーブルが存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="5175c-161">Verify that the `Drivers` table exists.</span></span>

    ```sql
    SELECT name FROM sys.tables;
    GO
    ```

    <span data-ttu-id="5175c-162">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-162">You see this.</span></span>

    ```console
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers

    (1 rows affected)
    ```

1. <span data-ttu-id="5175c-163">この `INSERT` T-SQL ステートメントを実行して、サンプル行をテーブルに追加します。</span><span class="sxs-lookup"><span data-stu-id="5175c-163">Run this `INSERT` T-SQL statement to add a sample row to the table.</span></span> <span data-ttu-id="5175c-164">これは**作成**操作です。</span><span class="sxs-lookup"><span data-stu-id="5175c-164">This is the **create** operation.</span></span>

    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Zirne', 'Laura', 'Springfield');
    GO
    ```

    <span data-ttu-id="5175c-165">これは操作が成功したことを示すために表示されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-165">You see this to indicate the operation succeeded.</span></span>

    ```console
    (1 rows affected)
    ```

1. <span data-ttu-id="5175c-166">テーブルのすべての行の`DriverID` 列のこの `SELECT` T-SQL ステートメント リストを実行します。</span><span class="sxs-lookup"><span data-stu-id="5175c-166">Run this `SELECT` T-SQL statement list in the `DriverID` column from all rows in the table.</span></span> <span data-ttu-id="5175c-167">これは**読み取り**操作です。</span><span class="sxs-lookup"><span data-stu-id="5175c-167">This is the **read** operation.</span></span>

    ```sql
    SELECT DriverID FROM Drivers;
    GO
    ```

    <span data-ttu-id="5175c-168">1 つの結果として、前の手順で作成した行の `DriverID` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-168">You see one result, the `DriverID` for the row you created in the previous step.</span></span>

    ```console
    DriverID
    -----------
            123

    (1 rows affected)
    ```

1. <span data-ttu-id="5175c-169">この `UPDATE` T-SQL ステートメントを実行して、`DriverID` が 123 のドライバーについて出発地を "Springfield" から "Springfield, OR" に変更します。</span><span class="sxs-lookup"><span data-stu-id="5175c-169">Run this `UPDATE` T-SQL statement to change the city of origin from "Springfield" to "Springfield, OR" for the driver with a `DriverID` of 123.</span></span> <span data-ttu-id="5175c-170">これは**更新**操作です。</span><span class="sxs-lookup"><span data-stu-id="5175c-170">This is the **update** operation.</span></span>

    ```sql
    UPDATE Drivers SET OriginCity='Springfield, AK' WHERE DriverID=123;
    GO
    ```

    <span data-ttu-id="5175c-171">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="5175c-171">You see this.</span></span>

    ```console
    (1 rows affected)
    ```

1. <span data-ttu-id="5175c-172">この `DELETE` T-SQL ステートメントを実行して、レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="5175c-172">Run this `DELETE` T-SQL statement to delete the record.</span></span> <span data-ttu-id="5175c-173">これは**削除**操作です。</span><span class="sxs-lookup"><span data-stu-id="5175c-173">This is the **delete** operation.</span></span>

    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```

    ```console
    (1 rows affected)
    ```

1. <span data-ttu-id="5175c-174">この `SELECT` T-SQL ステートメントを実行して、`Drivers` テーブルが空であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="5175c-174">Run this `SELECT` T-SQL statement to verify the `Drivers` table is empty.</span></span>

    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```

    <span data-ttu-id="5175c-175">テーブルに行がないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="5175c-175">You see that the table contains no rows.</span></span>

    ```console
    -----------
              0

    (1 rows affected)
    ```

## <a name="summary"></a><span data-ttu-id="5175c-176">まとめ</span><span class="sxs-lookup"><span data-stu-id="5175c-176">Summary</span></span>

<span data-ttu-id="5175c-177">Cloud Shell から Azure SQL Database を操作する方法を学んだので、SQL Server Management Studio、Visual Studio などから、好みの SQL 管理ツールの接続文字列を取得できます。</span><span class="sxs-lookup"><span data-stu-id="5175c-177">Now that you have the hang of working with Azure SQL Database from Cloud Shell, you can get the connection string for your favorite SQL management tool &ndash; whether that's from SQL Server Management Studio, Visual Studio, or something else.</span></span>

<span data-ttu-id="5175c-178">Cloud Shell を使用すると、Azure のリソースに簡単にアクセスして操作できます。</span><span class="sxs-lookup"><span data-stu-id="5175c-178">Cloud Shell makes it easy to access and work with your Azure resources.</span></span> <span data-ttu-id="5175c-179">Cloud Shell はブラウザーベースなので、Windows、macOS、または Linux から、つまり、基本的には Web ブラウザーを使用する任意のシステムからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="5175c-179">Because Cloud Shell is browser-based, you can access it from Windows, macOS, or Linux &ndash; essentially any system with a web browser.</span></span>

<span data-ttu-id="5175c-180">ここでは、Azure CLI コマンドを実行して SQL Azure SQL Database に関する情報を取得する実践的な経験を得ました。</span><span class="sxs-lookup"><span data-stu-id="5175c-180">You gained some hands-on experience running Azure CLI commands to get information about your Azure SQL database.</span></span> <span data-ttu-id="5175c-181">さらに、T-SQL のスキルについても実践しました。</span><span class="sxs-lookup"><span data-stu-id="5175c-181">As a bonus, you practiced your T-SQL skills.</span></span>

<span data-ttu-id="5175c-182">最後に、データベースの破棄方法を要約して説明します。</span><span class="sxs-lookup"><span data-stu-id="5175c-182">Finally, let's wrap up and see how to tear down your database.</span></span>
