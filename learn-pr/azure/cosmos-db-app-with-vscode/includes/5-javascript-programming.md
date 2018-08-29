データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。 このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。

## <a name="create-a-stored-procedure-in-your-app"></a>アプリでストアド プロシージャを作成する

このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。 注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。

1. 次のコードをコピーして、Program.cs ファイルの末尾に貼り付けます。

    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->
    ```csharp
    private async Task UpdateOrderTotal(string databaseName, string collectionName, Order orderId)
    {
    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);
    }
    ```

2. 次のコードをコピーして、**BasicOperations** メソッドの末尾に貼り付けます。

    ```
    await this.UpdateOrderTotal("Users", "WebCustomers", "5-6235");
    ```

3. Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。

    ```
    dotnet run
    ```

    コンソールには次の出力が表示されます。

    ```
    Order total is 90.85.
    Press any key to continue ...
    ```

## <a name="clean-up"></a>クリーンアップ

このラーニング パスのモジュールを引き続き行う場合は、クリーンアップ プロセスをスキップしてください。それ以外の場合は、次の手順を使用して、サービスの使用料がかからないようにリソースを削除します。

1. Azure Portal の左端で **[リソース グループ]** を選択し、作成したリソース グループを選択します。  

    左側のメニューが折りたたまれている場合は、 ![展開ボタン](../media/5-javascript-programming/expand.png) をクリックして展開します。

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources-select.png)

2. 新しいウィンドウでリソース グループを選択し、**[リソース グループの削除]** をクリックします。

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources.png)

3. 新しいウィンドウで、削除するリソース グループの名前を入力し、**[削除]** をクリックします。

## <a name="summary"></a>まとめ

このモジュールで作成した .NET Core コンソール アプリケーションでは、ユーザー レコードを作成、更新、削除し、SQL と LINQ を使用してユーザーのクエリを行い、ユーザーの給付金とクーポンを考慮した注文合計を作成するストアド プロシージャを実行しました。