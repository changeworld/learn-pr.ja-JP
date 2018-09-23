データベースをアプリに接続する前に、データベースに接続し、基本的なテーブルを追加し、サンプル データを操作できることを確認する必要があります。

Azure SQL Database のインフラストラクチャ、ソフトウェア更新プログラム、修正プログラムは Microsoft で保持しています。 Azure SQL Database のそれ以外の部分については、他の SQL Server インストールと同様に扱うことができます。 たとえば、Visual Studio、SQL Server Management Studio、SQL Server Operations Studio、またはその他のツールを使用して、Azure SQL Database を管理することができます。

データベースにアクセスしてアプリに接続する方法は任意です。 しかし、データベースの操作経験をある程度積むために、ここではポータルから直接接続し、テーブルを作成し、基本的な CRUD 操作をいくつか実行します。 学習内容:

- Cloud Shell の概要と、ポータルからアクセスする方法。
- 接続文字列などのデータベースに関する情報に Azure CLI からアクセスする方法。
- `sqlcmd` を使用してデータベースに接続する方法。
- 基本的なテーブルといくつかのサンプル データを使用してデータベースを初期化する方法。

## <a name="what-is-azure-cloud-shell"></a>Azure Cloud Shell とは

Azure Cloud Shell は、Azure リソースを開発および管理するための、ブラウザー ベースのシェル環境です。 Cloud Shell は、クラウド上で動作する対話型コンソールと考えてください。

バックグラウンドでは Cloud Shell は Linux 上で動作しています。 しかし、Linux 環境と Windows 環境のどちらを好むかによって、Bash と PowerShell のいずれかから選択できます。

Cloud Shell は任意の場所からアクセスできます。 ポータル以外にも、[shell.azure.com](https://shell.azure.com/)、Azure mobile app、または Visual Studio Code からも Cloud Shell にアクセスできます。 右側のパネルは、この演習の間に使用する Cloud Shell ターミナルです。

Cloud Shell には、一般的なツールとテキスト エディターが含まれています。 次に、このドキュメントのタスクに使用する 3 つのツール `az`、`jq`、`sqlcmd` について簡単に説明します。

- `az` は Azure CLI とも呼ばれます。 Azure のリソースを使用するためのコマンド ライン インターフェイスです。 接続文字列など、データベースに関する情報を取得するために使用します。
- `jq` はコマンドライン JSON パーサーです。 `az` コマンドの出力をこのツールにパイプして、JSON 出力から重要なフィールドを抽出します。
- `sqlcmd` を使用すると、SQL Server 上でステートメントを実行できます。 Azure SQL Database との対話型セッションを作成するために `sqlcmd` を使用します。

## <a name="get-information-about-your-azure-sql-database"></a>Azure SQL Database に関する情報を入手する

データベースに接続する前に、データベースが存在していて、オンラインであることを確認することをお勧めします。

ここでは、`az` ユーティリティを使用してデータベースを一覧表示し、最大サイズと状態などの **Logistics** データベースに関する一部の情報を表示します。

1. `az` コマンドを実行する場合、リソース グループの名前と Azure SQL 論理サーバーの名前が必要です。 入力を省略できるように、この `azure configure` コマンドを実行して既定値として指定します。
    `<server-name>` を Azure SQL 論理サーバーの名前に置き換えます。 ポータルにあるブレードによっては、FQDN (servername.database.windows.net) として表示されることがありますが、必要なのは .database.windows.net サフィックスを除く論理名だけです。

    ```azurecli
    az configure --defaults group=<rgn>[Sandbox resource group name]</rgn> sql-server=<server-name>
    ```

1. `az sql db list` を実行して、Azure SQL 論理サーバー上のすべてのデータベースを一覧表示します。

    ```azurecli
    az sql db list
    ```
    出力として大きなブロックの JSON が表示されます。

1. データベース名のみが必要なので、このコマンドをもう一度実行します。 今回は、出力を `jq` にパイプして、名前フィールドのみを出力します。
   
     ```azurecli
    az sql db list | jq '[.[] | {name: .name}]'
    ```
    
    次のように出力されます。
    
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

    **Logistics** はデータベースです。 SQL Server と同様に、**master** には、サインイン アカウントやシステム構成設定などのサーバー メタデータが含まれています。

1. この `az sql db show` コマンドを実行すると、**Logistics** データベースに関する詳細情報が得られます。

    ```azurecli
    az sql db show --name Logistics
    ```

    前回と同様に、出力として大きなブロックの JSON が表示されます。

1. このコマンドをもう一度実行します。 今回は、出力を `jq` にパイプして、**Logistics** データベースの名前、最大サイズ、状態のみに出力を制限します。

    ```azurecli
    az sql db show --name Logistics | jq '{name: .name, maxSizeBytes: .maxSizeBytes, status: .status}'
    ```

    データベースがオンラインであり、約 2 GB のデータを保持できることがわかります。

    ```json
    {
      "name": "Logistics",
      "maxSizeBytes": 2147483648,
      "status": "Online"
    }
    ```

## <a name="connect-to-your-database"></a>データベースに接続する

データベースの概要を少し理解したところで、`sqlcmd` を使用してデータベースに接続し、輸送ドライバーに関する情報を保持するテーブルを作成し、いくつかの基本的な CRUD 操作を実行してみましょう。

CRUD は、**作成** (create)、**読み取り** (read)、**更新** (update)、**削除** (delete) の略であることに注意してください。 これらの用語はテーブル データに対して実行する操作を指します。また、アプリに必要な 4 つの基本操作です。 今が各操作を実行できることを確認するよい機会です。

1. この `az sql db show-connection-string` コマンドを実行すると、**Logistics** データベースに対する接続文字列を `sqlcmd` に使用できる形式で取得できます。

    ```azurecli
    az sql db show-connection-string --client sqlcmd --name Logistics
    ```

    次のような内容が出力されます。

    ```output
    "sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U <username> -P <password> -N -l 30"
    ```

1. 前の手順の出力から `sqlcmd` ステートメントを実行して、対話型セッションを作成します。 囲んでいる引用符を削除し、`<username>` と `<password>` をデータベースの作成時に指定したユーザー名とパスワードに置き換えます。 次に例を示します。

    ```console
    sqlcmd -S tcp:contoso-1.database.windows.net,1433 -d Logistics -U martina -P "password1234$" -N -l 30
    ```

    > [!TIP]
    > "&" と他の特殊文字が処理命令として解釈されないように、パスワードを引用符で囲みます。

1. `sqlcmd` セッションから、`Drivers` という名前のテーブルを作成します。

    ```sql
    CREATE TABLE Drivers (DriverID int, LastName varchar(255), FirstName varchar(255), OriginCity varchar(255));
    GO
    ```

    このテーブルには、一意の識別子、ドライバーの姓、名、ドライバーの出発地 (市区町村) という 4 つの列があります。

    > [!NOTE]
    > ここに表示されている言語は、Transact-SQL または T-SQL です。

1. `Drivers` テーブルが存在することを確認します。

    ```sql
    SELECT name FROM sys.tables;
    GO
    ```

    次のように出力されます。

    ```output
    name
    --------------------------------------------------------------------------------------------------------------------------------
    Drivers

    (1 rows affected)
    ```

1. この `INSERT` T-SQL ステートメントを実行して、サンプル行をテーブルに追加します。 これは**作成**操作です。

    ```sql
    INSERT INTO Drivers (DriverID, LastName, FirstName, OriginCity) VALUES (123, 'Zirne', 'Laura', 'Springfield');
    GO
    ```

    これは操作が成功したことを示すために表示されます。

    ```output
    (1 rows affected)
    ```

1. この `SELECT` T-SQL ステートメントを実行して、テーブルのすべての行から `DriverID` 列および `OriginCity` 列を一覧表示します。 これは**読み取り**操作です。

    ```sql
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    1 つの結果として、前の手順で作成した行の `DriverID` および `OriginCity` が表示されます。

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Springfield

    (1 rows affected)
    ```

1. この `UPDATE` T-SQL ステートメントを実行して、`DriverID` が 123 のドライバーについて出発地を "Springfield" から "Boston" に変更します。 これは**更新**操作です。

    ```sql
    UPDATE Drivers SET OriginCity='Boston' WHERE DriverID=123;
    GO
    SELECT DriverID, OriginCity FROM Drivers;
    GO
    ```

    次の出力が表示されます。 `OriginCity` でボストンへの更新がどのように反映されているかを確認します。

    ```output
    DriverID    OriginCity
    ----------- --------------------------
            123 Boston

    (1 rows affected)
    ```

1. この `DELETE` T-SQL ステートメントを実行して、レコードを削除します。 これは**削除**操作です。

    ```sql
    DELETE FROM Drivers WHERE DriverID=123;
    GO
    ```

    ```output
    (1 rows affected)
    ```

1. この `SELECT` T-SQL ステートメントを実行して、`Drivers` テーブルが空であることを確認します。

    ```sql
    SELECT COUNT(*) FROM Drivers;
    GO
    ```

    テーブルに行がないことがわかります。

    ```output
    -----------
              0

    (1 rows affected)
    ```

Cloud Shell から Azure SQL Database を操作する方法を学んだので、SQL Server Management Studio、Visual Studio などから、好みの SQL 管理ツール&ndash;の接続文字列を取得できます。

Cloud Shell を使用すると、Azure のリソースに簡単にアクセスして操作できます。 Cloud Shell はブラウザーベースなので、Windows、macOS、または Linux から、つまり、基本的には Web ブラウザーを使用する任意のシステムからアクセスできます。

ここでは、Azure CLI コマンドを実行して SQL Azure SQL Database に関する情報を取得する実践的な経験を得ました。 さらに、T-SQL のスキルについても実践しました。

最後に、データベースの破棄方法を要約して説明します。
