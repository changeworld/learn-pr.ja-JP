<span data-ttu-id="46b57-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> ご利用のアプリケーションでドキュメントが作成されました。それでは、アプリケーションからドキュメントにクエリを実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="46b57-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Now that you've created documents in your application, let's query them from your application.</span></span> <span data-ttu-id="46b57-102">Azure Cosmos DB では、SQL クエリと LINQ クエリが使用されます。</span><span class="sxs-lookup"><span data-stu-id="46b57-102">Azure Cosmos DB uses SQL queries and LINQ queries.</span></span> <span data-ttu-id="46b57-103">このユニットでは、ポータルではなく、アプリケーションから SQL クエリと LINQ クエリを実行する方法について重点的に取り上げます。</span><span class="sxs-lookup"><span data-stu-id="46b57-103">This unit focuses on running SQL queries and LINQ queries from your application, as opposed to the portal.</span></span>

<span data-ttu-id="46b57-104">オンライン小売店アプリケーションのために作成したユーザー ドキュメントを利用し、これらのクエリをテストします。</span><span class="sxs-lookup"><span data-stu-id="46b57-104">We'll use the user documents you've created for your online retailer application to test these queries.</span></span>

## <a name="linq-query-basics"></a><span data-ttu-id="46b57-105">LINQ クエリの基本</span><span class="sxs-lookup"><span data-stu-id="46b57-105">LINQ query basics</span></span>

<span data-ttu-id="46b57-106">LINQ は、計算処理をオブジェクトのストリームに対するクエリとして表現する .NET プログラミング モデルです。</span><span class="sxs-lookup"><span data-stu-id="46b57-106">LINQ is a .NET programming model that expresses computations as queries on streams of objects.</span></span> <span data-ttu-id="46b57-107">Azure Cosmos DB に直接照会する **IQueryable** オブジェクトを作成できます。照会すると、LINQ クエリが Cosmos DB クエリに変換されます。</span><span class="sxs-lookup"><span data-stu-id="46b57-107">You can create an **IQueryable** object that directly queries Azure Cosmos DB, which translates the LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="46b57-108">次にクエリが Azure Cosmos DB サーバーに渡されることで、結果セットが JSON 形式で取得されます。</span><span class="sxs-lookup"><span data-stu-id="46b57-108">The query is then passed to the Azure Cosmos DB server to retrieve a set of results in JSON format.</span></span> <span data-ttu-id="46b57-109">返された結果は、クライアント側で .NET オブジェクトのストリームに逆シリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="46b57-109">The returned results are deserialized into a stream of .NET objects on the client side.</span></span> <span data-ttu-id="46b57-110">LINQ クエリはアプリケーション コードにおけるオブジェクトとの連携方法とデータベースで実行されるクエリ ロジックの表現方法に 1 つの一貫性のあるプログラミング モデルを提供するため、開発者の多くは LINQ クエリを好みます。</span><span class="sxs-lookup"><span data-stu-id="46b57-110">Many developers prefer LINQ queries, as they provide a single consistent programming model across how they work with objects in application code and how they express query logic running in the database.</span></span>

<span data-ttu-id="46b57-111">次の表は、LINQ クエリが SQL に変換されるしくみをまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="46b57-111">The following table shows how LINQ queries are translated into SQL.</span></span>

| <span data-ttu-id="46b57-112">LINQ 式</span><span class="sxs-lookup"><span data-stu-id="46b57-112">LINQ expression</span></span> | <span data-ttu-id="46b57-113">SQL 変換</span><span class="sxs-lookup"><span data-stu-id="46b57-113">SQL translation</span></span> |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a><span data-ttu-id="46b57-114">SQL クエリと LINQ クエリの実行</span><span class="sxs-lookup"><span data-stu-id="46b57-114">Run SQL and LINQ queries</span></span>

1. <span data-ttu-id="46b57-115">次のサンプルでは、.NET コードから SQL、LINQ、LINQ lambda でクエリが実行される様子を確認できます。</span><span class="sxs-lookup"><span data-stu-id="46b57-115">The following sample shows how a query could be performed in SQL, LINQ, or LINQ lambda from your .NET code.</span></span> <span data-ttu-id="46b57-116">コードをコピーし、Program.cs ファイルの末尾に追加します。</span><span class="sxs-lookup"><span data-stu-id="46b57-116">Copy the code and add it to the end of the Program.cs file.</span></span>

    ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1, EnableCrossPartitionQuery = true };

        // Here we find nelapin via their LastName
        IQueryable<User> userQuery = this.client.CreateDocumentQuery<User>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(u => u.LastName == "Pindakova");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (User user in userQuery)
        {
            Console.WriteLine("\tRead {0}", user);
        }

        // Now execute the same query via direct SQL
        IQueryable<User> userQueryInSql = this.client.CreateDocumentQuery<User>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM User WHERE User.lastName = 'Pindakova'", queryOptions );

        Console.WriteLine("Running direct SQL query...");
        foreach (User user in userQueryInSql)
        {
                Console.WriteLine("\tRead {0}", user);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
    ```

1. <span data-ttu-id="46b57-117">次のコードをコピーし、**BasicOperations** メソッドの `await this.DeleteUserDocument("Users", "WebCustomers", "1");` 行の前に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="46b57-117">Copy and paste the following code to your **BasicOperations** method, before the `await this.DeleteUserDocument("Users", "WebCustomers", "1");` line.</span></span>

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

1. <span data-ttu-id="46b57-118">統合ターミナルで、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="46b57-118">In the integrated terminal, run the following command.</span></span>

    ```bash
    dotnet run
    ```

    <span data-ttu-id="46b57-119">コンソールには、LINQ および SQL クエリの出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="46b57-119">The console displays the output of the LINQ and SQL queries.</span></span>

<span data-ttu-id="46b57-120">この演習では、LINQ クエリについて学習してから、LINQ クエリと SQL クエリをご利用のアプリケーションに追加し、ユーザー レコードを取得しました。</span><span class="sxs-lookup"><span data-stu-id="46b57-120">In this unit you learned about LINQ queries, and then added a LINQ and SQL query to your application to retrieve user records.</span></span>
