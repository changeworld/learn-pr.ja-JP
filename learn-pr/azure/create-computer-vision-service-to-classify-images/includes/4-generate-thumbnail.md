お客様の製品の第一線のディストリビューターでは、スキャンし、在庫補充いるストア シェルフのイメージをアップロードします。 社内で開発のリーダー、としては、画像のサムネイルを作成します。 サムネイルは、営業チームのオンラインに作成するレポートで使用されます。 最近では、営業マネージャー、レポート内のイメージがぼやけてと言って、製品の前面とセンターがない多くの場合、サイズの大きなレポートをスキャンするが難しくなります。 状況を改善するためです。

Computer Vision API のサムネイル生成機能をお試しください。 対象とするとします。 おそらく、サイズ変更関数を記述するよりも優れたジョブを実行できます。

Computer Vision では、最初に、高品質なサムネイルを生成し、し (ROI) の関心領域を判断する画像内のオブジェクトを分析します。 Computer Vision ではその後、関心領域の要件に合わせて、イメージがトリミングされます。 ユーザーのニーズに応じて、元のイメージの縦横比とは異なる縦横比を使用して、生成されたサムネイルを表示することができます。 実際の動作を見てみましょう。

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a>サムネイルを生成する Computer Vision API を呼び出す

`generateThumbnail`操作は、ユーザーが指定した幅と高さでサムネイル画像を作成します。 既定では、サービスは、イメージを分析し、目的 (ROI) の領域を識別、ROI に基づく、スマート トリミング座標が生成されます。 スマート トリミング入力画像の縦横比とは異なる縦横比を指定する場合に役立ちます。 要求 URL では、次の形式があります。

`https://[location].api.cognitive.microsoft.com/vision/v2.0/generateThumbnail[?width][&height][&smartCropping]`

異なるパラメーターを API に提供し、ニーズに応じた適切なサムネイルを生成できます。 `width`と`height`パラメーターが必要です。 特定のイメージに必要なサイズ、API に通知します。 `smartCropping`サムネイル内に保つために、イメージの関心領域を分析することでよりスマートなトリミング パラメーターが生成されます。 例として、スマート トリミングを有効になっている、トリミング プロファイル画像が状態に保つ他のユーザーの顔、画像のフレーム内でも、イメージがさまざまな側面の比率。

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a>サムネイルの生成

この例では、次の図を使用しますが、自由に他の画像の Url で同じコマンドを再試行してください。 

![芝が緑で座ってかわいらしい white dog の画像。](../media/4-dog.png)

1. Azure Cloud Shell で次のコマンドを実行します。

```azurecli
curl "https://westus2.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

この例では、100 x 100 のサムネイルを作成するサービスお願いします。 スマート トリミングが有効になっているとします。 正常な応答には、thumbnail.jpg という名前のファイルに記述するサムネイル イメージ、バイナリが含まれています。  

> [!CAUTION]
> `-o`パラメーターは、ファイルに出力をリダイレクトします。 ファイルが常に上書きされます、中、このコマンドは、複数のサムネイルを保持する場合のファイル名を変更するようにします。

## <a name="downloading-the-thumbnail"></a>サムネイルのダウンロード

生成したサムネイルは、Azure Cloud Shell のストレージ アカウントに記載されています。 ファイルという名前を付けて**thumbnail.jpg**します。 

1. ファイルを確認するための Azure Cloud Shell で次のコマンドを実行**thumbnail.jg**ホーム フォルダーに存在します。

```azurecli
cd ~
ls -l
```
2. Cloud Shell] メニューで、次のように選択します。**ファイルのアップロード/ダウンロード**、し、[**ダウンロード**ドロップダウン メニューから。

3. **ファイルをダウンロードする**ダイアログが表示されたら、入力**thumbnail.jpg**クリックし、必須フィールドに**ダウンロード**します。 ローカル コンピューターにこのイメージのダウンロードを開始します。

4. ダウンロードしたイメージを検索する**thumbnail.jpg**の downloads フォルダー。 作成されたサムネイルを表示するには、お気に入りイメージ ビューアーでは、イメージを開きます。

`generateThumbnail`操作は、サムネイルで、リージョンの目的 (ROI) のイメージを維持することのできる強力なサムネイル ジェネレーター。 

詳細については、`generateThumbnails`操作を参照してください、[サムネイルの取得](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)リファレンス ドキュメント。