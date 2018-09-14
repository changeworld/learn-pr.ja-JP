セキュリティと接続の信頼性に関する問題が、会社では、機密性の高いデータを処理し、大量の情報を Azure に格納されますが、パブリック インターネット経由であります。 会社がより高いレベルの接続性、セキュリティ、および信頼性を示すことができる場合を除き、Azure に全面的に移行するつもりはありません。

ここでは、Azure データ センターに直接専用の行に、インターネット経由で実行する接続を超えるしましょう。

## <a name="azure-expressroute"></a>Azure ExpressRoute

Microsoft Azure ExpressRoute を使用することにより、自社のオンプレミス ネットワークを接続プロバイダーによって実装されたプライベート接続経由で Microsoft クラウドに拡張します。 この配置では、Azure データ センターへの接続は、インターネット経由であっても専用のリンクに移動しないことを意味します。 ExpressRoute は、Office 365 や Dynamics 365 などの他の Microsoft Cloud ベース サービスとの効率的な接続も促進します。

ExpressRoute が提供する利点は次のとおりです。

- 帯域幅の動的スケーリングがある 50 Mbps から 10 Gbps のより速い速度

- より短い待機時間

- 組み込みのピアリングを介して信頼性が向上

- より高度なセキュリティ

ExpressRoute には、さらに次のような利点もあります。

- サポートされているすべての Azure サービスへの接続性

- すべてのリージョンへのグローバル接続 (Premium アドオンが必要)

- Border Gateway Protocol 経由での動的ルーティング

- 接続アップタイムのサービス レベル アグリーメント (Sla)

- Skype for Business のサービスの品質 (QoS)

さらに、ExpressRoute premium アドオンは、ルート制限の増加、グローバル サービスの接続、および回路ごとの強化された仮想ネットワーク リンクなどの利点があります。

## <a name="expressroute-connectivity-models"></a>ExpressRoute 接続モデル

ExpressRoute への接続には、次のメカニズムを使用できます。

- IP VPN ネットワーク (任意の環境間)

- イーサネット交換による仮想交差接続

- ポイント ツー ポイントのイーサネット接続

 ExpressRoute の機能は上記の接続モデルのすべてに共通しています。

### <a name="what-is-layer-3-connectivity"></a>レイヤー 3 接続とは

Microsoft は業界標準の動的ルーティング プロトコル (BGP) を交換するでは、パブリック アドレスを Azure のインスタンス、オンプレミス ネットワークと Microsoft の間にルーティングします。 さまざまなトラフィック プロファイルに合わせ、ネットワークとさまざまな BGP セッションを確立します。

### <a name="any-to-any-ipvpn-networks"></a>任意の環境間 (IPVPN) ネットワーク

通常、IPVPN プロバイダーは、管理対象レイヤー 3 接続経由でブランチ オフィスと企業のデータ センター間の接続を提供します。 Expressroute では、Azure データ センターでは、別のブランチ オフィスがあった場合とが表示されます。

### <a name="virtual-cross-connection-through-an-ethernet-exchange"></a>仮想交差接続、イーサネット交換経由

組織がクラウドの exchange の施設に併置されている場合を要求する、Microsoft Cloud へのクロス接続は、プロバイダーのイーサネット交換。 Microsoft Cloud へのこれらクロス接続は、レイヤー 2 またはレイヤー 3 のいずれかで動作できるネットワーク OSI モデルのように、接続を管理します。

### <a name="point-to-point-ethernet-connection"></a>ポイント ツー ポイントのイーサネット接続

ポイント ツー ポイントのイーサネット リンクでは、Microsoft のクラウドをオンプレミス データ センターまたはオフィス間のレイヤー 2 または管理対象レイヤー 3 接続を提供できます。

## <a name="how-expressroute-works"></a>ExpressRoute のしくみ

Azure ExpressRoute では、ExpressRoute 回線とルーティング ドメインの組み合わせを使用して、Microsoft クラウドに高帯域幅接続を提供します。

### <a name="what-are-expressroute-circuits"></a>ExpressRoute 回線とは

ExpressRoute 回線は、オンプレミス インフラストラクチャと Microsoft のクラウド間の論理接続です。 接続プロバイダーがその接続を実装しますが、組織によっては冗長性を確保するために複数の接続プロバイダーを使用します。 各回線は 50、100、200 のいずれかの固定の帯域幅 Mbps または 500 Mbps、または 1 Gbps または 10 Gbps、およびこれらの回線のそれぞれが、接続プロバイダーとピアリングの場所にマップします。 また、各 ExpressRoute 回線には既定のクォータと制限があります。

ExpressRoute 回線は、ネットワーク接続またはネットワーク デバイスと同じです。 各回線はという GUID を_サービス_または_s キー_します。 この s キーは、Microsoft、接続プロバイダー、および組織間の接続リンクを提供します。-暗号化シークレットはありません。 各 s キーには、Azure ExpressRoute 回線への 1 対 1 マッピングがあります。

各回線は、最大 3 つのピアリング、冗長性のために構成されている BGP セッションのペアであることができます。 次に例を示します。

- Azure プライベート
- Azure パブリック
- Microsoft

### <a name="routing-domains"></a>ルーティング ドメイン

ExpressRoute 回線は、複数のルーティング ドメインを持つ各 ExpressRoute 回線とルーティング ドメインは、しにマップします。 これらのドメインは、上記 3 のピアリングの場合と同じです。 アクティブ/アクティブ構成では、ルーターの各ペアの各ルーティング ドメインの構成が同じであるため、高可用性が実現されます。 Azure パブリックと Azure プライベート ピアリング名前は、IP アドレス指定スキームを表します。

#### <a name="azure-private-peering"></a>Azure プライベート ピアリング

Azure プライベート ピアリングは、仮想マシンなどの Azure コンピューティング サービスと仮想ネットワークにデプロイされているクラウド サービスに接続します。 セキュリティに関しては、プライベート ピアリング ドメインは単にオンプレミス ネットワークを Azure に拡張したものです。 そのネットワークと Azure の仮想ネットワーク間の双方向接続を有効にし、内部ネットワーク内で Azure VM の IP アドレスを公開します。

> [!NOTE]
> プライベート ピアリング ドメインには、1 つだけの仮想ネットワークを接続できます。

#### <a name="azure-public-peering"></a>Azure パブリック ピアリング

Azure Storage、Azure SQL database、Azure の web サービスなどのパブリック IP アドレスで使用可能なサービスにプライベート接続を有効に azure パブリック ピアリングします。 パブリック ピアリングでは、せずに、インターネット経由でルーティングされるトラフィックをそれらサービスのパブリック IP アドレスに接続できます。 接続は常に WAN から Azure であり、他の方法では行われません。 これはまた融通の利かないパブリック ピアリングを有効にするサービスを選択することはできません。

> [!NOTE]
> Azure PaaS サービスでは、Microsoft のパブリック ピアリングではなく、ピアリングを使用するがお勧めします。

#### <a name="microsoft-peering"></a>Microsoft ピアリング

Microsoft ピアリングには、Office 365 や Dynamics 365 など SaaS クラウド ベース サービスへの接続をサポートしています。 このピアリング オプションでは、会社の WAN と Microsoft クラウド サービス間の双方向接続が提供されます。

### <a name="expressroute-health"></a>ExpressRoute の正常性

Microsoft Azure のほとんどの機能と同様に、ExpressRoute 接続を監視して、正常に実行されていることを確認できます。 監視には、次の領域の対象範囲が含まれます。

- 可用性
- 仮想ネットワークへの接続
- 帯域幅の使用率

この監視アクティビティの主要ツールは Network Performance Monitor (特に ExpressRoute 用の NPM) です。

## <a name="summary"></a>まとめ

Azure ExpressRoute は、Azure のデータセンターとオンプレミスや共用環境にあるインフラストラクチャの間でプライベート接続を作成するために使用されます。 ExpressRoute 接続はパブリックなインターネットを経由しないし、詳細の信頼性、速度、および一般的なインターネット接続よりも低い待機時間を提供します。
