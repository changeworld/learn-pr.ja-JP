一部のアプリケーションでは、データ ストレージに対する要求が他よりも厳しくなります。 Dynamics CRM、Exchange Server、SAP Business Suite、SQL Server、Oracle、SharePoint などのアプリでは、最適な状態で実行するために常に高いパフォーマンスと低遅延が必要です。

VM を作成したり新しいディスクを追加したりするときに、ディスクのパフォーマンスに大きな影響を与える選択項目がいくつかあり、その最初のものが選択するストレージの "_種類_" です。

## <a name="types-of-disks"></a>ディスクの種類 
Azure ディスクは、99.999% の可用性で設計されています。 

ディスクを作成するときに選択できるストレージのパフォーマンス レベルは、Premium SSD ディスク、Standard HDD ストレージ、Standard SSD の 3 つです。 VM のサイズに応じて、これらのディスクの種類を組み合わせることができます。

### <a name="premium-ssd-disks"></a>Premium SSD ディスク 

Premium SSD ディスクでは、ソリッドステート ドライブ (SSD) を使用して、I/O 集中型のワークロードを実行する VM 向けの高パフォーマンスで待ち時間の少ないディスク サポートを提供します。 これらのドライブは可動部分がないため、信頼性が高い傾向があります。 要求されたデータを見つけるために、読み取りまたは書き込みヘッドが、ディスク上の正しい場所に移動するがありません。 

Premium SSD ディスクは、シリーズ名に "s" が含まれる VM サイズで使用できます。 たとえば、**Dv3 シリーズ**と **Dsv3 シリーズ**がある場合、**Dsv3 シリーズ**で Premium SSD ディスクを使用できます。

### <a name="standard-hdd-storage"></a>Standard HDD ストレージ

Standard HDD ディスクは、従来のハード ディスク ドライブ (HDD) を利用します。 Standard HDD ディスクは Premium ディスクより低いレートで課金されます。 Standard HDD ディスクは、すべての VM サイズで使用できます。

### <a name="standard-ssd"></a>Standard SSD

Premium ストレージは特定の VM サイズに制限されるので、作成する VM の種類が、ストレージの機能 (サイズ、最大容量、ストレージの種類) に影響します。 VM はローエンドでも、I/O のパフォーマンスのために SSD ストレージが必要な場合はどうしますか。 その場合は Standard SSD を利用できます。 Standard SSD は、パフォーマンスとコストの点で、Standard HDD と Premium SSD の間に位置します。

Standard SSD は、Premium Storage をサポートしていない VM サイズを含め、すべての VM サイズで使用できます。 これらの VM で SSD を使用する唯一の方法は、Standard SSD を使用することです。 このディスクの種類は、特定のリージョンでのみ、"_マネージド ディスク_" にだけ使用できます。

### <a name="unmanaged-vs-managed-disks"></a>アンマネージド ディスクとマネージド ディスク

VM や VHD を作成するときは、**アンマネージド** ディスクまたは**マネージド** ディスクのどちらを使うかを選択できます。

アンマネージド ディスクを使用する場合、VM ディスクに対応する VHD を保持するために使用するストレージ アカウントを自分で管理する必要があります。 使用するスペースの容量に対応したストレージ アカウント料金を支払うことになります。 1 つのストレージ アカウントには、I/O 操作数 20,000/秒という固定レート制限があります。これは、1 つのストレージ アカウントが最大スロットルで 40 個の標準仮想ハード ディスクをサポートできることを意味します。 スケールアウトを行う必要がある場合は、複数のストレージ アカウントが必要となります。これは複雑な状況をもたらす場合があります。

マネージド ディスクはより新しく、**推奨されるディスク ストレージ モデル**です。 Azure にストレージ アカウント管理の負荷を担わせることにより、この複雑性を適切に解決します。 ユーザーはディスクの種類とディスクのサイズを指定し、ディスクおよびディスク用ストレージの "_両方_" の作成と管理は Azure によって行われます。 ストレージ アカウントの制限について心配する必要はなく、これによりスケールアウトがより容易なものとなります。古いアンマネージド ディスクに比べて次のようないくつかの利点があります。

- **信頼性の向上**: Azure では、高信頼性の VM に関連付けられた VHD が Azure ストレージのさまざまな部分に配置されて、同等のレベルの回復力が提供されます。
- **セキュリティの向上**: マネージド ディスクは、リソース グループ内の管理対象リソースです。 つまり、ロールベースのアクセス制御を使用して、VHD データを使用できるユーザーを制限することができます。
- **スナップショットサポート**： VHD の読み取り専用コピーの作成にスナップショットが利用可能です。 所有している VM をシャットダウンする必要がありますが、スナップショットの作成に要する時間はわずか数秒です。 完了したら、VM の電源を入れ、スナップショットを使用して重複する VM を作成し、運用環境の問題をトラブルシューティングしたり、スナップショットが作成された時点に VM をロールバックしたりできます。
- **バックアップのサポート**: Azure Backup を使用して、ディザスター リカバリーのために、マネージド ディスクを異なるリージョンに自動的にバックアップできます。VM のサービスに影響はありません。

パフォーマンス特性の保証など、他のすべての利点を考えると、新しい VM では常にマネージド ディスクを選択すべきです。

### <a name="disk-comparison"></a>ディスクの比較
次の表では、Standard HDD、Standard SSD、および Premium SSD を比較しており、どちらを使用するかを決定するのに役立ちます。

|    | Azure Premium ディスク |Azure Standard SSD ディスク (プレビュー)| Azure Standard HDD ディスク 
|--- | ------------------ | ------------------------------- | ----------------------- 
| **ディスクの種類** | ソリッド ステート ドライブ (SSD) | ソリッド ステート ドライブ (SSD) | ハード ディスク ドライブ (HDD)  
| **概要**  | I/O 集中型のワークロードを実行している VM またはミッション クリティカルな運用環境をホストしている VM 向けの、SSD ベースの高パフォーマンスで待ち時間の少ないディスク サポート |一貫したパフォーマンスと信頼性が HDD よりも優れている。 低 IOPS ワークロード用に最適化| 不定期に起こるアクセス用の HDD ベースのコスト効果の高いディスク
| **シナリオ**  | 運用環境のワークロードやパフォーマンスに影響されやすいワークロード |Web サーバー、あまり使用されていないエンタープライズ アプリケーション、および開発/テスト| バックアップ、重要ではない、不定期に起こるアクセス 
| **ディスク サイズ** | 32 - 4095 GiB | 128 - 4095 GiB | 32 - 4095 GiB 
| **ディスクあたりの最大スループット** | 250 MiB/秒 | 最大 60 MiB/秒 | 最大 60 MiB/秒 
| **ディスクあたりの最大 IOPS** | 7,500 IOPS | 最大 500 IOPS | 最大 500 IOPS 

ディスクのパフォーマンスについては以下でさらに詳しく説明します。

## <a name="data-replication"></a>データのレプリケーション

Microsoft Azure ストレージ アカウント内のデータは、持続性と高可用性を保証するため、常にレプリケートされています。 Azure Storage のレプリケーションでは、計画されたイベントや計画外のイベント (一時的なハードウェア障害、ネットワークの停止または停電、大規模な自然災害など) からデータが保護されるようにデータがコピーされます。 同じデータ センター内、同じリージョンのゾーンのデータ センター間、さらにはリージョン間でデータをレプリケートすることもできます。 レプリケーションには、次の 4 種類があります。

- **ローカル冗長ストレージ (LRS)** - 同じ Azure データ センター内でデータがレプリケートされます。 ノードで障害が発生しても、データは引き続き使用できます。 ただし、データ センター全体で障害が発生すると、データが使用できなくなる場合があります。
- **geo 冗長ストレージ (GRS)** - プライマリ リージョンから数百マイル離れた第 2 のリージョンにデータがレプリケートされます。 お使いのストレージ アカウントで GRS が有効になっている場合、地域的な停電やプライマリ リージョンが復旧できない災害が発生しても、データは保持されます。
- **読み取りアクセス geo 冗長ストレージ (RA-GRS)** - セカンダリ ロケーションにあるデータに対する読み取り専用アクセス、および 2 つのリージョンにまたがる geo レプリケーションが提供されます。 データ センターで障害が発生しても、データは引き続き読み取り可能ですが、変更できなくなります。
- **ゾーン冗長ストレージ (ZRS)** - 1 つのリージョン内の 3 つのストレージ クラスターにデータが同期的にレプリケートされます。 各ストレージ クラスターは物理的に他のストレージ クラスターと分離されており、それぞれの可用性ゾーン (AZ) 内にあります。 この種類のレプリケーションでは、ゾーンが使用できなくなっても、お使いのデータには引き続きアクセスし、管理することができます。

Standard Storage アカウントでは、すべてのレプリケーションの種類がサポートされますが、Premium Storage アカウントでサポートされるのはローカル冗長ストレージ (LRS) のみです。 VM 自体が 1 つのリージョンで実行されているため、通常、この制限が VHD ストレージにとって問題になることはありません。

## <a name="disk-performance"></a>ディスクのパフォーマンス

ディスクのパフォーマンスは、選択したディスクの種類によって異なります。 各ディスクは、1 秒あたりの I/O 操作数つまり IOPS ("アイオプス" と発音します) で評価されます。 さらに、各ドライブにはスループットの評価があり、これにより 1 秒間に読み取りまたは書き込みできるデータの量が決まります。 これら 2 つの組み合わせで、ディスクの速度が決まります。

Standard ストレージでは、ディスクあたりのスループットの最大値は **500 IOPS と 60 MB/秒**です (SSD でも)。 Premium ストレージでは、IOPS は選択した Premium ディスクと VM のサイズによって決まります。

|  | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
|--|----|----|-----|-----|-----|-----|-----|
| **ディスク サイズ** | 32 GiB | 64 GiB | 128 GiB | 512 GiB | 1 TiB | 2 TiB | 4 TiB |
| **ディスクあたりの最大 IOPS** | 120 | 240 | 500 | 2300 | 5000 | 7500 | 7500 |
| **ディスクあたりの最大スループット** | 25 MB/秒 | 50 MB/秒 | 100 MB/秒 | 200 MB/秒 | 250 MB/秒 | 250 MB/秒 |

ご覧のように、**25 MB/秒**と **120 IOPS** から、**250 MB/秒**と **7500 IOPS** までの範囲になります。