データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。 このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。

## <a name="create-a-stored-procedure-in-your-app"></a>アプリでストアド プロシージャを作成する

このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。 注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。

1. Visual Studio Code の [Azure] タブで **[learning-module (SQL)]\(学習モジュール (SQL)\)** > **[ユーザー]** > **[WebCustomers]** の順に展開し、**[ストアド プロシージャ]** を右クリックして、[**ストアド プロシージャの作成]** をクリックします。

1. 画面上部のテキスト ボックスに「*UpdateOrderTotal*」と入力し、Enter キーを押して、ストアド プロシージャに名前を付けます。

1. **[ストアド プロシージャ]** を展開し、**UpdateOrderTotal** をクリックします。

1. 既定では、最初の項目を取得するストアド プロシージャが提供されます。

1. アプリケーションからこのストアド プロシージャを実行するには、Program.cs ファイルに次のコードを追加します。

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "sample"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```
    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->

1. 次のコードをコピーして、**BasicOperations** メソッドの末尾に貼り付けます。

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. 統合端末で次のコマンドを実行し、ストアド プロシージャを使用してサンプルを実行します。

    ```
    dotnet run
    ```
    コンソールに、ストアド プロシージャが完了したことを示す出力が表示されます。

## <a name="clean-up"></a>クリーンアップ

このラーニング パスのモジュールを引き続き行う場合は、クリーンアップ プロセスをスキップしてください。それ以外の場合は、次の手順を使用して、サービスの使用料がかからないようにリソースを削除します。

1. Azure Portal の左端で **[リソース グループ]** を選択し、作成したリソース グループを選択します。  

    左側のメニューが折りたたまれている場合は、 ![展開ボタン](../media/5-javascript-programming/expand.png) をクリックして展開します。

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources-select.png)

1. 新しいウィンドウでリソース グループを選択し、**[リソース グループの削除]** をクリックします。

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources.png)

1. 新しいウィンドウで、削除するリソース グループの名前を入力し、**[削除]** をクリックします。

## <a name="summary"></a>まとめ

このモジュールで作成した .NET Core コンソール アプリケーションでは、ユーザー レコードを作成、更新、削除し、SQL と LINQ を使用してユーザーのクエリを行い、データベース内の項目に対してクエリを実行するストアド プロシージャを実行しました。