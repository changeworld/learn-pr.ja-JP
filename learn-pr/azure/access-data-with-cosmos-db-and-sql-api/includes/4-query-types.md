クエリの対象としてデータベースに追加した 2 つのドキュメントを使用して、クエリの基本を見ていきましょう。 Azure Cosmos DB では、SQL Server と同じように SQL クエリを使用して、クエリ操作が実行されます。 すべてのプロパティには既定で自動的にインデックスが付けられるので、データベース内のすべてのデータをクエリにすぐに使用できます。

## <a name="sql-query-basics"></a>SQL クエリの基本
すべての SQL クエリは、SELECT 句と、省略可能な FROM 句および WHERE 句で構成されます。 さらに、ORDER BY や JOIN などの他の句を追加して、必要な情報を取得できます。

SAS クエリの形式は次のとおりです。

    SELECT <select_list>
    [FROM <optional_from_specification>]
    [WHERE <optional_filter_condition>]
    [ORDER BY <optional_sort_specification>]
    [JOIN <optional_join_specification>]

## <a name="select-clause"></a>SELECT 句

SELECT 句では、クエリの実行時に生成される値の種類を決定します。 値 `SELECT *` は、JSON ドキュメント全体が返されることを示します。

**クエリ**
```
SELECT *
FROM Products p
WHERE p.id ="1"
```

**戻り値**
```
[
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "price": "14.99",
        "shipping": {
            "weight": 1,
            "dimensions": {
                "width": 6,
                "height": 8,
                "depth": 1
            }
        },
        "_rid": "iAEeANrzNAAJAAAAAAAAAA==",
        "_self": "dbs/iAEeAA==/colls/iAEeANrzNAA=/docs/iAEeANrzNAAJAAAAAAAAAA==/",
        "_etag": "\"00003a02-0000-0000-0000-5b9208440000\"",
        "_attachments": "attachments/",
        "_ts": 1536297028
    }
]
```

または、SELECT 句にプロパティのリストを含めることによって、特定のプロパティのみを含むように出力を制限できます。 次のクエリでは、ID、製造元、および製品の説明のみが返されます。

**クエリ**
```
SELECT 
    p.id, 
    p.manufacturer, 
    p.description
FROM Products p
WHERE p.id ="1"
```

**戻り値**
```
[
    {
        "id": "1",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt"
    }
]
```

## <a name="from-clause"></a>FROM 句

FROM 句では、クエリの動作の対象となるデータ ソースを指定します。 コレクション全体をクエリのソースにすることも、コレクションのサブセットを指定することもできます。 FROM 句は省略可能です (ソースがクエリの後半でフィルター処理またはプロジェクションされる場合を除く)。

`SELECT * FROM Products` のようなクエリは、Products コレクション全体がクエリの列挙の対象となるソースであることを示します。

コレクションは、`SELECT p.id FROM Products AS p` または単に `SELECT p.id FROM Products p` のように、エイリアスにすることができます。`p` は `Products` と同じことです。 `AS` は識別子をエイリアスにするためのオプションのキーワードです。

エイリアス化すると、元のソースをバインドすることはできなくなります。 たとえば、`SELECT Products.id FROM Products p` は、識別子 "Products" をそれ以上解決できないため、構文的に無効です。

参照する必要があるすべてのプロパティは、完全修飾する必要があります。 準拠する厳密なスキーマが存在しない場合、これが強制されることによって曖昧なバインドが回避されます。 したがって、`SELECT id FROM Products p` は、プロパティ `id` がバインドされていないため、無効な構文になります。

### <a name="subdocuments-in-a-from-clause"></a>FROM 句内のサブドキュメント
ソースをさらに小さいサブセットに限定することもできます。 たとえば、各ドキュメントのサブツリーだけを列挙する場合、以下の例のようにサブルートをソースにすることができます。

**クエリ**
```
SELECT * 
FROM Products.shipping
```

**結果**  

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
        "weight": 2,
        "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
        }
    }
]
```

上記の例では配列をソースとして使用していますが、以下の例のようにオブジェクトをソースとして使用することもできます。 ソースで検索できる有効なすべての JSON 値 (未定義でないもの) は、クエリ結果に含められることが想定されます。 `shipping.weight` 値を持たない製品は、クエリ結果から除外されます。

**クエリ**

    SELECT * 
    FROM Products.shipping.weight

**結果**

    [
        1,
        2
    ]

## <a name="where-clause"></a>WHERE 句
WHERE 句は、ソースが提供する JSON ドキュメントを結果に含めるために満たす必要がある条件を指定します。 結果の対象となるには、指定された条件についてすべての JSON ドキュメントが **true** と評価される必要があります。 WHERE 句 は省略可能です。

次のクエリは、値が 1 である ID を含むドキュメントを要求しています。

**クエリ**

    SELECT p.description
    FROM Products p 
    WHERE p.id = "1"

**結果**

    [
        {
            "description": "Quick dry crew neck t-shirt"
        }
    ]

## <a name="order-by-clause"></a>ORDER BY 句

ORDER BY 句を使用すると、結果を昇順または降順に並べ替えることができます。

次の ORDER BY クエリでは、すべての製品の価格、説明、および製品 ID が、価格の昇順に並べ替えられて返されます。

**クエリ**

    SELECT p.price, p.description, p.productId
    FROM Products p
    ORDER BY p.price ASC

**結果**

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

## <a name="join-clause"></a>JOIN 句

JOIN 句を使用すると、ドキュメントおよびドキュメント サブルートとの内部結合を実行できます。 たとえば、製品のデータベースで、ドキュメントと配送データを結合することができます。  

次のクエリでは、配送方法のある各製品の製品 ID が返されます。 配送プロパティがない 3 つ目の製品を追加した場合、3 番目の品目は配送プロパティがないために除外されるので結果は同じになります。

**クエリ**

    SELECT p.productId
    FROM Products p
    JOIN p.shipping

**結果**

    [
        {
            "productId": "33218896"
        },
        {
            "productId": "33218897"
        }
    ]

## <a name="geospatial-queries"></a>地理空間クエリ

地理空間クエリを使用すると、GeoJSON ポイントを使用して空間クエリを実行できます。 データベース内の座標を使用して、2 つのポイント間の距離を計算し、Point、Polygon、または LineString が別の Point、Polygon、または LineString の中にあるかどうかを判断できます。

商品カタログ データの場合、これによりユーザーは自分の場所情報を入力して、探している商品の在庫がある店が半径 50 マイル以内に存在するかどうかを調べることができます。 

## <a name="summary"></a>まとめ

Azure Cosmos DB に追加したすべてのデータを、使い慣れた SQL クエリ手法を使用してすばやく照会できると、ユーザーや顧客が格納されているデータを調べて分析情報を得るのに役立ちます。