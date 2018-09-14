<span data-ttu-id="6db73-101">Container instances で環境変数を使用すると、アプリケーションまたはコンテナーによって実行されるスクリプトの動的な構成を提供できます。</span><span class="sxs-lookup"><span data-stu-id="6db73-101">Environment variables in your container instances allow you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="6db73-102">Azure CLI、PowerShell、または Azure portal を使用して、コンテナーを作成するときに変数を設定します。</span><span class="sxs-lookup"><span data-stu-id="6db73-102">You use the Azure CLI, PowerShell, or the Azure portal to set variables when you create the container.</span></span> <span data-ttu-id="6db73-103">コンテナー操作の出力に機密情報が表示されるのを防ぐには、セキュリティで保護された環境変数を使います。</span><span class="sxs-lookup"><span data-stu-id="6db73-103">Secured environment variables are used to prevent sensitive information from being displayed in container operations output.</span></span>

<span data-ttu-id="6db73-104">ここでは、Azure Cosmos DB インスタンスと Azure コンテナー インスタンスに接続情報を渡す使用の環境変数を作成します。</span><span class="sxs-lookup"><span data-stu-id="6db73-104">Here, you will create an Azure Cosmos DB instance and use environment variables to pass the connection information to an Azure container instance.</span></span> <span data-ttu-id="6db73-105">コンテナー内のアプリケーションでは、Cosmos DB からデータを読み書きする変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="6db73-105">An application in the container uses the variables to write and read data from Cosmos DB.</span></span> <span data-ttu-id="6db73-106">環境変数と、セキュリティで保護された環境変数の両方を作成します。</span><span class="sxs-lookup"><span data-stu-id="6db73-106">You will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="6db73-107">Azure Cosmos DB をデプロイする</span><span class="sxs-lookup"><span data-stu-id="6db73-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="6db73-108">`az Azure Cosmos DB create` コマンドを使用して Azure Cosmos DB インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6db73-108">Create the Azure Cosmos DB instance with the `az Azure Cosmos DB create` command.</span></span> <span data-ttu-id="6db73-109">また、この例では、Azure Cosmos DB のエンドポイント アドレスを *COSMOS_DB_ENDPOINT* という名前の変数に配置します。</span><span class="sxs-lookup"><span data-stu-id="6db73-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span>

<span data-ttu-id="6db73-110">このコマンドが完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="6db73-110">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query documentEndpoint -o tsv)
```

<span data-ttu-id="6db73-111">次に、`az cosmosdb list-keys` コマンドを使用して Azure Cosmos DB の接続キーを取得し、*COSMOS_DB_MASTERKEY* という名前の変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="6db73-111">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="6db73-112">コンテナー インスタンスをデプロイする</span><span class="sxs-lookup"><span data-stu-id="6db73-112">Deploy a container instance</span></span>

<span data-ttu-id="6db73-113">`az container create` コマンドを使用して、Azure コンテナー インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="6db73-113">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="6db73-114">`COSMOS_DB_ENDPOINT` および `COSMOS_DB_ENDPOINT` という 2 つの環境変数が作成されていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="6db73-114">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_ENDPOINT`.</span></span> <span data-ttu-id="6db73-115">これらの変数には、Azure Cosmos DB インスタンスに接続するために必要な値が保持されています。</span><span class="sxs-lookup"><span data-stu-id="6db73-115">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="6db73-116">コンテナーが作成された後、`az container show` コマンドを使用して IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="6db73-116">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query ipAddress.ip --output tsv
```

<span data-ttu-id="6db73-117">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="6db73-117">Open a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="6db73-118">次のアプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6db73-118">You should see the following application.</span></span> <span data-ttu-id="6db73-119">投票をキャストすると、投票は、Azure Cosmos DB インスタンスに格納されます。</span><span class="sxs-lookup"><span data-stu-id="6db73-119">When casting a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![猫または犬という 2 つの選択肢がある Azure 投票アプリケーション。](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="6db73-121">セキュリティで保護された環境変数</span><span class="sxs-lookup"><span data-stu-id="6db73-121">Secured environment variables</span></span>

<span data-ttu-id="6db73-122">上の演習では、コンテナーは 2 つの環境変数に格納されている Azure Cosmos DB に対する接続情報を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="6db73-122">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="6db73-123">既定では、環境変数は Azure portal とコマンド ライン ツールにプレーン テキストで表示されます。</span><span class="sxs-lookup"><span data-stu-id="6db73-123">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="6db73-124">たとえば、前の演習で作成したコンテナーに関する情報を `az container show` コマンドで取得する場合、環境変数にはプレーン テキストでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="6db73-124">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="6db73-125">出力例:</span><span class="sxs-lookup"><span data-stu-id="6db73-125">Example output:</span></span>

```json
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": "https://aci-cosmos.documents.azure.com:443/"
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": "Xm5BwdLlCllBvrR26V00000000S2uOusuglhzwkE7dOPMBQ3oA30n3rKd8PKA13700000000095ynys863Ghgw=="
  }
]
```

セキュリティで保護された環境変数を使用するとクリア テキストで出力されなくなります。 <span data-ttu-id="6db73-127">セキュリティで保護された環境変数を使用するには、`--environment-variables` 引数を `--secure-environment-variables` 引数に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6db73-127">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="6db73-128">セキュリティで保護された環境変数を使用する *aci-demo-secure* という名前のコンテナーを作成するには、次の例を実行します。</span><span class="sxs-lookup"><span data-stu-id="6db73-128">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="6db73-129">コンテナーが `az container show` コマンドで返されたとき、環境変数は表示されません。</span><span class="sxs-lookup"><span data-stu-id="6db73-129">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show --resource-group <rgn>[Sandbox resource group name]</rgn> --name aci-demo-secure --query containers[0].environmentVariables
```

<span data-ttu-id="6db73-130">出力例:</span><span class="sxs-lookup"><span data-stu-id="6db73-130">Example output:</span></span>

```json
[
  {
    "name": "COSMOS_DB_ENDPOINT",
    "secureValue": null,
    "value": null
  },
  {
    "name": "COSMOS_DB_MASTERKEY",
    "secureValue": null,
    "value": null
  }
]
```