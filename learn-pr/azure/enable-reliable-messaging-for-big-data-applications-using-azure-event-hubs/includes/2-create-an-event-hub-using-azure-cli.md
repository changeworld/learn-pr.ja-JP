あなたのチームは、そのシステム経由で到着するトランザクション量の増加に対応し、処理するために Azure Event Hubs の機能を利用することにしました。

イベント ハブは Azure のリソースです。そこでまず、Azure で新しいハブを作成し、アプリケーションに固有の要件に合わせて構成する必要があります。

## <a name="what-is-an-azure-event-hub"></a>Azure のイベント ハブとは

Azure Event Hubs はクラウドベースのイベント処理サービスです。毎秒数百万のイベントを受け取って処理することができます。 Event Hubs はイベント パイプラインの入り口として機能し、受信データを受け取り、処理リソースが使用できるようになるまでそのデータを格納します。

Event Hubs にデータを送信するエンティティは、*パブリッシャー*と呼ばれます。Event Hubs からデータを読み取るエンティティは、*コンシューマー*または*サブスクライバー*と呼ばれます。 Azure Event Hubs は、これら 2 つのエンティティの間に配置され、(パブリッシャーによる) イベント ストリームの生成と (サブスクライバーによる) 利用を分離します。 この分離によって、イベント生成速度が消費よりもはるかに速いシナリオを管理できます。

![パブリッシャーは複数のイベントを 1 つのイベント ハブに送信し、サブスクライバーがデータ利用できるようにします](../media-draft/2-event-hub-overview.png "イベント ハブの概要")

### <a name="events"></a>イベント

**イベント**は、変更の通知を含む小さなパケットであって、厳密な変更内容の詳細ではありません。 どのような形であれ、コンシューマー アプリケーションの構成を通じてクエリを実行し、詳細を調べることはできませんが、これは要件ではなく、また、Event Hubs で処理するものでもありません。 イベントは個別に、またはバッチとして発行できますが、1 回の発行 (個別またはバッチ) の上限は 256 KB です。

対照的に、(Azure Service Bus の) メッセージ キュー処理の場合、**メッセージ**にはデータとイベントが含まれており、メッセージのパブリッシャーはコンシューマーが特定の方法でメッセージを処理することを想定しています。

### <a name="publishers-and-subscribers"></a>パブリッシャーとサブスクライバー

イベントのパブリッシャーとは、HTTPS または Advanced Message Queuing Protocol (AQMP) 1.0 を使用してイベント データを送信できる任意のアプリケーションまたはデバイスです。 

データを頻繁に送信するパブリッシャーの場合、パフォーマンスの点で AMQP の方が優れています。 ただし、永続的な双方向ソケットとトランスポート層セキュリティ (TLS) または SSL/TLS を最初に設定する必要があるため、初回のセッションのオーバーヘッドは高くなります。 

より断続的な発行の場合は、HTTPS を使用することをお勧めします。 HTTPS の場合、要求ごとに SSL の追加のオーバーヘッドが必要ですが、セッションの初期化のオーバーヘッドはありません。

> [!NOTE] 
> 既存の Kafka ベースのアプリケーションでは、Apache Kafka 1.0 以降のクライアント バージョンを使用しており、Event Hubs のパブリッシャーとしても機能します。

イベントのサブスクライバーは、サポートされている 2 つのプログラム方式のいずれかを使用して、イベント ハブからイベントを受信し、処理するアプリケーションです。

- **EventHubReceiver**: 使用できる管理オプションが限られている簡単な方法です。
- **EventProcessorHost**: より効率的な方法です。このモジュールで後で使用します。

### <a name="consumer-groups"></a>コンシューマー グループ

イベント ハブの**コンシューマー グループ**は、イベント ハブ データ ストリームの特定のビューを表します。 別のコンシューマー グループを使用することで、複数のサブスクライバー アプリケーションが、他のアプリケーションに影響を与えることなく、独立してイベント ストリームを処理できます。 ただし、複数のコンシューマー グループの使用は必須ではありません。多くのアプリケーションでは、既定のコンシューマー グループが 1 つあれば十分です。

### <a name="pricing"></a>価格

Azure Event Hubs には、Basic、Standard、Dedicated という 3 つの価格レベルがあります。 各レベルには、サポートされる接続、使用できるコンシューマー グループの数、スループットの点で違いがあります。 価格レベルを指定せずに、Azure CLI を使用して Event Hubs 名前空間を作成すると、既定の **Standard** (20 個のコンシューマー グループ、1000 個の仲介型接続) が割り当てられます。

## <a name="creating-and-configuring-a-new-azure-event-hubs"></a>新しい Azure Event Hubs の作成と構成

新しい Azure Event Hubs を作成および構成する場合、主に 2 つの手順があります。 最初の手順は、Event Hubs **名前空間**を定義することです。 2 つ目の手順は、その名前空間にイベント ハブを作成することです。

### <a name="defining-an-event-hubs-namespace"></a>Event Hubs 名前空間を定義する

Event Hubs 名前空間は、1 つまたは複数のイベント ハブを管理するエンティティです。 

通常、Event Hubs 名前空間を作成するには、以下を実行します。

1. 名前空間レベルの設定を定義します。 名前空間の容量 (**スループット ユニット**を使用して構成されます)、価格レベル、パフォーマンス メトリックなど、一部の設定は名前空間レベルで定義されます。 これらは、その名前空間内のすべてのイベント ハブに適用されます。 これらの設定を定義しない場合、既定値 (容量には *1*、価格レベルには *Standard*) が使用されます。

    一度設定したスループット ユニットは変更できません。 Azure の予算見込みに合わせて構成を調整する必要があります。 さまざまなスループット要件に応じて異なるイベント ハブ構成を考慮する場合があります。 たとえば、売上データ アプリケーションがあり、2 つの Event Hubs を計画しているとします。1 つは高スループットなリアルタイム売上データ テレメトリの収集用、もう 1 つは頻度の低いイベント ログの収集用です。この場合、各ハブに別の名前空間を使用する方が合理的です。 この方法なら、テレメトリ ハブに対して高スループットな容量を構成する (そして支払う) だけで済みます。

1. 名前空間の一意の名前を選択します。 名前空間にアクセスするには、*_namespace_.servicebus.windows.net* という URL を使用します。

1. 次の省略可能なプロパティを定義します。

    - Kafka を有効にします。 このオプションによって、Kafka アプリケーションからイベント ハブにイベントを公開できるようになります。
    - この名前空間をゾーン冗長にします。 ゾーン冗長によって、独立した電力とネットワーク、冷却インフラストラクチャを独自に持つ個別のデータ センター間でデータがレプリケートされます。
    - 自動インフレを有効にし、最大スループット ユニットを自動的に増やします。 自動インフレは、スループット ユニット数を最大値まで増やすことで自動スケールアップ オプションを提供します。 これは、現在設定されているスループット ユニット数を受信または送信データ レートが超える場合に、スロットルを回避するために便利です。

### <a name="azure-cli-commands-for-creating-an-event-hubs-namespace"></a>Event Hubs 名前空間を作成するための Azure CLI コマンド

新しい Event Hubs 名前空間を作成するには、次のコマンドを使用します。

1. Azure では、リソース グループは、管理を容易にするために関連する Azure リソースを保持するコンテナーです。 必要に応じて、イベント ハブ用の新しいリソース グループを作成します。

    ```azurecli
    az group create --name <resource group name> --location <location>
    ```

1. 前の手順と同じリソース グループと場所を使用して、Event Hubs 名前空間を作成します。

    ```azurecli
    az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

1. 同じ Event Hubs 名前空間内のすべてのイベント ハブは、共通の接続資格情報を共有します。 そのイベント ハブを使用してメッセージを送受信するようにアプリケーションを構成する場合は、これらの資格情報が必要です。 以前と同じリソース グループと Event Hubs 名前空間名を使用して、Event Hubs 名前空間の接続文字列を取得するには、次のコマンドを使用します。

    ```azurecli
    az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

### <a name="configuring-a-new-event-hub"></a>新しいイベント ハブの構成

Event Hubs 名前空間を作成したら、イベント ハブを作成できます。 新しいイベント ハブを作成する場合、必須のパラメーターがいくつかあります。

イベント ハブを作成するには、次のパラメーターが必須です。

- **イベント ハブ名**: サブスクリプション内で一意のイベント ハブ名であり、以下の条件があります。
  - 1 から 50 文字で指定します
  - 使用できる文字は、英字、数字、ピリオド、ハイフン、アンダースコアのみです
  - 名前の先頭と末尾には英字または数字を使用します
- **パーティション数**: 1 つのイベント ハブに必要なパーティション数 (2 から 32 の間)。 これは、予想される同時コンシューマー数と直接関連する値です。 ハブの作成後はこの値を変更できません。 パーティションによってメッセージ ストリームは分離されます。そのため、コンシューマー アプリケーション (つまり受信側アプリケーション) はデータ ストリームの特定のサブセットのみを読み取る必要があります。 定義されていない場合、既定は *4* です。
- **メッセージの保持期間**: 何らかの理由でデータ ストリームを再生する必要がある場合、メッセージを利用可能なままにする日数 (1 から 7 の間)。 定義されていない場合、既定は *7* です。

必要に応じて、Azure Blob Storage または Azure Data Lake Store アカウントにデータをストリーミングするイベント ハブを構成することもできます。

### <a name="azure-cli-commands-for-creating-an-event-hub"></a>イベント ハブを作成するための Azure CLI コマンド

新しいイベント ハブを作成するには、次のコマンドを使用します。

1. 名前空間の作成時に使用したものと同じリソース グループと場所を使用して、イベント ハブを作成します。

    ```azurecli
    az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

1. 前と同じリソース グループとイベント ハブ名と名前空間名を使用して、名前空間内のイベント ハブの詳細を確認します。

    ```azurecli
    az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>

## Summary

To deploy Azure Event Hubs, you must configure an Event Hubs namespace and then configure the event hub itself. In the next section, you will go through the detailed configuration steps to create a new namespace and event hub.