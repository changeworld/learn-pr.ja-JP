<span data-ttu-id="60515-101">よく使用される値を格納して返すために、Azure Redis Cache インスタンスを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="60515-101">Let's create an Azure Redis Cache instance to store and return commonly used values.</span></span>

<!-- TODO: do we need to activate the sandbox here? -->

## <a name="create-a-redis-cache-in-azure"></a><span data-ttu-id="60515-102">Azure で Redis Cache を作成する</span><span class="sxs-lookup"><span data-stu-id="60515-102">Create a Redis cache in Azure</span></span>

1. <span data-ttu-id="60515-103">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="60515-103">Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="60515-104">**[リソースの作成]**、**[データベース]**、**[Redis Cache]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="60515-104">Click **Create a resource**, click **Databases**, and click **Redis Cache**.</span></span>

    <span data-ttu-id="60515-105">次のスクリーンショットは、Azure portal のさまざまなデータベース リソース オプション内の Redis Cache の場所を示しています。</span><span class="sxs-lookup"><span data-stu-id="60515-105">The following screenshot shows the Redis Cache location within the various database resource options on the Azure portal.</span></span>

    ![[リソースの作成]、[データベース]、[Redis Cache] オプションが強調表示された、Azure portal のデータベース オプションを示すスクリーンショット。](../media/4-create-a-cache-1.png)

### <a name="identify-the-location-for-the-cache"></a><span data-ttu-id="60515-107">キャッシュの場所を特定する</span><span class="sxs-lookup"><span data-stu-id="60515-107">Identify the location for the cache</span></span>

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="configure-your-cache"></a><span data-ttu-id="60515-108">キャッシュを構成する</span><span class="sxs-lookup"><span data-stu-id="60515-108">Configure your cache</span></span>

<span data-ttu-id="60515-109">キャッシュに次の設定を適用します。</span><span class="sxs-lookup"><span data-stu-id="60515-109">Apply the following settings on the cache.</span></span>

1. <span data-ttu-id="60515-110">**DNS 名:** **ContosoSportsApp1028** など、グローバルに一意の名前を作成します。</span><span class="sxs-lookup"><span data-stu-id="60515-110">**DNS Name:** Create a globally unique name such as **ContosoSportsApp1028**.</span></span>

1. <span data-ttu-id="60515-111">**サブスクリプション:** Azure サンドボックス サブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="60515-111">**Subscription:** Select the Azure Sandbox subscription.</span></span>

1. <span data-ttu-id="60515-112">**リソース グループ:** リソース グループの<rgn>サンドボックス リソース グループ名</rgn>を選択します。</span><span class="sxs-lookup"><span data-stu-id="60515-112">**Resource group:** Select <rgn>[Sandbox resource group name]</rgn> for the Resource Group.</span></span>

1. <span data-ttu-id="60515-113">**場所:** 通常は、顧客に近い場所 (この例では東海岸) を選択します。</span><span class="sxs-lookup"><span data-stu-id="60515-113">**Location:** Normally, you would select a location near your customers - in this case, the East Coast.</span></span> <span data-ttu-id="60515-114">ただし、Azure サンドボックスでは、上記のようにリソースには特定のリージョンのみを選択できます。</span><span class="sxs-lookup"><span data-stu-id="60515-114">However, the Azure Sandbox only allows specific regions to be selected for resources as noted above.</span></span> <span data-ttu-id="60515-115">それらの場所のいずれかを選択してください。</span><span class="sxs-lookup"><span data-stu-id="60515-115">Please select one of those locations.</span></span>

1. <span data-ttu-id="60515-116">**価格レベル:** **[Basic C0]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60515-116">**Pricing tier:** Select **Basic C0**.</span></span> <span data-ttu-id="60515-117">これは使用できる最下位レベルです。</span><span class="sxs-lookup"><span data-stu-id="60515-117">This is the lowest tier you can use.</span></span> <span data-ttu-id="60515-118">運用環境のアプリでは、より多くのデータを格納し、高いレベルを選択する必要があるクラスタリングなどの Premium 機能を利用する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="60515-118">Production apps would likely want to store more data and utilize some of the Premium features such as clustering which would require a higher tier selection.</span></span>

1. <span data-ttu-id="60515-119">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="60515-119">Click **Create**.</span></span>

    <span data-ttu-id="60515-120">次のスクリーンショットは、新しい Redis Cache リソースの作成に使用された、代表的な構成を示しています。</span><span class="sxs-lookup"><span data-stu-id="60515-120">The following screenshot shows a representative configuration used to create a new Redis Cache resource.</span></span> <span data-ttu-id="60515-121">構成は、Azure サンドボックスによって若干異なります。</span><span class="sxs-lookup"><span data-stu-id="60515-121">Note that yours will be slightly different due to the Azure Sandbox.</span></span>

    ![DNS 名、サブスクリプション、新しいリソース グループ、場所、価格レベルの構成例が入力された、新しい Redis Cache リソースを作成するときの Azure portal のブレードを示すスクリーンショット。](../media/4-create-a-cache-2.png)

> [!IMPORTANT]
> <span data-ttu-id="60515-123">続行する前に、キャッシュがデプロイされるまで待つ必要があります。</span><span class="sxs-lookup"><span data-stu-id="60515-123">You will have to wait until the cache is deployed before continuing.</span></span> <span data-ttu-id="60515-124">このプロセスにはしばらく時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="60515-124">This process might take some time.</span></span>

## <a name="retrieve-the-access-keys-and-host-name"></a><span data-ttu-id="60515-125">アクセス キーおよびホスト名を取得する</span><span class="sxs-lookup"><span data-stu-id="60515-125">Retrieve the access keys and host name</span></span>

1. <span data-ttu-id="60515-126">Azure portal で新しいキャッシュ インスタンスに移動し、**[設定]** > **[アクセス キー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="60515-126">Navigate to your new cache instance in the Azure portal and select **Settings** > **Access keys**.</span></span> 

1. <span data-ttu-id="60515-127">**プライマリ接続文字列 (StackExchange.Redis)** を安全な場所にコピーします。次の演習で必要になります。</span><span class="sxs-lookup"><span data-stu-id="60515-127">Copy the **Primary connection string (StackExchange.Redis)** to a safe place, you will need it for the next exercise.</span></span>

    <span data-ttu-id="60515-128">このキーには、使用する **StackExchange.Redis** パッケージのアプリケーション設定内で使用する完全な接続文字列のプライマリ キーとホスト名が含まれます。</span><span class="sxs-lookup"><span data-stu-id="60515-128">This key includes your primary key and host name in a complete connection string for use within your application settings for the **StackExchange.Redis** package we are going to use.</span></span>

<span data-ttu-id="60515-129">次に、キャッシュの問い合わせに使用できるコマンドについて学習しましょう。</span><span class="sxs-lookup"><span data-stu-id="60515-129">Next, let's learn about some of the commands we can use to interrogate the cache.</span></span>