<span data-ttu-id="f9989-101">現時点では、このアプリケーションは画像をアップロードして表示できる機能ギャラリーです。</span><span class="sxs-lookup"><span data-stu-id="f9989-101">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="f9989-102">このモジュールでは、Microsoft Cognitive Services の Computer Vision API を使用して、アップロードした画像のキャプションを生成し、Azure Cosmos DB 内の画像メタデータと共にそのキャプションを保存する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="f9989-102">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Azure Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="f9989-103">Computer Vision アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="f9989-103">Create a Computer Vision account</span></span>

<span data-ttu-id="f9989-104">Microsoft Cognitive Services は、開発者がよりインテリジェントなアプリケーションを開発するためのサービスのコレクションです。</span><span class="sxs-lookup"><span data-stu-id="f9989-104">Microsoft Cognitive Services is a collection of services that are available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="f9989-105">Computer Vision API は、高度なアルゴリズムを使用して画像を処理し、各画像に関する情報を返す、サーバーレス サービスです。</span><span class="sxs-lookup"><span data-stu-id="f9989-105">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="f9989-106">Free レベルでは、1 か月あたり最大 5,000 回の API 呼び出しを実行できます。</span><span class="sxs-lookup"><span data-stu-id="f9989-106">It has a free tier that provides up to 5,000 API calls per month.</span></span>

<span data-ttu-id="f9989-107">リソース グループ内で一意の名前を持つ、**ComputerVision** という種類の新しい Cognitive Services アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="f9989-107">Create a new Cognitive Services account of type **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="f9989-108">Free レベルの場合は、**F0** を SKU として使用します。</span><span class="sxs-lookup"><span data-stu-id="f9989-108">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="f9989-109">Computer Vision の既存のアカウントが既にある場合は、Standard アカウント (S1) の作成が必要になる場合があり、これによってコストが発生することがあります。</span><span class="sxs-lookup"><span data-stu-id="f9989-109">If you already have an existing Computer Vision account, you may need to create a Standard account (S1), which may incur some costs.</span></span>

```azurecli
az cognitiveservices account create \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <computer vision account name> \
    --kind ComputerVision \
    --sku F0 \
    --location $(az group show -n <rgn>[Sandbox resource group name]</rgn> --query location -o tsv)
```

<span data-ttu-id="f9989-110">データ同意通知に同意するように求められたら、`y` を入力し、`Enter` を押します。</span><span class="sxs-lookup"><span data-stu-id="f9989-110">When prompted to accept the data consent notice, enter `y` and press `Enter`.</span></span> <span data-ttu-id="f9989-111">コマンドが完了するまでに 1 〜 2 分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f9989-111">The command may take a minute or two to complete.</span></span>

## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="f9989-112">Computer Vision の URL およびキー用の関数アプリ設定を作成する</span><span class="sxs-lookup"><span data-stu-id="f9989-112">Create function app settings for Computer Vision URL and key</span></span>

<span data-ttu-id="f9989-113">Computer Vision API を呼び出すには、URL とキーが必要です。</span><span class="sxs-lookup"><span data-stu-id="f9989-113">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="f9989-114">Computer Vision API のキーと URL を取得し、Bash 変数に保存します。</span><span class="sxs-lookup"><span data-stu-id="f9989-114">Get the Computer Vision API key and URL and save them in Bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g <rgn>[Sandbox resource group name]</rgn> -n <computer vision account name> --query key1 --output tsv)
    export COMP_VISION_URL=$(az cognitiveservices account show -g <rgn>[Sandbox resource group name]</rgn> -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="f9989-115">それぞれ **COMP_VISION_KEY** と **COMP_VISION_URL** という名前のアプリ設定を、関数アプリで作成します。</span><span class="sxs-lookup"><span data-stu-id="f9989-115">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set \
        -n <function app name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```

## <a name="call-the-computer-vision-api-from-the-resizeimage-function"></a><span data-ttu-id="f9989-116">ResizeImage 関数から Computer Vision API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="f9989-116">Call the Computer Vision API from the ResizeImage function</span></span>

<span data-ttu-id="f9989-117">次の手順では、**ResizeImage** 関数を変更して Computer Vision API を呼び出し、アップロードした各画像について説明して、その説明を Azure Cosmos DB に保存します。</span><span class="sxs-lookup"><span data-stu-id="f9989-117">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="f9989-118">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="f9989-118">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="f9989-119">関数アプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="f9989-119">Open your function app.</span></span>

<span data-ttu-id="f9989-120">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="f9989-120">::: zone pivot="csharp"</span></span>

3. <span data-ttu-id="f9989-121">左側のナビゲーションを使用し、**ResizeImage** 関数を検索して、そのコード ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="f9989-121">Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

1. <span data-ttu-id="f9989-122">コードを [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f9989-122">Replace the code with the contents of the [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx) file.</span></span> <span data-ttu-id="f9989-123">このコードでは、`HttpClient` を使用して Computer Vision API を呼び出し、その結果を Azure Cosmos DB に保存しています。</span><span class="sxs-lookup"><span data-stu-id="f9989-123">This code uses `HttpClient` to call the Computer Vision API and save its result in Azure Cosmos DB.</span></span>

1. <span data-ttu-id="f9989-124">コード ウィンドウの下の **[ログ]** をクリックしてログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="f9989-124">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="f9989-125">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9989-125">Click **Save**.</span></span> <span data-ttu-id="f9989-126">関数が確実にエラーのない状態で正常に保存されていることをログ パネルで確認します。</span><span class="sxs-lookup"><span data-stu-id="f9989-126">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="f9989-127">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="f9989-127">::: zone-end</span></span>

<span data-ttu-id="f9989-128">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="f9989-128">::: zone pivot="javascript"</span></span>

2. <span data-ttu-id="f9989-129">この関数では、Computer Vision API への HTTP 呼び出しを行うために、npm の `axios` パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="f9989-129">This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="f9989-130">npm パッケージをインストールするには、左側のナビゲーションで関数アプリの名前をクリックし、**[プラットフォーム機能]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9989-130">To install the npm package, click on the function app name on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="f9989-131">**[コンソール]** をクリックしてコンソール ウィンドウを表示します。</span><span class="sxs-lookup"><span data-stu-id="f9989-131">Click **Console** to reveal a console window.</span></span>

1. <span data-ttu-id="f9989-132">コンソールで `npm install axios` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f9989-132">Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="f9989-133">操作が完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="f9989-133">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="f9989-134">左側のナビゲーションで関数名 (**ResizeImage**) をクリックし、関数を表示します。</span><span class="sxs-lookup"><span data-stu-id="f9989-134">Click on the function name (**ResizeImage**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="f9989-135">**index.js** ファイルのすべてを、[**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f9989-135">Replace all of the **index.js** file with the contents of the [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js) file.</span></span>

1. <span data-ttu-id="f9989-136">コード ウィンドウの下の **[ログ]** をクリックして、ログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="f9989-136">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="f9989-137">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f9989-137">Click **Save**.</span></span> <span data-ttu-id="f9989-138">関数が確実にエラーのない状態で正常に保存されていることをログ パネルで確認します。</span><span class="sxs-lookup"><span data-stu-id="f9989-138">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>

<span data-ttu-id="f9989-139">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="f9989-139">::: zone-end</span></span>

## <a name="test-the-application"></a><span data-ttu-id="f9989-140">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="f9989-140">Test the application</span></span>

1. <span data-ttu-id="f9989-141">ブラウザーでアプリケーションを開きます。</span><span class="sxs-lookup"><span data-stu-id="f9989-141">Open the application in a browser.</span></span>

1. <span data-ttu-id="f9989-142">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="f9989-142">Select an image file and upload it.</span></span>

1. <span data-ttu-id="f9989-143">数秒後に、新しい画像のサムネイルがこのページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f9989-143">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="f9989-144">画像をポイントすると、Computer Vision によって生成された説明が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f9989-144">Point to the image to see the description that's generated by Computer Vision.</span></span>

## <a name="summary"></a><span data-ttu-id="f9989-145">まとめ</span><span class="sxs-lookup"><span data-stu-id="f9989-145">Summary</span></span>

<span data-ttu-id="f9989-146">このユニットでは、Microsoft Cognitive Services の Computer Vision API を使用して、アップロードした各画像用のキャプションを自動的に生成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="f9989-146">In this unit, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="f9989-147">次は、Azure App Service 認証を使用して、アプリケーションに認証を追加する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="f9989-147">Next, you learn how to add authentication to the application using Azure App Service authentication.</span></span>