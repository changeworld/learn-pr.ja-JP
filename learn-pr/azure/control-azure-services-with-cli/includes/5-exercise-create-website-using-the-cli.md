<span data-ttu-id="b313b-101">次に、Azure CLI を使用してリソース グループを作成してから、このリソース グループに Web アプリをデプロイしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="b313b-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span>

<span data-ttu-id="b313b-102">自分の Azure サブスクリプションでインストールした Azure CLI を使用できますが、この演習に使用できる無料のサンド ボックス環境があり、その Azure Cloud Shell には Azure CLI が既にインストールされています。</span><span class="sxs-lookup"><span data-stu-id="b313b-102">You can use the Azure CLI you installed with your own Azure subscription, but there is a free sandbox environment available for this exercise with Azure Cloud Shell where the Azure CLI is already installed.</span></span> <span data-ttu-id="b313b-103">次の手順では、その無料サンドボックスを使うものとします。</span><span class="sxs-lookup"><span data-stu-id="b313b-103">The following instructions assume you will be using the free sandbox.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="b313b-104">リソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="b313b-104">Create a resource group</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

> [!NOTE]
> <span data-ttu-id="b313b-105">通常は、`az group create ...` コマンドを使用して関連するすべての Azure リソース用にリソース グループを作成しますが、以下の演習で使用するリソース グループは既に作成されています。</span><span class="sxs-lookup"><span data-stu-id="b313b-105">Normally, you would create a resource group for all your related Azure resources with an `az group create ...` command, but for these exercises one has been created for you.</span></span> <span data-ttu-id="b313b-106">このリソース グループの名前が、この演習の以降のコマンドに挿入されます。</span><span class="sxs-lookup"><span data-stu-id="b313b-106">This resource group name will be injected into the later commands in this exercise.</span></span>

<!-- TODO: This is original text prior to updates to use the sandbox. These can be worked back in as instructions for people using their own subscriptions. There is one more block like this below. Note that the assignment of RESOURCE_GROUP below would need to be different as well. -->

<!-- 1. Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows. -->

<!-- 1. Start the Azure CLI and run the login command.

    ```azurecli
    az login
    ```
    If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin). -->

<!-- 1. Create a resource group.

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ``` -->

1. <span data-ttu-id="b313b-107">すべてのリソース グループをテーブルに覧表示して、リソース グループが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b313b-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```azurecli
    az group list --output table
    ```

    <span data-ttu-id="b313b-108">無料のサンドボックスで使用するために生成された `<rgn>[Sandbox resource group name]</rgn>` リソース グループのみを表示できます。</span><span class="sxs-lookup"><span data-stu-id="b313b-108">You may only see the `<rgn>[Sandbox resource group name]</rgn>` resource group that was generated for your free sandbox usage.</span></span>

<!-- > [!TIP]
> You can also confirm the resource was created in the Azure portal. Open a web browser, sign in to the portal and navigate to the **Resource Groups** section. The new resource group should be displayed in the list. -->

1. <span data-ttu-id="b313b-109">Azure で開発を行っていると、複数のリソース グループができることがあります。</span><span class="sxs-lookup"><span data-stu-id="b313b-109">As you do more Azure development, you can end up with several resource groups.</span></span> <span data-ttu-id="b313b-110">グループ リスト内に複数のアイテムがある場合は、`--query` オプションを追加することで、返される値をフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="b313b-110">If you have several items in the group list, you can filter the return values by adding a `--query` option.</span></span> <span data-ttu-id="b313b-111">次のコマンドを試してください。</span><span class="sxs-lookup"><span data-stu-id="b313b-111">Try this command:</span></span>

    ```azurecli
    az group list --query "[?name == '<rgn>[Sandbox resource group name]</rgn>']"
    ```

    <span data-ttu-id="b313b-112">クエリは、JSON 要求のための標準クエリ言語である **JMESPath** を使用して書式設定されます。</span><span class="sxs-lookup"><span data-stu-id="b313b-112">The query is formated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="b313b-113">この強力なフィルター言語の詳細については、<http://jmespath.org/> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b313b-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="b313b-114">**Azure CLI を使用した VM の管理**に関するモジュールでもクエリの詳細を説明しています。</span><span class="sxs-lookup"><span data-stu-id="b313b-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="b313b-115">サービス プランの作成手順</span><span class="sxs-lookup"><span data-stu-id="b313b-115">Steps to create a service plan</span></span>

<span data-ttu-id="b313b-116">Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。</span><span class="sxs-lookup"><span data-stu-id="b313b-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="b313b-117">アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。</span><span class="sxs-lookup"><span data-stu-id="b313b-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="b313b-118">App Service プランを作成して、ご利用のアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="b313b-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="b313b-119">次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。</span><span class="sxs-lookup"><span data-stu-id="b313b-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="b313b-120">アプリとプランの名前は "_一意_" でなければなりません。そのため、名前にサフィックスを追加し、以下のコマンド内の `<unique>` テキストを一連の数字、自分のイニシャル、または他の何らかのテキストに置き換えて、Azure 全体で確実に一意になるようにします。</span><span class="sxs-lookup"><span data-stu-id="b313b-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span>

    ```azurecli
    az appservice plan create --name popupappplan-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --location eastus
    ```

    <span data-ttu-id="b313b-121">このコマンドは、完了までに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="b313b-121">This command can take several minutes to complete.</span></span>

1. <span data-ttu-id="b313b-122">テーブルにご利用のすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b313b-122">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="b313b-123">Web アプリの作成手順</span><span class="sxs-lookup"><span data-stu-id="b313b-123">Steps to create a web app</span></span>

<span data-ttu-id="b313b-124">次に、サービス プランに Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="b313b-124">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="b313b-125">それと同時にコードをデプロイすることができますが、この例では、デプロイを別の手順として行います。</span><span class="sxs-lookup"><span data-stu-id="b313b-125">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="b313b-126">Web アプリをプラン作成し、前に作成したプランの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="b313b-126">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="b313b-127">**プランと同じようにアプリ名も一意でなければなりませんので、名前がグローバルに一意となるように、`<unique>` マーカーを何らかのテキストに置換します。**</span><span class="sxs-lookup"><span data-stu-id="b313b-127">**Just like the plan, the app name must be unique, replace the `<unique>` marker with some text to make the name globally unique.**</span></span>
    ```azurecli
    az webapp create --name popupwebapp-<unique> --resource-group popupResGroup --plan popupappplan-<unique>
    ```

1. <span data-ttu-id="b313b-128">テーブルにご利用のすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="b313b-128">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="b313b-129">**DefaultHostName** を書き留めてください。これは後で必要になります。</span><span class="sxs-lookup"><span data-stu-id="b313b-129">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="b313b-130">GitHub からコードをデプロイする手順</span><span class="sxs-lookup"><span data-stu-id="b313b-130">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="b313b-131">最後の手順では、GitHub リポジトリから Web アプリにコードをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="b313b-131">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="b313b-132">Azure Samples Github リポジトリ内で提供されているシンプルな PHP ページを使用してみましょう。実行すると、</span><span class="sxs-lookup"><span data-stu-id="b313b-132">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="b313b-133">"HelloWorld!" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="b313b-133">when it executes.</span></span> <span data-ttu-id="b313b-134">自分で作成した Web アプリ名を必ず使用します。</span><span class="sxs-lookup"><span data-stu-id="b313b-134">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupwebapp-<unique> --resource-group "<rgn>[Sandbox resource group name]</rgn>" --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="b313b-135">コードがデプロイされると、Azure によって、その Web サイトは `azurewebsites.net` ドメイン内で一意のアプリ名を通して使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="b313b-135">Once it's deployed, Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="b313b-136">たとえば、アプリ名を "popupapp mslearn123" とした場合、Web サイトのアドレスは <http://popupapp-mslearn123.azurewebsites.net> となります。</span><span class="sxs-lookup"><span data-stu-id="b313b-136">For example, if my app name was "popupapp-mslearn123", then my website address would be: <http://popupapp-mslearn123.azurewebsites.net>.</span></span> <span data-ttu-id="b313b-137">特定のインスタンスにアクセスするには、正しい URL を入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b313b-137">You will need to type in the correct URL to hit your specific instance.</span></span>

1. <span data-ttu-id="b313b-138">ページに "Hello World!" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="b313b-138">The page displays "Hello World!"</span></span>

1. <span data-ttu-id="b313b-139">ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="b313b-139">Close the browser window.</span></span>

<span data-ttu-id="b313b-140">この演習では、対話型の Azure CLI セッションの典型的なパターンを示しました。</span><span class="sxs-lookup"><span data-stu-id="b313b-140">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="b313b-141">最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。</span><span class="sxs-lookup"><span data-stu-id="b313b-141">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="b313b-142">次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。</span><span class="sxs-lookup"><span data-stu-id="b313b-142">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="b313b-143">この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。</span><span class="sxs-lookup"><span data-stu-id="b313b-143">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
