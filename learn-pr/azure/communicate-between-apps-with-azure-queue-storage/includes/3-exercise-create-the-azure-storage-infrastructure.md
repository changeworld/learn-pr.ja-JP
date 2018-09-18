<span data-ttu-id="7581b-101">トラフィックの急増により中間層に負荷がかかることがわかりました。</span><span class="sxs-lookup"><span data-stu-id="7581b-101">You've discovered that spikes in traffic can overwhelm our middle-tier.</span></span> <span data-ttu-id="7581b-102">これに対処するため、アーティクル アップロード アプリケーションのフロントエンドと中間層の間にキューを追加することにしました。</span><span class="sxs-lookup"><span data-stu-id="7581b-102">To deal with this, you've decided to add a queue between the front-end and the middle tier in your article-upload application.</span></span>

<span data-ttu-id="7581b-103">キューを作成するための最初の手順は、データを格納する Azure ストレージ アカウントの作成です。</span><span class="sxs-lookup"><span data-stu-id="7581b-103">The first step in creating a queue is to create the Azure Storage Account that will store our data.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-storage-account-with-the-azure-cli"></a><span data-ttu-id="7581b-104">Azure CLI を使用してストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="7581b-104">Create a Storage Account with the Azure CLI</span></span>

> [!TIP] 
> <span data-ttu-id="7581b-105">通常は、関連するすべてのリソースを保持する "_リソース グループ_" を作成することで、新しいプロジェクトを開始します。</span><span class="sxs-lookup"><span data-stu-id="7581b-105">Normally, you'd start a new project by creating a _resource group_ to hold all the associated resources.</span></span> <span data-ttu-id="7581b-106">このケースでは、Azure Sandbox を使用します。このサンドボックスでは、<rgn>[サンドボックス リソース グループ名]</rgn> という名前のリソース グループが提供されます。</span><span class="sxs-lookup"><span data-stu-id="7581b-106">In this case, we'll be using the Azure Sandbox which provides a resource group named <rgn>[Sandbox resource group name]</rgn>.</span></span>

1. <span data-ttu-id="7581b-107">右側の Cloud shell で選択肢がある場合は、Bash を選択します。</span><span class="sxs-lookup"><span data-stu-id="7581b-107">In the Cloud shell on the right, select Bash if you are given a choice.</span></span>

1. <span data-ttu-id="7581b-108">`az storage account create` コマンドを使用して、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7581b-108">Use the `az storage account create` command to create the storage account.</span></span> <span data-ttu-id="7581b-109">いくつかのパラメーターを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7581b-109">You'll need to supply several parameters:</span></span>

| <span data-ttu-id="7581b-110">パラメーター</span><span class="sxs-lookup"><span data-stu-id="7581b-110">Parameter</span></span> | <span data-ttu-id="7581b-111">値</span><span class="sxs-lookup"><span data-stu-id="7581b-111">Value</span></span> |
|-----------|-------|
| `--name`  | <span data-ttu-id="7581b-112">名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="7581b-112">Sets the name.</span></span> <span data-ttu-id="7581b-113">ストレージ アカウントはその名前を使用してパブリック URL を生成するため、一意でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="7581b-113">Remember that storage accounts use the name to generate a public URL - so it has to be unique.</span></span> <span data-ttu-id="7581b-114">また、アカウント名の長さは 3 から 24 文字で、数字と小文字のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="7581b-114">In addition, the account name must be between 3 and 24 characters, and be composed of numbers and lowercase letters only.</span></span> <span data-ttu-id="7581b-115">**articles** のプレフィックスとランダムな数値のサフィックスを使用することをお勧めします。ただし、任意の文字列を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="7581b-115">We recommend you use the prefix **articles** with a random number suffix but you can use whatever you like.</span></span> |
| `-g`        | <span data-ttu-id="7581b-116">**リソース グループ**を指定します。<rgn>[サンドボックス リソース グループ名]</rgn> を値として使用します。</span><span class="sxs-lookup"><span data-stu-id="7581b-116">Supplies the **Resource Group**, use <rgn>[Sandbox resource group name]</rgn> as the value.</span></span> |
| `--kind`    | <span data-ttu-id="7581b-117">**ストレージ アカウントの種類**を設定します。_StorageV2_ を使用して汎用 V2 アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="7581b-117">Sets the **Storage Account type** - use _StorageV2_ to create a general-purpose V2 account.</span></span> |
| `-l`        | <span data-ttu-id="7581b-118">リソース グループの所有者に依存しない**場所**を設定します。</span><span class="sxs-lookup"><span data-stu-id="7581b-118">Sets the **Location** independent of the Resource Group owner.</span></span> <span data-ttu-id="7581b-119">これは省略可能ですが、これにより、リソース グループとは異なるリージョンにキューを配置できるようになります。</span><span class="sxs-lookup"><span data-stu-id="7581b-119">It's optional, but you can use it to place the queue in a different region than the Resource Group.</span></span> |
| `--sku`     | <span data-ttu-id="7581b-120">**レプリケーションとストレージの種類**を設定します。既定値は _Standard_RAGRS_ です。</span><span class="sxs-lookup"><span data-stu-id="7581b-120">Sets the **Replication and Storage type**, it defaults to _Standard_RAGRS_.</span></span> <span data-ttu-id="7581b-121">単にデータ センター内でローカルで冗長になる、_Standard_LRS_ を使用しましょう。</span><span class="sxs-lookup"><span data-stu-id="7581b-121">Let's use _Standard_LRS_ which means it's only locally redundant within the data center.</span></span> |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="7581b-122">上記のパラメーターを使用するコマンド ラインの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="7581b-122">Here's an example command line that uses the above parameters.</span></span> <span data-ttu-id="7581b-123">このコマンドをコピーして貼り付ける場合は、`--name` パラメーターと `--location` パラメーターを変更してください。</span><span class="sxs-lookup"><span data-stu-id="7581b-123">Make sure to change the `--name` and `--location` parameters if you decide to copy/paste this command.</span></span>

```azurecli
az storage account create --name [unique-name] -g <rgn>[Sandbox resource group name]</rgn> --kind StorageV2 -l [location-name] --sku Standard_LRS
```