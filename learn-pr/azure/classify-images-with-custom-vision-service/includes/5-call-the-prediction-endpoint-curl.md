<span data-ttu-id="811be-101">前の演習では、Custom Vision Service ポータルの**クイック テスト**機能を使用して、トレーニング済みのモデルをテストしました。</span><span class="sxs-lookup"><span data-stu-id="811be-101">In the last exercise, we tested our trained model using the **Quick Test** feature of the Custom Vision Service portal.</span></span> <span data-ttu-id="811be-102">これは、テスト画像をいくつか使ってモデルの精度を簡単に確認する優れた方法です。</span><span class="sxs-lookup"><span data-stu-id="811be-102">This is a great way to quickly check the accuracy of the model with some test images.</span></span> <span data-ttu-id="811be-103">次に、HTTP 経由でモデルの予測エンドポイントを呼び出してみましょう。</span><span class="sxs-lookup"><span data-stu-id="811be-103">Let's go a little further and make calls to the prediction endpoint of our model over HTTP.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

1. <span data-ttu-id="811be-104">Custom Vision Service ポータルで *Artworks*\* プロジェクトに戻り、**[Performance]\(パフォーマンス\)** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="811be-104">Returning to your *Artworks*\* project in the Custom Vision Service portal, select the  **Performance** tab.</span></span>

    ![[Performance]\(パフォーマンス\) タブの選択](../media/5-performance-tab.png)

1. <span data-ttu-id="811be-106">**[Make default]\(既定にする\)** を選択して、モデルの最新のイテレーションを既定のイテレーションにします。</span><span class="sxs-lookup"><span data-stu-id="811be-106">Select **Make default** to make sure the latest iteration of the model is the default iteration.</span></span>

1. <span data-ttu-id="811be-107">**[Prediction URL]\(予測 URL\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="811be-107">Select **Prediction URL**.</span></span> <span data-ttu-id="811be-108">呼び出しを行うのに必要な情報のダイアログ ボックスが表示されます。</span><span class="sxs-lookup"><span data-stu-id="811be-108">This displays a dialog of the information we need to make our calls.</span></span> 

    ![予測 URL の情報を表示](../media/5-portal-prediction-url.png)

    <span data-ttu-id="811be-110">ダイアログ ボックスで示されているように、予測エンドポイントを呼び出して画像の URL を渡すことできます。</span><span class="sxs-lookup"><span data-stu-id="811be-110">As the dialog shows, we can call the prediction endpoint and pass it an image URL.</span></span> <span data-ttu-id="811be-111">生の画像を要求の本文でエンドポイントに渡すこともできます。</span><span class="sxs-lookup"><span data-stu-id="811be-111">We can also pass a raw image to the endpoint in the body of the request.</span></span>

    <span data-ttu-id="811be-112">このダイアログ ボックスの 3 つの情報を書き留めておきます。</span><span class="sxs-lookup"><span data-stu-id="811be-112">Take note of three pieces of information from this dialog.</span></span>
     - <span data-ttu-id="811be-113">**[Prediction-Key]\(予測キー\)**: すべての要求のヘッダーとしてこのキーを設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="811be-113">**Prediction-Key**: This key has to be set as a header in all requests.</span></span> <span data-ttu-id="811be-114">このキーによってエンドポイントへのアクセス権が提供されます。</span><span class="sxs-lookup"><span data-stu-id="811be-114">That's what gives us access to the endpoint.</span></span>
    - <span data-ttu-id="811be-115">**[Request URL]\(要求 URL\)**: ダイアログには 2 つの異なる URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="811be-115">**Request URL**: The dialog shows two different URLs.</span></span> <span data-ttu-id="811be-116">画像の URL を送信する場合は、末尾が `/url` の最初の URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="811be-116">If we're posting an image URL, then use the first URL, which ends in `/url`.</span></span> <span data-ttu-id="811be-117">要求の本文で生の画像を送信する場合は、末尾が `/image` の 2 番目の URL を使用しします。</span><span class="sxs-lookup"><span data-stu-id="811be-117">If we want to post a raw image in the body of our request, we use the second URL, which ends in `/image`.</span></span>
    - <span data-ttu-id="811be-118">**[Content-Type]\(コンテンツ タイプ\)**: 生の画像を送信する場合は、要求の本文に画像のバイナリ表現を設定し、コンテンツ タイプを `application/octet-stream` に設定します。</span><span class="sxs-lookup"><span data-stu-id="811be-118">**Content-Type**: If we're posting a raw image, we set the body of the request to the binary representation of the image and the content type to `application/octet-stream`.</span></span> <span data-ttu-id="811be-119">画像の URL を送信する場合は、本文に JSON として URL を設定し、コンテンツ タイプを `application/json` に設定します。</span><span class="sxs-lookup"><span data-stu-id="811be-119">If we're posting an image URL, we put that as JSON in the body and set the content type to `application/json`.</span></span>
    

3. <span data-ttu-id="811be-120">**[How to use the Prediction API]\(予測 API の使用方法\)** ダイアログから最初の URL と `Prediction-Key` の値をコピーして保存します。</span><span class="sxs-lookup"><span data-stu-id="811be-120">Copy and save the first URL and the `Prediction-Key` value from the **How to use the Prediction API** dialog.</span></span> 

> [!TIP]
> <span data-ttu-id="811be-121">**cURL** は、ファイルを送受信するために使用できるコマンドライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="811be-121">**cURL** is a command line tool that can be used to send or receive files.</span></span> <span data-ttu-id="811be-122">このツールは、Linux、macOS、および Windows 10 に含まれており、その他のほとんどのオペレーティング システムでもダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="811be-122">It's included with Linux, macOS, and Windows 10, and can be downloaded for most other operating systems.</span></span> <span data-ttu-id="811be-123">cURL では、HTTP、HTTPS、FTP、FTPS、SFTP、LDAP、TELNET、SMTP、POP3 などのさまざまなプロトコルをサポートします。詳細については、次のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="811be-123">cURL supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3, etc. For more information, refer to the links below:</span></span>
>
>- <https://en.wikipedia.org/wiki/CURL>
>- <https://curl.haxx.se/docs/> 
> 
> <span data-ttu-id="811be-124">cURL はサンドボックスの Azure Cloud Shell に既にインストールされています。</span><span class="sxs-lookup"><span data-stu-id="811be-124">cURL is already installed in the Azure Cloud Shell in the sandbox.</span></span> <span data-ttu-id="811be-125">そこでこの演習では cURL を使用し、エンドポイントへの HTTP 呼び出しを行います。</span><span class="sxs-lookup"><span data-stu-id="811be-125">So, we'll cURL in this exercise to make HTTP calls to our endpoint.</span></span>

2. <span data-ttu-id="811be-126">Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="811be-126">Execute the following command in the Cloud Shell.</span></span> <span data-ttu-id="811be-127">**[endpoint-URL]** は、最後の手順で保存した URL に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="811be-127">Replace **[endpoint-URL]** with the URL you saved from the last step.</span></span> <span data-ttu-id="811be-128">**[Prediction-Key]** は、最後の手順で保存した `Prediction-Key` の値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="811be-128">Replace **[Prediction-Key]** with the value of `Prediction-Key` you saved from the last step.</span></span> 

    ```azurecli
    curl [endpoint-URL] \
    -H "Prediction-Key: [Prediction-Key]" \
    -H "Content-Type: application/json" \
    -d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/VanGoghTest_02.jpg'}" \
    | jq '.'
    ```

    <span data-ttu-id="811be-129">コマンドが完了すると、次のスクリーンショットのような JSON 応答が表示されます。</span><span class="sxs-lookup"><span data-stu-id="811be-129">When the command completes, you'll see a JSON response similar to the following screenshot.</span></span> <span data-ttu-id="811be-130">API は、モデル内のすべてのタグに対する確率を返します。</span><span class="sxs-lookup"><span data-stu-id="811be-130">The API returns a probability for every tag in the model.</span></span> <span data-ttu-id="811be-131">ご覧のように、`"painting"` **tagName** の値に 1 に近い確率が設定されているこの画像は、確かに絵画です。</span><span class="sxs-lookup"><span data-stu-id="811be-131">As you can see, with a probability close to 1 for the `"painting"` **tagName** value, this image is definitely a painting.</span></span> <span data-ttu-id="811be-132">ただし、モデルのトレーニングに使用したどの画家の絵画でもありません。</span><span class="sxs-lookup"><span data-stu-id="811be-132">However, it's not a painting by any of the artists with which we trained our model.</span></span> 

    ![テスト用のピカソの画像のサムネイル](../media/5-prediction-json.png) 

3. <span data-ttu-id="811be-134">上の要求本文内の URL を次の表にある URL に置き換え、さらに予測してみてください。</span><span class="sxs-lookup"><span data-stu-id="811be-134">Try more predictions by replacing the URL in the request body above, with the URLs in the following table.</span></span> 

    |<span data-ttu-id="811be-135">イメージ</span><span class="sxs-lookup"><span data-stu-id="811be-135">Image</span></span>  | <span data-ttu-id="811be-136">URL</span><span class="sxs-lookup"><span data-stu-id="811be-136">URL</span></span>  |
    |---------|---------|
    |![テスト用のピカソの画像のサムネイル](../media/picasso-test-02-thumb.jpg)     | `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PicassoTest_02.jpg`        |
    |![テスト用のレンブラントの画像のサムネイル](../media/rembrandt-test-01-thumb.jpg)     |  `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/RembrandtTest_01.jpg`       |
    |![テスト用のポロックの画像のサムネイル](../media/pollock-test-01-thumb.jpg)  |   `https://raw.githubusercontent.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/master/test-images/PollockTest_01.jpg`     |
   

<span data-ttu-id="811be-140">この予測エンドポイントは、想定どおりに動作しています。</span><span class="sxs-lookup"><span data-stu-id="811be-140">Our prediction endpoint is working as expected.</span></span> <span data-ttu-id="811be-141">API の呼び出しは、予測キーと画像の URL を指定してエンドポイントに HTTP 要求を行うだけの簡単なものです。</span><span class="sxs-lookup"><span data-stu-id="811be-141">Calling the API is as simple as making a HTTP request to the endpoint with a Prediction-Key and an image URL.</span></span>