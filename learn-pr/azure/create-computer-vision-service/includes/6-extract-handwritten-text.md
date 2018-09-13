<span data-ttu-id="226e9-101">このユニットでは、以前に作成した Computer Vision API サービスを使用して画像から手書き部分を抽出します。</span><span class="sxs-lookup"><span data-stu-id="226e9-101">In this unit, you will extract hand writing from an image with the Computer Vision API service that we created previously.</span></span>

# <a name="extracting-the-hand-writing--from-an-image"></a><span data-ttu-id="226e9-102">画像から手書き部分を抽出</span><span class="sxs-lookup"><span data-stu-id="226e9-102">Extracting the hand writing  from an image</span></span>

<span data-ttu-id="226e9-103">`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。</span><span class="sxs-lookup"><span data-stu-id="226e9-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="226e9-104">そのコマンドの出力を `key` 変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="226e9-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="226e9-105">`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、以前に宣言した `key` 変数を再利用します。</span><span class="sxs-lookup"><span data-stu-id="226e9-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reusing the previously declared variable `key`.</span></span>

<span data-ttu-id="226e9-106">手書き認識に使用する画像はこのノートボードの写真です。</span><span class="sxs-lookup"><span data-stu-id="226e9-106">The image we're going to be using for hand writing recognition is a picture of this note board.</span></span>

![手書き](../images/handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

<span data-ttu-id="226e9-108">上記のコマンドでは、ヘッダーを出力します。</span><span class="sxs-lookup"><span data-stu-id="226e9-108">The command above will output the headers.</span></span> <span data-ttu-id="226e9-109">次のコマンドに`Operation-Location`ヘッダーの値をコピーします。</span><span class="sxs-lookup"><span data-stu-id="226e9-109">Copy the `Operation-Location` header value into the command below.</span></span>

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

<span data-ttu-id="226e9-110">ここで、手書き認識の要求の結果を含む JSON ファイルを受け取ります。</span><span class="sxs-lookup"><span data-stu-id="226e9-110">You will now receive a JSON file containing the result of the handwriting recognition request.</span></span>

<span data-ttu-id="226e9-111">上記のサンプルで使用している URL を変更することで、他の画像を試して異なる結果を見ることができます。</span><span class="sxs-lookup"><span data-stu-id="226e9-111">Experiment with other images to get different result by changing the URL used in the sample above.</span></span>