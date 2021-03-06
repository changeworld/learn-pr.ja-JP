攻撃や未承認のアクセスからネットワークを保護することは、すべてのアーキテクチャの重要な部分です。 クラウドへの移行のための計画の一環として、Lamna Healthcare では、時間をかけてネットワーク インフラストラクチャに関する計画を策定しました。それは、ネットワーク インフラストラクチャを攻撃から保護するための適切なネットワーク セキュリティ管理が確実に実施されるようにするためです。 ここでは、ネットワーク セキュリティとはどのようなものか、階層型アプローチをご利用のアーキテクチャに統合する方法、ご利用の環境にネットワーク セキュリティを提供するときに Azure がどのように役立つかについて見ていきます。

## <a name="what-is-network-security"></a>ネットワークのセキュリティとは

ネットワーク セキュリティは、ネットワーク内およびネットワーク外にあるリソースの通信を保護します。 目標は、サービスおよびシステム全体のネットワーク層での露出を制限することです。 この露出を制限することで、リソースが攻撃される可能性が減ります。 ネットワーク セキュリティに注目すると、以下の部分に集中して作業を行うことができます。

- アプリケーションとインターネット間のトラフィック フローのセキュリティ保護
- アプリケーション間のトラフィック フローのセキュリティ保護
- ユーザーとアプリケーション間のトラフィック フローのセキュリティ保護

アプリケーションとインターネット間のトラフィック フローのセキュリティ保護では、自分のネットワークの外部への露出を制限することに注目します。 ネットワークへの攻撃はネットワークの外部から始まることが最も多いので、インターネットへの露出を制限して、境界をセキュリティで保護することにより、攻撃されるリスクを減らすことができます。

アプリケーション間のトラフィック フローのセキュリティ保護では、アプリケーション間、階層間、異なる環境間、ネットワーク内の他のサービス間のデータに注目します。 これらのリソース間の露出を制限することにより、侵害されたリソースが及ぼす影響を減らすことができます。 これにより、ネットワーク内で侵害が広がるのを抑えることができます。

ユーザーとアプリケーション間のトラフィック フローのセキュリティ保護では、エンド ユーザーに対するネットワーク フローのセキュリティ保護に注目します。 これにより、リソースが外部の攻撃にさらされる可能性が制限され、ユーザーがリソースを利用するためのセキュリティで保護されたメカニズムが提供されます。 

## <a name="a-layered-approach-to-network-security"></a>ネットワーク セキュリティに対する階層型アプローチ

このモジュールの共通スレッドでは、セキュリティに対して階層型アプローチが採用されており、このアプローチはネットワーク層でも違いません。 ネットワーク境界のセキュリティ保護、またはネットワーク内のサービス間のネットワーク セキュリティに注目するだけでは、十分ではありません。 階層型アプローチでは、攻撃者が 1 つの階層を突破した場合でも、それ以上の攻撃を制限するための保護がさらに存在するように、複数レベルの保護が提供されています。

Azure においてネットワーク フットプリントをセキュリティ保護するために階層型アプローチ向けのツールがどのように提供されるのかを見てみましょう。

### <a name="internet-protection"></a>インターネットからの保護

まず、ネットワークの境界の場合は、インターネットからの攻撃を制限および排除することに注目します。 出発点として最も適しているのは、インターネットに接続しているリソースを評価し、必要な場合にのみ受信および送信の通信を許可することです。 任意の種類の受信ネットワーク トラフィックを許可しているすべてのリソースを明らかにし、それらが必要であること、そして必要なポート/プロトコルのみに制限されていることを確認します。 Azure Security Center はこの情報を探すのに最適な場所です。ネットワーク セキュリティ グループ (NSG) が関連付けられていないインターネット接続リソース、およびファイアウォールの背後でセキュリティ保護されていないリソースがわかります。

境界で受信の保護を行うには 2 つの方法があります。 Application Gateway はレイヤー 7 のロード バランサーであり、HTTP ベースのサービスに対して高度なセキュリティを提供する Web アプリケーション ファイアウォール (WAF) も含まれています。 WAF は、OWASP 3.0 または 2.2.9 コア ルール セットのルールに基づき、クロスサイト スクリプティングや SQL インジェクションなどのよく知られた脆弱性からの保護を提供します。

![2 つの異なるサイトにある仮想マシンに対して行われたすべての外部要求をフィルタリングする 1 つのアプリケーション ゲートウェイを示している図。 アプリケーション ゲートウェイの Web アプリケーション ファイアウォール機能は悪意のある攻撃からシステムを保護しますが、ロード バランサーは仮想マシン間で正当な要求を分散します。](../media/appgw-waf.png)

非 HTTP ベースのサービスの保護の場合、またはさらにカスタマイズする場合は、ネットワーク仮想アプライアンス (NVA) を使用してネットワーク リソースをセキュリティ保護できます。 NVA は、オンプレミス ネットワークで使用されていることがあるファイアウォール アプライアンスに似ており、最も一般的な多くのネットワーク セキュリティ ベンダーから入手できます。 NVA は、それを必要とするアプリケーション用にセキュリティを詳細にカスタマイズできますが、複雑さが増すことがあるため、要件を慎重に検討することをお勧めします。

インターネットに公開されているすべてのリソースには、サービス拒否攻撃を受けるリスクがあります。 この種の攻撃では、リソースが低速または応答不能になるほど多くの要求を送信することによって、ネットワーク リソースに大きな負荷がかかるようにします。 これらの攻撃を軽減するため、Azure DDoS では、すべての Azure サービスに対する基本的な保護と、ユーザーのリソースに合わせてさらにカスタマイズできる拡張保護が提供されています。 DDoS 保護は、攻撃トラフィックをブロックし、残りのトラフィックをその目的の宛先に転送します。 攻撃の検出から数分以内に、Azure Monitor メトリックを使用してユーザーに通知します。

![仮想ネットワークと外部ユーザーの要求の間にインストールされている Azure DDoS 保護を示している図。 Azure DDoS 保護は、悪意のあるトラフィックの攻撃をブロックしますが、目的の宛先には正当なトラフィックを転送します。](../media/ddos.png)

### <a name="virtual-network-security"></a>仮想ネットワークのセキュリティ

仮想ネットワーク (VNet) の内部の場合は、リソース間の通信を必要なものだけに制限することが重要です。

仮想マシン間の通信では、ネットワーク セキュリティ グループ (NSG) が不要な通信を制限するための重要な部分です。 NSG はレイヤー 3 と 4 で動作し、ネットワーク インターフェイスおよびサブネットとの間の双方向の通信について、許可および拒否するものの一覧を提供します。 NSG は完全にカスタマイズ可能であり、仮想マシンとやり取りされるネットワーク通信を完全にロックダウンすることができます。 NSG を使用すると、環境間、階層間、およびサービス間でアプリケーションを分離できます。

![バックエンドと中間層がインターネットと直接通信することを制限するネットワーク セキュリティ グループの使用状況を示している図。 インターネット要求は、まずフロント エンドで受信され、その後中間層に渡されます。 中間層はバックエンドと通信します。 ](../media/azure-network-security.png)

Azure サービスを分離して仮想ネットワークからの通信のみを許可するには、VNet サービス エンドポイントを使用します。 サービス エンドポイントを使用すると、Azure サービス リソースへのアクセスを仮想ネットワークに限定することができます。 サービス リソースを仮想ネットワークに限定することで、リソースへのパブリック インターネット アクセスを完全に排除し、ご利用の仮想ネットワークからのトラフィックのみを許可することにより、セキュリティが向上します。 これにより、環境の攻撃対象領域が減り、VNet と Azure サービスの間の通信を制限するために必要な管理が減り、この通信に最適なルーティングが提供されます。

### <a name="network-integration"></a>ネットワーク統合

一般に、オンプレミス ネットワークからの通信を提供したり、Azure 内のサービス間の通信をよりよくするには、既存のネットワーク インフラストラクチャを統合する必要があります。 この統合を処理してネットワークのセキュリティを強化するには、いくつかの主要な方法があります。

仮想プライベート ネットワーク (VPN) 接続は、ネットワーク間に安全な通信チャネルを確立する一般的な方法であり、Azure で仮想ネットワークを使用する場合も同じです。 Azure VNet とオンプレミスの VPN デバイス間の接続は、ユーザーのネットワークと Azure 上のユーザーの仮想マシンの間にセキュリティ保護された通信を提供する優れた方法です。

ネットワークと Azure の間に専用のプライベート接続を提供するには、ExpressRoute を使用できます。 ExpressRoute を利用すると、接続プロバイダーが提供するプライベート接続を介して、オンプレミスのネットワークを Microsoft クラウドに拡張できます。 ExpressRoute では、Microsoft Azure、Office 365、Dynamics 365 などの Microsoft クラウド サービスへの接続を確立できます。 このようにすると、インターネット経由の代わりにプライベート回線でこのトラフィックを送信することによって、オンプレミスの通信のセキュリティが向上します。 エンド ユーザーにインターネットを介したこれらのサービスへのアクセスを許可する必要はなく、アプライアンス経由でこのトラフィックを送信することによりトラフィックをさらに検査できます。

![お客様のネットワークを Azure リソースと接続する ExpressRoute 回線を示すアーキテクチャ図。](../media/expressroute-connection-overview.png)

Azure 内の複数の VNet を簡単に統合するため、VNet ピアリングでは指定した VNet 間に直接接続が確立されます。 確立された後は、VNet 内のリソースを保護するのと同じ方法で、NSG を使用してリソース間の分離を提供できます。 この統合では、ピアリングされた VNet 間に同じ基本的なセキュリティ レイヤーを提供できます。 直接接続された VNet 間の通信だけが許可されます。

## <a name="network-security-at-lamna-healthcare"></a>Lamna Healthcare でのネットワークのセキュリティ

Lamna Healthcare は、これらのサービスの多くを利用して、セキュリティ保護されたネットワーク インフラストラクチャを構築しています。 リソース間の通信は既定では拒否され、必要な場合にのみ許可されます。 インターネットからの受信接続は、それを必要とするサービスに対してのみ有効になります。RDP と SSH はインターネット エンドポイントからは許可されず、信頼されている内部リソースからのみ許可されます。

インターネットに接続する Web サービスをセキュリティで保護するため、それらは WAF を有効にした Application Gateway の背後に配置されています。 仮想マシンで実行されているサービスと App Service の両方についてそのようになっています。 Application Gateway を使用することにより、多くの一般的な脆弱性から保護しています。

DDoS 標準を有効にして、インターネットに接続するエンドポイントをサービス拒否攻撃から保護しています。

NSG を使用することで、アプリケーション サービス間および環境間の通信を完全に分離できます。 環境内のサービス間では必要な通信のみを許可しており、運用環境と非運用環境の間のアクセスは許可していません。

エンド ユーザーと Azure のアプリケーションの間に専用接続を提供するため、オンプレミスのネットワークと接続する ExpressRoute 回線をプロビジョニングしています。 これにより、Azure へのトラフィックはインターネットを経由せず、Azure 内のサービスがシステムと通信するためのプライベート接続はオンプレミスに留まっています。

この方法により、Lamna Healthcare は Azure のサービスを利用して、ネットワーク インフラストラクチャの複数のレイヤーでセキュリティを提供しています。

## <a name="summary"></a>まとめ

ネットワーク セキュリティに対する階層型アプローチは、ネットワーク ベースの攻撃による露出のリスクを減らすのに役立ちます。 Azure では、インターネットに接続されたリソース、内部リソース、およびオンプレミス ネットワーク間の通信をセキュリティ保護するための、複数のサービスと機能が提供されています。 これらの機能を使用すると、Azure 上でセキュリティ保護されたソリューションを作成できます。