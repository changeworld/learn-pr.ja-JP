データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。 このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。

## <a name="create-a-stored-procedure-in-your-app"></a>アプリでストアド プロシージャを作成する

このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。 注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。

1. Visual Studio Code の **[Azure: Cosmos DB]** タブで自分の Azure Cosmos DB アカウント、**[ユーザー]**、**[WebCustomers]** の順に展開し、**[ストアド プロシージャ]** を右クリックし、**[ストアド プロシージャの作成]** をクリックします。

1. 画面上部のテキスト ボックスに、「`UpdateOrderTotal`」と入力し、Enter キーをクリックしてストアド プロシージャに名前を付けます。

1. **[Azure: Cosmos DB]** タブで **[ストアド プロシージャ]** を展開し、**[UpdateOrderTotal]** をクリックします。

    既定では、最初の項目を取得するストアド プロシージャが提供されます。

1. アプリケーションからこの既定のストアド プロシージャを実行するには、**Program.cs** ファイルに次のコードを追加します。

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
        Console.WriteLine("Stored procedure complete");
    }
    ```

1. 次のコードをコピーして、**BasicOperations** メソッドの `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` 行の前に貼り付けます。

    ```csharp
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. 統合端末で次のコマンドを実行し、ストアド プロシージャを使用してサンプルを実行します。

    ```bash
    dotnet run
    ```

コンソールに、ストアド プロシージャが完了したことを示す出力が表示されます。
