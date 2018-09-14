<span data-ttu-id="95be1-101">データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="95be1-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="95be1-102">このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="95be1-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="95be1-103">アプリでストアド プロシージャを作成する</span><span class="sxs-lookup"><span data-stu-id="95be1-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="95be1-104">このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="95be1-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="95be1-105">注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="95be1-105">The order total is calculated from the sum of the items in the order, less any dividend (credit) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="95be1-106">次のコードをコピーして、Program.cs ファイルの末尾に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="95be1-106">Copy the following code and paste it into the end of the Program.cs file.</span></span>

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

2. <span data-ttu-id="95be1-107">次のコードをコピーして、**BasicOperations** メソッドの末尾に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="95be1-107">Now copy the following code and paste it into the end of the **BasicOperations** method.</span></span>

    ```
    await this.UpdateOrderTotal("Users", "WebCustomers", "5-6235");
    ```

3. <span data-ttu-id="95be1-108">Program.cs ファイルを保存し、統合ターミナルで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="95be1-108">Save the Program.cs file and then, in the integrated terminal, run the following command.</span></span>

    ```
    dotnet run
    ```

    <span data-ttu-id="95be1-109">コンソールには次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="95be1-109">The console displays the following output.</span></span>

    ```
    Order total is 90.85.
    Press any key to continue ...
    ```

## <a name="clean-up"></a><span data-ttu-id="95be1-110">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="95be1-110">Clean up</span></span>

<span data-ttu-id="95be1-111">このラーニング パスのモジュールを引き続き行う場合は、クリーンアップ プロセスをスキップしてください。それ以外の場合は、次の手順を使用して、サービスの使用料がかからないようにリソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="95be1-111">If you plan to continue working on the modules in this learning path, skip the clean-up process, or else use the following steps to delete your resources to avoid incurring charges for use of the service.</span></span>

1. <span data-ttu-id="95be1-112">Azure Portal の左端で **[リソース グループ]** を選択し、作成したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="95be1-112">In the Azure portal, select **Resource groups** on the far left, and then select the resource group you created.</span></span>  

    <span data-ttu-id="95be1-113">左側のメニューが折りたたまれている場合は、</span><span class="sxs-lookup"><span data-stu-id="95be1-113">If the left menu is collapsed, click</span></span> ![展開ボタン](../media/5-javascript-programming/expand.png) <span data-ttu-id="95be1-115">をクリックして展開します。</span><span class="sxs-lookup"><span data-stu-id="95be1-115">to expand it.</span></span>

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources-select.png)

2. <span data-ttu-id="95be1-117">新しいウィンドウでリソース グループを選択し、**[リソース グループの削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="95be1-117">In the new window select the resource group, and then click **Delete resource group**.</span></span>

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources.png)

3. <span data-ttu-id="95be1-119">新しいウィンドウで、削除するリソース グループの名前を入力し、**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="95be1-119">In the new window, type the name of the resource group to delete, and then click **Delete**.</span></span>

## <a name="summary"></a><span data-ttu-id="95be1-120">まとめ</span><span class="sxs-lookup"><span data-stu-id="95be1-120">Summary</span></span>

<span data-ttu-id="95be1-121">このモジュールで作成した .NET Core コンソール アプリケーションでは、ユーザー レコードを作成、更新、削除し、SQL と LINQ を使用してユーザーのクエリを行い、ユーザーの給付金とクーポンを考慮した注文合計を作成するストアド プロシージャを実行しました。</span><span class="sxs-lookup"><span data-stu-id="95be1-121">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to create an order total that takes the users dividends and coupons into account.</span></span>