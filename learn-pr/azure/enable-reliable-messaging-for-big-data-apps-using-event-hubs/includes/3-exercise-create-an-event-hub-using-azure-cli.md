<span data-ttu-id="2ada7-101">これで新しいイベント ハブを作成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="2ada7-101">You're now ready to create a new event hub.</span></span> <span data-ttu-id="2ada7-102">イベント ハブを作成したら、Azure Portal を使用して新しいハブを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-102">After creating the event hub, you'll use the Azure portal to view your new hub.</span></span>

<span data-ttu-id="2ada7-103">Azure CLI を使用してイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-103">You'll create an event hub using the Azure CLI.</span></span> <span data-ttu-id="2ada7-104">この演習では、Azure CLI 2.0 を使用します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-104">For this exercise, use the Azure CLI 2.0.</span></span> 

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="2ada7-105">Event Hubs 名前空間の作成</span><span class="sxs-lookup"><span data-stu-id="2ada7-105">Create an Event Hubs namespace</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="2ada7-106">次の手順に従い、Azure Cloud Shell によってサポートされている bash シェルを使用して、Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-106">Use the following steps to create an Event Hubs namespace using bash shell supported by Azure Cloud shell:</span></span>

1. <span data-ttu-id="2ada7-107">次のコマンドを使用して Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-107">Create the Event Hubs namespace using the following command:</span></span>

    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <rgn>[Sandbox resource group name]</rgn> -l <location>
    ```

    |<span data-ttu-id="2ada7-108">パラメーター</span><span class="sxs-lookup"><span data-stu-id="2ada7-108">Parameter</span></span>      |<span data-ttu-id="2ada7-109">説明</span><span class="sxs-lookup"><span data-stu-id="2ada7-109">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="2ada7-110">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-110">--name (required)</span></span>      |<span data-ttu-id="2ada7-111">Event Hubs 名前空間の一意の名前を、6 文字から 50 文字の長さで入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-111">Enter a 6-50 characters-long unique name for your Event Hubs namespace.</span></span> <span data-ttu-id="2ada7-112">名前に含めることができるのは、文字、数字、ハイフンのみです。</span><span class="sxs-lookup"><span data-stu-id="2ada7-112">The name should contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="2ada7-113">これは文字で始まり、文字または数字で終わる必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ada7-113">It should start with a letter and end with a letter or number.</span></span>|
    |<span data-ttu-id="2ada7-114">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-114">--resource-group (required)</span></span>  |<span data-ttu-id="2ada7-115">手順 1 で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-115">Enter the resource group you created in step 1.</span></span>
    |<span data-ttu-id="2ada7-116">--l (省略可能)</span><span class="sxs-lookup"><span data-stu-id="2ada7-116">--l (optional)</span></span>     |<span data-ttu-id="2ada7-117">最も近い Azure データセンターの場所を入力します (例: westus)。</span><span class="sxs-lookup"><span data-stu-id="2ada7-117">Enter the location of your nearest Azure datacenter, for example, westus.</span></span>|

1. <span data-ttu-id="2ada7-118">次のコマンドを使用して、Event Hubs 名前空間の接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-118">Fetch the connection string for your Event Hubs namespace using the following command.</span></span> <span data-ttu-id="2ada7-119">これは、イベント ハブを使用してメッセージを送受信するようにアプリケーションを構成するために必要となります。</span><span class="sxs-lookup"><span data-stu-id="2ada7-119">You'll need this to configure applications to send and receive messages using your event hub.</span></span>

    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <rgn>[Sandbox resource group name]</rgn> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```

    |<span data-ttu-id="2ada7-120">パラメーター</span><span class="sxs-lookup"><span data-stu-id="2ada7-120">Parameter</span></span>      |<span data-ttu-id="2ada7-121">説明</span><span class="sxs-lookup"><span data-stu-id="2ada7-121">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="2ada7-122">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-122">--resource-group (required)</span></span>  |<span data-ttu-id="2ada7-123">手順 1 で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-123">Enter the resource group you created in step 1.</span></span>|
    |<span data-ttu-id="2ada7-124">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-124">--namespace-name (required)</span></span>      |<span data-ttu-id="2ada7-125">手順 2 で作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-125">Enter the namespace you created in step 2.</span></span>|

    <span data-ttu-id="2ada7-126">このコマンドによって Event Hubs 名前空間の接続文字列が返されます。これは、パブリッシャーおよびコンシューマー アプリケーションを構成するために、後ほど使用します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-126">This command returns the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications.</span></span> <span data-ttu-id="2ada7-127">後で使用するために次のキーの値を保存します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-127">Save the value of the following keys for later use.</span></span>

    - <span data-ttu-id="2ada7-128">**primaryConnectionString**</span><span class="sxs-lookup"><span data-stu-id="2ada7-128">**primaryConnectionString**</span></span>
    - <span data-ttu-id="2ada7-129">**primaryKey**</span><span class="sxs-lookup"><span data-stu-id="2ada7-129">**primaryKey**</span></span>

## <a name="create-an-event-hub"></a><span data-ttu-id="2ada7-130">イベント ハブの作成</span><span class="sxs-lookup"><span data-stu-id="2ada7-130">Create an event hub</span></span>

<span data-ttu-id="2ada7-131">次の手順に従って新しいイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-131">Use the following steps to create your new event hub:</span></span>

1. <span data-ttu-id="2ada7-132">次のコマンドを使用して新しいイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-132">Create a new event hub using the following command:</span></span>

    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <rgn>[Sandbox resource group name]</rgn> --namespace-name <Event Hubs namespace name>
    ```

    |<span data-ttu-id="2ada7-133">パラメーター</span><span class="sxs-lookup"><span data-stu-id="2ada7-133">Parameter</span></span>      |<span data-ttu-id="2ada7-134">説明</span><span class="sxs-lookup"><span data-stu-id="2ada7-134">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="2ada7-135">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-135">--name (required)</span></span>  |<span data-ttu-id="2ada7-136">イベント ハブの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-136">Enter a name for your event hub.</span></span>|
    |<span data-ttu-id="2ada7-137">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-137">--resource-group (required)</span></span>  |<span data-ttu-id="2ada7-138">前の手順で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-138">Enter the resource group you created in the previous procedure.</span></span>|
    |<span data-ttu-id="2ada7-139">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-139">--namespace-name (required)</span></span>      |<span data-ttu-id="2ada7-140">前の手順で作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-140">Enter the namespace you created in the previous procedure.</span></span>|

1. <span data-ttu-id="2ada7-141">次のコードを使用して、イベント ハブの詳細を確認します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-141">View the details of your event hub using the following command:</span></span> 

    ```azurecli
        az eventhubs eventhub show --resource-group <rgn>[Sandbox resource group name]</rgn> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```

    |<span data-ttu-id="2ada7-142">パラメーター</span><span class="sxs-lookup"><span data-stu-id="2ada7-142">Parameter</span></span>      |<span data-ttu-id="2ada7-143">説明</span><span class="sxs-lookup"><span data-stu-id="2ada7-143">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="2ada7-144">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-144">--resource-group (required)</span></span>  |<span data-ttu-id="2ada7-145">前の手順で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-145">Enter the resource group that you created in the previous procedure.</span></span>|
    |<span data-ttu-id="2ada7-146">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-146">--namespace-name (required)</span></span>      |<span data-ttu-id="2ada7-147">前の手順で作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-147">Enter the namespace you created in the previous procedure.</span></span>|
    |<span data-ttu-id="2ada7-148">--name  (必須)</span><span class="sxs-lookup"><span data-stu-id="2ada7-148">--name  (required)</span></span>|<span data-ttu-id="2ada7-149">手順 1 で作成したイベント ハブの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-149">Enter the name of the event hub you created in step 1.</span></span>|

## <a name="view-the-event-hub-in-the-azure-portal"></a><span data-ttu-id="2ada7-150">Azure Portal でのイベント ハブの確認</span><span class="sxs-lookup"><span data-stu-id="2ada7-150">View the event hub in the Azure portal</span></span>

<span data-ttu-id="2ada7-151">次の手順を使用して、Azure Portal でイベント ハブを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-151">Use the following steps view your event hub in the Azure portal.</span></span>

1. <span data-ttu-id="2ada7-152">[Azure Portal](https://portal.azure.com?azure-portal=true) の上部にある検索バーを使用して、Event Hubs 名前空間を検索します。</span><span class="sxs-lookup"><span data-stu-id="2ada7-152">Find your Event Hubs namespace using the Search bar at the top of the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="2ada7-153">名前空間をクリックして開きます。</span><span class="sxs-lookup"><span data-stu-id="2ada7-153">Click your namespace to open it.</span></span>

1. <span data-ttu-id="2ada7-154">**[Event Hubs 名前空間]** > **[エンティティ]** から **[イベント ハブ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ada7-154">From **Event Hubs Namespace** > **ENTITIES**, click **Event Hubs**.</span></span>
    <span data-ttu-id="2ada7-155">イベント ハブが **[アクティブ]** の状態で表示され、また **[メッセージのリテンション期間]** の規定値 (*7*) と、**[パーティション数]** の規定値 (*4*) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ada7-155">Your event hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).</span></span>

    ![Azure Portal に表示されるイベント ハブ](../media-draft/3-event-hub.png)

## <a name="summary"></a><span data-ttu-id="2ada7-157">まとめ</span><span class="sxs-lookup"><span data-stu-id="2ada7-157">Summary</span></span>

<span data-ttu-id="2ada7-158">これで新しいイベント ハブが作成されました。また、パブリッシャーおよびコンシューマー アプリケーションを構成するために必要なすべての情報の準備が完了しました。</span><span class="sxs-lookup"><span data-stu-id="2ada7-158">You've now created a new event hub, and you've all the necessary information ready to configure your publisher and consumer applications.</span></span>
