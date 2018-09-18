<span data-ttu-id="de038-101">チームは、システム経由で到着するトランザクション量の増加に対応し、処理するために Azure Event Hubs の機能を利用することにしました。</span><span class="sxs-lookup"><span data-stu-id="de038-101">Your team has decided to leverage the capabilities of the Azure Event Hubs to manage and process the increasing transaction volumes coming through your system.</span></span>

<span data-ttu-id="de038-102">イベント ハブは Azure のリソースです。そこでまず、Azure で新しいハブを作成し、アプリケーションに固有の要件に合わせて構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="de038-102">An event hub is an Azure resource, so your first step is to create a new hub in Azure and configure it to meet the specific requirements of your applications.</span></span>

## <a name="what-is-an-azure-event-hub"></a><span data-ttu-id="de038-103">Azure のイベント ハブとは</span><span class="sxs-lookup"><span data-stu-id="de038-103">What is an Azure event hub?</span></span>

<span data-ttu-id="de038-104">Azure Event Hubs はクラウドベースのイベント処理サービスです。毎秒数百万のイベントを受け取って処理することができます。</span><span class="sxs-lookup"><span data-stu-id="de038-104">Azure Event Hubs is a cloud-based, event-processing service that's capable of receiving and processing millions of events per second.</span></span> <span data-ttu-id="de038-105">Event Hubs はイベント パイプラインの入り口として機能し、受信データを受け取り、処理リソースが使用できるようになるまでそのデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="de038-105">Event Hubs acts as a front door for an event pipeline, where it receives incoming data and stores it until processing resources are available.</span></span>

<span data-ttu-id="de038-106">Event Hubs にデータを送信するエンティティは、"*パブリッシャー*" と呼ばれます。Event Hubs からデータを読み取るエンティティは、"*コンシューマー*" または "*サブスクライバー*" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="de038-106">An entity that sends data to the Event Hubs is called a *publisher* and an entity that reads data from the Event Hubs is called a *consumer* or a *subscriber*.</span></span> <span data-ttu-id="de038-107">Azure Event Hubs は、これら 2 つのエンティティの間に配置され、(パブリッシャーによる) イベント ストリームの生成と (サブスクライバーによる) 利用を分離します。</span><span class="sxs-lookup"><span data-stu-id="de038-107">Azure Event Hubs sits between these two entities to divide the production (from publisher) and consumption (to subscriber) of an event stream.</span></span> <span data-ttu-id="de038-108">この分離によって、イベント生成速度がその利用をはるかに超えるシナリオにも対応しやすくなります。</span><span class="sxs-lookup"><span data-stu-id="de038-108">This decoupling helps to manage scenarios where the rate of event production is much higher than the consumption.</span></span> <span data-ttu-id="de038-109">次の図は、イベント ハブのロールを示しています。</span><span class="sxs-lookup"><span data-stu-id="de038-109">The following illustration shows the role of an event hub.</span></span>

![4 つのパブリッシャーと 2 つのサブスクライバーとの間に配置された Azure Event Hubs を示す図。](../media-draft/2-event-hub-overview.png)

### <a name="events"></a><span data-ttu-id="de038-112">イベント</span><span class="sxs-lookup"><span data-stu-id="de038-112">Events</span></span>

<span data-ttu-id="de038-113">**イベント**は、変更の通知を含む小さなパケットであって、厳密な変更内容の詳細ではありません。</span><span class="sxs-lookup"><span data-stu-id="de038-113">An **event** is a small packet that contains a notification of a change and not the details of what exactly has changed.</span></span> <span data-ttu-id="de038-114">どのような形であれ、コンシューマー アプリケーションの構成を通じてクエリを実行し、詳細を調べることはできません。これは要件ではなく、また、Event Hubs で処理するものでもありません。</span><span class="sxs-lookup"><span data-stu-id="de038-114">You can't configure a consumer application to initiate some form of query to find out the details, but this isn't a requirement and Event Hubs doesn't handle this.</span></span> <span data-ttu-id="de038-115">イベントは個別に、またはバッチとして発行できますが、1 回の発行 (個別またはバッチ) の上限は 256 KB です。</span><span class="sxs-lookup"><span data-stu-id="de038-115">Events can be published individually, or in batches, but a single publication (individual or batch) can't exceed 256 KB.</span></span>

<span data-ttu-id="de038-116">対照的に、(Azure Service Bus の) メッセージ キュー処理の場合、**メッセージ**にはデータとイベントが含まれており、メッセージのパブリッシャーはコンシューマーが特定の方法でメッセージを処理することを想定しています。</span><span class="sxs-lookup"><span data-stu-id="de038-116">On the contrary, in message queuing (in Azure Service Bus), the **message** contains data and the event, and the publisher of the message expects the consumer to process the message in a particular way.</span></span>

### <a name="publishers-and-subscribers"></a><span data-ttu-id="de038-117">パブリッシャーとサブスクライバー</span><span class="sxs-lookup"><span data-stu-id="de038-117">Publishers and subscribers</span></span>

<span data-ttu-id="de038-118">イベントのパブリッシャーとは、HTTPS または Advanced Message Queuing Protocol (AQMP) 1.0 を使用してイベント データを送信できる任意のアプリケーションまたはデバイスです。</span><span class="sxs-lookup"><span data-stu-id="de038-118">Event publishers are any application or device that can send out event data using either HTTPS or Advanced Message Queuing Protocol (AQMP) 1.0.</span></span> 

<span data-ttu-id="de038-119">データを頻繁に送信するパブリッシャーの場合、パフォーマンスの点で AMQP の方が優れています。</span><span class="sxs-lookup"><span data-stu-id="de038-119">For publishers that send data frequently, AMQP has the better performance.</span></span> <span data-ttu-id="de038-120">ただし、永続的な双方向ソケットとトランスポート層セキュリティ (TLS) または SSL/TLS を最初に設定する必要があるため、初回のセッションのオーバーヘッドは高くなります。</span><span class="sxs-lookup"><span data-stu-id="de038-120">However, it has a higher initial session overhead, because a persistent bidirectional socket and transport level security (TLS) or SSL/TLS has to be set up first.</span></span> 

<span data-ttu-id="de038-121">より断続的な発行の場合は、HTTPS を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="de038-121">For more intermittent publishing, HTTPS is the better option.</span></span> <span data-ttu-id="de038-122">HTTPS の場合、要求ごとに SSL の追加のオーバーヘッドが必要ですが、セッションの初期化のオーバーヘッドはありません。</span><span class="sxs-lookup"><span data-stu-id="de038-122">Though HTTPS requires additional SSL overhead for every request, there isn’t the session initialization overhead.</span></span>

> [!NOTE] 
> <span data-ttu-id="de038-123">既存の Kafka ベースのアプリケーションでは、Apache Kafka 1.0 以降のクライアント バージョンを使用しており、Event Hubs のパブリッシャーとしても機能します。</span><span class="sxs-lookup"><span data-stu-id="de038-123">Existing Kafka-based applications, using Apache Kafka 1.0 and newer client versions, can also act as Event Hubs publishers.</span></span>

<span data-ttu-id="de038-124">イベントのサブスクライバーは、サポートされている 2 つのプログラム方式のいずれかを使用して、イベント ハブからイベントを受信し、処理するアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="de038-124">Event subscribers are applications that use one of two supported programmatic methods to receive and process events from an event hub.</span></span>

- <span data-ttu-id="de038-125">**EventHubReceiver**: 使用できる管理オプションが限られている簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="de038-125">**EventHubReceiver** - A simple method that provides limited management options.</span></span>
- <span data-ttu-id="de038-126">**EventProcessorHost**: より効率的な方法です。このモジュールで後で使用します。</span><span class="sxs-lookup"><span data-stu-id="de038-126">**EventProcessorHost** - An efficient method that we’ll use later in this module.</span></span>

### <a name="consumer-groups"></a><span data-ttu-id="de038-127">コンシューマー グループ</span><span class="sxs-lookup"><span data-stu-id="de038-127">Consumer groups</span></span>

<span data-ttu-id="de038-128">イベント ハブの**コンシューマー グループ**は、イベント ハブ データ ストリームの特定のビューを表します。</span><span class="sxs-lookup"><span data-stu-id="de038-128">An event hub **consumer group** represents a specific view of an event hub data stream.</span></span> <span data-ttu-id="de038-129">別のコンシューマー グループを使用することで、複数のサブスクライバー アプリケーションが、他のアプリケーションに影響を与えることなく、独立してイベント ストリームを処理できます。</span><span class="sxs-lookup"><span data-stu-id="de038-129">By using separate consumer groups, multiple subscriber applications can process an event stream independently, and without affecting other applications.</span></span> <span data-ttu-id="de038-130">ただし、複数のコンシューマー グループの使用は必須ではありません。多くのアプリケーションでは、既定のコンシューマー グループが 1 つあれば十分です。</span><span class="sxs-lookup"><span data-stu-id="de038-130">However, the use of multiple consumer groups is not a requirement, and for many applications, the single default consumer group is sufficient.</span></span>

### <a name="pricing"></a><span data-ttu-id="de038-131">価格</span><span class="sxs-lookup"><span data-stu-id="de038-131">Pricing</span></span>

<span data-ttu-id="de038-132">Azure Event Hubs には、Basic、Standard、Dedicated という 3 つの価格レベルがあります。</span><span class="sxs-lookup"><span data-stu-id="de038-132">There are three pricing tiers for Azure Event Hubs: Basic, Standard, and Dedicated.</span></span> <span data-ttu-id="de038-133">各レベルには、サポートされる接続、使用できるコンシューマー グループの数、スループットの点で違いがあります。</span><span class="sxs-lookup"><span data-stu-id="de038-133">The tiers differ in terms of supported connections, number of available Consumer groups, and throughput.</span></span> <span data-ttu-id="de038-134">価格レベルを指定せずに、Azure CLI を使用して Event Hubs 名前空間を作成すると、既定の **Standard** (20 個のコンシューマー グループ、1000 個の仲介型接続) が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="de038-134">When using Azure CLI to create an Event Hubs Namespace, if you don't specify a pricing tier, the default of **Standard** (20 Consumer groups, 1000 Brokered connections) is assigned.</span></span>

## <a name="creating-and-configuring-a-new-azure-event-hubs"></a><span data-ttu-id="de038-135">新しい Azure Event Hubs の作成と構成</span><span class="sxs-lookup"><span data-stu-id="de038-135">Creating and configuring a new Azure Event Hubs</span></span>

<span data-ttu-id="de038-136">新しい Azure Event Hubs を作成および構成する場合、主に 2 つの手順があります。</span><span class="sxs-lookup"><span data-stu-id="de038-136">There are two main steps when creating and configuring a new Azure Event Hubs.</span></span> <span data-ttu-id="de038-137">最初の手順は、Event Hubs **名前空間**を定義することです。</span><span class="sxs-lookup"><span data-stu-id="de038-137">The first step is to define an Event Hubs **namespace**.</span></span> <span data-ttu-id="de038-138">2 つ目の手順は、その名前空間にイベント ハブを作成することです。</span><span class="sxs-lookup"><span data-stu-id="de038-138">The second step is to create an event hub in that namespace.</span></span>

### <a name="defining-an-event-hubs-namespace"></a><span data-ttu-id="de038-139">Event Hubs 名前空間を定義する</span><span class="sxs-lookup"><span data-stu-id="de038-139">Defining an Event Hubs namespace</span></span>

<span data-ttu-id="de038-140">Event Hubs 名前空間は、1 つまたは複数のイベント ハブを管理するエンティティです。</span><span class="sxs-lookup"><span data-stu-id="de038-140">An Event Hubs namespace is a containing entity for managing one or more Event Hubs.</span></span> 

<span data-ttu-id="de038-141">通常、Event Hubs 名前空間を作成するには、以下を実行します。</span><span class="sxs-lookup"><span data-stu-id="de038-141">Creating an Event Hubs namespace typically involves the following:</span></span>

1. <span data-ttu-id="de038-142">名前空間レベルの設定を定義します。</span><span class="sxs-lookup"><span data-stu-id="de038-142">Defining namespace-level settings.</span></span> <span data-ttu-id="de038-143">名前空間の容量 (**スループット ユニット**を使用して構成されます)、価格レベル、パフォーマンス メトリックなど、一部の設定は名前空間レベルで定義されます。</span><span class="sxs-lookup"><span data-stu-id="de038-143">Certain settings such as namespace capacity (configured using **throughput units**), pricing tier, and performance metrics are defined at the namespace level.</span></span> <span data-ttu-id="de038-144">これらは、その名前空間内のすべてのイベント ハブに適用されます。</span><span class="sxs-lookup"><span data-stu-id="de038-144">These are applicable for all the event hubs within that namespace.</span></span> <span data-ttu-id="de038-145">これらの設定を定義しない場合、既定値 (容量には *1*、価格レベルには *Standard*) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="de038-145">If you don't define these settings, a default value is used: *1* for capacity and *Standard* for pricing tier.</span></span>

    <span data-ttu-id="de038-146">一度設定したスループット ユニットは変更できません。</span><span class="sxs-lookup"><span data-stu-id="de038-146">You cannot change the throughput unit once you set it.</span></span> <span data-ttu-id="de038-147">Azure の予算見込みに合わせて構成を調整する必要があります。</span><span class="sxs-lookup"><span data-stu-id="de038-147">You must balance your configuration against your Azure budget expectations.</span></span> <span data-ttu-id="de038-148">さまざまなスループット要件に応じて異なるイベント ハブ構成を考慮する場合があります。</span><span class="sxs-lookup"><span data-stu-id="de038-148">You might consider configuring different event hubs for different throughput requirements.</span></span> <span data-ttu-id="de038-149">たとえば、売上データ アプリケーションがあり、2 つの Event Hubs を計画しているとします。1 つは高スループットなリアルタイム売上データ テレメトリの収集用、もう 1 つは頻度の低いイベント ログの収集用です。この場合、各ハブに別の名前空間を使用する方が合理的です。</span><span class="sxs-lookup"><span data-stu-id="de038-149">For example, if you have a sales data application and you are planning for two Event Hubs, one for high throughput collection of real-time sales data telemetry and one for infrequent event log collection, it would make sense to use a separate namespace for each hub.</span></span> <span data-ttu-id="de038-150">この方法なら、テレメトリ ハブに対して高スループットな容量を構成する (そして支払う) だけで済みます。</span><span class="sxs-lookup"><span data-stu-id="de038-150">This way you only need to configure (and pay for) high throughput capacity on the telemetry hub.</span></span>

1. <span data-ttu-id="de038-151">名前空間の一意の名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="de038-151">Selecting a unique name for the namespace.</span></span> <span data-ttu-id="de038-152">名前空間にアクセスするには、*_namespace_.servicebus.windows.net* という URL を使用します。</span><span class="sxs-lookup"><span data-stu-id="de038-152">The namespace is accessible through this url: *_namespace_.servicebus.windows.net*</span></span>

1. <span data-ttu-id="de038-153">次の省略可能なプロパティを定義します。</span><span class="sxs-lookup"><span data-stu-id="de038-153">Defining the following optional properties:</span></span>

    - <span data-ttu-id="de038-154">Kafka を有効にします。</span><span class="sxs-lookup"><span data-stu-id="de038-154">Enable Kafka.</span></span> <span data-ttu-id="de038-155">このオプションによって、Kafka アプリケーションからイベント ハブにイベントを公開できるようになります。</span><span class="sxs-lookup"><span data-stu-id="de038-155">This option enables Kafka applications to publish events to the event hub.</span></span>
    - <span data-ttu-id="de038-156">この名前空間をゾーン冗長にします。</span><span class="sxs-lookup"><span data-stu-id="de038-156">Make this namespace zone redundant.</span></span> <span data-ttu-id="de038-157">ゾーン冗長によって、独立した電力とネットワーク、冷却インフラストラクチャを独自に持つ個別のデータ センター間でデータがレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="de038-157">Zone-redundancy replicates data across separate data centers with their own independent power, networking, and cooling infrastructures.</span></span>
    - <span data-ttu-id="de038-158">自動インフレを有効にし、最大スループット ユニットを自動的に増やします。</span><span class="sxs-lookup"><span data-stu-id="de038-158">Enable Auto-Inflate, and Auto-Inflate Maximum Throughput Units.</span></span> <span data-ttu-id="de038-159">自動インフレは、スループット ユニット数を最大値まで増やすことで自動スケールアップ オプションを提供します。</span><span class="sxs-lookup"><span data-stu-id="de038-159">Auto-Inflate provides an automatic scale-up option, by increasing the number of throughput units up to a maximum value.</span></span> <span data-ttu-id="de038-160">これは、現在設定されているスループット ユニット数を受信または送信データ レートが超える場合に、スロットルを回避するために便利です。</span><span class="sxs-lookup"><span data-stu-id="de038-160">This is useful to avoid throttling in situations when incoming or outgoing data rates exceed the currently set number of throughput units.</span></span>

### <a name="azure-cli-commands-for-creating-an-event-hubs-namespace"></a><span data-ttu-id="de038-161">Event Hubs 名前空間を作成するための Azure CLI コマンド</span><span class="sxs-lookup"><span data-stu-id="de038-161">Azure CLI commands for creating an Event Hubs namespace</span></span>

<span data-ttu-id="de038-162">新しい Event Hubs 名前空間を作成するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="de038-162">To create a new Event Hubs namespace, you use the following commands:</span></span>

1. <span data-ttu-id="de038-163">Azure では、リソース グループは、管理を容易にするために関連する Azure リソースを保持するコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="de038-163">In Azure, a resource group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="de038-164">必要に応じて、イベント ハブ用の新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="de038-164">If required, create a new resource group for your event hub.</span></span>

    ```azurecli
    az group create --name <resource group name> --location <location>
    ```

1. <span data-ttu-id="de038-165">前の手順と同じリソース グループと場所を使用して、Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="de038-165">Create the Event Hubs Namespace, using the same resource group and location as in the previous step.</span></span>

    ```azurecli
    az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```

1. <span data-ttu-id="de038-166">同じ Event Hubs 名前空間内のすべてのイベント ハブは、共通の接続資格情報を共有します。</span><span class="sxs-lookup"><span data-stu-id="de038-166">All Event Hubs within the same Event Hubs namespace share common connection credentials.</span></span> <span data-ttu-id="de038-167">そのイベント ハブを使用してメッセージを送受信するようにアプリケーションを構成する場合は、これらの資格情報が必要です。</span><span class="sxs-lookup"><span data-stu-id="de038-167">You will need these credentials when you configure applications to send and receive messages using the event hub.</span></span> <span data-ttu-id="de038-168">以前と同じリソース グループと Event Hubs 名前空間名を使用して、Event Hubs 名前空間の接続文字列を取得するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="de038-168">Use the following command to return the connection string for your Event Hubs namespace, using the same resource group and Event Hubs namespace name as before.</span></span>

    ```azurecli
    az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

### <a name="configuring-a-new-event-hub"></a><span data-ttu-id="de038-169">新しいイベント ハブの構成</span><span class="sxs-lookup"><span data-stu-id="de038-169">Configuring a new event hub</span></span>

<span data-ttu-id="de038-170">Event Hubs 名前空間を作成したら、イベント ハブを作成できます。</span><span class="sxs-lookup"><span data-stu-id="de038-170">After the Event Hubs namespace has been created, you can create an event hub.</span></span> <span data-ttu-id="de038-171">新しいイベント ハブを作成する場合、必須のパラメーターがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="de038-171">When creating a new event hub, there are several mandatory parameters.</span></span>

<span data-ttu-id="de038-172">イベント ハブを作成するには、次のパラメーターが必須です。</span><span class="sxs-lookup"><span data-stu-id="de038-172">The following parameters are required to create an event hub:</span></span>

- <span data-ttu-id="de038-173">**イベント ハブ名**: サブスクリプション内で一意のイベント ハブ名であり、以下の条件があります。</span><span class="sxs-lookup"><span data-stu-id="de038-173">**Event hub name** - Event hub name that is unique within your subscription and:</span></span>
  - <span data-ttu-id="de038-174">1 から 50 文字で指定します</span><span class="sxs-lookup"><span data-stu-id="de038-174">Is between 1 and 50 characters long</span></span>
  - <span data-ttu-id="de038-175">使用できる文字は、英字、数字、ピリオド、ハイフン、アンダースコアのみです</span><span class="sxs-lookup"><span data-stu-id="de038-175">Contains only letters, numbers, periods, hyphens, and underscores</span></span>
  - <span data-ttu-id="de038-176">名前の先頭と末尾には英字または数字を使用します</span><span class="sxs-lookup"><span data-stu-id="de038-176">Starts and ends with a letter or number</span></span>
- <span data-ttu-id="de038-177">**パーティション数**: 1 つのイベント ハブに必要なパーティション数 (2 から 32 の間)。</span><span class="sxs-lookup"><span data-stu-id="de038-177">**Partition Count** -  The number of partitions required in an event hub (between 2 and 32).</span></span> <span data-ttu-id="de038-178">これは、予想される同時コンシューマー数と直接関連する値です。</span><span class="sxs-lookup"><span data-stu-id="de038-178">This should be directly related to the expected number of concurrent consumers.</span></span> <span data-ttu-id="de038-179">ハブの作成後はこの値を変更できません。</span><span class="sxs-lookup"><span data-stu-id="de038-179">This cannot be changed after the hub has been created.</span></span> <span data-ttu-id="de038-180">パーティションによってメッセージ ストリームは分離されます。そのため、コンシューマー アプリケーション (つまり受信側アプリケーション) はデータ ストリームの特定のサブセットのみを読み取る必要があります。</span><span class="sxs-lookup"><span data-stu-id="de038-180">The partition separates the message stream, so that consumer or receiver applications only need to read a specific subset of the data stream.</span></span> <span data-ttu-id="de038-181">定義されていない場合、既定は *4* です。</span><span class="sxs-lookup"><span data-stu-id="de038-181">If not defined, this defaults to *4*.</span></span>
- <span data-ttu-id="de038-182">**メッセージの保持期間**: 何らかの理由でデータ ストリームを再生する必要がある場合、メッセージを利用可能なままにする日数 (1 から 7 の間)。</span><span class="sxs-lookup"><span data-stu-id="de038-182">**Message Retention** - The number of days (between 1 and 7) that messages will remain available, if the data stream needs to be replayed for any reason.</span></span> <span data-ttu-id="de038-183">定義されていない場合、既定は *7* です。</span><span class="sxs-lookup"><span data-stu-id="de038-183">If not defined, this defaults to *7*.</span></span>

<span data-ttu-id="de038-184">必要に応じて、Azure Blob Storage または Azure Data Lake Store アカウントにデータをストリーム配信するイベント ハブを構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="de038-184">You can also optionally configure an event hub to stream data to an Azure Blob storage or Azure Data Lake Store account.</span></span>

### <a name="azure-cli-commands-for-creating-an-event-hub"></a><span data-ttu-id="de038-185">イベント ハブを作成するための Azure CLI コマンド</span><span class="sxs-lookup"><span data-stu-id="de038-185">Azure CLI commands for creating an event hub</span></span>

<span data-ttu-id="de038-186">新しいイベント ハブを作成するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="de038-186">To create a new event hub, you use the following commands:</span></span>

1. <span data-ttu-id="de038-187">名前空間の作成時に使用したものと同じリソース グループと場所を使用して、イベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="de038-187">Create the event hub, using the same resource group and location as you used when creating the namespace.</span></span>

    ```azurecli
    az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```

1. <span data-ttu-id="de038-188">前と同じリソース グループとイベント ハブ名と名前空間名を使用して、名前空間内のイベント ハブの詳細を確認します。</span><span class="sxs-lookup"><span data-stu-id="de038-188">View the details of your event hub in the namespace, using the same resource group and event hub name and namespace name as before.</span></span>

    ```azurecli
    az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>

## Summary

To deploy Azure Event Hubs, you must configure an Event Hubs namespace and then configure the event hub itself. In the next section, you will go through the detailed configuration steps to create a new namespace and event hub.