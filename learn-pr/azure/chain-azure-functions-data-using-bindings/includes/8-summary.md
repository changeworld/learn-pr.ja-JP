このモジュールは、関数へのデータとサービスの統合に関するあらゆるでした。 関数に追加するときに表示されるバインドの種類のクイック ツアーの開始。 入力バインディングを使用して Azure Cosmos DB からデータを読み取るところとするとします。 プラットフォームに任せ接続文字列を管理して、バインドを使用して、コード内のデータを読み取ることがいかに簡単かを説明しました。 最後に注目は、データのさまざまなソースと出力バインドのヘルプの記述に集中します。 この旅は、次の表に示します。

[!INCLUDE [summary table](./summary-table.md)]

ここで追加し、関数でバインドをテストする方法がアプローチを適用できます。 いくつか興味深いアイデアのバインドでもっと練習を取得して、ここで学習した内容で構築します。

* Blob ストレージから読み取りをこのモジュールで使用していない他の入力バインドには、別の関数を作成します。

* その他のサポートされている出力バインドの種類を使用して変換先の詳細に記述する別の関数を作成します。

* 最後のユニットは、キューを導入し、出力バインディングでメッセージを投稿します。 次の手順として、キュー内のメッセージを読み取り、出力を別の関数の追加を検討する、**メッセージ テキスト**がコンソールに`Console.Log()`します。

この章で説明したように、Azure portal には、関数を作成し、それらをデータやその他のサービスに接続を開始する使いやすい機能が提供されます。

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="further-reading"></a>参考資料

この操作は、網羅するものではありません、中に次に立つ興味深いと思われるこの章で説明したトピックに関連する一部のリソース。

 * [Azure Functions のドキュメント](https://docs.microsoft.com/azure/azure-functions/)

* [Azure Functions の課題](https://aka.ms/afc)

* [Azure のサーバーレス コンピューティング クックブック](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)

 * [Node.js から Queue ストレージを使用する方法](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)

 * [Azure Cosmos DB の概要: SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)

* [Azure Cosmos DB の技術概要](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)

* [Azure Cosmos DB のドキュメント](https://docs.microsoft.com/azure/cosmos-db/)
