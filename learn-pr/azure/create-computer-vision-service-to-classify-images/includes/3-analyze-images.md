<span data-ttu-id="07678-101">あなたは Contoso Beverage Distribution 社の開発主任として、現場の販売業者が在庫補充のために店舗の棚をスキャンし、その画像をアップロードするための基幹業務アプリを構築、保守する仕事を担当しています。</span><span class="sxs-lookup"><span data-stu-id="07678-101">As a lead developer at Contoso Beverage Distribution, you're responsible for building and maintaining a line-of-business app that lets your frontline distributors scan and upload images of store shelves where they are restocking.</span></span> 

<span data-ttu-id="07678-102">ユーザーの投稿画像が会社の設定したコンテンツ規則に従っていることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="07678-102">You want to validate that any images posted by users respect the content rules set by your company.</span></span> <span data-ttu-id="07678-103">会社は、会社のサイトに不適切なコンテンツが投稿されることを望んでいません。</span><span class="sxs-lookup"><span data-stu-id="07678-103">The company doesn't want inappropriate content posted to company sites.</span></span> <span data-ttu-id="07678-104">人工知能の進歩を踏まえ、あなたは、アプリの中で Computer Vision API を使用することに決めました。</span><span class="sxs-lookup"><span data-stu-id="07678-104">Given the advancements in Artificial Intelligence, you decide to use the Computer Vision API in your app.</span></span> 

<span data-ttu-id="07678-105">最初に、ご自分の Azure サブスクリプションで Computer Vision アカウントを作成し、**分析** 機能のテストを開始します。</span><span class="sxs-lookup"><span data-stu-id="07678-105">To get started, you create a Computer Vision account in your Azure subscription and start testing the **analyze** feature.</span></span>

## <a name="calling-the-computer-vision-api-to-analyze-images"></a><span data-ttu-id="07678-106">イメージを分析する Computer Vision API を呼び出す</span><span class="sxs-lookup"><span data-stu-id="07678-106">Calling the Computer Vision API to analyze images</span></span>

<span data-ttu-id="07678-107">`analyze`操作では、イメージの内容に基づいて、さまざまな視覚的特徴のセットが抽出されます。</span><span class="sxs-lookup"><span data-stu-id="07678-107">The `analyze` operation extracts a rich set of visual features based on the image content.</span></span> <span data-ttu-id="07678-108">要求 URL の形式は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="07678-108">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>`

<span data-ttu-id="07678-109">サービスに送信されるパラメーターは、`visualFeatures`、`details`、`languages` です。</span><span class="sxs-lookup"><span data-stu-id="07678-109">The parameters that are sent to the service are `visualFeatures`, `details`, and `languages`.</span></span> <span data-ttu-id="07678-110">`details` パラメーターを `Landmarks` または `Celebrities` に設定すると、ランドマークまたは著名人の識別に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="07678-110">Set the `details` parameter to `Landmarks` or `Celebrities` to help you identify landmarks or celebrities.</span></span> <span data-ttu-id="07678-111">`visualFeatures` は、サービスから戻される情報の種類が示されます。</span><span class="sxs-lookup"><span data-stu-id="07678-111">`visualFeatures` identify what kind of information you want the service to return you.</span></span> <span data-ttu-id="07678-112">`Categories` オプションは、木やビルのように、画像の内容を分類します。</span><span class="sxs-lookup"><span data-stu-id="07678-112">The `Categories` option will categorize the content of the images like trees, buildings, and more.</span></span> <span data-ttu-id="07678-113">`Faces` では人の顔が識別され、性別と年齢が提供されます。</span><span class="sxs-lookup"><span data-stu-id="07678-113">`Faces` will identify people's faces and give you their gender and age.</span></span>

<span data-ttu-id="07678-114">Computer Vision API に対してイメージの処理を求める場合は、すべての操作において次の制限があります。</span><span class="sxs-lookup"><span data-stu-id="07678-114">All operations on the Computer Vision API have the following restrictions when it comes to the images you ask it to process:</span></span>

- <span data-ttu-id="07678-115">サポートされているイメージ形式: JPEG、PNG、GIF、BMP。</span><span class="sxs-lookup"><span data-stu-id="07678-115">Supported image formats: JPEG, PNG, GIF, BMP.</span></span> 
- <span data-ttu-id="07678-116">イメージ ファイル サイズは 4 MB 未満である必要があります。</span><span class="sxs-lookup"><span data-stu-id="07678-116">Image file size must be less than  4 MB.</span></span>
- <span data-ttu-id="07678-117">イメージの寸法は 50 x 50 以上である必要があります。</span><span class="sxs-lookup"><span data-stu-id="07678-117">Image dimensions must be at least 50 x 50.</span></span>

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a><span data-ttu-id="07678-118">画像内のランドマークを識別する</span><span class="sxs-lookup"><span data-stu-id="07678-118">Identify landmarks in an image</span></span>

<span data-ttu-id="07678-119">イメージ内のランドマークを検索するための呼び出しから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="07678-119">Let's start with a call to find landmarks in an image.</span></span> <span data-ttu-id="07678-120">この例では、次のイメージを使用しますが、他のイメージへの URL を指定して同じコマンドを自由に試すことができます。</span><span class="sxs-lookup"><span data-stu-id="07678-120">We'll use the following image for this example, but you're free to try the same command with URLs to other images.</span></span> 

![雪を頂いた山々の上に青空が広がっている山脈の画像。](../media/3-mountains.jpg)

<span data-ttu-id="07678-122">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="07678-122">Execute the following command in Azure Cloud Shell.</span></span> <span data-ttu-id="07678-123">コマンドの `<region>` を、実際の ＠Cognitive Services アカウントのリージョンに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="07678-123">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

- <span data-ttu-id="07678-124">この呼び出しでは、イメージ URL で指定されたイメージ内のランドマークが検索されます。</span><span class="sxs-lookup"><span data-stu-id="07678-124">This call looks for landmarks in the image specified by the image URL.</span></span> <span data-ttu-id="07678-125">分析しているイメージは、この演習では GitHub リポジトリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="07678-125">The image we are analyzing is stored in a GitHub repo for this exercise.</span></span> 
- <span data-ttu-id="07678-126">この呼び出しではまた、カテゴリ情報およびイメージに関する説明を返すようにサービスに対して要求が出されます。</span><span class="sxs-lookup"><span data-stu-id="07678-126">The call also asks the service to return category information and a description of the image.</span></span> <span data-ttu-id="07678-127">説明は完全な英語の文で返されます。</span><span class="sxs-lookup"><span data-stu-id="07678-127">The description is returned as a complete English sentence.</span></span> 
- <span data-ttu-id="07678-128">ご存知のように、API のすべての呼び出しにアクセス キーが必要です。</span><span class="sxs-lookup"><span data-stu-id="07678-128">As you know, every call to the API needs an access key.</span></span> <span data-ttu-id="07678-129">これは、要求の `Ocp-Apim-Subscription-Key` ヘッダーに設定されています。</span><span class="sxs-lookup"><span data-stu-id="07678-129">This is set in the `Ocp-Apim-Subscription-Key` header of the request.</span></span> 

> [!TIP]
> <span data-ttu-id="07678-130">要求の結果は、`url` 内の画像を説明する未加工の JSON です。</span><span class="sxs-lookup"><span data-stu-id="07678-130">The result of the request is the raw JSON describing the picture in the `url`.</span></span> <span data-ttu-id="07678-131">コマンドの末尾に ` | jq '.'` を追加して、JSON の出力を修飾しました。</span><span class="sxs-lookup"><span data-stu-id="07678-131">We added ` | jq '.'` at the end of the command to prettify the JSON output.</span></span>

<span data-ttu-id="07678-132">この呼び出しからの JSON 応答では、次の内容が返されます。</span><span class="sxs-lookup"><span data-stu-id="07678-132">The JSON response from this call returns the following:</span></span>

- <span data-ttu-id="07678-133">検出されたすべてのイメージ カテゴリを一覧表示する `categories` 配列と、イメージが指定されたカテゴリに属していることがサービスによってどの程度確信されているかを示す 0 から 1 のスコア。</span><span class="sxs-lookup"><span data-stu-id="07678-133">A `categories` array listing all image categories that were detected, along with a score between 0 and 1 of how confident the service is that the image belongs in the specified category.</span></span>
- <span data-ttu-id="07678-134">イメージに関連するタグまたは単語の配列を含む `description` エントリ。</span><span class="sxs-lookup"><span data-stu-id="07678-134">A `description` entry containing an array of tags or words that are related to the image.</span></span>
- <span data-ttu-id="07678-135">`captions` エントリと、イメージ内に含まれているものを英語で説明したテキスト フィールド。</span><span class="sxs-lookup"><span data-stu-id="07678-135">A `captions` entry with a text field that describes in English what is in the image.</span></span> <span data-ttu-id="07678-136">テキストには信頼度スコアも含まれていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="07678-136">Observe that the text also has a certainty score.</span></span> <span data-ttu-id="07678-137">このスコアは、この分析を使用して次に何を行うかを決めるのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="07678-137">This score can help you decide what to do next with this analysis.</span></span>


## <a name="check-for-inappropriate-content-in-an-image"></a><span data-ttu-id="07678-138">イメージ内に不適切な内容が含まれていないか確認する</span><span class="sxs-lookup"><span data-stu-id="07678-138">Check for inappropriate content in an image</span></span>

<span data-ttu-id="07678-139">この例では、成人向けコンテンツのイメージを分析します。</span><span class="sxs-lookup"><span data-stu-id="07678-139">In this example, we'll analyze an image for adult content.</span></span> <span data-ttu-id="07678-140">信頼度スコアでは、成人向けコンテンツまたはわいせつなコンテンツのいずれかがイメージに含まれている可能性が評価されます。</span><span class="sxs-lookup"><span data-stu-id="07678-140">The confidence score rates the likelihood that the image contains either adult or racy content.</span></span> 

<span data-ttu-id="07678-141">この例では、次のイメージを使用しますが、他のイメージへの URL を指定して同じコマンドを自由に試すことができます。</span><span class="sxs-lookup"><span data-stu-id="07678-141">We'll use the following image for this example, but you're free to try the same command with URLs to other images.</span></span> 

![笑顔の家族の画像。](../media/3-people.png)

1. <span data-ttu-id="07678-143">URL 内の `<region>` を置き換えて、Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="07678-143">Execute the following command in Azure Cloud Shell, replacing `<region>` in the URL.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

<span data-ttu-id="07678-144">この例では、`visualFeatures` を `Adult,Description` に設定します。</span><span class="sxs-lookup"><span data-stu-id="07678-144">In this example, we set the `visualFeatures` to `Adult,Description`.</span></span> 

<span data-ttu-id="07678-145">応答では、2 つの信頼度スコアが返されます。1 つはわいせつなコンテンツに対するもので、もう 1 つは成人向けコンテンツに対するものです。</span><span class="sxs-lookup"><span data-stu-id="07678-145">The response gives us two confidence scores, one for racy content and one for adult content.</span></span> <span data-ttu-id="07678-146">これらのスコアに加えて、イメージの説明およびその他の視覚的特徴を使用することで、ご利用のサーバーにポストされるイメージへのフラグ付けを開始することができます。</span><span class="sxs-lookup"><span data-stu-id="07678-146">Using these scores plus the image description and other visual features, you can start to flag images posted to your server.</span></span>

<span data-ttu-id="07678-147">この演習で試みた例では、`analyze` 操作を使用して実行できる分析の種類について説明しました。</span><span class="sxs-lookup"><span data-stu-id="07678-147">The examples we tried in this exercise give you an idea of the type of analysis that you can do with the `analyze` operation.</span></span> <span data-ttu-id="07678-148">さまざまなイメージに対して分析を試してみてください。また、visualFeatures、details、languages の各パラメーターのさまざまな組み合わせも試してみてください。</span><span class="sxs-lookup"><span data-stu-id="07678-148">Try out the analysis with different images and try different combinations of visualFeatures, details, and languages parameters.</span></span>

<span data-ttu-id="07678-149">`analyze` 操作の詳細については、「[Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)」(イメージの分析) リファレンス ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="07678-149">For more information about the `analyze` operation, see the [Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) reference documentation.</span></span>