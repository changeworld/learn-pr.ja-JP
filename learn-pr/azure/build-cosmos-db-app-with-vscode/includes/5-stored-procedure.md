<span data-ttu-id="00074-101">データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="00074-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="00074-102">このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="00074-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="00074-103">アプリでストアド プロシージャを作成する</span><span class="sxs-lookup"><span data-stu-id="00074-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="00074-104">このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="00074-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="00074-105">注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="00074-105">The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="00074-106">Visual Studio Code での**Azure: Cosmos DB**  タブで、Azure Cosmos DB アカウントを展開 >**ユーザー** > **WebCustomers** を右クリックし、**ストアド プロシージャ**し**ストアド プロシージャの作成**です。</span><span class="sxs-lookup"><span data-stu-id="00074-106">In Visual Studio Code, in the **Azure: Cosmos DB** tab, expand your Azure Cosmos DB account > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.</span></span>

1. <span data-ttu-id="00074-107">画面上部のテキスト ボックスに、「*UpdateOrderTotal*」と入力し、Enter キーをクリックして、ストアド プロシージャに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="00074-107">In the text box at the top of the screen, type *UpdateOrderTotal* and click Enter to give the stored procedure a name.</span></span>

1. <span data-ttu-id="00074-108">**Azure: Cosmos DB**  タブで、展開**ストアド プロシージャ**クリック**UpdateOrderTotal**します。</span><span class="sxs-lookup"><span data-stu-id="00074-108">In the **Azure: Cosmos DB** tab, expand **Stored Procedures** and click **UpdateOrderTotal**.</span></span>

    <span data-ttu-id="00074-109">既定では、最初の項目を取得するストアド プロシージャが提供されます。</span><span class="sxs-lookup"><span data-stu-id="00074-109">By default, a stored procedure that retrieves the first item is provided.</span></span>

1. <span data-ttu-id="00074-110">アプリケーションからこのストアド プロシージャを実行するには、Program.cs ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="00074-110">To run this stored procedure from your application, add the following code to the Program.cs file.</span></span>

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

1. <span data-ttu-id="00074-111">ここで、次のコードをコピーし、前に貼り付けます、`await this.DeleteUserDocument("Users", "WebCustomers", yanhe);`行、 **BasicOperations**メソッド。</span><span class="sxs-lookup"><span data-stu-id="00074-111">Now copy the following code and paste it before the `await this.DeleteUserDocument("Users", "WebCustomers", yanhe);` line in the **BasicOperations** method.</span></span>

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="00074-112">統合端末で次のコマンドを実行し、ストアド プロシージャを使用してサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="00074-112">In the integrated terminal, run the following command to run the sample with the stored procedure.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="00074-113">コンソールに、ストアド プロシージャが完了したことを示す出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00074-113">The console displays output indicating that the stored procedure was completed.</span></span>

## <a name="summary"></a><span data-ttu-id="00074-114">まとめ</span><span class="sxs-lookup"><span data-stu-id="00074-114">Summary</span></span>

<span data-ttu-id="00074-115">このモジュールで作成した .NET Core コンソール アプリケーションでは、ユーザー レコードを作成、更新、削除し、SQL と LINQ を使用してユーザーのクエリを行い、データベース内の項目に対してクエリを実行するストアド プロシージャを実行しました。</span><span class="sxs-lookup"><span data-stu-id="00074-115">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to query items in the database.</span></span>

[!include[](../../../includes/azure-sandbox-cleanup.md)]