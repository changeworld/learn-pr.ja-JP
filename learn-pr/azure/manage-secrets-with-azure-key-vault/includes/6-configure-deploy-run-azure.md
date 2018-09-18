<span data-ttu-id="1c627-101">次に Azure でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c627-101">Now it's time to run our app in Azure.</span></span> <span data-ttu-id="1c627-102">Azure App Service アプリを作成し、マネージド ID とコンテナー構成を使用してそのアプリを設定してから、コードをデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c627-102">We need to create an Azure App Service app, set it up with a managed identity and our vault configuration, and deploy our code.</span></span>

## <a name="create-the-app-service-plan-and-app"></a><span data-ttu-id="1c627-103">App Service のプランとアプリを作成する</span><span class="sxs-lookup"><span data-stu-id="1c627-103">Create the App Service plan and app</span></span>

<span data-ttu-id="1c627-104">App Service アプリの作成は 2 段階のプロセスです。最初に*プラン*を作成し、次に*アプリ*を作成します。</span><span class="sxs-lookup"><span data-stu-id="1c627-104">Creating an App Service app is a two-step process: First create the *plan*, then the *app*.</span></span>

<span data-ttu-id="1c627-105">*プラン*名は、サブスクリプション内においてのみ一意であればかまいません。そのため、**keyvault-exercise-plan** という使用済みの名前と同じ名前を使用できます。</span><span class="sxs-lookup"><span data-stu-id="1c627-105">The *plan* name only needs to be unique within your subscription, so you can use the same name we've used: **keyvault-exercise-plan**.</span></span> <span data-ttu-id="1c627-106">一方、アプリ名はグローバルに一意である必要があり、そのため独自の名前を選ぶ必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c627-106">The app name needs to be globally unique, though, so you'll need to pick your own.</span></span>

<span data-ttu-id="1c627-107">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="1c627-107">In Azure Cloud Shell, run the following:</span></span>

```azurecli
az appservice plan create --name keyvault-exercise-plan --resource-group keyvault-exercise-group
az webapp create --name <your-unique-app-name> --plan keyvault-exercise-plan --resource-group keyvault-exercise-group
```

## <a name="add-configuration-to-the-app"></a><span data-ttu-id="1c627-108">アプリに構成を追加する</span><span class="sxs-lookup"><span data-stu-id="1c627-108">Add configuration to the app</span></span>

<span data-ttu-id="1c627-109">Azure へのデプロイでは、App Service のベスト プラクティスに従い、構成ファイルではなくアプリケーションの設定に VaultName 構成を配置します。</span><span class="sxs-lookup"><span data-stu-id="1c627-109">For deploying to Azure, we'll follow the App Service best practice of putting the VaultName configuration in an application setting instead of a configuration file.</span></span>

```azurecli
az webapp config appsettings set --name <your-unique-app-name> --resource-group keyvault-exercise-group --settings VaultName=<your-unique-vault-name>
```

## <a name="enable-managed-identity"></a><span data-ttu-id="1c627-110">マネージド ID を有効にする</span><span class="sxs-lookup"><span data-stu-id="1c627-110">Enable managed identity</span></span>

<span data-ttu-id="1c627-111">アプリでマネージド ID を有効にするのは 1 行で済みます。</span><span class="sxs-lookup"><span data-stu-id="1c627-111">Enabling managed identity on an app is a one-liner:</span></span>

```azurecli
az webapp identity assign --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="1c627-112">JSON の出力結果から、**principalId** の値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="1c627-112">From the JSON output that results, copy the **principalId** value.</span></span> <span data-ttu-id="1c627-113">principalId は、Azure Active Directory 内にある、アプリの新しい ID を表す一意の ID です。次の手順で使用します。</span><span class="sxs-lookup"><span data-stu-id="1c627-113">PrincipalId is the unique ID of the app's new identity in Azure Active Directory, and we're going to use it in the next step.</span></span>

## <a name="grant-access-to-the-vault"></a><span data-ttu-id="1c627-114">コンテナーへのアクセスを許可する</span><span class="sxs-lookup"><span data-stu-id="1c627-114">Grant access to the vault</span></span>

<span data-ttu-id="1c627-115">次に、アプリの ID に対して、運用環境の資格情報コンテナーからシークレットを取得して一覧表示するためのアクセス許可を付与する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1c627-115">Now you need to give your app identity permissions to get and list secrets from your production-environment vault.</span></span> <span data-ttu-id="1c627-116">前の手順でコピーした **principalId** の値を、以下のコマンドの **object-id** の値として使用します。</span><span class="sxs-lookup"><span data-stu-id="1c627-116">Use the **principalId** value you copied from the previous step as the value for **object-id** in the command below.</span></span>

```azurecli
az keyvault set-policy --name <your-unique-vault-name> --object-id <your-managed-identity-principleid> --secret-permissions get list
```

## <a name="deploy-the-app-and-try-it-out"></a><span data-ttu-id="1c627-117">アプリをデプロイし、試してみる</span><span class="sxs-lookup"><span data-stu-id="1c627-117">Deploy the app and try it out</span></span>

<span data-ttu-id="1c627-118">すべての構成が設定され、デプロイする準備ができました。</span><span class="sxs-lookup"><span data-stu-id="1c627-118">All your configuration is set and you're ready to deploy!</span></span> <span data-ttu-id="1c627-119">以下のコマンドによってサイトを `pub` フォルダーに発行し、`site.zip` として圧縮した後、その zip を App Service にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="1c627-119">The below commands will publish the site to the `pub` folder, zip it up into `site.zip`, and deploy the zip to App Service.</span></span>

> [!NOTE]
> <span data-ttu-id="1c627-120">`cd` で KeyVaultDemoApp ディレクトリに戻る必要があります (まだそうしていない場合)。</span><span class="sxs-lookup"><span data-stu-id="1c627-120">You'll need to `cd` back to the KeyVaultDemoApp directory if you haven't already.</span></span>

```azurecli
dotnet publish -o pub
zip -j site.zip pub/*
az webapp deployment source config-zip --src site.zip --name <your-unique-app-name> --resource-group keyvault-exercise-group
```

<span data-ttu-id="1c627-121">サイトがデプロイされたことを示す結果が表示されたら、`https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` をブラウザーで開きます。</span><span class="sxs-lookup"><span data-stu-id="1c627-121">Once you get a result that indicates the site has deployed, open `https://<your-unique-app-name>.azurewebsites.net/api/SecretTest` in a browser.</span></span> <span data-ttu-id="1c627-122">シークレットの値として **reindeer_flotilla** が表示されます。</span><span class="sxs-lookup"><span data-stu-id="1c627-122">You should see the secret value, **reindeer_flotilla**.</span></span>

<span data-ttu-id="1c627-123">アプリが完成し、デプロイされました。</span><span class="sxs-lookup"><span data-stu-id="1c627-123">Your app is finished and deployed!</span></span>