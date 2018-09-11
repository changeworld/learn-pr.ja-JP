<span data-ttu-id="5a158-101">ご利用のデータベースにデータを追加し、データをクエリするのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="5a158-101">Adding data and querying data in your database should be straightforward.</span></span> 

<span data-ttu-id="5a158-102">オンラインの自動車部品販売店で仕事をしていて、Azure Cosmos DB にあるご利用の e コマース データベースに部品データを追加する必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="5a158-102">Image you work for an online car-parts retailer and you need to parts data to your e-commerce database in Azure Cosmos DB.</span></span> <span data-ttu-id="5a158-103">データがそのデータベースにあるときは、使い慣れた SQL クエリを使用してデータをクエリし (SQL Server で使用するものと同様)、ストアド プロシージャとユーザー定義関数を使用することによって複雑な操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="5a158-103">Once the data is in the database, you can query it by using familiar SQL queries (yes, just like the ones you use in SQL Server), and perform complex operations by using stored procedures and user defined functions.</span></span>

<span data-ttu-id="5a158-104">Azure Cosmos DB では、Azure portal でデータ エクスプローラーを提供します。これを使用すると、データの追加、データの変更、およびストアド プロシージャの作成と実行といった操作をすべて実行できます。</span><span class="sxs-lookup"><span data-stu-id="5a158-104">Azure Cosmos DB provides a Data Explorer in the Azure portal that you can use to perform all these operations - adding data, modifying data, and creating and running stored procedures.</span></span> <span data-ttu-id="5a158-105">データ エクスプローラーは、Azure portal で使用したり、ドッキングを解除し、ご自分のデータを操作するために追加の領域を提供することで、スタンドアロンの Web ブラウザーで使用したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="5a158-105">The Data Explorer can be used in the Azure portal, or can be undocked and used in a standalone web browser, providing additional space to work with your data.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="5a158-106">学習の目的</span><span class="sxs-lookup"><span data-stu-id="5a158-106">Learning objectives</span></span>

<span data-ttu-id="5a158-107">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="5a158-107">In this module, you will:</span></span>
- <span data-ttu-id="5a158-108">データ エクスプローラーで製品カタログのドキュメントを作成する</span><span class="sxs-lookup"><span data-stu-id="5a158-108">you create product catalog documents in the Data Explorer</span></span>
- <span data-ttu-id="5a158-109">Azure Cosmos DB クエリを実行する</span><span class="sxs-lookup"><span data-stu-id="5a158-109">perform Azure Cosmos DB queries</span></span>
- <span data-ttu-id="5a158-110">ストアド プロシージャを使用することによって、ご自分のドキュメントで操作を作成して実行する</span><span class="sxs-lookup"><span data-stu-id="5a158-110">create and run operations on your documents by using stored procedures</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a158-111">前提条件</span><span class="sxs-lookup"><span data-stu-id="5a158-111">Prerequisites</span></span>

- <span data-ttu-id="5a158-112">データベースとクエリを理解している</span><span class="sxs-lookup"><span data-stu-id="5a158-112">Have an understanding of databases and queries</span></span>
- <span data-ttu-id="5a158-113">"スケールのために組み込まれた Azure Cosmos DB データベースを作成する" モジュールで作成された、Azure Cosmos DB アカウント、データベース、およびコレクション</span><span class="sxs-lookup"><span data-stu-id="5a158-113">An Azure Cosmos DB account, database, and collection as created in the “Create an Azure Cosmos DB database built to scale” module</span></span>
