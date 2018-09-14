Microsoft Cognitive Services は、会社のアプリを強化するために使用できるインテリジェントなサービスの豊富なスイートです。 Text Analytics API では、テキストを有用な洞察に変換するいくつかの処理があります。 サービスを使用して、お客様からのフィードバックのテキストのセンチメントを検出しました。 異なるバケットにまたはキュー、さらに処理するためにこれらのテキスト メッセージを分類する関数を Azure でホストされているソリューションを作成しました。

REST API を呼び出す方法がわかれば、ソリューションにこれらのインテリジェント サービス簡単に統合できます。 これらはすべてには、同様のパターンに従ってください。

- アクセス キーにサインアップします。
- API のテスト コンソールに表示します。
- アクセス キーと適切なリージョンを使用して要求を作成します。
- ソリューションからの要求を投稿し、洞察の応答を解析します。

この情報は、Azure Functions で作成したサーバーレス ロジックを追加しました。 簡単に他の種類のアプリからこれらのサービスを呼び出すことができます。 多くのクライアント ライブラリがあるチュートリアル、および開始するためのクイック スタート。

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>ソリューションのさらに拡張機能の提案

ここでは、いくつかのアイデアをさらにはを実行するかどうかを検討してください。

- テキストのサンプルを多く使用して、ソリューションをテストします。 正、負の場合、中立的なセンチメント スコアを分類する設定のしきい値が適切かどうかを決定します。
- 関数アプリからのメッセージを読み取るに別の関数を追加することを検討してください、[!INCLUDE [negative-q](./q-name-negative.md)]キューとテキストのキー フレーズの検索、Text Analytics API の呼び出し。
- この入力キューには、未加工のテキスト フィードバックが含まれています。 現実世界でフィードバックを何らかの形式の電子メール アドレス、アカウントの数などのユーザー ID に関連付けるはおなど。 含む JSON ドキュメントを ID フィールドとテキストの入力キュー項目を強化します。 テキスト メッセージを使用する場合は、その ID を使用します。
- 現在、ソリューションが「ハード コード化された」を英語にします。 Text Analytics API でサポートされているすべての言語でテキストを処理できるようにするために実装は、変更について考えてみます。
- Logic Apps に慣れている場合は、text analytics と関連項目」セクションの組み込みのコネクタにリンクを参照してください。

これらの Cognitive Services APIs のいずれかを呼び出す方法がわかったら、他のサービスの一部を紹介し、それらをソリューションで使用する方法について検討します。

## <a name="further-reading"></a>参考資料

- [Text Analytics の概要](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [テキスト分析でセンチメントを検出する方法](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Cognitive Services のドキュメント](https://docs.microsoft.com/azure/cognitive-services/)

- [テキスト分析のロジック アプリ コネクタ](https://docs.microsoft.com/connectors/cognitiveservicestextanalytics/)

[!include[](../../../includes/azure-sandbox-cleanup.md)]
