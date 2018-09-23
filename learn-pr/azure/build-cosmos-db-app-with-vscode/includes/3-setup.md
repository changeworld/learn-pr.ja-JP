Visual Studio Code では、統合ターミナルといくつかの簡単なコマンドを使用してコンソール アプリケーションを作成できます。

このユニットでは、統合ターミナルを使用して単純なコンソール アプリを作成し、拡張機能から Azure Cosmos DB 接続文字列を取得して、アプリケーションから Azure Cosmos DB への接続を構成します。

## <a name="create-a-console-app"></a>コンソール アプリを作成する

1. Visual Studio Code で、**[ファイル]** > **[フォルダーを開く]** の順に選択します。

1. 任意の場所に `learning-module` という名前の新しいフォルダーを作成し、**[フォルダーの選択]** をクリックします。

1. [ファイル] メニューをクリックし、**[自動保存]** がオンになっていない場合はオンにして、ファイルの自動保存を確実に有効にします。 いくつかのコード ブロックをコピーします。これで、常にファイルの最新の編集内容に対して操作することができます。

1. Visual Studio Code で、メイン メニューの **[表示]** > **[ターミナル]** を選択して統合ターミナルを開きます。

1. ターミナル ウィンドウに、次のコマンドをコピーして貼り付けます。

    ```bash
    dotnet new console
    ```

    このコマンドにより、**learning-module.csproj** という名前の C# プロジェクト ファイルと共に、単純な "Hello World" プログラムが既に記述されている **Program.cs** ファイルがフォルダーに作成されます。

1. ターミナル ウィンドウに、次のコマンドをコピーして貼り付け、"Hello World" プログラムを実行します。

    ```bash
    dotnet run
    ```

    ターミナル ウィンドウに、出力として "Hello world!" と 表示されます。

## <a name="connect-the-app-to-azure-cosmos-db"></a>Azure Cosmos DB にアプリを接続する

1. ターミナルのプロンプトで、次のコマンド ブロックをコピーして貼り付け、必要な NuGet パッケージをインストールします。

    ```bash
    dotnet add package System.Net.Http
    dotnet add package System.Configuration
    dotnet add package System.Configuration.ConfigurationManager
    dotnet add package Microsoft.Azure.DocumentDB.Core
    dotnet add package Newtonsoft.Json
    dotnet add package System.Threading.Tasks
    dotnet add package System.Linq
    dotnet restore
    ```

1. エクスプローラー ウィンドウの上部にある **Program.cs** をクリックしてファイルを開きます。

1. `using System;` の後に、次の using ステートメントを追加します。

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

    見つからない必要なアセットの追加に関するメッセージが表示されたら、**[はい]** をクリックします。

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

1. 左側の ![Azure アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) Azure アイコンをクリックし、コンシェルジェ サブスクリプションを展開します。新しい Azure Cosmos DB アカウントを右クリックし、**[Copy Connection String]\(接続文字列のコピー\)** をクリックして接続文字列をコピーします。

1. App.config ファイルの末尾に接続文字列を貼り付け、接続文字列の **AccountEndpoint** 部分を App.config 内の **accountEndpoint** 値にコピーします。

    accountEndpoint は次のコードのようになります。

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. 接続文字列の **AccountKey** 値を **accountKey** 値にコピーし、コピーした元の接続文字列を削除します。

1. ターミナルのプロンプトで、次のコマンドをコピーして貼り付け、プログラムを実行します。

    ```csharp
    dotnet run
    ```

    プログラムによって、ターミナルに "Hello World!" と 表示されます。

## <a name="create-the-documentclient"></a>DocumentClient を作成する

次に、DocumentClient のインスタンスを作成します。これは、Azure Cosmos DB サービスのクライアント側の表現です。 このクライアントは、サービスに対する要求の構成と実行に使用されます。

1. Program.cs で、Program クラスの先頭に以下を追加します。

    ```csharp
    private DocumentClient client;
    ```

1. 新しいクライアントを作成する新しい非同期タスクを追加し、`Main` メソッドの後に次のメソッドを追加して、Users データベースが存在するかどうかを確認します。

    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["accountEndpoint"]), ConfigurationManager.AppSettings["accountKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });

        Console.WriteLine("Database and collection validation complete");
    }
    ```

1. 統合ターミナルで、もう一度、次のコマンドをコピーして貼り付け、プログラムを実行して、プログラムが実行されていることを確認します。

    ```csharp
    dotnet run
    ```

1. **Main** メソッドに次のコードをコピーして貼り付け、現在の `Console.WriteLine("Hello World!");` 行を上書きします。

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

    ```output
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

このユニットでは、Azure Cosmos DB アプリケーション用の基盤を設定しました。 Visual Studio Code で開発環境を設定し、シンプルな HelloWorld プロジェクトを作成し、そのプロジェクトを Azure Cosmos DB エンドポイントに接続して、データベースとコレクションが存在することを確認しました。
