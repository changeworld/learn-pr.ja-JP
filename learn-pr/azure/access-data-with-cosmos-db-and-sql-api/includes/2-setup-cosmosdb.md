<span data-ttu-id="0e59f-101">最初の手順を行うには、空の Azure Cosmos DB データベースとを使用するコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-101">The first thing we need to do is create an empty Azure Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="0e59f-102">このラーニング パスの最後のモジュールで作成したものと同じにするときに: という名前のデータベース **"Products"** という名前のコレクションと **"Clothing"** します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-102">We want them to match the ones you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="0e59f-103">以下の手順と、画面の右側の Azure Cloud Shell を使用して、データベースを再作成します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-103">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

# <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="0e59f-104">Azure CLI を使用して、Azure Cosmos DB アカウントとデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-104">Create an Azure Cosmos DB account + database with the Azure CLI</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

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

1. <span data-ttu-id="0e59f-105">たびに、一般的な値を入力する必要はありませんので、いくつかの環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-105">Set a few environment variables so you don't have to type the common values each time.</span></span> <span data-ttu-id="0e59f-106">たとえば、Azure Cosmos DB アカウントの名前を設定して開始`export NAME="mycosmosdbaccount"`します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-106">Start by setting a name for the Azure Cosmos DB account, for example `export NAME="mycosmosdbaccount"`.</span></span> <span data-ttu-id="0e59f-107">フィールドはのみ英小文字、数字を含めることができます、'-' 文字、および 3 文字以上 31 文字の間である必要があります。</span><span class="sxs-lookup"><span data-stu-id="0e59f-107">The field can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

2. <span data-ttu-id="0e59f-108">サンド ボックスの既存のリソース グループを使用するリソース グループを設定します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-108">Set the resource group to use the existing Sandbox resource group.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    ```

2. <span data-ttu-id="0e59f-109">最も近いリージョンを選択しなど、環境変数の設定`export LOCATION="EastUS"`します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-109">Select the region closest to you, and set the environment variable, such as `export LOCATION="EastUS"`.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

2. <span data-ttu-id="0e59f-110">データベース名の変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-110">Set a variable for the database name.</span></span> <span data-ttu-id="0e59f-111">ファイル名"Products"最後のモジュールで作成したデータベースに一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="0e59f-111">Name it "Products" so it matches the database we created in the last module.</span></span>

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

3. <span data-ttu-id="0e59f-112">次に、Azure Cosmos DB アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-112">Next, create the Azure Cosmos DB account.</span></span> <span data-ttu-id="0e59f-113">これが完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="0e59f-113">This will take a few minutes to complete.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

4. <span data-ttu-id="0e59f-114">アカウントで `Products` データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-114">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

5. <span data-ttu-id="0e59f-115">最後に、`Clothing` コレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e59f-115">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="0e59f-116">Azure Cosmos DB アカウント、データベース、およびコレクションをいくつかのデータを追加してみましょう go!</span><span class="sxs-lookup"><span data-stu-id="0e59f-116">Now that you have your Azure Cosmos DB account, database, and collection, let's go add some data!</span></span>