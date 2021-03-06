インフラストラクチャとしてのサービス (IaaS) とは、インターネット経由でプロビジョニングと管理を行うインスタント コンピューティング インフラストラクチャです。 IaaS では、需要に応じてリソースをすばやくスケーリングし、使用した分だけ支払うことができます。 IaaS を使用すると、物理サーバーとその他のデータ センター インフラストラクチャを独自に購入して管理するための費用と複雑さを回避できます。 各リソースは、個別のサービス コンポーネントとして提供され、必要な期間だけリソースを*レンタル*します。 したがって、IaaS には柔軟性があります。 VM、ストレージ、仮想サブネット、ファイアウォール、VPN などの一般的なインフラストラクチャをプロビジョニングして、ソリューションをビルドできます。 物理サーバーおよびアプライアンスを管理する必要はありません。 しかし、コンポーネントの構成と管理はご自身で行う必要があります。 たとえば、ファイアウォールの構成、VM の OS の更新、DBMS の更新、ランタイムなどが含まれます。

### <a name="common-scenarios"></a>一般的なシナリオ 

たとえば、あなたの勤務先の医療関連企業でデスクトップ ソフトウェアの特別なバージョンを実行する必要があるとしましょう。 ソフトウェアは、オペレーティング システムの特定のバージョンでしかサポートされず、1 つのユーザー ライセンスしか必要ありません。 あなたは必要なソフトウェアを使用して仮想マシンを作成することができます。 ユーザーは、リモート デスクトップ接続を使用して仮想マシンに接続して、ソフトウェアを使用することができます。

別のシナリオを考えてみましょう。 開発チームは、いくつかの固有の開発環境を必要としています。 チームは開発サイクルを通して、製品のさまざまなバージョンをテストする必要があります。 開発者は、必要なときに環境をプロビジョニングできます。 環境が不要になったら、簡単に削除できます。

その他の一般的なシナリオには、次のようなものがあります。

**Web サイトのホスティング:** Web サイトのホストの制御を強化するには、従来の Web ホスティングよりも、IaaS を使用して Web サイトを実行することをお勧めします。

**Web アプリ:** IaaS では、ストレージ、Web サーバーとアプリケーション サーバー、ネットワーク リソースなど、Web アプリをサポートするためのすべてのインフラストラクチャが提供されます。 組織では、IaaS に Web アプリをすばやくデプロイすることができ、アプリの需要が予測できない場合にインフラストラクチャのスケールアップやスケールダウンを簡単に行うことができます。

**ストレージ、バックアップ、および回復:** ストレージ管理は、データを管理し、法的要件およびコンプライアンス要件を満たすための大規模な資本投資と熟練のスタッフが必要となるため、複雑になる可能性があります。 IaaS は、予想不可能な需要、および着実に増加するストレージ ニーズの計画と管理を簡素化するのに役立ちます。

**ハイ パフォーマンス コンピューティング:** ハイ パフォーマンス コンピューティングを必要とするワークロードがある場合は、クラウドでワークロードを実行することで、ハードウェアの初期コストを回避して、必要なときに使った分だけ支払うことができます。 

**ビッグ データ分析:** 役に立つ可能性のあるパターン、傾向、および関連付けを含む大規模なデータ セットがある場合、IaaS ではデータ セットをマイニングしてパターンを見つけるための処理能力を提供することができます。

### <a name="advantages"></a>長所

**資本コストはかからず、継続的なコストを削減:** IaaS では、オンサイト データ センターのセットアップと管理の初期費用がかからないため、スタートアップ企業や新しいアイデアをテストする企業にとって経済的なオプションになります。 新しい製品またはイニシアティブを開始することを決めたら、必要なコンピューティング インフラストラクチャを数分から数時間ですぐに準備することができます。社内でセットアップした場合には、数日から数週間、ときには数か月かかる場合があります。

**ビジネス継続性の向上とディザスター リカバリー:** 高可用性、ビジネス継続性、ディザスター リカバリーを実現するには、多くのテクノロジとスタッフが必要になるため、多額のコストがかかります。 しかし、適切なサービス レベル アグリーメント (SLA) があれば、IaaS によってこのコストを削減し、障害時または停止時にもアプリケーションとデータに通常どおりアクセスすることができます。

**変化するビジネス条件にすばやく対応:** IaaS を使用すると、たとえば休暇期間中などのアプリケーションの需要の急増に対応するために、リソースを迅速にスケールアップして、アクティビティが減少したらリソースを元に戻して、コストを節約することができます。 IaaS を使用すると、事前にインフラストラクチャをセットアップしなくても、アプリの開発と配信が行えるため、より迅速にアプリをユーザーに提供することができます。

**安定性、信頼性、およびサポート性の向上:** IaaS を使用すると、ソフトウェアとハードウェアのメンテナンスやアップグレード、または機器の問題のトラブルシューティングの必要がなくなります。 適切な契約を結ぶことで、ご利用のインフラストラクチャの信頼性と SLA の達成がサービス プロバイダーによって保証されます。
