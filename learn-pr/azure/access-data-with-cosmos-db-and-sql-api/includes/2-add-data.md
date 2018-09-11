<span data-ttu-id="c3cd1-101">ご自分の Azure Cosmos DB データベースにデータを追加するのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-101">Adding data to your Azure Cosmos DB database should be simple.</span></span> <span data-ttu-id="c3cd1-102">Azure portal を開いてご自分のデータベースに移動し、**データ エクスプローラー**を使用して JSON ドキュメントをデータベースに追加します。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-102">You open the Azure portal, navigate to your database, and use the **Data Explorer** to add json documents to the database.</span></span> <span data-ttu-id="c3cd1-103">データを追加するには、他にも高度な方法がありますが、データ エクスプローラーが Azure Cosmos DB によって提供される内部動作と機能が明確にする最適なツールであるため、ここから開始します。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-103">There are more advanced ways to add data, but we'll start here as the Data Explorer is a great tool to get you aquainted with the inner workings and functionality provided by Azure Cosmos DB.</span></span>

## <a name="what-is-the-data-explorer"></a><span data-ttu-id="c3cd1-104">データ エクスプローラーとは何ですか?</span><span class="sxs-lookup"><span data-stu-id="c3cd1-104">What is the Data Explorer?</span></span>
<span data-ttu-id="c3cd1-105">Azure Cosmos DB データ エクスプローラーは、Azure portal に含まれる組み込みツールです。これは、Azure Cosmos DB で格納データを管理するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-105">The Azure Cosmos DB Data Explorer is a tool built included in the Azure Portal that is used to manage data stored in an Azure Cosmos DB.</span></span> <span data-ttu-id="c3cd1-106">ここでは、データ コレクションを表示および移動し、その DB 内のドキュメントを編集するための UI が提供されます。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-106">It provides UI to view and navigate data collections as well as edit documents within the db.</span></span>

## <a name="add-data-using-the-data-explorer"></a><span data-ttu-id="c3cd1-107">データ エクスプローラーを使用してデータを追加する</span><span class="sxs-lookup"><span data-stu-id="c3cd1-107">Add data using the Data Explorer</span></span>

1. <span data-ttu-id="c3cd1-108">前のモジュールから続けている場合は、Azure portal ウィンドウで **[データ エクスプローラー]** をクリックして、**[全画面表示で開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-108">If you're continuing from the previous module, click **Data Explorer** in the Azure portal window, and then click **Open Full Screen**.</span></span>

    <span data-ttu-id="c3cd1-109">それ以外の場合は、[Azure portal](https://portal.azure.com/) にサインインし、**[すべてのサービス]** > **[データベース]** > **[Azure Cosmos DB]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-109">Otherwise, sign in to the [Azure portal](https://portal.azure.com/), click **All services** > **Databases** > **Azure Cosmos DB**.</span></span> <span data-ttu-id="c3cd1-110">次に、ご自分のアカウントを選択し、**[データ エクスプローラー]**、**[全画面表示で開く]** の順にクリックします</span><span class="sxs-lookup"><span data-stu-id="c3cd1-110">Then select your account, click **Data Explorer**, and then click **Open Full Screen**</span></span>
 
   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media-draft/2-add-data/azure-cosmosdb-data-explorer-full-screen.png)

2. <span data-ttu-id="c3cd1-112">**[全画面表示で開く]** ボックスで、**[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-112">In the **Open Full Screen** box, click **Open**.</span></span>

    <span data-ttu-id="c3cd1-113">Web ブラウザーでは、新しいデータ エクスプローラーが全画面表示され、ご使用のデータベースを操作するために多くの領域と専用の環境が提供されます。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-113">The web browser displays the new full screen Data Explorer, which gives you more space and a dedicated environment for working with your database.</span></span>

3. <span data-ttu-id="c3cd1-114">新しい JSON ドキュメントを作成するには、**[新しいドキュメント]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-114">To create a new json document, click **New Document**.</span></span>

   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media-draft/2-add-data/azure-cosmosdb-data-explorer-new-document.png)

4. <span data-ttu-id="c3cd1-116">次の構造でコレクションにドキュメントを追加します。次のコードを **[ドキュメント]** タブにコピーして貼り付けるだけです。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-116">Now add a document to the collection with the following structure, just copy and paste the following code into the **Documents** tab.</span></span>

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

5. <span data-ttu-id="c3cd1-117">JSON を **[ドキュメント]** タブに追加したら、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-117">Once you've added the json to the **Documents** tab, click **Save**.</span></span>

    ![JSON データをコピーし、Azure portal のデータ エクスプローラーで [保存] をクリックします](../media-draft/2-add-data/azure-cosmosdb-data-explorer-save-document.png)

6. <span data-ttu-id="c3cd1-119">次の JSON オブジェクトをデータ エクスプローラーにコピーし、**[保存]** をクリックすることによって、ドキュメントをもう 1 つ作成し、保存します。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-119">Create and save one more document by copying the following json object into Data Explorer and clicking **Save**.</span></span>

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

7. <span data-ttu-id="c3cd1-120">左側のメニューで **[ドキュメント]** をクリックして、ドキュメントが保存されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-120">Confirm the documents have been saved by clicking **Documents** on the left-hand menu.</span></span> 

    <span data-ttu-id="c3cd1-121">データ エクスプローラーには、**[ドキュメント]** タブにドキュメントが 2 つ表示されます。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-121">Data Explorer displays the two documents in the **Documents** tab.</span></span>

## <a name="summary"></a><span data-ttu-id="c3cd1-122">まとめ</span><span class="sxs-lookup"><span data-stu-id="c3cd1-122">Summary</span></span>

<span data-ttu-id="c3cd1-123">このモジュールでは、データ エクスプローラーを使用することによってご使用のデータベースに、ドキュメントを 2 つ追加しました。それぞれ製品カタログに製品を 1 つ表しています。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-123">In this module you added two documents, each representing a product in your product catalog, to your database by using the Data Explorer.</span></span> <span data-ttu-id="c3cd1-124">データ エクスプローラーは、ドキュメントの作成、ドキュメントの変更、および Azure Cosmos DB の使用を開始するのに適しています。</span><span class="sxs-lookup"><span data-stu-id="c3cd1-124">The Data Explorer is a good way to create documents, modify documents, and get started with Azure Cosmos DB.</span></span>  
