飲み物の配布を contoso 社でリード開発者は、構築を担当して維持できるように、第一線のディストリビューター基幹業務アプリをスキャンし、それらは在庫補充ストア シェルフのイメージをアップロードします。 

ユーザーによって投稿されたすべてのイメージが、会社で設定されているコンテンツの規則を尊重を検証するには。 会社では、会社サイトに投稿された不適切なコンテンツを望ましくありません。 Computer Vision API をアプリで使用する人工知能で継承を指定できます。 

最初に、Azure サブスクリプションに Computer Vision アカウントを作成およびテストを開始、**分析**機能します。

## <a name="calling-the-computer-vision-api-to-analyze-images"></a>画像を分析する Computer Vision API を呼び出す

`analyze`操作は、イメージのコンテンツに基づいて視覚機能の豊富なセットを展開します。  要求 URL では、次の形式があります。

`https://[location].api.cognitive.microsoft.com/vision/v2.0/analyze[?visualFeatures][&details][&language] `

サービスに送信されるパラメーターは、`visualFeatures`、`details`、`languages` です。 設定、`details`パラメーターを`Landmarks`または`Celebrities`ランドマークまたは著名人を特定するのに役立ちます。 `visualFeatures` は、サービスから戻される情報の種類を示します。 `Categories`オプションは、ツリー、建造物などのイメージのコンテンツが分類されます。 `Faces` は人の顔を示し、性別と年齢が提供されます。

Computer Vision API のすべての操作では、イメージを処理するように指示するに次の制限があります。

- サポートされているイメージ形式: JPEG、PNG、GIF、BMP。 
- イメージ ファイルのサイズは 4 MB 未満である必要があります。
- イメージのサイズは、少なくとも 50 x 50 である必要があります。

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a>画像内のランドマークを識別します。

イメージ内のランドマークの検索への呼び出しから始めましょう。 この例では、次の図を使用しますが、自由に他の画像の Url で同じコマンドを再試行してください。 

![青色空 snow-capped 山の上で、mountain 範囲の画像。](../media/3-mountains.jpg)

1. Azure Cloud Shell で次のコマンドを実行します。

<!-- TODO Replace image URL with one that points to an image in the sample repo -->
```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

この呼び出しは、イメージの URL で指定されたイメージ内のランドマークを検索します。 呼び出しでは、カテゴリ情報およびイメージの説明を返すサービスも確認します。 説明は、完全な英語文として返されます。 ご存知のように、API を呼び出すたびには、アクセス キーが必要があります。 これですべての呼び出しで設定されますが、`Ocp-Apim-Subscription-Key`ヘッダー。 

> [!TIP]
> 要求の結果は、`url` の画像を説明する未加工の JSON です。 追加しました` | jq '.'`JSON に prettify をコマンドの最後に出力します。

この例では、JSON 応答が返されます。

- A`categories`配列の 0 ~ 取って、サービスは、イメージが指定したカテゴリに属することの 1 のスコアと、検出されたすべてのイメージのカテゴリを一覧表示します。
- A`description`タグまたはイメージに関連する単語の配列を含むエントリ。
- A`captions`イメージでは英語で説明するテキスト フィールドに入力します。 テキストも確実性スコアであることを確認します。 このスコアは、この分析で次の操作を決定するのに役立ちます。


## <a name="check-for-inappropriate-content-in-an-image"></a>画像内の不適切な内容を確認します。

この例で、成人向けコンテンツ イメージをについて分析します。 信頼度スコアは、イメージに、成人向けやわいせつなコンテンツが含まれている可能性を評価します。 

この例では、次の図を使用しますが、自由に他の画像の Url で同じコマンドを再試行してください。 

![笑顔のファミリの画像。](../media/3-people.png)

1. Azure Cloud Shell で次のコマンドを実行します。

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

この例では設定、`visualFeatures`に`Adult,Description`します。 応答により、2 つの信頼スコア、わいせつなコンテンツのいずれかと成人向けコンテンツのいずれか。 これらのスコアとイメージの説明とその他のビジュアルの機能を使用して、サーバーにポストされたイメージのフラグを開始できます。

この演習で試した例が理解できると分析の種類の`analyze`操作。 さまざまなイメージを使用した分析を試す visualFeatures、詳細、および言語のパラメーターのさまざまな組み合わせをやり直してください。

詳細については、`analyze`操作を参照してください、[分析イメージ](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)リファレンス ドキュメント。