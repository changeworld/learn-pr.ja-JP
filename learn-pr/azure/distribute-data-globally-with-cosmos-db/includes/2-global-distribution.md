オンラインの衣料品サイトで顧客に製品への迅速なアクセスを提供することは、その顧客にとっても、事業の成功にとっても極めて重要です。 データがユーザーに届くまでに移動する距離を短くすることで、コンテンツの配信をより高速化できます。 データが Azure Cosmos DB に格納されている場合、ポイント アンド クリック操作で世界中の複数のリージョンにサイトのデータがレプリケートされます。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

このユニットでは、グローバル配布と、ネイティブのマルチ マスター データベース サービスの利点について理解し、3 つの追加のリージョンにアカウントをレプリケートします。

## <a name="global-distribution-basics"></a>グローバル配布の基本

グローバル配布を使用すると、1 つのリージョンから複数の Azure リージョンにデータをレプリケートできます。 データベースがレプリケートされるリージョンは、いつでも追加または削除でき、Azure Cosmos DB では追加のリージョンが付け加えられた場合に、データが 100 TB 以下であることを前提とし、30 分以内にそのデータを利用可能にすることを保証しています。

複数のリージョンにデータをレプリケートする場合、一般的なシナリオとして次の 2 つがあります。

1. エンド ユーザーが世界中のどこにいても、短い待機時間でのデータ アクセスを実現する
2. ビジネス継続性とディザスター リカバリー (BCDR) のためにリージョンの回復性を追加する

顧客に短い待機時間でのアクセスを提供するために、ユーザーの所在地に最も近いリージョンにデータをレプリケートすることをお勧めします。 オンラインで衣料品を扱う会社で、ロサンゼルス、ニューヨーク、および東京に顧客がいるとします。 [Azure リージョン](https://azure.microsoft.com/global-infrastructure/regions/)のページを確認して、これらの顧客に最も近いリージョンを特定します。それらのリージョンが、ユーザーをレプリケートする場所になります。

BCDR ソリューションを提供するには、「[ビジネス継続性とディザスター リカバリー (BCDR): Azure のペアになっているリージョン](https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/)」に記載されているリージョン ペアに基づいてリージョンを追加することをお勧めします。

データベースがレプリケートされるときに、スループットとストレージも同等にレプリケートされます。 そのため、元のデータベースが 10 GB のストレージと 1,000 RU/秒のスループットを備えており、3 つの追加のリージョンにそのデータベースをレプリケートした場合、各リージョンは 10 GB のデータと 1,000 RU/秒のスループットを備えることになります。 ストレージとスループットが各リージョンにレプリケートされるので、リージョンへのレプリケート コストは、元のリージョンと同じになります。そのため、3 つの追加のリージョンにレプリケートした場合、レプリケートされていない元のデータベースの約 4 倍のコストが生じます。

## <a name="creating-an-azure-cosmos-db-account-in-the-portal"></a>ポータルで Azure Cosmos DB アカウントを作成する

1. サンドボックスのアクティブ化に使用したものと同じアカウントを使って、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。

    > [!IMPORTANT]
    > 同じアカウントを使って、Azure portal とサンドボックスにログインします。
    >
    > サンドボックスに確実に接続されるように、上記のリンクを使用して Azure portal にログインします。コンシェルジェ サブスクリプションへのアクセスが提供されます。

1. **[リソースの作成]** > **[データベース]** > **[Azure Cosmos DB]** の順にクリックします。

   ![Azure Portal の [データベース] ウィンドウ](../media/2-global-distribution/2-create-nosql-db-databases-json-tutorial.png)

1. **[Azure Cosmos DB アカウントを作成します]** ページで、場所などの新しい Azure Cosmos DB アカウントの設定を入力します。

    <!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    設定|値|説明
    ---|---|---
    サブスクリプション|*コンシェルジェ サブスクリプション*|使用しているコンシェルジェ サブスクリプションを選択します。 コンシェルジェ サブスクリプションが一覧に表示されない場合は、サブスクリプション上で複数のテナントを有効な状態にすると共に、テナントを変更する必要があります。 そのためには、ポータル リンク ([サンドボックスの Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true)) を使用して、もう一度ログインします。
    リソース グループ|既存のものを使用<br><br><rgn>[サンドボックス リソース グループ名]</rgn>|**[既存のものを使用]** を選択して、<rgn>[サンドボックス リソース グループ名]</rgn> を入力します。
    アカウント名|*一意の名前を入力*|この Azure Cosmos DB アカウントを識別するための一意の名前を入力します。 指定した ID に *documents.azure.com* が付加されて URI が作成されるので、ID は一意であっても識別可能なものを使用してください。<br><br>ID に含めることができるのは英小文字、数字、ハイフン (-) のみ、3 文字以上で 31 文字以内にする必要があります。
    API|SQL|API によって、作成するアカウントの種類が決まります。 Azure Cosmos DB には、アプリケーションのニーズに応じて、SQL (ドキュメント データベース)、Gremlin (グラフ データベース)、MongoDB (ドキュメント データベース)、Azure Table、および Cassandra の 5 つの API が用意されています。現時点では、それぞれ別個のアカウントが必要です。 <br><br>このモジュールでは、SQL 構文を使ってクエリを実行して SQL API でアクセスできるドキュメント データベースを作成しているので、**[SQL]** を選びます。|
    場所|<*最も近いリージョンを選択*>|上記のリージョンの一覧から最も近いリージョンを選択します。
    geo 冗長| 無効化 | この設定によって、データベースのレプリケート バージョンが 2 番目 (ペア) のリージョンに作成されます。 後でデータベースをレプリケートするので、ここでは、この設定を無効のままにしておきます。
    マルチリージョン ライター | 有効化 | この設定によって、同時に複数のリージョンに書き込むことが可能になります。 この設定は、アカウントの作成時にのみ構成できます。

1. **[確認および作成]** をクリックします。

    ![Azure Cosmos DB の新しいアカウント ページ](../media/2-global-distribution/2-azure-cosmos-db-create-new-account.png)

1. 設定の確認を終えたら、**[作成]** をクリックしてアカウントを作成します。

1. アカウントの作成には数分かかります。 ポータルにデプロイ成功の通知が表示されるまで待ち、その通知をクリックします。

    ![通知アラート](../media/2-global-distribution/2-azure-cosmos-db-notification.png)

1. 通知ウィンドウで、**[リソースに移動]** をクリックします。

    ![リソースに移動](../media/2-global-distribution/2-azure-cosmos-db-go-to-resource.png)

    ポータルに、"**ご利用ありがとうございます。Azure Cosmos DB アカウントが作成されました**" ページが表示されるまで待機します。

    ![Azure portal の [通知] ウィンドウ](../media/2-global-distribution/2-azure-cosmos-db-account-created.png)

## <a name="replicate-data-in-multiple-regions"></a>複数のリージョン内にデータをレプリケートする

ロスアンゼルス、ニューヨーク、および東京のグローバルなユーザーの最も近くにデータベースをレプリケートしましょう。

1. アカウントのページで、メニューから **[データをグローバルにレプリケートする]** をクリックします。
1. **[データをグローバルにレプリケートする]** ページで、[米国西部 2]、[米国東部]、および [東日本] のリージョンを選択して、**[保存]** をクリックします。

    Azure portal にマップが表示されていない場合は、画面の左側のメニューを最小化して、マップを表示します。

    データが新しいリージョンに書き込まれている間、ページには **[更新しています]** のメッセージが表示されます。 新しいリージョンのデータは、30 分以内に利用可能になります。

    ![マップ内でリージョンをクリックして追加する](../media/2-global-distribution/2-global-replication.gif)

## <a name="summary"></a>まとめ

このユニットでは、世界各地のユーザーが最も集中しているリージョンにデータベースをレプリケートして、サイト上のデータに短い待機時間でアクセスできるようにしました。
