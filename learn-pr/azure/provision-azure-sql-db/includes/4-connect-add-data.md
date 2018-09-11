<span data-ttu-id="b5947-101">データベースをアプリに接続する前に、データベースに接続し、基本的なテーブルを追加し、サンプル データを操作できることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b5947-101">Before you connect the database to your app, you want to check that you can connect to it, add a basic table, and work with sample data.</span></span>

<span data-ttu-id="b5947-102">Azure SQL Database のインフラストラクチャ、ソフトウェア更新プログラム、修正プログラムは Microsoft が保守しています。</span><span class="sxs-lookup"><span data-stu-id="b5947-102">We maintain the infrastructure, software updates, and patches for your Azure SQL Database.</span></span> <span data-ttu-id="b5947-103">Azure SQL Database のそれ以外の部分については、他の SQL Server インストールと同様に扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="b5947-103">Beyond that, you can treat your Azure SQL Database like you would any other SQL Server installation.</span></span> <span data-ttu-id="b5947-104">たとえば、Visual Studio、SQL Server Management Studio、またはその他のツールを使用して、Azure SQL Database を管理することができます。</span><span class="sxs-lookup"><span data-stu-id="b5947-104">For example, you can use Visual Studio, SQL Server Management Studio, or other tools to manage your Azure SQL Database.</span></span>

<span data-ttu-id="b5947-105">データベースにアクセスしてアプリに接続する方法は任意です。</span><span class="sxs-lookup"><span data-stu-id="b5947-105">How you access your database and connect it to your app is up to you.</span></span> <span data-ttu-id="b5947-106">しかし、データベースの操作経験をある程度積むために、ここではポータルから直接接続し、テーブルを作成し、基本的な CRUD 操作をいくつか実行します。</span><span class="sxs-lookup"><span data-stu-id="b5947-106">But to get some experience working with your database, here you'll connect to it directly from the portal, create a table, and run a few basic CRUD operations.</span></span> <span data-ttu-id="b5947-107">学習内容:</span><span class="sxs-lookup"><span data-stu-id="b5947-107">You'll learn:</span></span>

* <span data-ttu-id="b5947-108">Cloud Shell の概要と、ポータルからアクセスする方法。</span><span class="sxs-lookup"><span data-stu-id="b5947-108">What Cloud Shell is and how to access it from the portal.</span></span>
* <span data-ttu-id="b5947-109">接続文字列などのデータベースに関する情報に Azure CLI からアクセスする方法。</span><span class="sxs-lookup"><span data-stu-id="b5947-109">How to access information about your database from the Azure CLI, including connection strings.</span></span>
* <span data-ttu-id="b5947-110">`sqlcmd` を使用してデータベースに接続する方法。</span><span class="sxs-lookup"><span data-stu-id="b5947-110">How to connect to your database using `sqlcmd`.</span></span>
* <span data-ttu-id="b5947-111">基本的なテーブルといくつかのサンプル データを使用してデータベースを初期化する方法。</span><span class="sxs-lookup"><span data-stu-id="b5947-111">How to initialize your database with a basic table and some sample data.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="b5947-112">Azure Cloud Shell とは</span><span class="sxs-lookup"><span data-stu-id="b5947-112">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="b5947-113">Azure Cloud Shell は、Azure リソースを開発および管理するための、ブラウザー ベースのシェル環境です。</span><span class="sxs-lookup"><span data-stu-id="b5947-113">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span> <span data-ttu-id="b5947-114">Cloud Shell は、クラウド上で動作する対話型コンソールと考えてください。</span><span class="sxs-lookup"><span data-stu-id="b5947-114">Think of Cloud Shell as an interactive console that runs in the cloud.</span></span>

<span data-ttu-id="b5947-115">バックグラウンドでは Cloud Shell は Linux 上で動作しています。</span><span class="sxs-lookup"><span data-stu-id="b5947-115">Behind the scenes, Cloud Shell runs on Linux.</span></span> <span data-ttu-id="b5947-116">ただし、Linux 環境と Windows 環境のどちらを好むかによって、Bash と PowerShell のいずれかから選択できます。</span><span class="sxs-lookup"><span data-stu-id="b5947-116">But depending on whether you prefer a Linux or Windows environment, you have two experiences to choose from: Bash and PowerShell.</span></span>

<span data-ttu-id="b5947-117">Cloud Shell は任意の場所からアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b5947-117">The Cloud Shell is accessible from anywhere.</span></span> <span data-ttu-id="b5947-118">ポータル以外にも、[shell.azure.com](https://shell.azure.com/)、Azure mobile app、または Visual Studio Code からも Cloud Shell にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b5947-118">Besides the portal, you can also access Cloud Shell from [shell.azure.com](https://shell.azure.com/), the Azure mobile app, or from Visual Studio Code.</span></span>

<span data-ttu-id="b5947-119">Cloud Shell には、一般的なツールとテキスト エディターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b5947-119">Cloud Shell includes popular tools and text editors.</span></span> <span data-ttu-id="b5947-120">次に、このドキュメントのタスクに使用する 3 つのツール `az`、`jq`、`sqlcmd` について簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="b5947-120">Here's a brief look at `az`, `jq`, and `sqlcmd`, three tools you'll use for our current task.</span></span>

* <span data-ttu-id="b5947-121">`az` は Azure CLI とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="b5947-121">`az` is also known as the Azure CLI.</span></span> <span data-ttu-id="b5947-122">Azure のリソースを使用するためのコマンド ライン インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="b5947-122">It's the command-line interface for working with Azure resources.</span></span> <span data-ttu-id="b5947-123">接続文字列など、データベースに関する情報を取得するために使用します。</span><span class="sxs-lookup"><span data-stu-id="b5947-123">You'll use this to get information about your database, including the connection string.</span></span>
* <span data-ttu-id="b5947-124">`jq` はコマンドライン JSON パーサーです。</span><span class="sxs-lookup"><span data-stu-id="b5947-124">`jq` is a command-line JSON parser.</span></span> <span data-ttu-id="b5947-125">`az` コマンドの出力をこのツールにパイプして、JSON 出力から重要なフィールドを抽出します。</span><span class="sxs-lookup"><span data-stu-id="b5947-125">You'll pipe output from `az` commands to this tool to extract important fields from JSON output.</span></span>
* <span data-ttu-id="b5947-126">`sqlcmd` を使用すると、SQL Server 上でステートメントを実行できます。</span><span class="sxs-lookup"><span data-stu-id="b5947-126">`sqlcmd` enables you to execute statements on SQL Server.</span></span> <span data-ttu-id="b5947-127">Azure SQL Database との対話型セッションを作成するために `sqlcmd` を使用します。</span><span class="sxs-lookup"><span data-stu-id="b5947-127">You'll use `sqlcmd` to create an interactive session with your Azure SQL Database.</span></span>

## <a name="get-information-about-your-azure-sql-database"></a><span data-ttu-id="b5947-128">Azure SQL Database に関する情報を入手する</span><span class="sxs-lookup"><span data-stu-id="b5947-128">Get information about your Azure SQL Database</span></span>

<span data-ttu-id="b5947-129">データベースに接続する前に、データベースが存在していて、オンラインであることを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="b5947-129">Before you connect to your database, it's a good idea to verify it exists and is online.</span></span>

<span data-ttu-id="b5947-130">ここでは、Cloud Shell を起動し、`az` ユーティリティを使用してデータベースを一覧表示し、最大サイズと状態などの **Logistics** データベースに関する一部の情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="b5947-130">Here, you bring up Cloud Shell and use the `az` utility to list your databases and show some information about the **Logistics** database, including its maximum size and status.</span></span>

1. <span data-ttu-id="b5947-131">ポータルの上部にある **[Cloud Shell]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b5947-131">From the portal, at the top, click **Cloud Shell**.</span></span> 
    <span data-ttu-id="b5947-132">![Cloud Shell を開く](../media-draft/open-cloud-shell.png)</span><span class="sxs-lookup"><span data-stu-id="b5947-132">![Opening Cloud Shell](../media-draft/open-cloud-shell.png)</span></span>
1. <span data-ttu-id="b5947-133">`az` コマンドを実行する場合、リソース グループの名前と Azure SQL 論理サーバーの名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="b5947-133">The `az` commands you'll run require the name of your resource group and the name of your Azure SQL logical server.</span></span> <span data-ttu-id="b5947-134">入力を省略できるように、この `azure configure` コマンドを実行して既定値として指定します。</span><span class="sxs-lookup"><span data-stu-id="b5947-134">To save typing, run this `azure configure` command to specify them as default values.</span></span>
    <span data-ttu-id="b5947-135">`contoso-logistics` を Azure SQL 論理サーバーの名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b5947-135">Replace `contoso-logistics` with the name of your Azure SQL logical server.</span></span>
    ```azurecli
    az configure --defaults group=logistics-db-rg sql-server=contoso-logistics
    ```
1. <span data-ttu-id="b5947-136">`az sql db list` を実行して、Azure SQL 論理サーバー上のすべてのデータベースを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b5947-136">Run `az sql db list` to list all databases on your Azure SQL logical server.</span></span>
    ```azurecli
    az sql db list
    ```
    <span data-ttu-id="b5947-137">出力として大きなブロックの JSON が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-137">You see a large block of JSON as output.</span></span> 
1. <span data-ttu-id="b5947-138">データベース名のみが必要なので、このコマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="b5947-138">Since we want just the database names, run the command a second time.</span></span> <span data-ttu-id="b5947-139">今回は、出力を `jq` にパイプして、名前フィールドのみを出力します。</span><span class="sxs-lookup"><span data-stu-id="b5947-139">This time, pipe the output to `jq` to print out only the name fields.</span></span>
    ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    <span data-ttu-id="b5947-140">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-140">You see this.</span></span>
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
    <span data-ttu-id="b5947-141">**Logistics** はデータベースです。</span><span class="sxs-lookup"><span data-stu-id="b5947-141">**Logistics** is your database.</span></span> <span data-ttu-id="b5947-142">SQL Server と同様に、**master** には、ログオン アカウントやシステム構成設定などのサーバー メタデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b5947-142">Like SQL Server, **master** includes server metadata such as logon accounts and system configuration settings.</span></span>
1. <span data-ttu-id="b5947-143">この `az sql db show` コマンドを実行すると、**Logistics** データベースに関する詳細情報が得られます。</span><span class="sxs-lookup"><span data-stu-id="b5947-143">Run this `az sql db show` command to get details about the **Logistics** database.</span></span> 
    ```azurecli
    az sql db show --name Logistics
    ```
    <span data-ttu-id="b5947-144">前回と同様に、出力として大きなブロックの JSON が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-144">As before, you see a large block of JSON as output.</span></span>
1. <span data-ttu-id="b5947-145">このコマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="b5947-145">Run the command a second time.</span></span> <span data-ttu-id="b5947-146">今回は、出力を `jq` にパイプして、**Logistics** データベースの名前、最大サイズ、状態のみに出力を制限します。</span><span class="sxs-lookup"><span data-stu-id="b5947-146">This time, pipe the output to `jq` to limit output to only the name, maximum size, and status of the **Logistics** database.</span></span>
    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```
    <span data-ttu-id="b5947-147">データベースがオンラインであり、約 2 GB のデータを保持できることがわかります。</span><span class="sxs-lookup"><span data-stu-id="b5947-147">You see that the database is online and can hold around 2 GB of data.</span></span>
    ```console
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a><span data-ttu-id="b5947-148">データベースに接続する</span><span class="sxs-lookup"><span data-stu-id="b5947-148">Connect to your database</span></span>

<span data-ttu-id="b5947-149">データベースの概要を少し理解したところで、`sqlcmd` を使用してデータベースに接続し、輸送ドライバーに関する情報を保持するテーブルを作成し、いくつかの基本的な CRUD 操作を実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="b5947-149">Now that you understand a bit about your database, let's connect to it using `sqlcmd`, create a table that holds information about transportation drivers, and perform a few basic CRUD operations.</span></span>

<span data-ttu-id="b5947-150">CRUD は、**作成** (create)、**読み取り** (read)、**更新** (update)、**削除** (delete) の略です。</span><span class="sxs-lookup"><span data-stu-id="b5947-150">Remember that CRUD stands for **create**, **read**, **update**, and **delete**.</span></span> <span data-ttu-id="b5947-151">これらはテーブル データに対して実行する操作を指します。また、アプリに必要な 4 つの基本操作です。</span><span class="sxs-lookup"><span data-stu-id="b5947-151">These refer to operations you perform on table data, and are the four basic operations you need for your app.</span></span> <span data-ttu-id="b5947-152">今が各操作を実行できることを確認するよい機会です。</span><span class="sxs-lookup"><span data-stu-id="b5947-152">Now's a good time to verify you can perform each of them.</span></span>

1. <span data-ttu-id="b5947-153">この `az sql db show-connection-string` コマンドを実行すると、**Logistics** データベースに対する接続文字列を `sqlcmd` に使用できる形式で取得できます。</span><span class="sxs-lookup"><span data-stu-id="b5947-153">Run this `az sql db show-connection-string` command to get the connection string to the **Logistics** database in a format that `sqlcmd` can use.</span></span>
    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```
    <span data-ttu-id="b5947-154">次のような内容が出力されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-154">Your output resembles this.</span></span>
    ```console
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```
1. <span data-ttu-id="b5947-155">前の手順で `sqlcmd` ステートメントを実行して、対話型セッションを作成します。</span><span class="sxs-lookup"><span data-stu-id="b5947-155">Run the `sqlcmd` statement from the previous step to create an interactive session.</span></span>
    <span data-ttu-id="b5947-156">囲んでいる引用符を削除し、`<username>` と `<password>` をデータベースの作成時に指定したユーザー名とパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="b5947-156">Remove the surrounding quotes and replace `<username>` and `<password>` with the username and password you specified when you created your database.</span></span> <span data-ttu-id="b5947-157">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="b5947-157">Here's an example.</span></span>
    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```
    > [!TIP]
    > <span data-ttu-id="b5947-158">"&" と他の特殊文字が処理命令として解釈されないように、パスワードを引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="b5947-158">Place your password in quotes so that "&" and other special characters aren't interpreted as processing instructions.</span></span>
1. <span data-ttu-id="b5947-159">`sqlcmd` セッションから、`Drivers` という名前のテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="b5947-159">From your `sqlcmd` session, create a table named `Drivers`.</span></span>
    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255) );
    GO
    ```
    <span data-ttu-id="b5947-160">このテーブルには、一意の識別子、ドライバーの姓、名、ドライバーの出発地 (市区町村) という 4 つの列があります。</span><span class="sxs-lookup"><span data-stu-id="b5947-160">The table contains four columns: a unique identifier, the driver's last and first name, and the driver's city of origin.</span></span>
    > [!NOTE]
    > <span data-ttu-id="b5947-161">ここに表示されている言語は、Transact-SQL または T-SQL です。</span><span class="sxs-lookup"><span data-stu-id="b5947-161">The language you see here is Transact-SQL, or T-SQL.</span></span>
1. <span data-ttu-id="b5947-162">`Drivers` テーブルが存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="b5947-162">Verify that the `Drivers` table exists.</span></span>
    ```sql
    SELECT name FROM sys.tables;
    GO
    ```
    <span data-ttu-id="b5947-163">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-163">You see this.</span></span>
    ```console
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers
    
    (1 rows affected)
    ```
1. <span data-ttu-id="b5947-164">この `INSERT` T-SQL ステートメントを実行して、サンプル行をテーブルに追加します。</span><span class="sxs-lookup"><span data-stu-id="b5947-164">Run this `INSERT` T-SQL statement to add a sample row to the table.</span></span> <span data-ttu-id="b5947-165">これは**作成**操作です。</span><span class="sxs-lookup"><span data-stu-id="b5947-165">This is the **create** operation.</span></span>
    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Orton', 'Erick', 'Springfield');
    GO
    ```
    <span data-ttu-id="b5947-166">これは操作が成功したことを示すために表示されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-166">You see this to indicate the operation succeeded.</span></span>
    ```console
    (1 rows affected)
    ```
1. <span data-ttu-id="b5947-167">この `SELECT` T-SQL ステートメントを実行して、テーブルのすべての行から`DriverID` 列を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="b5947-167">Run this `SELECT` T-SQL statement list the `DriverID` column from all rows in the table.</span></span> <span data-ttu-id="b5947-168">これは**読み取り**操作です。</span><span class="sxs-lookup"><span data-stu-id="b5947-168">This is the **read** operation.</span></span>
    ```sql
    SELECT DriverID FROM Drivers;
    GO
    ```
    <span data-ttu-id="b5947-169">1 つの結果として、前の手順で作成した行の `DriverID` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-169">You see one result, the `DriverID` for the row you created in the previous step.</span></span>
    ```console
    DriverID
    -----------
            123
    
    (1 rows affected)
    ```
1. <span data-ttu-id="b5947-170">この `UPDATE` T-SQL ステートメントを実行して、`DriverID` が 123 のドライバーについて出発地を "Springfield" から "Springfield, OR" に変更します。</span><span class="sxs-lookup"><span data-stu-id="b5947-170">Run this `UPDATE` T-SQL statement to change the city of origin from "Springfield" to "Springfield, OR" for the driver with a `DriverID` of 123.</span></span> <span data-ttu-id="b5947-171">これは**更新**操作です。</span><span class="sxs-lookup"><span data-stu-id="b5947-171">This is the **update** operation.</span></span>
    ```sql
    UPDATE Drivers SET OriginCity='Springfield, OR' WHERE DriverID=123;
    GO
    ```
    <span data-ttu-id="b5947-172">次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="b5947-172">You see this.</span></span>
    ```console
    (1 rows affected)
    ```
1. <span data-ttu-id="b5947-173">この `DELETE` T-SQL ステートメントを実行して、レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="b5947-173">Run this `DELETE` T-SQL statement to delete the record.</span></span> <span data-ttu-id="b5947-174">これは**削除**操作です。</span><span class="sxs-lookup"><span data-stu-id="b5947-174">This is the **delete** operation.</span></span>
    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```
    
    ```console
    (1 rows affected)
    ```
1. <span data-ttu-id="b5947-175">この `SELECT` T-SQL ステートメントを実行して、`Drivers` テーブルが空であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="b5947-175">Run this `SELECT` T-SQL statement to verify the `Drivers` table is empty.</span></span>
    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```
    <span data-ttu-id="b5947-176">テーブルに行がないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="b5947-176">You see that the table contains no rows.</span></span>
    ```console
    -----------
              0
    
    (1 rows affected)
    ```

## <a name="summary"></a><span data-ttu-id="b5947-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="b5947-177">Summary</span></span>

<span data-ttu-id="b5947-178">Cloud Shell から Azure SQL Database を操作する方法を学んだので、SQL Server Management Studio、Visual Studio などから、好みの SQL 管理ツールの接続文字列を取得できます。</span><span class="sxs-lookup"><span data-stu-id="b5947-178">Now that you have the hang of working with Azure SQL Database from Cloud Shell, you can get the connection string for your favorite SQL management tool &ndash; whether that's from SQL Server Management Studio, Visual Studio, or something else.</span></span>

<span data-ttu-id="b5947-179">Cloud Shell を使用すると、Azure のリソースに簡単にアクセスして操作できます。</span><span class="sxs-lookup"><span data-stu-id="b5947-179">Cloud Shell makes it easy to access and work with your Azure resources.</span></span> <span data-ttu-id="b5947-180">Cloud Shell はブラウザーベースなので、Windows、macOS、または Linux から、つまり基本的には Web ブラウザーを使用する任意のシステムからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="b5947-180">Because Cloud Shell is browser-based, you can access it from Windows, macOS, or Linux &ndash; essentially any system with a web browser.</span></span>

<span data-ttu-id="b5947-181">ここでは、Azure CLI コマンドを実行して SQL Azure データベースに関する情報を取得する実践的な経験を得ました。</span><span class="sxs-lookup"><span data-stu-id="b5947-181">You gained some hands-on experience running Azure CLI commands to get information about your SQL Azure Database.</span></span> <span data-ttu-id="b5947-182">さらに、T-SQL のスキルについても実践しました。</span><span class="sxs-lookup"><span data-stu-id="b5947-182">As a bonus, you practiced your T-SQL skills.</span></span>

<span data-ttu-id="b5947-183">最後に、データベースの破棄方法を要約して説明します。</span><span class="sxs-lookup"><span data-stu-id="b5947-183">Finally, let's wrap up and see how to tear down your database.</span></span>