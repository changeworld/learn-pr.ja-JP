絵文字の画像を Face API に渡しても、感情を取得することはできません。Face API は人間ではないからです。 そのため、絵文字ごとに代理の人間、つまり私が必要でした。

私は、各絵文字を "_正確に_" まねしながら、自分の写真を撮りました。そしてその画像の "_感情ポイント_" を絵文字の代用品として使用しました。 さらにおもしろくするために、チームからも人を選んで、次のように絵文字に関連付けました。

![チームの文字](/media-drafts/team.jpg)

目がハートになっている絵文字 (😍) には、私の妻の写真を選びました ❤️。 [スティーブン ホーキング](https://en.wikipedia.org/wiki/Stephen_Hawking)を追悼して、🤔 を表すのは彼の写真にしました。

各絵文字の代理画像の一覧は、このチュートリアルに関連付けられているサンプル コードの `bin/proxy-images` フォルダーで見ることができます。

この章では、Azure Face API を使用できるようにキーを生成してから、Face API を使用して、私の代用画像を使いながらすべての絵文字を調整します。

## <a name="generate-an-azure-face-api-key"></a>Azure Face API のキーを生成する

<!-- To make calls to the Azure Face API we will need a special authorization key.

We are going to create one using the `az` CLI. -->

Azure Face API を使用するには、特殊な認証キーが必要です。 https://azure.microsoft.com/try/cognitive-services/ に移動して、Face API の試用版にサインアップします。

![チームの文字](/media-drafts/4.calibrating-emojis.get-face-api.png)

> TODO: faceAPI を作成してキーを取得するための az コマンドを見つける

<!-- > NOTE the Azure Face API doesn't return the emotion information by default, we need to switch on this behavior by setting some query parameters, like so:
> https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion -->

## <a name="setup-the-environment-variables"></a>環境変数を設定する

調整スクリプトで正しい呼び出しを行うには、Face API の URL とキーを指定する必要があります。それらをスクリプトにハードコーディングするのではなく、環境変数を使用して、アプリケーションを実行することになるターミナルで以下のコマンドを実行します。

```bash
FACE_API_URL=<the-face-api-url>
FACE_API_KEY=<your-face-api-key>
```

<!-- > NOTE
> Don't forget to add the query param returnFaceAttributes=emotion to ensure the Face API returns emotion as well -->

## <a name="create-some-proxy-images-for-emojis"></a>絵文字のいくつかの代理画像を作成する

すべての代理画像を用意してありますが、独自に画像を生成してもかまいません。

`bin/proxy-images` フォルダー内の絵文字ごとに、絵文字をまねして自分の写真を撮り、その画像で元の画像を置き換えます。

## <a name="try-it-out"></a>試してみる

ここがおもしろいところです。 Face API を使用して `bin/proxy-images` の各画像を実行し、"_感情空間_" でのその絵文字の感情ポイントを計算します。次のように実行します。

```bash
node bin/calibrate.js
```

このコマンドの出力は、次のようになります。

```json
...
Processing 🤓
Processing 🤔
Processing 🦄
Processing 😃
Processing 😆
...
{
    emotiveValues: new EmotivePoint({
        anger: 0,
        contempt: 0.005,
        disgust: 0.001,
        fear: 0,
        happiness: 0,
        neutral: 0.922,
        sadness: 0.071,
        surprise: 0
    }),
    emojiIcon: "😴"
}
```

最初に、処理している絵文字を出力し、最後に、すべての絵文字の `EmotivePoint` を定義する配列をコンソールに出力します。 これは、`shared/mojis.ts` の配列と同じ形式です。

代理画像の一部を変更した場合は、このスクリプトの出力を `mojis.ts` の該当するセクションにコピーします。
