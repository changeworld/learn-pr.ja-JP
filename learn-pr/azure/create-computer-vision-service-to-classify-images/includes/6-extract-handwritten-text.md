Computer Vision API がイメージから印刷したテキストを抽出する方法を説明しました。 この演習では、手書きのテキストを検出するためにサービスを使用します。

## <a name="calling-the-computer-vision-api-to-extract-handwritten-text"></a>Computer Vision API を呼び出して手書きのテキストを抽出する

`recognizeText` の操作では、メモ、手紙、レポート、ホワイトボード、用紙などのソースから手書きのテキストを検出し、抽出します。 要求 URL の形式は次のようになります。

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=<...>`

すべての呼び出しはアカウントが作成されたリージョンに対して行う必要があります。

存在する場合、`mode` パラメーターを `Handwritten` または `Printed` に設定する必要があります。大文字と小文字は区別されます。 パラメーターが `Handwritten` に設定されているか、指定されていない場合、手書き認識が実行されます。 パラメーターが `Printed` に設定されている場合、印刷テキスト認識が実行されます。 この呼び出しから結果を得るまでにかかる時間は、イメージの書き込みの量によって異なります。

この例で行うことは、次のとおりです。

- cURL の `-D` オプションを使用してコンソールに応答ヘッダーを印刷する
- 応答で受け取るヘッダーから `Operation-Location` ヘッダー値をコピーする
- 数秒後に、`Operation-Location` によって指定された URL で結果を確認する

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="detect-and-extract-handwritten-text-from-an-image"></a>イメージに含まれる手書きテキストの検出と抽出

この例では、次のイメージを使用しますが、他のイメージへの URL を指定して同じコマンドを自由に試すことができます。

![メモの手書きサンプルの画像](../media/6-handwriting.jpg)

1. Azure Cloud Shell で次のコマンドを実行します。 コマンドの `<region>` を、ご利用の Cognitive Services アカウントのリージョンに置き換えます。

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/recognizeText?mode=Handwritten" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/handwriting.jpg'}" \
-D - 
```

上記の場合、この操作のヘッダーがコンソールにダンプされます。 次に例を示します。

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

`Operation-Location` ヘッダーには完了した結果が投稿されます。

2. `Operation-Location` ヘッダー値をコピーします。
1. Azure Cloud Shell で次のコマンドを実行し、`"<Operation-Location>"` を前の手順からコピーした **Operation-Location** ヘッダーの値と置き換えます。

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>" | jq '.'
```

操作が完了すると、手書き認識の要求の結果を含む JSON ファイルを受け取ります。

`recognizeText` 操作の詳細については、[手書きテキストの認識](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200)に関するリファレンス ドキュメントを参照してください。