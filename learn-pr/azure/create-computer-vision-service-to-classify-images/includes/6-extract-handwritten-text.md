Computer Vision API がイメージから印刷したテキストを抽出する方法を説明しました。 この演習で手書きのテキストを検出するためにサービスを使用します。

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a>手書きのテキストを抽出する Computer Vision API を呼び出す

`recognizeText`操作が検出し、メモ、レター、レポート、ホワイト ボード、フォーム、およびその他のソースから手書きのテキストを抽出します。 要求 URL では、次の形式があります。

`https://[location].api.cognitive.microsoft.com/vision/v2.0/recognizeText[?mode] `

通常どおり、すべての呼び出しは、アカウントが作成された場所に実行する必要があります。 場合、`mode`パラメーター ust に設定する`Handwritten`または`Printed`されが区別されます。 パラメーターに設定されている場合`Handwritten`またはが指定されていない場合、手書き認識が実行されます。 パラメーターを設定する場合は、 `Printed` OCR 操作を呼び出すことによって印刷されるテキストの認識を実行します。

この呼び出しから結果を得るにかかる時間は、イメージでは、手書きの量によって異なります。 そのため、いくつかの秒間待機することがあります。 そのため、この例で作成します。

- CUrl を使用してコンソールに応答ヘッダーをダンプ`-D`オプション
- コピー、`Operation-Location`応答で受信ヘッダーからのヘッダー値
- 指定された URL を確認すると、数秒後に、`Operation-Location`結果を得る

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a>画像に含まれる手書きテキストの検出と抽出

この例では、次の図を使用しますが、自由に他の画像の Url で同じコマンドを再試行してください。

![メモの手書きサンプルの画像](../media/6-handwriting.jpg)

1. Azure Cloud Shell で次のコマンドを実行します。

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

コンソールには、この操作のヘッダーを上記にダンプします。 次に例を示します。

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

`Operation-Location`ヘッダーが完了するとに、結果が投稿されます。

2. コピー、`Operation-Location`ヘッダーの値。
1. Azure Cloud Shell の交換で、次のコマンドを実行`"<Operation-Location>"`の値を持つ、**操作場所**前の手順からコピーしたヘッダー。

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

操作が完了した場合は、手書き認識の要求の結果を含む JSON ファイルを受け取ります。

詳細については、`recognizeText`操作を参照してください、[手書きテキストの認識](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200)リファレンス ドキュメント。