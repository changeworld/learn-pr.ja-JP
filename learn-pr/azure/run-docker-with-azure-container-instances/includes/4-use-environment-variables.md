<span data-ttu-id="a20a4-101">Container Instances で環境変数を設定すると、コンテナーによって実行されるアプリケーションまたはスクリプトの動的な構成を提供できます。</span><span class="sxs-lookup"><span data-stu-id="a20a4-101">Setting environment variables in your container instances allows you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="a20a4-102">環境変数の設定は、コンテナーの作成時に Azure CLI、PowerShell、または Azure portal を使用して行います。</span><span class="sxs-lookup"><span data-stu-id="a20a4-102">Environment variables are set using the Azure CLI, PowerShell, or the Azure portal at container creation time.</span></span> <span data-ttu-id="a20a4-103">コンテナー操作の出力に機密情報が表示されるのを防ぐには、セキュリティで保護された環境変数を使います。</span><span class="sxs-lookup"><span data-stu-id="a20a4-103">Secured environment variables are used to prevent sensitive information from being displayed in container operations output.</span></span>

<span data-ttu-id="a20a4-104">このユニットでは、Azure Cosmos DB インスタンスが作成され、その後に Azure コンテナー インスタンスが環境変数として格納された Azure Cosmos DB インスタンスの接続情報を使って実行されます。</span><span class="sxs-lookup"><span data-stu-id="a20a4-104">In this unit, an Azure Cosmos DB instance is created, and then an Azure container instance runs with the connection information of the Azure Cosmos DB instance stored as environment variables.</span></span> <span data-ttu-id="a20a4-105">コンテナー内のアプリケーションは、Azure Cosmos DB からのデータの読み書きに変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-105">An application in the container uses the variables to write and read data from Azure Cosmos DB.</span></span> <span data-ttu-id="a20a4-106">このユニットでは、環境変数と、セキュアな環境変数の双方を作成します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-106">In this unit, you will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="a20a4-107">Azure Cosmos DB の展開</span><span class="sxs-lookup"><span data-stu-id="a20a4-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="a20a4-108">`az Azure Cosmos DB create` コマンドを使用して Azure Cosmos DB インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-108">Create the Azure Cosmos DB instance with the `az Azure Cosmos DB create` command.</span></span> <span data-ttu-id="a20a4-109">また、この例では、Azure Cosmos DB のエンドポイント アドレス *COSMOS_DB_ENDPOINT* という名前の変数に設定します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span>

<span data-ttu-id="a20a4-110">このコマンドの完了には数分を要することがあります。</span><span class="sxs-lookup"><span data-stu-id="a20a4-110">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group myResourceGroup --name aci-cosmos --query documentEndpoint -o tsv)
```

<span data-ttu-id="a20a4-111">次に、`az cosmosdb list-keys` のコマンドを使って Azure Cosmos DB 接続キーを取得し、*COSMOS_DB_MASTERKEY* という名の変数内に保存します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-111">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group myResourceGroup --name aci-cosmos --query primaryMasterKey -o tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="a20a4-112">コンテナー インスタンスの展開</span><span class="sxs-lookup"><span data-stu-id="a20a4-112">Deploy a container instance</span></span>

<span data-ttu-id="a20a4-113">`az container create` のコマンドを使って Azure コンテナー インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-113">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="a20a4-114">2 つの環境変数の値 `COSMOS_DB_ENDPOINT` と `COSMOS_DB_ENDPOINT` が作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-114">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_ENDPOINT`.</span></span> <span data-ttu-id="a20a4-115">これらの変数は、Azure Cosmos DB への接続に必要な値を保持します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-115">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="a20a4-116">コンテナーが作成されたら、`az container show` のコマンドを使って IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-116">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query ipAddress.ip --output tsv
```

<span data-ttu-id="a20a4-117">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-117">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="a20a4-118">次のアプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="a20a4-118">You should see the following application.</span></span> <span data-ttu-id="a20a4-119">投票を行うと、投票は Azure Cosmos DB インスタンス内に保存されます。</span><span class="sxs-lookup"><span data-stu-id="a20a4-119">When casing a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![猫または犬の 2 つの選択肢がある Azure 投票アプリケーション。](../media-draft/azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="a20a4-121">セキュリティで保護された環境変数</span><span class="sxs-lookup"><span data-stu-id="a20a4-121">Secured environment variables</span></span>

<span data-ttu-id="a20a4-122">上の演習では、コンテナーは 2 つの環境変数に格納されている Azure Cosmos DB に対する接続情報を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="a20a4-122">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="a20a4-123">デフォルトでは、環境変数は Azure portal およびコマンドライン ツールに平文で表示されます。</span><span class="sxs-lookup"><span data-stu-id="a20a4-123">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="a20a4-124">たとえば、`az container show` のコマンドを使って前回の練習で作成されたコンテナーに関する情報を取得する場合、環境変数は平文でアクセス可能となります。</span><span class="sxs-lookup"><span data-stu-id="a20a4-124">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="a20a4-125">出力例:</span><span class="sxs-lookup"><span data-stu-id="a20a4-125">Example output:</span></span>

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

セキュリティで保護された環境変数を使用するとクリア テキストで出力されなくなります。 <span data-ttu-id="a20a4-127">セキュリティで保護された環境変数を使用するには、`--environment-variables` 引数を `--secure-environment-variables` 引数に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="a20a4-127">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="a20a4-128">次の例を実行して、セキュリティで保護された環境変数を利用する *aci-demo-secure* という名前のコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-128">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="a20a4-129">これで、`az container show` のコマンドを使ってコンテナーが返される際、環境変数は表示されなくなります。</span><span class="sxs-lookup"><span data-stu-id="a20a4-129">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-secure --query containers[0].environmentVariables
```

<span data-ttu-id="a20a4-130">出力例:</span><span class="sxs-lookup"><span data-stu-id="a20a4-130">Example output:</span></span>

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

## <a name="summary"></a><span data-ttu-id="a20a4-131">まとめ</span><span class="sxs-lookup"><span data-stu-id="a20a4-131">Summary</span></span>

<span data-ttu-id="a20a4-132">このユニットでは、Azure Cosmos DB インスタンスおよび Azure コンテナー インスタンスの作成を行いました。</span><span class="sxs-lookup"><span data-stu-id="a20a4-132">In this unit, you created an Azure Cosmos DB instance and an Azure container instance.</span></span> <span data-ttu-id="a20a4-133">環境変数はコンテナー インスタンス内にセットされ、コンテナー内で実行されるアプリケーションが Azure Cosmos DB インスタンスにアクセスできるようになっていました。</span><span class="sxs-lookup"><span data-stu-id="a20a4-133">Environment variables were set in the container instance such that the application running inside of the container was able to connect to the Azure Cosmos DB instance.</span></span>

<span data-ttu-id="a20a4-134">次のユニットでは、データ保持のために Azure コンテナー インスタンスへデータ ボリュームをマウントする方法を紹介します。</span><span class="sxs-lookup"><span data-stu-id="a20a4-134">In the next unit, you will mount data volumes to an Azure container instance for data persistence.</span></span>
