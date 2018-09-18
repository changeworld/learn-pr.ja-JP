<span data-ttu-id="898b5-101">あなたはオンプレミスの PostgreSQL データベースを使っているものとしましょう。</span><span class="sxs-lookup"><span data-stu-id="898b5-101">Let's assume you're using an on-premises PostgreSQL database.</span></span> <span data-ttu-id="898b5-102">あなたの会社は、サーバーを Azure に移動することによって、デバイスのサポート、可用性、データの追跡、および処理の機能を拡張しようとしています。</span><span class="sxs-lookup"><span data-stu-id="898b5-102">Your company is now looking at expanding device support, availability, data tracking, and processing features by moving your server into Azure.</span></span> <span data-ttu-id="898b5-103">あなたは、Azure Database for PostgreSQL の作成の自動化にかかる労力を調べています。</span><span class="sxs-lookup"><span data-stu-id="898b5-103">You'll investigate how much effort it takes to automate the creation of an Azure Database for PostgreSQL.</span></span>

<span data-ttu-id="898b5-104">Azure portal を使用して作成する Azure Database for PostgreSQL サーバーが 1 つであれば難しくありません。</span><span class="sxs-lookup"><span data-stu-id="898b5-104">Creating a single Azure Database for PostgreSQL server using the Azure portal is easy.</span></span> <span data-ttu-id="898b5-105">複数のデータベースの作成と継続的なメンテナンスをポータルのみを使用して行おうとすると、面倒になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="898b5-105">Creating more than one database and running ongoing maintenance using only the portal may become tedious.</span></span> <span data-ttu-id="898b5-106">管理タスクを自動化したいときは、Azure CLI を使用してスクリプトを作成します。</span><span class="sxs-lookup"><span data-stu-id="898b5-106">You'll use the Azure CLI to create scripts when you want to automate management tasks.</span></span>

<span data-ttu-id="898b5-107">Azure CLI を使用すると、Microsoft Azure 内のほぼすべてのリソースの作成を自動化できます。</span><span class="sxs-lookup"><span data-stu-id="898b5-107">Creating almost any resource within Microsoft Azure can be automated using the Azure CLI.</span></span> <span data-ttu-id="898b5-108">このユニットでは、Azure CLI を使用して Azure Database for PostgreSQL サーバーの管理を自動化する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="898b5-108">In this unit, you'll learn how to automate management of your Azure Database for PostgreSQL servers using the Azure CLI.</span></span>

## <a name="what-is-azure-cli"></a><span data-ttu-id="898b5-109">Azure CLI とは</span><span class="sxs-lookup"><span data-stu-id="898b5-109">What is Azure CLI?</span></span>

<span data-ttu-id="898b5-110">[Azure CLI](https://docs.microsoft.com/cli/azure/) は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン環境です。</span><span class="sxs-lookup"><span data-stu-id="898b5-110">[Azure CLI](https://docs.microsoft.com/cli/azure/) is Microsoft’s cross-platform command-line environment for managing Azure resources.</span></span> <span data-ttu-id="898b5-111">ブラウザーから Azure Cloud Shell で Azure CLI を使用することも、Mac OS X、Linux、または Windows に Azure CLI をローカルにインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="898b5-111">You can use the Azure CLI from your browser with Azure Cloud Shell, or you can install Azure CLI locally on Mac OS X, Linux, or Windows.</span></span> <span data-ttu-id="898b5-112">Azure CLI は、bash または Powershell を使用してローカル コマンド ラインから実行します。</span><span class="sxs-lookup"><span data-stu-id="898b5-112">The Azure CLI is run from a local command line using bash or Powershell.</span></span> <span data-ttu-id="898b5-113">ただし、Azure CLI をローカルに実行するには追加のセットアップが必要です。</span><span class="sxs-lookup"><span data-stu-id="898b5-113">Running Azure CLI locally however requires additional setup.</span></span> <span data-ttu-id="898b5-114">ここでは、Azure Cloud Shell を使用して Azure CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="898b5-114">We'll use the Azure Cloud Shell for executing Azure CLI commands.</span></span>

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="898b5-115">Azure Cloud Shell とは</span><span class="sxs-lookup"><span data-stu-id="898b5-115">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="898b5-116">Azure Cloud Shell はクラウドでホストされているブラウザー ベースのシェル エクスペリエンスであり、認証されたセッションを使用して Azure に接続することができます。</span><span class="sxs-lookup"><span data-stu-id="898b5-116">Azure Cloud Shell is a browser-based shell experience that is hosted in the cloud and allows you to connect to Azure using an authenticated session.</span></span> <span data-ttu-id="898b5-117">Azure CLI コマンドを実行して、Azure Database for PostgreSQL の管理を自動化できます。</span><span class="sxs-lookup"><span data-stu-id="898b5-117">You can execute Azure CLI commands to automate the management of an Azure Database for PostgreSQL.</span></span> <span data-ttu-id="898b5-118">Cloud Shell には一般的な Azure CLI ツールが事前にインストールされており、アカウントで使用できるように構成されています。</span><span class="sxs-lookup"><span data-stu-id="898b5-118">Common Azure CLI tools are pre-installed and configured in Cloud Shell for you to use with your account.</span></span>

> [!NOTE]
> <span data-ttu-id="898b5-119">Cloud Shell には、Cloud Shell で作業中に作成するすべてのファイルを保持するための Azure ストレージ リソースが必要です。</span><span class="sxs-lookup"><span data-stu-id="898b5-119">Cloud Shell requires an Azure storage resource to persist any files you create while working in the Cloud Shell.</span></span> <span data-ttu-id="898b5-120">Cloud Shell の初回起動時に、リソース グループ、ストレージ アカウント、Azure Files 共有を作成するように求められます。</span><span class="sxs-lookup"><span data-stu-id="898b5-120">On first launch Cloud Shell prompts to create a resource group, storage account, and Azure Files share on your behalf.</span></span> <span data-ttu-id="898b5-121">これは 1 回限りの作業であり、それ以降のすべての Cloud Shell セッションに対して自動的に接続されます。</span><span class="sxs-lookup"><span data-stu-id="898b5-121">This is a one-time step and will be automatically attached for all future Cloud Shell sessions.</span></span>

## <a name="create-an-azure-database-for-postgresql-server-using-azure-cli"></a><span data-ttu-id="898b5-122">Azure CLI を使用して Azure Database for PostgreSQL サーバーを作成する</span><span class="sxs-lookup"><span data-stu-id="898b5-122">Create an Azure Database for PostgreSQL server using Azure CLI</span></span>

<span data-ttu-id="898b5-123">Azure Cloud Shell で Azure CLI を使用して、Azure Database for PostgreSQL サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="898b5-123">You'll use Azure Cloud Shell to create an Azure Database for PostgreSQL server using Azure CLI.</span></span> <span data-ttu-id="898b5-124">その手順を見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="898b5-124">Let's look at the steps you'll take.</span></span>

<span data-ttu-id="898b5-125">まず、Azure portal にサインインします。</span><span class="sxs-lookup"><span data-stu-id="898b5-125">First, sign into the Azure portal.</span></span>

<span data-ttu-id="898b5-126">Azure portal から Cloud Shell を開きます。</span><span class="sxs-lookup"><span data-stu-id="898b5-126">Open the Cloud Shell from the Azure portal.</span></span> <span data-ttu-id="898b5-127">ブラウザーを開いて [Azure portal](https://portal.azure.com?azure-portal=true) に移動し、[Open Cloud Shell]\(Cloud Shell を開く\) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="898b5-127">Open your browser and go to [Azure portal](https://portal.azure.com?azure-portal=true) and click the Open Cloud Shell button:</span></span>

![Cloud Shell ボタン](../media-draft/cloud-shell-button.png)

<span data-ttu-id="898b5-129">Cloud Shell では、`bash` または `PowerShell` のいずれかでコマンドを実行できます。</span><span class="sxs-lookup"><span data-stu-id="898b5-129">Cloud Shell allows you to run your commands either in `bash` or `PowerShell`.</span></span> <span data-ttu-id="898b5-130">すべての例で、`bash` コマンド ライン オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="898b5-130">We'll use the `bash` command-line option for all examples.</span></span>

<span data-ttu-id="898b5-131">複数のサブスクリプションがある場合は、次のコマンドを使用して適切なサブスクリプションをアクティブにします。0 はお使いのサブスクリプション ID に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="898b5-131">If you have several subscriptions, make sure you activate the appropriate subscription with the following command, replacing the zeros with your subscription identifier.</span></span>

   ```bash
   az account set --subscription 00000000-0000-0000-0000-000000000000
   ```

<span data-ttu-id="898b5-132">自分のすべてのサブスクリプションを一覧表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="898b5-132">You'll run the following command to list all your subscriptions.</span></span>

   ```bash
   az account list --output table
   ```

<span data-ttu-id="898b5-133">次に、サーバーを管理するためのリソース グループと、リソース グループを配置する場所を作成します。</span><span class="sxs-lookup"><span data-stu-id="898b5-133">The next step is to create a resource group to manage the server and where the resource group will be located.</span></span> <span data-ttu-id="898b5-134">リソース グループはサーバーに関連するすべてのリソースを管理するために使用することを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="898b5-134">Recall, you'll use a resource group to manage all the resources related to your server.</span></span> <span data-ttu-id="898b5-135">場所のオプションでは、サーバーが物理的に作成される場所を指定できます。</span><span class="sxs-lookup"><span data-stu-id="898b5-135">The location option allows you to specify where the server is created physically.</span></span> <span data-ttu-id="898b5-136">次のコマンドの `<resource_group_name>` と `<location>` をそれぞれを適切な値に置き換えて実行します。</span><span class="sxs-lookup"><span data-stu-id="898b5-136">You'll run the next command and replace the `<resource_group_name>` and `<location>` respectively with appropriate values.</span></span>

   ```bash
   az group create --name <resourcegroup> --location <location>
   ```

<span data-ttu-id="898b5-137">最後に、Azure Database for PostgreSQL サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="898b5-137">The last step is to create the Azure Database for PostgreSQL server.</span></span>

   <span data-ttu-id="898b5-138">使用可能なすべてのパラメーターが示される Azure CLI のサーバー作成コマンドの使用方法のヘルプは、次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="898b5-138">The Azure CLI server creation command usage help showing all available parameters looks like the following example:</span></span>

   ```bash
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

   <span data-ttu-id="898b5-139">次のコマンド ラインでは、Azure Database for PostgreSQL サーバーを作成するために必要なパラメーターのセットを示します。</span><span class="sxs-lookup"><span data-stu-id="898b5-139">The following command line shows the required set of parameters to create an Azure Database for PostgreSQL server.</span></span> <span data-ttu-id="898b5-140">一部の省略可能なパラメーターは表示されていないことがわかります。</span><span class="sxs-lookup"><span data-stu-id="898b5-140">You'll notice some are optional parameters and aren't listed.</span></span>

   ```bash
   az postgres server create --resource-group <resource_group_name> --name <new_server_name> --admin-user <admin_user_name> --admin-password <server_admin_password> --sku-name <sku> --version <version_number>  --location <region_name> --storage-size <size> --backup-retention <days>
   ```

### <a name="parameter-descriptions"></a><span data-ttu-id="898b5-141">パラメーターの説明</span><span class="sxs-lookup"><span data-stu-id="898b5-141">Parameter descriptions</span></span>

<span data-ttu-id="898b5-142">`--resource-group <resource_group_name>` パラメーターは、サーバーを作成するリソース グループを指定します。</span><span class="sxs-lookup"><span data-stu-id="898b5-142">The `--resource-group <resource_group_name>` parameter specifies the resource group within which to create the server.</span></span>

<span data-ttu-id="898b5-143">指定するサーバーの `admin-user` と `admin-password` は、サーバーとそのデータベースにサインインするために必要です。</span><span class="sxs-lookup"><span data-stu-id="898b5-143">The server `admin-user` and `admin-password` that you specify is required to sign in to the server and its databases.</span></span> <span data-ttu-id="898b5-144">後で新しいサーバーとやりとりするときのために、この情報を憶えるか記録しておきます。</span><span class="sxs-lookup"><span data-stu-id="898b5-144">Remember or record this information for later when interacting with the new server.</span></span>

<span data-ttu-id="898b5-145">`--sku-name` パラメーターは、価格レベルの部分を指定するために使用します (この場合はコンピューティング リソース)。</span><span class="sxs-lookup"><span data-stu-id="898b5-145">You use the `--sku-name` parameter is used to specify part of the pricing tier, in this case compute resource.</span></span> <span data-ttu-id="898b5-146">値は `{pricing tier}_{compute generation}_{vCores}` という規則に従います。</span><span class="sxs-lookup"><span data-stu-id="898b5-146">The value follows the convention `{pricing tier}_{compute generation}_{vCores}`.</span></span>

<span data-ttu-id="898b5-147">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="898b5-147">Examples:</span></span>

- <span data-ttu-id="898b5-148">`--sku-name B_Gen4_4` は、"Basic、Gen 4、および 4 個の仮想コア" にマップされます。</span><span class="sxs-lookup"><span data-stu-id="898b5-148">`--sku-name B_Gen4_4` maps to Basic, Gen 4, and 4 vCores.</span></span>
- <span data-ttu-id="898b5-149">`--sku-name GP_Gen5_32` は、"汎用、Gen 5、および 32 個の仮想コア" にマップされます。</span><span class="sxs-lookup"><span data-stu-id="898b5-149">`--sku-name GP_Gen5_32` maps to General Purpose, Gen 5, and 32 vCores.</span></span>
- <span data-ttu-id="898b5-150">`--sku-name MO_Gen5_2` は、"メモリ最適化、Gen 5、および 2 個の仮想コア" にマップされます。</span><span class="sxs-lookup"><span data-stu-id="898b5-150">`--sku-name MO_Gen5_2` maps to Memory Optimized, Gen 5, and 2 vCores.</span></span>

<span data-ttu-id="898b5-151">ポータルを使用してサーバーを作成するユニットで 3 つの価格レベルを説明したことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="898b5-151">Recall, we discussed the three pricing tiers in the unit where we create the server using the portal.</span></span>

<span data-ttu-id="898b5-152">"Basic、Gen 5、1 仮想コア" のコンピューティング リソースを使用する場合は、`--sku-name B_Gen5_1` というパラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="898b5-152">Let's assume you want to use a Basic, Gen 5, and 1 vCore compute resource, you'll then specify the parameter as `--sku-name B_Gen5_1`.</span></span>

<span data-ttu-id="898b5-153">価格レベルの部分を指定するには `--storage-size` パラメーターも使用します。</span><span class="sxs-lookup"><span data-stu-id="898b5-153">You use the `--storage-size` parameter is also used the specify part of the pricing tier.</span></span> <span data-ttu-id="898b5-154">値を指定しない場合の既定値は 5,120 MB です。</span><span class="sxs-lookup"><span data-stu-id="898b5-154">If the value isn't specified, then it defaults to 5,120 MB.</span></span> <span data-ttu-id="898b5-155">有効なストレージ サイズの範囲は 5,120 MB - 1,048,576 MB で、1,024 MB 刻みです。</span><span class="sxs-lookup"><span data-stu-id="898b5-155">Valid storage sizes range from 5,120 MB and increases in additional increments of 1,024 MB up to 1,048,576 MB.</span></span>

<span data-ttu-id="898b5-156">`--backup-retention` パラメーターは、バックアップの保有期間の日数を指定する必要があるときに使用します。</span><span class="sxs-lookup"><span data-stu-id="898b5-156">The `--backup-retention` parameter is used when you need to specify the retention period for backups specified in days.</span></span> <span data-ttu-id="898b5-157">値を指定しない場合の既定値は 7 日です。</span><span class="sxs-lookup"><span data-stu-id="898b5-157">If the value isn't specified, then it defaults to seven days.</span></span>

<span data-ttu-id="898b5-158">`--version` パラメーターは、使用する PostgreSQL のメジャー バージョンを指定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="898b5-158">You use the `--version` parameter is used to specify the major version of PostgreSQL you would like to use.</span></span>

<span data-ttu-id="898b5-159">ここまでは、Azure CLI を使用して Azure Database for PostgreSQL を作成する手順を説明しました。</span><span class="sxs-lookup"><span data-stu-id="898b5-159">You've now seen the steps to create an Azure Database for PostgreSQL using Azure CLI.</span></span> <span data-ttu-id="898b5-160">次のユニットでは、Azure CLI を使用して Azure Database for PostgreSQL サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="898b5-160">In the next unit, you'll create an Azure Database for PostgreSQL server using Azure CLI.</span></span>