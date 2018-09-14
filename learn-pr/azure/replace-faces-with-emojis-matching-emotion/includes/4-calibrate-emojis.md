絵文字の画像の感情を取得ためなどは人間ではないために Face API に渡すことはできません。 人間のプロキシでは、各の絵文字の必要ようにします。

自分の写真を撮った_正確に_各の絵文字を模倣し、使用、_感情的なポイント_の絵文字をプロキシとしてそのイメージ。 また私のチームからユーザーを選択して、それらに関連付けられている絵文字も法令遵守するようになります。

![チームの文字](/media-drafts/team.jpg)

各絵文字のプロキシのイメージの一覧を表示できます、`bin/proxy-images`フォルダーに関連付けられたこのチュートリアルのサンプル コード。

## <a name="goal"></a>目標

この章では、Azure の Face API を使用して自分のプロキシのイメージを使用してすべての絵文字を調整する Face API を使用するために必要な認証キーを生成します。

## <a name="learning-objectives"></a>学習の目的

- API を生成する方法は、Cognitive Services で使用するキーします。
- Face API を介してイメージを実行し、感情の情報を抽出する方法。

## <a name="generate-an-azure-face-api-key"></a>Azure の Face API キーを生成します。

Azure の Face API を使用するには、認証キー必要があります。

キーを取得する最も簡単な方法は、することですに https://azure.microsoft.com/try/cognitive-services/?api=face-apiFace API の試用版にサインアップします。

1 回をサインアップの後で格納する必要がある情報のいくつかのビットが提供されます。

1. 取得、_エンドポイント_します。 ように表示されます。 https://westcentralus.api.cognitive.microsoft.com/face/v1.0

2. (問題ありませんが 1 つ) 後で使用するストアの 1 つ、2 つの API キーを表示します。

## <a name="setup-the-environment-variables"></a>環境変数を設定します。

ハード コーディング環境変数を使用して、ターミナルで次のコマンドを実行するスクリプトでこれらアプリケーションを実行する予定するのではなく、調整のスクリプトは、正しい呼び出しを実行するには、Face API URL とキーを知る必要があります。

```bash
export FACE_API_URL=<the-face-api-url>
export FACE_API_KEY=<your-face-api-key>
```

という名前のパッケージを使用しても`dotenv`ノード アプリケーションでします。 このパッケージみましょうストアを使用する環境変数ローカルという名前のファイルで`.env`します。 `dotenv`パッケージは、このファイル内の変数を読み込むし、に、アプリケーションを環境変数として提示します。

> **注**
>
> チェックインしない`.env`ファイルをソース管理にします。

Azure Functions を使用して環境変数を処理する別の方法がある、`local.settings.json`ファイルの詳細は後述します。

## <a name="create-some-proxy-images-for-emojis"></a>絵文字をいくつかプロキシ イメージを作成します。

自身が自由に独自の生成、プロキシのすべてのイメージを提供しています。

各絵文字での`bin/proxy-images`フォルダー、その絵文字を模倣して自分の写真を撮るし、イメージ、イメージを置き換えます。

## <a name="try-it-out"></a>試してみる

おもしろいの一部です。 内のイメージのそれぞれを実行しようとして、`bin/proxy-images`を計算する感情的なポイントでは、その絵文字を Face API を通じて_感情的な領域_を実行。

```bash
node bin/calibrate.js
```

このコマンドの出力は何かになりますようになります。

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

まず、絵文字の処理とし、最後コンソールに出力を定義する配列を出力、`EmotivePoint`のすべての絵文字。 これは、配列と同じ形式`shared/mojis.ts`します。

プロキシのイメージの一部を変更した場合の関連するセクションにこのスクリプトの出力をコピーし、 `mojis.ts`
