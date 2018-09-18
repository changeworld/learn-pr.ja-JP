<span data-ttu-id="3c3a1-101">Azure Container Registry や、Azure Container Instances、Azure Kubernetes Registry、Docker for Windows/Mac などの多くのコンテナー管理プラットフォームから、コンテナー イメージをプルできます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-101">Container images can be pulled from Azure Container Registry from many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="3c3a1-102">Azure Container Registry からコンテナー イメージを実行するときは、認証資格情報が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> 

<span data-ttu-id="3c3a1-103">Container Registry での認証には、Azure サービス プリンシパルを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="3c3a1-104">さらに、Azure Key Vault で Azure サービス プリンシパルの資格情報をセキュリティ保護することもお勧めします。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-104">Furthermore, it is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span> <span data-ttu-id="3c3a1-105">このアプローチをお勧めするため、このユニットではこのアプローチがどのように行われるかを見ていきます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-105">Since that's the recommended approach we will examine how it would be done in this unit.</span></span>

<span data-ttu-id="3c3a1-106">ただし、実際の演習では、すべての Azure コンテナー レジストリで有効にできる組み込みの管理アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-106">However, for our live practice we will use the built-in admin account that can be enabled in all Azure Container Registries.</span></span> <span data-ttu-id="3c3a1-107">この管理者アカウントは、Microsoft の無料のサンドボックス リソースで動作します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-107">The admin account works with our free sandbox resources.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="3c3a1-108">最初に、小文字のコンテナー レジストリ名を含む変数を作成します (たとえば、"MyContainer" ではなく "mycontainer" の値で)。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-108">First, create a variable with the name of your container registry in lowercase (for example, intstead of "MyContainer" make the value "mycontainer").</span></span> <span data-ttu-id="3c3a1-109">この変数は、このユニット全体で使われます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-109">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

### <a name="service-principal"></a><span data-ttu-id="3c3a1-110">サービス プリンシパル</span><span class="sxs-lookup"><span data-stu-id="3c3a1-110">Service Principal</span></span>

<span data-ttu-id="3c3a1-111">ここで、実稼働アプリケーションのためにサービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-111">Now, for a production application, we would want to create a service principal here.</span></span> <span data-ttu-id="3c3a1-112">**これは、サンドボックス環境では動作しません**が、ご自身のシステムではベスト プラクティスです。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-112">Remember **this won't work in our sandbox environment**, but it's a best practice in your own systems.</span></span> <span data-ttu-id="3c3a1-113">実際には、下記の管理者アカウントの手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-113">For our live practice you'll use the Admin Account instructions below.</span></span>

<span data-ttu-id="3c3a1-114">サービス プリンシパルを作成するには、`az ad sp create-for-rbac` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-114">The `az ad sp create-for-rbac` command can be used to create the service principal.</span></span> <span data-ttu-id="3c3a1-115">`--role` 引数により、*reader* ロールを持つサービス プリンシパルが構成され、レジストリに対するプルのみのアクセス権が付与されます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-115">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="3c3a1-116">プッシュ アクセス権とプル アクセス権の両方を付与するには、`--role` 引数を *contributor* に変更します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-116">To grant both push and pull access, you would change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="3c3a1-117">サービス プリンシパルの作成の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-117">This is what the output of the service principal creation would look like.</span></span> <span data-ttu-id="3c3a1-118">`appId` と `password` の値を書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-118">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="3c3a1-119">これらは Azure キー コンテナーに格納されるはずです。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-119">These should be stored in the Azure key vault.</span></span>

```output
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="admin-account"></a><span data-ttu-id="3c3a1-120">管理者アカウント</span><span class="sxs-lookup"><span data-stu-id="3c3a1-120">Admin Account</span></span>

<span data-ttu-id="3c3a1-121">Azure コンテナー レジストリには、組み込みの管理者アカウントが付属しています。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-121">Azure Container Registries come with a built-in Admin account.</span></span> <span data-ttu-id="3c3a1-122">これは Azure AD またはロールベースのアクセス制御とは関連付けられていないため、**テストのみで使用する必要があります**。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-122">This isn't tied to Azure AD or role based access controls and therefore **should only be used for testing**.</span></span> 

<span data-ttu-id="3c3a1-123">最初に、管理者アカウントを有効にする必要があります</span><span class="sxs-lookup"><span data-stu-id="3c3a1-123">First we must enable the Admin Account</span></span>
```azurecli
  az acr update -n $ACR_NAME --admin-enabled true
```

<span data-ttu-id="3c3a1-124">自動生成されたユーザー名とパスワードを取得するクエリを実行します</span><span class="sxs-lookup"><span data-stu-id="3c3a1-124">Now query to get the autogenerated username and password</span></span>

```azurecli
  az acr credential show --name $ACR_NAME
```

<span data-ttu-id="3c3a1-125">次のように出力されます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-125">Output is similar to below.</span></span> <span data-ttu-id="3c3a1-126">`username` と、`name` "password" とペアになっている `value` を書き留めます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-126">Take note of the `username`, and the `value` paired with the `name` "password".</span></span> <span data-ttu-id="3c3a1-127">これらをキー コンテナーに保存します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-127">You'll save these into key vault.</span></span>

```output
{  "passwords": [
    {
      "name": "password",
      "value": "aaaaa"
    },
    {
      "name": "password2",
      "value": "bbbbb"
    }
  ],
  "username": "ccccc"
}
```

### <a name="save-the-username-and-password-to-keyvault"></a><span data-ttu-id="3c3a1-128">ユーザー名とパスワードをキー コンテナーに保存します</span><span class="sxs-lookup"><span data-stu-id="3c3a1-128">Save the username and password to keyvault</span></span>

<span data-ttu-id="3c3a1-129">`az keyvault create` コマンドで Azure キー コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-129">Create an Azure key vault with the `az keyvault create` command.</span></span>

```azurecli
az keyvault create --resource-group <rgn>[Sandbox resource group name]</rgn> --name $ACR_NAME-keyvault
```

<span data-ttu-id="3c3a1-130">次に、`az keyvault secret set` コマンドを使用して、ACR のユーザー名をコンテナーに格納します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-130">Next, use the `az keyvault secret set` command to store the username for the ACR in the vault.</span></span> <span data-ttu-id="3c3a1-131">サービス プリンシパルを使用していた場合は、この値の appId を使用します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-131">If you were using service principals you'd use the appId for this value.</span></span> <span data-ttu-id="3c3a1-132">今度は管理者アカウントを使用しているため、上記のクエリのユーザー名を保存します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-132">Since we're using the admin account this time we'll save the username from our query above.</span></span> <span data-ttu-id="3c3a1-133">次のコマンドを発行します。`<username>` は忘れずに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-133">Issue the command below, don't forget to replace `<username>`.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <username>
```

<span data-ttu-id="3c3a1-134">次に、`az keyvault secret set` コマンドを使用して、*password* をコンテナーに格納します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-134">Now, use the `az keyvault secret set` command to store the *password* in the vault.</span></span> <span data-ttu-id="3c3a1-135">`<password>` は、上記のクエリの `password` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-135">Replace `<password>` with the `password` from the query above.</span></span>

```azurecli
az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
```

<span data-ttu-id="3c3a1-136">Azure キー コンテナーを作成し、そこに 2 つのシークレットを格納しました。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-136">You've created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="3c3a1-137">`$ACR_NAME-pull-usr`: コンテナー レジストリの**ユーザー名**。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-137">`$ACR_NAME-pull-usr`: the container registry **username**.</span></span>
* <span data-ttu-id="3c3a1-138">`$ACR_NAME-pull-pwd`: コンテナー レジストリの**パスワード**。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-138">`$ACR_NAME-pull-pwd`: the container registry **password**.</span></span>

<span data-ttu-id="3c3a1-139">これらのシークレットは、ユーザーまたはアプリケーションやサービスがレジストリからイメージをプルするときに名前で参照できます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-139">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="3c3a1-140">Azure CLI でコンテナーを展開する</span><span class="sxs-lookup"><span data-stu-id="3c3a1-140">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="3c3a1-141">サービス プリンシパルの資格情報を Azure Key Vault に格納したので、アプリケーションやサービスでそれを使用してプライベート レジストリにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-141">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="3c3a1-142">コンテナー インスタンスを展開するには、次の `az container create` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-142">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="3c3a1-143">このコマンドでは、Azure Key Vault に格納されているサービス プリンシパルの資格情報を使用して、コンテナー レジストリに対する認証を行います。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-143">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

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

<span data-ttu-id="3c3a1-144">Azure コンテナー インスタンスの IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-144">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group  <rgn>[Sandbox resource group name]</rgn> --name acr-build --query ipAddress.ip --output table
```

<span data-ttu-id="3c3a1-145">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-145">Open up a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="3c3a1-146">すべてが正しく構成されている場合、次の結果が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="3c3a1-146">If everything has been configured correctly, you should see the following results:</span></span>

![テキスト Hello World が表示されるサンプル Web アプリケーション](../media/hello.png)

