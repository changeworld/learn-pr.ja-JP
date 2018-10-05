Azure データ ストレージの利点を調べると、ラーニング ポータルを格納するための最適なオプションが提供されることがわかります。 ここでは、利点とオプションを詳しく調べて、ご自分のビジネス ニーズにどのように適合するかを見てみましょう。

## <a name="how-azure-data-storage-can-meet-your-business-storage-needs"></a>Azure データ ストレージでビジネス ストレージ ニーズをどのように満たせるか

Azure では、特定の種類のデータ ストレージ ニーズに対応するストレージ オプションがいくつか提供されます。 その一部を簡単に見てみましょう。

:::row:::
  :::column:::
    ![Azure SQL Database](../media/3-azure-sql-db.png)
  :::column-end:::
    :::column span="3":::
**Azure SQL Database**

**Azure SQL Database** は、堅牢なフル マネージド リレーショナル クラウド データベースです。 この機能を使用すると、スタッフの個人情報やトレーニングに関する情報など、頻繁にアクセスおよび更新するデータを格納できます。 また、アプリケーションを変更することなく、既存の SQL Server データベースを移行することもできます。 Azure SQL データベースに格納されるオンライン ラーニング ポータル シナリオからのデータの種類を、次の図に示します。

![成績証明書、認定資格、学習教材など、生徒の情報を格納するために使用する Azure SQL を示す図。](../media/3-Azure_SQL.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Cosmos DB](../media/3-cosmos-db.png)
  :::column-end:::
    :::column span="3":::
**Azure Cosmos DB**

Azure Cosmos DB は、グローバル分散型データベース サービスです。 このサービスでは、スキーマのないデータがサポートされ、絶えず変化するデータをサポートするための応答性に優れた **Always On** アプリケーションを構築することができます。 この機能を使用して、世界中のユーザーによって更新および保守されるデータを格納することができます。 世界中のユーザーによってアクセスされるデータを格納するために使用される Azure Cosmos DB データベースのサンプルを次の図に示します。

![オンライン トレーニング シナリオにおいてコースのカタログを格納するための Azure Cosmos DB の使用状況を示す図。 カタログは管理者によって更新され、世界中の生徒からアクセスされるため、ここでは Azure Cosmos DB が適切な選択となります。](../media/3-Azure_cosmos_db.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Blob Storage](../media/3-azure-blob-storage.png)
  :::column-end:::
    :::column span="3":::
**Azure BLOB Storage**

Azure Blob Storage では、大きな動画ファイルや音声ファイルを世界中の任意の場所からユーザーのブラウザーに直接ストリーミングすることができます。 Blob Storage は、バックアップと復元、ディザスター リカバリー、アーカイブのためのデータの格納にも使用されます。 仮想マシンには最大 8 TB のデータを格納することができます。 Azure Blob Storage の使用例を次の図に示します。

![ビデオまたはオーディオ ファイルの格納とストリームのために使用する Azure BLOB ストレージを示す図。](../media/3-Azure_blob.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Data Lake Storage Gen2](../media/3-azure-data-lake.png)
  :::column-end:::
    :::column span="3":::
**Azure Data Lake Storage Gen2**

Data Lake 機能を使用すると、データの使用状況を分析し、レポートを準備することができます。 Data Lake は、構造化データと非構造化データの両方を格納する大規模なリポジトリです。

**Azure Data Lake Storage Gen2** では、オブジェクト ストレージのスケーラビリティおよびコストに関する利点と、ビッグ データ ファイル システム機能の信頼性とパフォーマンスが結合されています。 Azure Data Lake によってどのようにすべてのビジネス データが格納され、分析のために使用可能になるのかを次の図に示します。

![分析ツールで使用するためにデータを準備して格納する Azure Data Lake の役割を示す図。 Azure Data Lake では、リレーショナル データ、ビデオ データ、センサー データなど、さまざまな種類の入力を処理できます。](../media/3-Data_lake_store_concept.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Files](../media/3-azure-files.png)
  :::column-end:::
    :::column span="3":::
**Azure Files**

Azure Files では、クラウドでのフル マネージド ファイル共有が提供されます。 Azure で実行されているアプリケーションでは、VM 間でファイルを簡単に共有できます。 Windows、Linux、macOS のクラウドまたはオンプレミスの展開に対して同時に Azure ファイル共有を使用できます。 2 つの地理的な場所間でデータを共有するために使用されている Azure Files を、次の図に示します。 Azure Files では、保存時と転送中のデータの暗号化が保証されるサーバー メッセージ ブロック (SMB) プロトコルが使用されます。

![Azure Files のファイル共有機能を示す図。 ](../media/3-Azure_Files.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Queue](../media/3-azure-queue.png)
  :::column-end:::
    :::column span="3":::
**Azure Queue**

Azure Queue Storage は、世界中のどこからでもアクセスできる大量のメッセージを格納するためのサービスです。 大局的に見ると、単一のキュー メッセージのサイズは最大 64 KB であり、キューには数百万件のメッセージを格納することができます。

通常は、1 つ以上の送信側コンポーネントと 1 つ以上の受信側コンポーネントがあります。 送信側コンポーネントではメッセージをキューに追加しますが、受信側コンポーネントではキューの先頭から処理対象のメッセージを取得します。 次の図は、メッセージを Azure Queue に追加する複数の送信側アプリケーションと、メッセージを取得する 1 つの受信側アプリケーションを示しています。

![Azure Queue Storage のアーキテクチャの概要を示す図](../media/3-Azure_Queue.png)

Queue Storage を使用して、次のことを行うことができます。

- 作業のバックログを作成し、異なる Azure Web サーバー間でメッセージを受け渡す。
- 異なる Web サーバー/インフラストラクチャの間で負荷を分散し、トラフィックの急激な増加を管理する。
- 複数のユーザーがデータに同時にアクセスしたときのコンポーネントの障害に対する回復力を構築する。

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Azure Standard Storage](../media/3-azure-standard-storage.png)
  :::column-end:::
    :::column span="3":::
**Azure Standard Storage**

Azure の仮想マシンでは、オペレーティング システム、アプリケーション、およびデータを格納するためにディスクが使用されます。 Azure Standard Storage では、ミッション クリティカルではないワークロードを実行する VM 向けに、信頼性の高い低コストのディスク サポートが提供されます。 Standard Storage では、データはハード ディスク ドライブ (HDD) に格納されます。

VM を操作するときに、重要度の低いワークロードには Standard SSD および HDD ディスクを使用し、ミッション クリティカルな運用アプリケーションには Premium SSD ディスクを使用できます。 Azure ディスクはエンタープライズレベルの持続性を、業界トップレベルの年間故障率 0% で一貫して提供してきました。 個別のディスクを使用してさまざまなデータを格納する Azure 仮想マシンを次の図に示します。

![仮想マシン内の、オペレーティング システムを格納するディスクとデータを格納するディスクの 2 つのディスクを示す図。](../media/3-Azure_disks.png)

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![ストレージ層](../media/3-storage-tiers.png)
  :::column-end:::
    :::column span="3":::
**ストレージ層**

Azure では、BLOB オブジェクト ストレージ用に 3 つのストレージ層が提供されています。

1. **ホット ストレージ層**: 頻繁にアクセスされるデータを格納するために最適化されています。

1. **クール ストレージ層**: アクセスされる頻度は低いものの、30 日以上格納されるデータ用に最適化されています。

1. **アーカイブ ストレージ層**: ほとんどアクセスされず、180 日以上格納され、待ち時間の要件が柔軟であるデータ用です。

:::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![暗号化とレプリケーション](../media/3-azure-storage-encryption.png)
  :::column-end:::
    :::column span="3":::
**暗号化とレプリケーション**

Azure では、暗号化機能とレプリケーション機能によって、データのセキュリティと高可用性が提供されます。

#### <a name="encryption-for-storage-services"></a>ストレージ サービスの暗号化

ご利用のリソースには次の種類の暗号化を使用できます。

1. 保存データ用の **Azure Storage Service Encryption (SSE)** は、データをセキュリティで保護して組織のセキュリティと規制コンプライアンスを満たすのに役立ちます。 データは格納前に暗号化され、取得前に復号化されます。 暗号化と復号化はユーザーに対して透過的です。

1. **クライアント側暗号化**では、データはクライアント ライブラリによって既に暗号化されています。 Azure では保存時に暗号化された状態でデータが格納され、取得の間に復号化されます。

#### <a name="replication-for-storage-availability"></a>ストレージの可用性のためのレプリケーション

ストレージ アカウントを作成するときに、レプリケーションの種類を設定します。 レプリケーション機能により、データが永続的で常に使用できることが保証されます。 Azure ではリージョン レプリケーションと地理レプリケーションが提供され、自然災害および火災や洪水などの他の局地災害からデータが保護されます。

  :::column-end:::
:::row-end:::