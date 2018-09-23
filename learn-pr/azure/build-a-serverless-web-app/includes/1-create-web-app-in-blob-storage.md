<span data-ttu-id="2a4d3-101">このモジュールでは、HTML ベースのユーザー インターフェイスを表示するシンプルな Web アプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-101">In this module, you will deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="2a4d3-102">サーバーレス バックエンドにより、アプリケーションで画像をアップロードし、説明キャプションを自動的に生成できます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-102">A serverless back end enables the application to upload images and automatically generate descriptive captions.</span></span>

![Web アプリの実行](../media/0-app-screenshot-finished.png)

<span data-ttu-id="2a4d3-104">次の図では、アプリケーションによって使用されている Azure サービスを示します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-104">The following illustration shows the Azure services that are used by the application.</span></span>

![<span data-ttu-id="2a4d3-105">Azure Blob Storage、Azure Functions、Cosmos DB、Azure Logic Apps、Azure Active Directory などの異なる Azure サービスがアプリケーションでどのように使用されているかを示す図。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-105">An illustration showing how different Azure services such as, Azure Blob storage, Azure functions, Cosmos DB, Azure logic apps, and Azure active directory are used by the application.</span></span> ](../media/0-architecture.jpg)

1. <span data-ttu-id="2a4d3-106">Azure Blob Storage は静的 Web コンテンツ (HTML、CSS、JS) を提供し、画像が格納されます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-106">Azure Blob storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="2a4d3-107">Azure Functions によって、画像のアップロード、サイズ変更、およびメタデータ ストレージが管理されます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-107">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="2a4d3-108">Azure Cosmos DB には、画像のメタデータが格納されます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-108">Azure Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="2a4d3-109">Azure Logic Apps によって、Cognitive Services Computer Vision API から画像のキャプションが取得されます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-109">Azure Logic Apps retrieves image captions from the Cognitive Services Computer Vision API.</span></span>
5. <span data-ttu-id="2a4d3-110">Azure Active Directory によりユーザー認証が管理されます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-110">Azure Active Directory manages user authentication.</span></span>

<span data-ttu-id="2a4d3-111">Azure Blob Storage は、静的ファイルをホストする際に使用できる、低コストで非常にスケーラブルなサービスです。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-111">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="2a4d3-112">このモジュールでは、Blob Storage を使用して、構築する Web アプリに静的コンテンツ (たとえば、HTML、JavaScript、CSS) を提供します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-112">In this module, you will use Blob storage to serve static content (for example, HTML, JavaScript, or CSS) for a web app you build.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="2a4d3-113">Azure Storage アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="2a4d3-113">Create an Azure Storage account</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="2a4d3-114">Azure Storage アカウントは、テーブル、キュー、ファイル、BLOB (オブジェクト)、仮想マシン ディスクを格納できる Azure リソースです。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-114">An Azure Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="2a4d3-115">このモジュールの静的コンテンツ (HTML、CSS、および JavaScript ファイル) は Blob Storage でホストされます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-115">The static content (HTML, CSS, and JavaScript files) for this module is hosted in Blob storage.</span></span> <span data-ttu-id="2a4d3-116">Blob Storage にはストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-116">Blob storage requires a Storage account.</span></span> <span data-ttu-id="2a4d3-117">リソース グループに汎用 v2 (GPv2) ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-117">Create a general-purpose v2 (GPv2) Storage account in the resource group.</span></span> <span data-ttu-id="2a4d3-118">`<storage account name>` を一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-118">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create \
        -n <storage account name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --kind StorageV2 \
        --https-only true \
        --sku Standard_LRS
    ```
    
1. <span data-ttu-id="2a4d3-119">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-119">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="2a4d3-120">ポータルの上部にある検索バーを使用して、先ほど作成したストレージ アカウントを検索します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-120">Use the Search bar at the top of the portal to find the storage account that you just created.</span></span> <span data-ttu-id="2a4d3-121">アカウントを開きます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-121">Open the account.</span></span>

1. <span data-ttu-id="2a4d3-122">左側のナビゲーションで、**[静的な Web サイト (プレビュー)]** を選択して、静的な Web サイトをホストするコンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-122">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="2a4d3-123">**[有効]** を選択して、静的な Web サイトを有効にします。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-123">Select **Enabled** to enable a static website.</span></span>
    - <span data-ttu-id="2a4d3-124">インデックス ドキュメント名として「**index.html**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-124">Enter **index.html** as the index document name.</span></span> <span data-ttu-id="2a4d3-125">ボックスには既にグレーのフォントで *index.html* と表示されていますが、これはテキストの例でしかありません。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-125">The box already has *index.html* in a gray font, but this is only example text.</span></span> <span data-ttu-id="2a4d3-126">やはり、ボックスに「**index.html**」と入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-126">You still have to enter **index.html** in the box.</span></span>
    - <span data-ttu-id="2a4d3-127">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-127">Click **Save**.</span></span>
    
    ![静的な Web サイトの設定を入力する](../media/1-storage-static-website.png)

1. <span data-ttu-id="2a4d3-129">このモジュールの作業中に簡単にコピーできる場所に**プライマリ エンドポイント**を保存します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-129">Save the **Primary Endpoint** in a place where you can conveniently copy it from while working through the module.</span></span> <span data-ttu-id="2a4d3-130">このエンドポイントは Web アプリケーションの URL です。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-130">This endpoint is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="2a4d3-131">Web アプリケーションをアップロードする</span><span class="sxs-lookup"><span data-stu-id="2a4d3-131">Upload the web application</span></span>

1. <span data-ttu-id="2a4d3-132">このモジュールでビルドするアプリケーションのソース ファイルは、[GitHub リポジトリ](https://github.com/Azure-Samples/functions-first-serverless-web-application)にあります。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-132">The source files for the application that you build in this module are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="2a4d3-133">Cloud Shell のホーム ディレクトリに移動して、このリポジトリを複製します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-133">Go to your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="2a4d3-134">リポジトリは `/home/<username>/functions-first-serverless-web-application` に複製されます</span><span class="sxs-lookup"><span data-stu-id="2a4d3-134">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`</span></span>

1. <span data-ttu-id="2a4d3-135">クライアント側の Web アプリケーションは **www** フォルダーにあり、Vue.js JavaScript フレームワークを使用して構築されています。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-135">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="2a4d3-136">**www** フォルダーを開き、**npm** コマンドを実行してアプリケーションの依存関係をインストールして、アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-136">Open the **www** folder and run **npm** commands to install the application dependencies and build the application.</span></span> <span data-ttu-id="2a4d3-137">これらのコマンドの最後は、完了に数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-137">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="2a4d3-138">アプリケーションが **dist** フォルダーに生成されます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-138">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="2a4d3-139">現在のディレクトリを **dist** フォルダーに変更し、アプリケーションを **$web** BLOB コンテナーにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-139">Change the current directory to the **dist** folder and upload the application to the **$web** blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="2a4d3-140">アプリケーションを表示するには、Web ブラウザーで静的な Web サイトのプライマリ エンドポイント URL を開きます。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-140">To view the application, open the static website’s primary endpoint URL in a web browser.</span></span>

    ![最初のサーバーレス Web アプリのホーム ページ](../media/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="2a4d3-142">まとめ</span><span class="sxs-lookup"><span data-stu-id="2a4d3-142">Summary</span></span>

<span data-ttu-id="2a4d3-143">このユニットでは、ストレージ アカウントを作成しました。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-143">In this unit, you created a storage account.</span></span> <span data-ttu-id="2a4d3-144">ストレージ アカウント内の **$web** という名前の BLOB コンテナーに Web アプリケーションの静的コンテンツを格納し、コンテンツを公開しました。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-144">A blob container named **$web** in the storage account stores the static content for your web application and makes the content publicly available.</span></span> <span data-ttu-id="2a4d3-145">次は、サーバーレス関数を使用して、この Web アプリケーションから Blob Storage にイメージをアップロードする方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="2a4d3-145">Next, you will learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>