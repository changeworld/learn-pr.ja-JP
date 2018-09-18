<span data-ttu-id="14186-101">このユニットでは、以前に作成した Computer Vision API サービスを使用して画像からテキストを抽出します。</span><span class="sxs-lookup"><span data-stu-id="14186-101">In this unit, you will extract text from an image with the Computer Vision API service that we created previously.</span></span>

# <a name="extracting-the-text-from-an-image"></a><span data-ttu-id="14186-102">画像からテキストを抽出</span><span class="sxs-lookup"><span data-stu-id="14186-102">Extracting the text from an image</span></span>

<span data-ttu-id="14186-103">`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。</span><span class="sxs-lookup"><span data-stu-id="14186-103">Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API.</span></span> <span data-ttu-id="14186-104">そのコマンドの出力を `key` 変数に格納します。</span><span class="sxs-lookup"><span data-stu-id="14186-104">Store the output of that command within the `key` variable.</span></span>

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

<span data-ttu-id="14186-105">`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、以前に宣言した `key` 変数を再利用します。</span><span class="sxs-lookup"><span data-stu-id="14186-105">Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.</span></span>

<span data-ttu-id="14186-106">光学式文字認識 (OCR) に使用する画像は、[.NET マイクロ サービス: コンテナー化された .NET アプリケーション用のアーキテクチャ](/dotnet/standard/microservices-architecture/)という本のカバーです。</span><span class="sxs-lookup"><span data-stu-id="14186-106">The image we're going to be using for Optical Character Recognition (OCR) is the cover from the book [.NET Microservices: Architecture for Containerized .NET Applications](/dotnet/standard/microservices-architecture/).</span></span>

![電子ブック カバー](../images/ebook.png)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/ocr" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/ebook.png\"}" | jq '.'
```

<span data-ttu-id="14186-108">最初の 3 つのプロパティは、言語、方向、テキストの角度です。</span><span class="sxs-lookup"><span data-stu-id="14186-108">The first three properties are the language, the orientation, and the text angle.</span></span> <span data-ttu-id="14186-109">次に、`regions` プロパティには、テキストの位置 (画像内における位置) とテキストに含まれている実際の単語を示すための一連の値が格納されます。</span><span class="sxs-lookup"><span data-stu-id="14186-109">Then, the `regions` properties will contain a list of values used to show where the text is, its position in the picture, and the actual words it contains.</span></span>

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions" : [
        /*... snipped*/
        {
          "boundingBox": "766,1419,302,33",
          "words": [
            {
              "boundingBox": "766,1419,126,25",
              "text": "Microsoft"
            },
            {
              "boundingBox": "903,1420,165,32",
              "text": "Corporation"
            }
          ]
        }]
}
```

<span data-ttu-id="14186-110">上記のサンプルで使用している URL を変更することで、他の画像を試して異なる結果を見ることができます。</span><span class="sxs-lookup"><span data-stu-id="14186-110">Experiment with other images to get different results by changing the URL used in the sample above.</span></span>