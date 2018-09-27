Azure Cognitive Services は、アプリの強化に利用できる充実したインテリジェントなサービス スイートです。 Text Analytics API では、テキストを意味のある分析情報に変換するいくつかの操作を実行できます。 Microsoft では、このサービスを使用し、お客様からのテキスト形式のフィードバックの感情を検出しています。 それらテキスト メッセージをさらに処理するために、いくつかのバケット、あるいはキューに仕分けするため、Azure Functions でホストされるソリューションを作成しました。

REST API の呼び出し方法が分かると、これらのインテリジェント サービスをソリューションに簡単に組み込むことができます。 これらのすべてのパターンは類似しています。

- サインアップしてアクセス キーを入手する。
- API のテスト コンソールに表示する。
- アクセス キーと適切なリージョンを使用して要求を作成する。
- ソリューションからの要求を POST し、応答を解析して分析情報を得る。

私たちはこの情報を Azure Functions に作成したサーバーレス ロジックに追加しました。 これらのサービスは、他の種類のアプリからも簡単に呼び出すことができます。 すぐに取り組めるよう、数多くのクライアント ライブラリやチュートリアル、クイックスタートがあります。

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>ソリューションをさらに充実させるために

ここでは、これまでに行ったことをさらに深めたい場合に検討すべきいくつかのアイデアを紹介します。

- より多くのサンプル テキストでソリューションをテストします。 感情点数を肯定的、否定的、中立的に分類するために設定したしきい値が適切かどうかを判断します。
- [!INCLUDE [negative-q](./q-name-negative.md)] キューからメッセージを読み取り、Text Analytics API の呼び出て、テキスト内の鍵となる語句を探す関数を関数アプリに追加することを検討してください。
- この入力キューには、未加工のテキスト フィードバックが含まれます。 現実世界では、フィードバックは電子メール アドレスやアカウント番号などの何らかの形式のユーザー識別情報に関連付けられます。 入力キュー項目を強化して、ID フィールドとテキストを含む JSON ドキュメントにします。 こうして、テキスト メッセージを操作するときにその ID を利用します。
- 現在、私たちのソリューションは英語にハード コーディングされています。 Text Analytics API でサポートされているすべての言語のテキストを処理できるようにするために、実行した方がよい変更内容について考えてみましょう。
- Logic Apps を習熟している場合、「参考資料」の Text Analytics 用の組み込みのコネクタへのリンクを参照してください。

これで、Cognitive Services APIs の呼び出し方法がわかりました。他のサービスもいくつか調べてみて、それらをソリューションで利用する方法を考えてみましょう。

## <a name="further-reading"></a>参考資料

- [Text Analytics の概要](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Text Analytics のデモ](https://azure.microsoft.com/services/cognitive-services/text-analytics/)
- [Text Analytics でセンチメントを検出する方法](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Cognitive Services のドキュメント](https://docs.microsoft.com/azure/cognitive-services/)

- [Text Analytics Logic Apps コネクタ](https://docs.microsoft.com/connectors/cognitiveservicestextanalytics/)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
