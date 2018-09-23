<span data-ttu-id="1d262-101">トラフィックの急増により中間層に負荷がかかることがわかりました。</span><span class="sxs-lookup"><span data-stu-id="1d262-101">You've discovered that spikes in traffic can overwhelm our middle-tier.</span></span> <span data-ttu-id="1d262-102">これに対処するため、アーティクル アップロード アプリケーションのフロントエンドと中間層の間にキューを追加することにしました。</span><span class="sxs-lookup"><span data-stu-id="1d262-102">To deal with this, you've decided to add a queue between the front-end and the middle tier in your article-upload application.</span></span>

<span data-ttu-id="1d262-103">キューを作成するための最初の手順は、データを格納する Azure ストレージ アカウントの作成です。</span><span class="sxs-lookup"><span data-stu-id="1d262-103">The first step in creating a queue is to create the Azure Storage Account that will store our data.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-storage-account-with-the-azure-cli"></a><span data-ttu-id="1d262-104">Azure CLI を使用してストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="1d262-104">Create a Storage Account with the Azure CLI</span></span>

> [!TIP] 
> <span data-ttu-id="1d262-105">通常は、関連するすべてのリソースを保持する "_リソース グループ_" を作成することで、新しいプロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="1d262-105">Normally, you'd start a new project by creating a _resource group_ to hold all the associated resources.</span></span> <span data-ttu-id="1d262-106">このケースでは、Azure サンドボックスを使用します。このサンドボックスでは、<rgn>[サンドボックス リソース グループ名]</rgn> という名前のリソース グループが提供されます。</span><span class="sxs-lookup"><span data-stu-id="1d262-106">In this case, we'll be using the Azure Sandbox which provides a resource group named <rgn>[Sandbox resource group name]</rgn>.</span></span>

<span data-ttu-id="1d262-107">`az storage account create` コマンドを使用して、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d262-107">Use the `az storage account create` command to create the storage account.</span></span> <span data-ttu-id="1d262-108">コマンドは、右側の Cloud Shell ウィンドウに入力できます。</span><span class="sxs-lookup"><span data-stu-id="1d262-108">You can type the command into the Cloud Shell window on the right.</span></span>

<span data-ttu-id="1d262-109">コマンドには、いくつかのパラメーターが必要です。</span><span class="sxs-lookup"><span data-stu-id="1d262-109">The command needs several parameters:</span></span>

| <span data-ttu-id="1d262-110">パラメーター</span><span class="sxs-lookup"><span data-stu-id="1d262-110">Parameter</span></span> | <span data-ttu-id="1d262-111">値</span><span class="sxs-lookup"><span data-stu-id="1d262-111">Value</span></span> |
|-----------|-------|
| `--name`  | <span data-ttu-id="1d262-112">名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="1d262-112">Sets the name.</span></span> <span data-ttu-id="1d262-113">ストレージ アカウントはその名前を使用してパブリック URL を生成するため、一意でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="1d262-113">Remember that storage accounts use the name to generate a public URL - so it has to be unique.</span></span> <span data-ttu-id="1d262-114">また、アカウント名の長さは 3 から 24 文字で、数字と小文字のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1d262-114">In addition, the account name must be between 3 and 24 characters, and be composed of numbers and lowercase letters only.</span></span> <span data-ttu-id="1d262-115">**articles** のプレフィックスとランダムな数値のサフィックスを使用することをお勧めします。ただし、任意の文字列を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="1d262-115">We recommend you use the prefix **articles** with a random number suffix but you can use whatever you like.</span></span> |
| `-g`        | <span data-ttu-id="1d262-116">**リソース グループ**を指定します。_<rgn>[サンドボックス リソース グループ名]</rgn>_ を値として使用します。</span><span class="sxs-lookup"><span data-stu-id="1d262-116">Supplies the **Resource Group**, use _<rgn>[Sandbox resource group name]</rgn>_ as the value.</span></span> |
| `--kind`    | <span data-ttu-id="1d262-117">**ストレージ アカウントの種類**を設定します。_StorageV2_ を使用して汎用 V2 アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d262-117">Sets the **Storage Account type** - use _StorageV2_ to create a general-purpose V2 account.</span></span> |
| `--sku`     | <span data-ttu-id="1d262-118">**レプリケーションとストレージの種類**を設定します。既定値は _Standard_RAGRS_ です。</span><span class="sxs-lookup"><span data-stu-id="1d262-118">Sets the **Replication and Storage type**, it defaults to _Standard_RAGRS_.</span></span> <span data-ttu-id="1d262-119">単にデータ センター内でローカルで冗長になるだけの _Standard_LRS_ を使用しましょう。</span><span class="sxs-lookup"><span data-stu-id="1d262-119">Let's use _Standard_LRS_ which means it's only locally redundant within the data center.</span></span> |
| `-l`        | <span data-ttu-id="1d262-120">リソース グループの所有者に依存しない**場所**を設定します。</span><span class="sxs-lookup"><span data-stu-id="1d262-120">Sets the **Location** independent of the resource group owner.</span></span> <span data-ttu-id="1d262-121">これは省略可能ですが、これにより、リソース グループとは異なるリージョンにキューを配置できるようになります。</span><span class="sxs-lookup"><span data-stu-id="1d262-121">It's optional, but you can use it to place the queue in a different region than the resource group.</span></span> <span data-ttu-id="1d262-122">自分の近くに配置します。下のサンドボックスで利用可能なリージョンの一覧から選択します。</span><span class="sxs-lookup"><span data-stu-id="1d262-122">Place it close to you, choosing from the below list of available regions in the Sandbox.</span></span> |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="1d262-123">上記のパラメーターを使用するコマンド ラインの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1d262-123">Here's an example command line that uses the above parameters.</span></span> <span data-ttu-id="1d262-124">`--name` パラメーターを必ず変更してください。</span><span class="sxs-lookup"><span data-stu-id="1d262-124">Make sure to change the `--name` parameter.</span></span>

```azurecli
az storage account create --name [unique-name] -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 --sku Standard_LRS
```

<!-- Paste tip-->
[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
