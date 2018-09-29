<span data-ttu-id="aca42-101">利用のデータベースにデータを追加し、データをクエリするのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="aca42-101">Adding data and querying data in your database should be straightforward.</span></span> 

<span data-ttu-id="aca42-102">オンラインの衣料販売店で仕事をしていて、Azure Cosmos DB にあるご利用の eコマース データベースに在庫データを追加する必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="aca42-102">Imagine you work for an online clothing retailer and you need to add inventory data to your e-commerce database in Azure Cosmos DB.</span></span> <span data-ttu-id="aca42-103">データがそのデータベースにあるときは、使い慣れた SQL クエリを使用してデータをクエリし (SQL Server で使用するものと同様)、ストアド プロシージャとユーザー定義関数 (UDF) を使用することによって複雑な操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="aca42-103">Once the data is in the database, you can query it by using familiar SQL queries (yes, just like the ones you use in SQL Server) and perform complex operations by using stored procedures and user-defined functions (UDFs).</span></span>

<span data-ttu-id="aca42-104">Azure Cosmos DB では、Azure portal でデータ エクスプローラーを提供します。これを使用すると、データの追加、データの変更、ストアド プロシージャの作成と実行といった操作をすべて実行できます。</span><span class="sxs-lookup"><span data-stu-id="aca42-104">Azure Cosmos DB provides a Data Explorer in the Azure portal that you can use to perform all these operations: adding data, modifying data, and creating and running stored procedures.</span></span> <span data-ttu-id="aca42-105">データ エクスプローラーは、Azure portal で使用したり、ドッキングを解除し、スタンドアロンの Web ブラウザーで使用したりできます。ブラウザーで使用すれば、データを扱うための領域が増えます。</span><span class="sxs-lookup"><span data-stu-id="aca42-105">The Data Explorer can be used in the Azure portal or it can be undocked and used in a standalone web browser, providing additional space to work with your data.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="aca42-106">学習の目的</span><span class="sxs-lookup"><span data-stu-id="aca42-106">Learning objectives</span></span>

<span data-ttu-id="aca42-107">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="aca42-107">In this module, you will:</span></span>

- <span data-ttu-id="aca42-108">データ エクスプローラーで製品カタログのドキュメントを作成する</span><span class="sxs-lookup"><span data-stu-id="aca42-108">Create product catalog documents in the Data Explorer</span></span>
- <span data-ttu-id="aca42-109">Azure Cosmos DB クエリを実行する</span><span class="sxs-lookup"><span data-stu-id="aca42-109">Perform Azure Cosmos DB queries</span></span>
- <span data-ttu-id="aca42-110">ストアド プロシージャを使用することによって、自分のドキュメントで操作を作成して実行する</span><span class="sxs-lookup"><span data-stu-id="aca42-110">Create and run operations on your documents by using stored procedures</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aca42-111">前提条件</span><span class="sxs-lookup"><span data-stu-id="aca42-111">Prerequisites</span></span>

- <span data-ttu-id="aca42-112">データベースとクエリを理解している</span><span class="sxs-lookup"><span data-stu-id="aca42-112">Have an understanding of databases and queries</span></span>
