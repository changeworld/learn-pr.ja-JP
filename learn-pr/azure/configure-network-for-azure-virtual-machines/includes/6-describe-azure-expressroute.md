企業では機密性の高いデータを扱い、Azure に格納される情報が大量にあるため、パブリック インターネット上での接続のセキュリティと信頼性についていくつかの考慮事項があります。 企業は、より高いレベルの接続性、セキュリティ、信頼性を実証できるまで、Azure への大規模な移行を望みません。

ここでは、パブリック インターネット経由ではなく、専用回線で Azure データセンターに直接接続します。

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute を利用すれば、組織では接続プロバイダーによって実装されるプライベート接続で、オンプレミスのネットワークを Microsoft Cloud に拡張できます。 この配置は、Azure データセンターへの接続がインターネット経由ではなく、専用リンク経由であることを意味します。 ExpressRoute により、Office 365 や Dynamics 365 などの他の Microsoft のクラウドベース サービスとの効率的な接続も容易になります。

ExpressRoute が提供する利点は次のとおりです。

- 帯域幅の動的スケーリングがある 50 Mbps から 10 Gbps のより速い速度

- より短い待機時間

- 組み込みピアリングを通じたより高い信頼性

- より高度なセキュリティ

ExpressRoute には、さらに次のような利点もあります。

- サポートされているすべての Azure サービスへの接続性

- すべてのリージョンへのグローバル接続 (Premium アドオンが必要)

- Border Gateway Protocol 経由での動的ルーティング

- 接続アップタイムに関するサービス レベル アグリーメント (SLA)

- Skype for Business のサービスの品質 (QoS)

さらに、ルート制限の増加、グローバル サービスの接続、回線ごとの仮想ネットワーク リンクの増加などのメリットを提供する ExpressRoute Premium アドオンがあります。

## <a name="expressroute-connectivity-models"></a>ExpressRoute 接続モデル

ExpressRoute への接続には、次のメカニズムを使用できます。

- IP VPN ネットワーク (任意の環境間)

- イーサネット交換による仮想交差接続

- ポイント ツー ポイントのイーサネット接続

 ExpressRoute の機能は上記の接続モデルのすべてに共通しています。

### <a name="what-is-layer-3-connectivity"></a>レイヤー 3 接続とは

Microsoft では業界標準の動的ルーティング プロトコル (BGP) を利用して、オンプレミス ネットワーク、Azure のインスタンス、および Microsoft パブリック アドレスの間のルートを交換します。 さまざまなトラフィック プロファイルに合わせ、ネットワークとの複数の BGP セッションを確立します。

### <a name="any-to-any-ipvpn-networks"></a>任意の環境間 (IPVPN) ネットワーク

通常、IPVPN プロバイダーは、マネージド レイヤー 3 接続経由でブランチ オフィスと会社のデータセンターの間の接続を提供します。 ExpressRoute では、Azure データセンターは別のブランチ オフィスであるかのように見えます。

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>イーサネット交換による仮想交差接続

組織がクラウド エクスチェンジ施設に併置されている場合は、プロバイダーのイーサネット交換を通じて Microsoft Cloud への交差接続を要求します。 Microsoft Cloud へのこれらの交差接続は、ネットワーク OSI モデルの場合と同様に、レイヤー 2 またはレイヤー 3 マネージド接続で動作できます。

### <a name="point-to-point-ethernet-connection"></a>ポイント ツー ポイントのイーサネット接続

ポイント ツー ポイントのイーサネット リンクでは、Microsoft Cloud にオンプレミス データセンターまたはオフィス間のレイヤー 2 またはマネージド レイヤー 3 接続を提供できます。

## <a name="how-expressroute-works"></a>ExpressRoute のしくみ

Azure ExpressRoute では、ExpressRoute 回線とルーティング ドメインの組み合わせを使用して Microsoft Cloud への高帯域幅接続が提供されます。

### <a name="what-are-expressroute-circuits"></a>ExpressRoute 回線とは

ExpressRoute 回線は、オンプレミス インフラストラクチャと Microsoft Cloud 間の論理接続です。 接続プロバイダーがその接続を実装しますが、組織によっては冗長性を確保するために複数の接続プロバイダーを使用します。 各回線には 50、100、200、500 Mbps、1 または 10 Gbps の固定の帯域幅があり、これらの回線それぞれが接続プロバイダーとピアリングの場所にマップされます。 また、各 ExpressRoute 回線には既定のクォータと制限があります。

ExpressRoute 回線は、ネットワーク接続やネットワーク デバイスと同じではありません。 各回線は_サービス_または _s キー_という GUID で定義されます。 この s キーでは、Microsoft、接続プロバイダー、および組織間の接続リンクが提供されます。これは暗号化シークレットではありません。 各 s キーには、Azure ExpressRoute 回線への 1 対 1 のマッピングがあります。

各回線は、冗長性を確保するために構成されている BGP セッションのペアであるピアリングを最大 3 つ持つことができます。 そのペアリングは以下のとおりです。

- Azure プライベート
- Azure パブリック
- Microsoft

### <a name="routing-domains"></a>ルーティング ドメイン

その後、ExpressRoute 回線はルーティング ドメインにマップされます。各 ExpressRoute 回線には複数のルーティング ドメインがあります。 これらのドメインは上にリストした 3 つのピアリングと同じです。 アクティブ/アクティブ構成では、ルーターの各ペアの各ルーティング ドメインの構成が同じであるため、高可用性が実現されます。 Azure パブリックおよび Azure プライベートのピアリング名は、IP アドレス指定スキームを表します。

#### <a name="azure-private-peering"></a>Azure プライベート ピアリング

Azure プライベート ピアリングでは、仮想ネットワークでデプロイされている仮想マシンやクラウド サービスなどの Azure コンピューティング サービスに接続します。 セキュリティに関しては、プライベート ピアリング ドメインは単にオンプレミス ネットワークを Azure に拡張したものです。 その後、そのネットワークと Azure 仮想ネットワーク間の双方向接続を有効にし、内部ネットワーク内で Azure VM の IP アドレスを公開します。

> [!NOTE]
> プライベート ピアリング ドメインには 1 つの仮想ネットワークのみを接続できます。

#### <a name="azure-public-peering"></a>Azure パブリック ピアリング

Azure パブリック ピアリングでは、Azure Storage、Azure SQL データベース、Azure Web サービスなど、パブリック IP アドレスで使用可能なサービスへのプライベート接続を有効にします。 パブリック ピアリングでは、トラフィックをインターネット経由でルーティングせずに、それらのサービスのパブリック IP アドレスに接続できます。 接続は常に WAN から Azure であり、他の方法では行われません。 また、パブリック ピアリングを有効にするサービスを選択することはできないので、これはオールオアナッシングのアプローチです。

> [!NOTE]
> Azure PaaS サービスでは、パブリック ピアリングではなく、Microsoft ピアリングを使用することをお勧めします。

#### <a name="microsoft-peering"></a>Microsoft ピアリング

Microsoft ピアリングでは、Office 365 や Dynamics 365 などのクラウド ベースの SaaS サービスへの接続がサポートされます。 このピアリング オプションでは、会社の WAN と Microsoft クラウド サービス間の双方向接続が提供されます。

### <a name="expressroute-health"></a>ExpressRoute の正常性

Microsoft Azure のほとんどの機能と同様に、ExpressRoute 接続を監視して、正常に実行されていることを確認できます。 監視には、次の領域の対象範囲が含まれます。

- 可用性
- 仮想ネットワークへの接続
- 帯域幅の使用率

この監視アクティビティの主要ツールは Network Performance Monitor (特に ExpressRoute 用の NPM) です。

## <a name="summary"></a>まとめ

Azure ExpressRoute は、Azure のデータセンターとオンプレミスや共用環境にあるインフラストラクチャの間でプライベート接続を作成するために使用されます。 ExpressRoute 接続はパブリック インターネットを経由しないので、一般的なインターネット接続と比べて信頼性が高く、高速で、待ち時間も短くなります。
