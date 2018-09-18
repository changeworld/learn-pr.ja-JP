Microsoft Cognitive Services は、アプリの強化に利用できる充実したインテリジェントなサービス スイートです。 Text Analytics API サービスのごく一部を利用して､テキストの高次の情報を調べてみました｡ お客様からのテキスト形式のフィードバックを感情面から分析しました。 それらテキスト メッセージをさらに処理するために､いくつかのバケット､あるいはキューに仕分けするため､Azure Functions でホストされるソリューションを作成しました。

REST API の呼び出し方法が分かると､これらのインテリジェント サービスをソリューションに簡単に組み込むことができます｡ こうしたことのすべてが類似のパターンに従っています｡

- サインアップしてアクセス キーを入手する。
- API のテスト コンソールに表示する。
- アクセス キーと適切なリージョンを使用して要求を作成する。
- ソリューションからのリクエストを POST し、応答を解析して洞察を得る｡

私たちはこの情報を Azure Functions に作成したサーバーレス ロジックに追加しました。 これらのサービスは､他の種類のアプリからも簡単に呼び出すことができます。 すぐに取り組めるよう､数多くのクライアント ライブラリやチュートリアル、クイックスタートがあります｡

## <a name="suggestions-for-further-enhancement-of-our-solution"></a>ソリューションをさらに充実させるために

ここでは、これまでに行ったことをさらに深めたい場合に検討すべきいくつかのアイデアを紹介します｡ 

- もっと多くのテキスト例でソリューションをテストし､感情点数を肯定的､否定的､中立的に分類するために設定したしきい値が適切かどうかを判断してください｡ 
- [!INCLUDE [negative-q](./q-name-negative.md)] キューからメッセージを読み取り､Text Analytics API の呼び出て､テキスト内の鍵となる語句を探す関数を関数アプリに追加することを検討してください｡
- この入力キューには、未加工のテキスト フィードバックが含まれます。 現実世界では､フィードバックは電子メール アドレスやアカウント番号などの何らかの形式のユーザー識別情報に関連付けられます｡ このため、入力キュー項目を強化して､ID フィールドとテキストを含む JSON ドキュメントにします｡ こうして､テキスト メッセージを操作するときはその ID を利用します。
 - 現在、私たちのソリューションは英語にハード コーディングされています｡ Text Analytics API でサポートされているすべての言語のテキストを処理できるようにするためには､どのような変更を行えばよいのか考えてみてくだます。  

これらの Cognitive Services API の呼び出し方法が分かったわけですから、他のサービスもいくつか検討し､それらをソリューションで利用する方法を考えてください｡ 

## <a name="further-reading"></a>参考資料

- [Text Analytics の概要](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview)
- [Text Analytics で感情を検出する方法](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis)
- [Cognitive Services のドキュメント](https://docs.microsoft.com/azure/cognitive-services/)

## <a name="clean-up-resources"></a>リソースのクリーンアップ

Azure の *リソース*とは、Function App や関数、ストレージ アカウントなどのことを意味します。 これらは*リソース グループ*に分類されており、グループを削除することでグループ内のすべてのものを削除できます。

あなたは､このモジュールを終了するためにリソースを作成しました｡ [アカウントの状態](https://azure.microsoft.com/account/)と[サービスの価格](https://azure.microsoft.com/pricing/)によっては､それらリソースは課金されてることがあります｡ リソースの必要がなくなった場合にそれらを削除する方法を、次に示します。

1. Azure Portal で、**[リソース グループ]** ページに移動します。

   Function App ページからこのページに移動するには、**[概要]** タブを選択してから、**[リソース グループ]** の下にあるリンクを選択します。

   ダッシュボードからこのページに移動するには、**[リソース グループ]** を選択してから、このモジュールで使用したリソース グループを選択します。 

> [!NOTE]
> このモジュールで推奨された既定のリソース グループ名は [!INCLUDE [resource-group-name](./rg-name.md)] でしたが､別の名前を使用することもできます。

2. **[リソース グループ]** ページで、含まれているリソースの一覧を確認し、削除してもよいリソースであることを確認します。

3. **[リソース グループの削除]** を選択し、指示に従います。

   削除には数分かかることがあります。 実行されると、通知が数秒間表示されます。 ページの上部にあるベルのアイコンを選択することで、通知を表示することもできます。

## <a name="further-reading"></a>参考資料

このリストはすべての資料を網羅するものではありません｡以下はこのモジュールで取り上げられ､ユーザーに有用と思われるトピックに関係しているリソースです｡

 * [Azure Functions のドキュメント](https://docs.microsoft.com/azure/azure-functions/)

* [Azure Functions の課題](https://aka.ms/afc)

* [Azure のサーバーレス コンピューティング クックブック](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [Node.js から Queue ストレージを使用する方法](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [Azure Cosmos DB の概要: SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [Azure Cosmos DB の技術概要](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [Azure Cosmos DB のドキュメント](https://docs.microsoft.com/azure/cosmos-db/)
