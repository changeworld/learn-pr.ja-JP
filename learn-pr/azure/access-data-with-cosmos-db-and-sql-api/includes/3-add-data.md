自分の Azure Cosmos DB データベースにデータを追加するのは簡単です。 Azure portal を開いてデータベースに移動し、データ エクスプローラーを使用して JSON ドキュメントをデータベースに追加します。 データを追加するには、他にも高度な方法がありますが、データ エクスプローラーは Azure Cosmos DB によって提供される内部動作と機能を理解するのに最適なツールなので、まずはデータ エクスプローラーを使用します。

## <a name="what-is-the-data-explorer"></a>データ エクスプローラーとは
Azure Cosmos DB データ エクスプローラーは、Azure portal に含まれるツールであり、Azure Cosmos DB に格納されているデータを管理するために使用します。 データ コレクションの表示とデータ コレクション内の移動、およびデータベース内のドキュメントの編集、データのクエリ、ストアド プロシージャの作成と実行のための UI が提供されています。

## <a name="add-data-using-the-data-explorer"></a>データ エクスプローラーを使用してデータを追加する

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[サンドボックスの Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。

    > [!IMPORTANT]
    > 同じアカウントを使って、Azure portal とサンドボックスにログインします。
    >
    > サンドボックスに確実に接続されるように、上記のリンクを使用して Azure portal にログインします。コンシェルジェ サブスクリプションへのアクセスが提供されます。

1. **[すべてのサービス]** > **[データベース]** > **[Azure Cosmos DB]** の順にクリックします。 次に、ご自分のアカウントを選択し、**[データ エクスプローラー]**、**[全画面表示で開く]** の順にクリックします

   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media/3-azure-cosmosdb-data-explorer-full-screen.png)

2. **[全画面表示で開く]** ボックスで、**[開く]** をクリックします。

    Web ブラウザーでは、新しいデータ エクスプローラーが全画面表示され、データベースを操作するための多くの領域と専用の環境が提供されます。

3. 新しい JSON ドキュメントを作成するには、SQL API ウィンドウで、**Clothing** を展開し、**[ドキュメント]** をクリックして、**[新しいドキュメント]** をクリックします。

   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media/3-azure-cosmosdb-data-explorer-new-document.png)

4. ここで、次の構造のドキュメントをコレクションに追加します。 次のコードをコピーして **[ドキュメント]** タブに貼り付けて、現在の内容を上書きします。

     ```json
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
        }
    }
     ```

5. JSON を **[ドキュメント]** タブに追加したら、**[保存]** をクリックします。

    ![Azure portal のデータ エクスプローラーで JSON データをコピーして [保存] をクリックする](../media/3-azure-cosmosdb-data-explorer-save-document.png)

6. **[新しいドキュメント]** をもう一度クリックし、次の JSON オブジェクトをデータ エクスプローラーにコピーして **[保存]** をクリックすることにより、ドキュメントをもう 1 つ作成して保存します。

     ```json
    {
        "id": "2",
        "productId": "33218897",
        "category": "Women's Outerwear",
        "manufacturer": "Contoso",
        "description": "Black wool pea-coat",
        "price": "49.99",
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

7. 左側のメニューで **[ドキュメント]** をクリックして、ドキュメントが保存されていることを確認します。

    データ エクスプローラーには、**[ドキュメント]** タブにドキュメントが 2 つ表示されます。

このユニットでは、データ エクスプローラーを使用して、それぞれが製品カタログ内の製品を表す 2 つのドキュメントを、データベースに追加しました。 データ エクスプローラーは、ドキュメントの作成、ドキュメントの変更、Azure Cosmos DB の使用開始に適しています。
