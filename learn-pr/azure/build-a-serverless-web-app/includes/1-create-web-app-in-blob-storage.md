<span data-ttu-id="41a86-101">このモジュールでは、HTML ベースのユーザー インターフェイスを表示するシンプルな Web アプリケーションをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="41a86-101">In this module, you will deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="41a86-102">サーバーレス バックエンドにより、アプリケーションで画像をアップロードし、その画像について説明するキャプションを自動的に取得できます。</span><span class="sxs-lookup"><span data-stu-id="41a86-102">A serverless back end enables the application to upload images and automatically get captions that describe them.</span></span>

![Web アプリの実行](../media/0-app-screenshot-finished.png)

<span data-ttu-id="41a86-104">次の図では、アプリケーションによって使用されている Azure サービスを示します。</span><span class="sxs-lookup"><span data-stu-id="41a86-104">The following diagram shows the Azure services that are used by the application.</span></span>

1. <span data-ttu-id="41a86-105">Azure Blob Storage は静的 Web コンテンツ (HTML、CSS、JS) を提供し、画像が格納されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-105">Azure Blob storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="41a86-106">Azure Functions によって、画像のアップロード、サイズ変更、およびメタデータ ストレージが管理されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-106">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="41a86-107">Azure Cosmos DB には、画像のメタデータが格納されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-107">Azure Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="41a86-108">Azure Logic Apps によって、Cognitive Services Computer Vision API から画像のキャプションが取得されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-108">Azure Logic Apps gets image captions from the Cognitive Services Computer Vision API.</span></span>
5. <span data-ttu-id="41a86-109">Azure Active Directory によりユーザー認証が管理されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-109">Azure Active Directory manages user authentication.</span></span>

![ソリューションのアーキテクチャ図](../media/0-architecture.jpg)

<span data-ttu-id="41a86-111">このユニットでは、以下の方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="41a86-111">In this unit, you will learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="41a86-112">静的な Web サイトとアップロードされた画像がホストされるように Azure Blob Storage を構成します。</span><span class="sxs-lookup"><span data-stu-id="41a86-112">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="41a86-113">Azure Functions を使用して、画像を Azure Blob Storage にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="41a86-113">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="41a86-114">Azure Functions を使用して、画像のサイズを変更します。</span><span class="sxs-lookup"><span data-stu-id="41a86-114">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="41a86-115">画像のメタデータを Azure Cosmos DB に格納します。</span><span class="sxs-lookup"><span data-stu-id="41a86-115">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="41a86-116">Cognitive Services Vision API を使用して、画像のキャプションを自動生成します。</span><span class="sxs-lookup"><span data-stu-id="41a86-116">Use the Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="41a86-117">Azure Active Directory を使用してユーザーを認証することにより、Web アプリをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="41a86-117">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="41a86-118">Azure Blob Storage は、静的ファイルをホストする際に使用できる、低コストで非常にスケーラブルなサービスです。</span><span class="sxs-lookup"><span data-stu-id="41a86-118">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="41a86-119">このチュートリアルでは、Blob Storage を使用して、ビルドする Web アプリの静的コンテンツ (HTML、JavaScript、CSS など) を提供します。</span><span class="sxs-lookup"><span data-stu-id="41a86-119">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="41a86-120">Azure Storage アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="41a86-120">Create an Azure Storage account</span></span>

<span data-ttu-id="41a86-121">Azure Storage アカウントは、テーブル、キュー、ファイル、BLOB (オブジェクト)、仮想マシン ディスクを格納できる Azure リソースです。</span><span class="sxs-lookup"><span data-stu-id="41a86-121">An Azure Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="41a86-122">**[Enter focus mode]\(フォーカス モードにする\)** ボタンを選択して、Azure Cloud Shell (Bash) を起動します。</span><span class="sxs-lookup"><span data-stu-id="41a86-122">Select the **Enter focus mode** button to launch Azure Cloud Shell (Bash).</span></span> <span data-ttu-id="41a86-123">このボタンは、ブラウザー ウィンドウの幅に応じて、ページの右上または下部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-123">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="41a86-124">フォーカス モードでは、ブラウザー ウィンドウの右側に Cloud Shell ウィンドウがドッキングされるので、このチュートリアルで示すコマンドを簡単に実行できます。</span><span class="sxs-lookup"><span data-stu-id="41a86-124">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="41a86-125">Azure では、リソース グループは、管理を容易にするために関連する Azure リソースを保持するコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="41a86-125">In Azure, a resource group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="41a86-126">**first-serverless-app** という名前の新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="41a86-126">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="41a86-127">このチュートリアルの静的コンテンツ (HTML、CSS、および JavaScript ファイル) は Blob Storage でホストされます。</span><span class="sxs-lookup"><span data-stu-id="41a86-127">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob storage.</span></span> <span data-ttu-id="41a86-128">Blob Storage にはストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="41a86-128">Blob storage requires a Storage account.</span></span> <span data-ttu-id="41a86-129">リソース グループにストレージ アカウント (汎用 V2) を作成します。</span><span class="sxs-lookup"><span data-stu-id="41a86-129">Create a Storage account (general-purpose V2) in the resource group.</span></span> <span data-ttu-id="41a86-130">`<storage account name>` を一意の名前に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="41a86-130">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```
    
1. <span data-ttu-id="41a86-131">[Azure portal](https://portal.azure.com) の上部にある検索バーを使用して、先ほど作成したストレージ アカウントを検索します。</span><span class="sxs-lookup"><span data-stu-id="41a86-131">Use the Search bar at the top of the [Azure portal](https://portal.azure.com) to find the storage account that you just created.</span></span> <span data-ttu-id="41a86-132">アカウントを開きます。</span><span class="sxs-lookup"><span data-stu-id="41a86-132">Open the account.</span></span>

1. <span data-ttu-id="41a86-133">左側のナビゲーションで、**[静的な Web サイト (プレビュー)]** を選択して、静的な Web サイトをホストするコンテナーを構成します。</span><span class="sxs-lookup"><span data-stu-id="41a86-133">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="41a86-134">**[有効]** を選択して、静的な Web サイトを有効にします。</span><span class="sxs-lookup"><span data-stu-id="41a86-134">Select **Enabled** to enable a static website.</span></span>
    - <span data-ttu-id="41a86-135">インデックス ドキュメント名として「**index.html**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="41a86-135">Enter **index.html** as the index document name.</span></span> <span data-ttu-id="41a86-136">ボックスには既にグレーのフォントで *index.html* と表示されていますが、これはテキストの例でしかありません。</span><span class="sxs-lookup"><span data-stu-id="41a86-136">The box already has *index.html* in a gray font, but this is only example text.</span></span> <span data-ttu-id="41a86-137">やはり、ボックスに「**index.html**」と入力する必要があります。</span><span class="sxs-lookup"><span data-stu-id="41a86-137">You still have to enter **index.html** in the box.</span></span>
    - <span data-ttu-id="41a86-138">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="41a86-138">Click **Save**.</span></span>
    
    ![静的な Web サイトの設定を入力する](../media/1-storage-static-website.png)

1. <span data-ttu-id="41a86-140">このチュートリアルの作業中に簡単にコピーできる場所に**プライマリ エンドポイント**を保存します。</span><span class="sxs-lookup"><span data-stu-id="41a86-140">Save the **Primary Endpoint** in a place where you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="41a86-141">このエンドポイントは Web アプリケーションの URL です。</span><span class="sxs-lookup"><span data-stu-id="41a86-141">This endpoint is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="41a86-142">Web アプリケーションをアップロードする</span><span class="sxs-lookup"><span data-stu-id="41a86-142">Upload the web application</span></span>

1. <span data-ttu-id="41a86-143">このチュートリアルでビルドするアプリケーションのソース ファイルは、[GitHub リポジトリ](https://github.com/Azure-Samples/functions-first-serverless-web-application)にあります。</span><span class="sxs-lookup"><span data-stu-id="41a86-143">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="41a86-144">Cloud Shell のホーム ディレクトリから、このリポジトリを複製します。</span><span class="sxs-lookup"><span data-stu-id="41a86-144">Make sure that you're in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="41a86-145">リポジトリは `/home/<username>/functions-first-serverless-web-application` に複製されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-145">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="41a86-146">クライアント側の Web アプリケーションは **www** フォルダーにあり、Vue.js JavaScript フレームワークを使用して構築されています。</span><span class="sxs-lookup"><span data-stu-id="41a86-146">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="41a86-147">このフォルダーに移動した後、**npm** コマンドを実行してアプリケーションの依存関係をインストールし、アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="41a86-147">Change into the folder and run **npm** commands to install the application dependencies and build the application.</span></span> <span data-ttu-id="41a86-148">これらのコマンドの最後は、完了に数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="41a86-148">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="41a86-149">アプリケーションが **dist** フォルダーに生成されます。</span><span class="sxs-lookup"><span data-stu-id="41a86-149">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="41a86-150">現在のディレクトリを **dist** フォルダーに変更し、アプリケーションを **$web** BLOB コンテナーにアップロードします。</span><span class="sxs-lookup"><span data-stu-id="41a86-150">Change the current directory to the **dist** folder and upload the application to the **$web** blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="41a86-151">アプリケーションを表示するには、Web ブラウザーで Storage の静的な Web サイトのプライマリ エンドポイント URL を開きます。</span><span class="sxs-lookup"><span data-stu-id="41a86-151">To view the application, open the Storage static websites primary endpoint URL in a web browser.</span></span>

    ![最初のサーバーレス Web アプリのホーム ページ](../media/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="41a86-153">まとめ</span><span class="sxs-lookup"><span data-stu-id="41a86-153">Summary</span></span>

<span data-ttu-id="41a86-154">このユニットでは、ストレージ アカウントを含む **first-serverless-app** という名前のリソース グループを作成しました。</span><span class="sxs-lookup"><span data-stu-id="41a86-154">In this unit, you created a resource group named **first-serverless-app** that contains a Storage account.</span></span> <span data-ttu-id="41a86-155">ストレージ アカウント内の **$web** という名前の BLOB コンテナーに Web アプリケーションの静的コンテンツを格納し、コンテンツを公開しました。</span><span class="sxs-lookup"><span data-stu-id="41a86-155">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content publicly available.</span></span> <span data-ttu-id="41a86-156">次に、サーバレス関数を使用して、この Web アプリケーションから Blob Storage に画像をアップロードする方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="41a86-156">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>