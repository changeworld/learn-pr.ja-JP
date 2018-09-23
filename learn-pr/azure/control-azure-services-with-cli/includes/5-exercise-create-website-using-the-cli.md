<span data-ttu-id="80b56-101">次に、Azure CLI を使用してリソース グループを作成してから、このリソース グループに Web アプリをデプロイしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="80b56-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="using-a-resource-group"></a><span data-ttu-id="80b56-102">リソース グループを使用</span><span class="sxs-lookup"><span data-stu-id="80b56-102">Using a resource group</span></span>

<span data-ttu-id="80b56-103">独自のマシンと Azure サブスクリプションを使用している場合は、最初に `az login` コマンドを使用して Azure にログインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="80b56-103">When you are working with your own machine and Azure subscription you will need to first login to Azure using the `az login` command.</span></span> <span data-ttu-id="80b56-104">クラウド シェル環境ではこの作業は不要です。</span><span class="sxs-lookup"><span data-stu-id="80b56-104">This is unnecessary with the Cloud Shell environment.</span></span>

<span data-ttu-id="80b56-105">次に、通常は `az group create` コマンドを使用して関連するすべての Azure リソース用にリソース グループを作成しますが、以下の演習で使用するリソース グループは既に作成されています。</span><span class="sxs-lookup"><span data-stu-id="80b56-105">Next, you would normally create a resource group for all your related Azure resources with an `az group create` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="80b56-106">リソース グループに **<rgn>[サンドボックス リソース グループ名]</rgn>** を使用します。</span><span class="sxs-lookup"><span data-stu-id="80b56-106">Use **<rgn>[Sandbox resource group name]</rgn>** for your resource group.</span></span>

1. <span data-ttu-id="80b56-107">テーブルにすべてのリソース グループを一覧表示するよう Azure CLI に指示できます。</span><span class="sxs-lookup"><span data-stu-id="80b56-107">You can ask the Azure CLI to list all your resource groups in a table.</span></span> <span data-ttu-id="80b56-108">無料の Azure サンドボックスを使用している場合、リソース グループは 1 つしかありません。</span><span class="sxs-lookup"><span data-stu-id="80b56-108">There should just be one while you are in the free Azure Sandbox.</span></span>

    ```azurecli
    az group list --output table
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="80b56-109">Azure で開発を行っていると、複数のリソース グループができることがあります。</span><span class="sxs-lookup"><span data-stu-id="80b56-109">As you do more Azure development, you can end up with several resource groups.</span></span> <span data-ttu-id="80b56-110">グループ リスト内に複数のアイテムがある場合は、`--query` オプションを追加することで、返される値をフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="80b56-110">If you have several items in the group list, you can filter the return values by adding a `--query` option.</span></span> <span data-ttu-id="80b56-111">次のコマンドを試してください。</span><span class="sxs-lookup"><span data-stu-id="80b56-111">Try this command:</span></span>

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    <span data-ttu-id="80b56-112">クエリは、JSON 要求のための標準クエリ言語である **JMESPath** を使用して書式設定されます。</span><span class="sxs-lookup"><span data-stu-id="80b56-112">The query is formated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="80b56-113">この強力なフィルター言語の詳細については、<http://jmespath.org/> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="80b56-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="80b56-114">**Azure CLI を使用した VM の管理**に関するモジュールでもクエリの詳細を説明しています。</span><span class="sxs-lookup"><span data-stu-id="80b56-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="80b56-115">サービス プランの作成手順</span><span class="sxs-lookup"><span data-stu-id="80b56-115">Steps to create a service plan</span></span>

<span data-ttu-id="80b56-116">Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。</span><span class="sxs-lookup"><span data-stu-id="80b56-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="80b56-117">アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。</span><span class="sxs-lookup"><span data-stu-id="80b56-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="80b56-118">App Service プランを作成して、ご利用のアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="80b56-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="80b56-119">次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。</span><span class="sxs-lookup"><span data-stu-id="80b56-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="80b56-120">アプリとプランの名前は "_一意_" でなければなりません。そのため、名前にサフィックスを追加し、以下のコマンド内の `<unique>` テキストを一連の数字、自分のイニシャル、または他の何らかのテキストに置き換えて、Azure 全体で確実に一意になるようにします。</span><span class="sxs-lookup"><span data-stu-id="80b56-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span>

    <span data-ttu-id="80b56-121">`--location` パラメーターでは、以下のいずれかの場所の値を使用します。</span><span class="sxs-lookup"><span data-stu-id="80b56-121">For the `--location` parameter, use one of the below location values.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --location <location>
    ```

    <span data-ttu-id="80b56-122">このコマンドは、完了までに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="80b56-122">This command can take several minutes to complete.</span></span>

1. <span data-ttu-id="80b56-123">テーブルにご利用のすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="80b56-123">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="80b56-124">Web アプリの作成手順</span><span class="sxs-lookup"><span data-stu-id="80b56-124">Steps to create a web app</span></span>

<span data-ttu-id="80b56-125">次に、サービス プランに Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="80b56-125">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="80b56-126">それと同時にコードをデプロイすることができますが、この例では、デプロイを別の手順として行います。</span><span class="sxs-lookup"><span data-stu-id="80b56-126">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="80b56-127">Web アプリをプラン作成し、前に作成したプランの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="80b56-127">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="80b56-128">**プランと同じようにアプリ名も一意でなければならない**ので、名前がグローバルに一意となるように、`<unique>` マーカーを何らかのテキストに置換します。</span><span class="sxs-lookup"><span data-stu-id="80b56-128">**Just like the plan, the app name must be unique**, replace the `<unique>` marker with some text to make the name globally unique.</span></span>

    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --plan popupappplan-<unique>
    ```

1. <span data-ttu-id="80b56-129">テーブルにご利用のすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="80b56-129">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="80b56-130">テーブルに表示された **DefaultHostName** をメモしておいてください。これは、新しい Web サイトのアクセス可能な Web アドレスです。</span><span class="sxs-lookup"><span data-stu-id="80b56-130">Make a note of the **DefaultHostName** listed in the table; this is the reachable web address for the new website.</span></span> <span data-ttu-id="80b56-131">Azure によって、その Web サイトは `azurewebsites.net` ドメイン内で一意のアプリ名を通して使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="80b56-131">Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="80b56-132">たとえば、アプリ名を "popupwebapp-mslearn123" とした場合、Web サイトの URL は `http://popupwebapp-mslearn123.azurewebsites.net` となります。</span><span class="sxs-lookup"><span data-stu-id="80b56-132">For example, if my app name was "popupwebapp-mslearn123", then my website URL would be: `http://popupwebapp-mslearn123.azurewebsites.net`.</span></span>

1. <span data-ttu-id="80b56-133">サイトには、Azure で作成された "クイック スタート" ページがあり、ブラウザーで、または CURL で単に **DefaultHostName** を使用して表示できます。</span><span class="sxs-lookup"><span data-stu-id="80b56-133">Your site has a "QuickStart" page created by Azure that you can see either in a browser, or with CURL, just use the **DefaultHostName**:</span></span>

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="80b56-134">GitHub からコードをデプロイする手順</span><span class="sxs-lookup"><span data-stu-id="80b56-134">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="80b56-135">最後の手順では、GitHub リポジトリから Web アプリにコードをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="80b56-135">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="80b56-136">Azure Samples Github リポジトリ内で提供されているシンプルな PHP ページを使用してみましょう。実行すると、</span><span class="sxs-lookup"><span data-stu-id="80b56-136">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="80b56-137">"HelloWorld!" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="80b56-137">when it executes.</span></span> <span data-ttu-id="80b56-138">自分で作成した Web アプリ名を必ず使用します。</span><span class="sxs-lookup"><span data-stu-id="80b56-138">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group <rgn>[Sandbox resource group name]</rgn> --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="80b56-139">デプロイされたら、ブラウザーまたは CURL でサイトを再表示します。</span><span class="sxs-lookup"><span data-stu-id="80b56-139">Once it's deployed, hit your site again with a browser or CURL.</span></span>

    ```bash
    curl popupwebapp-<unique>.azurewebsites.net
    ```
    
    <span data-ttu-id="80b56-140">ページに "Hello World!" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="80b56-140">The page displays "Hello World!"</span></span>

    ```output
    Hello World!
    ```

<span data-ttu-id="80b56-141">この演習では、対話型の Azure CLI セッションの典型的なパターンを示しました。</span><span class="sxs-lookup"><span data-stu-id="80b56-141">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="80b56-142">最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。</span><span class="sxs-lookup"><span data-stu-id="80b56-142">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="80b56-143">次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。</span><span class="sxs-lookup"><span data-stu-id="80b56-143">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="80b56-144">この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。</span><span class="sxs-lookup"><span data-stu-id="80b56-144">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>