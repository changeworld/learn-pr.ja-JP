<span data-ttu-id="46d71-101">これで新しいイベント ハブを作成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="46d71-101">You're now ready to create a new event hub.</span></span> <span data-ttu-id="46d71-102">イベント ハブを作成したら、Azure Portal を使用して新しいハブを確認します。</span><span class="sxs-lookup"><span data-stu-id="46d71-102">After creating the event hub, you'll use the Azure portal to view your new hub.</span></span>

<span data-ttu-id="46d71-103">Azure CLI を使用してイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="46d71-103">You'll create an event hub using the Azure CLI.</span></span> <span data-ttu-id="46d71-104">この演習では、Azure CLI 2.0 を使用します。</span><span class="sxs-lookup"><span data-stu-id="46d71-104">For this exercise, use the Azure CLI 2.0.</span></span> 

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="46d71-105">Event Hubs 名前空間の作成</span><span class="sxs-lookup"><span data-stu-id="46d71-105">Create an Event Hubs namespace</span></span>

<span data-ttu-id="46d71-106">次の手順に従い、Azure Cloud Shell によってサポートされている Bash シェルを使用して、Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="46d71-106">Use the following steps to create an Event Hubs namespace using Bash shell supported by Azure Cloud shell:</span></span>

1. <span data-ttu-id="46d71-107">Cloud Shell (Bash) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="46d71-107">Sign in to the Cloud Shell (Bash).</span></span>  

2. <span data-ttu-id="46d71-108">次のコマンドを使用して、Azure リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="46d71-108">Create an Azure resource group using the following command:</span></span>
    ```azurecli
        az group create --name <resource group name> --location <location>
    ```
    |<span data-ttu-id="46d71-109">パラメーター</span><span class="sxs-lookup"><span data-stu-id="46d71-109">Parameter</span></span>      |<span data-ttu-id="46d71-110">説明</span><span class="sxs-lookup"><span data-stu-id="46d71-110">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="46d71-111">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-111">--name (required)</span></span>      |<span data-ttu-id="46d71-112">新しいリソース グループの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-112">Enter a new resource group name.</span></span>|
    |<span data-ttu-id="46d71-113">--location (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-113">--location (required)</span></span>     |<span data-ttu-id="46d71-114">最も近い Azure データセンターの場所を入力します (例: westus)。</span><span class="sxs-lookup"><span data-stu-id="46d71-114">Enter the location of your nearest Azure datacenter, for example, westus.</span></span>|
3. <span data-ttu-id="46d71-115">次のコマンドを使用して Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="46d71-115">Create the Event Hubs namespace using the following command:</span></span>
    ```azurecli
        az eventhubs namespace create --name <Event Hubs namespace name> --resource-group <resource group name> -l <location>
    ```
    |<span data-ttu-id="46d71-116">パラメーター</span><span class="sxs-lookup"><span data-stu-id="46d71-116">Parameter</span></span>      |<span data-ttu-id="46d71-117">説明</span><span class="sxs-lookup"><span data-stu-id="46d71-117">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="46d71-118">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-118">--name (required)</span></span>      |<span data-ttu-id="46d71-119">Event Hubs 名前空間の一意の名前を、6 文字から 50 文字の長さで入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-119">Enter a 6-50 characters-long unique name for your Event Hubs namespace.</span></span> <span data-ttu-id="46d71-120">名前に含めることができるのは、文字、数字、ハイフンのみです。</span><span class="sxs-lookup"><span data-stu-id="46d71-120">The name should contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="46d71-121">これは文字で始まり、文字または数字で終わる必要があります。</span><span class="sxs-lookup"><span data-stu-id="46d71-121">It should start with a letter and end with a letter or number.</span></span>|
    |<span data-ttu-id="46d71-122">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-122">--resource-group (required)</span></span>  |<span data-ttu-id="46d71-123">手順 1 で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-123">Enter the resource group you created in step 1.</span></span>
    |<span data-ttu-id="46d71-124">--l (省略可能)</span><span class="sxs-lookup"><span data-stu-id="46d71-124">--l (optional)</span></span>     |<span data-ttu-id="46d71-125">最も近い Azure データセンターの場所を入力します (例: westus)。</span><span class="sxs-lookup"><span data-stu-id="46d71-125">Enter the location of your nearest Azure datacenter, for example, westus.</span></span>|
4. <span data-ttu-id="46d71-126">次のコマンドを使用して、Event Hubs 名前空間の接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="46d71-126">Fetch the connection string for your Event Hubs namespace using the following command.</span></span> <span data-ttu-id="46d71-127">これは、イベント ハブを使用してメッセージを送受信するようにアプリケーションを構成するために必要となります。</span><span class="sxs-lookup"><span data-stu-id="46d71-127">You'll need this to configure applications to send and receive messages using your event hub.</span></span>
    ```azurecli
        az eventhubs namespace authorization-rule keys list --resource-group <resource group name> --namespace-name <EventHub namespace name> --name RootManageSharedAccessKey
    ```
    |<span data-ttu-id="46d71-128">パラメーター</span><span class="sxs-lookup"><span data-stu-id="46d71-128">Parameter</span></span>      |<span data-ttu-id="46d71-129">説明</span><span class="sxs-lookup"><span data-stu-id="46d71-129">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="46d71-130">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-130">--resource-group (required)</span></span>  |<span data-ttu-id="46d71-131">手順 1 で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-131">Enter the resource group you created in step 1.</span></span>|
    |<span data-ttu-id="46d71-132">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-132">--namespace-name (required)</span></span>      |<span data-ttu-id="46d71-133">手順 2 で作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-133">Enter the namespace you created in step 2.</span></span>|

    <span data-ttu-id="46d71-134">このコマンドによって Event Hubs 名前空間の接続文字列が返されます。これは、パブリッシャーおよびコンシューマー アプリケーションを構成するために、後ほど使用します。</span><span class="sxs-lookup"><span data-stu-id="46d71-134">This command returns the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications.</span></span> <span data-ttu-id="46d71-135">後で使用するために次のキーの値を保存します。</span><span class="sxs-lookup"><span data-stu-id="46d71-135">Save the value of the following keys for later use.</span></span>
    - <span data-ttu-id="46d71-136">**primaryConnectionString**</span><span class="sxs-lookup"><span data-stu-id="46d71-136">**primaryConnectionString**</span></span>
    - <span data-ttu-id="46d71-137">**primaryKey**</span><span class="sxs-lookup"><span data-stu-id="46d71-137">**primaryKey**</span></span>

## <a name="create-an-event-hub"></a><span data-ttu-id="46d71-138">イベント ハブの作成</span><span class="sxs-lookup"><span data-stu-id="46d71-138">Create an event hub</span></span>

<span data-ttu-id="46d71-139">次の手順に従って新しいイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="46d71-139">Use the following steps to create your new event hub:</span></span>

1. <span data-ttu-id="46d71-140">次のコマンドを使用して新しいイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="46d71-140">Create a new event hub using the following command:</span></span>
    ```azurecli
        az eventhubs eventhub create --name <event hub name> --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name>
    ```
    |<span data-ttu-id="46d71-141">パラメーター</span><span class="sxs-lookup"><span data-stu-id="46d71-141">Parameter</span></span>      |<span data-ttu-id="46d71-142">説明</span><span class="sxs-lookup"><span data-stu-id="46d71-142">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="46d71-143">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-143">--name (required)</span></span>  |<span data-ttu-id="46d71-144">イベント ハブの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-144">Enter a name for your event hub.</span></span>|
    |<span data-ttu-id="46d71-145">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-145">--resource-group (required)</span></span>  |<span data-ttu-id="46d71-146">前の手順で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-146">Enter the resource group you created in the previous procedure.</span></span>|
    |<span data-ttu-id="46d71-147">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-147">--namespace-name (required)</span></span>      |<span data-ttu-id="46d71-148">前の手順で作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-148">Enter the namespace you created in the previous procedure.</span></span>|
2. <span data-ttu-id="46d71-149">次のコードを使用して、イベント ハブの詳細を確認します。</span><span class="sxs-lookup"><span data-stu-id="46d71-149">View the details of your event hub using the following command:</span></span> 
    ```azurecli
        az eventhubs eventhub show --resource-group <Resource Group name> --namespace-name <Event Hubs namespace name> --name <event hub name>
    ```
    |<span data-ttu-id="46d71-150">パラメーター</span><span class="sxs-lookup"><span data-stu-id="46d71-150">Parameter</span></span>      |<span data-ttu-id="46d71-151">説明</span><span class="sxs-lookup"><span data-stu-id="46d71-151">Description</span></span>|
    |---------------|-----------|
    |<span data-ttu-id="46d71-152">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-152">--resource-group (required)</span></span>  |<span data-ttu-id="46d71-153">前の手順で作成したリソース グループを入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-153">Enter the resource group that you created in the previous procedure.</span></span>|
    |<span data-ttu-id="46d71-154">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-154">--namespace-name (required)</span></span>      |<span data-ttu-id="46d71-155">前の手順で作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-155">Enter the namespace you created in the previous procedure.</span></span>|
    |<span data-ttu-id="46d71-156">--name  (必須)</span><span class="sxs-lookup"><span data-stu-id="46d71-156">--name  (required)</span></span>|<span data-ttu-id="46d71-157">手順 1 で作成したイベント ハブの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="46d71-157">Enter the name of the event hub you created in step 1.</span></span>|

## <a name="view-the-event-hub-in-the-azure-portal"></a><span data-ttu-id="46d71-158">Azure Portal でのイベント ハブの確認</span><span class="sxs-lookup"><span data-stu-id="46d71-158">View the event hub in the Azure portal</span></span>

<span data-ttu-id="46d71-159">次の手順を使用して、Azure Portal でイベント ハブを確認します。</span><span class="sxs-lookup"><span data-stu-id="46d71-159">Use the following steps view your event hub in the Azure portal.</span></span>

1. <span data-ttu-id="46d71-160">[Azure Portal](https://portal.azure.com?azure-portal=true) の上部にある検索バーを使用して、Event Hubs 名前空間を検索します。</span><span class="sxs-lookup"><span data-stu-id="46d71-160">Find your Event Hubs namespace using the Search bar at the top of the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>
1. <span data-ttu-id="46d71-161">名前空間をクリックして開きます。</span><span class="sxs-lookup"><span data-stu-id="46d71-161">Click your namespace to open it.</span></span>
1. <span data-ttu-id="46d71-162">**[Event Hubs 名前空間]** > **[エンティティ]** から **[イベント ハブ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="46d71-162">From **Event Hubs Namespace** > **ENTITIES**, click **Event Hubs**.</span></span>
    <span data-ttu-id="46d71-163">イベント ハブが **[アクティブ]** の状態で表示され、また **[メッセージのリテンション期間]** の規定値 (*7*) と、**[パーティション数]** の規定値 (*4*) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="46d71-163">Your event hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).</span></span>

    ![Azure Portal に表示されるイベント ハブ](../media-draft/3-event-hub.png)

## <a name="summary"></a><span data-ttu-id="46d71-165">まとめ</span><span class="sxs-lookup"><span data-stu-id="46d71-165">Summary</span></span>

<span data-ttu-id="46d71-166">これで新しいイベント ハブが作成されました。また、パブリッシャーおよびコンシューマー アプリケーションを構成するために必要なすべての情報の準備が完了しました。</span><span class="sxs-lookup"><span data-stu-id="46d71-166">You've now created a new event hub, and you've all the necessary information ready to configure your publisher and consumer applications.</span></span>
