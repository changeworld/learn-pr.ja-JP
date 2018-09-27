<span data-ttu-id="319b9-101">グローバル企業の営業チーム向けアプリケーションがあるとします。</span><span class="sxs-lookup"><span data-stu-id="319b9-101">Suppose you have an application for the sales team in your global company.</span></span> <span data-ttu-id="319b9-102">各チーム メンバーは、アプリをインストールする携帯電話を持っています。</span><span class="sxs-lookup"><span data-stu-id="319b9-102">Each team member has a mobile phone where your app will be installed.</span></span> <span data-ttu-id="319b9-103">Azure でホストされている Web サービスには、アプリケーションのビジネス ロジックが実装され、情報は Azure SQL Database に格納されます。</span><span class="sxs-lookup"><span data-stu-id="319b9-103">A web service hosted in Azure implements the business logic for your application and stores information in Azure SQL Database.</span></span> <span data-ttu-id="319b9-104">地理的リージョンごとに 1 インスタンスの Web サービスがあります。</span><span class="sxs-lookup"><span data-stu-id="319b9-104">There is one instance of the web service for each geographical region.</span></span> <span data-ttu-id="319b9-105">次のように、モバイル アプリと Web サービスの間でメッセージを送信する目的を特定しました。</span><span class="sxs-lookup"><span data-stu-id="319b9-105">You have identified the following purposes for sending messages between the mobile app and the web service:</span></span>

- <span data-ttu-id="319b9-106">個々の販売に関連するメッセージは、ユーザーのリージョンの Web サービス インスタンスにのみ送信される必要があります。</span><span class="sxs-lookup"><span data-stu-id="319b9-106">Messages that relate to individual sales must be sent only to the web service instance in the user's region.</span></span>
- <span data-ttu-id="319b9-107">販売実績に関連するメッセージは、Web サービスのすべてのインスタンスに送信される必要があります。</span><span class="sxs-lookup"><span data-stu-id="319b9-107">Messages that relate to sales performance must be sent to all instances of the web service.</span></span>

<span data-ttu-id="319b9-108">1 つ目のユース ケースのために Service Bus キュー、2 つ目のユース ケースのために Service Bus トピックを実装することに決めました。</span><span class="sxs-lookup"><span data-stu-id="319b9-108">You have decided to implement a Service Bus queue for the first use case and the Service Bus topic for the second use case.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="319b9-109">この演習では、サブスクリプションに関するキューとトピックの両方を含む Service Bus 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="319b9-109">In this exercise, you will create a Service Bus namespace, which will contain both a queue and a topic with subscriptions.</span></span>

## <a name="create-a-service-bus-namespace"></a><span data-ttu-id="319b9-110">Service Bus 名前空間を作成する</span><span class="sxs-lookup"><span data-stu-id="319b9-110">Create a Service Bus namespace</span></span>

<span data-ttu-id="319b9-111">Azure Service Bus における名前空間とは、キュー、トピック、およびリレー用の一意の完全修飾ドメイン名を持つコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="319b9-111">In Azure Service Bus, a namespace is a container, with a unique fully qualified domain name, for queues, topics, and relays.</span></span> <span data-ttu-id="319b9-112">まず名前空間の作成から始める必要があります。</span><span class="sxs-lookup"><span data-stu-id="319b9-112">You must start by creating the namespace.</span></span>

<span data-ttu-id="319b9-113">各名前空間には、プライマリおよびセカンダリの Shared Access Signature 暗号化キーもあります。</span><span class="sxs-lookup"><span data-stu-id="319b9-113">Each namespace also has primary and secondary shared access signature encryption keys.</span></span> <span data-ttu-id="319b9-114">送信側または受信側のコンポーネントは、名前空間内のオブジェクトに対するアクセス権を得るために、接続時にこれらのキーを提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="319b9-114">A sending or receiving component must provide these keys when it connects to gain access to the objects within the namespace.</span></span>

<span data-ttu-id="319b9-115">Azure portal を使用して Service Bus 名前空間を作成するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="319b9-115">To create a Service Bus namespace by using the Azure portal, follow these steps:</span></span>

1. <span data-ttu-id="319b9-116">[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="319b9-116">Sign in to the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true).</span></span>

1. <span data-ttu-id="319b9-117">左側のナビゲーションで、**[すべてのサービス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-117">In the navigation on the left, click **All services**.</span></span>

1. <span data-ttu-id="319b9-118">**[すべてのサービス]** ブレードで、**[統合]** セクションまで下へスクロールし、**[Service Bus]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-118">In the **All Services** blade, scroll down to the **INTEGRATION** section, and then click **Service Bus**.</span></span>

    ![Service Bus 名前空間を作成する](../media/3-create-namespace-1.png)

1. <span data-ttu-id="319b9-120">**[Service Bus]** ブレードの左上にある **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-120">In the top left of the **Service Bus** blade, click **Add**.</span></span>

1. <span data-ttu-id="319b9-121">**[名前]** ボックスに、名前空間の一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="319b9-121">In the **Name** text box, type a unique name for the namespace.</span></span> <span data-ttu-id="319b9-122">たとえば、「salesteamapp」 + "*自分のイニシャル* + *現在の日付*" を入力します。</span><span class="sxs-lookup"><span data-stu-id="319b9-122">For example: "salesteamapp" + *your initials* + *current date*.</span></span>

1. <span data-ttu-id="319b9-123">**[価格レベル]** ドロップダウン リストで **[Standard]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="319b9-123">In the **Pricing tier** drop-down list, select **Standard**.</span></span>

1. <span data-ttu-id="319b9-124">**[サブスクリプション]** ドロップダウン リストで、該当するサブスクリプション ("コンシェルジェ サブスクリプション") を選択します。</span><span class="sxs-lookup"><span data-stu-id="319b9-124">In the **Subscription** drop-down list, select your subscription ("Concierge subscription").</span></span>

1. <span data-ttu-id="319b9-125">**[リソース グループ]** で **[既存のものを使用]** を選択し、"<rgn>[サンドボックス リソース グループ名]</rgn>" を選択します。</span><span class="sxs-lookup"><span data-stu-id="319b9-125">Under **Resource group**, select **Use existing** and choose "<rgn>[sandbox resource group name]</rgn>".</span></span>

1. <span data-ttu-id="319b9-126">**[場所]** ドロップダウン リストで、下記の一覧の最寄りの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="319b9-126">In the **Location** drop-down list, select a location near you from the below list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="319b9-127">**[作成]** をクリックして、Service Bus 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="319b9-127">Click **Create** to create the Service Bus namespace.</span></span>

    ![Service Bus 名前空間を作成する](../media/3-create-namespace-2.png)

## <a name="create-a-service-bus-queue"></a><span data-ttu-id="319b9-129">Service Bus キューを作成する</span><span class="sxs-lookup"><span data-stu-id="319b9-129">Create a Service Bus queue</span></span>

<span data-ttu-id="319b9-130">名前空間を用意できたので、次は個々の販売に関するメッセージのキューを作成します。</span><span class="sxs-lookup"><span data-stu-id="319b9-130">Now that you have a namespace, you can create a queue for messages about individual sales.</span></span> <span data-ttu-id="319b9-131">その手順は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="319b9-131">To do this, follow these steps:</span></span>

1. <span data-ttu-id="319b9-132">**[Service Bus]** ブレードで **[更新]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-132">In the **Service Bus** blade, click **Refresh**.</span></span> <span data-ttu-id="319b9-133">作成した名前空間が表示されます。</span><span class="sxs-lookup"><span data-stu-id="319b9-133">The namespace you just created is displayed.</span></span>

1. <span data-ttu-id="319b9-134">先ほど作成した名前空間をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-134">Click the namespace you just created.</span></span>

1. <span data-ttu-id="319b9-135">名前空間ブレードの左上にある **[+ キュー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-135">In the top left of the namespace blade, click **+ Queue**.</span></span>

1. <span data-ttu-id="319b9-136">**[キューの作成]** ブレードで、**[名前]** ボックスに「**salesmessages**」と入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-136">In the **Create queue** blade, in the **Name** text box, type **salesmessages**, and then click **Create**.</span></span> <span data-ttu-id="319b9-137">名前空間にキューが作成されます。</span><span class="sxs-lookup"><span data-stu-id="319b9-137">Azure creates the queue in your namespace.</span></span>

    ![キューの作成](../media/3-create-queue.png)

## <a name="create-a-service-bus-topic-and-subscriptions"></a><span data-ttu-id="319b9-139">Service Bus トピックとサブスクリプションを作成する</span><span class="sxs-lookup"><span data-stu-id="319b9-139">Create a Service Bus topic and subscriptions</span></span>

<span data-ttu-id="319b9-140">販売実績に関連するメッセージに使用されるトピックを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="319b9-140">You also want to create a topic that will be used for messages that relate to sales performance.</span></span> <span data-ttu-id="319b9-141">ビジネス ロジック Web サービスの複数のインスタンスが、さまざまな国からこのトピックをサブスクライブします。</span><span class="sxs-lookup"><span data-stu-id="319b9-141">Multiple instances of the business logic web service will subscribe to this topic from different countries.</span></span> <span data-ttu-id="319b9-142">各メッセージは複数のインスタンスに配信されます。</span><span class="sxs-lookup"><span data-stu-id="319b9-142">Each message will be delivered to multiple instances.</span></span>

<span data-ttu-id="319b9-143">次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="319b9-143">Follow these steps:</span></span>

1. <span data-ttu-id="319b9-144">**[Service Bus 名前空間]** ブレードで **[+ トピック]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-144">In the **Service Bus Namespace** blade, click **+ Topic**.</span></span>

1. <span data-ttu-id="319b9-145">**[トピックの作成]** ブレードで、**[名前]** ボックスに「**salesperformancemessages**」と入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-145">In the **Create topic** blade, in the **Name** text box, type **salesperformancemessages**, and then click **Create**.</span></span> <span data-ttu-id="319b9-146">名前空間にトピックが作成されます。</span><span class="sxs-lookup"><span data-stu-id="319b9-146">Azure creates the topic in your namespace.</span></span>

    ![トピックの作成](../media/3-create-topic.png)

1. <span data-ttu-id="319b9-148">トピックが作成されたら、**[Service Bus 名前空間]** ブレードの **[エンティティ]** で **[トピック]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-148">When the topic has been created, in the **Service Bus Namespace** blade, under **Entities**, click **Topics**.</span></span>

1. <span data-ttu-id="319b9-149">トピックのリストから **[salesperformancemessages]** をクリックし、**[+ サブスクリプション]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-149">In the list of topics, click **salesperformancemessages**, and then click **+ Subscription**.</span></span>

1. <span data-ttu-id="319b9-150">**[名前]** ボックスに「**Americas**」と入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-150">In the **Name** text box, type **Americas**, and then click **Create**.</span></span>

1. <span data-ttu-id="319b9-151">**[+ サブスクリプション]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-151">Click **+ Subscription**.</span></span>

1. <span data-ttu-id="319b9-152">**[名前]** ボックスに「**EuropeAndAfrica**」と入力し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="319b9-152">In the **Name** text box, type **EuropeAndAfrica**, and then click **Create**.</span></span>

<span data-ttu-id="319b9-153">営業部門の分散アプリケーションの回復力を高めるために、Service Bus の使用に必要なインフラストラクチャを構築しました。</span><span class="sxs-lookup"><span data-stu-id="319b9-153">You have built the infrastructure required to use Service Bus to increase the resilience of your sales force distributed application.</span></span> <span data-ttu-id="319b9-154">個々の販売に関するメッセージのキューと、販売実績に関するメッセージのトピックを作成しました。</span><span class="sxs-lookup"><span data-stu-id="319b9-154">You have created a queue for messages about individual sales and a topic for messages about sales performance.</span></span> <span data-ttu-id="319b9-155">トピックには複数のサブスクリプションが含まれています。これは、そのトピックに送信されたメッセージが、世界各地の複数の受信側 Web サービスに配信される可能性があるためです。</span><span class="sxs-lookup"><span data-stu-id="319b9-155">The topic includes multiple subscriptions because messages sent to that topic can be delivered to multiple recipient web services around the world.</span></span>
