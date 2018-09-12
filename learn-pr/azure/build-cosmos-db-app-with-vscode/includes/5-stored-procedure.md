<span data-ttu-id="b67fd-101">データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="b67fd-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> <span data-ttu-id="b67fd-102">このユニットでは、.NET コンソール アプリケーションからストアド プロシージャを作成、登録、および実行する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b67fd-102">This unit discusses how to create, register, and run stored procedures from your .NET console application.</span></span>

## <a name="create-a-stored-procedure-in-your-app"></a><span data-ttu-id="b67fd-103">アプリでストアド プロシージャを作成する</span><span class="sxs-lookup"><span data-stu-id="b67fd-103">Create a stored procedure in your app</span></span>

<span data-ttu-id="b67fd-104">このストアド プロシージャでは、すべての項目の一覧が順番に格納されている OrderId を使用して注文合計を計算します。</span><span class="sxs-lookup"><span data-stu-id="b67fd-104">In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total.</span></span> <span data-ttu-id="b67fd-105">注文合計は、注文の項目を加算し、顧客が持っている給付金 (クレジット) を引き、すべてのクーポン コードを考慮することによって計算されます。</span><span class="sxs-lookup"><span data-stu-id="b67fd-105">The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.</span></span>

1. <span data-ttu-id="b67fd-106">Visual Studio Code の [Azure] タブで **[learning-module (SQL)]\(学習モジュール (SQL)\)** > **[ユーザー]** > **[WebCustomers]** の順に展開し、**[ストアド プロシージャ]** を右クリックし、**[ストアド プロシージャの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b67fd-106">In Visual Studio Code, in the Azure tab, expand the **learning-module (SQL)** > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.</span></span>

1. <span data-ttu-id="b67fd-107">画面上部のテキスト ボックスに、「*UpdateOrderTotal*」と入力し、Enter キーをクリックして、ストアド プロシージャに名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="b67fd-107">In the text box at the top of the screen, type *UpdateOrderTotal* and click Enter to give the stored procedure a name.</span></span>

1. <span data-ttu-id="b67fd-108">**[ストアド プロシージャ]** を展開し、**[UpdateOrderTotal]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b67fd-108">Expand **Stored Procedures** and click **UpdateOrderTotal**.</span></span>

1. <span data-ttu-id="b67fd-109">既定では、最初の項目を取得するストアド プロシージャが提供されます。</span><span class="sxs-lookup"><span data-stu-id="b67fd-109">By default, a stored procedure that retrieves the first item is provided.</span></span>

1. <span data-ttu-id="b67fd-110">アプリケーションからこのストアド プロシージャを実行するには、Program.cs ファイルに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="b67fd-110">To run this stored procedure from your application, add the following code to the Program.cs file.</span></span>

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

1. <span data-ttu-id="b67fd-111">次のコードをコピーして、**BasicOperations** メソッドの末尾に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="b67fd-111">Now copy the following code and paste it into the end of the **BasicOperations** method.</span></span>

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. <span data-ttu-id="b67fd-112">統合端末で次のコマンドを実行し、ストアド プロシージャを使用してサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="b67fd-112">In the integrated terminal, run the following command to run the sample with the stored procedure.</span></span>

    ```
    dotnet run
    ```
    <span data-ttu-id="b67fd-113">コンソールに、ストアド プロシージャが完了したことを示す出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b67fd-113">The console displays output indicating that the stored procedure was completed.</span></span>

## <a name="clean-up"></a><span data-ttu-id="b67fd-114">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="b67fd-114">Clean up</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="b67fd-115">このラーニング パスのモジュールを引き続き行う場合は、クリーンアップ プロセスをスキップしてください。それ以外の場合は、次の手順を使用して、サービスの使用料がかからないようにリソースを削除します。</span><span class="sxs-lookup"><span data-stu-id="b67fd-115">If you plan to continue working on the modules in this learning path, skip the clean-up process, or else use the following steps to delete your resources to avoid incurring charges for use of the service.</span></span>

1. <span data-ttu-id="b67fd-116">Azure Portal の左端で **[リソース グループ]** を選択し、作成したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="b67fd-116">In the Azure portal, select **Resource groups** on the far left, and then select the resource group you created.</span></span>  

    <span data-ttu-id="b67fd-117">左側のメニューが折りたたまれている場合は、</span><span class="sxs-lookup"><span data-stu-id="b67fd-117">If the left menu is collapsed, click</span></span> ![展開ボタン](../media/5-javascript-programming/expand.png) <span data-ttu-id="b67fd-119">をクリックして展開します。</span><span class="sxs-lookup"><span data-stu-id="b67fd-119">to expand it.</span></span>

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources-select.png)

1. <span data-ttu-id="b67fd-121">新しいウィンドウでリソース グループを選択し、**[リソース グループの削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b67fd-121">In the new window select the resource group, and then click **Delete resource group**.</span></span>

   ![Azure ポータルのメトリック](../media/5-javascript-programming/delete-resources.png)

1. <span data-ttu-id="b67fd-123">新しいウィンドウで、削除するリソース グループの名前を入力し、**[削除]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="b67fd-123">In the new window, type the name of the resource group to delete, and then click **Delete**.</span></span>

## <a name="summary"></a><span data-ttu-id="b67fd-124">まとめ</span><span class="sxs-lookup"><span data-stu-id="b67fd-124">Summary</span></span>

<span data-ttu-id="b67fd-125">このモジュールで作成した .NET Core コンソール アプリケーションでは、ユーザー レコードを作成、更新、削除し、SQL と LINQ を使用してユーザーのクエリを行い、データベース内の項目に対してクエリを実行するストアド プロシージャを実行しました。</span><span class="sxs-lookup"><span data-stu-id="b67fd-125">In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to query items in the database.</span></span>