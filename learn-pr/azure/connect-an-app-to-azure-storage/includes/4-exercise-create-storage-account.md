<span data-ttu-id="d2618-101">アプリの準備ができたので、次に操作するために Azure ストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="d2618-101">Now that we have an app, we need an Azure storage account to work with.</span></span> <span data-ttu-id="d2618-102">Azure ストレージ アカウントは、「**Azure ストレージ アカウントの作成**」モジュールで Azure portal を使って作成しました。</span><span class="sxs-lookup"><span data-stu-id="d2618-102">We created one using the Azure portal in the **Create an Azure storage account** module.</span></span> <span data-ttu-id="d2618-103">今度は、Azure CLI を使ってみましょう。</span><span class="sxs-lookup"><span data-stu-id="d2618-103">Let's use the Azure CLI this time.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="use-the-azure-cli-to-create-an-azure-storage-account"></a><span data-ttu-id="d2618-104">Azure CLI を使用して Azure ストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="d2618-104">Use the Azure CLI to create an Azure storage account</span></span>

<span data-ttu-id="d2618-105">`az storage account create` コマンドを使用して、新しいストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="d2618-105">We will use the `az storage account create` command to create a new storage account.</span></span> <span data-ttu-id="d2618-106">このコマンドには、望みどおりのストレージ アカウントを構成する上で指定する必要がある (または指定すべきである) パラメーターがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="d2618-106">It takes several parameters which we either need to supply (or should) to configure it the way we want.</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="d2618-107">オプションオプション</span><span class="sxs-lookup"><span data-stu-id="d2618-107">Option</span></span> | <span data-ttu-id="d2618-108">説明</span><span class="sxs-lookup"><span data-stu-id="d2618-108">Description</span></span> |
> |--------|-------------|
> | `--name` | <span data-ttu-id="d2618-109">**ストレージ アカウント名**です。</span><span class="sxs-lookup"><span data-stu-id="d2618-109">A **Storage account name**.</span></span> <span data-ttu-id="d2618-110">この名前は、アカウント内のデータへのアクセスで使用されるパブリック URL を作成するのに使用されます。</span><span class="sxs-lookup"><span data-stu-id="d2618-110">The name will be used to generate the public URL used to access the data in the account.</span></span> <span data-ttu-id="d2618-111">Azure 内の既存のすべてのストレージ アカウントを通して一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="d2618-111">It must be unique across all existing storage account names in Azure.</span></span> <span data-ttu-id="d2618-112">文字数は 3 から 24 文字にする必要があります。使用できる文字は英小文字と数字のみです。</span><span class="sxs-lookup"><span data-stu-id="d2618-112">It must be 3 to 24 characters long and can contain only lowercase letters and numbers.</span></span> |
> | `--resource-group` | <span data-ttu-id="d2618-113"><rgn>[サンド ボックス リソース グループ名]</rgn> を使用して、ストレージ アカウントを無料のサンド ボックスに配置します。</span><span class="sxs-lookup"><span data-stu-id="d2618-113">Use <rgn>[Sandbox resource group name]</rgn> to place the storage account into the free sandbox.</span></span> |
> | `--location` | <span data-ttu-id="d2618-114">お近くの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="d2618-114">Select a location near you.</span></span> |
> | `--kind` | <span data-ttu-id="d2618-115">これにより、ストレージ アカウントの_種類_が決まります。</span><span class="sxs-lookup"><span data-stu-id="d2618-115">This determines the storage account _type_.</span></span> <span data-ttu-id="d2618-116">オプションには、BlobStorage、Storage、StorageV2 があります。</span><span class="sxs-lookup"><span data-stu-id="d2618-116">Options include BlobStorage, Storage, and StorageV2.</span></span> |
> | `--sku` | <span data-ttu-id="d2618-117">これにより、ストレージ アカウントのパフォーマンスとレプリケーション モデルが決まります。</span><span class="sxs-lookup"><span data-stu-id="d2618-117">This decides the storage account performance and replication model.</span></span> <span data-ttu-id="d2618-118">オプションには、Premium_LRS、Standard_GRS、Standard_LRS、Standard_RAGRS、Standard_ZRS があります。</span><span class="sxs-lookup"><span data-stu-id="d2618-118">Options include Premium_LRS, Standard_GRS, Standard_LRS, Standard_RAGRS, and Standard_ZRS.</span></span> |
> | `--access-tier` | <span data-ttu-id="d2618-119">**アクセス層**は BLOB ストレージでのみ使用されます。使用できるオプションはクールとホットです。</span><span class="sxs-lookup"><span data-stu-id="d2618-119">The **Access tier** is only used for Blob storage, available options are Cool and Hot.</span></span> <span data-ttu-id="d2618-120">**ホット アクセス層**は頻繁にアクセスされるデータに最適です。**クール アクセス層**はアクセス頻度の低いデータに適しています。</span><span class="sxs-lookup"><span data-stu-id="d2618-120">The **Hot Access Tier** is ideal for frequently accessed data, and the **Cool Access Tier** is better for infrequently accessed data.</span></span> <span data-ttu-id="d2618-121">これによって設定されるのは_既定_値のみであることに注意してください。BLOB を作成する場合は、データに対して異なる値を設定できます。</span><span class="sxs-lookup"><span data-stu-id="d2618-121">Note that this only sets the _default_ value - when you create a Blob, you can set a different value for the data.</span></span> |
    
<span data-ttu-id="d2618-122">上記の表を使用して、Cloud Shell 内の右側にコマンド行を作成することで、アカウントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="d2618-122">Use the above table to craft a command line in the Cloud Shell on the right to create the account.</span></span>
- <span data-ttu-id="d2618-123">一意の名前を使用します。"photostore" と自分のイニシャルとランダムな数字を組み合わせたような名前にすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d2618-123">Use a unique name, we recommend something like "photostore" with your initials and a random number.</span></span> <span data-ttu-id="d2618-124">名前が一意ではない場合は、エラーが発生します。</span><span class="sxs-lookup"><span data-stu-id="d2618-124">You will get an error if it's not unique.</span></span>
- <span data-ttu-id="d2618-125">通常は、ご自分のアプリ リソースを保持するリソース グループを新規に作成します。この場合は、サンドボックス リソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="d2618-125">Normally you would create a new resource group to hold your app resources, in this case use the Sandbox resource group.</span></span>
- <span data-ttu-id="d2618-126">**sku** には "Standard_LRS" を使用します。これには、この例に適している、ローカル レプリケーションを備えた Standard Storage が使用されます。</span><span class="sxs-lookup"><span data-stu-id="d2618-126">Use "Standard_LRS" for the **sku**, this will use standard storage with local replication which is fine for this example.</span></span>
- <span data-ttu-id="d2618-127">**アクセス層**には "コールド" を使用します。</span><span class="sxs-lookup"><span data-stu-id="d2618-127">Use "Cold" for the **Access Tier**.</span></span>

### <a name="selecting-a-location"></a><span data-ttu-id="d2618-128">場所の選択</span><span class="sxs-lookup"><span data-stu-id="d2618-128">Selecting a location</span></span>
<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

### <a name="example-command"></a><span data-ttu-id="d2618-129">コマンドの例</span><span class="sxs-lookup"><span data-stu-id="d2618-129">Example command</span></span>

```bash
az storage account create \
        --name <name> \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --location <region> \
        --kind StorageV2 \
        --sku Standard_LRS \ 
        --access-tier Cold
```

> [!TIP]
> <span data-ttu-id="d2618-130">ストレージ アカウント用のオプションの詳細に関心がある場合は、オプションの内容を掘り下げて説明している「**Azure ストレージ アカウントの作成**」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d2618-130">If you are interested in exploring the options for the storage account, make sure to go through the **Create an Azure storage account** where we go through them in depth.</span></span>

<span data-ttu-id="d2618-131">アカウントがデプロイされるまで数分かかります。</span><span class="sxs-lookup"><span data-stu-id="d2618-131">It will take a few minutes to deploy the account.</span></span> <span data-ttu-id="d2618-132">Azure がそれに対して動作している間、このアカウントで使用する API について見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d2618-132">While Azure is working on that, let's explore the APIs we'll use with this account.</span></span>