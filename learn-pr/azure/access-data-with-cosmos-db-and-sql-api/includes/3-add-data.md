Azure Cosmos DB データベースにデータを追加するのは簡単です。 Azure portal を開いてデータベースに移動し、データ エクスプローラーを使用して JSON ドキュメントをデータベースに追加します。 データを追加するには、他にも高度な方法がありますが、データ エクスプローラーは Azure Cosmos DB によって提供される内部動作と機能を理解するのに最適なツールなので、まずはデータ エクスプローラーを使用します。

## <a name="what-is-the-data-explorer"></a>データ エクスプローラーとは
Azure Cosmos DB データ エクスプローラーは、Azure portal に含まれるツールであり、Azure Cosmos DB に格納されているデータを管理するために使用します。 データ コレクションの表示とデータ コレクション内の移動、およびデータベース内のドキュメントの編集のための UI が提供されています。

## <a name="add-data-using-the-data-explorer"></a>データ エクスプローラーを使用してデータを追加する

1. 前のモジュールから続けている場合は、Azure portal ウィンドウで **[データ エクスプローラー]** をクリックして、**[全画面表示で開く]** をクリックします。

    それ以外の場合は、[Azure portal](https://portal.azure.com/?azure-portal=true) にサインインし、**[すべてのサービス]** > **[データベース]** > **[Azure Cosmos DB]** をクリックします。 次に、ご自分のアカウントを選択し、**[データ エクスプローラー]**、**[全画面表示で開く]** の順にクリックします
 
   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media-draft/3-azure-cosmosdb-data-explorer-full-screen.png)

2. **[全画面表示で開く]** ボックスで、**[開く]** をクリックします。

    Web ブラウザーでは、新しいデータ エクスプローラーが全画面表示され、データベースを操作するための多くの領域と専用の環境が提供されます。

3. 新しい JSON ドキュメントを作成するには、**[新しいドキュメント]** をクリックします。

   ![Azure portal のデータ エクスプローラーで新しいドキュメントを作成する](../media-draft/3-azure-cosmosdb-data-explorer-new-document.png)

4. ここで、次の構造のドキュメントをコレクションに追加します。 次のコードをコピーして、**[ドキュメント]** タブに貼り付けるだけです。

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

5. JSON を **[ドキュメント]** タブに追加したら、**[保存]** をクリックします。

    ![JSON データをコピーし、Azure portal のデータ エクスプローラーで [保存] をクリックします](../media-draft/3-azure-cosmosdb-data-explorer-save-document.png)

6. 次の JSON オブジェクトをデータ エクスプローラーにコピーして、**[保存]** をクリックすることによって、ドキュメントをもう 1 つ作成して保存します。

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

7. 左側のメニューで **[ドキュメント]** をクリックして、ドキュメントが保存されていることを確認します。 

    データ エクスプローラーには、**[ドキュメント]** タブにドキュメントが 2 つ表示されます。

## <a name="summary"></a>まとめ

このモジュールでは、データ エクスプローラーを使用して、それぞれが製品カタログ内の製品を表す 2 つのドキュメントを、データベースに追加しました。 データ エクスプローラーは、ドキュメントの作成、ドキュメントの変更、Azure Cosmos DB の使用開始に適しています。  
