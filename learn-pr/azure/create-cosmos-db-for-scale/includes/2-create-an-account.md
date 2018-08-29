あなたの会社は、拡大する顧客および製品ベースの需要に合わせて Azure Cosmos DB を選択しました。 あなたにはデータベースを作成する仕事が課せられました。

最初の手順では、Azure Cosmos DB アカウントを作成します。 

## <a name="what-is-an-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントとは

Azure Cosmos DB アカウントは、データベースの組織エンティティとして機能する Azure リソースです。 課金の目的で、使用状況が Azure サブスクリプションに接続されています。

各 Azure Cosmos DB アカウントは、Azure Cosmos DB がサポートするいくつかのデータ モデルの 1 つに関連付けられており、アカウントを必要な数だけ作成できます。 

新しいアプリケーションを作成する場合は、SQL API が優先データ モデルとなります。 グラフやテーブルを使用するか、または MongoDB や Cassandra のデータを Azure に移行する場合は、追加のアカウントを作成し、関連するデータ モデルを選択します。

アカウントを作成する場合は、自分のアカウントを識別できるよう、わかりやすい ID を選択します。 また、データ センターとユーザーの間の待機時間を最小限に抑えるために、ユーザーに最も近い Azure リージョンでアカウントを作成します。

必要に応じて、アカウントの作成時に仮想ネットワークと geo 冗長性を設定できますが、この設定は後で行うこともできます。 このモジュールでは、これらの設定は有効にしません。

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>ポータルで Azure Cosmos DB アカウントを作成する

<!--TODO: Update portal link with one that routes to free Learning acct-->
1. [Azure Portal](https://portal.azure.com/) にサインインします。
2. **[リソースの作成]** > **[データベース]** > **[Azure Cosmos DB]** の順にクリックします。
   
   ![Azure Portal の [データベース] ウィンドウ](../media/1-introduction/create-nosql-db-databases-json-tutorial-1.png)

3. **[新しいアカウント]** ページで、新しい Azure Cosmos DB アカウントの設定を入力します。
 
    Setting|値|説明
    ---|---|---
    ID|<*一意の名前を入力*>|この Azure Cosmos DB アカウントを識別するための一意の名前を入力します。 指定した ID に *documents.azure.com* が付加されて URI が作成されるので、ID は一意であっても識別可能なものを使用してください。<br><br>ID に含めることができるのは英小文字、数字、ハイフン (-) のみ、3 文字以上で 50 文字以内にする必要があります。
    API|SQL|API によって、作成するアカウントの種類が決まります。 Azure Cosmos DB には、アプリケーションのニーズに応じて、SQL (ドキュメント データベース)、Gremlin (グラフ データベース)、MongoDB (ドキュメント データベース)、Azure Table、および Cassandra の 5 つの API が用意されています。現時点では、それぞれ別個のアカウントが必要です。 <br><br>このモジュールでは、SQL 構文を使ってクエリを実行し、SQL API でアクセスできるドキュメント データベースを作成しているので、**[SQL]** を選びます。|
    サブスクリプション|*該当するサブスクリプション*|この Azure Cosmos DB アカウントに使用する Azure サブスクリプションを選択します。 
    リソース グループ|新規作成<br><br>*上記の ID で指定したものと同じ一意の名前を入力*|**[新規作成]** を選択し、アカウントの新しいリソース グループ名を入力します。 簡略化のため、ID と同じ名前を使用できます。 
    Location|<*ユーザーに最も近いリージョンを選択*>|Azure Cosmos DB アカウントをホストする地理的な場所を選択します。 データに最も高速にアクセスできる、ユーザーに最も近い場所を使用します。
    Geo 冗長の有効化| 空白 | この設定では、データベースのレプリケート バージョンが 2 番目 (ペア) のリージョンに作成されます。 データベースは後でレプリケートできるので、ここでは空白のままにします。 
    仮想ネットワーク|Disabled|ここでは、仮想ネットワークを無効のままにします。 これは後で有効にすることができます。 

4. **Create** をクリックしてください。

    ![Azure Cosmos DB の新しいアカウント ページ](../media/1-introduction/azure-cosmos-db-create-new-account.png)

5. アカウントの作成には数分かかります。 ポータルで、デプロイ成功の通知が表示されるまで待ってから、通知をクリックします。 

    ![通知アラート](../media/1-introduction/azure-cosmos-db-notification.png)

6. 通知ウィンドウで、**[リソースに移動]** をクリックします。

    ![リソースに移動](../media/1-introduction/azure-cosmos-db-go-to-resource.png)

    ポータルに、"**ご利用ありがとうございます。Azure Cosmos DB アカウントが作成されました**" ページが表示されるまで待機します。

    ![Azure Portal の [通知] ウィンドウ](../media/1-introduction/azure-cosmos-db-account-created.png)

## <a name="summary"></a>まとめ

Azure Cosmos DB データベースの作成時の最初の手順として、Azure Cosmos DB アカウントを作成しました。 データ型に対して適切な設定を選択し、ユーザーの待機時間が最小限に抑えられるようアカウントの場所を設定しました。
