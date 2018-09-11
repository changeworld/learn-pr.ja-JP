> [!NOTE]
> <span data-ttu-id="c7a3e-101">**スケールを目的とした Azure Cosmos DB データベースの作成**に続いてこちらを参照し、作成した Cosmos DB データベースおよびコレクションを削除していない場合は、このユニットをスキップして、データ エクスプローラーによるデータの追加まで移動してください。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-101">If you are continuing on from **Create an Azure Cosmos DB database built to scale** and you did not delete the Cosmos DB database + collection that you created, then you can skip this unit and move on to adding data with the Data Explorer.</span></span>

<span data-ttu-id="c7a3e-102">最初に行う必要があるのは、使用するために空の Cosmos DB データベースおよびコレクションを作成することです。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-102">The first thing we need to do is create an empty Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="c7a3e-103">このラーニング パスの直前のモジュールで作成したのと同じものが必要です。**"Products"** という名前のデータベースと **"Clothing"** という名前のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-103">We want it to match the one you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="c7a3e-104">以下の手順と、画面の右側の Azure Cloud Shell を使用して、データベースを再作成します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-104">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

# <a name="create-a-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="c7a3e-105">Azure CLI を使用して Cosmos DB アカウントおよびデータベースを作成する</span><span class="sxs-lookup"><span data-stu-id="c7a3e-105">Create a Cosmos DB account + database with the Azure CLI</span></span>

1. <span data-ttu-id="c7a3e-106">まず適切なサブスクリプションを選択します。無料の教育用アクセス サブスクリプションに関連付けられたサブスクリプション ID を選択してください。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-106">Start by selecting the correct subscription - you want to select the subscription ID associated with your free education access subscription.</span></span>

    ```azurecli
    az account list --output table
    ```

1. <span data-ttu-id="c7a3e-107">サブスクリプションの一覧に "サンドボックス" が表示されているのを確認し、それを使用する最新のものとして設定します。 <!-- TODO: get official name here --></span><span class="sxs-lookup"><span data-stu-id="c7a3e-107">Make sure you see "sandbox" in the subscription list and set it as the current one to use: <!-- TODO: get official name here --></span></span>

    ```azurecli
    az account set --subscription "sandbox"
    ```
    
1. <span data-ttu-id="c7a3e-108">作成されたリソース グループを取得します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-108">Get the Resource Group that has been created for you.</span></span> <span data-ttu-id="c7a3e-109">自分のサブスクリプションを使用する場合は、この手順をスキップして、使用する一意の名前を下の `RESOURCE_GROUP` 環境変数に指定します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-109">If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below.</span></span> <span data-ttu-id="c7a3e-110">リソース グループ名をメモしておきます。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-110">Take note of the Resource Group name.</span></span> <span data-ttu-id="c7a3e-111">ここにデータベースを作成することになります。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-111">This is where we will create our database.</span></span> <!-- Do we get a token for this? -->

    ```azurecli
    az group list --out table
    ```

1. <span data-ttu-id="c7a3e-112">少し簡単にするには、いくつかの環境変数を設定して、毎回共通の値を入力しないで済むようにできます。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-112">To make this a bit easier, set a few environment variables so you don't have to type the common values each time.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="c7a3e-113">これらの値はセッションに合わせて適切な値に変更してください。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-113">Make sure to change these values to ones appropriate for your session.</span></span> <span data-ttu-id="c7a3e-114">たとえば、`<resource group>` 値は、上で確認したリソース グループ名で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-114">For example, replace the `<resource group>` value with the Resource Group name you identified above.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<resource group>"
    export NAME="<cosmos db name>"
    export LOCATION="<location>"
    ```
    
1. <span data-ttu-id="c7a3e-115">次に、データベース名の変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-115">Next, set a variable for the database name.</span></span> <span data-ttu-id="c7a3e-116">名前を "Users" と付けて、直前のモジュールで作成したデータベースと一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-116">Name it "Users" so it matches the database we created in the last module.</span></span>

    ```azurecli
    export DB_NAME="Products"
    ```
    
1. <span data-ttu-id="c7a3e-117">自分のサブスクリプションでこの手順を行い、_新しい_リソース グループを使用する場合 (推奨)、次のコマンドを使用してリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-117">If you are doing this on your own subscription, and you are using a _new_ Resource Group (recommended), then use the following command to create the Resource Group.</span></span> <span data-ttu-id="c7a3e-118">**重要:** Microsoft Learn 提供の無料教育用リソースを使用している場合は、この手順を実行する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-118">**Important:** If you are using the free education resources provided by Microsoft Learn, then you do not need to execute this step.</span></span> <span data-ttu-id="c7a3e-119">代わりに、上の `RESOURCE_GROUP` 変数に、割り当てられたリソース グループを設定してください。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-119">Instead, make sure the `RESOURCE_GROUP` variable above is set to your assigned resource group.</span></span>

    ```azurecli
    az group create --name $RESOURCE_GROUP --location $LOCATION
    ```
    
1. <span data-ttu-id="c7a3e-120">次に、Azure Cosmos DB アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-120">Next, create the Cosmos DB account.</span></span> <span data-ttu-id="c7a3e-121">これが完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-121">This will take a few minutes to complete.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```
    
1. <span data-ttu-id="c7a3e-122">アカウントで `Products` データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-122">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```
    
1. <span data-ttu-id="c7a3e-123">最後に、`Clothing` コレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-123">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="c7a3e-124">これで、Cosmos DB アカウント、データベース、コレクションができあがりました。データを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="c7a3e-124">Now that we have our Cosmos DB account, database, and collection, let's go add some data!</span></span>