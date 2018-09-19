<span data-ttu-id="a38cf-101">最初に必要な作業は、使用する空の Azure Cosmos DB データベースとコレクションを作成することです。</span><span class="sxs-lookup"><span data-stu-id="a38cf-101">The first thing we need to do is create an empty Azure Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="a38cf-102">このラーニング パスの直前のモジュールで作成したのと同じものが必要です。**"Products"** という名前のデータベースと **"Clothing"** という名前のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="a38cf-102">We want them to match the ones you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="a38cf-103">以下の手順と、画面の右側の Azure Cloud Shell を使用して、データベースを再作成します。</span><span class="sxs-lookup"><span data-stu-id="a38cf-103">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

# <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="a38cf-104">Azure CLI を使用して Azure Cosmos DB アカウントとデータベースを作成する</span><span class="sxs-lookup"><span data-stu-id="a38cf-104">Create an Azure Cosmos DB account + database with the Azure CLI</span></span>

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

1. <span data-ttu-id="a38cf-105">いくつかの環境変数を設定し、毎回共通の値を入力しないで済むようにできます。</span><span class="sxs-lookup"><span data-stu-id="a38cf-105">Set a few environment variables so you don't have to type the common values each time.</span></span> <span data-ttu-id="a38cf-106">まず、Azure Cosmos DB アカウントに名前を付けます。たとえば、「`export NAME="mycosmosdbaccount"`」とします。</span><span class="sxs-lookup"><span data-stu-id="a38cf-106">Start by setting a name for the Azure Cosmos DB account, for example `export NAME="mycosmosdbaccount"`.</span></span> <span data-ttu-id="a38cf-107">このフィールドには小文字、数字、'-' 文字のみを含めることができます。文字数は 3 - 31 文字にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a38cf-107">The field can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

2. <span data-ttu-id="a38cf-108">既存のサンドボックス リソース グループを使用するようにリソース グループを設定します。</span><span class="sxs-lookup"><span data-stu-id="a38cf-108">Set the resource group to use the existing Sandbox resource group.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<rgn>[Sandbox resource group name]</rgn>"
    ```

2. <span data-ttu-id="a38cf-109">最も近いリージョンを選択し、`export LOCATION="EastUS"` など、環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="a38cf-109">Select the region closest to you, and set the environment variable, such as `export LOCATION="EastUS"`.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

2. <span data-ttu-id="a38cf-110">データベース名の変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="a38cf-110">Set a variable for the database name.</span></span> <span data-ttu-id="a38cf-111">名前を "Products" と付けて、直前のモジュールで作成したデータベースと一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="a38cf-111">Name it "Products" so it matches the database we created in the last module.</span></span>

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

3. <span data-ttu-id="a38cf-112">次に、Azure Cosmos DB アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="a38cf-112">Next, create the Azure Cosmos DB account.</span></span> <span data-ttu-id="a38cf-113">これが完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="a38cf-113">This will take a few minutes to complete.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

4. <span data-ttu-id="a38cf-114">アカウントで `Products` データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="a38cf-114">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

5. <span data-ttu-id="a38cf-115">最後に、`Clothing` コレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="a38cf-115">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="a38cf-116">これで、Azure Cosmos DB アカウント、データベース、コレクションができあがりました。データを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="a38cf-116">Now that you have your Azure Cosmos DB account, database, and collection, let's go add some data!</span></span>