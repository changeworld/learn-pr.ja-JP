<span data-ttu-id="89274-101">Azure Container Registry から、Azure Container Instances、Azure Kubernetes Registry、Docker for Windows/Mac などの多くのコンテナー管理プラットフォームを使用して、コンテナー イメージをプルすることができます。</span><span class="sxs-lookup"><span data-stu-id="89274-101">Container images can be pulled from Azure Container Registry using many container management platforms, such as Azure Container Instances, Azure Kubernetes Registry, and Docker for Windows or Mac.</span></span> <span data-ttu-id="89274-102">Azure Container Registry からコンテナー イメージを実行するときは、認証資格情報が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="89274-102">When running container images from Azure Container Registry, authentication credentials may be needed.</span></span> 

<span data-ttu-id="89274-103">Container Registry での認証には、Azure サービス プリンシパルを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="89274-103">It is recommended to use an Azure service principal for authentication with Container Registry.</span></span> <span data-ttu-id="89274-104">また、Azure Key Vault で Azure サービス プリンシパルの資格情報をセキュリティ保護することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="89274-104">It is also recommended to secure the Azure service principal credentials in Azure Key Vault.</span></span> <span data-ttu-id="89274-105">このユニットでは、推奨される方法に従います。</span><span class="sxs-lookup"><span data-stu-id="89274-105">In this unit we'll follow the recommended approach.</span></span>

<span data-ttu-id="89274-106">しかし、実際の演習では、すべての Azure Container Registry で有効にできる組み込みの管理者アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="89274-106">However, for our live practice we will use the built-in admin account that can be enabled in all Azure Container Registries.</span></span> <span data-ttu-id="89274-107">この管理者アカウントは、サンドボックス リソースで動作します。</span><span class="sxs-lookup"><span data-stu-id="89274-107">The admin account works with the free sandbox resources.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="89274-108">小文字のコンテナー レジストリ名を含む変数を作成します (たとえば、"MyContainer" ではなく "mycontainer" の値で)。</span><span class="sxs-lookup"><span data-stu-id="89274-108">Create a variable with the name of your container registry in lowercase (for example, instead of "MyContainer" make the value "mycontainer").</span></span> <span data-ttu-id="89274-109">この変数は、このユニット全体で使われます。</span><span class="sxs-lookup"><span data-stu-id="89274-109">This variable is used throughout this unit.</span></span>

```azurecli
ACR_NAME=<acrName>
```

### <a name="service-principal"></a><span data-ttu-id="89274-110">サービス プリンシパル</span><span class="sxs-lookup"><span data-stu-id="89274-110">Service Principal</span></span>

<span data-ttu-id="89274-111">実稼働アプリケーションのために、ここではサービス プリンシパルを作成します。</span><span class="sxs-lookup"><span data-stu-id="89274-111">For a production application, we would want to create a service principal here.</span></span> <span data-ttu-id="89274-112">**これはサンドボックス環境では動作しません**が、ご自身のシステムで従うベスト プラクティスとなります。</span><span class="sxs-lookup"><span data-stu-id="89274-112">Remember **this won't work in our sandbox environment**, but it's a best practice to follow in your own systems.</span></span> <span data-ttu-id="89274-113">実際には、下記の管理者アカウントの手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="89274-113">For our live practice, you'll use the Admin Account instructions below.</span></span>

<span data-ttu-id="89274-114">`az ad sp create-for-rbac` コマンドを使用して、サービス プリンシパルを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="89274-114">The `az ad sp create-for-rbac` command can be used to create the service principal.</span></span> <span data-ttu-id="89274-115">`--role` 引数により、*reader* ロールを持つサービス プリンシパルが構成され、レジストリに対するプルのみのアクセス権が付与されます。</span><span class="sxs-lookup"><span data-stu-id="89274-115">The `--role` argument configures the service principal with the *reader* role, which grants it pull-only access to the registry.</span></span> <span data-ttu-id="89274-116">プッシュ アクセス権とプル アクセス権の両方を付与するには、`--role` 引数を *contributor* に変更します。</span><span class="sxs-lookup"><span data-stu-id="89274-116">To grant both push and pull access, you would change the `--role` argument to *contributor*.</span></span>

```azurecli
az ad sp create-for-rbac --scopes $(az acr show --name $ACR_NAME --query id --output tsv) --role reader
```

<span data-ttu-id="89274-117">サービス プリンシパルの作成の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="89274-117">This is what the output of the service principal creation would look like.</span></span> <span data-ttu-id="89274-118">`appId` と `password` の値を書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="89274-118">Take note of the `appId` and the `password` values.</span></span> <span data-ttu-id="89274-119">これらは Azure キー コンテナーに格納されるはずです。</span><span class="sxs-lookup"><span data-stu-id="89274-119">These should be stored in the Azure key vault.</span></span>

```output
{
  "appId": "1fa05179-0000-0000-0000-e269a4e97c41",
  "displayName": "azure-cli-2018-08-19-22-35-26",
  "name": "http://azure-cli-2018-08-19-22-35-26",
  "password": "72377509-0000-0000-0000-c8edbcb2d950",
  "tenant": "00000000-0000-0000-0000-000000000000"
}
```

### <a name="admin-account"></a><span data-ttu-id="89274-120">管理者アカウント</span><span class="sxs-lookup"><span data-stu-id="89274-120">Admin Account</span></span>

<span data-ttu-id="89274-121">Azure コンテナー レジストリには、組み込みの管理者アカウントが付属しています。</span><span class="sxs-lookup"><span data-stu-id="89274-121">Azure Container Registries come with a built-in Admin account.</span></span> <span data-ttu-id="89274-122">これは Azure AD やロールベースのアクセス制御とは関連付けられていないため、**テストのみで使用する必要があります**。</span><span class="sxs-lookup"><span data-stu-id="89274-122">This isn't tied to Azure AD or role based access controls and therefore **should only be used for testing**.</span></span> 

1. <span data-ttu-id="89274-123">管理者アカウントを有効にします。</span><span class="sxs-lookup"><span data-stu-id="89274-123">Enable the Admin Account.</span></span>
    ```azurecli
      az acr update -n $ACR_NAME --admin-enabled true
    ```

2. <span data-ttu-id="89274-124">自動生成されたユーザー名とパスワードを取得するクエリを実行する</span><span class="sxs-lookup"><span data-stu-id="89274-124">Query to get the autogenerated username and password</span></span>

    ```azurecli
      az acr credential show --name $ACR_NAME
    ```

<span data-ttu-id="89274-125">出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="89274-125">The output is similar to below.</span></span> <span data-ttu-id="89274-126">`username` と、`name` "password" とペアになっている `value` を書き留めます。</span><span class="sxs-lookup"><span data-stu-id="89274-126">Take note of the `username` and the `value` paired with the `name` "password".</span></span> <span data-ttu-id="89274-127">これらをキー コンテナーに保存します。</span><span class="sxs-lookup"><span data-stu-id="89274-127">You'll save these into key vault.</span></span>

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

### <a name="save-the-username-and-password-to-the-key-vault"></a><span data-ttu-id="89274-128">ユーザー名とパスワードをキー コンテナーに保存する</span><span class="sxs-lookup"><span data-stu-id="89274-128">Save the username and password to the key vault</span></span>

1. <span data-ttu-id="89274-129">`az keyvault create` コマンドで Azure Key Vault を作成します。</span><span class="sxs-lookup"><span data-stu-id="89274-129">Create an Azure Key Vault with the `az keyvault create` command.</span></span>

    ```azurecli
    az keyvault create --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACR_NAME-keyvault
    ```

1. <span data-ttu-id="89274-130">`az keyvault secret set` コマンドを使用して、ACR のユーザー名をコンテナーに格納します。</span><span class="sxs-lookup"><span data-stu-id="89274-130">Use the `az keyvault secret set` command to store the username for the ACR in the vault.</span></span> <span data-ttu-id="89274-131">サービス プリンシパルを使用していた場合は、この値の appId を使用します。</span><span class="sxs-lookup"><span data-stu-id="89274-131">If you were using service principals you'd use the appId for this value.</span></span> <span data-ttu-id="89274-132">今度は管理者アカウントを使用するため、上記のクエリのユーザー名を保存します。</span><span class="sxs-lookup"><span data-stu-id="89274-132">Since we're using the admin account this time we'll save the username from our query above.</span></span> <span data-ttu-id="89274-133">次のコマンドを入力します。`<username>` は忘れずに置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="89274-133">Enter the command below, don't forget to replace `<username>`.</span></span>

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --value <username>
    ```

1. <span data-ttu-id="89274-134">`az keyvault secret set` コマンドを使用して、*password* をコンテナーに格納します。</span><span class="sxs-lookup"><span data-stu-id="89274-134">Use the `az keyvault secret set` command to store the *password* in the vault.</span></span> <span data-ttu-id="89274-135">`<password>` は、上記のクエリの `password` に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="89274-135">Replace `<password>` with the `password` from the query above.</span></span>

    ```azurecli
    az keyvault secret set --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --value <password>
    ```

<span data-ttu-id="89274-136">これで Azure キー コンテナーが作成され、そこに 2 つのシークレットが格納されました。</span><span class="sxs-lookup"><span data-stu-id="89274-136">You've now created an Azure key vault and stored two secrets in it:</span></span>

* <span data-ttu-id="89274-137">`$ACR_NAME-pull-usr`: コンテナー レジストリの**ユーザー名**。</span><span class="sxs-lookup"><span data-stu-id="89274-137">`$ACR_NAME-pull-usr`: the container registry **username**.</span></span>
* <span data-ttu-id="89274-138">`$ACR_NAME-pull-pwd`: コンテナー レジストリの**パスワード**。</span><span class="sxs-lookup"><span data-stu-id="89274-138">`$ACR_NAME-pull-pwd`: the container registry **password**.</span></span>

<span data-ttu-id="89274-139">これらのシークレットは、ユーザーまたはアプリケーションやサービスがレジストリからイメージをプルするときに名前で参照できます。</span><span class="sxs-lookup"><span data-stu-id="89274-139">You can now reference these secrets by name when you or your applications and services pull images from the registry.</span></span>

### <a name="deploy-a-container-with-azure-cli"></a><span data-ttu-id="89274-140">Azure CLI でコンテナーを展開する</span><span class="sxs-lookup"><span data-stu-id="89274-140">Deploy a container with Azure CLI</span></span>

<span data-ttu-id="89274-141">サービス プリンシパルの資格情報を Azure Key Vault に格納したので、アプリケーションやサービスでそれを使用してプライベート レジストリにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="89274-141">Now that the service principal credentials are stored in Azure Key Vault, your applications and services can use them to access your private registry.</span></span>

<span data-ttu-id="89274-142">コンテナー インスタンスを展開するには、次の `az container create` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="89274-142">Execute the following `az container create` command to deploy a container instance.</span></span> <span data-ttu-id="89274-143">このコマンドでは、Azure Key Vault に格納されているサービス プリンシパルの資格情報を使用して、コンテナー レジストリに対する認証を行います。</span><span class="sxs-lookup"><span data-stu-id="89274-143">The command uses the service principal's credentials stored in Azure Key Vault to authenticate to your container registry.</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name acr-tasks \
    --image $ACR_NAME.azurecr.io/helloacrtasks:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --ip-address Public \
    --location eastus \
    --registry-username $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $ACR_NAME-keyvault --name $ACR_NAME-pull-pwd --query value -o tsv)
```

<span data-ttu-id="89274-144">Azure コンテナー インスタンスの IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="89274-144">Get the IP address of the Azure container instance.</span></span>

```azurecli
az container show --resource-group  <rgn>[sandbox resource group name]</rgn> --name acr-tasks --query ipAddress.ip --output table
```

<span data-ttu-id="89274-145">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="89274-145">Open a browser and navigate to the IP address of the container.</span></span> <span data-ttu-id="89274-146">すべてが正しく構成されている場合、次の結果が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="89274-146">If everything has been configured correctly, you should see the following results:</span></span>

![テキスト Hello World が表示されるサンプル Web アプリケーション](../media/hello.png)

