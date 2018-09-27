<span data-ttu-id="32a66-101">よく使用される値を格納して返すために、Azure Redis Cache インスタンスを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="32a66-101">Let's create an Azure Redis Cache instance to store and return commonly used values.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-redis-cache-in-azure"></a><span data-ttu-id="32a66-102">Azure で Redis Cache を作成する</span><span class="sxs-lookup"><span data-stu-id="32a66-102">Create a Redis cache in Azure</span></span>

1. <span data-ttu-id="32a66-103">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="32a66-103">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="32a66-104">**[リソースの作成]**、**[データベース]**、**[Redis Cache]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="32a66-104">Click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="32a66-105">次のスクリーンショットは、Azure portal のさまざまなデータベース リソース オプション内の Redis Cache の場所を示しています。</span><span class="sxs-lookup"><span data-stu-id="32a66-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![[Create a resource]\(リソースの作成\)、[データベース]、[Redis Cache] オプションが強調表示された、Azure portal データベースのオプションを示すスクリーンショット。](../media/4-create-a-cache-1.png)

### <a name="configure-your-cache"></a><span data-ttu-id="32a66-107">キャッシュを構成する</span><span class="sxs-lookup"><span data-stu-id="32a66-107">Configure your cache</span></span>

<span data-ttu-id="32a66-108">キャッシュに次の設定を適用します。</span><span class="sxs-lookup"><span data-stu-id="32a66-108">Apply the following settings on the cache.</span></span>

1. <span data-ttu-id="32a66-109">**DNS 名:** **ContosoSportsApp[nnn]** のように、グローバルに一意の名前を作成します。`[nnn]` はランダムな数字に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="32a66-109">**DNS Name:** Create a globally unique name such as **ContosoSportsApp[nnn]**, where `[nnn]` is replaced with random numbers.</span></span>

1. <span data-ttu-id="32a66-110">**サブスクリプション:** コンシェルジェ サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="32a66-110">**Subscription:** Select the Concierge subscription.</span></span>

1. <span data-ttu-id="32a66-111">**リソース グループ:** リソース グループの <rgn>[サンドボックス リソース グループ名]</rgn> を選択します。</span><span class="sxs-lookup"><span data-stu-id="32a66-111">**Resource group:** Select <rgn>[sandbox resource group name]</rgn> for the Resource Group.</span></span>

1. <span data-ttu-id="32a66-112">**場所:** 通常は、顧客に近い場所 (この例では東海岸) を選択します。</span><span class="sxs-lookup"><span data-stu-id="32a66-112">**Location:** Normally, you would select a location near your customers - in this case, the East Coast.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

5. <span data-ttu-id="32a66-113">**価格レベル:** **[Basic C0]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="32a66-113">**Pricing tier:** Select **Basic C0**.</span></span> <span data-ttu-id="32a66-114">これは使用できる最下位レベルです。</span><span class="sxs-lookup"><span data-stu-id="32a66-114">This is the lowest tier you can use.</span></span> <span data-ttu-id="32a66-115">運用環境のアプリでは、より多くのデータを格納し、高いレベルを選択する必要があるクラスタリングなどの Premium 機能を利用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="32a66-115">Production apps would likely want to store more data and utilize some of the Premium features such as clustering which would require a higher tier selection.</span></span>

1. <span data-ttu-id="32a66-116">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="32a66-116">Click **Create**.</span></span> <span data-ttu-id="32a66-117">Azure によって、自分の Redis Cache インスタンスが作成されデプロイされます。</span><span class="sxs-lookup"><span data-stu-id="32a66-117">Azure will create and deploy the Redis Cache instance for you.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="32a66-118">通常、Redis Cache リソースは Azure portal で非常に迅速に作成され表示されますが、キャッシュ自体は数分間使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="32a66-118">Usually, the Redis cache resource will be created and viewable in the Azure portal very quickly, but the cache itself will not be available for a few minutes.</span></span> <span data-ttu-id="32a66-119">次の手順では、キャッシュの状態を確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="32a66-119">The next steps show how to check the status of your cache.</span></span>

## <a name="verify-your-data"></a><span data-ttu-id="32a66-120">データの検証</span><span class="sxs-lookup"><span data-stu-id="32a66-120">Verify your data</span></span>

<span data-ttu-id="32a66-121">Azure portal の**コンソール**機能を使用して、Redis Cache が作成された後に、このキャッシュに対するコマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="32a66-121">You can use the **Console** feature in the Azure portal to issue commands to your Redis cache instance after it has been created.</span></span>

1. <span data-ttu-id="32a66-122">デプロイの完了時の **[通知]** ポップアップ経由、または左側のサイドバーで **[すべてのリソース]** を選択して Redis Cache を見つけ、左のフィルター ボックスを使用して Redis Cache インスタンスを選択します。</span><span class="sxs-lookup"><span data-stu-id="32a66-122">Locate your Redis cache through the **Notification** popup when it finishes deployment, or by selecting **All Resources** in the left-hand sidebar and using the filter box on the left to select Redis Cache instances.</span></span> <span data-ttu-id="32a66-123">または、上部にある検索ボックスを使用して、キャッシュの名前を入力することもできます。</span><span class="sxs-lookup"><span data-stu-id="32a66-123">Alternatively, you can use the search box at the top and type the name of the cache.</span></span>

1. <span data-ttu-id="32a66-124">Redis Cache インスタンスを選択します。</span><span class="sxs-lookup"><span data-stu-id="32a66-124">Select your Redis cache instance.</span></span>

1. <span data-ttu-id="32a66-125">"Status" フィールドの値を確認します。</span><span class="sxs-lookup"><span data-stu-id="32a66-125">Check the value of the "Status" field.</span></span> <span data-ttu-id="32a66-126">状態が "Running" になるまで、キャッシュの準備ができていません。</span><span class="sxs-lookup"><span data-stu-id="32a66-126">The cache is not ready until the status is "Running".</span></span> <span data-ttu-id="32a66-127">続行する前に、数分間待機しなければならない場合があります。</span><span class="sxs-lookup"><span data-stu-id="32a66-127">You might have to wait for a few minutes before proceeding.</span></span>

1. <span data-ttu-id="32a66-128">キャッシュが実行されたら、Redis Cache の **[概要]** ブレードのツールバーにある [`>_ Console`] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="32a66-128">Once the cache is running, Click the `>_ Console` button in the toolbar on the **Overview** blade for your Redis Cache.</span></span> <span data-ttu-id="32a66-129">これにより、詳細な Redis コマンドを入力できる Redis コンソールが開きます。</span><span class="sxs-lookup"><span data-stu-id="32a66-129">This will open a Redis console, which allows you to enter low-level Redis commands.</span></span> <span data-ttu-id="32a66-130">次のいくつかの操作を試します。</span><span class="sxs-lookup"><span data-stu-id="32a66-130">Try some of the following:</span></span>

    ```console
    ping
    ```

    ```output
    PONG
    ```

    ```console
    set test one
    ```

    ```output
    OK
    ```

    ```console
    get test
    ```

    ```output
    "one"
    ```

<span data-ttu-id="32a66-131">上部の階層リンク バーを使用するか、またはスクロールバーを使用してビューを左へスライドし、**[概要]** パネルに戻ります。</span><span class="sxs-lookup"><span data-stu-id="32a66-131">Switch back to the **Overview** panel either through the breadcrumb bar on the top, or use the scrollbar to slide the view back to the left.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="32a66-132">アクセス キーおよびホスト名を取得する</span><span class="sxs-lookup"><span data-stu-id="32a66-132">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="32a66-133">**[設定]** > **[アクセス キー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="32a66-133">Select **Settings** > **Access keys**.</span></span>

1. <span data-ttu-id="32a66-134">**プライマリ接続文字列 (StackExchange.Redis)** を安全な場所にコピーします。次の演習で必要になります。</span><span class="sxs-lookup"><span data-stu-id="32a66-134">Copy the **Primary connection string (StackExchange.Redis)** to a safe place, you will need it for the next exercise.</span></span>

    <span data-ttu-id="32a66-135">このキーには、使用する **StackExchange.Redis** パッケージのアプリケーション設定内で使用する完全な接続文字列のプライマリ キーとホスト名が含まれます。</span><span class="sxs-lookup"><span data-stu-id="32a66-135">This key includes your primary key and host name in a complete connection string for use within your application settings for the **StackExchange.Redis** package we are going to use.</span></span>

<span data-ttu-id="32a66-136">次に、キャッシュの問い合わせに使用できるコマンドについて学習しましょう。</span><span class="sxs-lookup"><span data-stu-id="32a66-136">Next, let's learn about some of the commands we can use to interrogate the cache.</span></span>