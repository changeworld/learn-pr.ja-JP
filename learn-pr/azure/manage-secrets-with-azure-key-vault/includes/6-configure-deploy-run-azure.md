<span data-ttu-id="a8569-101">次に Azure でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="a8569-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="a8569-102">Azure App Service アプリを作成し、マネージド ID とコンテナー構成を使用してそのアプリを設定してから、コードをデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8569-102">We need to create an Azure App Service app, set it up with a managed identity and our vault configuration, and deploy our code.</span></span>

## <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="a8569-103">App Service のプランとアプリを作成する</span><span class="sxs-lookup"><span data-stu-id="a8569-103">Create the App Service plan and app</span></span>

<span data-ttu-id="a8569-104">App Service アプリの作成は 2 段階のプロセスです。最初に*プラン*を作成し、次に*アプリ*を作成します。</span><span class="sxs-lookup"><span data-stu-id="a8569-104">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="a8569-105">*プラン*名は、サブスクリプション内においてのみ一意であればかまいません。そのため、**keyvault-exercise-plan** という使用済みの名前と同じ名前を使用できます。</span><span class="sxs-lookup"><span data-stu-id="a8569-105">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="a8569-106">一方、アプリ名はグローバルに一意である必要があり、そのため独自の名前を選ぶ必要があります。</span><span class="sxs-lookup"><span data-stu-id="a8569-106">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span> <span data-ttu-id="a8569-107">コンテナーを作成するときに使用したのと同じ場所を使用します。</span><span class="sxs-lookup"><span data-stu-id="a8569-107">Use the same location you used when creating the vault.</span></span>

<span data-ttu-id="a8569-108">Azure Cloud Shell で次を実行します。</span><span class="sxs-lookup"><span data-stu-id="a8569-108">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create \
    --name keyvault-exercise-plan \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
    --location eastus

az webapp create \
    --name <your-unique-app-name> \
    --plan keyvault-exercise-plan \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

## <a name="add-configuration-to-the-app"></a><span data-ttu-id="a8569-109">アプリに構成を追加する</span><span class="sxs-lookup"><span data-stu-id="a8569-109">Add configuration to the app</span></span>

<span data-ttu-id="a8569-110">Azure へのデプロイでは、構成ファイルではなくアプリケーションの設定内に VaultName 構成を配置するという App Service のベスト プラクティスに従います。</span><span class="sxs-lookup"><span data-stu-id="a8569-110">For deploying to Azure, we'll follow the App Service best practice of putting the VaultName configuration in an application setting instead of a configuration file.</span></span> <span data-ttu-id="a8569-111">次のコマンドを実行して、アプリケーション設定を作成します。</span><span class="sxs-lookup"><span data-stu-id="a8569-111">Run this command to create the application setting:</span></span>

```azurecli
az webapp config appsettings set \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-managed-identity"></a><span data-ttu-id="a8569-112">マネージド ID を有効にする</span><span class="sxs-lookup"><span data-stu-id="a8569-112">Enable managed identity</span></span>

<span data-ttu-id="a8569-113">アプリ上でマネージド ID を有効にする処理は 1 行で済みます。次の内容を実行することで、ご利用のアプリ上で有効になります。</span><span class="sxs-lookup"><span data-stu-id="a8569-113">Enabling managed identity on an app is a one-liner &mdash; run this to enable it on your app:</span></span>

```azurecli
az webapp identity assign \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="a8569-114">JSON の出力結果から、**principalId** の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="a8569-114">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="a8569-115">principalId は、Azure Active Directory 内にある、アプリの新しい ID を表す一意の ID です。次の手順で使用します。</span><span class="sxs-lookup"><span data-stu-id="a8569-115">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

## <a name="grant-access-to-the-vault"></a><span data-ttu-id="a8569-116">コンテナーへのアクセスを許可する</span><span class="sxs-lookup"><span data-stu-id="a8569-116">Grant access to the vault</span></span>

<span data-ttu-id="a8569-117">デプロイする前に行う最後の手順は、Key Vault アクセス許可をご利用のアプリのマネージド ID に割り当てることです。</span><span class="sxs-lookup"><span data-stu-id="a8569-117">The last step before deploying is to assign Key Vault permissions to your app's managed identity.</span></span> <span data-ttu-id="a8569-118">前の手順でコピーした **principalId** の値を、以下のコマンドの **object-id** の値として使用します。</span><span class="sxs-lookup"><span data-stu-id="a8569-118">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span> <span data-ttu-id="a8569-119">このコマンドを実行すると、**Get** および **List** に以下へのアクセス許可が付与されます。</span><span class="sxs-lookup"><span data-stu-id="a8569-119">Running this command will grant **Get** and **List** access:</span></span>

```azurecli
az keyvault set-policy \
    --name <your-unique-vault-name> \
    --object-id <your-managed-identity-principleid> \
    --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="a8569-120">アプリをデプロイし、試してみる</span><span class="sxs-lookup"><span data-stu-id="a8569-120">Deploy the app and try it out</span></span>

<span data-ttu-id="a8569-121">すべての構成が設定され、デプロイする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="a8569-121">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="a8569-122">以下のコマンドによってサイトを `pub` フォルダーに発行し、`site.zip` として圧縮した後、その zip を App Service にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="a8569-122">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="a8569-123">`cd` で KeyVaultDemoApp ディレクトリに戻る必要があります (まだそうしていない場合)。</span><span class="sxs-lookup"><span data-stu-id="a8569-123">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*

az webapp deployment source config-zip \
    --src site.zip \
    --name <your-unique-app-name> \
    --resource-group <rgn>[Sandbox resource group name]</rgn>
```

<span data-ttu-id="a8569-124">サイトがデプロイされたことを示す結果が表示されたら、`https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` をブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="a8569-124">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="a8569-125">シークレットの値として **reindeer_flotilla** が表示されます。</span><span class="sxs-lookup"><span data-stu-id="a8569-125">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="a8569-126">アプリが完成し、デプロイされました。</span><span class="sxs-lookup"><span data-stu-id="a8569-126">Your app is finished and deployed!</span></span>