Azure Storage の利点を調べると、ラーニング ポータルを格納するための最適なオプションが提供されることがわかります。 ここでは、Azure Storage で利用できる利点とオプションを詳しく調べて、ビジネス ニーズにどのように適合するかを見てみましょう。

## <a name="how-azure-storage-can-meet-your-business-storage-needs"></a>Azure Storage がビジネス ストレージ ニーズを満たす方法

Azure Storage では、特定の種類のデータ ストレージ ニーズに対応する複数のオプションが提供されています。

### <a name="azure-sql-database"></a>Azure SQL Database

**Azure SQL Database** は、すべてのデータが格納される堅牢なフル マネージド リレーショナル クラウド データベースです。 この機能を使用すると、スタッフの個人情報やトレーニングに関する情報など、頻繁にアクセスおよび更新するデータを格納できます。 また、アプリケーションを変更することなく、既存の SQL Server データベースを移行することもできます。

![AzureSQL](../media-draft/Azure_SQL.png)

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB は、グローバル分散データベース サービスです。 スキーマのないデータをサポートし、絶えず変化するデータをサポートするための応答性に優れた *Always On* アプリケーションを構築する機能を提供します。 この機能を使用して、世界中のユーザーからに入力に基づいて更新および保守されるデータを格納できます。

![Cosmos DB](../media-draft/Azure_cosmos_db.png)

### <a name="azure-blob-storage"></a>Azure BLOB ストレージ

Azure Blob Storage では、大きな動画ファイルや音声ファイルを世界中の任意の場所からユーザーのブラウザーに直接ストリーミングすることができます。 Blob Storage は、バックアップと復元、ディザスター リカバリー、アーカイブのためのデータの格納にも使用されます。 Azure Blob Storage には、仮想マシン用のファイルのデータを最大 8 TB まで格納できます。

![AzureBlob](../media-draft/Azure_blob.png)

### <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

Azure Storage の Data Lake 機能を使用すると、データの使用状況を分析し、それに応じてレポートを準備できます。 Data Lake は、構造化データと非構造化データの両方を格納する大規模なリポジトリです。

**Azure Data Lake Storage Gen2** では、オブジェクト ストレージのスケーラビリティおよびコスト メリットと、ビッグ データ ファイル システム機能の信頼性とパフォーマンスが結合されています。

![Data_lake_Store_concept](../media-draft/Data_lake_store_concept.png)

### <a name="azure-files"></a>Azure Files

Azure Files では、クラウドでのフル マネージド ファイル共有が提供されます。 Azure で実行されているアプリケーションでは、VM 間でファイルを簡単に共有できます。 Windows、Linux、macOS のクラウドまたはオンプレミスの展開に対して同時に Azure ファイル共有を使用できます。

![Azure_Files](../media-draft/Azure_Files.png)

### <a name="azure-queue"></a>Azure Queue

Azure Queue Storage は、世界中のどこからでもアクセスできる大量のメッセージを格納するためのサービスです。 1 つのキュー メッセージに許容されるサイズは最大 64 KB であり、1 つのキューに数百万件のメッセージを格納することができます。

Queue Storage は主に次の目的で使用します。

- 作業のバックログを作成し、異なる Azure Web サーバー間でメッセージを受け渡すため。
- 異なる Web サーバー/インフラストラクチャの間で負荷を分散し、トラフィックの急激な増加を管理するため。
- 複数のユーザーがデータに同時にアクセスしたときのコンポーネントの障害に対する回復力を構築するため。

![Azure_Queue](../media-draft/Azure_Queue.png)

### <a name="azure-standard-storage"></a>Azure Standard Storage

Azure の仮想マシンでは、オペレーティング システム、アプリケーション、およびデータを格納するためにディスクが使用されます。 Azure Standard Storage では、ミッション クリティカルではないワークロードを実行する VM 向けに、信頼性の高い低コストのディスク サポートが提供されます。 Standard Storage では、データはハード ディスク ドライブ (HDD) に格納されます。

VM を使用するとき、重要度の低いワークロードには Standard SSD および HDD ディスクを使用し、ミッション クリティカルな運用アプリケーションには Premium SSD ディスクを使用できます。 エンタープライズレベルの持続性を、業界トップレベルの年間故障率 0% で一貫して提供してきました。

![Azure_disk](../media-draft/Azure_disks.png)

### <a name="storage-tiers"></a>ストレージ層

Azure Storage では、BLOB オブジェクト ストレージ用に 3 つのストレージ層が提供されています。

1. **ホット ストレージ層** - Azure ホット ストレージ層は、頻繁にアクセスされるデータの格納に適しています。 
1. **クール ストレージ層** - Azure クール ストレージ層は、アクセスされる頻度は低いものの、少なくとも 30 日以上保管されるデータの格納に適しています。
1. **アーカイブ ストレージ層** - Azure アーカイブ ストレージ層は、ほとんどアクセスされず、少なくとも 180 日以上保管され、待ち時間の要件が柔軟であるデータの格納に適しています。 Azure のアーカイブ ストレージは、データの古いバージョンを保存するのに最適です。これにより、監査やその他の頻繁にないアクティビティで必要になった場合にデータを取得できます。

![Archive_Tier](../media-draft/Archive_Storage_Tier.png)

### <a name="azure-storage-encryptionreplication"></a>Azure Storage の暗号化/レプリケーション

Azure Storage では、暗号化機能とレプリケーション機能によって、データのセキュリティと高可用性が提供されます。

#### <a name="encryption-for-storage-services"></a>ストレージ サービスの暗号化

リソースには次の種類の暗号化を使用できます。

1. 保存データ用の **Azure Storage Service Encryption (SSE)** は、データをセキュリティで保護して組織のセキュリティと規制コンプライアンスを満たすのに役立ちます。 Azure SSE では、格納前にデータが暗号化され、取得前に復号化されます。 暗号化と復号化をユーザーが認識することはありません。
1. **クライアント側暗号化**では、データはクライアント ライブラリによって既に暗号化されています。 保存時には暗号化された状態でデータが格納され、取得の間に復号化されます。

    この暗号化機能により、データがグローバルな保護標準を満たすことが保証されます。 個人データや金融データなどの機密情報を格納するのに適しています。

#### <a name="replication-for-storage-availability"></a>ストレージの可用性のためのレプリケーション

ストレージ アカウントを作成するときに、レプリケーションの種類を設定します。 レプリケーション機能により、データが永続的で常に使用できることが保証されます。 Azure Storage によりリージョン レプリケーションと地理レプリケーションが可能になり、自然災害および火災や洪水などの他の地域災害からデータが保護されます。
