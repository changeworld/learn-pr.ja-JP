<span data-ttu-id="cfd7a-101">Visual Studio Code 用 Azure Cosmos DB 拡張機能は、コマンド ウィンドウを使用してリソースを作成できるようにすることで、アカウント、データベース、およびコレクションの作成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-101">The Azure Cosmos DB extension for Visual Studio Code simplifies account, database, and collection creation by enabling you to create resources using the command window.</span></span>

<span data-ttu-id="cfd7a-102">このユニットでは、Visual Studio 用 Azure Cosmos DB 拡張機能をインストールした後、それを使用して、アカウント、データベース、およびコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-102">In this unit you will install the Azure Cosmos DB extension for Visual Studio, and then use it to create an account, database, and collection.</span></span>

## <a name="install-the-azure-cosmos-db-extension-for-visual-studio"></a><span data-ttu-id="cfd7a-103">Visual Studio 用 Azure Cosmos DB 拡張機能をインストールする</span><span class="sxs-lookup"><span data-stu-id="cfd7a-103">Install the Azure Cosmos DB extension for Visual Studio</span></span>

1. <span data-ttu-id="cfd7a-104">[Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) に移動し、Visual Studio Code 用 **Azure Cosmos DB** 拡張機能をインストールします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-104">Go to the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb) and install the **Azure Cosmos DB** extension for Visual Studio Code.</span></span>

1. <span data-ttu-id="cfd7a-105">拡張機能タブが Visual Studio Code に読み込まれたら、**[インストール]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-105">When the extension tab loads in Visual Studio Code, click **Install**.</span></span>

1. <span data-ttu-id="cfd7a-106">インストールが完了したら、**[再読み込み]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-106">After installation is complete, click **Reload**.</span></span>

    <span data-ttu-id="cfd7a-107">Visual Studio Code によって以下が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-107">Visual Studio Code displays the</span></span> ![Azure アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) <span data-ttu-id="cfd7a-109">拡張機能がインストールされ、再読み込みされた後の画面左側の Azure アイコン。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-109">Azure icon on the left side of the screen after the extension is installed and reloaded.</span></span>

## <a name="create-an-azure-cosmos-db-account-in-visual-studio-code"></a><span data-ttu-id="cfd7a-110">Visual Studio Code で Azure Cosmos DB アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="cfd7a-110">Create an Azure Cosmos DB account in Visual Studio Code</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

1. <span data-ttu-id="cfd7a-111">Visual Studio Code で、**[表示]** > **[コマンド パレット]** の順にクリックし、「**Azure: Sign In**」と入力して、Azure にサインインします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-111">In Visual Studio Code, sign in to Azure by clicking **View** > **Command Palette** and typing **Azure: Sign In**.</span></span> <span data-ttu-id="cfd7a-112">Azure: Sign In を使用するには、[Azure アカウント](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)拡張機能がインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-112">You must have the [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extension installed to use Azure: Sign In.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="cfd7a-113">サンドボックスを作成するために使用したアカウントで Azure にログインします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-113">Login to Azure using the same account used to create the sandbox.</span></span> <span data-ttu-id="cfd7a-114">サンドボックスは、コンシェルジェ サブスクリプションへのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-114">The sandbox provides access to a Concierge Subscription.</span></span>

    <span data-ttu-id="cfd7a-115">プロンプトに従って、Web ブラウザーで提供されるコードをコピーして貼り付けます。これにより、Visual Studio Code セッションが認証されます。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-115">Follow the prompts to copy and paste the code provided in the web browser, which authenticates your Visual Studio Code session.</span></span>

1. <span data-ttu-id="cfd7a-116">左側のメニューの![Azure アイコン](../media/2-setup/visual-studio-code-explorer-icon.png) **Azure** アイコンをクリックします。次に、**コンシェルジェ サブスクリプション**を右クリックし、**[アカウントの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-116">Click the ![Azure icon](../media/2-setup/visual-studio-code-explorer-icon.png) **Azure** icon on the left menu, and then right-click **Concierge Subscription**, and click **Create Account**.</span></span>

    <span data-ttu-id="cfd7a-117">コンシェルジェ サブスクリプションが表示されない場合は、確実に Visual Studio Code でサンドボックスを作成するために使用したアカウントで Azure にログインします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-117">If you do not see the Concierge Subscription listed, ensure you logged into Azure in Visual Studio Code using the same account used to create the sandbox.</span></span> <span data-ttu-id="cfd7a-118">また、Azure アカウント拡張機能で Azure サブスクリプションをフィルター処理した場合は、`> Azure: Select Subscriptions` コマンドでコンシェルジェ サブスクリプションがチェックされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-118">Additionally, if you have filtered your Azure subscriptions in the Azure Account extension, verify the Concierge Subscription is checked in the `> Azure: Select Subscriptions` command.</span></span>

1. <span data-ttu-id="cfd7a-119">__+__ ボタンをクリックして、Cosmos DB アカウントの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-119">Click the __+__ button to start creating a Cosmos DB account.</span></span> <span data-ttu-id="cfd7a-120">複数のサブスクリプションがある場合は、サブスクリプションを選択するように求められます。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-120">You will be asked to select the subscription if you have more than one.</span></span>

1. <span data-ttu-id="cfd7a-121">画面の上部にあるテキスト ボックスに、Azure Cosmos DB アカウントの一意の名前を入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-121">In the text box at the top of the screen, enter a unique name for your Azure Cosmos DB account, and then press enter.</span></span> <span data-ttu-id="cfd7a-122">アカウント名には、小文字、数字、'-' 文字のみを含めることができます。文字数は 3 から 31 文字にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-122">The account name can contain only lowercase letters, numbers and the '-' character, and must be between 3 and 31 characters.</span></span>

1. <span data-ttu-id="cfd7a-123">次に、**[SQL (DocumentDB)]** > **<rgn>[サンドボックス リソース グループ名]</rgn>** を選択し、場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-123">Next, select **SQL (DocumentDB)** > **<rgn>[Sandbox resource group name]</rgn>**, and then select a location.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    <span data-ttu-id="cfd7a-124">Visual Studio Code の [出力] タブに、アカウント作成の進行状況が表示されます。完了まで数分かかります。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-124">The output tab in Visual Studio Code displays the progress of the account creation, it takes a few minutes to complete.</span></span>

1. <span data-ttu-id="cfd7a-125">アカウントが作成されたら、**Azure: Cosmos DB** ウィンドウで Azure サブスクリプションを展開します。拡張機能によって新しい Azure Cosmos DB アカウントが表示します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-125">After the account is created, expand your Azure subscription in the **Azure: Cosmos DB** pane and the extension displays the new Azure Cosmos DB account.</span></span> <span data-ttu-id="cfd7a-126">次の図では、新しいアカウントの名前は **learning-modules** になっています。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-126">In the following image, the new account is named **learning-modules**.</span></span>

    ![Azure Cosmos DB の Visual Studio Code 拡張機能](../media/2-setup/azure-cosmos-db-vs-code-extension.png)

## <a name="create-an-azure-cosmos-db-database-and-collection-in-visual-studio-code"></a><span data-ttu-id="cfd7a-128">Visual Studio Code で Azure Cosmos DB データベースとコレクションを作成する</span><span class="sxs-lookup"><span data-stu-id="cfd7a-128">Create an Azure Cosmos DB database and collection in Visual Studio Code</span></span>

<span data-ttu-id="cfd7a-129">次は、顧客用の新しいデータベースとコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-129">Now let's create a new database and collection for your customers.</span></span>

1. <span data-ttu-id="cfd7a-130">Azure: Cosmos DB ウィンドウで、新しいアカウントを右クリックし、**[データベースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-130">In the Azure: Cosmos DB pane, right-click your new account, and then click **Create Database**.</span></span>
1. <span data-ttu-id="cfd7a-131">画面上部の入力パレットで、データベース名に「`Users`」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-131">In the input palette at the top of the screen, type `Users` for the database name and press Enter.</span></span>
1. <span data-ttu-id="cfd7a-132">コレクション名に「`WebCustomers`」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-132">Enter `WebCustomers` for the collection name and press Enter.</span></span>
1. <span data-ttu-id="cfd7a-133">パーティション キーに「`userId`」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-133">Enter `userId` for the partition key and press Enter.</span></span>
1. <span data-ttu-id="cfd7a-134">最後に、最初のスループット容量が `1000` であることを確認し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-134">Finally, confirm `1000` for the initial throughput capacity and press Enter.</span></span>
1. <span data-ttu-id="cfd7a-135">**[Azure: Cosmos DB]** ウィンドウでアカウントを展開します。新しい **Users** データベースと **WebCustomers** コレクションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-135">Expand the account in the **Azure: Cosmos DB** pane, and the new **Users** database and **WebCustomers** collection are displayed.</span></span>

    ![上記の手順を示すアニメーションは、Visual Studio Code の Azure Cosmos DB 拡張機能を実行します。](../media/2-setup/vs-code-azure-cosmos-db-extension.gif)

<span data-ttu-id="cfd7a-137">Azure Cosmos DB アカウントが作成されたので、Visual Studio Code での作業にとりかかることができます。</span><span class="sxs-lookup"><span data-stu-id="cfd7a-137">Now that you have your Azure Cosmos DB account, lets get to work in Visual Studio Code!</span></span>
