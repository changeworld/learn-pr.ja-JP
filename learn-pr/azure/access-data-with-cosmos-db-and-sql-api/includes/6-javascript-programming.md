データベース内の複数のドキュメントを同時に更新する必要があることがよくあります。 

この小売りオンライン アプリケーションでは、ユーザーが注文して、クーポン コード、クレジット、または配当 (または同時に 3 つすべて) を使用したい場合に、ユーザーのアカウントに対して所有するオプションをクエリし、ユーザーが使用したことを示すように、そのアカウントに更新を加え、注文の合計を更新し、注文を処理する必要があります。

これらすべてのアクションが、1 つのトランザクション内で同時に発生する必要があります。 ユーザーが注文をキャンセルすることを選んだ場合は、クーポン コード、クレジット、配当を次の購入で利用できるように、変更をロール バックし、アカウントの情報を変更しないようにする必要があります。

Azure Cosmos DB でこれらのトランザクションを実行するには、ストアド プロシージャとユーザー定義関数 (UDF) を使用します。 ストアド プロシージャは、サーバー上で実行されるときに、ACID (原子性、一貫性、分離性、持続性) トランザクションを確実にする唯一の方法であり、そのため、サーバー側プログラミングと呼ばれます。 また、UDF は、クエリ内の値またはドキュメントで計算ロジックを実行するために、サーバー上にも保存され、クエリ中に使用されます。 

このモジュールでは、ストアド プロシージャと UDF について学習し、ポータルでその一部を実行します。

## <a name="stored-procedure-basics"></a>ストアド プロシージャの基本

ストアド プロシージャでは、ドキュメントやプロパティに対する複雑なトランザクションが実行されます。 ストアド プロシージャは JavaScript で記述され、Azure Cosmos DB 上のコレクションに保存されます。 データベース エンジン上およびデータに近い場所でストアド プロシージャを実行することにより、クライアント側プログラミングよりパフォーマンスを向上させることができます。

ストアド プロシージャは、Azure Cosmos DB 内で原子性トランザクションを実現させる唯一の方法です。クライアント側の SDK はトランザクションをサポートしません。

また、個別のトランザクションを作成する必要性が減るため、ストアド プロシージャでバッチ操作を実行することもお勧めします。

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## <a name="stored-procedure-example"></a>ストアド プロシージャの例

次のサンプルは、現在のコンテキストを取得して "Hello, World" と表示する応答を送信する、簡単な HelloWorld ストアド プロシージャです。 ストアド プロシージャには、Azure Cosmos DB ドキュメントのように、ID 値があることに注意してください。

```javascript
function helloWorld() {
    var context = getContext();
    var response = context.getResponse();

    response.setBody("Hello, World");
}
```

## <a name="user-defined-function-basics"></a>ユーザー定義関数の基本

UDF は、Azure Cosmos DB の SQL クエリ言語の文法を拡張し、プロパティやドキュメントの計算のようなカスタム ビジネス ロジックを実装するために使用されます。 UDF はクエリ内からのみ呼び出すことができ、ストアド プロシージャとは異なります。UDF はコンテキスト オブジェクトへのアクセス権がないため、ドキュメントの読み取りや書き込みを行うことはできません。

オンライン コマースのシナリオでは、UDF は注文の合計に適用する消費税、または製品や注文に適用する割引率を決定するために使用することができます。

## <a name="user-defined-function-example"></a>ユーザー定義関数の例

次の例では、架空の会社の製品に対する税金を製品コストに基づいて算出する UDF を作成します。

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

## <a name="create-a-stored-procedure-in-the-portal"></a>ポータルでストアド プロシージャを作成する

ポータルで新しいストアド プロシージャを作成しましょう。 ポータルでは、コレクション内の最初の項目を取得するシンプルなストアド プロシージャが自動的に生成されるため、まずこのストアド プロシージャを実行します。

1. データ エクスプローラーで、**[新しいストアド プロシージャ]** をクリックします。

    データ エクスプローラーでは、サンプルのストアド プロシージャを含む新しいタブが表示されます。

2. **[Stored Procedure Id]\(ストアド プロシージャの ID\)** ボックスに、「*sample*」という名前を入力して、**[保存]**、**[実行]** の順にクリックします。


3. **[入力パラメーター]** ボックスにパーティション キーの名前「*33218896*」を入力し、**[実行]** をクリックします。 ストアド プロシージャは 1 つのパーティション内で機能することに注意してください。

    ![ポータルでストアド プロシージャを実行する](../media/6-stored-procedure.gif)

    **[結果]** ウィンドウには、コレクションの最初のドキュメントからのフィードが表示されます。

## <a name="create-a-stored-procedure-that-creates-documents"></a>ドキュメントを作成するストアド プロシージャを作成する

ドキュメントを作成するストアド プロシージャを作成しましょう。

1. データ エクスプローラーで、**[新しいストアド プロシージャ]** をクリックします。 このストアド プロシージャに *createMyDocument* という名前を付け、次のコードをコピーして **[ストアド プロシージャ本文]** ボックスに貼り付け、**[保存]** をクリックして **[実行]** をクリックします。

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

2. [入力パラメーター] ボックスにパーティション キーの値「*33218898*」を入力し、**[実行]** をクリックします。

    データ エクスプローラーには、新しく作成されたドキュメントが [結果] 領域に表示されます。

    [ドキュメント] タブに戻って **[更新]** ボタンをクリッすると、新しいドキュメントが表示されます。 

## <a name="create-a-user-defined-function"></a>ユーザー定義関数を作成する

次に、データ エクスプローラーで UDF を作成しましょう。

データ エクスプローラーで、**[New UDF]\(新しい UDF\)** をクリックします。 **[New UDF]\(新しい UDF\)** を表示するには、**[新しいストアド プロシージャ]** の横の下向き矢印をクリックする必要があります。 次のコードをウィンドウにコピーし、UDF に *producttax* という名前を付けて、**[保存]** をクリックします。

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

UDF を定義したら、**[クエリ 1]** タブに移動し、次のクエリをコピーしてクエリ領域に貼り付けて、UDF を実行します。

```sql
SELECT c.id, c.productId, c.price, udf.producttax(c.price) AS producttax FROM c
```
