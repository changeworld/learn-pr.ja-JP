<span data-ttu-id="ac300-101">この演習では、Azure 関数アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="ac300-101">In this exercise, we'll create an Azure function app.</span></span>

1. <span data-ttu-id="ac300-102">ご自分の Azure アカウントを使用して [Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="ac300-102">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true) using your Azure account.</span></span>
1. <span data-ttu-id="ac300-103">Azure portal の左上隅にある **[リソースの作成]** ボタンを選択し、**[計算]、[Function App]** の順に選択して、Function App の *[作成]* ブレードを開きます。</span><span class="sxs-lookup"><span data-stu-id="ac300-103">Select the **Create a resource** button found on the upper left-hand corner of the Azure portal, and then select **Compute > Function App** to open the Function App *Create* blade.</span></span>
  <span data-ttu-id="ac300-104">![*[リソースの作成]* に続き *[計算]* と *[Function App]* の選択が強調表示されている Azure portal のスクリーンショット](../images/4-create-function-app-blade.png)</span><span class="sxs-lookup"><span data-stu-id="ac300-104">![Screenshot of Azure portal highlighting the selection of *Create a resource* followed by *Compute* and then *Function App*](../images/4-create-function-app-blade.png)</span></span>
1. <span data-ttu-id="ac300-105">グローバルに一意のアプリ名を選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-105">Choose a globally unique app name.</span></span> <span data-ttu-id="ac300-106">これはご自分のサービスのベース URL として機能します。</span><span class="sxs-lookup"><span data-stu-id="ac300-106">This will serve as the base URL of your service.</span></span> <span data-ttu-id="ac300-107">たとえば、**escalator-functions-xxxxxxx** という名前を付けることができます。xxxxxxx は自分のイニシャルや誕生年に置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="ac300-107">For example, you can name it **escalator-functions-xxxxxxx**, where the x's can be replaced with your initials and your birth year.</span></span> <span data-ttu-id="ac300-108">グローバルに一意でない場合、別の組み合わせをお試しください。</span><span class="sxs-lookup"><span data-stu-id="ac300-108">If this isn't globally unique, you can try any other combination.</span></span> <span data-ttu-id="ac300-109">有効な文字は a-z、0-9、- (ハイフン) です。</span><span class="sxs-lookup"><span data-stu-id="ac300-109">Valid characters are a-z, 0-9 and -.</span></span>
1. <span data-ttu-id="ac300-110">関数アプリをホストする Azure サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-110">Select the Azure subscription where you would like the function app hosted.</span></span>
1. <span data-ttu-id="ac300-111">**escalator-functions-group** という名前の新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="ac300-111">Create a new resource group called **escalator-functions-group**.</span></span> <span data-ttu-id="ac300-112">このモジュールで使用されるすべてのリソースを保持するためのリソース グループを使用すると、後でクリーンアップするときに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="ac300-112">Using a resource group to hold all resources used in this module will help with clean-up later on.</span></span>
1. <span data-ttu-id="ac300-113">OS に **Windows** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-113">Select **Windows** for OS.</span></span>
1. <span data-ttu-id="ac300-114">**[ホスティング プラン]** には、Azure のサーバーなしという特徴を最大限に活用できるように、**[従量課金プラン]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-114">For **Hosting Plan**, select the **Consumption Plan**, so that we can take advantage of the serverless features of Azure.</span></span>
1. <span data-ttu-id="ac300-115">自分または顧客が住んでいる場所に最も近い地域を選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-115">Select the geographical location closest to you (or your customers).</span></span>
1. <span data-ttu-id="ac300-116">新しいストレージ アカウントを作成し、それに **escalator functions** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="ac300-116">Create a new storage account, and name it **escalator functions**.</span></span>
1. <span data-ttu-id="ac300-117">Azure Application Insights が **[オン]** になっていることを確認し、自分 (または顧客) から最も近いリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-117">Make sure that Azure Application Insights is **On**, and select the region closest to you (or your customers).</span></span>
<span data-ttu-id="ac300-118">完了すると、構成は次のスクリーンショットにある構成のようになります。</span><span class="sxs-lookup"><span data-stu-id="ac300-118">When you're finished, your configuration should look like the config in the following screenshot.</span></span>

  ![前の手順に従ってすべてのフィールドが構成された Function App の *[作成]* 構成画面のスクリーンショット。](../images/4-create-function-app-settings.png)

1. <span data-ttu-id="ac300-120">**[作成]** を選択します。デプロイには数分かかります。</span><span class="sxs-lookup"><span data-stu-id="ac300-120">Select **Create**; deployment will take a few minutes.</span></span> <span data-ttu-id="ac300-121">完了すると、通知を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="ac300-121">You'll receive a notification once it's complete.</span></span>

## <a name="verify-your-azure-function-app"></a><span data-ttu-id="ac300-122">Azure 関数アプリの確認</span><span class="sxs-lookup"><span data-stu-id="ac300-122">Verify your Azure function app</span></span>

1. <span data-ttu-id="ac300-123">Azure Portal の左側にあるメニューから、**[リソース グループ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-123">From the Azure portal left-hand menu, select **Resource groups**.</span></span> <span data-ttu-id="ac300-124">利用できるグループの一覧には **escalator-functions-group** が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="ac300-124">You should then see the **escalator-functions-group** in the list of available groups.</span></span>
  <span data-ttu-id="ac300-125">![ビューにリソース グループ **escalator-functions-group** がある、Azure portal のリソース グループ画面のスクリーンショット。](../images/4-resource-group.png)</span><span class="sxs-lookup"><span data-stu-id="ac300-125">![Screenshot of the Resource groups screen in the Azure portal with the resource group **escalator-functions-group** in view.](../images/4-resource-group.png)</span></span>
1. <span data-ttu-id="ac300-126">**escalator-functions-group** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ac300-126">Select the **escalator-functions-group**.</span></span> <span data-ttu-id="ac300-127">次のようなリソース リストが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="ac300-127">You should then see a resource list similar to the following list.</span></span>
  <span data-ttu-id="ac300-128">![App Service プラン、ストレージ アカウント、Application Insights および App Service へのエントリを含む、**escalator-functions-group** グループ内のすべてのリソースのスクリーンショット](../images/4-resource-list.png)</span><span class="sxs-lookup"><span data-stu-id="ac300-128">![Screenshot of all resources within the **escalator-functions-group** group including entries for App Service plan, Storage account, Application Insights and App Service](../images/4-resource-list.png)</span></span>

<span data-ttu-id="ac300-129">App Service として一覧表示されている稲妻アイコンの付いた項目は、あなたの新しい関数アプリです。</span><span class="sxs-lookup"><span data-stu-id="ac300-129">The item with the lightning bolt icon, listed as an App Service, is your new function app.</span></span> 