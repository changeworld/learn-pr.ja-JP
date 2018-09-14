このモジュールでは、統合ターミナルを使用してシンプルなコンソール アプリを作成し、NuGet パッケージをインストールします。また、Azure Cosmos DB 拡張機能を使用して、前のモジュールで作成したデータベースとコレクションを確認します。 拡張機能から Azure Cosmos DB 接続文字列を取得し、Azure Cosmos DB への接続を開始してユーザー データベースを作成します。

## <a name="create-a-console-app"></a>コンソール アプリを作成する

1. Visual Studio Code を開き、**[ファイル]** > **[フォルダーを開く]** の順に選択します。

1. 新しい C# プロジェクトとなる新しいフォルダーを作成し、**[フォルダーの選択]** をクリックします。

1. そのファイルの自動確認ファイル メニューをクリックし、チェック保存が有効になっている**自動保存**が空の場合。

1. Visual Studio Code から統合ターミナルを選択して開きます**ビュー** > **ターミナル**メイン メニューから。

1. ターミナル ウィンドウで、次のコマンドを入力します。

    ```
    dotnet new console
    ```

    このコマンドにより、フォルダーに **learning-module.csproj** という名前の C# プロジェクト ファイルとともに、シンプルな "Hello World" プログラムが既に記述されている **Program.cs** ファイルが作成されます。

1. ターミナル ウィンドウで、次のコマンドを入力して "Hello World" プログラムを実行します。 

    ```
    dotnet run
    ```

    ターミナル ウィンドウには、出力として "Hello world!" が 表示されます。

## <a name="connect-the-app-to-azure-cosmos-db"></a>Azure Cosmos DB にアプリを接続する

1. **[表示]** > **[コマンド パレット]** の順にクリックし、「**Azure: Sign In**」と入力して、Azure にサインインします。

    プロンプトに従って、Web ブラウザーで提供されるコードをコピーして貼り付けます。これにより、Visual Studio Code セッションが認証されます。

1. をクリックして、 ![Azure アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) **Azure**左側のメニューのアイコンを順に展開**Azure Cosmos DB**します。

1. Azure サブスクリプション、Azure Cosmos DB アカウントの順に展開します。 前のモジュールで **Products** データベースと **Clothing** コレクションを作成した場合は、拡張機能でそれらが表示されます。

   ![Azure Cosmos DB の Visual Studio Code 拡張機能](../media/2-setup/azure-cosmos-db-vs-code-extension.png) 

1. 次は、顧客用の新しいデータベースとコレクションを作成します。

    Azure ウィンドウで、自分のアカウントを右クリックし、 **Create Database**します。
    
    画面の上部にあるテキスト ボックスで、データベース名として「**Users**」を入力 > **Enter**  > コレクション名として「**WebCustomers**」 を入力 > **Enter** >  パーティション キーとして「**userId**」を入力 > **Enter** > 初期スループット容量として「**1000**」を入力 > **Enter** の順に進みます。

    ![Azure Cosmos DB の Visual Studio Code 拡張機能](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) 

    新しい Users データベースと WebCustomers コレクションがエクスプローラー ウィンドウに表示されます。

1. 統合ターミナルでは、新しいプロンプトで以下のコマンドをそれぞれ実行して、必要な NuGet パッケージをインストールします。

    ```
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet add package Bogus
    dotnet restore
    ```

1. エクスプローラー ウィンドウの上部にある **Program.cs** をクリックしてファイルを開きます。

1. 次の using ステートメントを `using System;` の後に追加します。

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

1. learning-module フォルダーに App.config という名前の新しいファイルを作成し、次のコードを追加します。
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. learning-module アカウントを右クリックし、**[Copy Connection String]\(接続文字列のコピー\)** をクリックして、Azure Cosmos DB 拡張機能から接続文字列をコピーします。

1. 接続文字列をテキスト ファイルに貼り付け、そのテキスト ファイルから **AccountEndpoint** 部分を App.config 内の **accountEndpoint** にコピーします。

    accountEndpoint は次のコードのようになります。

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. ここで、テキスト値から **AccountKey** 値を App.config 内の **accountKey** 値にコピーします。

1. 統合ターミナルで、次のコマンドを入力してプログラムを実行し、プログラムが実行されることを確認します。

    ```csharp
    dotnet run
    ```

1. 新しい非同期タスクを追加して新しいクライアントを作成し、main メソッドの後に次のメソッドを追加して、Users データベースが存在するかどうかを確認します。
    
    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpointUrl"]), ConfigurationManager.AppSettings["primaryKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });
    }
    ```

1. 統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。

    ```csharp
    dotnet run
    ```

1. 次のコードを **Main** メソッドにコピーして貼り付け、現在の `Console.WriteLine("Hello World!");` 行を上書きします。

    ```csharp
    try
            {
                    Program p = new Program();
                    p.BasicOperations().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }
    ```

1. 統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。

    ```csharp
    dotnet run
    ```

    コンソールには次の出力が表示されます。
    
    ```
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

## <a name="summary"></a>まとめ

このユニットでは、Azure Cosmos DB アプリケーション用の基盤を設定しました。 Visual Studio Code で開発環境を設定し、シンプルな HelloWorld プロジェクトを作成し、そのプロジェクトを Azure Cosmos DB エンドポイントに接続して、データベースとコレクションが存在することを確認しました。