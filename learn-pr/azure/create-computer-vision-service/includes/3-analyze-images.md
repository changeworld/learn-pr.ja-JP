<span data-ttu-id="51ccb-101">このユニットでは、前のステップで作成した Computer Vision API サービスを使用して画像を分析します。</span><span class="sxs-lookup"><span data-stu-id="51ccb-101">In this unit, you will analyze images with the Computer Vision API service that we created in the previous step.</span></span>

# <a name="analyzing-an-image-with-computer-vision-api"></a><span data-ttu-id="51ccb-102">Computer Vision API を使用して画像を分析する</span><span class="sxs-lookup"><span data-stu-id="51ccb-102">Analyzing an image with Computer Vision API</span></span>

<span data-ttu-id="51ccb-103">`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。</span><span class="sxs-lookup"><span data-stu-id="51ccb-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="51ccb-104">そのコマンドの出力を `key` 変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="51ccb-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="51ccb-105">`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、前に宣言した `key` 変数を再利用します。</span><span class="sxs-lookup"><span data-stu-id="51ccb-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reusing the previously declared variable `key`.</span></span>

<span data-ttu-id="51ccb-106">サービスに送信されるパラメーターは、`visualFeatures`、`details`、`languages` です。</span><span class="sxs-lookup"><span data-stu-id="51ccb-106">The parameters that are sent to the service are `visualFeatures`, `details`, and `languages`.</span></span> <span data-ttu-id="51ccb-107">これらのパラメーターがなくてもサービスは機能しますが、これらは画像の内容に関するヒントをサービスに提供します。</span><span class="sxs-lookup"><span data-stu-id="51ccb-107">While without them, the service will still work, it provides hints to the service on the content of the images.</span></span> <span data-ttu-id="51ccb-108">たとえば、`details` を `Landmarks` または `Celebrities` に設定して、ランドマークまたは著名人であることを示すことができます。</span><span class="sxs-lookup"><span data-stu-id="51ccb-108">As an example, `details` can be set to `Landmarks` or `Celebrities` to help you identify landmark or celebrities.</span></span> <span data-ttu-id="51ccb-109">`visualFeatures` は、サービスから戻される情報の種類を示します。</span><span class="sxs-lookup"><span data-stu-id="51ccb-109">`visualFeatures` identify what kind of information you want the service to return you.</span></span> <span data-ttu-id="51ccb-110">`Categories` オプションは、木やビルのように、画像の内容を分類します。</span><span class="sxs-lookup"><span data-stu-id="51ccb-110">`Categories` option will categorize the content of the images like trees, building, and more.</span></span> <span data-ttu-id="51ccb-111">`Faces` は人の顔を示し、性別と年齢が提供されます。</span><span class="sxs-lookup"><span data-stu-id="51ccb-111">`Faces` will identify people's faces and give you their gender and age.</span></span>

<span data-ttu-id="51ccb-112">詳細については、[こちらのドキュメント](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="51ccb-112">More details can be found in [the documentation](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa).</span></span>

<span data-ttu-id="51ccb-113">ここでは、ランドマークを示し、サービスから `Categories` と `Description` を取得することにします。</span><span class="sxs-lookup"><span data-stu-id="51ccb-113">For now, let's identify landmarks and have the service return us `Categories` and `Description`.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}"
```

<span data-ttu-id="51ccb-114">要求の結果は、`url` の画像を説明する未加工の JSON です。</span><span class="sxs-lookup"><span data-stu-id="51ccb-114">The result of the request is the raw JSON describing the picture in the `url`.</span></span> <span data-ttu-id="51ccb-115">末尾に ` | jq '.'` を追加して、JSON の出力を修飾することもできます。</span><span class="sxs-lookup"><span data-stu-id="51ccb-115">We can also add ` | jq '.'` at the end to prettify the JSON output.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" | jq '.'
```

<span data-ttu-id="51ccb-116">オンラインで見つかった他の画像へのリンクをコピーし、上記のコマンドの URL を置き換えることで、サービスを試してみてください。</span><span class="sxs-lookup"><span data-stu-id="51ccb-116">Copy the link to any other images found online and try it with the service by replacing the URL in the above commands.</span></span>