<span data-ttu-id="7f33e-101">最初に必要な作業は、使用する空の Azure Cosmos DB データベースとコレクションを作成することです。</span><span class="sxs-lookup"><span data-stu-id="7f33e-101">The first thing we need to do is create an empty Azure Cosmos DB database and collection to work with.</span></span> <span data-ttu-id="7f33e-102">このラーニング パスの直前のモジュールで作成したのと同じものが必要です。**"Products"** という名前のデータベースと **"Clothing"** という名前のコレクションです。</span><span class="sxs-lookup"><span data-stu-id="7f33e-102">We want them to match the ones you created in the last module in this Learning Path: a database named **"Products"** and a collection named **"Clothing"**.</span></span> <span data-ttu-id="7f33e-103">以下の手順と、画面の右側の Azure Cloud Shell を使用して、データベースを再作成します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-103">Use the following instructions and the Azure Cloud Shell on the right side of the screen to recreate the database.</span></span>

## <a name="create-an-azure-cosmos-db-account--database-with-the-azure-cli"></a><span data-ttu-id="7f33e-104">Azure CLI を使用して Azure Cosmos DB アカウントとデータベースを作成する</span><span class="sxs-lookup"><span data-stu-id="7f33e-104">Create an Azure Cosmos DB account + database with the Azure CLI</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="select-a-subscription"></a><span data-ttu-id="7f33e-105">サブスクリプションを選択する</span><span class="sxs-lookup"><span data-stu-id="7f33e-105">Select a subscription</span></span>

<span data-ttu-id="7f33e-106">Azure をしばらく使用したことがある場合は、複数のサブスクリプションを利用できるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="7f33e-106">If you've been using Azure for a while, you might have multiple subscriptions available to you.</span></span> <span data-ttu-id="7f33e-107">たとえば、開発者が Visual Studio 用のサブスクリプションと会社のリソース用の別のサブスクリプションを持っているのはよくあることです。</span><span class="sxs-lookup"><span data-stu-id="7f33e-107">This is often the case for developers who might have a subscription for Visual Studio, and another for corporate resources.</span></span>

<span data-ttu-id="7f33e-108">Azure サンドボックスでは既に Cloud Shell でお客様用のコンシェルジェ サブスクリプションが選択されており、次の手順を使用してサブスクリプションの設定を検証できます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-108">The Azure sandbox has already selected the Concierge Subscription for you in the Cloud Shell, and you can validate the subscription setting using these steps.</span></span> <span data-ttu-id="7f33e-109">または、お客様が自分のサブスクリプションで作業するのであれば、Azure CLI を使用して以下の手順でサブスクリプションを切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-109">Or, when you are working with your own subscription, you can use the following steps to switch subscriptions with the Azure CLI.</span></span>

1. <span data-ttu-id="7f33e-110">最初に、利用可能なサブスクリプションの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-110">Start by listing the available subscriptions.</span></span>

    ```azurecli
    az account list --output table
    ```

   <span data-ttu-id="7f33e-111">コンシェルジェ サブスクリプションをご利用の場合は、一覧にはそれだけが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-111">If you're working with a Concierge Subscription, it should be the only one listed.</span></span>

1. <span data-ttu-id="7f33e-112">次に、既定のサブスクリプションを使いたくない場合は、`account set` コマンドで変更できます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-112">Next, if the default subscription isn't the one you want to use, you can change it with the `account set` command:</span></span>

    ```azurecli
    az account set --subscription "<subscription name>"
    ```
    
1. <span data-ttu-id="7f33e-113">サンドボックスによってお客様用に作成されたリソース グループを取得します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-113">Get the Resource Group that has been created for you by the sandbox.</span></span> <span data-ttu-id="7f33e-114">ご自分のサブスクリプションを使用する場合は、この手順をスキップして、使用する一意の名前を下の `RESOURCE_GROUP` 環境変数に指定します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-114">If you are using your own subscription, skip this step and just supply a unique name you want to use in the `RESOURCE_GROUP` environment variable below.</span></span> <span data-ttu-id="7f33e-115">リソース グループ名をメモしておきます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-115">Take note of the Resource Group name.</span></span> <span data-ttu-id="7f33e-116">ここにデータベースを作成することになります。</span><span class="sxs-lookup"><span data-stu-id="7f33e-116">This is where we will create our database.</span></span>

    ```azurecli
    az group list --out table
    ```
### <a name="setup-environment-variables"></a><span data-ttu-id="7f33e-117">環境変数を設定する</span><span class="sxs-lookup"><span data-stu-id="7f33e-117">Setup environment variables</span></span>

1. <span data-ttu-id="7f33e-118">いくつかの環境変数を設定すると、毎回共通の値を入力しないで済むようにできます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-118">Set a few environment variables so you don't have to type the common values each time.</span></span> <span data-ttu-id="7f33e-119">まず、Azure Cosmos DB アカウントに名前を付けます。たとえば、「`export NAME="mycosmosdbaccount"`」とします。</span><span class="sxs-lookup"><span data-stu-id="7f33e-119">Start by setting a name for the Azure Cosmos DB account, for example `export NAME="mycosmosdbaccount"`.</span></span> <span data-ttu-id="7f33e-120">このフィールドには小文字、数字、'-' 文字のみを含めることができます。文字数は 3 から 31 文字にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f33e-120">The field can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

    ```azurecli
    export NAME="<Azure Cosmos DB account name>"
    ```

1. <span data-ttu-id="7f33e-121">既存のサンドボックス リソース グループを使用するようにリソース グループを設定します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-121">Set the resource group to use the existing sandbox resource group.</span></span>

    ```azurecli
    export RESOURCE_GROUP="<rgn>[sandbox resource group name]</rgn>"
    ```

1. <span data-ttu-id="7f33e-122">最も近いリージョンを選択し、`export LOCATION="EastUS"` など、環境変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-122">Select the region closest to you, and set the environment variable, such as `export LOCATION="EastUS"`.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    export LOCATION="<location>"
    ```

1. <span data-ttu-id="7f33e-123">データベース名の変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-123">Set a variable for the database name.</span></span> <span data-ttu-id="7f33e-124">名前を "Products" と付けて、直前のモジュールで作成したデータベースと一致するようにします。</span><span class="sxs-lookup"><span data-stu-id="7f33e-124">Name it "Products" so it matches the database we created in the last module.</span></span>

    ```azurecli
    export DB_NAME="Products"
    ```

### <a name="create-a-resource-group-in-your-subscription"></a><span data-ttu-id="7f33e-125">サブスクリプションにリソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="7f33e-125">Create a resource group in your subscription</span></span>

<span data-ttu-id="7f33e-126">ご自分のサブスクリプションに Cosmos DB を作成する場合は、すべての関連リソースを保持するための新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-126">When you are creating a Cosmos DB on your own subscription you will want to create a new resource group to hold all the related resources.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7f33e-127">Microsoft Learn 提供の Azure サンドボックスを使用している場合は、この手順を行う必要はありません。</span><span class="sxs-lookup"><span data-stu-id="7f33e-127">If you are using the Azure sandbox provided by Microsoft Learn, then you do not need to execute this step.</span></span> <span data-ttu-id="7f33e-128">代わりに、上の `RESOURCE_GROUP` 変数に **<rgn>[サンドボックス リソース グループ名]</rgn>** が設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-128">Instead, make sure the `RESOURCE_GROUP` variable above is set to **<rgn>[sandbox Resource Group Name</rgn>**.</span></span>

<span data-ttu-id="7f33e-129">ご自分のサブスクリプションの場合は、次のコマンドを使用してリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-129">In your own subscription you would use the following command to create the Resource Group.</span></span> 

```azurecli
az group create --name <name> --location <location>
```

### <a name="create-the-azure-cosmos-db-account"></a><span data-ttu-id="7f33e-130">Azure Cosmos DB アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="7f33e-130">Create the Azure Cosmos DB account</span></span>

1. <span data-ttu-id="7f33e-131">`cosmosdb create` コマンドを使用して Azure Cosmos DB アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-131">Create the Azure Cosmos DB account with the `cosmosdb create` command.</span></span> <span data-ttu-id="7f33e-132">このコマンドは次のパラメーターを使用し、推奨される環境変数を設定した場合は変更なしで実行できます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-132">The command uses the following parameters and can be run with no modifications if you set the environment variables as recommended.</span></span>
    - <span data-ttu-id="7f33e-133">`--name`: リソースの一意名です。</span><span class="sxs-lookup"><span data-stu-id="7f33e-133">`--name`: Unique name for the resource.</span></span>
    - <span data-ttu-id="7f33e-134">`--kind`: データベースの種類です。_GlobalDocumentDB_ を使用します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-134">`--kind`: Kind of database, use _GlobalDocumentDB_.</span></span>
    - <span data-ttu-id="7f33e-135">`--resource-group`: リソース グループです。</span><span class="sxs-lookup"><span data-stu-id="7f33e-135">`--resource-group`: The resource group.</span></span> <span data-ttu-id="7f33e-136">**<rgn>[サンドボックス リソース グループ名]</rgn>** を使用します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-136">Use **<rgn>[sandbox Resource Group]</rgn>**.</span></span> <span data-ttu-id="7f33e-137">この値が `RESOURCE_GROUP` 変数に割り当てられている必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f33e-137">It should be assigned to the `RESOURCE_GROUP` variable.</span></span>

    ```azurecli
    az cosmosdb create --name $NAME --kind GlobalDocumentDB --resource-group $RESOURCE_GROUP
    ```

    <span data-ttu-id="7f33e-138">コマンドが完了するまでには数分かかり、新しいアカウントが展開されると Cloud Shell にその設定が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7f33e-138">The command takes a few minutes to complete and the cloud shell displays the settings for the new account once it's deployed.</span></span>

1. <span data-ttu-id="7f33e-139">アカウントで `Products` データベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-139">Create the `Products` database in the account.</span></span>

    ```azurecli
    az cosmosdb database create --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

1. <span data-ttu-id="7f33e-140">最後に、`Clothing` コレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="7f33e-140">Finally, create the `Clothing` collection.</span></span>

    ```azurecli
    az cosmosdb collection create --collection-name "Clothing" --partition-key-path "/productId" --throughput 1000 --name $NAME --db-name $DB_NAME --resource-group $RESOURCE_GROUP
    ```

<span data-ttu-id="7f33e-141">これで、Azure Cosmos DB アカウント、データベース、コレクションができあがりました。データを追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="7f33e-141">Now that you have your Azure Cosmos DB account, database, and collection, let's go add some data!</span></span>