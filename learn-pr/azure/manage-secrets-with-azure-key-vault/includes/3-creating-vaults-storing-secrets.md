## <a name="creating-key-vaults-for-your-applications"></a><span data-ttu-id="04e81-101">アプリケーション用の Key Vault を作成する</span><span class="sxs-lookup"><span data-stu-id="04e81-101">Creating Key Vaults for your applications</span></span>

<span data-ttu-id="04e81-102">ご利用のアプリケーションのデプロイ環境 (開発、テスト、運用など) ごとに、個別のコンテナーを作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="04e81-102">Good practice is to create a separate vault for each deployment environment of each of your applications, such as development, test, and production.</span></span> <span data-ttu-id="04e81-103">複数のアプリでシークレットを共有して使用することは可能ですが、攻撃者がコンテナーへの読み取りアクセスを得た場合の影響は、コンテナー内のシークレットの数と共に高まります。</span><span class="sxs-lookup"><span data-stu-id="04e81-103">It's possible to use vaults to share secrets across multiple apps, but the impact of an attacker gaining read access to a vault increases with the number of secrets in the vault.</span></span>

> [!TIP]
> <span data-ttu-id="04e81-104">1 つのアプリケーションに対し、異なる環境間でシークレットに同じ名前を使用する場合、アプリ内で変更する必要がある環境固有の構成は、コンテナー URL だけです。</span><span class="sxs-lookup"><span data-stu-id="04e81-104">If you use the same names for secrets across different environments for an application, the only environment-specific configuration that has to change in your app is the vault URL.</span></span>

<span data-ttu-id="04e81-105">コンテナーの作成には、初期構成は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="04e81-105">Creating a vault requires no initial configuration.</span></span> <span data-ttu-id="04e81-106">ユーザー ID にはシークレット管理アクセス許可の完全なセットが自動的に付与され、すぐにシークレットの追加を開始することができます。</span><span class="sxs-lookup"><span data-stu-id="04e81-106">Your user identity is automatically granted the full set of secret management permissions and you can start adding secrets immediately.</span></span> <span data-ttu-id="04e81-107">コンテナーを作成したら、シークレットの追加と管理は、Azure 管理インターフェイス (Azure portal、Azure CLI、Azure PowerShell など) から実行できます。</span><span class="sxs-lookup"><span data-stu-id="04e81-107">Once you have a vault, adding and managing secrets can be done from any Azure administrative interface, including the Azure portal, the Azure CLI, and Azure PowerShell.</span></span> <span data-ttu-id="04e81-108">そのコンテナーを使用するようにアプリケーションを設定する場合、適切なアクセス許可を割り当てる必要があります。これについては次のユニットで説明します。</span><span class="sxs-lookup"><span data-stu-id="04e81-108">When you set up your application to use the vault, you'll need to assign the correct permissions to it; we'll see that in the next unit.</span></span>

## <a name="vault-authentication-and-permissions"></a><span data-ttu-id="04e81-109">コンテナーの認証とアクセス許可</span><span class="sxs-lookup"><span data-stu-id="04e81-109">Vault authentication and permissions</span></span>

<span data-ttu-id="04e81-110">Azure Key Vault の API では、Azure Active Directory を使用してユーザーとアプリケーションを認証します。</span><span class="sxs-lookup"><span data-stu-id="04e81-110">Azure Key Vault's API uses Azure Active Directory to authenticate users and applications.</span></span> <span data-ttu-id="04e81-111">コンテナー アクセス ポリシーは*アクション*に基づいており、コンテナー全体に適用されます。</span><span class="sxs-lookup"><span data-stu-id="04e81-111">Vault access policies are based on *actions*, and are applied across an entire vault.</span></span> <span data-ttu-id="04e81-112">たとえば、コンテナーへの **Get** (シークレット値の読み取り)、**List** (すべてのシークレットの名前を一覧表示)、および **Set** (シークレット値の作成または更新) アクセス許可を持つアプリケーションでは、シークレットの作成、シークレット名の一覧表示、およびそのコンテナー内のすべてのシークレット値を設定することができます。</span><span class="sxs-lookup"><span data-stu-id="04e81-112">For example, an application with **Get** (read secret values), **List** (list names of all secrets), and **Set** (create or update secret values) permissions to a vault is able to create secrets, list all secret names, and get and set all secret values in that vault.</span></span>

<span data-ttu-id="04e81-113">コンテナーで実行される*すべて*のアクションには、認証と承認が必要です &mdash; いかなる種類の匿名アクセスも許可されることはありません。</span><span class="sxs-lookup"><span data-stu-id="04e81-113">*All* actions performed on a vault require authentication and authorization &mdash; there is no way to grant any kind of anonymous access.</span></span>

> [!TIP]
> <span data-ttu-id="04e81-114">開発者とアプリにコンテナーへのアクセスを許可する場合は、必要最小限のアクセス許可のセットのみを付与します。</span><span class="sxs-lookup"><span data-stu-id="04e81-114">When granting vault access to developers and apps, grant only the minimum set of permissions needed.</span></span> <span data-ttu-id="04e81-115">アクセス許可の制限は、コードのバグに起因する事故を回避し、資格情報の盗難やアプリに挿入された悪意のあるコードによる影響を軽減することに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="04e81-115">Permissions restrictions help avoid accidents caused by code bugs and reduce the impact of stolen credentials or malicious code injected into your app.</span></span>

<span data-ttu-id="04e81-116">通常、開発者に必要なのは、開発環境コンテナーへの **Get** アクセス許可と **List** アクセス許可のみです。</span><span class="sxs-lookup"><span data-stu-id="04e81-116">Developers will usually only need **Get** and **List** permissions to a development-environment vault.</span></span> <span data-ttu-id="04e81-117">主任開発者または上級開発者には、必要に応じてシークレットを変更したり追加したりするため、コンテナーへの完全なアクセス許可が必要になります。</span><span class="sxs-lookup"><span data-stu-id="04e81-117">A lead or senior developer will need full permissions to the vault to change and add secrets when necessary.</span></span> <span data-ttu-id="04e81-118">運用環境コンテナーへの完全なアクセス許可は、通常は上級の運用スタッフのために予約されています。</span><span class="sxs-lookup"><span data-stu-id="04e81-118">Full permissions to production-environment vaults are typically reserved for senior operations staff.</span></span>

<span data-ttu-id="04e81-119">アプリでは、通常は **Get** アクセス許可のみが必要です。</span><span class="sxs-lookup"><span data-stu-id="04e81-119">For apps, typically only **Get** permissions are required.</span></span> <span data-ttu-id="04e81-120">一部のアプリでは、アプリの実装方法によっては、**List** アクセス許可が必要な場合があります。</span><span class="sxs-lookup"><span data-stu-id="04e81-120">Some apps may require **List** depending on the way the app is implemented.</span></span> <span data-ttu-id="04e81-121">このモジュールの演習で実装するアプリでは、コンテナーからシークレットを読み取るために使用する手法により、**List** アクセス許可が必要になります。</span><span class="sxs-lookup"><span data-stu-id="04e81-121">The app we'll implement in this module's exercise requires the **List** permission because of the technique it uses to read secrets from the vault.</span></span>

## <a name="exercise"></a><span data-ttu-id="04e81-122">演習</span><span class="sxs-lookup"><span data-stu-id="04e81-122">Exercise</span></span>

<span data-ttu-id="04e81-123">経営陣は、アプリケーション シークレットについて会社が抱えているすべての問題を考慮して、他の開発者を正しい道に誘導するための小規模なスターター アプリを作成することをあなたに求めています。</span><span class="sxs-lookup"><span data-stu-id="04e81-123">Given all the trouble the company's been having with application secrets, management has asked you to create a small starter app to set the other developers on the right path.</span></span> <span data-ttu-id="04e81-124">アプリは、できる限り簡単かつ安全にシークレットを管理するためのベスト プラクティスを示す必要があります。</span><span class="sxs-lookup"><span data-stu-id="04e81-124">The app needs to demonstrate best practices for managing secrets as simply and securely as possible.</span></span>

<span data-ttu-id="04e81-125">手始めに、コンテナーを作成し、1 つのシークレットを格納します。</span><span class="sxs-lookup"><span data-stu-id="04e81-125">To start, you'll create a vault and store one secret.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="04e81-126">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="04e81-126">Create a resource group</span></span>

<span data-ttu-id="04e81-127">この演習のすべてのリソース用に、`keyvault-exercise-group` というリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="04e81-127">Create a resource group called `keyvault-exercise-group` for all of the resources in this exercise.</span></span> <span data-ttu-id="04e81-128">このモジュールの最後に、このリソース グループを削除して、一度にすべてクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="04e81-128">At the end of this module, we'll be deleting this resource group to clean up everything at once.</span></span> <span data-ttu-id="04e81-129">この演習でのすべての場所として `eastus` を使用します。</span><span class="sxs-lookup"><span data-stu-id="04e81-129">We'll use `eastus` as the location for everything in this exercise.</span></span>

<span data-ttu-id="04e81-130">右側の Azure Cloud Shell のターミナルを使用して、次の Azure CLI コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="04e81-130">Use the Azure Cloud Shell terminal on the right to run the following Azure CLI command.</span></span> <span data-ttu-id="04e81-131">これにより、ご利用のサブスクリプションにリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="04e81-131">This will create the resource group in your subscription.</span></span>

```azurecli
az group create --name keyvault-exercise-group --location eastus
```

### <a name="create-the-vault-and-store-the-secret-in-it"></a><span data-ttu-id="04e81-132">コンテナーを作成してシークレットを格納する</span><span class="sxs-lookup"><span data-stu-id="04e81-132">Create the vault and store the secret in it</span></span>

<span data-ttu-id="04e81-133">次に、コンテナーを作成し、そこにシークレットを格納します。</span><span class="sxs-lookup"><span data-stu-id="04e81-133">Next, we'll create the vault and store our secret in it.</span></span>

<span data-ttu-id="04e81-134">**Key Vault 名はグローバルに一意である必要があるため、一意の名前を選択する必要があります**。</span><span class="sxs-lookup"><span data-stu-id="04e81-134">**Key Vault names must be globally unique, so you'll need to pick a unique name**.</span></span> <span data-ttu-id="04e81-135">Vault の名前は、3 - 24 文字の長さで、英数字と特殊文字のハイフン (-) のみを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="04e81-135">Vault names must be 3-24 characters long and contain only alphanumeric characters and dashes.</span></span>

```azurecli
az keyvault create --name <your-unique-vault-name> --resource-group keyvault-exercise-group --location eastus
```

<span data-ttu-id="04e81-136">完了すると、新しいコンテナーを記述する JSON 出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="04e81-136">When it finishes, you'll see JSON output describing the new vault.</span></span>

<span data-ttu-id="04e81-137">シークレットを追加します。シークレットは **SecretPassword** という名前で、**reindeer_flotilla** の値を持ちます。</span><span class="sxs-lookup"><span data-stu-id="04e81-137">Now add the secret: our secret will be named **SecretPassword** with a value of **reindeer_flotilla**.</span></span>

```azurecli
az keyvault secret set --name SecretPassword --value reindeer_flotilla --vault-name <your-unique-vault-name>
```

<span data-ttu-id="04e81-138">コンテナー名をメモしておきます &mdash; すぐにもう一度必要になります。</span><span class="sxs-lookup"><span data-stu-id="04e81-138">Make a note of the vault name &mdash; you'll be needing it again soon.</span></span>

<span data-ttu-id="04e81-139">この後すぐにアプリケーションのコードを記述しますが、まず、アプリがコンテナーへの認証を行う方法について少し説明する必要があります。</span><span class="sxs-lookup"><span data-stu-id="04e81-139">We'll write the code for our application shortly, but first we need to learn a little bit about how our app is going to authenticate to a vault.</span></span>