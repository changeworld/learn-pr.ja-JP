あなたの会社は、拡大する顧客および製品ベースの需要に合わせて Azure Cosmos DB を選択しました。 あなたにはデータベースを作成する仕事が課せられました。

最初の手順では、Azure Cosmos DB アカウントを作成します。

## <a name="what-is-an-azure-cosmos-db-account"></a>Azure Cosmos DB アカウントとは

Azure Cosmos DB アカウントは、データベースの組織エンティティとして機能する Azure リソースです。 課金の目的で、使用状況が Azure サブスクリプションに接続されています。

各 Azure Cosmos DB アカウントは、Azure Cosmos DB がサポートするさまざまなデータ モデルの 1 つに関連付けられており、アカウントを必要な数だけ作成できます。 

SQL API は、新しいアプリケーションを作成する場合の優先データ モデルです。 グラフやテーブルを扱う場合や、MongoDB または Cassandra のデータを Azure に移行する場合は、追加のアカウントを作成して関連するデータ モデルを選択します。

アカウントを作成する場合は、自分のアカウントを識別できるよう、わかりやすい ID を選択します。 また、データ センターとユーザーの間の待ち時間を最小限に抑えるために、ユーザーに最も近い Azure リージョンでアカウントを作成します。

必要に応じて、アカウントの作成時に仮想ネットワークと geo 冗長性を設定できますが、この設定は後で行うこともできます。 このモジュールでは、これらの設定は有効にしません。

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>ポータルで Azure Cosmos DB アカウントを作成する

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[[Azure portal for Sandbox]\(Azure portal for Sandbox\)](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。

    > [!IMPORTANT]
    > サンドボックスに確実に接続されるように、上記のリンクを使用して Azure portal にログインします。コンシェルジェ サブスクリプションへのアクセスが提供されます。

1. **[リソースの作成]** > **[データベース]** > **[Azure Cosmos DB]** の順にクリックします。
   
   ![Azure Portal の [データベース] ウィンドウ](../media/2-create-nosql-db-databases-json-tutorial.png)

1. **[Azure Cosmos DB アカウントを作成します]** ページで、場所などの新しい Azure Cosmos DB アカウントの設定を入力します。

    [!INCLUDE[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]
 
    設定|値|説明
    ---|---|---
    サブスクリプション|*コンシェルジェ サブスクリプション*|コンシェルジェ サブスクリプションを選択します。 コンシェルジェ サブスクリプションが一覧に表示されない場合は、サブスクリプション上で複数のテナントを有効な状態にすると共に、テナントを変更する必要があります。 これを行うには、ポータル リンク [サンドボックスへの Azure Portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) を使用して、もう一度ログインします。
    リソース グループ|既存のものを使用<br><br>**<rgn>[サンドボックス リソース グループ名]</rgn>**|ここで、新しいリソース グループを作成するか、お使いのサブスクリプションにある既存のリソース グループを選択します。 
    アカウント名|*一意の名前を入力*|この Azure Cosmos DB アカウントを識別するための一意の名前を入力します。 指定した ID に *documents.azure.com* が付加されて URI が作成されるので、ID は一意であっても識別可能なものを使用してください。<br><br>ID に含めることができるのは英小文字、数字、ハイフン (-) のみ、3 文字以上で 50 文字以内にする必要があります。
    API|SQL|API によって、作成するアカウントの種類が決まります。 Azure Cosmos DB には、アプリケーションのニーズに応じて、SQL (ドキュメント データベース)、Gremlin (グラフ データベース)、MongoDB (ドキュメント データベース)、Azure Table、および Cassandra の 5 つの API が用意されています。現時点では、それぞれ別個のアカウントが必要です。 <br><br>このモジュールでは、SQL 構文を使ってクエリを実行して SQL API でアクセスできるドキュメント データベースを作成しているので、**[SQL]** を選びます。|
    場所|*上記の一覧から最も近いリージョンを選択*|データベースを置く場所を選択します。
    geo 冗長| 無効化 | この設定によって、2 番目 (ペア) のリージョンでデータベースが複製されます。 データベースは後で複製できるので、ここでは空白のままにします。
    マルチ マスター | 無効化 | この設定によって、同時に複数のリージョンに書き込むことが可能になります。 この設定は、アカウントの作成時のみ構成できます。 今回、このユニットでは、これを無効のままにします。
    仮想ネットワーク|空白のまま|ここでは、仮想ネットワークを空白のままにします。 これは、後で構成できます。

1. **[Review + Create]\(レビュー + 作成\)** をクリックします。

    ![Azure Cosmos DB の新しいアカウント ページ](../media/2-azure-cosmos-db-create-new-account.png)

1. 設定の確認を終えたら、**[作成]** をクリックしてアカウントを作成します。 

1. アカウントの作成には数分かかります。 ポータルにデプロイ成功の通知が表示されるまで待ち、その通知をクリックします。 

    ![通知アラート](../media/2-azure-cosmos-db-notification.png)

1. 通知ウィンドウで、**[リソースに移動]** をクリックします。

    ![リソースに移動](../media/2-azure-cosmos-db-go-to-resource.png)

    ポータルに、"**ご利用ありがとうございます。Azure Cosmos DB アカウントが作成されました**" ページが表示されるまで待機します。

    ![Azure portal の [通知] ウィンドウ](../media/2-azure-cosmos-db-account-created.png)

## <a name="summary"></a>まとめ

Azure Cosmos DB データベースの作成時の最初の手順として、Azure Cosmos DB アカウントを作成しました。 データ型に対して適切な設定を選択し、ユーザーの待ち時間が最小限に抑えられるようアカウントの場所を設定しました。
