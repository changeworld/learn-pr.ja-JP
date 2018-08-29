<span data-ttu-id="06b79-101">次に Azure でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="06b79-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="06b79-102">Azure App Service アプリを作成し、MSI とコンテナー構成を使用してそのアプリを設定したうえで、コードをデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="06b79-102">We need to create an Azure App Service app, set it up with MSI and our vault configuration, and deploy our code.</span></span>

## <a name="exercise"></a><span data-ttu-id="06b79-103">演習</span><span class="sxs-lookup"><span data-stu-id="06b79-103">Exercise</span></span>

### <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="06b79-104">App Service のプランとアプリを作成する</span><span class="sxs-lookup"><span data-stu-id="06b79-104">Create the App Service plan and app</span></span>

<span data-ttu-id="06b79-105">App Service アプリの作成は 2 段階のプロセスです。最初に*プラン*を作成し、次に*アプリ*を作成します。</span><span class="sxs-lookup"><span data-stu-id="06b79-105">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="06b79-106">*プラン*名は、サブスクリプション内においてのみ一意であればかまいません。そのため、**keyvault-exercise-plan** という使用済みの名前と同じ名前を使用できます。</span><span class="sxs-lookup"><span data-stu-id="06b79-106">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="06b79-107">一方、アプリ名はグローバルに一意である必要があり、そのため独自の名前を選ぶ必要があります。</span><span class="sxs-lookup"><span data-stu-id="06b79-107">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span>

<span data-ttu-id="06b79-108">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="06b79-108">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

### <a name="add-configuration-to-the-app"></a><span data-ttu-id="06b79-109">アプリに構成を追加する</span><span class="sxs-lookup"><span data-stu-id="06b79-109">Add configuration to the app</span></span>

<span data-ttu-id="06b79-110">Azure へのデプロイでは、App Service のベスト プラクティスに従い、構成ファイルではなくアプリケーションの設定に構成を配置します。</span><span class="sxs-lookup"><span data-stu-id="06b79-110">For deploying to Azure, we'll follow the App Service best practice of putting configuration in Application Settings instead of a configuration file.</span></span>

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

### <a name="enable-msi"></a><span data-ttu-id="06b79-111">MSI を有効化する</span><span class="sxs-lookup"><span data-stu-id="06b79-111">Enable MSI</span></span>

<span data-ttu-id="06b79-112">アプリでの MSI の有効化は 1 行で指定できます。</span><span class="sxs-lookup"><span data-stu-id="06b79-112">Enabling MSI on an app is a one-liner:</span></span>

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="06b79-113">JSON の出力結果から、**principalId** の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="06b79-113">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="06b79-114">principalId は、Azure Active Directory 内にある、アプリの新しい ID を表す一意の ID です。次の手順で使用します。</span><span class="sxs-lookup"><span data-stu-id="06b79-114">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

### <a name="grant-access-to-the-vault"></a><span data-ttu-id="06b79-115">コンテナーへのアクセスを許可する</span><span class="sxs-lookup"><span data-stu-id="06b79-115">Grant access to the vault</span></span>

<span data-ttu-id="06b79-116">次に、アプリの ID に対して、運用環境の資格情報コンテナーからシークレットを取得して一覧表示するためのアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="06b79-116">Now you need to give your app identity permissions to get and list secrets from your production-environment vault.</span></span> <span data-ttu-id="06b79-117">前の手順でコピーした **principalId** の値を、以下のコマンドの **object-id** の値として使用します。</span><span class="sxs-lookup"><span data-stu-id="06b79-117">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span>

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-msi-principleid> --secret-permissions get list
```

### <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="06b79-118">アプリをデプロイし、試してみる</span><span class="sxs-lookup"><span data-stu-id="06b79-118">Deploy the app and try it out</span></span>

<span data-ttu-id="06b79-119">すべての構成が設定され、デプロイする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="06b79-119">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="06b79-120">以下のコマンドによってサイトを `pub` フォルダーに発行し、`site.zip` として圧縮した後、その zip を App Service にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="06b79-120">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="06b79-121">`cd` で KeyVaultDemoApp ディレクトリに戻る必要があります (まだそうしていない場合)。</span><span class="sxs-lookup"><span data-stu-id="06b79-121">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```console
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="06b79-122">サイトがデプロイされたことを示す結果が表示されたら、`https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` をブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="06b79-122">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="06b79-123">シークレットの値として **reindeer_flotilla** が表示されます。</span><span class="sxs-lookup"><span data-stu-id="06b79-123">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="06b79-124">アプリが完成し、デプロイされました。</span><span class="sxs-lookup"><span data-stu-id="06b79-124">Your app is finished and deployed!</span></span>