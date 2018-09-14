これで、第一線のディストリビューターが、サーバーにポストされるストック イメージからテキストを読み取るしたいとします。 具体的には、製品を含むプロモーション ステッカーを探してをスキャンする販売価格。 Computer Vision API の光学式文字認識 (OCR) 機能を試すための時間になります。 

## <a name="calling-the-computer-vision-api-to-extract-printed-text"></a>印刷したテキストを抽出する Computer Vision API を呼び出す

`ocr`操作は、イメージのテキストを検出し、マシンで使用可能な文字ストリームに認識されている文字を抽出します。 要求 URL では、次の形式があります。

`https://[location].api.cognitive.microsoft.com/vision/v2.0/ocr[?language][&detectOrientation ] `

通常どおり、すべての呼び出しは、アカウントが作成された場所に実行する必要があります。 呼び出しでは、2 つの省略可能なパラメーターを受け取ります。

- **言語**: イメージで検出されたテキストの言語コード。 既定値は`unk`、または不明です。 このサービスの自動がイメージのテキストの言語を検出しましょう。
- **detectOrientation**: true の場合、サービスは、イメージの向きを検出し、さらに処理する前に修正しますが、たとえば、かどうか、イメージが上下します。 

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="extract-printed-text-from-an-image-using-ocr"></a>OCR を使用してイメージから印刷したテキストを抽出します。

光学式文字認識 (OCR) に使用する画像は、*.NET マイクロ サービス: コンテナー化された .NET アプリケーション用のアーキテクチャ*という本のカバーです。

![.NET マイクロ サービス電子ブックのカバーの画像: コンテナー化された .NET アプリケーションのアーキテクチャ](../media/5-ebook.png)

1. Azure Cloud Shell で次のコマンドを実行します。

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/ocr" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json"  \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/ebook.png'}" \
 | jq '.'
```

次の JSON では、この呼び出しから取得応答の例を示します。 ページに収まるように、スニペットには、JSON の一部の行が削除されました。

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

- サービスは、テキストをテキストとして英語を特定します。 値、`language`フィールドには、イメージで検出されたテキストの bcp-47 言語コードが含まれています。 この例では**en**、または英語版。 
- `orientation`として検出された**を**します。 このプロパティは、イメージが検出されたテキストの角度に応じて、その中心の周り回転された後、認識されたテキストの先頭が向いている方向。 
- `textAngle`水平または垂直方向にテキストを回転する必要があります角度のことです。 この例では、テキストが完全に水平ため、返される値は**0**します。  
- `regions`プロパティには値のテキストが、図では、その位置を示すために使用し、イメージの部分で見つかった単語の一覧が含まれています。 
- 4 つの整数、`boundingBox`値します。 
    - 左端の x 座標 
    - 上端の y 座標
    - 境界ボックスの幅
    - 境界ボックスの高さ 
   
    これらの値は、テキスト、イメージ内のすべての四角形を描画するために使用できます。

この演習でわかるように、`ocr`サービスでは、画像の印刷したテキストに関する詳細情報。 

詳細については、`ocr`操作を参照してください、 [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc)リファレンス ドキュメント。