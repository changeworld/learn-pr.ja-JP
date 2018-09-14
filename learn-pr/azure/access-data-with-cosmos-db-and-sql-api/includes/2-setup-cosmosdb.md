> [!NOTE]
> **スケールを目的とした Azure Cosmos DB データベースの作成**に続いてこちらを参照し、作成した Cosmos DB データベースおよびコレクションを削除していない場合は、このユニットをスキップして、データ エクスプローラーによるデータの追加まで移動してください。

最初に行う必要があるのは、使用するために空の Cosmos DB データベースおよびコレクションを作成することです。 このラーニング パスの直前のモジュールで作成したのと同じものが必要です。**"Products"** という名前のデータベースと **"Clothing"** という名前のコレクションです。 以下の手順と、画面の右側の Azure Cloud Shell を使用して、データベースを再作成します。

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a>Azure CLI を使用して Cosmos DB アカウントおよびデータベースを作成する

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<!--
TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well.

1. Start by selecting the correct subscription - you want to select the subscription ID associated with your free education access subscription.

    ```azurecli
    az account list --output table
    ```

1. Make sure you see "sandbox" in the subscription list and set it as the current one to use:

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. Get the Resource Group that has been created for you. If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below. Take note of the Resource Group name. This is where we will create our database.

    ```azurecli
    az group list --out table
    ```
-->

1. たびに、一般的な値を入力する必要はありませんので、いくつかの環境変数を設定します。

    > [!IMPORTANT]
    > これらの値はセッションに合わせて適切な値に変更してください。 たとえば、置換、`<comsos db name>`が Cosmos DB の名前を持つ値。

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```

2. 次に、データベース名の変数を設定します。 名前を "Users" と付けて、直前のモジュールで作成したデータベースと一致するようにします。

    ```azurecli
    export DB_NAME="Products"
    ```

<!-- 

TODO: Pre-sandbox text to be worked back in.

1. If you are doing this on your own subscription, and you are using a _new_ Resource Group (recommended), then use the following command to create the Resource Group. **Important:** If you are using the free education resources provided by Microsoft Learn, then you do not need to execute this step. Instead, make sure the `RESOURCE_GROUP` variable above is set to your assigned resource group.

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
-->

3. 次に、Azure Cosmos DB アカウントを作成します。 これが完了するまでに数分かかります。

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

4. アカウントで `Products` データベースを作成します。

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

5. 最後に、`Clothing` コレクションを作成します。

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

これで、Cosmos DB アカウント、データベース、コレクションができあがりました。データを追加しましょう。