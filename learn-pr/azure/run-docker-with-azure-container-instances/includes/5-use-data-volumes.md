<span data-ttu-id="dd4e7-101">既定では、Azure Container Instances はステートレスです。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-101">By default, Azure Container Instances is stateless.</span></span> <span data-ttu-id="dd4e7-102">コンテナーがクラッシュまたは停止すると、すべての状態が失われます。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-102">If the container crashes or stops, all of its state is lost.</span></span> <span data-ttu-id="dd4e7-103">コンテナーの有効期間後も状態を保持するには、外部ストアからボリュームをマウントする必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-103">To persist state beyond the lifetime of the container, you must mount a volume from an external store.</span></span>

<span data-ttu-id="dd4e7-104">ここでは、データの保管と取得のため、Azure コンテナー インスタンスに Azure ファイル共有をマウントします。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-104">Here, you will mount an Azure file share to an Azure container instance for data storage and retrieval.</span></span>

## <a name="create-an-azure-file-share"></a><span data-ttu-id="dd4e7-105">Azure ファイル共有を作成する</span><span class="sxs-lookup"><span data-stu-id="dd4e7-105">Create an Azure file share</span></span>

<span data-ttu-id="dd4e7-106">次のスクリプトを実行して、ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-106">Run the following script to create a storage account.</span></span> <span data-ttu-id="dd4e7-107">ストレージ アカウント名はグローバルに一意にする必要があるため、このスクリプトではベース文字列にランダムな値が追加されます。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-107">The storage account name must be globally unique, so the script adds a random value to the base string:</span></span>

```azurecli
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM

az storage account create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --sku Standard_LRS \
    --location eastus
```

<span data-ttu-id="dd4e7-108">次のコマンドを実行して、*AZURE_STORAGE_CONNECTION_STRING* という名前の環境変数にストレージ アカウントの接続文字列を設定します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-108">Run the following command to place the storage account connection string into an environment variable named *AZURE_STORAGE_CONNECTION_STRING*.</span></span> <span data-ttu-id="dd4e7-109">この環境変数は、Azure CLI によって解釈され、次のストレージ関連の操作で使用できます。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-109">This environment variable is understood by the Azure CLI and can be used in storage-related operations:</span></span>

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string --resource-group <rgn>[sandbox resource group name]</rgn> --name $ACI_PERS_STORAGE_ACCOUNT_NAME --output tsv)
```

<span data-ttu-id="dd4e7-110">`az storage share create` コマンドを実行することで、ストレージ アカウントにファイル共有を作成します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-110">Create a file share in the storage account by running the `az storage share create` command.</span></span> <span data-ttu-id="dd4e7-111">次の例では、*aci-share-demo* という名前の共有を作成します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-111">The following example creates a share with the name *aci-share-demo*:</span></span>

```azurecli
az storage share create --name aci-share-demo
```

## <a name="get-storage-credentials"></a><span data-ttu-id="dd4e7-112">ストレージの資格情報を取得する</span><span class="sxs-lookup"><span data-stu-id="dd4e7-112">Get storage credentials</span></span>

<span data-ttu-id="dd4e7-113">Azure Container Instances 内のボリュームとして Azure ファイル共有をマウントするには、ストレージ アカウント名、共有名、ストレージ アカウント アクセス キーの 3 つの値が必要です。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-113">To mount an Azure file share as a volume in Azure Container Instances, you need three values: the storage account name, the share name, and the storage account access key.</span></span>

<span data-ttu-id="dd4e7-114">上のスクリプトを使用した場合、ストレージ アカウント名は最後にランダムな値を付加して作成されています。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-114">If you used the script above, the storage account name was created with a random value at the end.</span></span> <span data-ttu-id="dd4e7-115">最後の文字列 (ランダムな部分を含む) のクエリを実行するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-115">To query the final string (including the random portion), use the following commands:</span></span>

```azurecli
STORAGE_ACCOUNT=$(az storage account list --resource-group <rgn>[sandbox resource group name]</rgn> --query "[?contains(name,'$ACI_PERS_STORAGE_ACCOUNT_NAME')].[name]" --output tsv)
echo $STORAGE_ACCOUNT
```

<span data-ttu-id="dd4e7-116">共有名は既にわかっているので (aci-share-demo)、あとはストレージ アカウント キーです。このキーを見つけるには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-116">The share name is already known (aci-share-demo), so all that remains is the storage account key, which can be found using the following command:</span></span>

```azurecli
STORAGE_KEY=$(az storage account keys list --resource-group <rgn>[sandbox resource group name]</rgn> --account-name $STORAGE_ACCOUNT --query "[0].value" --output tsv)
echo $STORAGE_KEY
```

## <a name="deploy-container-and-mount-volume"></a><span data-ttu-id="dd4e7-117">コンテナーをデプロイしてボリュームをマウントする</span><span class="sxs-lookup"><span data-stu-id="dd4e7-117">Deploy container and mount volume</span></span>

<span data-ttu-id="dd4e7-118">Azure ファイル共有をコンテナーのボリュームとしてマウントするには、コンテナーを作成するときに、共有とボリュームのマウント ポイントを指定します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-118">To mount an Azure file share as a volume in a container, specify the share and volume mount point when you create the container:</span></span>

```azurecli
az container create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --image microsoft/aci-hellofiles \
    --location eastus \
    --ports 80 \
    --ip-address Public \
    --azure-file-volume-account-name $ACI_PERS_STORAGE_ACCOUNT_NAME \
    --azure-file-volume-account-key $STORAGE_KEY \
    --azure-file-volume-share-name aci-share-demo \
    --azure-file-volume-mount-path /aci/logs/
```

<span data-ttu-id="dd4e7-119">コンテナーが作成された後、パブリック IP アドレスを取得します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-119">Once the container has been created, get the public IP address:</span></span>

```azurecli
az container show \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name aci-demo-files \
    --query ipAddress.ip \
    --output tsv
```

<span data-ttu-id="dd4e7-120">ブラウザーを開き、コンテナーの IP アドレスに移動します。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-120">Open a browser and navigate to the container's IP address.</span></span> <span data-ttu-id="dd4e7-121">シンプルなフォームが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-121">You will be presented with a simple form.</span></span> <span data-ttu-id="dd4e7-122">いくつかのテキストを入力して、**[送信]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-122">Enter some text and click **Submit**.</span></span> <span data-ttu-id="dd4e7-123">このアクションにより、入力したテキストを含むファイルが Azure Files 共有で作成されます。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-123">This action will create a file in the Azure Files share containing the entered text.</span></span>

![Azure Container Instances のファイル共有のデモ](../media/5-files-ui.png)

<span data-ttu-id="dd4e7-125">検証するには、Azure portal でファイル共有に移動し、ファイルをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-125">To validate, you can navigate to the file share in the Azure portal and download the file.</span></span>

1. <span data-ttu-id="dd4e7-126">ファイル名を取得します</span><span class="sxs-lookup"><span data-stu-id="dd4e7-126">Get the filename</span></span>

    ```azurecli
    az storage file list -s aci-share-demo -o table
    ```

1. <span data-ttu-id="dd4e7-127">ファイルをダウンロードします。必ず以下の `<filename>` を置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-127">Download it, make sure to replace the `<filename>` below.</span></span>

    ```azurecli
    az storage file download -s aci-share-demo -n <filename>
    ```
    
![コンテンツ デモ アプリケーションのサンプル テキスト ファイル](../media/5-sample-text.png)

<span data-ttu-id="dd4e7-129">Azure Files 共有に格納されているファイルとデータに何らかの価値がある場合は、この共有を新しいコンテナー インスタンスに再マウントして、ステートフルなデータを提供できます。</span><span class="sxs-lookup"><span data-stu-id="dd4e7-129">If the files and data stored in the Azure Files share were of any value, this share could be remounted on a new container instance to provide stateful data.</span></span>