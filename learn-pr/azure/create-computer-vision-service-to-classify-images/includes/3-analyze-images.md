あなたは Contoso Beverage Distribution 社の開発主任として、現場の販売業者が在庫補充のために店舗の棚をスキャンし、その画像をアップロードするための基幹業務アプリを構築、保守する仕事を担当しています。 

ユーザーの投稿画像が会社の設定したコンテンツ規則に従っていることを確認する必要があります。 会社は、会社のサイトに不適切なコンテンツが投稿されることを望んでいません。 人工知能の進歩を踏まえ、あなたは、アプリの中で Computer Vision API を使用することに決めました。 

最初に、ご自分の Azure サブスクリプションで Computer Vision アカウントを作成し、**分析** 機能のテストを開始します。

## <a name="calling-the-computer-vision-api-to-analyze-images"></a>イメージを分析する Computer Vision API を呼び出す

`analyze`操作では、イメージの内容に基づいて、さまざまな視覚的特徴のセットが抽出されます。 要求 URL の形式は次のようになります。

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=<...>&details=<...>&language=<...>`

サービスに送信されるパラメーターは、`visualFeatures`、`details`、`languages` です。 `details` パラメーターを `Landmarks` または `Celebrities` に設定すると、ランドマークまたは著名人の識別に役立ちます。 `visualFeatures` は、サービスから戻される情報の種類が示されます。 `Categories` オプションは、木やビルのように、画像の内容を分類します。 `Faces` では人の顔が識別され、性別と年齢が提供されます。

Computer Vision API に対してイメージの処理を求める場合は、すべての操作において次の制限があります。

- サポートされているイメージ形式: JPEG、PNG、GIF、BMP。 
- イメージ ファイル サイズは 4 MB 未満である必要があります。
- イメージの寸法は 50 x 50 以上である必要があります。

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="identify-landmarks-in-an-image"></a>画像内のランドマークを識別する

イメージ内のランドマークを検索するための呼び出しから始めましょう。 この例では、次のイメージを使用しますが、他のイメージへの URL を指定して同じコマンドを自由に試すことができます。 

![雪を頂いた山々の上に青空が広がっている山脈の画像。](../media/3-mountains.jpg)

Azure Cloud Shell で次のコマンドを実行します。 コマンドの `<region>` を、実際の ＠Cognitive Services アカウントのリージョンに置き換えます。

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Categories,Description&details=Landmarks" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/mountains.jpg'}" \
| jq '.'
```

- この呼び出しでは、イメージ URL で指定されたイメージ内のランドマークが検索されます。 分析しているイメージは、この演習では GitHub リポジトリに格納されます。 
- この呼び出しではまた、カテゴリ情報およびイメージに関する説明を返すようにサービスに対して要求が出されます。 説明は完全な英語の文で返されます。 
- ご存知のように、API のすべての呼び出しにアクセス キーが必要です。 これは、要求の `Ocp-Apim-Subscription-Key` ヘッダーに設定されています。 

> [!TIP]
> 要求の結果は、`url` 内の画像を説明する未加工の JSON です。 コマンドの末尾に ` | jq '.'` を追加して、JSON の出力を修飾しました。

この呼び出しからの JSON 応答では、次の内容が返されます。

- 検出されたすべてのイメージ カテゴリを一覧表示する `categories` 配列と、イメージが指定されたカテゴリに属していることがサービスによってどの程度確信されているかを示す 0 から 1 のスコア。
- イメージに関連するタグまたは単語の配列を含む `description` エントリ。
- `captions` エントリと、イメージ内に含まれているものを英語で説明したテキスト フィールド。 テキストには信頼度スコアも含まれていることを確認してください。 このスコアは、この分析を使用して次に何を行うかを決めるのに役立ちます。


## <a name="check-for-inappropriate-content-in-an-image"></a>イメージ内に不適切な内容が含まれていないか確認する

この例では、成人向けコンテンツのイメージを分析します。 信頼度スコアでは、成人向けコンテンツまたはわいせつなコンテンツのいずれかがイメージに含まれている可能性が評価されます。 

この例では、次のイメージを使用しますが、他のイメージへの URL を指定して同じコマンドを自由に試すことができます。 

![笑顔の家族の画像。](../media/3-people.png)

1. URL 内の `<region>` を置き換えて、Azure Cloud Shell で次のコマンドを実行します。

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/analyze?visualFeatures=Adult,Description" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/people.png'}" \
| jq '.'
```

この例では、`visualFeatures` を `Adult,Description` に設定します。 

応答では、2 つの信頼度スコアが返されます。1 つはわいせつなコンテンツに対するもので、もう 1 つは成人向けコンテンツに対するものです。 これらのスコアに加えて、イメージの説明およびその他の視覚的特徴を使用することで、ご利用のサーバーにポストされるイメージへのフラグ付けを開始することができます。

この演習で試みた例では、`analyze` 操作を使用して実行できる分析の種類について説明しました。 さまざまなイメージに対して分析を試してみてください。また、visualFeatures、details、languages の各パラメーターのさまざまな組み合わせも試してみてください。

`analyze` 操作の詳細については、「[Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)」(イメージの分析) リファレンス ドキュメントを参照してください。