このユニットでは、前に作成した Computer Vision API サービスを使用してソース画像からサムネイルを生成します。

# <a name="generate-a-thumbnail-from-an-image-with-computer-vision-api"></a>Computer Vision API で画像からサムネイルを生成する

`az cognitiveservices account keys list` コマンドを実行して、API に対する認証に使用されるキーを取得します。 そのコマンドの出力を `key` 変数に格納します。

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

`curl` コマンドを実行して、Computer Vision API に対して HTTP 要求を行い、前に宣言した `key` 変数を再利用します。

異なるパラメーターを API に提供し、ニーズに応じた適切なサムネイルを生成できます。 `width` と `height` は必須であり、特定の画像に必要なサイズを API に通知します。 最後に、`smartCropping` パラメーターを指定すると、画像内の関心領域を分析してサムネイル内にそれらを維持することで、よりスマートなトリミングが生成されます。 たとえば、スマート トリミングを有効にすると、画像が要求したものと同じ比率ではない場合でも、トリミングされたプロフィール画像では、画像フレーム内の人の顔が保たれます。

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/mountains.jpg\"}" -o clouddrive/thumbnail.jpg
```

# <a name="downloading-the-thumbnail"></a>サムネイルのダウンロード

生成されたサムネイルは、`cloud-shell-storage-<region>` という名前のリソース グループ内の Cloud Shell ストレージ アカウントにあります。

1. 自動的に生成されたストレージ アカウントに移動します

![画像](../images/storage-account.png)

2. ファイル セクションをクリックします

![画像](../images/storage-account-click-on-files.png)

3. コンテナーのルートにサムネイルが表示されます。

![画像](../images/storage-account-thumbnail.png)

4. ファイルをクリックしてダウンロードします

ダウンロード フォルダーから、画像ビューアーで `100x100` ピクセルの画像を開くことができます。