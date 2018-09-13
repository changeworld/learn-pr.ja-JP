このユニットでは、以前に作成した Computer Vision API サービスを使用して画像からテキストを抽出します。

# <a name="extracting-the-text-from-an-image"></a>画像からテキストを抽出

`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。 そのコマンドの出力を `key` 変数に格納します。

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、以前に宣言した `key` 変数を再利用します。

光学式文字認識 (OCR) に使用する画像は、[.NET マイクロ サービス: コンテナー化された .NET アプリケーション用のアーキテクチャ](/dotnet/standard/microservices-architecture/)という本のカバーです。

![電子ブック カバー](../images/ebook.png)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/ocr" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/ebook.png\"}" | jq '.'
```

最初の 3 つのプロパティは、言語、方向、テキストの角度です。 次に、`regions`プロパティ一に、テキストの位置を示すのに使われる値の一覧が含められます。これらは画像の中のテキストの位置と、実際の言葉を示します。

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

上記のサンプルで使用している URL を変更することで、他の画像を試して異なる結果を見ることができます。