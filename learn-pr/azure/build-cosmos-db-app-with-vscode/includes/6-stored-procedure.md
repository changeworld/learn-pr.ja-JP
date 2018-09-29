<span data-ttu-id="b25f4-101">データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="b25f4-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="b25f4-102">このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b25f4-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="b25f4-103">アプリでストアド プロシージャを作成する</span><span class="sxs-lookup"><span data-stu-id="b25f4-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="b25f4-104">このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="b25f4-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="b25f4-105">注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="b25f4-105">The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="b25f4-106">Visual Studio Code の **[Azure: Cosmos DB]** タブで自分の Azure Cosmos DB アカウント、**[ユーザー]**、**[WebCustomers]** の順に展開し、**[ストアド プロシージャ]** を右クリックし、**[ストアド プロシージャの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b25f4-106">In Visual Studio Code, in the **Azure: Cosmos DB** tab, expand your Azure Cosmos DB account > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.</span></span>

1. <span data-ttu-id="b25f4-107">画面上部のテキスト ボックスに、「`UpdateOrderTotal`」と入力し、Enter キーをクリックしてストアド プロシージャに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="b25f4-107">In the text box at the top of the screen, type `UpdateOrderTotal` and press Enter to give the stored procedure a name.</span></span>

1. <span data-ttu-id="b25f4-108">**[Azure: Cosmos DB]** タブで **[ストアド プロシージャ]** を展開し、**[UpdateOrderTotal]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b25f4-108">In the **Azure: Cosmos DB** tab, expand **Stored Procedures** and click **UpdateOrderTotal**.</span></span>

    <span data-ttu-id="b25f4-109">既定では、最初の項目を取得するストアド プロシージャが提供されます。</span><span class="sxs-lookup"><span data-stu-id="b25f4-109">By default, a stored procedure that retrieves the first item is provided.</span></span>

1. <span data-ttu-id="b25f4-110">アプリケーションからこの既定のストアド プロシージャを実行するには、**Program.cs** ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="b25f4-110">To run this default stored procedure from your application, add the following code to the **Program.cs** file.</span></span>

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "UpdateOrderTotal"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
        Console.WriteLine("Stored procedure complete");
    }
    ```

1. <span data-ttu-id="b25f4-111">次のコードをコピーして、**BasicOperations** メソッドの `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` 行の前に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="b25f4-111">Now copy the following code and paste it before the `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` line in the **BasicOperations** method.</span></span>

    ```csharp
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="b25f4-112">統合端末で次のコマンドを実行し、ストアド プロシージャを使用してサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="b25f4-112">In the integrated terminal, run the following command to run the sample with the stored procedure.</span></span>

    ```bash
    dotnet run
    ```

<span data-ttu-id="b25f4-113">コンソールに、ストアド プロシージャが完了したことを示す出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b25f4-113">The console displays output indicating that the stored procedure was completed.</span></span>
