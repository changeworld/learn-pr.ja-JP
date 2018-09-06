<span data-ttu-id="61e3e-101">既定では、Azure Container Instances はステートレスです。</span><span class="sxs-lookup"><span data-stu-id="61e3e-101">By default, Azure Container Instances are stateless.</span></span> <span data-ttu-id="61e3e-102">コンテナーがクラッシュまたは停止すると、すべての状態が失われます。</span><span class="sxs-lookup"><span data-stu-id="61e3e-102">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="61e3e-103">コンテナーの有効期間後も状態を保持するには、外部ストアからボリュームをマウントする必要があります。</span><span class="sxs-lookup"><span data-stu-id="61e3e-103">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span>

<span data-ttu-id="61e3e-104">このユニットでは、データの保管と取得のため、Azure コンテナー インスタンスに Azure ファイル共有をマウントします。</span><span class="sxs-lookup"><span data-stu-id="61e3e-104">In this unit, you will mount an Azure file share to an Azure Container Instance for data storage and retrieval.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="61e3e-105">Azure ファイル共有を作成する</span><span class="sxs-lookup"><span data-stu-id="61e3e-105">Create an Azure file share</span></span>

<span data-ttu-id="61e3e-106">Azure Container Instances で Azure ファイル共有を使用するには、まず、ファイル共有を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="61e3e-106">Before using an Azure file share with Azure Container Instances, you must create it.</span></span> <span data-ttu-id="61e3e-107">次のスクリプトを実行して、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-107">Run the following script to create a storage account.</span></span> <span data-ttu-id="61e3e-108">ストレージ アカウント名はグローバルに一意にする必要があるため、このスクリプトではベース文字列にランダムな値が追加されます。</span><span class="sxs-lookup"><span data-stu-id="61e3e-108">The storage account name must be globally unique, so the script adds a random value to the base string.</span></span>

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --location eastus --sku Standard_LRS
```

<span data-ttu-id="61e3e-109">次のコマンドを実行して、環境変数 *AZURE_STORAGE_CONNECTION_STRING* にストレージ アカウントの接続文字列を設定します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-109">Run the following command to place the storage account connection string into an environment variable named *AZURE_STORAGE_CONNECTION_STRING*.</span></span> <span data-ttu-id="61e3e-110">この環境変数は、Azure CLI によって認識され、ストレージ関連の操作で使用できます。</span><span class="sxs-lookup"><span data-stu-id="61e3e-110">This environment variable is understood by the Azure CLI and can be used in storage-related operations.</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group myResourceGroup --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv`
```

<span data-ttu-id="61e3e-111">`az storage share create` コマンドを使用してファイル共有を作成します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-111">Create the files share the `az storage share create` command.</span></span> <span data-ttu-id="61e3e-112">次の例では、*aci-share-demo* という名前の共有を作成します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-112">The following example creates a share with the name *aci-share-demo*.</span></span>

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a><span data-ttu-id="61e3e-113">ストレージの資格情報を取得する</span><span class="sxs-lookup"><span data-stu-id="61e3e-113">Get storage credentials</span></span>

<span data-ttu-id="61e3e-114">Azure Container Instances 内のボリュームとして Azure ファイル共有をマウントするには、ストレージ アカウント名、共有名、ストレージ アクセス キーの 3 つの値が必要です。</span><span class="sxs-lookup"><span data-stu-id="61e3e-114">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage access key.</span></span>

<span data-ttu-id="61e3e-115">上のスクリプトを使用した場合、ストレージ アカウント名は最後にランダムな値を付加して作成されています。</span><span class="sxs-lookup"><span data-stu-id="61e3e-115">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="61e3e-116">最後の文字列 (ランダムな部分を含む) のクエリを実行するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-116">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="61e3e-117">共有名は既にわかっているので (aci-share-demo)、あとはストレージ アカウント キーです。このキーを見つけるには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-117">The share name is already known (aci-share-demo), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a><span data-ttu-id="61e3e-118">コンテナーをデプロイしてボリュームをマウントする</span><span class="sxs-lookup"><span data-stu-id="61e3e-118">Deploy container and mount volume</span></span>

<span data-ttu-id="61e3e-119">Azure ファイル共有をコンテナーのボリュームとしてマウントするには、コンテナーを作成するときに、共有とボリュームのマウント ポイントを指定します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-119">To mount an Azure file share as a volume in a container, specify the share and volume mount point when you create the container.</span></span>

```azurecli
az container create \
    --resource-group myResourceGroup \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

<span data-ttu-id="61e3e-120">コンテナーが作成された後、パブリック IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-120">Once the container has been created, get the public IP address.</span></span>

```azurecli
az container show --resource-group myResourceGroup --name aci-demo-files --query ipAddress.ip -o tsv
```

<span data-ttu-id="61e3e-121">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-121">Open a browser and navigate to the containers IP address.</span></span> <span data-ttu-id="61e3e-122">シンプルなフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="61e3e-122">You will be presented with a simple form.</span></span> <span data-ttu-id="61e3e-123">いくつかのテキストを入力して、**[送信]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="61e3e-123">Enter some text and click **Submit**.</span></span> <span data-ttu-id="61e3e-124">これにより、ここで入力したテキストを本体とするファイルが Azure ファイル共有に作成されます。</span><span class="sxs-lookup"><span data-stu-id="61e3e-124">This action will create a file in the Azure File share with the text entered here as the file body.</span></span>

![Azure Container Instances のファイル共有のデモ。](../media-draft/files-ui.png)

<span data-ttu-id="61e3e-126">検証するには、Azure portal でファイル共有に移動し、ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="61e3e-126">To validate, you can navigate to the file share in the Azure portal and download the file.</span></span>

![コンテンツ デモ アプリケーションのサンプル テキスト ファイル。](../media-draft/sample-text.png)

<span data-ttu-id="61e3e-128">Azure ファイル共有に格納されているファイルとデータに何らかの価値がある場合は、この共有を新しいコンテナー インスタンスに再マウントして、ステートフルなデータを提供できます。</span><span class="sxs-lookup"><span data-stu-id="61e3e-128">If the files and data stored in the Azure File share were of any value, this share could be remounted on a new container instance to provide stateful data.</span></span>


## <a name="summary"></a><span data-ttu-id="61e3e-129">まとめ</span><span class="sxs-lookup"><span data-stu-id="61e3e-129">Summary</span></span>

<span data-ttu-id="61e3e-130">このユニットでは、Azure ファイル共有とコンテナーを作成し、そのコンテナーにファイル共有をマウントしました。</span><span class="sxs-lookup"><span data-stu-id="61e3e-130">In this unit, you have created an Azure File Share, a container, and have mounted the file share to that container.</span></span> <span data-ttu-id="61e3e-131">この共有を使用して、アプリケーション データを格納しました。</span><span class="sxs-lookup"><span data-stu-id="61e3e-131">This share was then used to store application data.</span></span>

<span data-ttu-id="61e3e-132">次のユニットでは、Container Instances の一般的なトラブルシューティング操作について説明します。</span><span class="sxs-lookup"><span data-stu-id="61e3e-132">In the next unit, you will work through some common Container Instances troubleshooting operations.</span></span>

