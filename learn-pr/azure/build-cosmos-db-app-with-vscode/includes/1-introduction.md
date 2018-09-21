<span data-ttu-id="b3a1f-101">オンライン小売業者のストレージを管理しているとします。</span><span class="sxs-lookup"><span data-stu-id="b3a1f-101">Imagine you're managing storage for an online retailer.</span></span> <span data-ttu-id="b3a1f-102">ユーザーおよび製品データを作成、更新、削除するためのツールが必要です。</span><span class="sxs-lookup"><span data-stu-id="b3a1f-102">You need tools to create, update, and delete your user and product data.</span></span>

<span data-ttu-id="b3a1f-103">このモジュールでは、Visual Studio Code で .NET Core コンソール アプリケーションをビルドして、C# を使用してユーザー レコードの作成、更新、削除、データのクエリを行い、ストアド プロシージャを実行します。</span><span class="sxs-lookup"><span data-stu-id="b3a1f-103">In this module, you will build a .NET Core console application in Visual Studio Code to create, update, and delete user records, query your data, and perform stored procedures using C#.</span></span>

<span data-ttu-id="b3a1f-104">データは Azure Cosmos DB に格納されます。Azure Cosmos DB には便利な Visual Studio Code の拡張機能があり、Azure Cosmos DB アカウント、データベース、コレクションを簡単に作成したり、Azure portal を開かず、接続文字列をアプリにコピーしたりできます。</span><span class="sxs-lookup"><span data-stu-id="b3a1f-104">Your data will be stored in Azure Cosmos DB, which has a convenient Visual Studio Code extension so you can easily create an Azure Cosmos DB account, database, and collection, and copy your connection string into the app without having to open the Azure portal.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="b3a1f-105">学習の目的</span><span class="sxs-lookup"><span data-stu-id="b3a1f-105">Learning objectives</span></span>

<span data-ttu-id="b3a1f-106">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="b3a1f-106">In this module, you will:</span></span>  

- <span data-ttu-id="b3a1f-107">Azure Cosmos DB 拡張機能を使用し、Visual Studio Code で Azure Cosmos DB アカウント、データベース、コレクションを作成します</span><span class="sxs-lookup"><span data-stu-id="b3a1f-107">Create an Azure Cosmos DB account, database, and collection in Visual Studio Code using the Azure Cosmos DB extension</span></span>
- <span data-ttu-id="b3a1f-108">Azure Cosmos DB にデータを格納したり、そこにあるデータを問い合わせたりするためのアプリケーションを作成します</span><span class="sxs-lookup"><span data-stu-id="b3a1f-108">Create an application to store and query data in Azure Cosmos DB</span></span>
- <span data-ttu-id="b3a1f-109">Visual Studio Code のターミナルを使用し、コンソール アプリケーションを簡単に作成します</span><span class="sxs-lookup"><span data-stu-id="b3a1f-109">Use the Terminal in Visual Studio Code to quickly create a console application</span></span>
- <span data-ttu-id="b3a1f-110">Visual Studio Code 用 Azure Cosmos DB 拡張機能を利用して Azure Cosmos DB 機能を追加します</span><span class="sxs-lookup"><span data-stu-id="b3a1f-110">Add Azure Cosmos DB functionality with the help of the Azure Cosmos DB extension for Visual Studio Code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3a1f-111">前提条件</span><span class="sxs-lookup"><span data-stu-id="b3a1f-111">Prerequisites</span></span>

- <span data-ttu-id="b3a1f-112">[Visual Studio Code](https://code.visualstudio.com/) をインストールしている必要があります</span><span class="sxs-lookup"><span data-stu-id="b3a1f-112">Must have [Visual Studio Code](https://code.visualstudio.com/) installed</span></span>
- <span data-ttu-id="b3a1f-113">[.NET Core 2.1](https://www.microsoft.com/net/download) がインストールされている必要があります</span><span class="sxs-lookup"><span data-stu-id="b3a1f-113">Must have [.NET Core 2.1](https://www.microsoft.com/net/download) installed</span></span>
- <span data-ttu-id="b3a1f-114">[Azure アカウント](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)拡張機能が Visual Studio Code にインストールされている必要があります</span><span class="sxs-lookup"><span data-stu-id="b3a1f-114">Must have the [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extension installed in Visual Studio Code</span></span>
