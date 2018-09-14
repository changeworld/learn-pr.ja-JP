<span data-ttu-id="06db3-101">次に、Azure CLI を使用してリソース グループを作成してから、このリソース グループに Web アプリをデプロイしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="06db3-101">Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="06db3-102">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="06db3-102">Create a resource group</span></span>

1. <span data-ttu-id="06db3-103">Linux または macOS 上では Bash Shell を開き、Windows から作業している場合は、コマンド プロンプト ウィンドウまたは PowerShell を開きます。</span><span class="sxs-lookup"><span data-stu-id="06db3-103">Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows.</span></span>

1. <span data-ttu-id="06db3-104">Azure CLI を起動し、ログイン コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="06db3-104">Start the Azure CLI and run the login command.</span></span>

    ```azurecli
    az login
    ```
    <span data-ttu-id="06db3-105">Web ブラウザーで Azure のサインイン ページが表示されない場合は、コマンド ラインの手順に従って、[https://aka.ms/devicelogin](https://aka.ms/devicelogin) に認証コードを入力します。</span><span class="sxs-lookup"><span data-stu-id="06db3-105">If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

1. <span data-ttu-id="06db3-106">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="06db3-106">Create a resource group.</span></span>

    ```azurecli
    az group create --location westeurope --name popupResGroup
    ```

1. <span data-ttu-id="06db3-107">テーブルにご利用のリソース グループをすべて一覧表示して、リソース グループが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="06db3-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```azurecli
    az group list --output table
    ```

> [!TIP]
> <span data-ttu-id="06db3-108">Azure portal でリソースが作成されたことを確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="06db3-108">You can also confirm the resource was created in the Azure portal.</span></span> <span data-ttu-id="06db3-109">Web ブラウザーを開き、ポータルにサインインし、**[リソース グループ]** セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="06db3-109">Open a web browser, sign in to the portal and navigate to the **Resource Groups** section.</span></span> <span data-ttu-id="06db3-110">新しいリソース グループが一覧に表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="06db3-110">The new resource group should be displayed in the list.</span></span>

1. <span data-ttu-id="06db3-111">グループ内にアイテムが多数含まれている場合は、`--query` オプションを追加することで、戻り値をフィルター処理することができます。次のコマンドを試してみてください。</span><span class="sxs-lookup"><span data-stu-id="06db3-111">If you have a lot of items in the group, you can filter the return values by adding a `--query` option, try this command:</span></span>

    ```azurecli
    az group list --query "[?name == 'popupResGroup']"
    ```

    <span data-ttu-id="06db3-112">クエリは、JSON 要求を対象とした標準クエリ言語である **JMESPath** を使用してフォーマットされます。</span><span class="sxs-lookup"><span data-stu-id="06db3-112">The query is formmated using **JMESPath** which is a standard query language for JSON requests.</span></span> <span data-ttu-id="06db3-113">この強力なフィルター言語の詳細については、<http://jmespath.org/> を参照してください。</span><span class="sxs-lookup"><span data-stu-id="06db3-113">You can learn more about this powerful filter language at <http://jmespath.org/>.</span></span> <span data-ttu-id="06db3-114">**Azure CLI を使用した VM の管理**に関するモジュールでもクエリの詳細を説明しています。</span><span class="sxs-lookup"><span data-stu-id="06db3-114">We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.</span></span>

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="06db3-115">サービス プランの作成手順</span><span class="sxs-lookup"><span data-stu-id="06db3-115">Steps to create a service plan</span></span>

<span data-ttu-id="06db3-116">Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。</span><span class="sxs-lookup"><span data-stu-id="06db3-116">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="06db3-117">アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。</span><span class="sxs-lookup"><span data-stu-id="06db3-117">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="06db3-118">App Service プランを作成して、ご利用のアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="06db3-118">Create an App Service plan to run your app.</span></span> <span data-ttu-id="06db3-119">次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。</span><span class="sxs-lookup"><span data-stu-id="06db3-119">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    > [!WARNING]
    > <span data-ttu-id="06db3-120">アプリとプランの名前は_一意_でなければなりません。そのため、名前にサフィックスを追加し、以下のコマンド内の `<unique>` テキストを、Azure 全体で確実に一意になるように、一連の数値、自分のイニシャル、またはその他の何らかのテキストに置換します。</span><span class="sxs-lookup"><span data-stu-id="06db3-120">The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure.</span></span> 

    ```azurecli
    az appservice plan create --name popupapp-<unique> --resource-group popupResGroup --location westeurope
    ```

1. <span data-ttu-id="06db3-121">テーブルにご利用のすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="06db3-121">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```azurecli
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="06db3-122">Web アプリの作成手順</span><span class="sxs-lookup"><span data-stu-id="06db3-122">Steps to create a web app</span></span>

<span data-ttu-id="06db3-123">次に、サービス プランに Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="06db3-123">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="06db3-124">それと同時にコードをデプロイすることができますが、この例では、デプロイを別の手順として行います。</span><span class="sxs-lookup"><span data-stu-id="06db3-124">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="06db3-125">Web アプリをプラン作成し、前に作成したプランの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="06db3-125">Create the web app, supply the name of the plan you created above.</span></span> <span data-ttu-id="06db3-126">**プランと同じようにアプリ名も一意でなければなりませんので、名前がグローバルに一意となるように、`<unique>` マーカーを何らかのテキストに置換します。**</span><span class="sxs-lookup"><span data-stu-id="06db3-126">**Just like the plan, the app name must be unique, replace the `<unique>` marker with some text to make the name globally unique.**</span></span>
    ```azurecli
    az webapp create --name popupapp-<unique> --resource-group popupResGroup --plan popupapp-<unique>
    ```

1. <span data-ttu-id="06db3-127">テーブルにご利用のすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="06db3-127">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```azurecli
    az webapp list --output table
    ```

1. <span data-ttu-id="06db3-128">**DefaultHostName** を書き留めてください。これは後で必要になります。</span><span class="sxs-lookup"><span data-stu-id="06db3-128">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="06db3-129">GitHub からコードをデプロイする手順</span><span class="sxs-lookup"><span data-stu-id="06db3-129">Steps to deploy code from GitHub</span></span>

1. <span data-ttu-id="06db3-130">最後の手順では、GitHub リポジトリから Web アプリにコードをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="06db3-130">The final step is to deploy code from a GitHub repository to the web app.</span></span> <span data-ttu-id="06db3-131">Azure Samples Github リポジトリ内で提供されているシンプルな PHP ページを使用してみましょう。実行すると、</span><span class="sxs-lookup"><span data-stu-id="06db3-131">Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!"</span></span> <span data-ttu-id="06db3-132">"HelloWorld!" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="06db3-132">when it executes.</span></span> <span data-ttu-id="06db3-133">自分で作成した Web アプリ名を必ず使用します。</span><span class="sxs-lookup"><span data-stu-id="06db3-133">Make sure to use the web app name you created.</span></span>

    ```azurecli
    az webapp deployment source config --name popupapp-<unique> --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="06db3-134">コードがデプロイされると、Azure によって、その Web サイトは `azurewebsites.net` ドメイン内で一意のアプリ名を通して使用できるようになります。</span><span class="sxs-lookup"><span data-stu-id="06db3-134">Once it's deployed, Azure will make your website available through the unique app name in the `azurewebsites.net` domain.</span></span> <span data-ttu-id="06db3-135">たとえば、アプリ名を "popupapp mslearn123" とした場合、Web サイトのアドレスは <http://popupapp-mslearn123.azurewebsites.net> となります。</span><span class="sxs-lookup"><span data-stu-id="06db3-135">For example, if my app name was "popupapp-mslearn123", then my website address would be: <http://popupapp-mslearn123.azurewebsites.net>.</span></span> <span data-ttu-id="06db3-136">使用する特定のインスタンスに達するには、正しい URL を入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="06db3-136">You will need to type in the correct URL to hit your specific instance.</span></span>

1. <span data-ttu-id="06db3-137">ページに "HelloWorld!" と表示されます。</span><span class="sxs-lookup"><span data-stu-id="06db3-137">The page displays "HelloWorld!"</span></span>

1. <span data-ttu-id="06db3-138">ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="06db3-138">Close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="06db3-139">まとめ</span><span class="sxs-lookup"><span data-stu-id="06db3-139">Summary</span></span>

<span data-ttu-id="06db3-140">この演習では、対話型の Azure CLI セッションの典型的なパターンが示されています。</span><span class="sxs-lookup"><span data-stu-id="06db3-140">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="06db3-141">最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。</span><span class="sxs-lookup"><span data-stu-id="06db3-141">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="06db3-142">次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。</span><span class="sxs-lookup"><span data-stu-id="06db3-142">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="06db3-143">この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。</span><span class="sxs-lookup"><span data-stu-id="06db3-143">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
