データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。 このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。

## <a name="create-a-stored-procedure-in-your-app"></a>アプリでストアド プロシージャを作成する

このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。 注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。

1. Visual Studio Code での**Azure: Cosmos DB**  タブで、Azure Cosmos DB アカウントを展開 >**ユーザー** > **WebCustomers** を右クリックし、**ストアド プロシージャ**し**ストアド プロシージャの作成**です。

1. 画面上部のテキスト ボックスに、「*UpdateOrderTotal*」と入力し、Enter キーをクリックして、ストアド プロシージャに名前を付けます。

1. **Azure: Cosmos DB**  タブで、展開**ストアド プロシージャ**クリック**UpdateOrderTotal**します。

    既定では、最初の項目を取得するストアド プロシージャが提供されます。

1. アプリケーションからこのストアド プロシージャを実行するには、Program.cs ファイルに次のコードを追加します。

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```

1. ここで、次のコードをコピーし、前に貼り付けます、`await this.DeleteUserDocument("Users", "WebCustomers", yanhe);`行、 **BasicOperations**メソッド。

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. 統合端末で次のコマンドを実行し、ストアド プロシージャを使用してサンプルを実行します。

    ```
    dotnet run
    ```
    コンソールに、ストアド プロシージャが完了したことを示す出力が表示されます。

## <a name="summary"></a>まとめ

このモジュールで作成した .NET Core コンソール アプリケーションでは、ユーザー レコードを作成、更新、削除し、SQL と LINQ を使用してユーザーのクエリを行い、データベース内の項目に対してクエリを実行するストアド プロシージャを実行しました。

[!include[](../../../includes/azure-sandbox-cleanup.md)]