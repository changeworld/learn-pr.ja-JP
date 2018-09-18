このユニットでは、以前に作成した Computer Vision API サービスを使用して画像から手書き部分を抽出します。

# <a name="extracting-the-handwriting-from-an-image"></a>画像からの手書き部分の抽出

`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。 そのコマンドの出力を `key` 変数に格納します。

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、以前に宣言した `key` 変数を再利用します。

手書き認識に使用する画像はこのノートボードの写真です。

![手書き](../images/handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

上記のコマンドでは、ヘッダーを出力します。 次のコマンドに `Operation-Location` ヘッダーの値をコピーします。

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

ここで、手書き認識の要求の結果を含む JSON ファイルを受け取ります。

上記のサンプルで使用している URL を変更することで、他の画像を試して異なる結果を見ることができます。