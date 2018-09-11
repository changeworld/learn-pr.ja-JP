<span data-ttu-id="1753c-101">チームは、システムに送信されるトランザクション量の増加を管理し、処理するために Azure Event Hubs の機能を利用することにしました。</span><span class="sxs-lookup"><span data-stu-id="1753c-101">Your team has decided to leverage the capabilities of the Azure Event Hubs to manage and process the increasing transaction volumes coming through your system.</span></span>

<span data-ttu-id="1753c-102">イベント ハブは Azure のリソースです。そのため、最初の手順は、Azure で新しいハブを作成し、アプリケーションに固有の要件に合わせて構成することです。</span><span class="sxs-lookup"><span data-stu-id="1753c-102">An event hub is an Azure resource, so your first step is to create a new hub in Azure and configure it to meet the specific requirements of your applications.</span></span>

## <a name="what-is-an-azure-event-hub"></a><span data-ttu-id="1753c-103">Azure のイベント ハブとは</span><span class="sxs-lookup"><span data-stu-id="1753c-103">What is an Azure event hub?</span></span>

<span data-ttu-id="1753c-104">Azure Event Hubs はクラウドベースのイベント処理サービスです。毎秒数百万のイベントを受け取って処理することができます。</span><span class="sxs-lookup"><span data-stu-id="1753c-104">Azure Event Hubs is a cloud-based, event-processing service that's capable of receiving and processing millions of events per second.</span></span> <span data-ttu-id="1753c-105">Event Hubs はイベント パイプラインの入り口として機能し、受信データを受け取り、処理リソースが使用できるようになるまでそのデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="1753c-105">Event Hubs acts as a front door for an event pipeline, where it receives incoming data and stores it until processing resources are available.</span></span>

<span data-ttu-id="1753c-106">Event Hubs にデータを送信するエンティティは、*パブリッシャー*と呼ばれます。Event Hubs からデータを読み取るエンティティは、*コンシューマー*または*サブスクライバー*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="1753c-106">An entity that sends data to the Event Hubs is called a *publisher* and an entity that reads data from the Event Hubs is called a *consumer* or a *subscriber*.</span></span> <span data-ttu-id="1753c-107">Azure Event Hubs は、これら 2 つのエンティティの間に配置され、(パブリッシャーからの) イベント ストリームの生成と (サブスクライバーへの) 消費を分離します。</span><span class="sxs-lookup"><span data-stu-id="1753c-107">Azure Event Hubs sits between these two entities to divide the production (from publisher) and consumption (to subscriber) of an event stream.</span></span> <span data-ttu-id="1753c-108">この分離によって、イベント生成速度が消費よりもはるかに速いシナリオを管理できます。</span><span class="sxs-lookup"><span data-stu-id="1753c-108">This decoupling helps to manage scenarios where the rate of event production is much higher than the consumption.</span></span>

<span data-ttu-id="1753c-109">![パブリッシャーは複数のイベントを 1 つのイベント ハブに送信し、サブスクライバーがデータ利用できるようにします](../media-draft/2-event-hub-overview.png "イベント ハブの概要")</span><span class="sxs-lookup"><span data-stu-id="1753c-109">![Publishers send multiple events to a single event hub, and make the data available to subscribers](../media-draft/2-event-hub-overview.png "event hub Overview")</span></span>

### <a name="events"></a><span data-ttu-id="1753c-110">イベント</span><span class="sxs-lookup"><span data-stu-id="1753c-110">Events</span></span>

<span data-ttu-id="1753c-111">**イベント**は、正確な変更内容の詳細ではなく、変更の通知を含む小さなパケットです。</span><span class="sxs-lookup"><span data-stu-id="1753c-111">An **event** is a small packet that contains a notification of a change and not the details of what exactly has changed.</span></span> <span data-ttu-id="1753c-112">詳細を確認する何らかの形式のクエリを開始するようにコンシューマー アプリケーションを構成することはできませんが、コンシューマー アプリケーションは必須ではなく、Event Hubs でも処理されません。</span><span class="sxs-lookup"><span data-stu-id="1753c-112">You can't configure a consumer application to initiate some form of query to find out the details, but this isn't a requirement and Event Hubs doesn't handle this.</span></span> <span data-ttu-id="1753c-113">イベントは個別に、またはバッチとして発行できますが、1 回の発行 (個別またはバッチ) の上限は 256 KB です。</span><span class="sxs-lookup"><span data-stu-id="1753c-113">Events can be published individually, or in batches, but a single publication (individual or batch) can't exceed 256 KB.</span></span>

<span data-ttu-id="1753c-114">対照的に、(Azure Service Bus の) メッセージ キュー処理の場合、**メッセージ**にはデータとイベントが含まれており、メッセージのパブリッシャーはコンシューマーが特定の方法でメッセージを処理することを期待しています。</span><span class="sxs-lookup"><span data-stu-id="1753c-114">On the contrary, in message queuing (in Azure Service Bus), the **message** contains data and the event, and the publisher of the message expects the consumer to process the message in a particular way.</span></span>

### <a name="publishers-and-subscribers"></a><span data-ttu-id="1753c-115">パブリッシャーとサブスクライバー</span><span class="sxs-lookup"><span data-stu-id="1753c-115">Publishers and subscribers</span></span>

<span data-ttu-id="1753c-116">イベントのパブリッシャーとは、HTTPS または Advanced Message Queuing Protocol (AQMP) 1.0 を使用してイベント データを送信できる任意のアプリケーションまたはデバイスです。</span><span class="sxs-lookup"><span data-stu-id="1753c-116">Event publishers are any application or device that can send out event data using either HTTPS or Advanced Message Queuing Protocol (AQMP) 1.0.</span></span> 

<span data-ttu-id="1753c-117">データを頻繁に送信するパブリッシャーの場合、パフォーマンスの点で AMQP の方が優れています。</span><span class="sxs-lookup"><span data-stu-id="1753c-117">For publishers that send data frequently, AMQP has the better performance.</span></span> <span data-ttu-id="1753c-118">ただし、永続的な双方向ソケットとトランスポート層セキュリティ (TLS) または SSL/TLS を最初に設定する必要があるため、初回のセッションのオーバーヘッドは高くなります。</span><span class="sxs-lookup"><span data-stu-id="1753c-118">However, it has a higher initial session overhead, because a persistent bidirectional socket and transport level security (TLS) or SSL/TLS has to be set up first.</span></span> 

<span data-ttu-id="1753c-119">より断続的な発行の場合は、HTTPS を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="1753c-119">For more intermittent publishing, HTTPS is the better option.</span></span> <span data-ttu-id="1753c-120">HTTPS の場合、要求ごとに SSL の追加のオーバーヘッドが必要ですが、セッションの初期化のオーバーヘッドはありません。</span><span class="sxs-lookup"><span data-stu-id="1753c-120">Though HTTPS requires additional SSL overhead for every request, there isn’t the session initialization overhead.</span></span>

> [!NOTE] 
> <span data-ttu-id="1753c-121">既存の Kafka ベースのアプリケーションでは、Apache Kafka 1.0 以降のクライアント バージョンを使用しており、Event Hubs のパブリッシャーとしても機能します。</span><span class="sxs-lookup"><span data-stu-id="1753c-121">Existing Kafka-based applications, using Apache Kafka 1.0 and newer client versions, can also act as Event Hubs publishers.</span></span>

<span data-ttu-id="1753c-122">イベントのサブスクライバーは、サポートされている 2 つのプログラム方式のいずれかを使用して、イベント ハブからイベントを受信し、処理するアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="1753c-122">Event subscribers are applications that use one of two supported programmatic methods to receive and process events from an event hub.</span></span>

- <span data-ttu-id="1753c-123">**EventHubReceiver**: 使用できる管理オプションが限られている簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="1753c-123">**EventHubReceiver** - A simple method that provides limited management options.</span></span>
- <span data-ttu-id="1753c-124">**EventProcessorHost**: より効率的な方法です。このモジュールで後で使用します。</span><span class="sxs-lookup"><span data-stu-id="1753c-124">**EventProcessorHost** - An efficient method that we’ll use later in this module.</span></span>

### <a name="consumer-groups"></a><span data-ttu-id="1753c-125">コンシューマー グループ</span><span class="sxs-lookup"><span data-stu-id="1753c-125">Consumer groups</span></span>

<span data-ttu-id="1753c-126">イベント ハブの**コンシューマー グループ**は、イベント ハブ データ ストリームの特定のビューを表します。</span><span class="sxs-lookup"><span data-stu-id="1753c-126">An event hub **consumer group** represents a specific view of an event hub data stream.</span></span> <span data-ttu-id="1753c-127">別のコンシューマー グループを使用することで、複数のサブスクライバー アプリケーションが、他のアプリケーションに影響を与えることなく、独立してイベント ストリームを処理できます。</span><span class="sxs-lookup"><span data-stu-id="1753c-127">By using separate consumer groups, multiple subscriber applications can process an event stream independently, and without affecting other applications.</span></span> <span data-ttu-id="1753c-128">ただし、複数のコンシューマー グループの使用は必須ではありません。多くのアプリケーションでは、既定のコンシューマー グループが 1 つあれば十分です。</span><span class="sxs-lookup"><span data-stu-id="1753c-128">However, the use of multiple consumer groups is not a requirement, and for many applications, the single default consumer group is sufficient.</span></span>

### <a name="pricing"></a><span data-ttu-id="1753c-129">価格</span><span class="sxs-lookup"><span data-stu-id="1753c-129">Pricing</span></span>

<span data-ttu-id="1753c-130">Azure Event Hubs には、Basic、Standard、Dedicated という 3 つの価格レベルがあります。</span><span class="sxs-lookup"><span data-stu-id="1753c-130">There are three pricing tiers for Azure Event Hubs: Basic, Standard, and Dedicated.</span></span> <span data-ttu-id="1753c-131">各レベルは、サポートされる接続、使用できるコンシューマー グループの数、およびスループットの点が異なります。</span><span class="sxs-lookup"><span data-stu-id="1753c-131">The tiers differ in terms of supported connections, number of available Consumer groups, and throughput.</span></span> <span data-ttu-id="1753c-132">価格レベルを指定せずに、Azure CLI を使用して Event Hubs 名前空間を作成すると、既定の **Standard** (20 個のコンシューマー グループ、1000 個の仲介型接続) が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="1753c-132">When using Azure CLI to create an Event Hubs Namespace, if you don't specify a pricing tier, the default of **Standard** (20 Consumer groups, 1000 Brokered connections) is assigned.</span></span>

## <a name="creating-and-configuring-a-new-azure-event-hubs"></a><span data-ttu-id="1753c-133">新しい Azure Event Hubs の作成と構成</span><span class="sxs-lookup"><span data-stu-id="1753c-133">Creating and configuring a new Azure Event Hubs</span></span>

<span data-ttu-id="1753c-134">新しい Azure Event Hubs を作成および構成する場合、主に 2 つの手順があります。</span><span class="sxs-lookup"><span data-stu-id="1753c-134">There are two main steps when creating and configuring a new Azure Event Hubs.</span></span> <span data-ttu-id="1753c-135">最初の手順は、Event Hubs **名前空間**を定義することです。</span><span class="sxs-lookup"><span data-stu-id="1753c-135">The first step is to define an Event Hubs **namespace**.</span></span> <span data-ttu-id="1753c-136">2 つ目の手順は、その名前空間にイベント ハブを作成することです。</span><span class="sxs-lookup"><span data-stu-id="1753c-136">The second step is to create an event hub in that namespace.</span></span>

### <a name="defining-an-event-hubs-namespace"></a><span data-ttu-id="1753c-137">Event Hubs 名前空間を定義する</span><span class="sxs-lookup"><span data-stu-id="1753c-137">Defining an Event Hubs namespace</span></span>

<span data-ttu-id="1753c-138">Event Hubs 名前空間は、1 つまたは複数のイベント ハブを管理するエンティティです。</span><span class="sxs-lookup"><span data-stu-id="1753c-138">An Event Hubs namespace is a containing entity for managing one or more Event Hubs.</span></span> 

<span data-ttu-id="1753c-139">通常、Event Hubs 名前空間を作成するには、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="1753c-139">Creating an Event Hubs namespace typically involves the following:</span></span>

1. <span data-ttu-id="1753c-140">名前空間レベルの設定を定義します。</span><span class="sxs-lookup"><span data-stu-id="1753c-140">Defining namespace-level settings.</span></span> <span data-ttu-id="1753c-141">名前空間の容量 (**スループット ユニット**を使用して構成されます)、価格レベル、パフォーマンス メトリックなど、一部の設定は名前空間レベルで定義されます。</span><span class="sxs-lookup"><span data-stu-id="1753c-141">Certain settings such as namespace capacity (configured using **throughput units**), pricing tier, and performance metrics are defined at the namespace level.</span></span> <span data-ttu-id="1753c-142">これらは、その名前空間内のすべてのイベント ハブに適用されます。</span><span class="sxs-lookup"><span data-stu-id="1753c-142">These are applicable for all the event hubs within that namespace.</span></span> <span data-ttu-id="1753c-143">これらの設定を定義しない場合、既定値 (容量には *1*、価格レベルには *Standard*) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1753c-143">If you don't define these settings, a default value is used: *1* for capacity and *Standard* for pricing tier.</span></span>

    <span data-ttu-id="1753c-144">一度設定したスループット ユニットは変更できません。</span><span class="sxs-lookup"><span data-stu-id="1753c-144">You cannot change the throughput unit once you set it.</span></span> <span data-ttu-id="1753c-145">Azure の予算見込みに合わせて構成を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1753c-145">You must balance your configuration against your Azure budget expectations.</span></span> <span data-ttu-id="1753c-146">さまざまなスループット要件に応じて異なるイベント ハブ構成を考慮する場合があります。</span><span class="sxs-lookup"><span data-stu-id="1753c-146">You might consider configuring different event hubs for different throughput requirements.</span></span> <span data-ttu-id="1753c-147">たとえば、売上データ アプリケーションがあり、2 つの Event Hubs を計画しているとします。1 つは高スループットなリアルタイム売上データ テレメトリの収集用、もう 1 つは頻度の低いイベント ログの収集用です。この場合、各ハブに別の名前空間を使用する方が合理的です。</span><span class="sxs-lookup"><span data-stu-id="1753c-147">For example, if you have a sales data application and you are planning for two Event Hubs, one for high throughput collection of real-time sales data telemetry and one for infrequent event log collection, it would make sense to use a separate namespace for each hub.</span></span> <span data-ttu-id="1753c-148">この方法なら、テレメトリ ハブに対して高スループットな容量を構成する (そして支払う) だけで済みます。</span><span class="sxs-lookup"><span data-stu-id="1753c-148">This way you only need to configure (and pay for) high throughput capacity on the telemetry hub.</span></span>

1. <span data-ttu-id="1753c-149">名前空間の一意の名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="1753c-149">Selecting a unique name for the namespace.</span></span> <span data-ttu-id="1753c-150">名前空間にアクセスするには、*_namespace_.servicebus.windows.net* という URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="1753c-150">The namespace is accessible through this url: *_namespace_.servicebus.windows.net*</span></span>

1. <span data-ttu-id="1753c-151">次の省略可能なプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="1753c-151">Defining the following optional properties:</span></span>

    - <span data-ttu-id="1753c-152">Kafka を有効にします。</span><span class="sxs-lookup"><span data-stu-id="1753c-152">Enable Kafka.</span></span> <span data-ttu-id="1753c-153">このオプションによって、Kafka アプリケーションからイベント ハブにイベントを公開できるようになります。</span><span class="sxs-lookup"><span data-stu-id="1753c-153">This option enables Kafka applications to publish events to the event hub.</span></span>
    - <span data-ttu-id="1753c-154">この名前空間をゾーン冗長にします。</span><span class="sxs-lookup"><span data-stu-id="1753c-154">Make this namespace zone redundant.</span></span> <span data-ttu-id="1753c-155">ゾーン冗長によって、独立した電力、ネットワーク、および冷却インフラストラクチャを独自に持つ個別のデータ センター間でデータがレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="1753c-155">Zone-redundancy replicates data across separate data centers with their own independent power, networking, and cooling infrastructures.</span></span>
    - <span data-ttu-id="1753c-156">自動インフレを有効にし、最大スループット ユニットを自動的に増やします。</span><span class="sxs-lookup"><span data-stu-id="1753c-156">Enable Auto-Inflate, and Auto-Inflate Maximum Throughput Units.</span></span> <span data-ttu-id="1753c-157">自動インフレは、スループット ユニット数を最大値まで増やすことで自動スケールアップ オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="1753c-157">Auto-Inflate provides an automatic scale-up option, by increasing the number of throughput units up to a maximum value.</span></span> <span data-ttu-id="1753c-158">これは、現在設定されているスループット ユニット数を受信または送信データ レートが超える場合に、スロットルを回避するために便利です。</span><span class="sxs-lookup"><span data-stu-id="1753c-158">This is useful to avoid throttling in situations when incoming or outgoing data rates exceed the currently set number of throughput units.</span></span>

### <a name="azure-cli-commands-for-creating-an-event-hubs-namespace"></a><span data-ttu-id="1753c-159">Event Hubs 名前空間を作成するための Azure CLI コマンド</span><span class="sxs-lookup"><span data-stu-id="1753c-159">Azure CLI commands for creating an Event Hubs namespace</span></span>

<span data-ttu-id="1753c-160">新しい Event Hubs 名前空間を作成するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="1753c-160">To create a new Event Hubs namespace, you use the following commands:</span></span>

1. <span data-ttu-id="1753c-161">Azure では、リソース グループは、管理を容易にするために関連する Azure リソースを保持するコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="1753c-161">In Azure, a resource group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="1753c-162">必要に応じて、イベント ハブ用の新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="1753c-162">If required, create a new resource group for your event hub.</span></span>

    ```azurecli
    az group create --name <resource group name> --location <location>
    ```

1. <span data-ttu-id="1753c-163">前の手順と同じリソース グループと場所を使用して、Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="1753c-163">Create the Event Hubs Namespace, using the same resource group and location as in the previous step.</span></span>

    ```azurecli
    az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

1. <span data-ttu-id="1753c-164">同じ Event Hubs 名前空間内のすべてのイベント ハブは、共通の接続資格情報を共有します。</span><span class="sxs-lookup"><span data-stu-id="1753c-164">All Event Hubs within the same Event Hubs namespace share common connection credentials.</span></span> <span data-ttu-id="1753c-165">そのイベント ハブを使用してメッセージを送受信するようにアプリケーションを構成する場合は、これらの資格情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="1753c-165">You will need these credentials when you configure applications to send and receive messages using the event hub.</span></span> <span data-ttu-id="1753c-166">次のコマンドを使用して、以前と同じリソース グループと Event Hubs 名前空間の名前を使用して、Event Hubs 名前空間の接続文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="1753c-166">Use the following command to return the connection string for your Event Hubs namespace, using the same resource group and Event Hubs namespace name as before.</span></span>

    ```azurecli
    az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

### <a name="configuring-a-new-event-hub"></a><span data-ttu-id="1753c-167">新しいイベント ハブの構成</span><span class="sxs-lookup"><span data-stu-id="1753c-167">Configuring a new event hub</span></span>

<span data-ttu-id="1753c-168">Event Hubs 名前空間を作成したら、イベント ハブを作成できます。</span><span class="sxs-lookup"><span data-stu-id="1753c-168">After the Event Hubs namespace has been created, you can create an event hub.</span></span> <span data-ttu-id="1753c-169">新しいイベント ハブを作成する場合、必須のパラメーターがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="1753c-169">When creating a new event hub, there are several mandatory parameters.</span></span>

<span data-ttu-id="1753c-170">イベント ハブを作成するには、次のパラメーターが必須です。</span><span class="sxs-lookup"><span data-stu-id="1753c-170">The following parameters are required to create an event hub:</span></span>

- <span data-ttu-id="1753c-171">**イベント ハブ名**: サブスクリプション内で一意のイベント ハブ名であり、以下の条件があります。</span><span class="sxs-lookup"><span data-stu-id="1753c-171">**Event hub name** - Event hub name that is unique within your subscription and:</span></span>
  - <span data-ttu-id="1753c-172">1 から 50 文字で指定します</span><span class="sxs-lookup"><span data-stu-id="1753c-172">Is between 1 and 50 characters long</span></span>
  - <span data-ttu-id="1753c-173">使用できる文字は、英字、数字、ピリオド、ハイフン、アンダースコアのみです</span><span class="sxs-lookup"><span data-stu-id="1753c-173">Contains only letters, numbers, periods, hyphens, and underscores</span></span>
  - <span data-ttu-id="1753c-174">名前の先頭と末尾には英字または数字を使用します</span><span class="sxs-lookup"><span data-stu-id="1753c-174">Starts and ends with a letter or number</span></span>
- <span data-ttu-id="1753c-175">**パーティション数**: 1 つのイベント ハブに必要なパーティション数 (2 から 32 の間)。</span><span class="sxs-lookup"><span data-stu-id="1753c-175">**Partition Count** -  The number of partitions required in an event hub (between 2 and 32).</span></span> <span data-ttu-id="1753c-176">これは、予想される同時コンシューマー数と直接関連する値です。</span><span class="sxs-lookup"><span data-stu-id="1753c-176">This should be directly related to the expected number of concurrent consumers.</span></span> <span data-ttu-id="1753c-177">ハブの作成後はこの値を変更できません。</span><span class="sxs-lookup"><span data-stu-id="1753c-177">This cannot be changed after the hub has been created.</span></span> <span data-ttu-id="1753c-178">パーティションによってメッセージ ストリームは分離されます。そのため、コンシューマー アプリケーション (つまり受信側アプリケーション) はデータ ストリームの特定のサブセットのみを読み取る必要があります。</span><span class="sxs-lookup"><span data-stu-id="1753c-178">The partition separates the message stream, so that consumer or receiver applications only need to read a specific subset of the data stream.</span></span> <span data-ttu-id="1753c-179">定義されていない場合、既定は *4* です。</span><span class="sxs-lookup"><span data-stu-id="1753c-179">If not defined, this defaults to *4*.</span></span>
- <span data-ttu-id="1753c-180">**メッセージの保持期間**: 何らかの理由でデータ ストリームを再生する必要がある場合、メッセージを利用可能なままにする日数 (1 から 7 の間)。</span><span class="sxs-lookup"><span data-stu-id="1753c-180">**Message Retention** - The number of days (between 1 and 7) that messages will remain available, if the data stream needs to be replayed for any reason.</span></span> <span data-ttu-id="1753c-181">定義されていない場合、既定は *7* です。</span><span class="sxs-lookup"><span data-stu-id="1753c-181">If not defined, this defaults to *7*.</span></span>

<span data-ttu-id="1753c-182">必要に応じて、Azure Blob Storage または Azure Data Lake Store アカウントにデータをストリーミングするイベント ハブを構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="1753c-182">You can also optionally configure an event hub to stream data to an Azure Blob storage or Azure Data Lake Store account.</span></span>

### <a name="azure-cli-commands-for-creating-an-event-hub"></a><span data-ttu-id="1753c-183">イベント ハブを作成するための Azure CLI コマンド</span><span class="sxs-lookup"><span data-stu-id="1753c-183">Azure CLI commands for creating an event hub</span></span>

<span data-ttu-id="1753c-184">新しいイベント ハブを作成するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="1753c-184">To create a new event hub, you use the following commands:</span></span>

1. <span data-ttu-id="1753c-185">名前空間の作成時に使用したものと同じリソース グループと場所を使用して、イベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="1753c-185">Create the event hub, using the same resource group and location as you used when creating the namespace.</span></span>

    ```azurecli
    az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

1. <span data-ttu-id="1753c-186">前と同じリソース グループとイベント ハブ名と名前空間名を使用して、名前空間内のイベント ハブの詳細を確認します。</span><span class="sxs-lookup"><span data-stu-id="1753c-186">View the details of your event hub in the namespace, using the same resource group and event hub name and namespace name as before.</span></span>

    ```azurecli
    az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>

## Summary

To deploy Azure Event Hubs, you must configure an Event Hubs namespace and then configure the event hub itself. In the next section, you will go through the detailed configuration steps to create a new namespace and event hub.