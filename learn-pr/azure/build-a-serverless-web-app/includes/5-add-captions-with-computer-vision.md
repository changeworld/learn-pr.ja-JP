<span data-ttu-id="2eb7f-101">現時点では、このアプリケーションは画像をアップロードして表示できる機能ギャラリーです。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-101">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="2eb7f-102">このモジュールでは、Microsoft Cognitive Services の Computer Vision API を使用して、アップロードした画像のキャプションを生成し、Azure Cosmos DB 内の画像メタデータと共にそのキャプションを保存する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-102">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Azure Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="2eb7f-103">Computer Vision アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="2eb7f-103">Create a Computer Vision account</span></span>

<span data-ttu-id="2eb7f-104">Microsoft Cognitive Services は、開発者がよりインテリジェントなアプリケーションを開発するためのサービスのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-104">Microsoft Cognitive Services is a collection of services that are available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="2eb7f-105">Computer Vision API は、高度なアルゴリズムを使用して画像を処理し、各画像に関する情報を返す、サーバーレス サービスです。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-105">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="2eb7f-106">Free レベルでは、1 か月あたり最大 5,000 回の API 呼び出しを実行できます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-106">It has a free tier that provides up to 5,000 API calls per month.</span></span>

1. <span data-ttu-id="2eb7f-107">Cloud Shell に引き続きサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-107">Ensure you're still signed into Cloud Shell.</span></span> <span data-ttu-id="2eb7f-108">そうでない場合は、**[Enter focus mode]\(フォーカス モードにする\)** を選択して、Cloud Shell ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-108">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="2eb7f-109">リソース グループ内で一意の名前を持つ、**ComputerVision** という種類の新しい Cognitive Services アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-109">Create a new Cognitive Services account of type **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="2eb7f-110">Free レベルの場合は、**F0** を SKU として使用します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-110">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="2eb7f-111">Computer Vision の既存のアカウントが既にある場合は、Standard アカウント (S1) の作成が必要になる場合があり、これによってコストが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-111">If you already have an existing Computer Vision account, you may need to create a Standard account (S1), which may incur some costs.</span></span>

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="2eb7f-112">Computer Vision の URL およびキー用の関数アプリ設定を作成する</span><span class="sxs-lookup"><span data-stu-id="2eb7f-112">Create function app settings for Computer Vision URL and key</span></span>

<span data-ttu-id="2eb7f-113">Computer Vision API を呼び出すには、URL とキーが必要です。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-113">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="2eb7f-114">Computer Vision API のキーと URL を取得し、Bash 変数に保存します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-114">Get the Computer Vision API key and URL and save them in Bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="2eb7f-115">それぞれ **COMP_VISION_KEY** と **COMP_VISION_URL** という名前のアプリ設定を、関数アプリで作成します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-115">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-the-computer-vision-api-from-the-resizeimage-function"></a><span data-ttu-id="2eb7f-116">ResizeImage 関数から Computer Vision API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="2eb7f-116">Call the Computer Vision API from the ResizeImage function</span></span>

<span data-ttu-id="2eb7f-117">次の手順では、**ResizeImage** 関数を変更して Computer Vision API を呼び出し、アップロードした各画像について説明して、その説明を Azure Cosmos DB に保存します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-117">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="2eb7f-118">Azure portal で関数アプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-118">Open your function app in the Azure portal.</span></span>

1. <span data-ttu-id="2eb7f-119">**C#**</span><span class="sxs-lookup"><span data-stu-id="2eb7f-119">**C#**</span></span>

    1. <span data-ttu-id="2eb7f-120">(C#) 左側のナビゲーションを使用して、**ResizeImage** 関数を見つけ、そのコード ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-120">(C#) Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

    1. <span data-ttu-id="2eb7f-121">(C#) コードを [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-121">(C#) Replace the code with the contents of the [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) file.</span></span> <span data-ttu-id="2eb7f-122">このコードでは、`HttpClient` を使用して Computer Vision API を呼び出し、その結果を Azure Cosmos DB に保存しています。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-122">This code uses `HttpClient` to call the Computer Vision API and save its result in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="2eb7f-123">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="2eb7f-123">**JavaScript**</span></span>

    1. <span data-ttu-id="2eb7f-124">(JavaScript) この関数では、Computer Vision API への HTTP 呼び出しを行うために、npm の `axios` パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-124">(JavaScript) This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="2eb7f-125">npm パッケージをインストールするには、左側のナビゲーションで関数アプリの名前をクリックし、**[プラットフォーム機能]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-125">To install the npm package, click on the function app name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="2eb7f-126">(JavaScript) **[コンソール]** をクリックしてコンソール ウィンドウを表示します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-126">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="2eb7f-127">(JavaScript) コンソールで `npm install axios` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-127">(JavaScript) Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="2eb7f-128">操作が完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-128">It may take a few minutes to complete the operation.</span></span>

    1. <span data-ttu-id="2eb7f-129">(JavaScript) 左側のナビゲーションで関数名 (**ResizeImage**) をクリックして、関数を表示します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-129">(JavaScript) Click on the function name (**ResizeImage**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="2eb7f-130">**index.js** ファイルのすべてを、[**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-130">Replace all of the **index.js** file with the contents of the [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) file.</span></span>

1. <span data-ttu-id="2eb7f-131">コード ウィンドウの下の **[ログ]** をクリックして、ログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-131">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="2eb7f-132">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-132">Click **Save**.</span></span> <span data-ttu-id="2eb7f-133">関数が正常に保存され、エラーがないことをログ パネルで確認します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-133">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="2eb7f-134">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="2eb7f-134">Test the application</span></span>

1. <span data-ttu-id="2eb7f-135">ブラウザーでアプリケーションを開きます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-135">Open the application in a browser.</span></span> <span data-ttu-id="2eb7f-136">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-136">Select an image file, and upload it.</span></span>

1. <span data-ttu-id="2eb7f-137">数秒後に、新しい画像のサムネイルがこのページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-137">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="2eb7f-138">画像をポイントすると、Computer Vision によって生成された説明が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-138">Point to the image to see the description that's generated by Computer Vision.</span></span>


## <a name="summary"></a><span data-ttu-id="2eb7f-139">まとめ</span><span class="sxs-lookup"><span data-stu-id="2eb7f-139">Summary</span></span>

<span data-ttu-id="2eb7f-140">このユニットでは、Microsoft Cognitive Services の Computer Vision API を使用して、アップロードした各画像用のキャプションを自動的に生成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-140">In this unit, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="2eb7f-141">次は、Azure App Service 認証を使用して、アプリケーションに認証を追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="2eb7f-141">Next, you learn how to add authentication to the application using Azure App Service authentication.</span></span>