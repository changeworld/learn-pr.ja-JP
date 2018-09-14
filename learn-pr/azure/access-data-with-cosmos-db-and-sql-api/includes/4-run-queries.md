<span data-ttu-id="ff18c-101">作成できるクエリの種類について学習しましたので、Azure portal でデータ エクスプローラーを使用して、ご自分の製品データを取得およびフィルター処理してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ff18c-101">Now that you've learned about what kinds of queries you can create, let's use the Data Explorer in the Azure portal to retrieve and filter your product data.</span></span>

<span data-ttu-id="ff18c-102">データ エクスプローラー ウィンドウでは、既定で、**[ドキュメント]** タブのクエリが `SELECT * FROM c` に設定されています。</span><span class="sxs-lookup"><span data-stu-id="ff18c-102">In your Data Explorer window, note that by default, the query on the **Document** tab is set to `SELECT * FROM c`.</span></span> <span data-ttu-id="ff18c-103">この既定のクエリでは、コレクション内のすべてのドキュメントが取得されて表示されます。</span><span class="sxs-lookup"><span data-stu-id="ff18c-103">This default query retrieves and displays all documents in the collection.</span></span>

![データ エクスプローラーの既定のクエリは、"SELECT \* FROM c" です](../media-draft/4-run-queries/azure-cosmosdb-data-explorer-query.png)

## <a name="create-a-new-query"></a><span data-ttu-id="ff18c-105">新しいクエリを作成する</span><span class="sxs-lookup"><span data-stu-id="ff18c-105">Create a new query</span></span>

1. <span data-ttu-id="ff18c-106">データ エクスプローラーで、**[新しい SQL クエリ]** タブをクリックします。新しい **[クエリ 1]** タブ上の既定のクエリが `SELECT * from c` になっていることを確認し、**[クエリの実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ff18c-106">In Data Explorer, click the **New SQL Query** tab. Note that the default query on the new  **Query 1** tab is `SELECT * from c`, and then click **Execute Query**.</span></span> <span data-ttu-id="ff18c-107">このクエリでは、そのデータベースの結果がすべて返されます。</span><span class="sxs-lookup"><span data-stu-id="ff18c-107">This query returns all results in the database.</span></span>

    ![ORDER BY c._ts DESC を追加し [フィルタの適用] をクリックすることで既定のクエリを変更する](../media-draft/4-run-queries/azure-cosmosdb-data-explorer-edit-query.png)

2. <span data-ttu-id="ff18c-109">前のユニットで説明したクエリの一部を実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ff18c-109">Now, let's run some of the queries discussed in the previous unit.</span></span> <span data-ttu-id="ff18c-110">クエリ タブで、`SELECT * from c` を削除し、次のクエリをコピーして貼り付け、**[クエリの実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ff18c-110">On the query tab, delete `SELECT * from c`, copy and paste the following query, and then click **Execute Query**:</span></span>

    ```
    SELECT *
    FROM Products p
    WHERE p.id ="1"
    ```

    <span data-ttu-id="ff18c-111">結果には、`productId` が 1 の製品が返されます。</span><span class="sxs-lookup"><span data-stu-id="ff18c-111">The results return the product whose `productId` is 1.</span></span>

    ![「ORDER BY c._ts DESC」を追加し [フィルタの適用] をクリックすることで、既定のクエリを変更](../media-draft/4-run-queries/azure-cosmosdb-data-explorer-query-by-id.png)

3. <span data-ttu-id="ff18c-113">前のクエリを削除し、次のクエリをコピーして貼り付け、**[クエリの実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ff18c-113">Delete the previous query, copy and paste the following query, and click **Execute Query**.</span></span> <span data-ttu-id="ff18c-114">このクエリでは、すべての製品の価格、説明、および製品 ID が、価格の昇順に並べ替えられて返されます。</span><span class="sxs-lookup"><span data-stu-id="ff18c-114">This query returns the price, description, and product ID for all products, ordered by price, in ascending order.</span></span>
 
    ```
    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC
    ```

## <a name="summary"></a><span data-ttu-id="ff18c-115">まとめ</span><span class="sxs-lookup"><span data-stu-id="ff18c-115">Summary</span></span>

<span data-ttu-id="ff18c-116">これで、Azure Cosmos DB のデータでの基本的なクエリを完了しました。</span><span class="sxs-lookup"><span data-stu-id="ff18c-116">You have now completed some basic queries on your data in Azure Cosmos DB.</span></span> 