<span data-ttu-id="a969f-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> ご利用のアプリケーションでドキュメントが作成されました。それでは、アプリケーションからドキュメントにクエリを実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="a969f-101"><!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> Now that you've created documents in your application, let's query them from your application.</span></span> <span data-ttu-id="a969f-102">Azure Cosmos DB では、SQL クエリと LINQ クエリが使用されます。</span><span class="sxs-lookup"><span data-stu-id="a969f-102">Azure Cosmos DB uses SQL queries and LINQ queries.</span></span> <span data-ttu-id="a969f-103">SQL クエリとそれをポータルで実行する方法については、「**Add data and query data in your database**」 (データベースでデータを追加し、クエリを実行する) モジュールで説明します。</span><span class="sxs-lookup"><span data-stu-id="a969f-103">SQL queries and how to run them in the portal is discussed in the **Add data and query data in your database** module.</span></span> 

<span data-ttu-id="a969f-104">そのため、この演習では、アプリケーションから SQL クエリと LINQ クエリを実行する方法について重点的に取り上げます。</span><span class="sxs-lookup"><span data-stu-id="a969f-104">So this unit focuses on running SQL queries and LINQ queries from your application.</span></span>

<span data-ttu-id="a969f-105">オンライン小売店アプリケーションのために作成したユーザー ドキュメントを利用し、これらのクエリをテストします。</span><span class="sxs-lookup"><span data-stu-id="a969f-105">We'll use the user documents you've created for your online retailer application to test these queries.</span></span>

## <a name="linq-query-basics"></a><span data-ttu-id="a969f-106">LINQ クエリの基本</span><span class="sxs-lookup"><span data-stu-id="a969f-106">LINQ query basics</span></span>

<span data-ttu-id="a969f-107">LINQ は、計算処理をオブジェクトのストリームに対するクエリとして表現する .NET プログラミング モデルです。</span><span class="sxs-lookup"><span data-stu-id="a969f-107">LINQ is a .NET programming model that expresses computations as queries on streams of objects.</span></span> <span data-ttu-id="a969f-108">Azure Cosmos DB に直接照会する **IQueryable** オブジェクトを作成できます。照会すると、LINQ クエリが Cosmos DB クエリに変換されます。</span><span class="sxs-lookup"><span data-stu-id="a969f-108">You can create an **IQueryable** object that directly queries Azure Cosmos DB, which translates the LINQ query into a Cosmos DB query.</span></span> <span data-ttu-id="a969f-109">次にクエリが Azure Cosmos DB サーバーに渡されることで、結果セットが JSON 形式で取得されます。</span><span class="sxs-lookup"><span data-stu-id="a969f-109">The query is then passed to the Azure Cosmos DB server to retrieve a set of results in JSON format.</span></span> <span data-ttu-id="a969f-110">返された結果は、クライアント側で .NET オブジェクトのストリームに逆シリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="a969f-110">The returned results are deserialized into a stream of .NET objects on the client side.</span></span> <span data-ttu-id="a969f-111">LINQ クエリはアプリケーション コードにおけるオブジェクトとの連携方法とデータベースで実行されるクエリ ロジックの表現方法に 1 つの一貫性のあるプログラミング モデルを提供するため、開発者の多くは LINQ クエリを好みます。</span><span class="sxs-lookup"><span data-stu-id="a969f-111">Many developers prefer LINQ queries, as they provide a single consistent programming model across how they work with objects in application code and how they express query logic running in the database.</span></span>

<span data-ttu-id="a969f-112">次の表は、LINQ クエリが SQL に変換されるしくみをまとめたものです。</span><span class="sxs-lookup"><span data-stu-id="a969f-112">The following table shows how LINQ queries are translated into SQL.</span></span>

| <span data-ttu-id="a969f-113">LINQ 式</span><span class="sxs-lookup"><span data-stu-id="a969f-113">LINQ expression</span></span> | <span data-ttu-id="a969f-114">SQL 変換</span><span class="sxs-lookup"><span data-stu-id="a969f-114">SQL translation</span></span> |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a><span data-ttu-id="a969f-115">SQL クエリと LINQ クエリの実行</span><span class="sxs-lookup"><span data-stu-id="a969f-115">Run SQL and LINQ queries</span></span>

1. <span data-ttu-id="a969f-116">次のサンプルでは、.NET コードから SQL、LINQ、LINQ lambda でクエリが実行される様子を確認できます。</span><span class="sxs-lookup"><span data-stu-id="a969f-116">The following sample shows how a query could be performed in SQL, LINQ, or LINQ lambda from your .NET code.</span></span> <span data-ttu-id="a969f-117">コードをコピーし、**DeleteUserDocument**メソッドの後に追加します。</span><span class="sxs-lookup"><span data-stu-id="a969f-117">Copy the code and add it after your **DeleteUserDocument** method.</span></span>

    ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };
    
            // Here we find the Andersen family via its LastName
            IQueryable<USer> userQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(u => u.LastName == "Sun");
    
            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (User user in userQuery)
            {
                Console.WriteLine("\tRead {0}", user);
            }
    
            // Now execute the same query via direct SQL
            IQueryable<User> userQueryInSql = this.client.CreateDocumentQuery<User>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM User WHERE User.LastName = 'Sun'",
                    queryOptions);
    
            Console.WriteLine("Running direct SQL query...");
            foreach (User user in userQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", user);
            }
    
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }
    ```

2. <span data-ttu-id="a969f-118">次のコードをコピーし、**BasicOperations** メソッドの `await this.DeleteUserDocument("Users", "WebCustomers", "1");` 行の前に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="a969f-118">Copy and paste the following code to your **BasicOperations** method, before the `await this.DeleteUserDocument("Users", "WebCustomers", "1");` line.</span></span>

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

3. <span data-ttu-id="a969f-119">Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="a969f-119">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>
    
    ```
    dotnet run
    ```

    <span data-ttu-id="a969f-120">コンソールには次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a969f-120">The console displays the following output.</span></span>

    ```
    Running LINQ query...
    Running direct SQL query...
    Press any key to continue ...
    ```

    <span data-ttu-id="a969f-121">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="a969f-121">Congratulations!</span></span> <span data-ttu-id="a969f-122">SQL クエリと LINQ クエリを使用し、ユーザーの Azure Cosmos DB コレクションを用意できました。</span><span class="sxs-lookup"><span data-stu-id="a969f-122">You have successfully your Azure Cosmos DB collection of users by using SQL and LINQ queries.</span></span>

## <a name="summary"></a><span data-ttu-id="a969f-123">まとめ</span><span class="sxs-lookup"><span data-stu-id="a969f-123">Summary</span></span>

<span data-ttu-id="a969f-124">この演習では、LINQ クエリについて学習し、LINQ クエリと SQL クエリをアプリケーションに追加し、ユーザー レコードを取得しました。</span><span class="sxs-lookup"><span data-stu-id="a969f-124">In this unit you learned about LINQ queries, and then added a LINQ and SQL query to your application to retrieve user records.</span></span>