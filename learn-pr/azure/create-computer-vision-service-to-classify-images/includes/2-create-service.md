
## <a name="what-is-microsoft-cognitive-services"></a><span data-ttu-id="124c4-101">Microsoft Cognitive Services とは</span><span class="sxs-lookup"><span data-stu-id="124c4-101">What is Microsoft Cognitive Services?</span></span>

<span data-ttu-id="124c4-102">Microsoft Cognitive Services は、誰でもサービスとして使用できる機械学習アルゴリズムのセットです。</span><span class="sxs-lookup"><span data-stu-id="124c4-102">Microsoft Cognitive Services is a set of machine learning algorithms available as a service for anyone to use.</span></span> <span data-ttu-id="124c4-103">このインテリジェンスをアプリのためにゼロから構築する代わりに、視覚、音声、言語、知識、検索に対してこれらのサービスを使用できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-103">Instead of building this intelligence for our apps from scratch, we can use these services for vision, speech, language, knowledge, and search.</span></span> <span data-ttu-id="124c4-104">各サービスは無料で試すことができます。</span><span class="sxs-lookup"><span data-stu-id="124c4-104">You can try each service for free.</span></span> <span data-ttu-id="124c4-105">サービスをアプリに統合する場合は、有料のサブスクリプションにサインアップします。</span><span class="sxs-lookup"><span data-stu-id="124c4-105">When you decide to integrate a service into your app, you sign up for a paid subscription.</span></span> <span data-ttu-id="124c4-106">このモデルの Computer Vision API に注目します。</span><span class="sxs-lookup"><span data-stu-id="124c4-106">We'll focus our attention on the Computer Vision API in this model.</span></span>

> [!TIP]
> <span data-ttu-id="124c4-107">すべての Cognitive Services の一覧を確認するには、「[Cognitive Services のディレクトリ](https://azure.microsoft.com/services/cognitive-services/directory/)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="124c4-107">To see a list of all Cognitive Services, check out the [Cognitive Services Directory](https://azure.microsoft.com/services/cognitive-services/directory/).</span></span> 

## <a name="what-is-the-computer-vision-api"></a><span data-ttu-id="124c4-108">Computer Vision API とは</span><span class="sxs-lookup"><span data-stu-id="124c4-108">What is the Computer Vision API?</span></span>

<span data-ttu-id="124c4-109">Computer Vision API では、画像を処理して分析情報を返すアルゴリズムが提供されます。</span><span class="sxs-lookup"><span data-stu-id="124c4-109">The Computer Vision API provides algorithms to process images and return insights.</span></span> <span data-ttu-id="124c4-110">たとえば、画像に成人向けコンテンツが含まれるかどうか調べたり、画像からすべての顔を検索するのに使用したりできます。</span><span class="sxs-lookup"><span data-stu-id="124c4-110">For example, you can find out if an image has mature content, or can use it to find all the faces in an image.</span></span> <span data-ttu-id="124c4-111">他にも、ドミナント カラーやアクセント カラーの推定、画像の内容の分類、完全な英語の文による画像の説明などの機能もあります。</span><span class="sxs-lookup"><span data-stu-id="124c4-111">It also has other features like estimating dominant and accent colors, categorizing the content of images, and describing an image with complete English sentences.</span></span> <span data-ttu-id="124c4-112">さらに、大きな画像を効果的に表示するために画像のサムネイルをインテリジェントに生成することもできます。</span><span class="sxs-lookup"><span data-stu-id="124c4-112">Additionally, it can also intelligently generate images thumbnails for displaying large images effectively.</span></span>

> [!TIP]
> <span data-ttu-id="124c4-113">Computer Vision API は世界中の多くのリージョンで使用できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-113">The Computer Vision API is available in many regions across the globe.</span></span> <span data-ttu-id="124c4-114">近くのリージョンを探すには、「[リージョン別の利用可能な製品](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="124c4-114">To find the region nearest you, see the [Products available by region](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all).</span></span>

<span data-ttu-id="124c4-115">次の目的に Computer Vision API を使用できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-115">You can use the Computer Vision API to:</span></span>

- <span data-ttu-id="124c4-116">画像を分析して分析情報を得る</span><span class="sxs-lookup"><span data-stu-id="124c4-116">Analyze images for insight</span></span>
- <span data-ttu-id="124c4-117">光学式文字認識 (OCR) を使用して画像から印刷したテキストを抽出する</span><span class="sxs-lookup"><span data-stu-id="124c4-117">Extract printed text from images using optical character recognition (OCR).</span></span>
- <span data-ttu-id="124c4-118">画像から印刷テキストと手書きテキストを認識する</span><span class="sxs-lookup"><span data-stu-id="124c4-118">Recognize printed and handwritten text from images</span></span>
- <span data-ttu-id="124c4-119">著名人やランドマークを認識する</span><span class="sxs-lookup"><span data-stu-id="124c4-119">Recognize celebrities and landmarks</span></span>
- <span data-ttu-id="124c4-120">動画を分析する</span><span class="sxs-lookup"><span data-stu-id="124c4-120">Analyze video</span></span> 
- <span data-ttu-id="124c4-121">画像のサムネイルを生成する</span><span class="sxs-lookup"><span data-stu-id="124c4-121">Generate a thumbnail of an image</span></span> 

## <a name="how-to-call-the-computer-vision-api"></a><span data-ttu-id="124c4-122">Computer Vision API を呼び出す方法</span><span class="sxs-lookup"><span data-stu-id="124c4-122">How to call the Computer Vision API</span></span>

<span data-ttu-id="124c4-123">アプリケーションで Computer Vision を呼び出すには、クライアント ライブラリまたは直接 REST API を使用します。</span><span class="sxs-lookup"><span data-stu-id="124c4-123">You call Computer Vision in your application using client libraries or the REST API directly.</span></span> <span data-ttu-id="124c4-124">このモジュールでは REST API を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="124c4-124">We'll call the REST API in this module.</span></span> <span data-ttu-id="124c4-125">呼び出しを行うには次のようにします。</span><span class="sxs-lookup"><span data-stu-id="124c4-125">To make a call:</span></span>

1. <span data-ttu-id="124c4-126">API アクセス キーを取得する</span><span class="sxs-lookup"><span data-stu-id="124c4-126">Get an API access key</span></span>

    <span data-ttu-id="124c4-127">Computer Vision サービス アカウントにサインアップすると、アクセス キーを割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="124c4-127">You are assigned access keys when you sign up for a Computer Vision service account.</span></span> <span data-ttu-id="124c4-128">**すべての**要求のヘッダーでキーを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="124c4-128">A key must be passed in the header of **every** request.</span></span> 

1. <span data-ttu-id="124c4-129">API に対して POST 呼び出しを行う</span><span class="sxs-lookup"><span data-stu-id="124c4-129">Make a POST call to the API</span></span>

    <span data-ttu-id="124c4-130">URL の書式を次のように設定します。**<リージョン>**.api.cognitive.microsoft.com/vision/v2.0/**<リソース>**/**<パラメーター>**</span><span class="sxs-lookup"><span data-stu-id="124c4-130">Format the URL as follows: **region**.api.cognitive.microsoft.com/vision/v2.0/**resource**/**[parameters]**</span></span> 

    - <span data-ttu-id="124c4-131">**<リージョン>** - アカウントを作成したリージョンです (例: *westus*)。</span><span class="sxs-lookup"><span data-stu-id="124c4-131">**region** - the region where you created the account, for example, *westus*.</span></span>
    - <span data-ttu-id="124c4-132">**<リソース>** - 呼び出す Computer Vision リソースです (例: `analyze`、`describe`、`generateThumbnail`、`ocr`、`models`、`recognizeText`、`tag`)。</span><span class="sxs-lookup"><span data-stu-id="124c4-132">**resource** - the Computer Vision resource you are calling such as `analyze`, `describe`, `generateThumbnail`, `ocr`, `models`, `recognizeText`, `tag`.</span></span>

    <span data-ttu-id="124c4-133">処理対象の画像は、生画像バイナリまたは画像の URL として提供できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-133">You can supply the image to be processed either as a raw image binary or an image URL.</span></span>

    <span data-ttu-id="124c4-134">要求ヘッダーには、この API へのアクセスを提供するサブスクリプション キーを含める必要があります。</span><span class="sxs-lookup"><span data-stu-id="124c4-134">The request header must contain the subscription key, which provides access to this API.</span></span>

1. <span data-ttu-id="124c4-135">応答を解析する</span><span class="sxs-lookup"><span data-stu-id="124c4-135">Parse the response</span></span>

    <span data-ttu-id="124c4-136">応答には、Computer Vision API によって画像から検出された分析情報が JSON ペイロードとして保持されています。</span><span class="sxs-lookup"><span data-stu-id="124c4-136">The response holds the insight the Computer Vision API had about your image as a JSON payload.</span></span>

<span data-ttu-id="124c4-137">`westus` リージョンで Computer Vision サブスクリプションを作成し、API にアクセスするためのサブスクリプション キーを取得したものとします。</span><span class="sxs-lookup"><span data-stu-id="124c4-137">Suppose you created my Computer Vision subscription in the `westus` region and got a subscription key to access the API.</span></span> <span data-ttu-id="124c4-138">キーは架空の値 `xxxx-xxxxx-xxxx-xxxx` にしましょう。</span><span class="sxs-lookup"><span data-stu-id="124c4-138">Let's give the key a fictitious value of `xxxx-xxxxx-xxxx-xxxx`.</span></span> <span data-ttu-id="124c4-139">そして、`http://example.com/images/test.jpg` にある画像のサムネイルを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="124c4-139">You now want to generate a thumbnail for an image located at `http://example.com/images/test.jpg`.</span></span> <span data-ttu-id="124c4-140">`generateThumbnail` を使用すると、要求は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="124c4-140">Using `generateThumbnail`, the request would look like the following.</span></span>

```
curl POST "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {xxxx-xxxxx-xxxx-xxxx}"
-d "{'url':'http://example.com/images/test.jpg'}"
```

<span data-ttu-id="124c4-141">このモジュールでは、統合された Cloud Shell を使用して Azure CLI ですべての演習を実行します。</span><span class="sxs-lookup"><span data-stu-id="124c4-141">In this module, we'll run all exercises in the Azure CLI using the integrated Cloud Shell.</span></span> <span data-ttu-id="124c4-142">このセットアップについてもう少し調べてみましょう。</span><span class="sxs-lookup"><span data-stu-id="124c4-142">Let's find out a little more about this setup.</span></span>

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="124c4-143">Azure CLI とは</span><span class="sxs-lookup"><span data-stu-id="124c4-143">What is the Azure CLI</span></span>

<span data-ttu-id="124c4-144">Azure CLI は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="124c4-144">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources.</span></span> <span data-ttu-id="124c4-145">このツールは、macOS、Linux、および Windows で使用できます。またはブラウザーで [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を使用して使用できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-145">It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="124c4-146">現在使用できる Azure CLI ツールには、Azure CLI 1.0 と Azure CLI 2.0 の 2 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="124c4-146">There are two versions of the Azure CLI tool available today: Azure CLI 1.0 and Azure CLI 2.0.</span></span> <span data-ttu-id="124c4-147">ここでは最新バージョンである Azure CLI 2.0 を使用します。レガシ スクリプトを実行する場合を除いて、こちらを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="124c4-147">We'll be using Azure CLI 2.0, which is the latest version and is recommended unless you're running legacy scripts.</span></span> <span data-ttu-id="124c4-148">Azure CLI 1.0 は `azure` コマンドで始まり、Azure CLI 2.0 は `az` コマンドで始まります。</span><span class="sxs-lookup"><span data-stu-id="124c4-148">Azure CLI 1.0 is started with the `azure` command, and Azure CLI 2.0 is started with the `az` command.</span></span>

## <a name="az-cognitiveservices-commands"></a><span data-ttu-id="124c4-149">az cognitiveservices のコマンド</span><span class="sxs-lookup"><span data-stu-id="124c4-149">az cognitiveservices commands</span></span>

<span data-ttu-id="124c4-150">Azure CLI には、Azure で Cognitive Services アカウントを管理するための `cognitiveservices` コマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="124c4-150">The Azure CLI includes the `cognitiveservices` command to manage Cognitive Services accounts in Azure.</span></span> <span data-ttu-id="124c4-151">特定のタスクを実行するためのサブコマンドがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="124c4-151">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="124c4-152">最も一般的なものを次に示します。</span><span class="sxs-lookup"><span data-stu-id="124c4-152">The most common include:</span></span>

| <span data-ttu-id="124c4-153">サブコマンド</span><span class="sxs-lookup"><span data-stu-id="124c4-153">Subcommand</span></span> | <span data-ttu-id="124c4-154">説明</span><span class="sxs-lookup"><span data-stu-id="124c4-154">Description</span></span> |
|-------------|-------------|
| `list` | <span data-ttu-id="124c4-155">利用可能な Azure Cognitive Services アカウントを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="124c4-155">List available Azure Cognitive Services accounts.</span></span> |
| `account show` | <span data-ttu-id="124c4-156">Azure Cognitive Services アカウントの詳細を取得します。</span><span class="sxs-lookup"><span data-stu-id="124c4-156">Get the details of an Azure Cognitive Services account.</span></span> |
| `account create` | <span data-ttu-id="124c4-157">Azure Cognitive Services アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="124c4-157">Create an Azure Cognitive Services account.</span></span> |
| `account delete` | <span data-ttu-id="124c4-158">Azure Cognitive Services アカウントを削除します。</span><span class="sxs-lookup"><span data-stu-id="124c4-158">Delete an Azure Cognitive Services account.</span></span> |
| `keys list` | <span data-ttu-id="124c4-159">Azure Cognitive Services アカウントのキーを一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="124c4-159">List the keys of an Azure Cognitive Services account.</span></span> |

<span data-ttu-id="124c4-160">Azure CLI を使用して、Cognitive Services を作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="124c4-160">Let's create a Cognitive Services with the Azure CLI.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-cognitive-services-account"></a><span data-ttu-id="124c4-161">Cognitive Services アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="124c4-161">Create a Cognitive Services account</span></span>

<span data-ttu-id="124c4-162">Computer Vision API を呼び出すには、API アクセス キーが必要です。</span><span class="sxs-lookup"><span data-stu-id="124c4-162">We need an API access key to make calls to the Computer Vision API.</span></span> <span data-ttu-id="124c4-163">アクセス キーを取得するには、Computer Vision API 用の Cognitive Services アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="124c4-163">To get access keys, we need a Cognitive Services account for the Computer Vision API.</span></span> <span data-ttu-id="124c4-164">サブスクリプションでアカウントを作成するには、`az cognitiveservices create` を使用します。</span><span class="sxs-lookup"><span data-stu-id="124c4-164">We'll use `az cognitiveservices create` to create the account in our subscription.</span></span>

 <span data-ttu-id="124c4-165">リソース グループに Cognitive Services アカウントを作成するには、`az cognitiveservices create` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="124c4-165">The command `az cognitiveservices create` is used to create a Cognitive Services account in a resource group.</span></span>  <span data-ttu-id="124c4-166">このコマンドを呼び出すときは、次の 5 つのパラメーターを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="124c4-166">The following five parameters must be supplied when calling this command.</span></span>

> [!Tip]
> <span data-ttu-id="124c4-167">Azure CLI のパラメーターで使用するほとんどのフラグは、1 文字に省略できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-167">Most flags for Azure CLI parameters can be abbreviated to a single character.</span></span> <span data-ttu-id="124c4-168">たとえば、`--location` の代わりに `-l` と指定できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-168">For example, we could say `-l` instead of `--location`.</span></span> <span data-ttu-id="124c4-169">長い形式は、わかりやすくするために使用されます。</span><span class="sxs-lookup"><span data-stu-id="124c4-169">The long-form is used for clarity.</span></span>

| <span data-ttu-id="124c4-170">パラメーター</span><span class="sxs-lookup"><span data-stu-id="124c4-170">Parameter</span></span> | <span data-ttu-id="124c4-171">説明</span><span class="sxs-lookup"><span data-stu-id="124c4-171">Description</span></span> |
|-----------|-------------|
| `resource-group` | <span data-ttu-id="124c4-172">Cognitive Services アカウントを所有するリソース グループです。</span><span class="sxs-lookup"><span data-stu-id="124c4-172">The resource group that will own the cognitive services account.</span></span> <span data-ttu-id="124c4-173">この対話型サンドボックス セッションでは、<rgn>[サンドボックス リソース グループ名]</rgn> を使用します</span><span class="sxs-lookup"><span data-stu-id="124c4-173">In this interactive sandbox session, you'll use <rgn>[sandbox resource group name]</rgn></span></span> |
| `kind` | <span data-ttu-id="124c4-174">Cognitive Services アカウントの API 名です。</span><span class="sxs-lookup"><span data-stu-id="124c4-174">The API name of cognitive services account.</span></span> |
| `name` | <span data-ttu-id="124c4-175">Cognitive Services アカウントの名前です。</span><span class="sxs-lookup"><span data-stu-id="124c4-175">Cognitive service account name.</span></span> |
| `sku` | <span data-ttu-id="124c4-176">Cognitive Services アカウントの SKU です。</span><span class="sxs-lookup"><span data-stu-id="124c4-176">The Sku of cognitive services account.</span></span>|
| `location` | <span data-ttu-id="124c4-177">この API の呼び出しを行う場所つまりリージョンです。</span><span class="sxs-lookup"><span data-stu-id="124c4-177">The location, or region, from which you want to make calls to this API.</span></span> <span data-ttu-id="124c4-178">次の一覧からいずれかの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="124c4-178">Select one of the locations from the list below.</span></span> |

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)] 

<span data-ttu-id="124c4-179">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="124c4-179">Execute the following command in Azure Cloud Shell.</span></span> <span data-ttu-id="124c4-180">`[location]` は近くの場所に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="124c4-180">Make sure to replace `[location]` with a location near you.</span></span>

```azurecli
az cognitiveservices account create \
--kind ComputerVision \
--name ComputerVisionService \
--sku S1 \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--location [location]
```

> [!NOTE]
> <span data-ttu-id="124c4-181">選択した場所を憶えておいてください。</span><span class="sxs-lookup"><span data-stu-id="124c4-181">Remember the location you have selected.</span></span> <span data-ttu-id="124c4-182">API に対するすべての呼び出しは、このリージョンから行います。</span><span class="sxs-lookup"><span data-stu-id="124c4-182">You'll make all calls to the API from that region.</span></span>

<span data-ttu-id="124c4-183">**Computer Vision** API 用の Cognitive Services アカウントを作成しました。</span><span class="sxs-lookup"><span data-stu-id="124c4-183">We've created a cognitive services account for the **ComputerVision** API.</span></span> <span data-ttu-id="124c4-184">*S1* SKU を選択し、アカウントの名前を **ComputerVisionService** にしました。</span><span class="sxs-lookup"><span data-stu-id="124c4-184">We selected the *S1* sku and named our account **ComputerVisionService**.</span></span> <span data-ttu-id="124c4-185">このアカウントはリソース グループ **<rgn>[サンドボックス リソース グループ名]</rgn>** によって所有されており、`--location` パラメーターで設定した場所から API を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="124c4-185">Our account is owned by the resource group **<rgn>[sandbox resource group name]</rgn>** and we'll call the API from the location we set in the `--location` parameter.</span></span> 

<span data-ttu-id="124c4-186">コマンドで Cognitive Services アカウントの作成が完了すると、**provisioningState** プロパティが **Succeeded** に設定された JSON 応答を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="124c4-186">Once the command finishes creating the cognitive services account, you'll get a JSON response, which includes **provisioningState** property set to **Succeeded**.</span></span>

## <a name="get-an-access-key"></a><span data-ttu-id="124c4-187">アクセス キーを取得する</span><span class="sxs-lookup"><span data-stu-id="124c4-187">Get an access key</span></span>

<span data-ttu-id="124c4-188">アカウントが正常に作成されると、そのアカウントのサブスクリプション キーつまりアクセス キーを取得できます。</span><span class="sxs-lookup"><span data-stu-id="124c4-188">Once we have our account successfully created we can retrieve the subscription keys, or access keys, for this account.</span></span>

1. <span data-ttu-id="124c4-189">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="124c4-189">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn>
    ```
    
    <span data-ttu-id="124c4-190">上のコマンドでは、特定のリソース グループによって所有されている **ComputerVisionService** という名前の Cognitive Services アカウントに関連付けられているキーが返されます。</span><span class="sxs-lookup"><span data-stu-id="124c4-190">The above command returns the keys associated with the cognitive services account called **ComputerVisionService**, which is owned by the given resource group.</span></span> <span data-ttu-id="124c4-191">2 つのキーが返されますが、1 つは予備のキーです。</span><span class="sxs-lookup"><span data-stu-id="124c4-191">It returns two keys - one is a spare key.</span></span> <span data-ttu-id="124c4-192">キーは憶えにくいので、1 番目のキーを変数に格納し、API のすべての呼び出しにそれを使用します。</span><span class="sxs-lookup"><span data-stu-id="124c4-192">The keys are difficult to remember, so we'll store the first key in a variable that we'll use for all calls to the API.</span></span>

2.  <span data-ttu-id="124c4-193">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="124c4-193">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    key=$(az cognitiveservices account keys list \
    --name ComputerVisionService \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --query key1 -o tsv)
    ```
    
    <span data-ttu-id="124c4-194">Azure CLI 2.0 では、`--query` 引数を使用して、コマンドの結果に対して JMESPath クエリを実行します。</span><span class="sxs-lookup"><span data-stu-id="124c4-194">The Azure CLI 2.0 uses the `--query` argument to execute a JMESPath query on the results of commands.</span></span> <span data-ttu-id="124c4-195">JMESPath は、CLI の出力からデータを選択して表示できるようにする、JSON 用のクエリ言語です。</span><span class="sxs-lookup"><span data-stu-id="124c4-195">JMESPath is a query language for JSON, giving you the ability to select and present data from CLI output.</span></span> <span data-ttu-id="124c4-196">これらのクエリは、表示書式設定の前に、JSON 出力に対して実行されます。</span><span class="sxs-lookup"><span data-stu-id="124c4-196">These queries are executed on the JSON output before any display formatting.</span></span>
    <span data-ttu-id="124c4-197">`--query` 引数は、Azure CLI のすべてのコマンドでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="124c4-197">The `--query` argument is supported by all commands in the Azure CLI.</span></span> 
    
    <span data-ttu-id="124c4-198">この例では、"key1" という名前のエントリに対するキーの一覧をクエリし、結果を **tsv** の形式に出力します。</span><span class="sxs-lookup"><span data-stu-id="124c4-198">In our example, we query the list of keys for an entry named "key1" and output the result to **tsv** format.</span></span> <span data-ttu-id="124c4-199">この形式では、文字列値を囲む引用符が削除されます。</span><span class="sxs-lookup"><span data-stu-id="124c4-199">This format removes quotations around the string value.</span></span> <span data-ttu-id="124c4-200">結果を変数 **key** に代入します。</span><span class="sxs-lookup"><span data-stu-id="124c4-200">We assign the result to a variable **key**.</span></span>
    
    > [!IMPORTANT]
    > <span data-ttu-id="124c4-201">モジュール全体でこのキーを使用するので、変数に保存するのはよい方法です。</span><span class="sxs-lookup"><span data-stu-id="124c4-201">We're going to use this key throughout the module, so saving it in a variable is a good idea.</span></span> <span data-ttu-id="124c4-202">値を紛失したり、変数が設定されなくなったときは、コマンドをもう一度実行して設定します。</span><span class="sxs-lookup"><span data-stu-id="124c4-202">If you lose the value or the variable becomes unset, run the command again to set it.</span></span>  

3. <span data-ttu-id="124c4-203">キーの値を表示するには、Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="124c4-203">To see the value of our key, execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    echo $key
    ```

<span data-ttu-id="124c4-204">アカウントとキーが用意できたので、いよいよ API の呼び出しを実行します。</span><span class="sxs-lookup"><span data-stu-id="124c4-204">Now that we have an account and a key, it's time to make some calls to the API.</span></span>