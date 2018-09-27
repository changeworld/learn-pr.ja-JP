<span data-ttu-id="090bf-101">これで新しいイベント ハブを作成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="090bf-101">You're now ready to create a new Event Hub.</span></span> <span data-ttu-id="090bf-102">イベント ハブを作成したら、Azure portal を使用してご自分の新しいハブを確認します。</span><span class="sxs-lookup"><span data-stu-id="090bf-102">After creating the Event Hub, you'll use the Azure portal to view your new hub.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="set-some-defaults-in-the-azure-cli"></a><span data-ttu-id="090bf-103">Azure CLI でいくつかの既定値を設定する</span><span class="sxs-lookup"><span data-stu-id="090bf-103">Set some defaults in the Azure CLI</span></span>

<span data-ttu-id="090bf-104">Cloud Shell で Azure CLI 用の既定値をいくつか指定することから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="090bf-104">Let's start by providing some default values for the Azure CLI in the Cloud Shell.</span></span> <span data-ttu-id="090bf-105">これで、毎回、それらの値を入力しなくてもよくなります。</span><span class="sxs-lookup"><span data-stu-id="090bf-105">This will keep you from having to type these in every time.</span></span> <span data-ttu-id="090bf-106">具体的に、_リソース グループ_と_場所_を設定してみましょう。</span><span class="sxs-lookup"><span data-stu-id="090bf-106">In particular, let's set the _resource group_ and _location_.</span></span> <span data-ttu-id="090bf-107">次の一覧から場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="090bf-107">Select a location from the following list.</span></span>

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="090bf-108">さらに Azure CLI に次のコマンドを入力し、場所を必ず、自分に近い場所に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="090bf-108">Then type the following command into the Azure CLI, make sure to replace the location with one close to you.</span></span>

```azurecli
az configure --defaults group=<rgn>[sandbox Resource Group]</rgn> location=westus2
```

## <a name="create-an-event-hubs-namespace"></a><span data-ttu-id="090bf-109">Event Hubs 名前空間を作成する</span><span class="sxs-lookup"><span data-stu-id="090bf-109">Create an Event Hubs namespace</span></span>

<span data-ttu-id="090bf-110">次の手順に従い、Azure Cloud Shell によってサポートされている bash シェルを使用して、Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="090bf-110">Use the following steps to create an Event Hubs namespace using bash shell supported by Azure Cloud shell:</span></span>

1. <span data-ttu-id="090bf-111">`az eventhubs namespace create` コマンドを使用して Event Hubs 名前空間を作成します。</span><span class="sxs-lookup"><span data-stu-id="090bf-111">Create the Event Hubs namespace using the `az eventhubs namespace create` command.</span></span> <span data-ttu-id="090bf-112">次のパラメーターを使用します。</span><span class="sxs-lookup"><span data-stu-id="090bf-112">Use the following parameters.</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="090bf-113">パラメーター</span><span class="sxs-lookup"><span data-stu-id="090bf-113">Parameter</span></span>      |<span data-ttu-id="090bf-114">説明</span><span class="sxs-lookup"><span data-stu-id="090bf-114">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="090bf-115">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-115">--name (required)</span></span>      |<span data-ttu-id="090bf-116">Event Hubs 名前空間の一意の名前を、6 文字から 50 文字の長さで入力します。</span><span class="sxs-lookup"><span data-stu-id="090bf-116">Enter a 6-50 characters-long unique name for your Event Hubs namespace.</span></span> <span data-ttu-id="090bf-117">名前に含めることができるのは、文字、数字、ハイフンのみです。</span><span class="sxs-lookup"><span data-stu-id="090bf-117">The name should contain only letters, numbers, and hyphens.</span></span> <span data-ttu-id="090bf-118">これは文字で始まり、文字または数字で終わる必要があります。</span><span class="sxs-lookup"><span data-stu-id="090bf-118">It should start with a letter and end with a letter or number.</span></span>|
    > |<span data-ttu-id="090bf-119">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-119">--resource-group (required)</span></span> | <span data-ttu-id="090bf-120">これは、既定値から指定される、事前に作成された Azure サンド ボックス リソース グループとなります。</span><span class="sxs-lookup"><span data-stu-id="090bf-120">This will be the pre-created Azure sandbox resource group supplied from the defaults.</span></span> |
    > |<span data-ttu-id="090bf-121">--l (省略可能)</span><span class="sxs-lookup"><span data-stu-id="090bf-121">--l (optional)</span></span>     |<span data-ttu-id="090bf-122">最も近くにある Azure データセンターの場所を入力します。ご利用の既定値が使用されます。</span><span class="sxs-lookup"><span data-stu-id="090bf-122">Enter the location of your nearest Azure datacenter, this will use your default.</span></span>|
    > |<span data-ttu-id="090bf-123">--sku (省略可能)</span><span class="sxs-lookup"><span data-stu-id="090bf-123">--sku (optional)</span></span> | <span data-ttu-id="090bf-124">名前空間 [Basic </span><span class="sxs-lookup"><span data-stu-id="090bf-124">The pricing tier for the namespace [Basic</span></span> | <span data-ttu-id="090bf-125">Standard] の価格レベルです。既定値は _Standard_ です。</span><span class="sxs-lookup"><span data-stu-id="090bf-125">Standard], defaults to _Standard_.</span></span> <span data-ttu-id="090bf-126">これにより、接続とコンシューマーしきい値が決定されます。</span><span class="sxs-lookup"><span data-stu-id="090bf-126">This determines the connections and consumer thresholds.</span></span> |

    <span data-ttu-id="090bf-127">再利用できるように環境変数に名前を設定します。</span><span class="sxs-lookup"><span data-stu-id="090bf-127">Set the name into an environment variable so we can reuse it.</span></span>

    ```azurecli
    NS_NAME=myEvt-HubNs1
    ````

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```azurecli
    az eventhubs namespace create --name $NS_NAME
    ```

    > [!NOTE]
    > <span data-ttu-id="090bf-128">Azure では名前については非常に厳しくチェックされ、名前が既に存在しているか、または無効である場合、CLI から **[無効な要求]** が返されます。</span><span class="sxs-lookup"><span data-stu-id="090bf-128">Azure is very picky about the name and the CLI returns **Bad Request** if the name exists or is invalid.</span></span> <span data-ttu-id="090bf-129">別の名前を試してみるには、ご利用の環境変数を変更して、コマンドを再発行します。</span><span class="sxs-lookup"><span data-stu-id="090bf-129">Try a different name by changing your environment variable and reissuing the command.</span></span>


1. <span data-ttu-id="090bf-130">次のコマンドを使用して、ご利用の Event Hubs 名前空間の接続文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="090bf-130">Fetch the connection string for your Event Hubs namespace using the following command.</span></span> <span data-ttu-id="090bf-131">これは、ご利用のイベント ハブを使用してメッセージを送受信するようにアプリケーションを構成するために必要となります。</span><span class="sxs-lookup"><span data-stu-id="090bf-131">You'll need this to configure applications to send and receive messages using your Event Hub.</span></span>

    ```azurecli
    az eventhubs namespace authorization-rule keys list --name RootManageSharedAccessKey --namespace-name $NS_NAME
    ```

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="090bf-132">パラメーター</span><span class="sxs-lookup"><span data-stu-id="090bf-132">Parameter</span></span>      |<span data-ttu-id="090bf-133">説明</span><span class="sxs-lookup"><span data-stu-id="090bf-133">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="090bf-134">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-134">--resource-group (required)</span></span>  | <span data-ttu-id="090bf-135">これは、既定値から指定される、事前に作成された Azure サンド ボックス リソース グループとなります。</span><span class="sxs-lookup"><span data-stu-id="090bf-135">This will be the pre-created Azure sandbox resource group supplied from the defaults.</span></span> |
    > |<span data-ttu-id="090bf-136">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-136">--namespace-name (required)</span></span>  | <span data-ttu-id="090bf-137">作成した名前空間の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="090bf-137">Enter the name of the namespace you created.</span></span> |

    <span data-ttu-id="090bf-138">このコマンドでは、ご利用の Event Hubs 名前空間の接続文字列を含む JSON ブロックが返されます。これは、パブリッシャーおよびコンシューマー アプリケーションを構成するために、後ほど使用します。</span><span class="sxs-lookup"><span data-stu-id="090bf-138">This command returns a JSON block with the connection string for your Event Hubs namespace that you'll use later to configure your publisher and consumer applications.</span></span> <span data-ttu-id="090bf-139">後で使用するために次のキーの値を保存します。</span><span class="sxs-lookup"><span data-stu-id="090bf-139">Save the value of the following keys for later use.</span></span>

    - <span data-ttu-id="090bf-140">**primaryConnectionString**</span><span class="sxs-lookup"><span data-stu-id="090bf-140">**primaryConnectionString**</span></span>
    - <span data-ttu-id="090bf-141">**primaryKey**</span><span class="sxs-lookup"><span data-stu-id="090bf-141">**primaryKey**</span></span>

## <a name="create-an-event-hub"></a><span data-ttu-id="090bf-142">イベント ハブを作成する</span><span class="sxs-lookup"><span data-stu-id="090bf-142">Create an Event Hub</span></span>

<span data-ttu-id="090bf-143">次の手順に従って新しいイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="090bf-143">Use the following steps to create your new Event Hub:</span></span>

1. <span data-ttu-id="090bf-144">`eventhub create` コマンドを使用して新しいイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="090bf-144">Create a new Event Hub using the `eventhub create` command.</span></span> <span data-ttu-id="090bf-145">必要なパラメーターは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="090bf-145">It needs the following parameters:</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="090bf-146">パラメーター</span><span class="sxs-lookup"><span data-stu-id="090bf-146">Parameter</span></span>      |<span data-ttu-id="090bf-147">説明</span><span class="sxs-lookup"><span data-stu-id="090bf-147">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="090bf-148">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-148">--name (required)</span></span>  |<span data-ttu-id="090bf-149">イベント ハブの名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="090bf-149">Enter a name for your Event Hub.</span></span>|
    > |<span data-ttu-id="090bf-150">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-150">--resource-group (required)</span></span>  |<span data-ttu-id="090bf-151">リソース グループの所有者です。</span><span class="sxs-lookup"><span data-stu-id="090bf-151">Resource group owner.</span></span>|
    > |<span data-ttu-id="090bf-152">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-152">--namespace-name (required)</span></span>      |<span data-ttu-id="090bf-153">作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="090bf-153">Enter the namespace you created.</span></span>|

    <span data-ttu-id="090bf-154">まず、環境変数にイベント ハブの名前を定義してみましょう。</span><span class="sxs-lookup"><span data-stu-id="090bf-154">Let's define the Event Hub name in an environment variable first.</span></span>

    ```azurecli
    HUB_NAME=[name]
    ```

    ```azurecli
    az eventhubs eventhub create --name $HUB_NAME --namespace-name $NS_NAME
    ```

1. <span data-ttu-id="090bf-155">`eventhub show` を使用して、イベント ハブの詳細を確認します。</span><span class="sxs-lookup"><span data-stu-id="090bf-155">View the details of your Event Hub using the `eventhub show` command.</span></span> <span data-ttu-id="090bf-156">使用されるのは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="090bf-156">It takes:</span></span>

    > [!div class="mx-tableFixed"]
    > |<span data-ttu-id="090bf-157">パラメーター</span><span class="sxs-lookup"><span data-stu-id="090bf-157">Parameter</span></span>      |<span data-ttu-id="090bf-158">説明</span><span class="sxs-lookup"><span data-stu-id="090bf-158">Description</span></span>|
    > |---------------|-----------|
    > |<span data-ttu-id="090bf-159">--resource-group (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-159">--resource-group (required)</span></span>  |<span data-ttu-id="090bf-160">リソース グループの所有者です。</span><span class="sxs-lookup"><span data-stu-id="090bf-160">Resource group owner.</span></span>|
    > |<span data-ttu-id="090bf-161">--namespace-name (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-161">--namespace-name (required)</span></span>      |<span data-ttu-id="090bf-162">作成した名前空間を入力します。</span><span class="sxs-lookup"><span data-stu-id="090bf-162">Enter the namespace you created.</span></span>|
    > |<span data-ttu-id="090bf-163">--name (必須)</span><span class="sxs-lookup"><span data-stu-id="090bf-163">--name  (required)</span></span>|<span data-ttu-id="090bf-164">イベント ハブの名前です。</span><span class="sxs-lookup"><span data-stu-id="090bf-164">Name of the Event Hub.</span></span>|

    ```azurecli
    az eventhubs eventhub show --namespace-name $NS_NAME --name $HUB_NAME
    ```

## <a name="view-the-event-hub-in-the-azure-portal"></a><span data-ttu-id="090bf-165">Azure portal でイベント ハブを確認する</span><span class="sxs-lookup"><span data-stu-id="090bf-165">View the Event Hub in the Azure portal</span></span>

<span data-ttu-id="090bf-166">次に、Azure portal でこれがどのように表示されるか見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="090bf-166">Next, let's see what this looks like in the Azure portal.</span></span>

1. <span data-ttu-id="090bf-167">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="090bf-167">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="090bf-168">ポータルの上部にある検索バーを使用して、Event Hubs 名前空間を検索します。</span><span class="sxs-lookup"><span data-stu-id="090bf-168">Find your Event Hubs namespace using the Search bar at the top of portal.</span></span>

1. <span data-ttu-id="090bf-169">名前空間を選択して開きます。</span><span class="sxs-lookup"><span data-stu-id="090bf-169">Select your namespace to open it.</span></span>

1. <span data-ttu-id="090bf-170">**[エンティティ]** セクションで **[Event Hubs 名前空間]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="090bf-170">Select **Event Hubs namespace** under the **ENTITIES** section.</span></span>

1. <span data-ttu-id="090bf-171">**[Event Hubs]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="090bf-171">Click **Event Hubs**.</span></span>

    <span data-ttu-id="090bf-172">ご自分のイベント ハブが **[アクティブ]** の状態で表示され、また **[メッセージのリテンション期間]** の規定値 (*7*) と、**[パーティション数]** の規定値 (*4*) が表示されます。</span><span class="sxs-lookup"><span data-stu-id="090bf-172">Your Event Hub displays with a status of **Active**, and default values for **Message Retention** (*7*) and **Partition Count** of (*4*).</span></span>

    ![Azure portal に表示されるイベント ハブ](../media/3-event-hub.png)

## <a name="summary"></a><span data-ttu-id="090bf-174">まとめ</span><span class="sxs-lookup"><span data-stu-id="090bf-174">Summary</span></span>

<span data-ttu-id="090bf-175">これで新しいイベント ハブが作成されました。また、パブリッシャーおよびコンシューマー アプリケーションを構成するために必要なすべての情報の準備が完了しました。</span><span class="sxs-lookup"><span data-stu-id="090bf-175">You've now created a new Event Hub, and you've all the necessary information ready to configure your publisher and consumer applications.</span></span>
