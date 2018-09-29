<span data-ttu-id="a5a24-101">アプリの準備ができたので、次に操作するために Azure ストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="a5a24-101">Now that we have an app, we need an Azure storage account to work with.</span></span>

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a><span data-ttu-id="a5a24-102">Azure CLI を使用して Azure ストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="a5a24-102">Use the Azure CLI to create an Azure storage account</span></span>

<span data-ttu-id="a5a24-103">`az storage account create` コマンドを使用して新しいストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="a5a24-103">We will use the `az storage account create` command to create a new storage account.</span></span> <span data-ttu-id="a5a24-104">ストレージ アカウントの構成を制御するパラメーターがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="a5a24-104">There are several parameters to control the configuration of the storage account.</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="a5a24-105">オプション</span><span class="sxs-lookup"><span data-stu-id="a5a24-105">Option</span></span> | <span data-ttu-id="a5a24-106">説明</span><span class="sxs-lookup"><span data-stu-id="a5a24-106">Description</span></span> |
> |--------|-------------|
> | `--name` | <span data-ttu-id="a5a24-107">**ストレージ アカウント名**です。</span><span class="sxs-lookup"><span data-stu-id="a5a24-107">A **Storage account name**.</span></span> <span data-ttu-id="a5a24-108">この名前は、アカウント内のデータへのアクセスで使用されるパブリック URL を作成するのに使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5a24-108">The name will be used to generate the public URL used to access the data in the account.</span></span> <span data-ttu-id="a5a24-109">Azure に存在するストレージ アカウント名の中で一意になる名前を付ける必要があります。</span><span class="sxs-lookup"><span data-stu-id="a5a24-109">It must be unique across all existing storage account names in Azure.</span></span> <span data-ttu-id="a5a24-110">名前の長さは 3 から 24 文字にする必要があり、小文字と数字のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a5a24-110">It must be 3 to 24 characters long and can contain only lowercase letters and numbers.</span></span> |
> | `--resource-group` | <span data-ttu-id="a5a24-111">**<rgn>[サンド ボックス リソース グループ名]</rgn>** を使用し、ストレージ アカウントを無料のサンド ボックスに配置します。</span><span class="sxs-lookup"><span data-stu-id="a5a24-111">Use **<rgn>[sandbox resource group name]</rgn>** to place the storage account into the free sandbox.</span></span> |
> | `--location` | <span data-ttu-id="a5a24-112">お近くの場所を選択します (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="a5a24-112">Select a location near you (see below).</span></span> |
> | `--kind` | <span data-ttu-id="a5a24-113">これにより、ストレージ アカウントの_種類_が決まります。</span><span class="sxs-lookup"><span data-stu-id="a5a24-113">This determines the storage account _type_.</span></span> <span data-ttu-id="a5a24-114">オプションには `BlobStorage`、`Storage`、`StorageV2` があります。</span><span class="sxs-lookup"><span data-stu-id="a5a24-114">Options include `BlobStorage`, `Storage`, and `StorageV2`.</span></span> |
> | `--sku` | <span data-ttu-id="a5a24-115">これにより、ストレージ アカウントのパフォーマンスとレプリケーション モデルが決まります。</span><span class="sxs-lookup"><span data-stu-id="a5a24-115">This decides the storage account performance and replication model.</span></span> <span data-ttu-id="a5a24-116">オプションには `Premium_LRS`、`Standard_GRS`、`Standard_LRS`、`Standard_RAGRS`、`Standard_ZRS` があります。</span><span class="sxs-lookup"><span data-stu-id="a5a24-116">Options include `Premium_LRS`, `Standard_GRS`, `Standard_LRS`, `Standard_RAGRS`, and `Standard_ZRS`.</span></span> |
> | `--access-tier` | <span data-ttu-id="a5a24-117">**アクセス層**は BLOB ストレージでのみ使用されます。使用できるオプションは [`Cool` \| `Hot`] です。</span><span class="sxs-lookup"><span data-stu-id="a5a24-117">The **Access tier** is only used for Blob storage, available options are [`Cool` \| `Hot`].</span></span> <span data-ttu-id="a5a24-118">**ホット アクセス層**は頻繁にアクセスされるデータに最適です。**クール アクセス層**はアクセス頻度の低いデータに適しています。</span><span class="sxs-lookup"><span data-stu-id="a5a24-118">The **Hot Access Tier** is ideal for frequently accessed data, and the **Cool Access Tier** is better for infrequently accessed data.</span></span> <span data-ttu-id="a5a24-119">これによって設定されるのは_既定_値のみであることに注意してください。BLOB を作成する場合は、データに対して異なる値を設定できます。</span><span class="sxs-lookup"><span data-stu-id="a5a24-119">Note that this only sets the _default_ value&mdash;when you create a Blob, you can set a different value for the data.</span></span> |
    
<span data-ttu-id="a5a24-120">上記の表を使用して、Cloud Shell 内の右側にコマンド行を作成することで、アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="a5a24-120">Use the above table to craft a command line in the Cloud Shell on the right to create the account.</span></span>
- <span data-ttu-id="a5a24-121">一意の名前を使用してください。</span><span class="sxs-lookup"><span data-stu-id="a5a24-121">Use a unique name.</span></span> <span data-ttu-id="a5a24-122">"photostore" と自分のイニシャルとランダムな数字を組み合わせたような名前にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="a5a24-122">We recommend something like "photostore" with your initials and a random number.</span></span> <span data-ttu-id="a5a24-123">名前が一意ではない場合は、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="a5a24-123">You will get an error if it's not unique.</span></span>
- <span data-ttu-id="a5a24-124">通常は、自分のアプリ リソースを保持するリソース グループを新規に作成します。ただし、その場合、サンドボックス リソース グループ "**<rgn>[サンドボックス リソース グループ名]</rgn>**" を使用します。</span><span class="sxs-lookup"><span data-stu-id="a5a24-124">Normally you would create a new resource group to hold your app resources, but in this case, use the sandbox resource group "**<rgn>[sandbox resource group name]</rgn>**".</span></span>
- <span data-ttu-id="a5a24-125">**sku** には "Standard_LRS" を使用します。</span><span class="sxs-lookup"><span data-stu-id="a5a24-125">Use "Standard_LRS" for the **sku**.</span></span> <span data-ttu-id="a5a24-126">これには、この例に適している、ローカル レプリケーションを備えた Standard Storage が使用されます。</span><span class="sxs-lookup"><span data-stu-id="a5a24-126">This will use standard storage with local replication, which is fine for this example.</span></span>
- <span data-ttu-id="a5a24-127">**アクセス層**には "クール" を使用します。</span><span class="sxs-lookup"><span data-stu-id="a5a24-127">Use "Cool" for the **Access Tier**.</span></span>

### <a name="selecting-a-location"></a><span data-ttu-id="a5a24-128">場所の選択</span><span class="sxs-lookup"><span data-stu-id="a5a24-128">Selecting a location</span></span>
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a><span data-ttu-id="a5a24-129">コマンドの例</span><span class="sxs-lookup"><span data-stu-id="a5a24-129">Example command</span></span>

```azurecli
az storage account create \
        --name <name> \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \
        --access-tier Cool
```

> [!TIP]
> <span data-ttu-id="a5a24-130">ストレージ アカウント用のオプションの詳細に関心がある場合は、オプションの内容を掘り下げて説明している「**Azure ストレージ アカウントの作成**」モジュールを参照してください。</span><span class="sxs-lookup"><span data-stu-id="a5a24-130">If you are interested in exploring the options for the storage account, make sure to go through the **Create an Azure storage account** module where we go through them in depth.</span></span>

<span data-ttu-id="a5a24-131">アカウントがデプロイされるまで数分かかります。</span><span class="sxs-lookup"><span data-stu-id="a5a24-131">It will take a few minutes to deploy the account.</span></span> <span data-ttu-id="a5a24-132">Azure がそれに対して動作している間、このアカウントで使用する API について見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="a5a24-132">While Azure is working on that, let's explore the APIs we'll use with this account.</span></span>
