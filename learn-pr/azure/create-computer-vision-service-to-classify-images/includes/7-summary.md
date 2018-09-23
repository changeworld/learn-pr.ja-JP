
Computer Vision API は、イメージを処理するための強力なツールです。 独自に構築する代わりにこのサービスを使用すると、重要な開発とメンテナンスのコストを節約できる可能性があります。 ビジネスに関連する意思決定を行うためのビジネス ロジックの作成に専念し、Computer Vision API がビジュアル データを分類および処理するためにイメージから豊富な情報を抽出できるようにします。

このモジュールでは、イメージを分析し、テキストを抽出して、サムネイルを生成しました。 Computer Vision API を呼び出すために、Azure サブスクリプションで Cognitive Services アカウントを作成しました。 このアカウントでは、呼び出しを行うために必要なアクセス キーが付与されました。

次の手順では、少し時間をかけて、独自のイメージを使用して同じ呼び出しを行います。 その後、呼び出しごとにパラメーターを調整し、結果に示される効果を確認します。 さらに、API 上で `describe` や `tag` などのその他の操作を試してみます。

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="further-reading"></a>参考資料

- [Azure CLI の概要](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Azure CLI コマンド リファレンス](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
- [Computer Vision API リファレンス ドキュメント](https://westus2.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fb/console)