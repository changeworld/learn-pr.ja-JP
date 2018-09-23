<span data-ttu-id="50b04-101">Visual Studio Code では、統合ターミナルといくつかの簡単なコマンドを使用してコンソール アプリケーションを作成できます。</span><span class="sxs-lookup"><span data-stu-id="50b04-101">Visual Studio Code enables you to create a console application by using the integrated terminal and a few short commands.</span></span>

<span data-ttu-id="50b04-102">このユニットでは、統合ターミナルを使用して単純なコンソール アプリを作成し、拡張機能から Azure Cosmos DB 接続文字列を取得して、アプリケーションから Azure Cosmos DB への接続を構成します。</span><span class="sxs-lookup"><span data-stu-id="50b04-102">In this unit, you will create a simple console app using the integrated terminal, retrieve your Azure Cosmos DB connection string from the extension, and then configure the connection from your application to Azure Cosmos DB.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="50b04-103">コンソール アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="50b04-103">Create a console app</span></span>

1. <span data-ttu-id="50b04-104">Visual Studio Code で、**[ファイル]** > **[フォルダーを開く]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="50b04-104">In Visual Studio Code, select **File** > **Open Folder**.</span></span>

1. <span data-ttu-id="50b04-105">任意の場所に `learning-module` という名前の新しいフォルダーを作成し、**[フォルダーの選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50b04-105">Create a new folder named `learning-module` in the location of your choice, and then click **Select Folder**.</span></span>

1. <span data-ttu-id="50b04-106">[ファイル] メニューをクリックし、**[自動保存]** がオンになっていない場合はオンにして、ファイルの自動保存を確実に有効にします。</span><span class="sxs-lookup"><span data-stu-id="50b04-106">Ensure that file auto-save is enabled by clicking on the File menu and checking **Auto Save** if it is blank.</span></span> <span data-ttu-id="50b04-107">いくつかのコード ブロックをコピーします。これで、常にファイルの最新の編集内容に対して操作することができます。</span><span class="sxs-lookup"><span data-stu-id="50b04-107">You will be copying in several blocks of code, and this will ensure you are always operating against the latest edits of your files.</span></span>

1. <span data-ttu-id="50b04-108">Visual Studio Code で、メイン メニューの **[表示]** > **[ターミナル]** を選択して統合ターミナルを開きます。</span><span class="sxs-lookup"><span data-stu-id="50b04-108">Open the integrated terminal from Visual Studio Code by selecting **View** > **Terminal** from the main menu.</span></span>

1. <span data-ttu-id="50b04-109">ターミナル ウィンドウに、次のコマンドをコピーして貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="50b04-109">In the terminal window, copy and paste the following command.</span></span>

    ```bash
    dotnet new console
    ```

    <span data-ttu-id="50b04-110">このコマンドにより、**learning-module.csproj** という名前の C# プロジェクト ファイルと共に、単純な "Hello World" プログラムが既に記述されている **Program.cs** ファイルがフォルダーに作成されます。</span><span class="sxs-lookup"><span data-stu-id="50b04-110">This command creates a **Program.cs** file in your folder with a simple "Hello World" program already written, along with a C# project file named **learning-module.csproj**.</span></span>

1. <span data-ttu-id="50b04-111">ターミナル ウィンドウに、次のコマンドをコピーして貼り付け、"Hello World" プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="50b04-111">In the terminal window, copy and paste the following command to run the "Hello World" program.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="50b04-112">ターミナル ウィンドウに、出力として "Hello world!" と</span><span class="sxs-lookup"><span data-stu-id="50b04-112">The terminal window displays "Hello world!"</span></span> <span data-ttu-id="50b04-113">表示されます。</span><span class="sxs-lookup"><span data-stu-id="50b04-113">as output.</span></span>

## <a name="connect-the-app-to-azure-cosmos-db"></a><span data-ttu-id="50b04-114">Azure Cosmos DB にアプリを接続する</span><span class="sxs-lookup"><span data-stu-id="50b04-114">Connect the app to Azure Cosmos DB</span></span>

1. <span data-ttu-id="50b04-115">ターミナルのプロンプトで、次のコマンド ブロックをコピーして貼り付け、必要な NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="50b04-115">At the terminal prompt, copy and paste the following command block to install the required NuGet packages.</span></span>

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

1. <span data-ttu-id="50b04-116">エクスプローラー ウィンドウの上部にある **Program.cs** をクリックしてファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="50b04-116">At the top of the Explorer pane, click **Program.cs** to open the file.</span></span>

1. <span data-ttu-id="50b04-117">`using System;` の後に、次の using ステートメントを追加します。</span><span class="sxs-lookup"><span data-stu-id="50b04-117">Add the following using statements after `using System;`.</span></span>

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

    <span data-ttu-id="50b04-118">見つからない必要なアセットの追加に関するメッセージが表示されたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="50b04-118">If you get a message about adding required missing assets, click **Yes**.</span></span>

1. <span data-ttu-id="50b04-119">learning-module フォルダーに App.config という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="50b04-119">Create a new file named App.config in the learning-module folder, and add the following code.</span></span>

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. <span data-ttu-id="50b04-120">左側の ![Azure アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) Azure アイコンをクリックし、コンシェルジェ サブスクリプションを展開します。新しい Azure Cosmos DB アカウントを右クリックし、**[Copy Connection String]\(接続文字列のコピー\)** をクリックして接続文字列をコピーします。</span><span class="sxs-lookup"><span data-stu-id="50b04-120">Copy your connection string by clicking the ![Azure icon](../media/2-setup/visual-studio-code-explorer-icon.png) Azure icon on the left, expanding your Concierge Subscription, right-clicking your new Azure Cosmos DB account, and then clicking **Copy Connection String**.</span></span>

1. <span data-ttu-id="50b04-121">App.config ファイルの末尾に接続文字列を貼り付け、接続文字列の **AccountEndpoint** 部分を App.config 内の **accountEndpoint** 値にコピーします。</span><span class="sxs-lookup"><span data-stu-id="50b04-121">Paste the connection string into the end of the App.config file, and then copy the **AccountEndpoint** portion from the connection string into the **accountEndpoint** value in App.config.</span></span>

    <span data-ttu-id="50b04-122">accountEndpoint は次のコードのようになります。</span><span class="sxs-lookup"><span data-stu-id="50b04-122">The accountEndpoint should look like the following code:</span></span>

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. <span data-ttu-id="50b04-123">接続文字列の **AccountKey** 値を **accountKey** 値にコピーし、コピーした元の接続文字列を削除します。</span><span class="sxs-lookup"><span data-stu-id="50b04-123">Now copy the **AccountKey** value from the connection string into the **accountKey** value, and then delete the original connection string you copied in.</span></span>

1. <span data-ttu-id="50b04-124">ターミナルのプロンプトで、次のコマンドをコピーして貼り付け、プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="50b04-124">At the terminal prompt, copy and paste the following command to run the program.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="50b04-125">プログラムによって、ターミナルに "Hello World!" と</span><span class="sxs-lookup"><span data-stu-id="50b04-125">The program displays Hello World!</span></span> <span data-ttu-id="50b04-126">表示されます。</span><span class="sxs-lookup"><span data-stu-id="50b04-126">in the terminal.</span></span>

## <a name="create-the-documentclient"></a><span data-ttu-id="50b04-127">DocumentClient を作成する</span><span class="sxs-lookup"><span data-stu-id="50b04-127">Create the DocumentClient</span></span>

<span data-ttu-id="50b04-128">次に、DocumentClient のインスタンスを作成します。これは、Azure Cosmos DB サービスのクライアント側の表現です。</span><span class="sxs-lookup"><span data-stu-id="50b04-128">Now it's time to create an instance of the DocumentClient, which is the client-side representation of the Azure Cosmos DB service.</span></span> <span data-ttu-id="50b04-129">このクライアントは、サービスに対する要求の構成と実行に使用されます。</span><span class="sxs-lookup"><span data-stu-id="50b04-129">This client is used to configure and execute requests against the service.</span></span>

1. <span data-ttu-id="50b04-130">Program.cs で、Program クラスの先頭に以下を追加します。</span><span class="sxs-lookup"><span data-stu-id="50b04-130">In Program.cs, add the following to the beginning of the Program class.</span></span>

    ```csharp
    private DocumentClient client;
    ```

1. <span data-ttu-id="50b04-131">新しいクライアントを作成する新しい非同期タスクを追加し、`Main` メソッドの後に次のメソッドを追加して、Users データベースが存在するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="50b04-131">Add a new asynchronous task to create a new client, and check whether the Users database exists by adding the following method after the `Main` method.</span></span>

    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["accountEndpoint"]), ConfigurationManager.AppSettings["accountKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });

        Console.WriteLine("Database and collection validation complete");
    }
    ```

1. <span data-ttu-id="50b04-132">統合ターミナルで、もう一度、次のコマンドをコピーして貼り付け、プログラムを実行して、プログラムが実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="50b04-132">In the integrated terminal, again, copy and paste the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="50b04-133">**Main** メソッドに次のコードをコピーして貼り付け、現在の `Console.WriteLine("Hello World!");` 行を上書きします。</span><span class="sxs-lookup"><span data-stu-id="50b04-133">Copy and paste the following code into the **Main** method, overwriting the current `Console.WriteLine("Hello World!");` line.</span></span>

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

1. <span data-ttu-id="50b04-134">統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="50b04-134">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="50b04-135">コンソールには次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="50b04-135">The console displays the following output.</span></span>

    ```output
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

<span data-ttu-id="50b04-136">このユニットでは、Azure Cosmos DB アプリケーション用の基盤を設定しました。</span><span class="sxs-lookup"><span data-stu-id="50b04-136">In this unit, you set up the groundwork for your Azure Cosmos DB application.</span></span> <span data-ttu-id="50b04-137">Visual Studio Code で開発環境を設定し、シンプルな HelloWorld プロジェクトを作成し、そのプロジェクトを Azure Cosmos DB エンドポイントに接続して、データベースとコレクションが存在することを確認しました。</span><span class="sxs-lookup"><span data-stu-id="50b04-137">You set up your development environment in Visual Studio Code, created a simple HelloWorld project, connected the project to the Azure Cosmos DB endpoint, and ensured your database and collection exist.</span></span>
