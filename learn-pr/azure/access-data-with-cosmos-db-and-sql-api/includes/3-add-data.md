<span data-ttu-id="97334-101">Azure Cosmos DB データベースにデータを追加するのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="97334-101">Adding data to your Azure Cosmos DB database should be simple.</span></span> <span data-ttu-id="97334-102">Azure portal を開いてデータベースに移動し、データ エクスプローラーを使用して JSON ドキュメントをデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="97334-102">You open the Azure portal, navigate to your database, and use the Data Explorer to add JSON documents to the database.</span></span> <span data-ttu-id="97334-103">データを追加するには、他にも高度な方法がありますが、データ エクスプローラーは Azure Cosmos DB によって提供される内部動作と機能を理解するのに最適なツールなので、まずはデータ エクスプローラーを使用します。</span><span class="sxs-lookup"><span data-stu-id="97334-103">There are more advanced ways to add data, but we'll start here because the Data Explorer is a great tool to get you acquainted with the inner workings and functionality provided by Azure Cosmos DB.</span></span>

## <a name="what-is-the-data-explorer"></a><span data-ttu-id="97334-104">データ エクスプローラーとは</span><span class="sxs-lookup"><span data-stu-id="97334-104">What is the Data Explorer?</span></span>
<span data-ttu-id="97334-105">Azure Cosmos DB データ エクスプローラーは、Azure portal に含まれるツールであり、Azure Cosmos DB に格納されているデータを管理するために使用します。</span><span class="sxs-lookup"><span data-stu-id="97334-105">The Azure Cosmos DB Data Explorer is a tool included in the Azure portal that is used to manage data stored in an Azure Cosmos DB.</span></span> <span data-ttu-id="97334-106">データ コレクションの表示とデータ コレクション内の移動、およびデータベース内のドキュメントの編集のための UI が提供されています。</span><span class="sxs-lookup"><span data-stu-id="97334-106">It provides a UI for viewing and navigating data collections, as well as for editing documents within the database.</span></span>

## <a name="add-data-using-the-data-explorer"></a><span data-ttu-id="97334-107">データ エクスプローラーを使用してデータを追加する</span><span class="sxs-lookup"><span data-stu-id="97334-107">Add data using the Data Explorer</span></span>

1. <span data-ttu-id="97334-108">前のモジュールから続けている場合は、Azure portal ウィンドウで **[データ エクスプローラー]** をクリックして、**[全画面表示で開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97334-108">If you're continuing from the previous module, click **Data Explorer** in the Azure portal window, and then click **Open Full Screen**.</span></span>

    <span data-ttu-id="97334-109">それ以外の場合は、[Azure portal](https://portal.azure.com/?azure-portal=true) にサインインし、**[すべてのサービス]** > **[データベース]** > **[Azure Cosmos DB]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97334-109">Otherwise, sign in to the [Azure portal](https://portal.azure.com/?azure-portal=true), click **All services** > **Databases** > **Azure Cosmos DB**.</span></span> <span data-ttu-id="97334-110">次に、ご自分のアカウントを選択し、**[データ エクスプローラー]**、**[全画面表示で開く]** の順にクリックします</span><span class="sxs-lookup"><span data-stu-id="97334-110">Then select your account, click **Data Explorer**, and then click **Open Full Screen**</span></span>
 
   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media-draft/3-azure-cosmosdb-data-explorer-full-screen.png)

2. <span data-ttu-id="97334-112">**[全画面表示で開く]** ボックスで、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97334-112">In the **Open Full Screen** box, click **Open**.</span></span>

    <span data-ttu-id="97334-113">Web ブラウザーでは、新しいデータ エクスプローラーが全画面表示され、データベースを操作するための多くの領域と専用の環境が提供されます。</span><span class="sxs-lookup"><span data-stu-id="97334-113">The web browser displays the new full-screen Data Explorer, which gives you more space and a dedicated environment for working with your database.</span></span>

3. <span data-ttu-id="97334-114">新しい JSON ドキュメントを作成するには、**[新しいドキュメント]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97334-114">To create a new JSON document, click **New Document**.</span></span>

   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media-draft/3-azure-cosmosdb-data-explorer-new-document.png)

4. <span data-ttu-id="97334-116">ここで、次の構造のドキュメントをコレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="97334-116">Now, add a document to the collection with the following structure.</span></span> <span data-ttu-id="97334-117">次のコードをコピーして、**[ドキュメント]** タブに貼り付けるだけです。</span><span class="sxs-lookup"><span data-stu-id="97334-117">Just copy and paste the following code into the **Documents** tab:</span></span>

     ```
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "shipping": {
            "weight": 1,
            "dimensions": {
            "width": 6,
            "height": 8,
            "depth": 1
           }
        }
     ```

5. <span data-ttu-id="97334-118">JSON を **[ドキュメント]** タブに追加したら、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="97334-118">Once you've added the JSON to the **Documents** tab, click **Save**.</span></span>

    ![JSON データをコピーし、Azure portal のデータ エクスプローラーで [保存] をクリックします](../media-draft/3-azure-cosmosdb-data-explorer-save-document.png)

6. <span data-ttu-id="97334-120">次の JSON オブジェクトをデータ エクスプローラーにコピーして、**[保存]** をクリックすることによって、ドキュメントをもう 1 つ作成して保存します。</span><span class="sxs-lookup"><span data-stu-id="97334-120">Create and save one more document by copying the following JSON object into Data Explorer and clicking **Save**.</span></span>

     ```
    {
        "id": "2",
        "productId": "33218897",
        "category": "Women's Outerwear",
        "manufacturer": "Contoso",
        "description": "Black wool pea-coat",
        "shipping": {
            "weight": 2,
            "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
           }
        }
    }
     ```

7. <span data-ttu-id="97334-121">左側のメニューで **[ドキュメント]** をクリックして、ドキュメントが保存されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="97334-121">Confirm the documents have been saved by clicking **Documents** on the left-hand menu.</span></span> 

    <span data-ttu-id="97334-122">データ エクスプローラーには、**[ドキュメント]** タブにドキュメントが 2 つ表示されます。</span><span class="sxs-lookup"><span data-stu-id="97334-122">Data Explorer displays the two documents in the **Documents** tab.</span></span>

## <a name="summary"></a><span data-ttu-id="97334-123">まとめ</span><span class="sxs-lookup"><span data-stu-id="97334-123">Summary</span></span>

<span data-ttu-id="97334-124">このモジュールでは、データ エクスプローラーを使用して、それぞれが製品カタログ内の製品を表す 2 つのドキュメントを、データベースに追加しました。</span><span class="sxs-lookup"><span data-stu-id="97334-124">In this module, you added two documents, each representing a product in your product catalog, to your database by using the Data Explorer.</span></span> <span data-ttu-id="97334-125">データ エクスプローラーは、ドキュメントの作成、ドキュメントの変更、Azure Cosmos DB の使用開始に適しています。</span><span class="sxs-lookup"><span data-stu-id="97334-125">The Data Explorer is a good way to create documents, modify documents, and get started with Azure Cosmos DB.</span></span>  
