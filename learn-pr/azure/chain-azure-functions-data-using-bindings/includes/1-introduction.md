プロフェッショナル向けのソーシャル ネットワーク サイトを実行するとします。 ユーザーが自分の顔写真画像をアップロードしてプロファイルに投稿できるようにします。 Web サーバー上のワークロードを減らすために、Azure Functions を使用してこのデータを処理するサーバーレス バックエンドを作成します。 画像のサムネイルを作成し、永続ストレージに保存します。 

Azure Functions の能力は、主に、さまざまなデータ ソースやサービスを提供する統合によってもたらされます。 これらの統合はバインディングによって定義されます。 バインディングを使用することで、開発者は関数との間のデータ フローを気にせずに、他のデータ ソースやサービスとやりとりできます。

## <a name="learning-objectives"></a>学習の目的

このモジュールでは、次のことを行います。

- バインディングを使用してアクセスできるデータ ソースの種類を確認する
- Azure Functions を使用して Azure Cosmos DB からデータを読み取る
- Azure Functions を使用して Azure Cosmos DB にデータを格納する
- Azure Functions を使用して Azure Queue Storage にメッセージを送信する
