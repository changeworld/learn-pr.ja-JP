これについては、実装にすぎない、前にいくつかより高いレベルの質問に回答しましょう最初。

## <a name="how-to-calculate-the-emotion-of-a-face"></a>顔の感情を計算する方法でしょうか。

感情の計算では、アプリケーションの最もアクセスしやすい部分の 1 つです。 使用して、 [FaceAPI](https://azure.microsoft.com/services/cognitive-services/face/)Azure Cognitive Services ソリューションの一部であります。

イメージを返すなど、イメージ内の顔の場所の任意の顔が検出された場合と、要求された場合、イメージに関する情報を計算し、同様に、顔の感情を返すの入力として FaceAPI 受け取るようになります。

```json
{
  "anger": 0.572,
  "contempt": 0.025,
  "disgust": 0.242,
  "fear": 0.001,
  "happiness": 0.014,
  "neutral": 0.111,
  "sadness": 0.033,
  "surprise": 0.003
}
```

たとえばこのイメージを実行します。

![顔の例](/media-drafts/example-face.jpg)

このイメージを処理するには、このような API エンドポイントに POST 要求をするつもり。

https://xxxxxxxxx.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion

本文内のイメージを提供しましたようになります。

```json
{
  "url": "<path-to-image>"
}
```

> **注**
>
> 既定では、API、emotion を返さない、するは、クエリ パラメーターを明示的に指定する必要があります。 `returnFaceAttributes=emotion`

秘密キーの使用認証 APIこのキーは、ヘッダーを送信する必要があります。

```
Ocp-Apim-Subscription-Key: <your-subscription-key>
```

上記のクエリ パラメーターを使用して API は、JSON を返すようになります。

```json
[
  {
    "faceRectangle": {
      "top": 207,
      "left": 198,
      "width": 229,
      "height": 229
    },
    "faceAttributes": {
      "emotion": {
        "anger": 0.001,
        "contempt": 0.014,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.306,
        "neutral": 0.675,
        "sadness": 0.003,
        "surprise": 0.001
      }
    }
  }
]
```

イメージで検出された顔ごとに 1 つの結果の配列を返します。 顔ごととして面のサイズと位置を返します`faceRectangle`と感情の 0 からとして 1 の数値として表されます`faceAttributes`します。

> **ヒント**
>
> Cognitive Services にもいろいろを開始する_せず_には、Azure アカウントを持つ、[このページ](https://azure.microsoft.com/try/cognitive-services/?api=face-api&WT.mc_id=mojifier-sandbox-ashussai)試用版へのアクセスを取得する電子メール アドレスを入力します。

## <a name="how-to-map-an-emotion-to-an-emoji"></a>絵文字を感情をマップする方法でしょうか。

2 つだけの感情、恐怖および幸福、0 から 1 までの値があったことを想像してください。 あらゆる顔の識別を 2D にプロットする可能性がありますし、_感情的な領域_ユーザーの感情に基づくようになります。

![ユークリッド距離 1](/media-drafts/graph-1.jpg)

私たちも各の絵文字の感情的なポイントを見つけプロットも感情的な 2 次元空間では、想像してください。 文字盤の感情に最も近い絵文字を考えてこの 2D 感情的な領域に他のすべての絵文字間の距離を計算する場合、次のようにします。

![ユークリッド距離 2](/media-drafts/graph-2.png)

この計算と呼ばれる、 `euclidian distance`、使用しましたが、(怒り、軽蔑、嫌悪感、恐怖、喜び、中立、悲しみ、驚き) と 8 D 感情的なスペースに含まれないの 2D 感情的な領域です。

> **ヒント**
>
> ユークリッド距離と呼ばれる、npm パッケージを使用して容易にするように<https://www.npmjs.com/package/euclidean-distance>します。

## <a name="shared-code"></a>共有コード

サンプルのスタート プロジェクトには既に、多くの上記のユース ケースを処理するコードに共有フォルダー。

### <a name="emotivepoint"></a>EmotivePoint

よく見る場合、`EmotivePoint`クラス`shared/emmotive-point.ts`はいくつかの点に注意してください。

Emotive 情報と、ローカル メンバー変数としてストアを含むオブジェクトを入力としてコンス トラクターが次のようにします。

```typescript
 constructor({
    anger,
    contempt,
    disgust,
    fear,
    happiness,
    neutral,
    sadness,
    surprise
  }) {
    this.anger = anger;
    this.contempt = contempt;
    this.disgust = disgust;
    this.fear = fear;
    this.happiness = happiness;
    this.neutral = neutral;
    this.sadness = sadness;
    this.surprise = surprise;
  }
```

Emotive の 2 点間のユークリッド距離を計算に使用できる距離という名前の関数も次のようにします。

```typescript
  distance(other) {
    let myPoint = this.toArray();
    let otherPoint = other.toArray();
    return distance(myPoint, otherPoint);
  }
```

そのため、私たち emotive の 2 つのポイントを作成して閉じる方法は計算ようになります。

```typescript
let a = new EmotivePoint({
  /* ... */
});
let b = new EmotivePoint({
  /* ... */
});
let distance = a.distance(b);
```

### <a name="face"></a>Face

別のヘルパー クラスは、`Face`クラスなど、いくつかの異なるプロパティを組み合わせることによって、`EmotivePoint`面とも、イメージの表面を定義する四角形の使用、`Rect`するためのクラス。

よく見る場合、`Face`クラス コンス トラクターの`shared/face.ts`のコード行を確認します。

```typescript
this.moji = this.chooseMoji(this.emotivePoint);
```

`emotivePoint` emotive 自体の表面のポイントです。
`chooseMoji` 顔の emotivePoint に基づいて適切な絵文字を返します。

```typescript
  chooseMoji(point) {
    let closestMoji = null;
    let closestDistance = Number.MAX_VALUE;
    for (let moji of MOJIS) {
      let emoPoint = moji.emotiveValues;
      let distance = emoPoint.distance(point);
      if (distance < closestDistance) {
        closestMoji = moji;
        closestDistance = distance;
      }
    }
    return closestMoji;
  }
```

`MOJIS` 一覧は、すべての絵文字の emotive ポイントは、の次の講義でこれらを生成する方法を示します。

`chooseMoji`関数は、この面と、最も近いものを返すすべての絵文字間の距離を計算します。

# <a name="summary"></a>まとめ

各絵文字の特定の時点を計算します_感情的な_というこれは、スペース、_調整_し、[次へ] の章でこれについて説明します。

Azure の Face API を使用して、画像の顔ごとの感情的なポイントでの顔の一覧を取得しました。 顔ごとに最も近い絵文字を検索するのには、ユークリッド距離アルゴリズムを使用しています。
