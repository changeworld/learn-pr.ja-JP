製品の現場の販売業者は自分が補充している店の棚の画像をスキャンし、アップロードします。 会社の開発主任であるあなたは、その画像のサムネイル作成を担当します。 営業チームのために作成するオンライン レポートでそのサムネイルが使用されます。 最近、レポートの画像がぼやけている、製品が正面中央から撮影されていないことが多い、大きなレポートをスキャンするのが難しいと営業マネージャーに言われました。 この状況はあなたが改善しなければなりません。

Computer Vision API のサムネイル生成機能を試すことにします。 自分で作成したサイズ変更機能よりはおそらく良い仕事をするでしょう。

Computer Vision では最初に高品質のサムネイルを生成し、画像内のオブジェクトを分析して関心領域 (ROI) を決定します。 Computer Vision では次に、関心領域の要件に合わせて画像がトリミングされます。 生成されたサムネイルは、必要に応じて、元の画像とは異なる縦横比で表示できます。 実際の動作を見てみましょう。

## <a name="calling-the-computer-vision-api-to-generate-a-thumbnail"></a>サムネイルを生成する Computer Vision API を呼び出す

`generateThumbnail` 操作により、ユーザーが指定した幅と高さでサムネイル画像が作成されます。 このサービスでは既定で、画像が分析され、関心領域 (ROI) が特定され、ROI に基づいてスマート トリミング座標が生成されます。 スマート トリミングは、入力画像とは異なる縦横比を指定するときに便利です。 要求 URL の形式は次のようになります。

`https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=<...>&height=<...>&smartCropping=<...>`

異なるパラメーターを API に与え、求められるサムネイルを生成できます。 `width` パラメーターと `height` パラメーターは必須です。 この 2 つのパラメーターにより、特定の画像に必要なサイズが API に伝えられます。 `smartCropping` パラメーターにより、画像内の関心領域を分析してサムネイル内に維持することで、よりスマートにトリミングされます。 たとえば、スマート トリミングを有効にすると、画像の縦横比が異なるとき、トリミングされたプロフィール画像では、人の顔が写真のフレーム内に収まります。

[!INCLUDE [get-key-note](./get-key.md)]

## <a name="generate-a-thumbnail"></a>サムネイルの生成

この例では次の画像を使用しますが、同じコマンドを試すとき、他の画像の URL を自由に指定できます。 

![緑の芝生に白いかわいい犬が座っている写真。](../media/4-dog.png)

1. Azure Cloud Shell で次のコマンドを実行します。 コマンド内の `<region>` を、使用する Cognitive Services アカウントのリージョンに置き換える

```azurecli
curl "https://<region>.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail?width=100&height=100&smartCropping=true" \
-H "Ocp-Apim-Subscription-Key: $key" \
-H "Content-Type: application/json" \
-d "{'url' : 'https://raw.githubusercontent.com/MicrosoftDocs/mslearn-process-images-with-the-computer-vision-service/master/images/dog.png'}" \
-o  thumbnail.jpg
```

この例では、100x100 のサムネイルを作成するようにサービスに指示します。 スマート トリミングが有効になっています。 正しい応答にはサムネイル画像のバイナリが含まれます。thumbnail.jpg と呼ばれているファイルにそれを書き込みます。

> [!CAUTION]
> `-o` パラメーターにより、出力がファイルにリダイレクトされます。 このファイルは常に上書きされます。そのため、このコマンドを試す間、複数のサムネイルを手元に置くならファイル名を変更してください。

## <a name="view-the-generated-thumbnail"></a>生成されたサムネイルを表示する

生成されたサムネイルは、Azure Cloud Shell ストレージ アカウントに置かれます。 このファイルには **thumbnail.jpg** という名前を付けました。 

Microsoft Learn の Cloud Shell にはファイルをダウンロードする機能はありませんが、Azure portal からサムネイルをダウンロードするには、この手順に従ってください。

1. **thumbnail.jpg** ファイルがホーム フォルダーに置かれていることを確認するには、Azure Cloud Shell で次のコマンドを実行します。

    ```azurecli
    cd ~
    ls -l
    ```

    

1. 次のコマンドを実行して、`thumbnail.jpg` を clouddrive フォルダーに移動します。

    ```azurecli
    mv ~/thumbnail.jpg ~/clouddrive
    ```
1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。
1. ポータル ダッシュ ボードの **[すべてのリソース]** パネルで、 名前が `cloudshell` で始まるストレージ アカウントを選択します。 
1. [ストレージ アカウント] パネルで、**[Storage Explorer]**、**[FILE SHARES]** を選択し、名前が **cloudshellfiles*** で始まるそのコレクション内の共有ファイルを選択します。
1. *thunbnail.jpg* ファイルを選択し、上部メニューから**ダウンロード**して画像を参照します。

`generateThumbnail` 操作は高性能なサムネイル生成機能であり、サムネイルで画像の関心領域 (ROI) を維持できます。

`generateThumbnails` 操作の詳細については、「[Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb)」 (サムネイルの取得) リファレンス ドキュメントを参照してください。