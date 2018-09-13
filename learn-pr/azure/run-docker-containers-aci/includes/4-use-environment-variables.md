<span data-ttu-id="09231-101">Container Instances で環境変数を設定すると、コンテナーによって実行されるアプリケーションまたはスクリプトの動的な構成を提供できます。</span><span class="sxs-lookup"><span data-stu-id="09231-101">Setting environment variables in your container instances allows you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="09231-102">環境変数の設定は、コンテナーの作成時に Azure CLI、PowerShell、または Azure portal を使用して行います。</span><span class="sxs-lookup"><span data-stu-id="09231-102">Environment variables are set using the Azure CLI, PowerShell, or the Azure portal at container creation time.</span></span> <span data-ttu-id="09231-103">コンテナー操作の出力に機密情報が表示されるのを防ぐには、セキュリティで保護された環境変数を使います。</span><span class="sxs-lookup"><span data-stu-id="09231-103">Secured environment variables are used to prevent sensitive information from being displayed in container operations output.</span></span>

<span data-ttu-id="09231-104">このユニットでは、Azure Cosmos DB インスタンスを作成した後、環境変数として格納されている Azure Cosmos DB インスタンスの接続情報を使用して Azure コンテナー インスタンスを実行します。</span><span class="sxs-lookup"><span data-stu-id="09231-104">In this unit, an Azure Cosmos DB instance is created, and then an Azure container instance runs with the connection information of the Azure Cosmos DB instance stored as environment variables.</span></span> <span data-ttu-id="09231-105">コンテナー内のアプリケーションでは、変数を使用して Azure Cosmos DB のデータを読み書きします。</span><span class="sxs-lookup"><span data-stu-id="09231-105">An application in the container uses the variables to write and read data from Azure Cosmos DB.</span></span> <span data-ttu-id="09231-106">このユニットでは、環境変数とセキュリティで保護された環境変数の両方を作成します。</span><span class="sxs-lookup"><span data-stu-id="09231-106">In this unit, you will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="09231-107">Azure Cosmos DB をデプロイする</span><span class="sxs-lookup"><span data-stu-id="09231-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="09231-108">`az Azure Cosmos DB create` コマンドを使用して Azure Cosmos DB インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="09231-108">Create the Azure Cosmos DB instance with the `az Azure Cosmos DB create` command.</span></span> <span data-ttu-id="09231-109">また、この例では、Azure Cosmos DB のエンドポイント アドレスを *COSMOS_DB_ENDPOINT* という名前の変数に配置します。</span><span class="sxs-lookup"><span data-stu-id="09231-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span>

<span data-ttu-id="09231-110">このコマンドが完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="09231-110">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group myResourceGroup --name aci-cosmos --query documentEndpoint -o tsv)
```

<span data-ttu-id="09231-111">次に、`az cosmosdb list-keys` コマンドを使用して Azure Cosmos DB の接続キーを取得し、*COSMOS_DB_MASTERKEY* という名前の変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="09231-111">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group myResourceGroup --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="09231-112">コンテナー インスタンスをデプロイする</span><span class="sxs-lookup"><span data-stu-id="09231-112">Deploy a container instance</span></span>

<span data-ttu-id="09231-113">`az container create` コマンドを使用して、Azure コンテナー インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="09231-113">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="09231-114">`COSMOS_DB_ENDPOINT` および `COSMOS_DB_ENDPOINT` という 2 つの環境変数が作成されていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="09231-114">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_ENDPOINT`.</span></span> <span data-ttu-id="09231-115">これらの変数には、Azure Cosmos DB インスタンスに接続するために必要な値が保持されています。</span><span class="sxs-lookup"><span data-stu-id="09231-115">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="09231-116">コンテナーが作成された後、`az container show` コマンドを使用して IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="09231-116">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query ipAddress.ip --output tsv
```

<span data-ttu-id="09231-117">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="09231-117">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="09231-118">次のアプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09231-118">You should see the following application.</span></span> <span data-ttu-id="09231-119">投票を行うと、Azure Cosmos DB インスタンスに投票が格納されます。</span><span class="sxs-lookup"><span data-stu-id="09231-119">When casing a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![猫または犬という 2 つの選択肢がある Azure 投票アプリケーション。](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="09231-121">セキュリティで保護された環境変数</span><span class="sxs-lookup"><span data-stu-id="09231-121">Secured environment variables</span></span>

<span data-ttu-id="09231-122">上の演習では、コンテナーは 2 つの環境変数に格納されている Azure Cosmos DB に対する接続情報を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="09231-122">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="09231-123">既定では、環境変数は Azure portal とコマンド ライン ツールにプレーン テキストで表示されます。</span><span class="sxs-lookup"><span data-stu-id="09231-123">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="09231-124">たとえば、前の演習で作成したコンテナーに関する情報を `az container show` コマンドで取得する場合、環境変数にはプレーン テキストでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="09231-124">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="09231-125">出力例:</span><span class="sxs-lookup"><span data-stu-id="09231-125">Example output:</span></span>

```bash
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

セキュリティで保護された環境変数を使用するとクリア テキストで出力されなくなります。 <span data-ttu-id="09231-127">セキュリティで保護された環境変数を使用するには、`--environment-variables` 引数を `--secure-environment-variables` 引数に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="09231-127">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="09231-128">セキュリティで保護された環境変数を使用する *aci-demo-secure* という名前のコンテナーを作成するには、次の例を実行します。</span><span class="sxs-lookup"><span data-stu-id="09231-128">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="09231-129">コンテナーが `az container show` コマンドで返されたとき、環境変数は表示されません。</span><span class="sxs-lookup"><span data-stu-id="09231-129">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-secure --query containers[0].environmentVariables
```

<span data-ttu-id="09231-130">出力例:</span><span class="sxs-lookup"><span data-stu-id="09231-130">Example output:</span></span>

```bash
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

## <a name="summary"></a><span data-ttu-id="09231-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="09231-131">Summary</span></span>

<span data-ttu-id="09231-132">このユニットでは、Azure Cosmos DB インスタンスと Azure コンテナー インスタンスを作成しました。</span><span class="sxs-lookup"><span data-stu-id="09231-132">In this unit, you created an Azure Cosmos DB instance and an Azure container instance.</span></span> <span data-ttu-id="09231-133">コンテナー内で実行されているアプリケーションが Azure Cosmos DB インスタンスに接続できるように、コンテナー インスタンスで環境変数を設定しました。</span><span class="sxs-lookup"><span data-stu-id="09231-133">Environment variables were set in the container instance such that the application running inside of the container was able to connect to the Azure Cosmos DB instance.</span></span>

<span data-ttu-id="09231-134">次のユニットでは、データの永続化のために Azure コンテナー インスタンスにデータ ボリュームをマウントします。</span><span class="sxs-lookup"><span data-stu-id="09231-134">In the next unit, you will mount data volumes to an Azure container instance for data persistence.</span></span>
