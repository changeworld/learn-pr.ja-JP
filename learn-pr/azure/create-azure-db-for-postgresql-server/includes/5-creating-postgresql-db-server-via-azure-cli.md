<span data-ttu-id="666e1-101">あなたはオンプレミスの PostgreSQL データベースを使っているものとしましょう。</span><span class="sxs-lookup"><span data-stu-id="666e1-101">Let's assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="666e1-102">あなたの会社は、サーバーを Azure に移動することによって、デバイスのサポート、可用性、データの追跡、および処理の機能を拡張しようとしています。</span><span class="sxs-lookup"><span data-stu-id="666e1-102">Your company is now looking at expanding device support, availability, data tracking, and processing features by moving your server into Azure.</span></span> <span data-ttu-id="666e1-103">あなたは、Azure Database for PostgreSQL サーバーの作成の自動化にかかる労力を調べています。</span><span class="sxs-lookup"><span data-stu-id="666e1-103">You'll investigate how much effort it takes to automate the creation of an Azure Database for PostgreSQL server.</span></span>

<span data-ttu-id="666e1-104">Azure portal を使用して単一の Azure Database for PostgreSQL サーバーを作成するのは簡単です。</span><span class="sxs-lookup"><span data-stu-id="666e1-104">Creating a single Azure Database for PostgreSQL server using the Azure portal is easy.</span></span> <span data-ttu-id="666e1-105">複数のデータベースの作成と継続的なメンテナンスをポータルのみを使用して行おうとすると、面倒になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="666e1-105">Creating more than one database and running on-going maintenance using only the portal may become tedious.</span></span> <span data-ttu-id="666e1-106">管理タスクを自動化したいときは、Azure CLI を使用してスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="666e1-106">You'll use the Azure CLI to create scripts when you want to automate management tasks.</span></span>

<span data-ttu-id="666e1-107">Azure CLI を使用すると、Microsoft Azure 内のほぼすべてのリソースの作成を自動化できます。</span><span class="sxs-lookup"><span data-stu-id="666e1-107">Creating almost any resource within Microsoft Azure can be automated using the Azure CLI.</span></span> <span data-ttu-id="666e1-108">このユニットでは、Azure CLI を使用して Azure Database for PostgreSQL サーバーの管理を自動化する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="666e1-108">In this unit, you'll learn how to automate management of your Azure Database for PostgreSQL servers using the Azure CLI.</span></span>

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="666e1-109">Azure CLI とは</span><span class="sxs-lookup"><span data-stu-id="666e1-109">What is the Azure CLI?</span></span>

<span data-ttu-id="666e1-110">[Azure CLI](https://docs.microsoft.com/cli/azure/) は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン環境です。</span><span class="sxs-lookup"><span data-stu-id="666e1-110">The [Azure CLI](https://docs.microsoft.com/cli/azure/) is Microsoft’s cross-platform command-line environment for managing Azure resources.</span></span> <span data-ttu-id="666e1-111">ブラウザーから Azure Cloud Shell で Azure CLI を使用することも、Mac OS X、Linux、または Windows に Azure CLI をローカルにインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="666e1-111">You can use the Azure CLI from your browser with Azure Cloud Shell, or you can install the Azure CLI locally on Mac OS X, Linux, or Windows.</span></span> <span data-ttu-id="666e1-112">Azure CLI は、bash または PowerShell を使用してローカル コマンド ラインから実行します。</span><span class="sxs-lookup"><span data-stu-id="666e1-112">The Azure CLI is run from a local command line using bash or PowerShell.</span></span> <span data-ttu-id="666e1-113">Azure CLI をローカルに実行するには追加のセットアップが必要です。</span><span class="sxs-lookup"><span data-stu-id="666e1-113">Running the Azure CLI locally requires additional setup.</span></span> <span data-ttu-id="666e1-114">ここでは、Azure Cloud Shell を使用して Azure CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="666e1-114">We'll use Azure Cloud Shell for executing the Azure CLI commands.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="666e1-115">Azure Cloud Shell とは</span><span class="sxs-lookup"><span data-stu-id="666e1-115">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="666e1-116">Azure Cloud Shell はクラウドでホストされているブラウザー ベースのシェル エクスペリエンスであり、認証されたセッションを使用して Azure に接続することができます。</span><span class="sxs-lookup"><span data-stu-id="666e1-116">Azure Cloud Shell is a browser-based shell experience that's hosted in the cloud and allows you to connect to Azure using an authenticated session.</span></span> <span data-ttu-id="666e1-117">Azure CLI コマンドを実行して、Azure Database for PostgreSQL サーバーの管理を自動化できます。</span><span class="sxs-lookup"><span data-stu-id="666e1-117">You can execute the Azure CLI commands to automate the management of an Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="666e1-118">Cloud Shell には一般的な Azure CLI ツールが事前にインストールされており、アカウントで使用できるように構成されています。</span><span class="sxs-lookup"><span data-stu-id="666e1-118">Common Azure CLI tools are pre-installed and configured in Cloud Shell for you to use with your account.</span></span>

> [!NOTE]
> <span data-ttu-id="666e1-119">Cloud Shell には、Cloud Shell で作業中に作成するすべてのファイルを保持するための Azure ストレージ リソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="666e1-119">Cloud Shell requires an Azure storage resource to persist any files you create while working in Cloud Shell.</span></span> <span data-ttu-id="666e1-120">Cloud Shell の初回起動時に、リソース グループ、ストレージ アカウント、Azure Files 共有を作成するように求められます。</span><span class="sxs-lookup"><span data-stu-id="666e1-120">On first launch, Cloud Shell prompts to create a resource group, storage account, and Azure Files share on your behalf.</span></span> <span data-ttu-id="666e1-121">これは 1 回限りの作業であり、それ以降のすべての Cloud Shell セッションに対して自動的に接続されます。</span><span class="sxs-lookup"><span data-stu-id="666e1-121">This is a one-time step and will be automatically attached for all future Cloud Shell sessions.</span></span>

## <a name="create-an-azure-database-for-postgresql-server-using-the-azure-cli"></a><span data-ttu-id="666e1-122">Azure CLI を使用した Azure Database for PostgreSQL サーバーの作成</span><span class="sxs-lookup"><span data-stu-id="666e1-122">Create an Azure Database for PostgreSQL server using the Azure CLI</span></span>

<span data-ttu-id="666e1-123">右側の Azure Cloud Shell ターミナルで Azure CLI を使用して、Azure Database for PostgreSQL サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="666e1-123">You'll use the Azure Cloud Shell terminal on the right to create an Azure Database for PostgreSQL server using Azure CLI.</span></span>

<span data-ttu-id="666e1-124">使用可能なすべてのパラメーターが示される Azure CLI のサーバー作成コマンドの使用方法のヘルプは、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="666e1-124">The Azure CLI server creation command usage help showing all available parameters looks like the following example:</span></span>

```azurecli
az postgres server create [-h] [--verbose] [--debug]
                            [--output {json,jsonc,table,tsv}]
                            [--query JMESPATH]
                            --resource-group RESOURCE_GROUP_NAME --name SERVER_NAME
                            --sku-name SKU_NAME [--location LOCATION]
                            --admin-user ADMINISTRATOR_LOGIN
                            [--admin-password ADMINISTRATOR_LOGIN_PASSWORD]
                            [--backup-retention BACKUP_RETENTION]
                            [--geo-redundant-backup GEO_REDUNDANT_BACKUP]
                            [--ssl-enforcement {Enabled,Disabled}]
                            [--storage-size STORAGE_MB]
                            [--tags [TAGS [TAGS ...]]]
                            [--version VERSION]
                            [--subscription _SUBSCRIPTION]

```

<span data-ttu-id="666e1-125">省略可能なパラメーターは、角かっこで囲まれています。</span><span class="sxs-lookup"><span data-stu-id="666e1-125">The optional parameters are surrounded in brackets.</span></span> <span data-ttu-id="666e1-126">一般的なものをいくつか見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="666e1-126">Let's examine a few of the common ones.</span></span>

### <a name="parameters"></a><span data-ttu-id="666e1-127">パラメーター</span><span class="sxs-lookup"><span data-stu-id="666e1-127">Parameters</span></span>

<span data-ttu-id="666e1-128">`--resource-group <resource_group_name>` パラメーターでは、サーバーを作成するリソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="666e1-128">The `--resource-group <resource_group_name>` parameter specifies the resource group within which to create the server.</span></span>

<span data-ttu-id="666e1-129">指定するサーバーの `admin-user` と `admin-password` は、サーバーとそのデータベースにサインインするために必要です。</span><span class="sxs-lookup"><span data-stu-id="666e1-129">The server `admin-user` and `admin-password` that you specify is required to sign in to the server and its databases.</span></span> <span data-ttu-id="666e1-130">後で新しいサーバーとやりとりするときのために、この情報を憶えるか記録しておきます。</span><span class="sxs-lookup"><span data-stu-id="666e1-130">Remember or record this information for later when interacting with the new server.</span></span>

<span data-ttu-id="666e1-131">`--sku-name` パラメーターを使用して、価格レベルの部分を指定します (この場合はコンピューティング リソース)。</span><span class="sxs-lookup"><span data-stu-id="666e1-131">You use the `--sku-name` parameter to specify part of the pricing tier, in this case, compute resource.</span></span> <span data-ttu-id="666e1-132">値は `{pricing tier}_{compute generation}_{vCores}` という規則に従います。</span><span class="sxs-lookup"><span data-stu-id="666e1-132">The value follows the convention `{pricing tier}_{compute generation}_{vCores}`.</span></span>

<span data-ttu-id="666e1-133">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="666e1-133">Examples:</span></span>

- <span data-ttu-id="666e1-134">`--sku-name B_Gen4_4` は、"Basic、Gen 4、および 4 個の仮想コア" にマップされます。</span><span class="sxs-lookup"><span data-stu-id="666e1-134">`--sku-name B_Gen4_4` maps to Basic, Gen 4, and 4 vCores.</span></span>
- <span data-ttu-id="666e1-135">`--sku-name GP_Gen5_32` は、"汎用、Gen 5、および 32 個の仮想コア" にマップされます。</span><span class="sxs-lookup"><span data-stu-id="666e1-135">`--sku-name GP_Gen5_32` maps to General Purpose, Gen 5, and 32 vCores.</span></span>
- <span data-ttu-id="666e1-136">`--sku-name MO_Gen5_2` は、"メモリ最適化、Gen 5、および 2 個の仮想コア" にマップされます。</span><span class="sxs-lookup"><span data-stu-id="666e1-136">`--sku-name MO_Gen5_2` maps to Memory Optimized, Gen 5, and 2 vCores.</span></span>

<span data-ttu-id="666e1-137">ポータルを使用してサーバーを作成するユニットで 3 つの価格レベルを説明したことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="666e1-137">Recall that we discussed the three pricing tiers in the unit where we created the server using the portal.</span></span>

<span data-ttu-id="666e1-138">"Basic、Gen 5、1 仮想コア" のコンピューティング リソースを使用するものとします。</span><span class="sxs-lookup"><span data-stu-id="666e1-138">Let's assume you want to use a Basic, Gen 5, and 1 vCore compute resource.</span></span> <span data-ttu-id="666e1-139">その場合は、`--sku-name B_Gen5_1` というパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="666e1-139">You'll specify the parameter as `--sku-name B_Gen5_1`.</span></span>

<span data-ttu-id="666e1-140">価格レベルの部分を指定するには `--storage-size` パラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="666e1-140">You use the `--storage-size` parameter to specify part of the pricing tier.</span></span> <span data-ttu-id="666e1-141">値を指定しない場合の既定値は 5,120 MB です。</span><span class="sxs-lookup"><span data-stu-id="666e1-141">If the value isn't specified, then it defaults to 5,120 MB.</span></span> <span data-ttu-id="666e1-142">有効なストレージ サイズの範囲は 5,120 MB - 1,048,576 MB で、1,024 MB 刻みです。</span><span class="sxs-lookup"><span data-stu-id="666e1-142">Valid storage sizes range from 5,120 MB and increases in increments of 1,024 MB up to 1,048,576 MB.</span></span>

<span data-ttu-id="666e1-143">`--backup-retention` パラメーターは、バックアップの保有期間の日数を指定する必要があるときに使用します。</span><span class="sxs-lookup"><span data-stu-id="666e1-143">The `--backup-retention` parameter is used when you need to specify the retention period for backups, which is specified in days.</span></span> <span data-ttu-id="666e1-144">値を指定しない場合の既定値は 7 日です。</span><span class="sxs-lookup"><span data-stu-id="666e1-144">If the value isn't specified, then it defaults to seven days.</span></span>

<span data-ttu-id="666e1-145">`--version` パラメーターは、使用する PostgreSQL のメジャー バージョンを指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="666e1-145">You use the `--version` parameter to specify the major version of PostgreSQL that you'd like to use.</span></span>

<span data-ttu-id="666e1-146">ここまでは、Azure CLI を使用して Azure Database for PostgreSQL サーバーを作成する手順を説明しました。</span><span class="sxs-lookup"><span data-stu-id="666e1-146">You've now seen the steps to create an Azure Database for PostgreSQL server using the Azure CLI.</span></span> <span data-ttu-id="666e1-147">次のユニットでは、Azure CLI を使用して Azure Database for PostgreSQL サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="666e1-147">In the next unit, you'll create an Azure Database for PostgreSQL server using the Azure CLI.</span></span>