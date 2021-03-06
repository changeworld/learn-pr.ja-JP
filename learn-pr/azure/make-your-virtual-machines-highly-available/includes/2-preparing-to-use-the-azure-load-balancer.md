あなたが勤務する会社は、Azure Load Balancer がエンタープライズ リソース プラニング (ERP) に対応しているかどうか知りたいとします。 アプリケーションにはユーザーのための Web インターフェイスがあり、複数の Web サーバーで実行されます。 各サーバーには ERP データベースのローカル コピーが含まれます。ERP データベースはすべてのサーバーの間で同期されています。

ここで、サービスの高可用性に対してロード バランサーがどのように貢献できるか確認します。 ロード バランサー オプションの基本と標準の違いを特定し、Azure Virtual Machines 向けのロード バランサーを作成する方法を確認します。

## <a name="what-is-load-balancing"></a>負荷分散とは

_負荷分散_とは、作業負荷をコンピューティング デバイス、ストレージ デバイス、ネットワーク デバイスなどの複数のデバイスに分散するさまざまな手法を言い表したものです。 負荷分散の目的は、さまざまなリソースを最適な方法で活用すること、インフラストラクチャの拡張に合わせ、リソースを最も効率的に活用すること、一部の構成要素が利用不可になった場合でもサービスを維持することです。

ここでは、仮想マシン (VM) 向けの Azure の負荷分散のサポートを確認します。

### <a name="what-is-high-availability"></a>高可用性とは

高可用性 (HA) とは、システムの一部の構成要素に障害が発生しても、アプリケーションまたはサービスを引き続き利用できる能力を計る指標です。 著しいサービスの停止が発生しないことが理想です。

負荷分散は複数の VM がサーバー プールとして機能するため、HA 実現の基礎となります。 一部の VM がクラッシュしたり、保守管理のためにオンラインになったりしても、このプールは引き続き要求に応答できます。

## <a name="what-is-azure-load-balancer"></a>Azure Load Balancer とは

**Azure Load Balancer** は、あるプールに含まれる複数の VM 間で受信した要求を分散する Azure サービスです。 入ってくるネットワーク トラフィックを正常な一連の VM 間で分散し、応答できない VM があれば、その VM を回避します。

 Azure Load Balancer は OSI 7 層モデルの Layer-4 (TCP、UDP) で動作します。 トラフィックが Azure VM に入ってくる TCP/UDP アプリケーション シナリオと、他の Azure サービスにより、Azure VM 経由で外部エンドポイントに TCP/UDP トラフィックが渡されるアウトバウンド シナリオをサポートするように構成できます。

#### <a name="what-is-a-load-balancer"></a>Load Balancer とは

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yBWo]

## <a name="public-vs-internal-load-balancers"></a>パブリック ロード バランサーと内部ロード バランサー

Azure Load Balancer は、入ってくる要求の源に基づき、_パブリック_または_内部_になります。

**パブリック ロード バランサー**では、Azure インフラストラクチャの外から入ってきたクライアント要求が処理されます。 このロード バランサーには、パブリック IP アドレスが割り当てられます。これは、プライベート仮想ネットワーク上にあるリソースのセットに受信要求をルーティングします。 一般的に、このアプローチを使用して、Web サーバーの可用性を高くします。 パブリック ロード バランサーを次の図に示します。

![パブリック ロード バランサーが内部から仮想ネットワーク上の 3 つの VM にクライアント要求を分散する図。](../media/2-public-load-balancer.png)

**内部ロード バランサー**は、仮想ネットワーク内からの要求 (または VPN 経由の要求) を処理します。 それは、リソースに対する要求を仮想ネットワーク内に分散します。 ロード バランサー、フロント エンド IP アドレス、仮想ネットワークにはインターネットから直接アクセスできません。 次の図は、パブリック ロード バランサーと内部ロード バランサーの両方を含むアーキテクチャを示しています。 パブリック ロード バランサーでは外部要求が処理されますが、内部ロード バランサーでは処理のために内部の VM とデータベースに要求が転送されます。

![パブリック ロード バランサーがクライアント要求を内部ロード バランサーに転送する図。 次に、内部ロード バランサーが要求の種類に基づき、Web 層サブネットまたはデータベース層サブネットに要求を分配します。 Web 層サブネットとデータベース層サブネットの両方に、要求を処理するための複数のサーバーが用意されます。](../media/2-internal-load-balancer.png)

## <a name="how-does-azure-load-balancer-work"></a>Azure Load Balancer のしくみとは

Azure Load Balancer では、_規則_と_正常性プローブ_に構成されている情報を使用して、ロード バランサーの_フロントエンド_で受信されたインバウンド トラフィックを_バックエンド アドレス プール_の VM インスタンスに分散する方法を決定します。 これらの各コンポーネントを確認してみましょう。

### <a name="what-is-the-load-balancer-front-end"></a>ロード バランサーのフロント エンドとは

ロード バランサー フロントエンドは IP 構成であり、1 つまたは複数のパブリック IP アドレスが含まれ、インターネットを経由してロード バランサーとそのアプリケーションにアクセスできます。

### <a name="what-is-the-backend-address-pool"></a>バックエンド アドレス プールとは

仮想マシンは、仮想ネットワーク インターフェイス (vNIC) を使用してロード バランサーに接続されます。 バックエンド アドレス プールには、ロード バランサーに接続される vNIC の IP アドレスが含まれます。 可用性セットにすべての VM を配置した場合は、それを使用して、ロード バランサーの構成時に、バックエンド プールに VM を簡単に追加できます。

### <a name="what-is-a-health-probe"></a>正常性プローブとは

ロード バランサーでは_正常性プローブ_が使用され、要求に対応する仮想マシンが決定されます。 ロード バランサーでは、作動中で利用可能な VM にのみトラフィックが分配されます。

正常性プローブによって、各 VM の特定のポートが監視されます。 どのような種類の応答が "正常" に相当するか定義できます。たとえば、Web アプリケーションから `HTTP 200 Available` 応答を要求することがあります。 VM では既定で、15 秒の間隔をおいて 2 回連続で失敗すると "利用不可" と見なされます。

### <a name="load-balancer-rules"></a>ロード バランサーの規則

ロード バランサーの_規則_により、バックエンド VM にトラフィックを分配する方法が定義されます。 その目的は、バックエンド プールにある正常な VM 間で等しく要求を分配することにあります。

Azure Load Balancer では、ハッシュベースのアルゴリズムが使用され、インバウンド パケットのヘッダーが書き換えられます。 Load Balancer では、既定で、以下からハッシュが作成されます。

- 送信元 IP アドレス
- 送信元ポート
- 宛先 IP アドレス
- 宛先ポート
- IP プロトコル番号

このメカニズムにより、あるパケット クライアント フロー内のすべてのパケットが同じバックエンド VM インスタンスに確実に送信されます。 クライアントからの新しいフローでは、ランダムに割り当てられたソース ポートが使用されます。 これはハッシュが変更され、ロード バランサーは異なるバックエンド エンドポイントにフローを送信する可能性があることを意味します。

## <a name="basic-vs-standard-load-balancer-skus"></a>Basic と Standard の Load Balancer SKU

Azure Load Balancer には **Basic** と **Standard** という 2 つのバージョンがあります。 両者の間には、スケール、機能、料金の違いがあります。 例:

- Standard では、HTTPS を介したトラフィックの安全なルーティングがサポートされています。
- バックエンドのプール サイズは Standard の方がはるかに大きくなります (Basic SKU は 1000 に対して 100)。
- トラフィックは、スケール セット、可用性セット、および VM の混合など、エンドポイントのより大きなプールに送ることができます。 Basic SKU は、1 つの可用性セット、スケール セット、または VM に限定されています。
- 内部ロード バランサーとして使用する場合、すべてのポートで TCP フローと UDP フローの負荷分散を同時に行うことができる、高可用性 (HA) ポートがサポートされます。
- Basic は無料であり、Standard は規則とスループットに基づいて課金されます。

Standard は Basic のスーパーセットです。そのため、Basic に適したシナリオは Standard でも機能します。 Basic SKU は一般的に試作品の作成と試験に利用され、運用には Standard が推奨されます。

## <a name="start-the-deployment-of-a-basic-public-load-balancer"></a>Basic Public Load Balancer のデプロイを始める

負荷分散された VM システムを構築するには、ロード バランサー自体を構築し、VM を含める仮想ネットワークを構築し、仮想ネットワークに VM を追加する必要があります。

Azure portal を使用してロード バランサーを構築するには、以下を定義します。

- ロード バランサー名
- 種類: パブリックまたは内部
- SKU: Basic または Standard
- パブリック IP アドレス: 動的または静的
- リソース グループと場所

バックエンド VM はすべて同じ仮想ネットワークに接続されます。そのため、次にこのリソースを構成する必要があります。

- 仮想ネットワーク名
- 使用するアドレス空間 (172.20.0.0/16 など)
- リソース グループ
- 使用するサブネットの名前
- サブネットのアドレス空間 (172.20.0.0/24 など) (メイン空間内に存在する必要があります)

受信トラフィックを想定しているために、ネットワーク セキュリティ グループ (NSG) を使用してセキュリティ規則をいくつか作成する必要があります。 この場合、HTTP トラフィック用にポート 80 を開きます。

最後に、仮想マシンを構築してデプロイし、仮想ネットワークを使用するように構成する必要があります。 それらをまとめるために、可用性セットも作成します。 可用性セットにより、VM のグループ全体でフォールト トレランスのレベルが定義されます。ただし、負荷分散については、VM をバックエンド プールに割り当てる際にも役立ちます。

今回、高可用性ソリューションの一部として Azure Load Balancer を使用する方法をご覧いただきました。 次はここで学習した手順で独自のロード バランサーをデプロイします。