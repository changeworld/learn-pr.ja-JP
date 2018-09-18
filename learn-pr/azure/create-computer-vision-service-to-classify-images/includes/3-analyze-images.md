このユニットでは、前のステップで作成した Computer Vision API サービスを使用して画像を分析します。

# <a name="analyzing-an-image-with-computer-vision-api"></a>Computer Vision API を使用して画像を分析する

`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。 そのコマンドの出力を `key` 変数に格納します。

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、以前に宣言した `key` 変数を再利用します。

サービスに送信されるパラメーターは、`visualFeatures`、`details`、`languages` です。 これらのパラメーターがなくてもサービスは機能しますが、これらは画像の内容に関するヒントをサービスに提供します。 たとえば、`details` を `Landmarks` または `Celebrities` に設定して、ランドマークまたは著名人であることを示すことができます。 `visualFeatures` は、サービスから戻される情報の種類を示します。 `Categories` オプションは、木やビルのように、画像の内容を分類します。 `Faces` は人の顔を示し、性別と年齢が提供されます。

詳細については、[こちらのドキュメント](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa)をご覧ください。

ここでは、ランドマークを示し、サービスから `Categories` と `Description` を取得することにします。

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}"
```

要求の結果は、`url` の画像を説明する未加工の JSON です。 末尾に ` | jq '.'` を追加して、JSON の出力を修飾することもできます。

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" | jq '.'
```

オンラインで見つかった他の画像へのリンクをコピーし、上記のコマンドの URL を置き換えることで、サービスを試してみてください。