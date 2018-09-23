現場の販売業者がサーバーに投稿するストック イメージからテキストを読み取る必要があるとします。 つまり、販売価格を含むプロモーション ステッカーを探すために製品をスキャンする必要があります。 ここで、Computer Vision API の光学式文字認識 (OCR) 機能を試します。 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a>Computer Vision API を呼び出して印刷されたテキストを抽出する

`ocr` 操作ではイメージのテキストを検出し、認識された文字をコンピューターで扱えるように文字ストリームに抽出します。 要求 URL の形式は次のようになります。

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr?language=<...>&detectOrientation=<...>`

通常どおり、すべての呼び出しはアカウントが作成されたリージョンに対して行う必要があります。 呼び出しでは、2 つの省略可能なパラメーターを受け入れます。

- **言語**: イメージで検出されるテキストの言語コード。 既定値は `unk` (つまり、不明) です。 これにより、サービスでイメージ内のテキストの言語を自動検出できるようになります。
- **detectOrientation**: true の場合、サービスでは、さらに処理を行う前に、イメージの向きの検出とその修正が試行されます。たとえば、イメージが上下反対になっていないかどうかが確認されます。 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a>OCR を使用してイメージから印刷されたテキストを抽出する

光学式文字認識 (OCR) に使用するイメージは、*.NET Microservices: Architecture for Containerized .NET Applications* (.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ) という本のカバーです。

![.NET Microservices: Architecture for Containerized .NET Application (.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ) という電子ブックのカバーの図](../media/5-ebook.png)

1. Cloud Shell で次のコマンドを実行します。 コマンドの `<region>` を、ご利用の Cognitive Services アカウントのリージョンに置き換えます。

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

次の JSON は、この呼び出しからの応答例です。 JSON の一部の行は、スニペットがページに収まるように削除されています。

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

この応答について詳しく見ていきましょう。 

- サービスではテキストが英語と認識されました。 `language` フィールドの値には、イメージで検出されたテキストの BCP-47 言語コードが含まれています。 この例では、**en** (つまり、英語) となっています。 
- `orientation` は **up** として検出されました。 このプロパティは、イメージがその中心を軸に、検出されたテキストの角度に応じて回転された後の、認識されたテキストの先頭の向きを示しています。 
- `textAngle` は、テキストを水平または垂直になるように回転する必要がある角度です。 この例では、テキストが完全に水平だったため、返された値は **0** となっています。  
- `regions` プロパティには、テキストの位置、図におけるその位置、およびイメージのその部分で検出された単語を示すために使用される値のリストが含まれています。 
- `boundingBox` の値の 4 つの整数は次のとおりです。 
    - 左端の x 座標 
    - 上端の y 座標
    - 境界ボックスの幅
    - 境界ボックスの高さ 
   
    これらの値を使用して、イメージで検出されたテキストをすべて囲むボックスを描画することができます。

この演習でわかるように、`ocr` サービスによって、イメージ内の印刷されたテキストに関する詳細が示されます。 

`ocr` 操作の詳細については、[OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) のリファレンス ドキュメントを参照してください。