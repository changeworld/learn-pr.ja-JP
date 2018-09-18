> [!NOTE]
> 「**スケーリングするようにビルドされた Azure Cosmos DB データベースを作成する**」または「**自分の Azure Cosmos DB データベースにデータを挿入し、クエリを実行する**」に続いてこちらを参照し、作成した Cosmos DB データベースおよびコレクションを削除していない場合は、このユニットをスキップして、Visual Studio Code の設定まで移動してください。

最初に行う必要があるのは、使用するために空の Cosmos DB データベースおよびコレクションを作成することです。 このラーニング パスの直前のモジュールで作成したのと同じものが必要です。**"Products"** という名前のデータベースと **"Clothing"** という名前のコレクションです。 以下の手順と、画面の右側の Azure Cloud Shell を使用して、データベースを再作成します。

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a>Azure CLI を使用して Cosmos DB アカウントおよびデータベースを作成する

1. まず適切なサブスクリプションを選択します。無料の教育用アクセス サブスクリプションに関連付けられたサブスクリプション ID を選択してください。

    ```azurecli
    az account list --output table
    ```

1. サブスクリプションの一覧に "サンドボックス" が表示されているのを確認し、それを使用する最新のものとして設定します。 <!-- TODO: get official name here -->

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. 作成されたリソース グループを取得します。 自分のサブスクリプションを使用する場合は、この手順をスキップして、使用する一意の名前を下の `RESOURCE_GROUP` 環境変数に指定します。 リソース グループ名をメモしておきます。 ここにデータベースを作成することになります。 <!-- Do we get a token for this? -->

    ```azurecli
    az group list --out table
    ```

1. 少し簡単にするには、いくつかの環境変数を設定して、毎回共通の値を入力しないで済むようにできます。 

    > [!IMPORTANT]
    > これらの値はセッションに合わせて適切な値に変更してください。 たとえば、`<resource group>` 値は、上で確認したリソース グループ名で置き換えます。

    ```azurecli
    export RESOURCE_GROUP="<resource group>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```
    
1. 次に、データベース名の変数を設定します。 名前を "Users" と付けて、直前のモジュールで作成したデータベースと一致するようにします。

    ```azurecli
    export DB_NAME="Products"
    ```
    
1. 自分のサブスクリプションでこの手順を行い、_新しい_リソース グループを使用する場合 (推奨)、次のコマンドを使用してリソース グループを作成します。 **重要:** Microsoft Learn 提供の無料教育用リソースを使用している場合は、この手順を実行する必要はありません。 代わりに、上の `RESOURCE_GROUP` 変数に、割り当てられたリソース グループを設定してください。

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. 次に、Azure Cosmos DB アカウントを作成します。 これが完了するまでに数分かかります。

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```
    
1. アカウントで `Products` データベースを作成します。

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```
    
1. 最後に、`Clothing` コレクションを作成します。

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

これで、Cosmos DB アカウント、データベース、コレクションを用意できました。Visual Studio Code で作業を開始しましょう。