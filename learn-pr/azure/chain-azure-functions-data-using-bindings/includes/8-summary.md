このモジュールでは、関数へのデータとサービスの統合について説明しました。 バインディングの種類と、どのような場合にそれらを関数に追加するのかを説明するクイック ツアーから始めました。 次に、入力バインディングを使用して、Azure Cosmos DB からデータを読み取る方法を説明しました。 接続文字列の管理はプラットフォームによって行われるので、バインディングを使用してコード内でデータを読み取ることがいかに簡単であるかがわかりました。 最後に、出力バインディングの補助を使用して、さまざまなソースにデータを書き込む方法に注目しました。 この学習内容を次の表にまとめます。

[!INCLUDE [summary table](./summary-table.md)]

ここで学習した方法を適用して、ご利用の関数にバインディングを追加し、テストすることができます。 バインディングの使い方をさらに練習し、ここで学んだことを活かすための興味深いアイデアをいくつか紹介します。

* Blob Storage から読み取る別の関数と、このモジュールで使用しなかった他の入力バインディングを作成します。

* サポートされている他の種類の出力バインディングを使用して、より多くの送信先に書き込む別の関数を作成します。

* 前のユニットでは、キューを導入し、出力バインディングを使用してキューにメッセージを送信しました。 次のステップとして、キュー内のメッセージを読み取り、`Console.Log()` を使用して**メッセージ テキスト**をコンソールに出力する別の関数を追加することを検討してください。

このモジュールで説明したように、Azure portal には、関数を作成し、それらをデータや他のサービスに接続する作業を開始するための使いやすい機能が用意されています。

ビジュアル ワークフローを使用して、そしてカスタム コードはほとんどまたはまったく使用しないで、このようなサーバーレス統合を行うことに関心がある場合は、Azure Logic Apps もご確認ください。

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>その他のリソース

このリストはすべての資料を網羅するものではありません｡以下はこのモジュールで取り上げられ､ユーザーに有用と思われるトピックに関係しているリソースです｡

* [Azure Functions のドキュメント](https://docs.microsoft.com/azure/azure-functions/)
* [Azure Functions の課題](https://aka.ms/afc)
* [Azure のサーバーレス コンピューティング クックブック](https://azure.microsoft.com/resources/azure-serverless-computing-cookbook/)
* [Node.js から Queue ストレージを使用する方法](https://docs.microsoft.com/azure/storage/queues/storage-nodejs-how-to-use-queues)
* [Azure Cosmos DB の概要: SQL API](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
* [Azure Cosmos DB の技術概要](https://azure.microsoft.com/blog/a-technical-overview-of-azure-cosmos-db/)
* [Azure Cosmos DB のドキュメント](https://docs.microsoft.com/azure/cosmos-db/)
