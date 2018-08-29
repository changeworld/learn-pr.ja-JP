
<span data-ttu-id="4a518-101">この演習では、ローカル コンピューター上で Azure CLI を使用してリソース グループを作成し、このリソース グループに Web アプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="4a518-101">In this exercise, you will use the Azure CLI on your local machine to create a resource group, and then to deploy a web app into this resource group.</span></span> 

### <a name="steps-to-create-a-resource-group"></a><span data-ttu-id="4a518-102">リソース グループの作成手順</span><span class="sxs-lookup"><span data-stu-id="4a518-102">Steps to create a resource group</span></span>
1. <span data-ttu-id="4a518-103">Linux または macOS 上で Bash Shell を開くか、Windows から作業している場合は、コマンド プロンプト ウィンドウまたは PowerShell を開きます。</span><span class="sxs-lookup"><span data-stu-id="4a518-103">Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows.</span></span>

1. <span data-ttu-id="4a518-104">Azure CLI を起動し、ログイン コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4a518-104">Start the Azure CLI and run the login command.</span></span>

    ```bash
    az login
    ```
    <span data-ttu-id="4a518-105">Web ブラウザーで Azure のサインイン ページが表示されない場合は、コマンド ラインの手順に従って、[https://aka.ms/devicelogin](https://aka.ms/devicelogin) に認証コードを入力します。</span><span class="sxs-lookup"><span data-stu-id="4a518-105">If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).</span></span>

1. <span data-ttu-id="4a518-106">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="4a518-106">Create a resource group.</span></span>

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. <span data-ttu-id="4a518-107">テーブルにすべてのリソース グループを一覧表示して、リソース グループが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4a518-107">Verify that the resource group was created successfully by listing all your resource groups in a table.</span></span>

    ```bash
    az group list --output table
    ```
1. <span data-ttu-id="4a518-108">必要に応じて、Azure portal でリソースが作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4a518-108">Optionally, confirm the resource was created in the Azure portal.</span></span> <span data-ttu-id="4a518-109">Web ブラウザーを開き、ポータルにサインインし、**リソース グループ**のセクション (下記参照) に移動します。</span><span class="sxs-lookup"><span data-stu-id="4a518-109">Open a web browser, sign in to the portal and navigate to the **Resource Groups** section (see below).</span></span> <span data-ttu-id="4a518-110">新しいリソース グループが一覧に表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="4a518-110">The new resource group should be displayed in the list.</span></span>

![ポータルを使用したリソース グループの一覧表示](../media-drafts/5-listing-resource-groups.png)

### <a name="steps-to-create-a-service-plan"></a><span data-ttu-id="4a518-112">サービス プランの作成手順</span><span class="sxs-lookup"><span data-stu-id="4a518-112">Steps to create a service plan</span></span>
<span data-ttu-id="4a518-113">Azure App Service を使用して Web アプリを実行すると、アプリで使用された Azure コンピューティング リソースに対して課金されます。これは、お使いの Web アプリに関連する App Service プランによって異なります。</span><span class="sxs-lookup"><span data-stu-id="4a518-113">When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps.</span></span> <span data-ttu-id="4a518-114">アプリのデータセンターで使用されるリージョン、使用される VM の数、価格レベルは、サービス プランによって決まります。</span><span class="sxs-lookup"><span data-stu-id="4a518-114">Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.</span></span>

1. <span data-ttu-id="4a518-115">アプリを実行するための App Service プランを作成します。</span><span class="sxs-lookup"><span data-stu-id="4a518-115">Create an App Service plan to run your app.</span></span> <span data-ttu-id="4a518-116">アプリとプランの名前は一意である必要があります。そのため、文字列 "12345" をランダムな数字に変更してください。</span><span class="sxs-lookup"><span data-stu-id="4a518-116">The name of the app and plan must be unique, so change the string "12345" to a random number.</span></span> <span data-ttu-id="4a518-117">次のコマンドでは、価格レベルや VM インスタンスの詳細が指定されません。そのため、既定では、1 つの**小規模**な VM インスタンスが含まれる **Basic** プランになります。</span><span class="sxs-lookup"><span data-stu-id="4a518-117">The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.</span></span>

    ```bash
    az appservice plan create --name popupapp12345 --resource-group popupResGroup --location westeurope
    ```

1. <span data-ttu-id="4a518-118">テーブルにすべてのプランを一覧表示して、サービス プランが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4a518-118">Verify that the service plan was created successfully by listing all your plans in a table.</span></span>

    ```bash
    az appservice plan list --output table
    ```

### <a name="steps-to-create-a-web-app"></a><span data-ttu-id="4a518-119">Web アプリの作成手順</span><span class="sxs-lookup"><span data-stu-id="4a518-119">Steps to create a web app</span></span>
<span data-ttu-id="4a518-120">次に、サービス プランに Web アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="4a518-120">Next, you'll create the web app in your service plan.</span></span> <span data-ttu-id="4a518-121">コードを同時に展開することができますが、この例では、別の手順として行います。</span><span class="sxs-lookup"><span data-stu-id="4a518-121">You can deploy the code at the same time, but for our example, we'll do this as separate steps.</span></span>

1. <span data-ttu-id="4a518-122">Web アプリを作成します。文字列 "12345" を、以前に使用したものと同じランダムな数字に変更することを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="4a518-122">Create the web app, remembering to change the string "12345" to the same random number you used earlier.</span></span>
    ```bash
    az webapp create --name popupapp12345 --resource-group popupResGroup --plan popupapp12345
    ```

1. <span data-ttu-id="4a518-123">テーブルにすべてのアプリを一覧表示して、アプリが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4a518-123">Verify that the app was created successfully by listing all your apps in a table.</span></span>

    ```bash
    az webapp list --output table
    ```

1. <span data-ttu-id="4a518-124">**DefaultHostName** を書き留めてください。これは後で必要になります。</span><span class="sxs-lookup"><span data-stu-id="4a518-124">Make a note of the **DefaultHostName**; you will need this later.</span></span>

### <a name="steps-to-deploy-code-from-github"></a><span data-ttu-id="4a518-125">GitHub からコードを展開する手順</span><span class="sxs-lookup"><span data-stu-id="4a518-125">Steps to deploy code from GitHub</span></span>
1. <span data-ttu-id="4a518-126">最後の手順は、GitHub リポジトリから Web アプリへのコードの展開です。ここでも、文字列 "12345" を、以前に使用したものと同じランダムな数字に変更することを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="4a518-126">The final step is to deploy code from a GitHub repository to the web app, again remembering to change the string "12345" to the same random number you used earlier.</span></span>
    ```bash
    az webapp deployment source config --name popupapp12345 --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. <span data-ttu-id="4a518-127">Web アプリを表示するには、ブラウザーに次の URL をコピーします。</span><span class="sxs-lookup"><span data-stu-id="4a518-127">Copy the following url into a browser to see the web app.</span></span>
<span data-ttu-id="4a518-128">http://popupapp12345.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="4a518-128">http://popupapp12345.azurewebsites.net</span></span>

1. <span data-ttu-id="4a518-129">ページに "HelloWorld!" と表示されます</span><span class="sxs-lookup"><span data-stu-id="4a518-129">The page displays "HelloWorld!"</span></span>

1. <span data-ttu-id="4a518-130">ブラウザー ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="4a518-130">Close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="4a518-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="4a518-131">Summary</span></span>
<span data-ttu-id="4a518-132">この演習では、対話型の Azure CLI セッションの典型的なパターンが示されています。</span><span class="sxs-lookup"><span data-stu-id="4a518-132">This exercise demonstrated a typical pattern for an interactive Azure CLI session.</span></span> <span data-ttu-id="4a518-133">最初に、標準のコマンドを使用して、新しいリソース グループを作成しました。</span><span class="sxs-lookup"><span data-stu-id="4a518-133">You first used a standard command to create a new resource group.</span></span> <span data-ttu-id="4a518-134">次に、一連のコマンドを使用して、リソース (この例では Web アプリ) をこのリソース グループに展開しました。</span><span class="sxs-lookup"><span data-stu-id="4a518-134">You then used a set of commands to deploy a resource (in this example, a web app) into this resource group.</span></span> <span data-ttu-id="4a518-135">この一連のコマンドは、簡単にシェル スクリプトに結合して、同じリソースを作成する必要があるたびに実行できます。</span><span class="sxs-lookup"><span data-stu-id="4a518-135">This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.</span></span>
