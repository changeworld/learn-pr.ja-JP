<span data-ttu-id="2c6b5-101">データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-101">Multiple documents in your database frequently need to be updated at the same time.</span></span> 

<span data-ttu-id="2c6b5-102">この小売りオンライン アプリケーションでは、ユーザーが注文して、クーポン コード、クレジット、または配当 (または同時に 3 つすべて) を使用したい場合に、ユーザーのアカウントに対して所有するオプションをクエリし、ユーザーが使用したことを示すように、そのアカウントに更新を加え、注文の合計を更新し、注文を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-102">For your online retail application, when a user places an order and wants to use a coupon code, a credit, or a dividend (or all three at once), you need to query their account for those options, make updates to their account indicating they used them, update the order total, and process the order.</span></span>

<span data-ttu-id="2c6b5-103">これらすべてのアクションが、1 つのトランザクション内で同時に発生する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-103">All of these actions need to happen at the same time, within a single transaction.</span></span> <span data-ttu-id="2c6b5-104">ユーザーが注文をキャンセルすることを選んだ場合は、クーポン コード、クレジット、配当を次の購入で利用できるように、変更をロール バックし、アカウントの情報を変更しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-104">If the user chooses to cancel the order, you want to roll back the changes and not modify their account information, so that their coupon codes, credits, and dividends are available for their next purchase.</span></span>

<span data-ttu-id="2c6b5-105">Azure Cosmos DB でこれらのトランザクションを実行するには、ストアド プロシージャとユーザー定義関数 (UDF) を使用します。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-105">The way to perform these transactions in Azure Cosmos DB is by using stored procedures and user-defined functions (UDFs).</span></span> <span data-ttu-id="2c6b5-106">ストアド プロシージャは、サーバー上で実行されるときに、ACID (原子性、一貫性、分離性、持続性) トランザクションを確実にする唯一の方法であり、そのため、サーバー側プログラミングと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-106">Stored procedures are the only way to ensure ACID (Atomicity, Consistency, Isolation, Durability) transactions because they are run on the server, and are thus referred to as server-side programming.</span></span> <span data-ttu-id="2c6b5-107">また、UDF は、クエリ内の値またはドキュメントで計算ロジックを実行するために、サーバー上にも保存され、クエリ中に使用されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-107">UDFs are also stored on the server and are used during queries to perform computational logic on values or documents within the query.</span></span> 

<span data-ttu-id="2c6b5-108">このモジュールでは、ストアド プロシージャと UDF について学習し、ポータルでその一部を実行します。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-108">In this module, you'll learn about stored procedures and UDFs, and then run some in the portal.</span></span>

## <a name="stored-procedure-basics"></a><span data-ttu-id="2c6b5-109">ストアド プロシージャの基本</span><span class="sxs-lookup"><span data-stu-id="2c6b5-109">Stored procedure basics</span></span>

<span data-ttu-id="2c6b5-110">ストアド プロシージャでは、ドキュメントやプロパティに対する複雑なトランザクションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-110">Stored procedures perform complex transactions on documents and properties.</span></span> <span data-ttu-id="2c6b5-111">ストアド プロシージャは JavaScript で記述され、Azure Cosmos DB 上のコレクションに保存されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-111">Stored procedures are written in JavaScript and are stored in a collection on Azure Cosmos DB.</span></span> <span data-ttu-id="2c6b5-112">データベース エンジン上およびデータに近い場所でストアド プロシージャを実行することにより、クライアント側プログラミングよりパフォーマンスを向上させることができます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-112">By performing the stored procedures on the database engine and close to the data, you can improve performance over client-side programming.</span></span>

<span data-ttu-id="2c6b5-113">ストアド プロシージャは、Azure Cosmos DB 内で原子性トランザクションを実現させる唯一の方法です。クライアント側の SDK はトランザクションをサポートしません。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-113">Stored procedures are the only way to achieve atomic transactions within Azure Cosmos DB; the client-side SDKs do not support transactions.</span></span>

<span data-ttu-id="2c6b5-114">また、個別のトランザクションを作成する必要性が減るため、ストアド プロシージャでバッチ操作を実行することもお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-114">Performing batch operations in stored procedures is also recommended because of the reduced need to create separate transactions.</span></span>

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## <a name="stored-procedure-example"></a><span data-ttu-id="2c6b5-115">ストアド プロシージャの例</span><span class="sxs-lookup"><span data-stu-id="2c6b5-115">Stored procedure example</span></span>

<span data-ttu-id="2c6b5-116">次のサンプルは、現在のコンテキストを取得して "Hello, World" と表示する応答を送信する、簡単な HelloWorld ストアド プロシージャです。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-116">The following sample is a simple HelloWorld stored procedure that gets the current context and sends a response that displays "Hello, World".</span></span> <span data-ttu-id="2c6b5-117">ストアド プロシージャには、Azure Cosmos DB ドキュメントのように、ID 値があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-117">Note that the stored procedure has an ID value, just like Azure Cosmos DB documents.</span></span>

```javascript
function helloWorld() {
    var context = getContext();
    var response = context.getResponse();

    response.setBody("Hello, World");
}
```

## <a name="user-defined-function-basics"></a><span data-ttu-id="2c6b5-118">ユーザー定義関数の基本</span><span class="sxs-lookup"><span data-stu-id="2c6b5-118">User-defined function basics</span></span>

<span data-ttu-id="2c6b5-119">UDF は、Azure Cosmos DB の SQL クエリ言語の文法を拡張し、プロパティやドキュメントの計算のようなカスタム ビジネス ロジックを実装するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-119">UDFs are used to extend the Azure Cosmos DB SQL query language grammar and implement custom business logic, such as calculations on properties and documents.</span></span> <span data-ttu-id="2c6b5-120">UDF はクエリ内からのみ呼び出すことができ、ストアド プロシージャとは異なります。UDF はコンテキスト オブジェクトへのアクセス権がないため、ドキュメントの読み取りや書き込みを行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-120">UDFs can be called only from inside queries and, unlike stored procedures, they do not have access to the context object, so they cannot read or write documents.</span></span>

<span data-ttu-id="2c6b5-121">オンライン コマースのシナリオでは、UDF は注文の合計に適用する消費税、または製品や注文に適用する割引率を決定するために使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-121">In an online commerce scenario, a UDF could be used to determine the sales tax to apply to an order total or a percentage discount to apply to products or orders.</span></span>

## <a name="user-defined-function-example"></a><span data-ttu-id="2c6b5-122">ユーザー定義関数の例</span><span class="sxs-lookup"><span data-stu-id="2c6b5-122">User-defined function example</span></span>

<span data-ttu-id="2c6b5-123">次の例では、架空の会社の製品に対する税金を製品コストに基づいて算出する UDF を作成します。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-123">The following sample creates a UDF to calculate tax on a product in a fictitious company based the product cost:</span></span>

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

## <a name="create-a-stored-procedure-in-the-portal"></a><span data-ttu-id="2c6b5-124">ポータルでストアド プロシージャを作成する</span><span class="sxs-lookup"><span data-stu-id="2c6b5-124">Create a stored procedure in the portal</span></span>

<span data-ttu-id="2c6b5-125">ポータルで新しいストアド プロシージャを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-125">Let's create a new stored procedure in the portal.</span></span> <span data-ttu-id="2c6b5-126">ポータルでは、コレクション内の最初の項目を取得するシンプルなストアド プロシージャが自動的に生成されるため、まずこのストアド プロシージャを実行します。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-126">The portal automatically populates a simple stored procedure that retrieves the first item in the collection, so we'll run this stored procedure first.</span></span>

1. <span data-ttu-id="2c6b5-127">データ エクスプローラーで、**[新しいストアド プロシージャ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-127">In the Data Explorer, click **New Stored Procedure**.</span></span>

    <span data-ttu-id="2c6b5-128">データ エクスプローラーでは、サンプルのストアド プロシージャを含む新しいタブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-128">Data Explorer displays a new tab with a sample stored procedure.</span></span>

2. <span data-ttu-id="2c6b5-129">**[Stored Procedure Id]\(ストアド プロシージャの ID\)** ボックスに、「*sample*」という名前を入力して、**[保存]**、**[実行]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-129">In the **Stored Procedure Id** box, enter the name *sample*, click **Save**, and then click **Execute**.</span></span>


3. <span data-ttu-id="2c6b5-130">**[入力パラメーター]** ボックスにパーティション キーの名前「*33218896*」を入力し、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-130">In the **Input parameters** box, type the name of a partition key, *33218896*, and then click **Execute**.</span></span> <span data-ttu-id="2c6b5-131">ストアド プロシージャは 1 つのパーティション内で機能することに注意してください。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-131">Note that stored procedures work within a single partition.</span></span>

    ![ポータルでストアド プロシージャを実行する](../media/6-stored-procedure.gif)

    <span data-ttu-id="2c6b5-133">**[結果]** ウィンドウには、コレクションの最初のドキュメントからのフィードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-133">The **Result** pane displays the feed from the first document in the collection.</span></span>

## <a name="create-a-stored-procedure-that-creates-documents"></a><span data-ttu-id="2c6b5-134">ドキュメントを作成するストアド プロシージャを作成する</span><span class="sxs-lookup"><span data-stu-id="2c6b5-134">Create a stored procedure that creates documents</span></span>

<span data-ttu-id="2c6b5-135">ドキュメントを作成するストアド プロシージャを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-135">Now, let's create a stored procedure that creates documents.</span></span>

1. <span data-ttu-id="2c6b5-136">データ エクスプローラーで、**[新しいストアド プロシージャ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-136">In the Data Explorer, click **New Stored Procedure**.</span></span> <span data-ttu-id="2c6b5-137">このストアド プロシージャに *createMyDocument* という名前を付け、次のコードをコピーして **[ストアド プロシージャ本文]** ボックスに貼り付け、**[保存]** をクリックして **[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-137">Name this stored procedure *createMyDocument*, copy and paste the following code into the **Stored Procedure Body** box, click **Save**, and then click **Execute**.</span></span>

    ```javascript
    function createMyDocument() {
        var context = getContext();
        var collection = context.getCollection();

        var doc = {
            "id": "3",
            "productId": "33218898",
            "description": "Contoso microfleece zip-up jacket",
            "price": "44.99"
        };

        var accepted = collection.createDocument(collection.getSelfLink(),
            doc,
            function (err, documentCreated) {
                if (err) throw new Error('Error' + err.message);
                context.getResponse().setBody(documentCreated)
            });
        if (!accepted) return;
    }
    ```

2. <span data-ttu-id="2c6b5-138">[入力パラメーター] ボックスにパーティション キーの値「*33218898*」を入力し、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-138">In the Input parameters box, enter a Partition Key Value of *33218898*, and then click **Execute**.</span></span>

    <span data-ttu-id="2c6b5-139">データ エクスプローラーには、新しく作成されたドキュメントが [結果] 領域に表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-139">Data Explorer displays the newly created document in the Result area.</span></span>

    <span data-ttu-id="2c6b5-140">[ドキュメント] タブに戻って **[更新]** ボタンをクリッすると、新しいドキュメントが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-140">You can go back to the Documents tab, click the **Refresh** button and see the new document.</span></span> 

## <a name="create-a-user-defined-function"></a><span data-ttu-id="2c6b5-141">ユーザー定義関数を作成する</span><span class="sxs-lookup"><span data-stu-id="2c6b5-141">Create a user-defined function</span></span>

<span data-ttu-id="2c6b5-142">次に、データ エクスプローラーで UDF を作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-142">Now, let's create a UDF in Data Explorer.</span></span>

<span data-ttu-id="2c6b5-143">データ エクスプローラーで、**[New UDF]\(新しい UDF\)** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-143">In the Data Explorer, click **New UDF**.</span></span> <span data-ttu-id="2c6b5-144">**[New UDF]\(新しい UDF\)** を表示するには、**[新しいストアド プロシージャ]** の横の下向き矢印をクリックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-144">You may need to click the down arrow next to **New Stored Prodedure** to see **New UDF**.</span></span> <span data-ttu-id="2c6b5-145">次のコードをウィンドウにコピーし、UDF に *producttax* という名前を付けて、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-145">Copy the following code into the window, name the UDF *producttax*, and then click **Save**.</span></span>

```javascript
function producttax(price) {
    if (price == undefined) 
        throw 'no input';

    var amount = parseFloat(price);

    if (amount < 1000) 
        return amount * 0.1;
    else if (amount < 10000) 
        return amount * 0.2;
    else
        return amount * 0.4;
}
```

<span data-ttu-id="2c6b5-146">UDF を定義したら、**[クエリ 1]** タブに移動し、次のクエリをコピーしてクエリ領域に貼り付けて、UDF を実行します。</span><span class="sxs-lookup"><span data-stu-id="2c6b5-146">Once you have defined the UDF, go to the **Query 1** tab and copy and paste the following query into the query area to run the UDF.</span></span>

```sql
SELECT c.id, c.productId, c.price, udf.producttax(c.price) AS producttax FROM c
```
