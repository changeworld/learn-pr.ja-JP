<span data-ttu-id="918fb-101">クエリの対象としてデータベースに追加した 2 つのドキュメントを使用して、クエリの基本を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="918fb-101">Using the two documents you added to the database as the target of our queries, lets walk through some query basics.</span></span> <span data-ttu-id="918fb-102">Azure Cosmos DB では、SQL Server と同じように SQL クエリを使用して、クエリ操作が実行されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-102">Azure Cosmos DB uses SQL queries, just like SQL Server, to perform query operations.</span></span> <span data-ttu-id="918fb-103">すべてのプロパティには既定で自動的にインデックスが付けられるので、データベース内のすべてのデータをクエリにすぐに使用できます。</span><span class="sxs-lookup"><span data-stu-id="918fb-103">All properties are automatically indexed by default, so all data in the database is instantly available to query.</span></span>

## <a name="sql-query-basics"></a><span data-ttu-id="918fb-104">SQL クエリの基本</span><span class="sxs-lookup"><span data-stu-id="918fb-104">SQL query basics</span></span>
<span data-ttu-id="918fb-105">すべての SQL クエリは、SELECT 句と、省略可能な FROM 句および WHERE 句で構成されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-105">Every SQL query consists of a SELECT clause and optional FROM and WHERE clauses.</span></span> <span data-ttu-id="918fb-106">さらに、ORDER BY や JOIN などの他の句を追加して、必要な情報を取得できます。</span><span class="sxs-lookup"><span data-stu-id="918fb-106">In addition, you can add other clauses like ORDER BY and JOIN to get the info you need.</span></span>

<span data-ttu-id="918fb-107">SQL クエリの形式は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="918fb-107">A SQL query has the following format.</span></span>

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a><span data-ttu-id="918fb-108">SELECT 句</span><span class="sxs-lookup"><span data-stu-id="918fb-108">SELECT clause</span></span>

<span data-ttu-id="918fb-109">SELECT 句では、クエリの実行時に生成される値の種類を決定します。</span><span class="sxs-lookup"><span data-stu-id="918fb-109">The SELECT clause determines the type of values that will be produced when the query is executed.</span></span> <span data-ttu-id="918fb-110">値 `SELECT *` は、JSON ドキュメント全体が返されることを示します。</span><span class="sxs-lookup"><span data-stu-id="918fb-110">A value of `SELECT *` indicates that the entire JSON document is returned.</span></span>

<span data-ttu-id="918fb-111">**クエリ**</span><span class="sxs-lookup"><span data-stu-id="918fb-111">**Query**</span></span>
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="918fb-112">**戻り値**</span><span class="sxs-lookup"><span data-stu-id="918fb-112">**Returns**</span></span>
```
[
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
        },
        "_rid": "glAZAJFm0VsBAAAAAAAAAA==",
        "_self": "dbs/glAZAA==/colls/glAZAJFm0Vs=/docs/glAZAJFm0VsBAAAAAAAAAA==/",
        "_etag": "\"00006000-0000-0000-0000-5b71be760000\"",
        "_attachments": "attachments/",
        "_ts": 1534180982
    }
]
```

<span data-ttu-id="918fb-113">または、SELECT 句にプロパティのリストを含めることによって、特定のプロパティのみを含むように出力を制限できます。</span><span class="sxs-lookup"><span data-stu-id="918fb-113">Or, you can limit the output to only include certain properties by including a list of properties in the SELECT clause.</span></span> <span data-ttu-id="918fb-114">次のクエリでは、ID、製造元、および製品の説明のみが返されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-114">In the following query, only the id, manufacturer, and product description are returned.</span></span>

<span data-ttu-id="918fb-115">**クエリ**</span><span class="sxs-lookup"><span data-stu-id="918fb-115">**Query**</span></span>
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

<span data-ttu-id="918fb-116">**戻り値**</span><span class="sxs-lookup"><span data-stu-id="918fb-116">**Returns**</span></span>
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a><span data-ttu-id="918fb-117">FROM 句</span><span class="sxs-lookup"><span data-stu-id="918fb-117">FROM clause</span></span>

<span data-ttu-id="918fb-118">FROM 句では、クエリの動作の対象となるデータ ソースを指定します。</span><span class="sxs-lookup"><span data-stu-id="918fb-118">The FROM clause specifies the data source upon which the query operates.</span></span> <span data-ttu-id="918fb-119">コレクション全体をクエリのソースにすることも、コレクションのサブセットを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="918fb-119">You can make the whole collection the source of the query, or you can specify a subset of the collection instead.</span></span> <span data-ttu-id="918fb-120">ソースがクエリの後半でフィルター処理またはプロジェクションされる場合を除いて、FROM 句はオプションです。</span><span class="sxs-lookup"><span data-stu-id="918fb-120">The FROM clause is optional unless the source is filtered or projected later in the query.</span></span>

<span data-ttu-id="918fb-121">`SELECT * FROM Products` のようなクエリは、Products コレクション全体がクエリの列挙の対象となるソースであることを示します。</span><span class="sxs-lookup"><span data-stu-id="918fb-121">A query such as `SELECT * FROM Products` indicates that the entire Products collection is the source over which to enumerate the query.</span></span>

<span data-ttu-id="918fb-122">コレクションは、`SELECT p.id FROM Products AS p` または単に `SELECT p.id FROM Products p` のように、エイリアスにすることができます。`p` は `Products` と同じことです。</span><span class="sxs-lookup"><span data-stu-id="918fb-122">A collection can be aliased, such as `SELECT p.id FROM Products AS p` or simply `SELECT p.id FROM Products p`, where `p` is the equivalent of `Products`.</span></span> <span data-ttu-id="918fb-123">`AS` は識別子をエイリアスにするためのオプションのキーワードです。</span><span class="sxs-lookup"><span data-stu-id="918fb-123">`AS` is an optional keyword to alias the identifier.</span></span>

<span data-ttu-id="918fb-124">エイリアスに設定されると、元のソースをバインドすることはできなくなります。</span><span class="sxs-lookup"><span data-stu-id="918fb-124">Once aliased, the original source cannot be bound.</span></span> <span data-ttu-id="918fb-125">たとえば、`SELECT Products.id FROM Products p` は、識別子 "Products" をそれ以上解決できないため、構文的に正しくありません。</span><span class="sxs-lookup"><span data-stu-id="918fb-125">For example, `SELECT Products.id FROM Products p` is syntactically invalid since the identifier "Products" cannot be resolved anymore.</span></span>

<span data-ttu-id="918fb-126">参照先になる必要があるすべてのプロパティは、完全修飾にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="918fb-126">All properties that need to be referenced must be fully qualified.</span></span> <span data-ttu-id="918fb-127">準拠する厳密なスキーマが存在しない場合、これが強制されることによって曖昧なバインドが回避されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-127">In the absence of strict schema adherence, this is enforced to avoid any ambiguous bindings.</span></span> <span data-ttu-id="918fb-128">したがって、プロパティ `id` がバインドされていないため、`SELECT id FROM Products p` の構文は正しくありません。</span><span class="sxs-lookup"><span data-stu-id="918fb-128">Therefore, `SELECT id FROM Products p` is syntactically invalid since the property `id` is not bound.</span></span>

### <a name="subdocuments-in-a-from-clause"></a><span data-ttu-id="918fb-129">FROM 句内のサブドキュメント</span><span class="sxs-lookup"><span data-stu-id="918fb-129">Subdocuments in a FROM clause</span></span>
<span data-ttu-id="918fb-130">ソースはさらに小さいサブセットに限定することもできます。</span><span class="sxs-lookup"><span data-stu-id="918fb-130">The source can also be reduced to a smaller subset.</span></span> <span data-ttu-id="918fb-131">たとえば、各ドキュメントのサブツリーだけを列挙するには、以下の例のように、サブルートをソースにすることができます。</span><span class="sxs-lookup"><span data-stu-id="918fb-131">For instance, to enumerate only a subtree in each document, the subroot could then become the source, as shown in the following example.</span></span>

<span data-ttu-id="918fb-132">**クエリ**</span><span class="sxs-lookup"><span data-stu-id="918fb-132">**Query**</span></span>
```
SELECT * 
FROM Products.shipping
```

<span data-ttu-id="918fb-133">**結果**</span><span class="sxs-lookup"><span data-stu-id="918fb-133">**Results**</span></span>  

```
[
{
    "weight": 1,
    "dimensions": {
        "width": 6,
        "height": 8,
        "depth": 1
    }
},
{
    "weight": 1,
    "dimensions": {
        "width": 6,
        "height": 8,
        "depth": 1
    }
}
]
```

<span data-ttu-id="918fb-134">上記の例では配列をソースとして使用していますが、以下の例に示すように、オブジェクトをソースとして使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="918fb-134">While the above example used an array as the source, an object could also be used as the source, which is what's shown in the following example.</span></span> <span data-ttu-id="918fb-135">ソースで検索できる有効な JSON 値 (未定義でないもの) はすべて、クエリ結果に含められると想定されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-135">Any valid JSON value (that's not undefined) that can be found in the source is considered for inclusion in the result of the query.</span></span> <span data-ttu-id="918fb-136">製品に `shipping.weight` 値がない場合は、クエリ結果から除外されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-136">If some products don’t have an `shipping.weight` value, they are excluded in the query result.</span></span>

<span data-ttu-id="918fb-137">**クエリ**</span><span class="sxs-lookup"><span data-stu-id="918fb-137">**Query**</span></span>

    SELECT * 
    FROM Products.shipping.weight

<span data-ttu-id="918fb-138">**結果**</span><span class="sxs-lookup"><span data-stu-id="918fb-138">**Results**</span></span>

    [
        1,
        2
    ]

## <a name="where-clause"></a><span data-ttu-id="918fb-139">WHERE 句</span><span class="sxs-lookup"><span data-stu-id="918fb-139">WHERE clause</span></span>
<span data-ttu-id="918fb-140">WHERE 句は、ソースによって提供される JSON ドキュメントを、結果に含めるために満たす必要がある条件を指定します。</span><span class="sxs-lookup"><span data-stu-id="918fb-140">The WHERE clause specifies the conditions that the JSON documents provided by the source must satisfy in order to be included as part of the result.</span></span> <span data-ttu-id="918fb-141">結果の対象となるには、指定された条件についてすべての JSON ドキュメントが true と評価される必要があります。</span><span class="sxs-lookup"><span data-stu-id="918fb-141">Any JSON document must evaluate the specified conditions to true to be considered for the result.</span></span> <span data-ttu-id="918fb-142">WHERE 句は省略可能です。</span><span class="sxs-lookup"><span data-stu-id="918fb-142">The WHERE clause is optional.</span></span>

<span data-ttu-id="918fb-143">次のクエリでは、値が 1 である ID を含むドキュメントを要求しています。</span><span class="sxs-lookup"><span data-stu-id="918fb-143">The following query requests documents that contain an id whose value is 1.</span></span>

<span data-ttu-id="918fb-144">**クエリ**</span><span class="sxs-lookup"><span data-stu-id="918fb-144">**Query**</span></span>

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

<span data-ttu-id="918fb-145">**結果**</span><span class="sxs-lookup"><span data-stu-id="918fb-145">**Results**</span></span>

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a><span data-ttu-id="918fb-146">ORDER BY 句</span><span class="sxs-lookup"><span data-stu-id="918fb-146">ORDER BY clause</span></span>

<span data-ttu-id="918fb-147">ORDER BY 句を使用すると、結果を昇順または降順に並べ替えることができます。</span><span class="sxs-lookup"><span data-stu-id="918fb-147">The ORDER BY clause enables you to order the results in ascending or descending order.</span></span>

<span data-ttu-id="918fb-148">次の ORDER BY クエリでは、すべての製品の価格、説明、および製品 ID が、価格の昇順に並べ替えられて返されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-148">The following ORDER BY query returns the price, description, and product ID for all products, ordered by price, in ascending order.</span></span>

<span data-ttu-id="918fb-149">**クエリ**</span><span class="sxs-lookup"><span data-stu-id="918fb-149">**Query**</span></span>

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

<span data-ttu-id="918fb-150">**結果**</span><span class="sxs-lookup"><span data-stu-id="918fb-150">**Results**</span></span>

```
[
    {
        "price": "14.99",
        "description": "Quick dry crew neck t-shirt",
        "productId": "33218896"
    },
    {
        "price": "49.99",
        "description": "Black wool pea-coat",
        "productId": "33218897"
    }
]
```

## <a name="join-clause"></a><span data-ttu-id="918fb-151">JOIN 句</span><span class="sxs-lookup"><span data-stu-id="918fb-151">JOIN clause</span></span>

<span data-ttu-id="918fb-152">JOIN 句を使用すると、ドキュメントおよびドキュメント サブルートとの内部結合を実行できます。</span><span class="sxs-lookup"><span data-stu-id="918fb-152">The JOIN clause enables you to perform inner joins with the document and the document sub-roots.</span></span> <span data-ttu-id="918fb-153">たとえば、製品のデータベースで、ドキュメントと発送データを結合することができます。</span><span class="sxs-lookup"><span data-stu-id="918fb-153">So in the product database, for example, you can join the documents with the shipping data.</span></span>  

<span data-ttu-id="918fb-154">次のクエリでは、発送方法のある各製品の製品 ID が返されます。</span><span class="sxs-lookup"><span data-stu-id="918fb-154">In the following query, the product IDs are returned for each product that has a shipping method.</span></span> <span data-ttu-id="918fb-155">発送プロパティがない 3 つ目の製品を追加した場合、3 番目の品目は発送プロパティがないために除外されるので結果は同じになります。</span><span class="sxs-lookup"><span data-stu-id="918fb-155">If you added a third product that didn't have a shipping property, the result would be the same as the third item would be excluded for not having a shipping property.</span></span>

<span data-ttu-id="918fb-156">**クエリ**</span><span class="sxs-lookup"><span data-stu-id="918fb-156">**Query**</span></span>

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

<span data-ttu-id="918fb-157">**結果**</span><span class="sxs-lookup"><span data-stu-id="918fb-157">**Results**</span></span>

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a><span data-ttu-id="918fb-158">地理空間クエリ</span><span class="sxs-lookup"><span data-stu-id="918fb-158">Geospatial queries</span></span>

<span data-ttu-id="918fb-159">地理空間クエリを使用すると、GeoJSON ポイントを使用して空間クエリを実行できます。</span><span class="sxs-lookup"><span data-stu-id="918fb-159">Geospatial queries enable you to perform spatial queries using GeoJSON points.</span></span> <span data-ttu-id="918fb-160">データベース内の座標を使用して、2 つのポイント間の距離を計算し、Point、Polygon、または LineString が別の Point、Polygon、または LineString の中にあるかどうかを判断できます。</span><span class="sxs-lookup"><span data-stu-id="918fb-160">Using the coordinates in the database, you can calculate the distance between two points, determine whether a point, polygon, or linestring is within another point, polygon or linestring.</span></span>

<span data-ttu-id="918fb-161">商品カタログ データの場合、これによりユーザーは自分の場所情報を入力して、探している商品の在庫がある店が半径 50 マイル以内に存在するかどうかを調べることができます。</span><span class="sxs-lookup"><span data-stu-id="918fb-161">For product catalog data this would enable your users to enter their location information and determine whether there were any stores within a 50 mile radius that have the item they're looking for.</span></span> 

## <a name="summary"></a><span data-ttu-id="918fb-162">まとめ</span><span class="sxs-lookup"><span data-stu-id="918fb-162">Summary</span></span>

<span data-ttu-id="918fb-163">Azure Cosmos DB に追加したすべてのデータを、使い慣れた SQL クエリ手法を使用してすばやく照会できると、ユーザーや顧客が格納されているデータを調べて分析情報を得るのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="918fb-163">Being able to quickly query all your data after adding it to Azure Cosmos DB, and using familiar SQL query techniques, can help you and your customers to explore and gain insights into your stored data.</span></span>