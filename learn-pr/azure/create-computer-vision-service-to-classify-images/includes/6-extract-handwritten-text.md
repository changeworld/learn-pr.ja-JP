<span data-ttu-id="bd578-101">Computer Vision API がイメージから印刷したテキストを抽出する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="bd578-101">We saw how the Computer Vision API can extract printed text from images.</span></span> <span data-ttu-id="bd578-102">この演習では、手書きのテキストを検出するためにサービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="bd578-102">In this exercise, we'll use the service to detect handwritten text.</span></span>

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a><span data-ttu-id="bd578-103">Computer Vision API を呼び出して手書きのテキストを抽出する</span><span class="sxs-lookup"><span data-stu-id="bd578-103">Calling the Computer Vision API to extract handwritten text</span></span>

<span data-ttu-id="bd578-104">`recognizeText` の操作では、メモ、手紙、レポート、ホワイトボード、用紙などのソースから手書きのテキストを検出し、抽出します。</span><span class="sxs-lookup"><span data-stu-id="bd578-104">The `recognizeText` operation detects and extracts handwritten text from notes, letters, essays, whiteboards, forms, and other sources.</span></span> <span data-ttu-id="bd578-105">要求 URL の形式は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="bd578-105">The request URL has the following format:</span></span>

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>`

<span data-ttu-id="bd578-106">すべての呼び出しはアカウントが作成されたリージョンに対して行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="bd578-106">All calls must be made to the region where the account was created.</span></span>

<span data-ttu-id="bd578-107">存在する場合、`mode` パラメーターを `Handwritten` または `Printed` に設定する必要があります。大文字と小文字は区別されます。</span><span class="sxs-lookup"><span data-stu-id="bd578-107">If present, the `mode` parameter must be set to `Handwritten` or `Printed` and is case-sensitive.</span></span> <span data-ttu-id="bd578-108">パラメーターが `Handwritten` に設定されているか、指定されていない場合、手書き認識が実行されます。</span><span class="sxs-lookup"><span data-stu-id="bd578-108">If the parameter is set to `Handwritten` or is not specified, handwriting recognition is performed.</span></span> <span data-ttu-id="bd578-109">パラメーターが `Printed` に設定されている場合、印刷テキスト認識が実行されます。</span><span class="sxs-lookup"><span data-stu-id="bd578-109">If the parameter is set to `Printed`, then printed text recognition is performed.</span></span> <span data-ttu-id="bd578-110">この呼び出しから結果を得るまでにかかる時間は、イメージの書き込みの量によって異なります。</span><span class="sxs-lookup"><span data-stu-id="bd578-110">The time it takes to get a result from this call depends on the amount of writing in the image.</span></span>

<span data-ttu-id="bd578-111">この例で行うことは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="bd578-111">In this example, we'll:</span></span>

- <span data-ttu-id="bd578-112">cURL の `-D` オプションを使用してコンソールに応答ヘッダーを印刷する</span><span class="sxs-lookup"><span data-stu-id="bd578-112">Print the response headers to the console using cUrl's `-D` option</span></span>
- <span data-ttu-id="bd578-113">応答で受け取るヘッダーから `Operation-Location` ヘッダー値をコピーする</span><span class="sxs-lookup"><span data-stu-id="bd578-113">Copy the `Operation-Location` header value from the headers we receive in the response</span></span>
- <span data-ttu-id="bd578-114">数秒後に、`Operation-Location` によって指定された URL で結果を確認する</span><span class="sxs-lookup"><span data-stu-id="bd578-114">After a few seconds, check the URL specified by the `Operation-Location` for the results</span></span>

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a><span data-ttu-id="bd578-115">イメージに含まれる手書きテキストの検出と抽出</span><span class="sxs-lookup"><span data-stu-id="bd578-115">Detect and extract handwritten text from an image</span></span>

<span data-ttu-id="bd578-116">この例では、次のイメージを使用しますが、他のイメージへの URL を指定して同じコマンドを自由に試すことができます。</span><span class="sxs-lookup"><span data-stu-id="bd578-116">We'll use the following image in this example, but you're free to try the same command with URLs to other images.</span></span>

![メモの手書きサンプルの画像](../media/6-handwriting.jpg)

1. <span data-ttu-id="bd578-118">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="bd578-118">Execute the following commands in Azure Cloud Shell.</span></span> <span data-ttu-id="bd578-119">コマンドの `<region>` を、ご利用の Cognitive Services アカウントのリージョンに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd578-119">Replace `<region>` in the command with the region of your cognitive services account.</span></span>

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

<span data-ttu-id="bd578-120">上記の場合、この操作のヘッダーがコンソールにダンプされます。</span><span class="sxs-lookup"><span data-stu-id="bd578-120">The above dumps the headers of this operation to the console.</span></span> <span data-ttu-id="bd578-121">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="bd578-121">Here's an example:</span></span>

```azurecli
HTTP/1.1 202 Accepted
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Expires: -1
Operation-Location: https://westus2.api.cognitive.microsoft.com/vision/v2.0/textOperations/d0e9b397-4072-471c-ae61-7490bec8f077
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
apim-request-id: f5663487-03c6-4760-9be7-c9157fac10a1
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
Date: Wed, 12 Sep 2018 19:22:00 GMT
```

<span data-ttu-id="bd578-122">`Operation-Location` ヘッダーには完了した結果が投稿されます。</span><span class="sxs-lookup"><span data-stu-id="bd578-122">The `Operation-Location` header is where the results will be posted once complete.</span></span>

2. <span data-ttu-id="bd578-123">`Operation-Location` ヘッダー値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="bd578-123">Copy the `Operation-Location` header value.</span></span>
1. <span data-ttu-id="bd578-124">Azure Cloud Shell で次のコマンドを実行し、`"<Operation-Location>"` を前の手順からコピーした **Operation-Location** ヘッダーの値と置き換えます。</span><span class="sxs-lookup"><span data-stu-id="bd578-124">Execute the following command in Azure Cloud Shell replacing `"<Operation-Location>"` with the value for the **Operation-Location** header you copied from the preceding step.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

<span data-ttu-id="bd578-125">操作が完了すると、手書き認識の要求の結果を含む JSON ファイルを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="bd578-125">If the operation has completed, you'll receive a JSON file containing the result of the handwriting recognition request.</span></span>

<span data-ttu-id="bd578-126">`recognizeText` 操作の詳細については、[手書きテキストの認識](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200)に関するリファレンス ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="bd578-126">For more information about the `recognizeText` operation, see the [Recognize Handwritten Text](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) reference documentation.</span></span>