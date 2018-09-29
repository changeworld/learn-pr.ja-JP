<span data-ttu-id="4f952-101">コンテナー インスタンス内の環境変数により、コンテナーによって実行されるアプリケーションまたはスクリプトの動的な構成を指定できます。</span><span class="sxs-lookup"><span data-stu-id="4f952-101">Environment variables in your container instances allow you to provide dynamic configuration of the application or script run by the container.</span></span> <span data-ttu-id="4f952-102">Azure CLI、PowerShell、または Azure portal を使用して、コンテナーを作成するときに変数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="4f952-102">You can use the Azure CLI, PowerShell, or the Azure portal to set variables when you create the container.</span></span> <span data-ttu-id="4f952-103">コンテナーの出力に機密情報が表示されるのを防ぐには、セキュリティで保護された環境変数を使います。</span><span class="sxs-lookup"><span data-stu-id="4f952-103">Secured environment variables are used to prevent sensitive information from being displayed in container output.</span></span>

<span data-ttu-id="4f952-104">ここでは、Azure Cosmos DB インスタンスを作成し、環境変数を使用して接続情報を Azure コンテナー インスタンスに渡します。</span><span class="sxs-lookup"><span data-stu-id="4f952-104">Here, you will create an Azure Cosmos DB instance and use environment variables to pass the connection information to an Azure container instance.</span></span> <span data-ttu-id="4f952-105">コンテナー内のアプリケーションでは、変数を使用して Cosmos DB のデータを読み書きします。</span><span class="sxs-lookup"><span data-stu-id="4f952-105">An application in the container uses the variables to write and read data from Cosmos DB.</span></span> <span data-ttu-id="4f952-106">環境変数とセキュリティで保護された環境変数の両方を作成します。</span><span class="sxs-lookup"><span data-stu-id="4f952-106">You will create both an environment variable and a secured environment variable.</span></span>

## <a name="deploy-azure-cosmos-db"></a><span data-ttu-id="4f952-107">Azure Cosmos DB をデプロイする</span><span class="sxs-lookup"><span data-stu-id="4f952-107">Deploy Azure Cosmos DB</span></span>

<span data-ttu-id="4f952-108">`az cosmosdb create` コマンドを使用して Azure Cosmos DB インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4f952-108">Create the Azure Cosmos DB instance with the `az cosmosdb create` command.</span></span> <span data-ttu-id="4f952-109">また、この例では、Azure Cosmos DB のエンドポイント アドレスを *COSMOS_DB_ENDPOINT* という名前の変数に配置します。</span><span class="sxs-lookup"><span data-stu-id="4f952-109">This example will also place the Azure Cosmos DB endpoint address in a variable named *COSMOS_DB_ENDPOINT*.</span></span> <span data-ttu-id="4f952-110">`[cosmos-db-name]` には一意の名前を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4f952-110">You'll need to supply a unique name for `[cosmos-db-name]`.</span></span>

<span data-ttu-id="4f952-111">このコマンドが完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="4f952-111">This command can take a few minutes to complete:</span></span>

```azurecli
COSMOS_DB_ENDPOINT=$(az cosmosdb create --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query documentEndpoint --output tsv)
```

<span data-ttu-id="4f952-112">次に、`az cosmosdb list-keys` コマンドを使用して Azure Cosmos DB の接続キーを取得し、それを *COSMOS_DB_MASTERKEY* という名前の変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="4f952-112">Next, get the Azure Cosmos DB connection key with the `az cosmosdb list-keys` command and store it in a variable named *COSMOS_DB_MASTERKEY*.</span></span> <span data-ttu-id="4f952-113">`[cosmos-db-name]` をもう一度置換することを忘れないでください。</span><span class="sxs-lookup"><span data-stu-id="4f952-113">Don't forget to replace `[cosmos-db-name]` again:</span></span>

```azurecli
COSMOS_DB_MASTERKEY=$(az cosmosdb list-keys --resource-group <rgn>[sandbox resource group name]</rgn> --name [cosmos-db-name] --query primaryMasterKey --output tsv)
```

## <a name="deploy-a-container-instance"></a><span data-ttu-id="4f952-114">コンテナー インスタンスをデプロイする</span><span class="sxs-lookup"><span data-stu-id="4f952-114">Deploy a container instance</span></span>

<span data-ttu-id="4f952-115">`az container create` コマンドを使用して、Azure コンテナー インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="4f952-115">Create an Azure container instance using the `az container create` command.</span></span> <span data-ttu-id="4f952-116">2 つの環境変数の値 `COSMOS_DB_ENDPOINT` と `COSMOS_DB_MASTERKEY` が作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4f952-116">Take note that two environment variables are created, `COSMOS_DB_ENDPOINT` and `COSMOS_DB_MASTERKEY`.</span></span> <span data-ttu-id="4f952-117">これらの変数には、Azure Cosmos DB インスタンスに接続するために必要な値が保持されています。</span><span class="sxs-lookup"><span data-stu-id="4f952-117">These variables hold the values needed to connect to the Azure Cosmos DB instance:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --environment-variables COSMOS_DB_ENDPOINT=$COSMOS_DB_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_DB_MASTERKEY
```

<span data-ttu-id="4f952-118">コンテナーが作成された後、`az container show` コマンドを使用して IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="4f952-118">Once the container has been created, get the IP address with the `az container show` command:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo \
    --query ipAddress.ip \
    --output tsv
```

<span data-ttu-id="4f952-119">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="4f952-119">Open a browser and navigate to the IP address of the container.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="4f952-120">コンテナーが完全に起動し、接続を受信できるようになるまで 1 分から 2 分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="4f952-120">Sometimes containers take a minute or two to fully start up and be able to receive connections.</span></span> <span data-ttu-id="4f952-121">ブラウザー内で IP アドレスに移動しても応答がない場合は、しばらく待ってからページを更新します。</span><span class="sxs-lookup"><span data-stu-id="4f952-121">If there's no response when you navigate to the IP address in your browser,  wait a few moments and refresh the page.</span></span>

 <span data-ttu-id="4f952-122">アプリが使用できるようになると、次のアプリケーションが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4f952-122">Once the app is available, you should see the following application.</span></span> <span data-ttu-id="4f952-123">投票を行うと、Azure Cosmos DB インスタンスに投票が格納されます。</span><span class="sxs-lookup"><span data-stu-id="4f952-123">When casting a vote, the vote is stored in the Azure Cosmos DB instance.</span></span>

![猫または犬という 2 つの選択肢がある Azure 投票アプリケーション。](../media/4-azure-vote.png)

## <a name="secured-environment-variables"></a><span data-ttu-id="4f952-125">セキュリティで保護された環境変数</span><span class="sxs-lookup"><span data-stu-id="4f952-125">Secured environment variables</span></span>

<span data-ttu-id="4f952-126">上の演習では、コンテナーは 2 つの環境変数に格納されている Azure Cosmos DB に対する接続情報を使用して作成されました。</span><span class="sxs-lookup"><span data-stu-id="4f952-126">In the previous exercise, a container was created with connection information for Azure Cosmos DB stored in two environment variables.</span></span> <span data-ttu-id="4f952-127">既定では、環境変数は Azure portal とコマンド ライン ツールにプレーン テキストで表示されます。</span><span class="sxs-lookup"><span data-stu-id="4f952-127">By default, environment variables are displayed in the Azure portal and command-line tools in plain text.</span></span>

<span data-ttu-id="4f952-128">たとえば、前の演習で作成したコンテナーに関する情報を `az container show` コマンドで取得する場合、環境変数にはプレーン テキストでアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="4f952-128">For example, if you get information about the container created in the previous exercise with the `az container show` command, the environment variables are accessible in plain text:</span></span>

```azurecli
az container show --resource-group <rgn>[sandbox resource group name]</rgn> --name aci-demo --query containers[0].environmentVariables
```

<span data-ttu-id="4f952-129">出力例:</span><span class="sxs-lookup"><span data-stu-id="4f952-129">Example output:</span></span>

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

セキュリティで保護された環境変数を使用するとクリア テキストで出力されなくなります。 <span data-ttu-id="4f952-131">セキュリティで保護された環境変数を使用するには、`--environment-variables` 引数を `--secure-environment-variables` 引数に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="4f952-131">To use secure environment variables, replace the `--environment-variables` argument with the `--secure-environment-variables` argument.</span></span>

<span data-ttu-id="4f952-132">セキュリティで保護された環境変数を使用する *aci-demo-secure* という名前のコンテナーを作成するには、次の例を実行します。</span><span class="sxs-lookup"><span data-stu-id="4f952-132">Run the following example to create a container named *aci-demo-secure* that utilizes secured environment variables:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --image microsoft/azure-vote-front:cosmosdb \
    --ip-address Public \
    --location eastus \
    --secure-environment-variables COSMOS_DB_ENDPOINT=$COSMOS_ENDPOINT \
    COSMOS_DB_MASTERKEY=$COSMOS_KEY
```

<span data-ttu-id="4f952-133">コンテナーが `az container show` コマンドで返されたとき、環境変数は表示されません。</span><span class="sxs-lookup"><span data-stu-id="4f952-133">Now, when the container is returned with the `az container show` command, the environment variables are not displayed:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-secure \
    --query containers[0].environmentVariables
```

<span data-ttu-id="4f952-134">出力例:</span><span class="sxs-lookup"><span data-stu-id="4f952-134">Example output:</span></span>

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