<span data-ttu-id="16f46-101">オンライン小売業者のストレージを管理しているとします。</span><span class="sxs-lookup"><span data-stu-id="16f46-101">Imagine you're managing storage for an online retailer.</span></span> <span data-ttu-id="16f46-102">ユーザーおよび製品データを作成、更新、削除するためのツールが必要です。</span><span class="sxs-lookup"><span data-stu-id="16f46-102">You need tools to create, update, and delete your user and product data.</span></span>

<span data-ttu-id="16f46-103">このモジュールでは、Visual Studio Code で .NET Core コンソール アプリケーションをビルドして、C# を使用してユーザー レコードの作成、更新、削除、データのクエリを行い、ストアド プロシージャを実行します。</span><span class="sxs-lookup"><span data-stu-id="16f46-103">In this module, you will build a .NET Core console application in Visual Studio Code to create, update, and delete user records, query your data, and perform stored procedures using C#.</span></span>

<span data-ttu-id="16f46-104">データは Azure Cosmos DB に格納され、Azure Cosmos DB には便利な Visual Studio Code 拡張機能があるので、前のモジュールで作成したデータベース、コレクション、およびドキュメントを簡単に確認できます。また、この拡張機能を使用して新しいリソースを作成し、Azure portal を開くことなく接続文字列をコピーすることもできます。</span><span class="sxs-lookup"><span data-stu-id="16f46-104">Your data will be stored in Azure Cosmos DB, which has a convenient Visual Studio Code extension so you can easily see the database, collection, and documents you created in the previous modules, plus you can create new resources using the extension, and copy your connection string without having to open the Azure portal.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="16f46-105">学習の目的</span><span class="sxs-lookup"><span data-stu-id="16f46-105">Learning objectives</span></span>

<span data-ttu-id="16f46-106">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="16f46-106">In this module, you will:</span></span>  

- <span data-ttu-id="16f46-107">Azure Cosmos DB でデータの格納とクエリを行うアプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="16f46-107">Create an application to store and query data in Azure Cosmos DB</span></span>
- <span data-ttu-id="16f46-108">Visual Studio Code で統合ターミナルを使用してコンソール アプリケーションを簡単に作成する</span><span class="sxs-lookup"><span data-stu-id="16f46-108">Use the Integrated Terminal in Visual Studio Code to quickly create a console application</span></span>
- <span data-ttu-id="16f46-109">Visual Studio Code 用 Azure Cosmos DB 拡張機能を利用して Azure Cosmos DB の機能を追加する</span><span class="sxs-lookup"><span data-stu-id="16f46-109">Add Azure Cosmos DB functionality with the help of the Azure Cosmos DB extension for Visual Studio Code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16f46-110">前提条件</span><span class="sxs-lookup"><span data-stu-id="16f46-110">Prerequisites</span></span>

- <span data-ttu-id="16f46-111">[Visual Studio Code](https://code.visualstudio.com/) をインストールしている必要があります</span><span class="sxs-lookup"><span data-stu-id="16f46-111">Must have [Visual Studio Code](https://code.visualstudio.com/) installed</span></span>
- <span data-ttu-id="16f46-112">[.NET Core 2.1](https://www.microsoft.com/net/download) がインストールされている必要があります</span><span class="sxs-lookup"><span data-stu-id="16f46-112">Must have [.NET Core 2.1](https://www.microsoft.com/net/download) installed</span></span>
- <span data-ttu-id="16f46-113">[Azure アカウント](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)拡張機能が Visual Studio Code にインストールされている必要があります</span><span class="sxs-lookup"><span data-stu-id="16f46-113">Must have the [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extension installed in Visual Studio Code</span></span>
