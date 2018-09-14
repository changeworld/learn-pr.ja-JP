> [!NOTE]
> 続きは場合**組み込まれているため、Azure Cosmos DB データベースを作成**または**Azure Cosmos DB データベースのデータを挿入とクエリ**Azure Cosmos DB アカウントが既にあるし、これを省略します。単位と Visual Studio Code の設定に移動します。

最初の手順を行うには、Azure Cosmos DB アカウントを作成します。

# <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントを作成する

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
      
1. 自分のサブスクリプションでこの手順を行い、_新しい_リソース グループを使用する場合 (推奨)、次のコマンドを使用してリソース グループを作成します。 **重要:** Microsoft Learn 提供の無料教育用リソースを使用している場合は、この手順を実行する必要はありません。 代わりに、上の `RESOURCE_GROUP` 変数に、割り当てられたリソース グループを設定してください。

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. 次に、Azure Cosmos DB アカウントを作成します。 これが完了するまでに数分かかります。

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

あるので、Cosmos DB アカウントは、Visual Studio Code での作業にとりかかることができます。
