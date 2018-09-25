<span data-ttu-id="57fff-101">データベースをアプリに接続する前に、データベースに接続し、基本的なテーブルを追加し、サンプル データを操作できることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="57fff-101">Before you connect the database to your app, you want to check that you can connect to it, add a basic table, and work with sample data.</span></span>

<span data-ttu-id="57fff-102">Azure SQL Database のインフラストラクチャ、ソフトウェア更新プログラム、修正プログラムは Microsoft で保持しています。</span><span class="sxs-lookup"><span data-stu-id="57fff-102">We maintain the infrastructure, software updates, and patches for your Azure SQL database.</span></span> <span data-ttu-id="57fff-103">Azure SQL Database のそれ以外の部分については、他の SQL Server インストールと同様に扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="57fff-103">Beyond that, you can treat your Azure SQL database like you would any other SQL Server installation.</span></span> <span data-ttu-id="57fff-104">たとえば、Visual Studio、SQL Server Management Studio、SQL Server Operations Studio、またはその他のツールを使用して、Azure SQL Database を管理することができます。</span><span class="sxs-lookup"><span data-stu-id="57fff-104">For example, you can use Visual Studio, SQL Server Management Studio, SQL Server Operations Studio, or other tools to manage your Azure SQL database.</span></span>

<span data-ttu-id="57fff-105">データベースにアクセスしてアプリに接続する方法は任意です。</span><span class="sxs-lookup"><span data-stu-id="57fff-105">How you access your database and connect it to your app is up to you.</span></span> <span data-ttu-id="57fff-106">しかし、データベースの操作経験をある程度積むために、ここではポータルから直接接続し、テーブルを作成し、基本的な CRUD 操作をいくつか実行します。</span><span class="sxs-lookup"><span data-stu-id="57fff-106">But to get some experience working with your database, here you'll connect to it directly from the portal, create a table, and run a few basic CRUD operations.</span></span> <span data-ttu-id="57fff-107">学習内容:</span><span class="sxs-lookup"><span data-stu-id="57fff-107">You'll learn:</span></span>

- <span data-ttu-id="57fff-108">Cloud Shell の概要と、ポータルからアクセスする方法。</span><span class="sxs-lookup"><span data-stu-id="57fff-108">What Cloud Shell is and how to access it from the portal.</span></span>
- <span data-ttu-id="57fff-109">接続文字列などのデータベースに関する情報に Azure CLI からアクセスする方法。</span><span class="sxs-lookup"><span data-stu-id="57fff-109">How to access information about your database from the Azure CLI, including connection strings.</span></span>
- <span data-ttu-id="57fff-110">`sqlcmd` を使用してデータベースに接続する方法。</span><span class="sxs-lookup"><span data-stu-id="57fff-110">How to connect to your database using `sqlcmd`.</span></span>
- <span data-ttu-id="57fff-111">基本的なテーブルといくつかのサンプル データを使用してデータベースを初期化する方法。</span><span class="sxs-lookup"><span data-stu-id="57fff-111">How to initialize your database with a basic table and some sample data.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="57fff-112">Azure Cloud Shell とは</span><span class="sxs-lookup"><span data-stu-id="57fff-112">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="57fff-113">Azure Cloud Shell は、Azure リソースを開発および管理するための、ブラウザー ベースのシェル環境です。</span><span class="sxs-lookup"><span data-stu-id="57fff-113">Azure Cloud Shell is a browser-based shell experience to manage and develop Azure resources.</span></span> <span data-ttu-id="57fff-114">Cloud Shell は、クラウド上で動作する対話型コンソールと考えてください。</span><span class="sxs-lookup"><span data-stu-id="57fff-114">Think of Cloud Shell as an interactive console that runs in the cloud.</span></span>

<span data-ttu-id="57fff-115">バックグラウンドでは Cloud Shell は Linux 上で動作しています。</span><span class="sxs-lookup"><span data-stu-id="57fff-115">Behind the scenes, Cloud Shell runs on Linux.</span></span> <span data-ttu-id="57fff-116">しかし、Linux 環境と Windows 環境のどちらを好むかによって、Bash と PowerShell のいずれかから選択できます。</span><span class="sxs-lookup"><span data-stu-id="57fff-116">But depending on whether you prefer a Linux or Windows environment, you have two experiences to choose from: Bash and PowerShell.</span></span>

<span data-ttu-id="57fff-117">Cloud Shell は任意の場所からアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="57fff-117">Cloud Shell is accessible from anywhere.</span></span> <span data-ttu-id="57fff-118">ポータル以外にも、[shell.azure.com](https://shell.azure.com/)、Azure mobile app、または Visual Studio Code からも Cloud Shell にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="57fff-118">Besides the portal, you can also access Cloud Shell from [shell.azure.com](https://shell.azure.com/), the Azure mobile app, or from Visual Studio Code.</span></span> <span data-ttu-id="57fff-119">右側のパネルは、この演習の間に使用する Cloud Shell ターミナルです。</span><span class="sxs-lookup"><span data-stu-id="57fff-119">The panel on the right is a Cloud Shell terminal for you to use during this exercise.</span></span>

<span data-ttu-id="57fff-120">Cloud Shell には、一般的なツールとテキスト エディターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="57fff-120">Cloud Shell includes popular tools and text editors.</span></span> <span data-ttu-id="57fff-121">次に、このドキュメントのタスクに使用する 3 つのツール `az`、`jq`、`sqlcmd` について簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="57fff-121">Here's a brief look at `az`, `jq`, and `sqlcmd`, three tools you'll use for our current task.</span></span>

- <span data-ttu-id="57fff-122">`az` は Azure CLI とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="57fff-122">`az` is also known as the Azure CLI.</span></span> <span data-ttu-id="57fff-123">Azure のリソースを使用するためのコマンド ライン インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="57fff-123">It's the command-line interface for working with Azure resources.</span></span> <span data-ttu-id="57fff-124">接続文字列など、データベースに関する情報を取得するために使用します。</span><span class="sxs-lookup"><span data-stu-id="57fff-124">You'll use this to get information about your database, including the connection string.</span></span>
- <span data-ttu-id="57fff-125">`jq` はコマンドライン JSON パーサーです。</span><span class="sxs-lookup"><span data-stu-id="57fff-125">`jq` is a command-line JSON parser.</span></span> <span data-ttu-id="57fff-126">`az` コマンドの出力をこのツールにパイプして、JSON 出力から重要なフィールドを抽出します。</span><span class="sxs-lookup"><span data-stu-id="57fff-126">You'll pipe output from `az` commands to this tool to extract important fields from JSON output.</span></span>
- <span data-ttu-id="57fff-127">`sqlcmd` を使用すると、SQL Server 上でステートメントを実行できます。</span><span class="sxs-lookup"><span data-stu-id="57fff-127">`sqlcmd` enables you to execute statements on SQL Server.</span></span> <span data-ttu-id="57fff-128">Azure SQL Database との対話型セッションを作成するために `sqlcmd` を使用します。</span><span class="sxs-lookup"><span data-stu-id="57fff-128">You'll use `sqlcmd` to create an interactive session with your Azure SQL database.</span></span>

## <a name="get-information-about-your-azure-sql-database"></a><span data-ttu-id="57fff-129">Azure SQL Database に関する情報を入手する</span><span class="sxs-lookup"><span data-stu-id="57fff-129">Get information about your Azure SQL database</span></span>

<span data-ttu-id="57fff-130">データベースに接続する前に、データベースが存在していて、オンラインであることを確認することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="57fff-130">Before you connect to your database, it's a good idea to verify it exists and is online.</span></span>

<span data-ttu-id="57fff-131">ここでは、`az` ユーティリティを使用してデータベースを一覧表示し、最大サイズと状態などの **Logistics** データベースに関する一部の情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="57fff-131">Here, you use the `az` utility to list your databases and show some information about the **Logistics** database, including its maximum size and status.</span></span>

1. <span data-ttu-id="57fff-132">`az` コマンドを実行する場合、リソース グループの名前と Azure SQL 論理サーバーの名前が必要です。</span><span class="sxs-lookup"><span data-stu-id="57fff-132">The `az` commands you'll run require the name of your resource group and the name of your Azure SQL logical server.</span></span> <span data-ttu-id="57fff-133">入力を省略できるように、この `azure configure` コマンドを実行して既定値として指定します。</span><span class="sxs-lookup"><span data-stu-id="57fff-133">To save typing, run this `azure configure` command to specify them as default values.</span></span>
    <span data-ttu-id="57fff-134">`<server-name>` を Azure SQL 論理サーバーの名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="57fff-134">Replace `<server-name>` with the name of your Azure SQL logical server.</span></span> <span data-ttu-id="57fff-135">ポータルにあるブレードによっては、FQDN (servername.database.windows.net) として表示されることがありますが、必要なのは .database.windows.net サフィックスを除く論理名だけです。</span><span class="sxs-lookup"><span data-stu-id="57fff-135">Note that depending on the blade you are on in the portal this may show as a FQDN (servername.database.windows.net), but you only need the logical name without the .database.windows.net suffix.</span></span>

    ```azurecli
    az configure --defaults group=<rgn>[sandbox resource group name]</rgn> sql-server=<server-name>
    ```

1. <span data-ttu-id="57fff-136">`az sql db list` を実行して、Azure SQL 論理サーバー上のすべてのデータベースを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="57fff-136">Run `az sql db list` to list all databases on your Azure SQL logical server.</span></span>

    ```azurecli
    az sql db list
    ```
    <span data-ttu-id="57fff-137">出力として大きなブロックの JSON が表示されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-137">You see a large block of JSON as output.</span></span>

1. <span data-ttu-id="57fff-138">データベース名のみが必要なので、このコマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="57fff-138">Since we want just the database names, run the command a second time.</span></span> <span data-ttu-id="57fff-139">今回は、出力を `jq` にパイプして、名前フィールドのみを出力します。</span><span class="sxs-lookup"><span data-stu-id="57fff-139">This time, pipe the output to `jq` to print out only the name fields.</span></span>
   
     ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    
    <span data-ttu-id="57fff-140">次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-140">You should see this output.</span></span>
    
    ```json
    [
      {
        "name": "Logistics"
      },
      {
        "name": "master"
      }
    ]
    ```

    <span data-ttu-id="57fff-141">**Logistics** はデータベースです。</span><span class="sxs-lookup"><span data-stu-id="57fff-141">**Logistics** is your database.</span></span> <span data-ttu-id="57fff-142">SQL Server と同様に、**master** には、サインイン アカウントやシステム構成設定などのサーバー メタデータが含まれています。</span><span class="sxs-lookup"><span data-stu-id="57fff-142">Like SQL Server, **master** includes server metadata, such as sign-in accounts and system configuration settings.</span></span>

1. <span data-ttu-id="57fff-143">この `az sql db show` コマンドを実行すると、**Logistics** データベースに関する詳細情報が得られます。</span><span class="sxs-lookup"><span data-stu-id="57fff-143">Run this `az sql db show` command to get details about the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics
    ```

    <span data-ttu-id="57fff-144">前回と同様に、出力として大きなブロックの JSON が表示されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-144">As before, you see a large block of JSON as output.</span></span>

1. <span data-ttu-id="57fff-145">このコマンドをもう一度実行します。</span><span class="sxs-lookup"><span data-stu-id="57fff-145">Run the command a second time.</span></span> <span data-ttu-id="57fff-146">今回は、出力を `jq` にパイプして、**Logistics** データベースの名前、最大サイズ、状態のみに出力を制限します。</span><span class="sxs-lookup"><span data-stu-id="57fff-146">This time, pipe the output to `jq` to limit output to only the name, maximum size, and status of the **Logistics** database.</span></span>

    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```

    <span data-ttu-id="57fff-147">データベースがオンラインであり、約 2 GB のデータを保持できることがわかります。</span><span class="sxs-lookup"><span data-stu-id="57fff-147">You see that the database is online and can hold around 2 GB of data.</span></span>

    ```json
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a><span data-ttu-id="57fff-148">データベースに接続する</span><span class="sxs-lookup"><span data-stu-id="57fff-148">Connect to your database</span></span>

<span data-ttu-id="57fff-149">データベースの概要を少し理解したところで、`sqlcmd` を使用してデータベースに接続し、輸送ドライバーに関する情報を保持するテーブルを作成し、いくつかの基本的な CRUD 操作を実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="57fff-149">Now that you understand a bit about your database, let's connect to it using `sqlcmd`, create a table that holds information about transportation drivers, and perform a few basic CRUD operations.</span></span>

<span data-ttu-id="57fff-150">CRUD は、**作成** (create)、**読み取り** (read)、**更新** (update)、**削除** (delete) の略であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="57fff-150">Remember that CRUD stands for **create**, **read**, **update**, and **delete**.</span></span> <span data-ttu-id="57fff-151">これらの用語はテーブル データに対して実行する操作を指します。また、アプリに必要な 4 つの基本操作です。</span><span class="sxs-lookup"><span data-stu-id="57fff-151">These terms refer to operations you perform on table data and are the four basic operations you need for your app.</span></span> <span data-ttu-id="57fff-152">今が各操作を実行できることを確認するよい機会です。</span><span class="sxs-lookup"><span data-stu-id="57fff-152">Now's a good time to verify you can perform each of them.</span></span>

1. <span data-ttu-id="57fff-153">この `az sql db show-connection-string` コマンドを実行すると、**Logistics** データベースに対する接続文字列を `sqlcmd` に使用できる形式で取得できます。</span><span class="sxs-lookup"><span data-stu-id="57fff-153">Run this `az sql db show-connection-string` command to get the connection string to the **Logistics** database in a format that `sqlcmd` can use.</span></span>

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```

    <span data-ttu-id="57fff-154">次のような内容が出力されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-154">Your output resembles this.</span></span>

    ```output
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```

1. <span data-ttu-id="57fff-155">前の手順の出力から `sqlcmd` ステートメントを実行して、対話型セッションを作成します。</span><span class="sxs-lookup"><span data-stu-id="57fff-155">Run the `sqlcmd` statement from the output of the previous step to create an interactive session.</span></span> <span data-ttu-id="57fff-156">囲んでいる引用符を削除し、`<username>` と `<password>` をデータベースの作成時に指定したユーザー名とパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="57fff-156">Remove the surrounding quotes and replace `<username>` and `<password>` with the username and password you specified when you created your database.</span></span> <span data-ttu-id="57fff-157">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="57fff-157">Here's an example.</span></span>

    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```

    > [!TIP]
    > <span data-ttu-id="57fff-158">"&" と他の特殊文字が処理命令として解釈されないように、パスワードを引用符で囲みます。</span><span class="sxs-lookup"><span data-stu-id="57fff-158">Place your password in quotes so that "&" and other special characters aren't interpreted as processing instructions.</span></span>

1. <span data-ttu-id="57fff-159">`sqlcmd` セッションから、`Drivers` という名前のテーブルを作成します。</span><span class="sxs-lookup"><span data-stu-id="57fff-159">From your `sqlcmd` session, create a table named `Drivers`.</span></span>

    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255));
    GO
    ```

    <span data-ttu-id="57fff-160">このテーブルには、一意の識別子、ドライバーの姓、名、ドライバーの出発地 (市区町村) という 4 つの列があります。</span><span class="sxs-lookup"><span data-stu-id="57fff-160">The table contains four columns: a unique identifier, the driver's last and first name, and the driver's city of origin.</span></span>

    > [!NOTE]
    > <span data-ttu-id="57fff-161">ここに表示されている言語は、Transact-SQL または T-SQL です。</span><span class="sxs-lookup"><span data-stu-id="57fff-161">The language you see here is Transact-SQL, or T-SQL.</span></span>

1. <span data-ttu-id="57fff-162">`Drivers` テーブルが存在することを確認します。</span><span class="sxs-lookup"><span data-stu-id="57fff-162">Verify that the `Drivers` table exists.</span></span>

    ```sql
    SELECT name FROM sys.tables;
    GO
    ```

    <span data-ttu-id="57fff-163">次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-163">You should see this output.</span></span>

    ```output
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers

    (1 rows affected)
    ```

1. <span data-ttu-id="57fff-164">この `INSERT` T-SQL ステートメントを実行して、サンプル行をテーブルに追加します。</span><span class="sxs-lookup"><span data-stu-id="57fff-164">Run this `INSERT` T-SQL statement to add a sample row to the table.</span></span> <span data-ttu-id="57fff-165">これは**作成**操作です。</span><span class="sxs-lookup"><span data-stu-id="57fff-165">This is the **create** operation.</span></span>

    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Zirne', 'Laura', 'Springfield');
    GO
    ```

    <span data-ttu-id="57fff-166">これは操作が成功したことを示すために表示されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-166">You see this to indicate the operation succeeded.</span></span>

    ```output
    (1 rows affected)
    ```

1. <span data-ttu-id="57fff-167">この `SELECT` T-SQL ステートメントを実行して、テーブルのすべての行から `DriverID` 列および `OriginCity` 列を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="57fff-167">Run this `SELECT` T-SQL statement to list the `DriverID` and `OriginCity` columns from all rows in the table.</span></span> <span data-ttu-id="57fff-168">これは**読み取り**操作です。</span><span class="sxs-lookup"><span data-stu-id="57fff-168">This is the **read** operation.</span></span>

    ```sql
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    <span data-ttu-id="57fff-169">1 つの結果として、前の手順で作成した行の `DriverID` および `OriginCity` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-169">You see one result with the `DriverID` and `OriginCity` for the row you created in the previous step.</span></span>

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Springfield

    (1 rows affected)
    ```

1. <span data-ttu-id="57fff-170">この `UPDATE` T-SQL ステートメントを実行して、`DriverID` が 123 のドライバーについて出発地を "Springfield" から "Boston" に変更します。</span><span class="sxs-lookup"><span data-stu-id="57fff-170">Run this `UPDATE` T-SQL statement to change the city of origin from "Springfield" to "Boston" for the driver with a `DriverID` of 123.</span></span> <span data-ttu-id="57fff-171">これは**更新**操作です。</span><span class="sxs-lookup"><span data-stu-id="57fff-171">This is the **update** operation.</span></span>

    ```sql
    UPDATE Drivers SET OriginCity='Boston' WHERE DriverID=123;
    GO
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    <span data-ttu-id="57fff-172">次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="57fff-172">You should see the following output.</span></span> <span data-ttu-id="57fff-173">`OriginCity` でボストンへの更新がどのように反映されているかを確認します。</span><span class="sxs-lookup"><span data-stu-id="57fff-173">Notice how the `OriginCity` reflects the update to Boston.</span></span>

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Boston

    (1 rows affected)
    ```

1. <span data-ttu-id="57fff-174">この `DELETE` T-SQL ステートメントを実行して、レコードを削除します。</span><span class="sxs-lookup"><span data-stu-id="57fff-174">Run this `DELETE` T-SQL statement to delete the record.</span></span> <span data-ttu-id="57fff-175">これは**削除**操作です。</span><span class="sxs-lookup"><span data-stu-id="57fff-175">This is the **delete** operation.</span></span>

    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```

    ```output
    (1 rows affected)
    ```

1. <span data-ttu-id="57fff-176">この `SELECT` T-SQL ステートメントを実行して、`Drivers` テーブルが空であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="57fff-176">Run this `SELECT` T-SQL statement to verify the `Drivers` table is empty.</span></span>

    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```

    <span data-ttu-id="57fff-177">テーブルに行がないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="57fff-177">You see that the table contains no rows.</span></span>

    ```output
    -----------
              0

    (1 rows affected)
    ```

<span data-ttu-id="57fff-178">Cloud Shell から Azure SQL Database を操作する方法を学んだので、SQL Server Management Studio、Visual Studio などから、好みの SQL 管理ツール&ndash;の接続文字列を取得できます。</span><span class="sxs-lookup"><span data-stu-id="57fff-178">Now that you have the hang of working with Azure SQL Database from Cloud Shell, you can get the connection string for your favorite SQL management tool &ndash; whether that's from SQL Server Management Studio, Visual Studio, or something else.</span></span>

<span data-ttu-id="57fff-179">Cloud Shell を使用すると、Azure のリソースに簡単にアクセスして操作できます。</span><span class="sxs-lookup"><span data-stu-id="57fff-179">Cloud Shell makes it easy to access and work with your Azure resources.</span></span> <span data-ttu-id="57fff-180">Cloud Shell はブラウザーベースなので、Windows、macOS、または Linux から、つまり、基本的には Web ブラウザーを使用する任意のシステムからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="57fff-180">Because Cloud Shell is browser-based, you can access it from Windows, macOS, or Linux &ndash; essentially any system with a web browser.</span></span>

<span data-ttu-id="57fff-181">ここでは、Azure CLI コマンドを実行して SQL Azure SQL Database に関する情報を取得する実践的な経験を得ました。</span><span class="sxs-lookup"><span data-stu-id="57fff-181">You gained some hands-on experience running Azure CLI commands to get information about your Azure SQL database.</span></span> <span data-ttu-id="57fff-182">さらに、T-SQL のスキルについても実践しました。</span><span class="sxs-lookup"><span data-stu-id="57fff-182">As a bonus, you practiced your T-SQL skills.</span></span>

<span data-ttu-id="57fff-183">最後に、データベースの破棄方法を要約して説明します。</span><span class="sxs-lookup"><span data-stu-id="57fff-183">Finally, let's wrap up and see how to tear down your database.</span></span>
