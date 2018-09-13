<span data-ttu-id="81bd5-101">Azure Container Registry や、Azure Container Instances、Azure Kubernetes Registry、Docker for Windows/Mac などの多くのコンテナー管理プラットフォームから、コンテナー イメージをプルできます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-101">Container images can be pulled from Azure Container Registry from many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="81bd5-102">Azure Container Registry からコンテナー イメージを実行するときは、認証資格情報が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="81bd5-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> <span data-ttu-id="81bd5-103">Container Registry での認証には、Azure サービス プリンシパルを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="81bd5-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="81bd5-104">さらに、Azure Key Vault で Azure サービス プリンシパルの資格情報をセキュリティ保護することも勧めします。</span><span class="sxs-lookup"><span data-stu-id="81bd5-104">Furthermore, it is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span>

<span data-ttu-id="81bd5-105">このユニットでは、Azure Container Registry 用のサービス プリンシパルを作成し、それを Azure Key Vault に格納してから、サービス プリンシパルの資格情報を使用して Azure Container Instances にコンテナーを展開します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-105">In this unit, you will create a service principal for your Azure container registry, store it in Azure Key Vault, and then deploy the container to Azure Container Instances using the service principal's credentials.</span></span>

## <a name="configure-registry-authentication"></a><span data-ttu-id="81bd5-106">レジストリの認証を構成する</span><span class="sxs-lookup"><span data-stu-id="81bd5-106">Configure registry authentication</span></span>

<span data-ttu-id="81bd5-107">すべての運用シナリオでは、サービス プリンシパルを使用して Azure コンテナー レジストリにアクセスします。</span><span class="sxs-lookup"><span data-stu-id="81bd5-107">All production scenarios should use service principals to access an Azure container registry.</span></span> <span data-ttu-id="81bd5-108">サービス プリンシパルを使用すると、コンテナー イメージにロールベースのアクセス制御 (RBAC) を提供できます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-108">Service principals allow you to provide role-based access control (RBAC) to your container images.</span></span> <span data-ttu-id="81bd5-109">たとえば、レジストリに対するプルのみのアクセス権を持つサービス プリンシパルを構成できます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-109">For example, you can configure a service principal with pull-only access to a registry.</span></span>

<span data-ttu-id="81bd5-110">Azure Key Vault にコンテナーがまだない場合は、Azure CLI の次のコマンドを使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-110">If you don't already have a vault in Azure Key Vault, create one with the Azure CLI using the following commands.</span></span>

<span data-ttu-id="81bd5-111">最初に、コンテナー レジストリの名前で変数を作成します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-111">First, create a variable with the name of your container registry.</span></span> <span data-ttu-id="81bd5-112">この変数は、このユニット全体で使われます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-112">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

<span data-ttu-id="81bd5-113">`az keyvault create` コマンドで Azure キー コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-113">Create an Azure key vault with the `az keyvault create` command.</span></span>

```azurecli
az keyvault create --resource-group myResourceGroup --name $ACR_NAME-keyvault
```

<span data-ttu-id="81bd5-114">次に、サービス プリンシパルを作成し、その資格情報をキー コンテナーに格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="81bd5-114">Now, you need to create a service principal and store its credentials in your key vault.</span></span>

<span data-ttu-id="81bd5-115">サービス プリンシパルを作成するには、`az ad sp create-for-rbac` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-115">Use the `az ad sp create-for-rbac` command to create the service principal.</span></span> <span data-ttu-id="81bd5-116">`--role` 引数により、*reader* ロールを持つサービス プリンシパルが構成され、レジストリに対するプルのみのアクセス権が付与されます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-116">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="81bd5-117">プッシュ アクセス権とプル アクセス権の両方を付与するには、`--role` 引数を *contributor* に変更します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-117">To grant both push and pull access, change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="81bd5-118">サービス プリンシパルの作成の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="81bd5-118">This is what the output of the service principal creation will look like.</span></span> <span data-ttu-id="81bd5-119">`appId` と `password` の値を書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-119">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="81bd5-120">これらは Azure キー コンテナーに格納されます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-120">These will be stored in the Azure key vault.</span></span>

```output
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

<span data-ttu-id="81bd5-121">次に、`az keyvault secret set` コマンドを使用して、サービス プリンシパルの *appId* をコンテナーに格納します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-121">Next, use the `az keyvault secret set` command to store the service principal's *appId* in the vault.</span></span> <span data-ttu-id="81bd5-122">`<appId>` は、サービス プリンシパルの `appId` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-122">Replace `<appId>` with the `appId` of the service principal.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <appId>
```

<span data-ttu-id="81bd5-123">次に、`az keyvault secret set` コマンドを使用して、サービス プリンシパルの *password* をコンテナーに格納します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-123">Now, use the `az keyvault secret set` command to store the service principal's *password* in the vault.</span></span> <span data-ttu-id="81bd5-124">`<password>` は、サービス プリンシパルの `password` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-124">Replace `<password>` with the `password` of the service principal.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
```

<span data-ttu-id="81bd5-125">Azure キー コンテナーを作成し、そこに 2 つのシークレットを格納しました。</span><span class="sxs-lookup"><span data-stu-id="81bd5-125">You've created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="81bd5-126">`$ACR_NAME-pull-usr`: サービス プリンシパルの ID。コンテナー レジストリの**ユーザー名**として使用します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-126">`$ACR_NAME-pull-usr`: The service principal ID, for use as the container registry **username**.</span></span>
* <span data-ttu-id="81bd5-127">`$ACR_NAME-pull-pwd`: サービス プリンシパルのパスワード。コンテナー レジストリの**パスワード**として使用します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-127">`$ACR_NAME-pull-pwd`: The service principal password, for use as the container registry **password**.</span></span>

<span data-ttu-id="81bd5-128">これらのシークレットは、ユーザーまたはアプリケーションやサービスがレジストリからイメージをプルするときに名前で参照できます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-128">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="81bd5-129">Azure CLI でコンテナーを展開する</span><span class="sxs-lookup"><span data-stu-id="81bd5-129">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="81bd5-130">サービス プリンシパルの資格情報を Azure Key Vault に格納したので、アプリケーションやサービスでそれを使用してプライベート レジストリにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="81bd5-130">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="81bd5-131">コンテナー インスタンスを展開するには、次の `az container create` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-131">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="81bd5-132">このコマンドでは、Azure Key Vault に格納されているサービス プリンシパルの資格情報を使用して、コンテナー レジストリに対する認証を行います。</span><span class="sxs-lookup"><span data-stu-id="81bd5-132">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

<span data-ttu-id="81bd5-133">Azure コンテナー インスタンスの IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-133">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group myResourceGroup --name acr-build --query ipAddress.ip --output table
```

<span data-ttu-id="81bd5-134">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="81bd5-134">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="81bd5-135">すべてが正しく構成されている場合、次の結果が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="81bd5-135">If everything has been configured correctly, you should see the following results:</span></span>

![テキスト Hello World が表示されるサンプル Web アプリケーション](../media/hello.png)

