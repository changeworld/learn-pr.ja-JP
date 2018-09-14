<span data-ttu-id="6b8eb-101">このモジュールでは、統合ターミナルを使用してシンプルなコンソール アプリを作成し、NuGet パッケージをインストールします。また、Azure Cosmos DB 拡張機能を使用して、前のモジュールで作成したデータベースとコレクションを確認します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-101">In this module you will create a simple console app using the integrated terminal, install NuGet packages, and use the Azure Cosmos DB extension to see databases and collections created in the previous module.</span></span> <span data-ttu-id="6b8eb-102">拡張機能から Azure Cosmos DB 接続文字列を取得し、Azure Cosmos DB への接続を開始してユーザー データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-102">You'll retrieve your Azure Cosmos DB connection string from the extension, and then start configure the connection to Azure Cosmos DB to create your User database.</span></span>

## <a name="create-a-console-app"></a><span data-ttu-id="6b8eb-103">コンソール アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="6b8eb-103">Create a console app</span></span>

1. <span data-ttu-id="6b8eb-104">作業するフォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-104">Create a folder where you will be working.</span></span>

1. <span data-ttu-id="6b8eb-105">コマンド プロンプトを起動し、そのフォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-105">Open a command prompt and navigate into the folder.</span></span>

1. <span data-ttu-id="6b8eb-106">新しい .NET Core コンソール アプリケーションを作成します</span><span class="sxs-lookup"><span data-stu-id="6b8eb-106">Create a new .NET Core console application</span></span>

```bash
dotnet new console 
```

1. <span data-ttu-id="6b8eb-107">Visual Studio Code を開き、**[ファイル]** > **[フォルダーを開く]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-107">Open Visual Studio Code, and then select **File** > **Open Folder**.</span></span>

1. <span data-ttu-id="6b8eb-108">新しい C# プロジェクトとなる新しいフォルダーを作成し、**[フォルダーの選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-108">Create a new folder where you want your new C# project to be, and then click **Select Folder**.</span></span>

1. <span data-ttu-id="6b8eb-109">[ファイル] メニューをクリックし、自動保存がオンになってない場合はオンにして、ファイルの自動保存を確実に有効にします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-109">Ensure that file auto save is enabled by clicking on the File menu and checking Auto Save if it is blank.</span></span>

1. <span data-ttu-id="6b8eb-110">Visual Studio Code で、メイン メニューから **[表示]** > **[統合ターミナル]** の順に選択して統合ターミナルを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-110">Open the integrated terminal from Visual Studio Code by selecting **View** > **Integrated Terminal** from the main menu.</span></span>

1. <span data-ttu-id="6b8eb-111">ターミナル ウィンドウで、「**dotnet new console**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-111">In the terminal window, type **dotnet new console**.</span></span>

    <span data-ttu-id="6b8eb-112">このコマンドにより、フォルダーに **learning-module.csproj** という名前の C# プロジェクト ファイルとともに、シンプルな "Hello World" プログラムが既に記述されている **Program.cs** ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-112">This command creates a **Program.cs** file in your folder with a simple "Hello World" program already written, along with a C# project file named **learning-module.csproj**.</span></span>

1. <span data-ttu-id="6b8eb-113">ターミナル ウィンドウで、次のコマンドを入力して "Hello World" プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-113">In the terminal window, type the following command to run the "Hello World" program.</span></span> 

    ```
    dotnet run
    ```

    <span data-ttu-id="6b8eb-114">ターミナル ウィンドウには、出力として "Hello world!" が</span><span class="sxs-lookup"><span data-stu-id="6b8eb-114">The terminal window displays "Hello world!"</span></span> <span data-ttu-id="6b8eb-115">表示されます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-115">as output.</span></span>

## <a name="connect-the-app-to-azure-cosmos-db"></a><span data-ttu-id="6b8eb-116">Azure Cosmos DB にアプリを接続する</span><span class="sxs-lookup"><span data-stu-id="6b8eb-116">Connect the app to Azure Cosmos DB</span></span>

1. <span data-ttu-id="6b8eb-117">**[表示]** > **[コマンド パレット]** の順にクリックし、「**Azure: Sign In**」と入力して、Azure にサインインします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-117">Sign in to Azure by clicking **View** > **Command Palette** and typing **Azure: Sign In**.</span></span>

    <span data-ttu-id="6b8eb-118">プロンプトに従って、Web ブラウザーで提供されるコードをコピーして貼り付けます。これにより、Visual Studio Code セッションが認証されます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-118">Follow the prompts to copy and paste the code provided in the web browser, which authenticates your Visual Studio Code session.</span></span>

1. <span data-ttu-id="6b8eb-119">左側のメニューの ![[エクスプローラー] アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) **[エクスプローラー]** アイコンをクリックし、**[Azure Cosmos DB]** を展開します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-119">Click the ![Explorer icon](../media/2-setup/visual-studio-code-explorer-icon.png) **Explorer** icon on the left menu, and then expand **Azure Cosmos DB**.</span></span>

1. <span data-ttu-id="6b8eb-120">Azure サブスクリプション、Azure Cosmos DB アカウントの順に展開します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-120">Expand your Azure subscription > Azure Cosmos DB account.</span></span> <span data-ttu-id="6b8eb-121">前のモジュールで **Products** データベースと **Clothing** コレクションを作成した場合は、拡張機能でそれらが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-121">If you created the **Products** database and **Clothing** collection in the previous modules, the extension displays them.</span></span>

   ![Azure Cosmos DB の Visual Studio Code 拡張機能](../media/2-setup/azure-cosmos-db-vs-code-extension.png) 

1. <span data-ttu-id="6b8eb-123">次は、顧客用の新しいデータベースとコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-123">Now let's create a new database and collection for your customers.</span></span>

    <span data-ttu-id="6b8eb-124">エクスプローラー ウィンドウで、アカウントを右クリックし、**[データベースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-124">In the Explorer window, right-click your account, and then click **Create Database**.</span></span> 
    
    <span data-ttu-id="6b8eb-125">画面の上部にあるテキスト ボックスで、データベース名として「**Users**」を入力 > **Enter**  > コレクション名として「**WebCustomers**」 を入力 > **Enter** >  パーティション キーとして「**userId**」を入力 > **Enter** > 初期スループット容量として「**1000**」を入力 > **Enter** の順に進みます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-125">In the text box at the top of the screen, type **Users** for the database name > **Enter** > **WebCustomers** for the collection name > **Enter** > **userId** for the partition key > **Enter** > **1000** for the initial throughput capacity > **Enter**.</span></span>

    <span data-ttu-id="6b8eb-126">![Azure Cosmos DB の Visual Studio Code 拡張機能](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span><span class="sxs-lookup"><span data-stu-id="6b8eb-126">![Azure Cosmos DB Visual Studio Code extension](../media/2-setup/vs-code-azure-cosmos-db-extension.gif) <!--Retake on fresh machine without the other subscriptions showing--></span></span>

    <span data-ttu-id="6b8eb-127">新しい Users データベースと WebCustomers コレクションがエクスプローラー ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-127">The new Users database and WebCustomers collection are displayed in the Explorer window.</span></span>

1. <span data-ttu-id="6b8eb-128">統合ターミナルでは、新しいプロンプトで以下のコマンドをそれぞれ実行して、必要な NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-128">In the integrated terminal, run each of the following commands at a new prompt to install the required NuGet packages.</span></span>

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

1. <span data-ttu-id="6b8eb-129">エクスプローラー ウィンドウの上部にある **Program.cs** をクリックしてファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-129">At the top of the Explorer pane, click **Program.cs** to open the file.</span></span>

1. <span data-ttu-id="6b8eb-130">次の using ステートメントを `using System;` の後に追加します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-130">Add the following using statements after `using System;`.</span></span>

    ```csharp
    using System.Configuration;
    using System.Linq;
    using System.Threading.Tasks;
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;
    ```

1. <span data-ttu-id="6b8eb-131">learning-module フォルダーに App.config という名前の新しいファイルを作成し、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-131">Create a new file named App.config in the learning-module folder, and add the following code.</span></span>
  
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <appSettings>
            <add key="accountEndpoint" value="<replace with your Endpoint URL>" />
            <add key="accountKey" value="<replace with your Primary Key>" />
          </appSettings>
    </configuration>
    ```

1. <span data-ttu-id="6b8eb-132">learning-module アカウントを右クリックし、**[Copy Connection String]\(接続文字列のコピー\)** をクリックして、Azure Cosmos DB 拡張機能から接続文字列をコピーします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-132">Copy your connection string from the Azure Cosmos DB extension by right-clicking the learning-module account, and clicking **Copy Connection String**.</span></span>

    ![Azure Cosmos DB の Visual Studio Code 拡張機能](../media/2-setup/vs-code-copy-connection-string.gif) 

1. <span data-ttu-id="6b8eb-134">接続文字列をテキスト ファイルに貼り付け、そのテキスト ファイルから **AccountEndpoint** 部分を App.config 内の **accountEndpoint** にコピーします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-134">Paste the connection string into a text file, and then copy the **AccountEndpoint** portion from the text file into the **accountEndpoint** in App.config.</span></span>

    <span data-ttu-id="6b8eb-135">accountEndpoint は次のコードのようになります。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-135">The accountEndpoint should look like the following code:</span></span>

    ```xml
    <add key="accountEndpoint" value="https://<account-name>.documents.azure.com:443/" />
    ```

1. <span data-ttu-id="6b8eb-136">ここで、テキスト値から **AccountKey** 値を App.config 内の **accountKey** 値にコピーします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-136">Now copy the **AccountKey** value from the text value into the **accountKey** value in App.config.</span></span>

1. <span data-ttu-id="6b8eb-137">統合ターミナルで、次のコマンドを入力してプログラムを実行し、プログラムが実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-137">In the integrated terminal, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="6b8eb-138">新しい非同期タスクを追加して新しいクライアントを作成し、main メソッドの後に次のメソッドを追加して、Users データベースが存在するかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-138">Add a new asynchronous task to create a new client, and check whether the Users database exists by adding the following method after the main method.</span></span>
    
    ```csharp
    private async Task BasicOperations()
    {
        this.client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpointUrl"]), ConfigurationManager.AppSettings["primaryKey"]);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "Users" });

        await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("Users"), new DocumentCollection { Id = "WebCustomers" });
    }
    ```

1. <span data-ttu-id="6b8eb-139">統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-139">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

1. <span data-ttu-id="6b8eb-140">次のコードを **Main** メソッドにコピーして貼り付け、現在の `Console.WriteLine("Hello World!");` 行を上書きします。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-140">Copy and paste the following code into the **Main** method, overwriting the current `Console.WriteLine("Hello World!");` line.</span></span>

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

1. <span data-ttu-id="6b8eb-141">統合ターミナルで、もう一度、次のコマンドを入力してプログラムを実行し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-141">In the integrated terminal, again, type the following command to run the program to ensure it runs.</span></span>

    ```csharp
    dotnet run
    ```

    <span data-ttu-id="6b8eb-142">コンソールには次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-142">The console displays the following output.</span></span>
    
    ```
    Database and collection validation complete
    End of demo, press any key to exit.
    ```

## <a name="summary"></a><span data-ttu-id="6b8eb-143">まとめ</span><span class="sxs-lookup"><span data-stu-id="6b8eb-143">Summary</span></span>

<span data-ttu-id="6b8eb-144">このユニットでは、Azure Cosmos DB アプリケーション用の基盤を設定しました。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-144">In this unit, you set up the groundwork for your Azure Cosmos DB application.</span></span> <span data-ttu-id="6b8eb-145">Visual Studio Code で開発環境を設定し、シンプルな HelloWorld プロジェクトを作成し、そのプロジェクトを Azure Cosmos DB エンドポイントに接続して、データベースとコレクションが存在することを確認しました。</span><span class="sxs-lookup"><span data-stu-id="6b8eb-145">You set up your development environment in Visual Studio Code, created a simple HelloWorld project, connected the project to the Azure Cosmos DB endpoint, and ensured your database and collection exist.</span></span>