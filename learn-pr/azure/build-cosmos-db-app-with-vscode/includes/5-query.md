<!--TODO: Explain how to do ExecuteNext (pages closer to SDK imp) vs ToList (continuation token)--> ご利用のアプリケーションでドキュメントが作成されました。それでは、アプリケーションからドキュメントにクエリを実行してみましょう。 Azure Cosmos DB では、SQL クエリと LINQ クエリが使用されます。 このユニットでは、ポータルではなく、アプリケーションから SQL クエリと LINQ クエリを実行する方法について重点的に取り上げます。

オンライン小売店アプリケーションのために作成したユーザー ドキュメントを利用し、これらのクエリをテストします。

## <a name="linq-query-basics"></a>LINQ クエリの基本

LINQ は、計算処理をオブジェクトのストリームに対するクエリとして表現する .NET プログラミング モデルです。 Azure Cosmos DB に直接照会する **IQueryable** オブジェクトを作成できます。照会すると、LINQ クエリが Cosmos DB クエリに変換されます。 次にクエリが Azure Cosmos DB サーバーに渡されることで、結果セットが JSON 形式で取得されます。 返された結果は、クライアント側で .NET オブジェクトのストリームに逆シリアル化されます。 LINQ クエリはアプリケーション コードにおけるオブジェクトとの連携方法とデータベースで実行されるクエリ ロジックの表現方法に 1 つの一貫性のあるプログラミング モデルを提供するため、開発者の多くは LINQ クエリを好みます。

次の表は、LINQ クエリが SQL に変換されるしくみをまとめたものです。

| LINQ 式 | SQL 変換 |
|---|---|
| `input.Select(family => family.parents[0].familyName);`| `SELECT VALUE f.parents[0].familyName FROM Families f` |
|`input.Select(family => family.children[0].grade + c); // c is an int variable` | `SELECT VALUE f.children[0].grade + c FROM Families f` |
|`input.Select(family => new { name = family.children[0].familyName, grade = family.children[0].grade + 3});`| `SELECT VALUE {"name":f.children[0].familyName, "grade": f.children[0].grade + 3 } FROM Families f`|
|`input.Where(family=> family.parents[0].familyName == "Smith");`|`SELECT * FROM Families f WHERE f.parents[0].familyName = "Smith"`|

## <a name="run-sql-and-linq-queries"></a>SQL クエリと LINQ クエリの実行

1. 次のサンプルでは、.NET コードから SQL、LINQ、LINQ lambda でクエリが実行される様子を確認できます。 コードをコピーし、Program.cs ファイルの末尾に追加します。

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

1. 次のコードをコピーし、**BasicOperations** メソッドの `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` 行の前に貼り付けます。

    ```csharp
    this.ExecuteSimpleQuery("Users", "WebCustomers");
    ```

1. 統合ターミナルで、次のコマンドを実行します。

    ```bash
    dotnet run
    ```

    コンソールには、LINQ および SQL クエリの出力が表示されます。

この演習では、LINQ クエリについて学習してから、LINQ クエリと SQL クエリをご利用のアプリケーションに追加し、ユーザー レコードを取得しました。
