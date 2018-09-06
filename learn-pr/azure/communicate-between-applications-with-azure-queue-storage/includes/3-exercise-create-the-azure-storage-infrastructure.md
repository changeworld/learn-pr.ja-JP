<span data-ttu-id="ba3fb-101">トラフィックの急増により中間層に負荷がかかることがわかりました。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-101">You've discovered that spikes in traffic can overwhelm our middle-tier.</span></span> <span data-ttu-id="ba3fb-102">これに対処するため、アーティクル アップロード アプリケーションのフロントエンドと中間層の間にキューを追加することにしました。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-102">To deal with this, you've decided to add a queue between the front-end and the middle tier in your article-upload application.</span></span>

<span data-ttu-id="ba3fb-103">キューを作成するための最初の手順は、データを格納する Azure ストレージ アカウントの作成です。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-103">The first step in creating a queue is to create the Azure Storage Account that will store our data.</span></span>

## <a name="create-a-storage-account-with-the-azure-cli"></a><span data-ttu-id="ba3fb-104">Azure CLI を使用してストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="ba3fb-104">Create a Storage Account with the Azure CLI</span></span>

<span data-ttu-id="ba3fb-105">最初に、ストレージ アカウントを保持する Azure リソース グループを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-105">To start with, let's create an Azure Resource Group to hold our storage account.</span></span>

1. <span data-ttu-id="ba3fb-106">右側の Cloud shell で選択肢が与えられている場合は、Bash を選択します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-106">In the Cloud shell on the right, select Bash if you are given a choice.</span></span>

2. <span data-ttu-id="ba3fb-107">`az group create` の Azure CLI コマンドを使用して新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-107">Use the `az group create` Azure CLI command to create a new resource group.</span></span> <span data-ttu-id="ba3fb-108">これに、**ExerciseResources** という名前を付け、自分から近い場所に配置します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-108">Give it the name **ExerciseResources** and place it into a location near you.</span></span> 
    - <span data-ttu-id="ba3fb-109">次の例では、場所として "eastus" を使用しています。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-109">The example below uses "eastus" as the location.</span></span>

    ```azurecli
    az group create -n ExerciseResources --location eastus
    ```
        
2. <span data-ttu-id="ba3fb-110">次に、`az storage account create` のコマンドを使用して、実際のストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-110">Next, use the `az storage account create` command to create the actual Storage Account.</span></span> <span data-ttu-id="ba3fb-111">いくつかのパラメーターを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-111">You'll need to supply several parameters:</span></span>

| <span data-ttu-id="ba3fb-112">パラメーター</span><span class="sxs-lookup"><span data-stu-id="ba3fb-112">Parameter</span></span> | <span data-ttu-id="ba3fb-113">値</span><span class="sxs-lookup"><span data-stu-id="ba3fb-113">Value</span></span> |
|-----------|-------|
| `--name`  | <span data-ttu-id="ba3fb-114">名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-114">Sets the name.</span></span> <span data-ttu-id="ba3fb-115">ストレージ アカウントはその名前を使用してパブリック URL を生成するため、一意でなければなりません。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-115">Remember that storage accounts use the name to generate a public URL - so it has to be unique.</span></span> <span data-ttu-id="ba3fb-116">また、アカウント名の長さは 3 から 24 文字で、数字と小文字のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-116">In addition, the account name must be between 3 and 24 characters, and be composed of numbers and lowercase letters only.</span></span> <span data-ttu-id="ba3fb-117">**articles** のプレフィックスとランダムな数値のサフィックスを使用することをお勧めしますが、使用するかどうかは任意です。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-117">We recommend you use the prefix **articles** with a random number suffix but you can use whatever you like.</span></span> |
| `-g`        | <span data-ttu-id="ba3fb-118">リソース グループを指定します。値として **ExerciseResources** を使用します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-118">Supplies the Resource Group, use **ExerciseResources** as the value.</span></span> |
| `--kind`    | <span data-ttu-id="ba3fb-119">ストレージ アカウントの種類を設定します。**StorageV2** を使用して汎用 V2 アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-119">Sets the Storage Account type - use **StorageV2** to create a general-purpose V2 account.</span></span> |
| `-l`        | <span data-ttu-id="ba3fb-120">リソース グループの所有者に依存しない場所を設定します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-120">Sets the location independent of the Resource Group owner.</span></span> <span data-ttu-id="ba3fb-121">これは省略可能ですが、これにより、リソース グループとは異なるリージョンにキューを配置できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-121">It's optional, but you can use it to place the queue in a different region than the Resource Group.</span></span> |
| `--sku`     | <span data-ttu-id="ba3fb-122">アカウントの種類を設定します。**Standard_RAGRS** がデフォルトに設定されますが、これはこのデモには不適切です。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-122">Sets the account type, it defaults to **Standard_RAGRS**, which is overkill for this demonstration.</span></span> <span data-ttu-id="ba3fb-123">単にデータ センター内でローカルで冗長になる、**Standard_LRS** を使用しましょう。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-123">Let's use **Standard_LRS** which means it's only locally redundant within the data center.</span></span> |

<span data-ttu-id="ba3fb-124">上記のパラメーターを使用するコマンドラインの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-124">Here's an example command line that uses the above parameters.</span></span> <span data-ttu-id="ba3fb-125">このコマンドをコピー/貼り付ける場合は、`--name` パラメーターを一意のものに変更してください。</span><span class="sxs-lookup"><span data-stu-id="ba3fb-125">Make sure to change the `--name` parameter to be something unique if you decide to copy/paste this command.</span></span>

```azurecli
az storage account create --name <name> -g ExerciseResources --kind StorageV2 -l eastus --sku Standard_LRS
```
