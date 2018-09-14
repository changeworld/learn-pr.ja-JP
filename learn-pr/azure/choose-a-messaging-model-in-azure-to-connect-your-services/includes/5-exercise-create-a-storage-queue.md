<span data-ttu-id="2ba9c-101">この演習では、Azure サブスクリプションで新しいストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-101">In this exercise, you will create a new storage account in your Azure subscription.</span></span> <span data-ttu-id="2ba9c-102">次に Azure Cloud Shell を使用して新しいキューを作成し、それにメッセージを追加し、そのメッセージを読み、キューから削除します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-102">You will then use Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="2ba9c-103">これらは、分散アプリケーションのコンポーネントによって行われる同じアクションです。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="2ba9c-104">たとえば、モバイル アプリがメッセージをキューに追加し、Web サービスがメッセージを取得して処理するまで待機することがあります。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="2ba9c-105">ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="2ba9c-105">Create a storage account</span></span>

<span data-ttu-id="2ba9c-106">Azure Storage キューは Azure 汎用ストレージ アカウントの一部なので、まずストレージ アカウントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-106">Since Azure Storage queues are part of Azure general purpose storage accounts, you must start by creating a storage account:</span></span>

1. <span data-ttu-id="2ba9c-107">ブラウザーで [Azure portal](http://portal.azure.com) に移動し、ご自分の通常の資格情報でサインインします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-107">In a browser, navigate to the [Azure portal](http://portal.azure.com), and sign in with your normal credentials.</span></span>
1. <span data-ttu-id="2ba9c-108">左上の **[すべてのサービス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-108">In the top left, click **All services**.</span></span>
1. <span data-ttu-id="2ba9c-109">下へスクロールして **[ストレージ]** セクションを表示し、**[ストレージ アカウント]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-109">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>
1. <span data-ttu-id="2ba9c-110">**[ストレージ アカウント]** ブレードの左上にある **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-110">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

    ![[追加] が強調表示されたストレージ アカウントのスクリーン ショット](../images/5-create-a-storage-account-1.png)

1. <span data-ttu-id="2ba9c-112">**[名前]** テキスト ボックスにストレージ アカウントの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-112">In the **Name** text box, type a unique name for the storage account.</span></span>
1. <span data-ttu-id="2ba9c-113">**[デプロイ モデル]** で **[Resource Manager]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-113">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
1. <span data-ttu-id="2ba9c-114">**[アカウントの種類]** ドロップダウン リストで **[ストレージ (汎用 v2)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-114">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
1. <span data-ttu-id="2ba9c-115">**[場所]** ドロップダウン リストで、近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-115">In the **Location** drop-down list, select a region near you.</span></span>
1. <span data-ttu-id="2ba9c-116">**[レプリケーション]** ドロップダウン リストで、**[ローカル冗長ストレージ (LRS)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-116">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
1. <span data-ttu-id="2ba9c-117">**[パフォーマンス]** で **[Standard]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-117">Under **Performance**, select **Standard**.</span></span>
1. <span data-ttu-id="2ba9c-118">**[アクセス層]** で **[クール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-118">Under **Access tier**, select **Cool**.</span></span>
1. <span data-ttu-id="2ba9c-119">**[安全な転送が必須]** で **[無効]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-119">Under **Secure transfer required**, select **Disabled**.</span></span>
1. <span data-ttu-id="2ba9c-120">**[サブスクリプション]** でご使用のサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-120">Under **Subscription**, select your subscription.</span></span>

    ![[ストレージ アカウントの作成] ダイアログ ボックスのスクリーンショット](../images/5-create-a-storage-account-2.png)

1. <span data-ttu-id="2ba9c-122">**[リソース グループ]** で **[新規作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-122">Under **Resource group**, select **Create new**.</span></span> <span data-ttu-id="2ba9c-123">テキスト ボックスに「**MusicSharingResourceGroup**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-123">In the text box, type **MusicSharingResourceGroup**.</span></span>
1. <span data-ttu-id="2ba9c-124">**[仮想ネットワーク]** で **[無効]** を選択し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-124">Under **Virtual networks**, select **Disabled**, and then click **Create**.</span></span>

    ![[作成] が強調表示された [ストレージ アカウントの作成] ダイアログ ボックスのスクリーンショット](../images/5-create-a-storage-account-3.png)

<span data-ttu-id="2ba9c-126">Azure によって新しいストレージ アカウントと新しいリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-126">Azure creates the new storage account and the new resource group.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="2ba9c-127">キューを作成する</span><span class="sxs-lookup"><span data-stu-id="2ba9c-127">Create a queue</span></span>

<span data-ttu-id="2ba9c-128">ストレージ アカウントが作成されたので、それに新しいキューを追加できます。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-128">Now that the storage account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="2ba9c-129">PowerShell コマンドを使用してキューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-129">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="2ba9c-130">ポータルの上部で、**[Cloud Shell]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-130">In the top right of the portal, click the **Cloud Shell** link.</span></span>

    ![[Cloud Shell] アイコンが強調表示された Azure portal のスクリーンショット](../images/5-create-a-storage-queue-1.png)

1. <span data-ttu-id="2ba9c-132">**[Azure Cloud Shell へようこそ]** 画面で **[PowerShell (Linux)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-132">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>
1. <span data-ttu-id="2ba9c-133">**[ストレージがマウントされていません]** 画面が表示されたら、**[ストレージの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-133">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>
1. <span data-ttu-id="2ba9c-134">`PS Azure` プロンプトが表示されたら、ストレージ アカウントを取得するために次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-134">When the `PS Azure` prompt appears, to obtain the storage account, type the following command.</span></span> <span data-ttu-id="2ba9c-135">`<storageaccountname>` をストレージ アカウントの一意の名前に置き換え、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-135">Substitute `<storageaccountname>` with the unique name of your storage account, and then press Enter:</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="2ba9c-136">ストレージ アカウントのコンテキストを取得するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-136">To obtain the context of the storage account, type the following command, and then press Enter:</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="2ba9c-137">新しいキューを作成するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-137">To create a new queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="2ba9c-138">メッセージをキューに追加する</span><span class="sxs-lookup"><span data-stu-id="2ba9c-138">Add a message to the queue</span></span>

<span data-ttu-id="2ba9c-139">これでストレージ アカウントにキューが作成されたので、それにメッセージを追加できます。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-139">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="2ba9c-140">新しいメッセージを作成するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-140">To create a new message, type the following command, and then press Enter:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="2ba9c-141">新しいメッセージを新しいキューに追加するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-141">To add the new message to the new queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. <span data-ttu-id="2ba9c-142">Azure portal の左側のナビゲーションで **[すべてのリソース]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-142">In the Azure portal, in the navigation on the left, click **All resources**.</span></span>
1. <span data-ttu-id="2ba9c-143">リソースの一覧で、先に作成したストレージ アカウントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-143">In the list of resources, click the storage account you created earlier.</span></span>
1. <span data-ttu-id="2ba9c-144">[ストレージ アカウント] ブレードで、**[ストレージ エクスプローラー (プレビュー)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-144">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>
1. <span data-ttu-id="2ba9c-145">ストレージ エクスプローラーの **[キュー]** の下で **[musicsharingmessages]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-145">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="2ba9c-146">ストレージ エクスプローラーには、追加したメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-146">The Storage Explorer displays the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="2ba9c-147">メッセージの取得と削除</span><span class="sxs-lookup"><span data-stu-id="2ba9c-147">Retrieve and remove the message</span></span>

<span data-ttu-id="2ba9c-148">ストレージ キュー内のメッセージの宛先コンポーネントは、キューの先頭にあるメッセージを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-148">A destination component for a message in a Storage queue must retrieve the message at the front of the queue.</span></span> <span data-ttu-id="2ba9c-149">次に、宛先コンポーネントはそのメッセージを処理してキューから削除し、他のコンポーネントがメッセージを取得しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-149">Then the destination component must process the message, and delete it from the queue so that other components do not retrieve it.</span></span>

1. <span data-ttu-id="2ba9c-150">Cloud Shell で、キューの先頭にあるメッセージを取得するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-150">In Cloud Shell, to get the message at the front of the queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="2ba9c-151">メッセージを表示するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-151">To display the message, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="2ba9c-152">メッセージのプロパティをすべて表示するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-152">To display all the properties of the message, type the following command, and then press Enter:</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="2ba9c-153">キューからメッセージを削除するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-153">To remove the message from the queue, type the following command, and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="2ba9c-154">Azure portal で、キューの表示を更新するには、[ストレージ アカウント] ブレードに移動します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-154">In the Azure portal, to refresh the queue display, go to the Storage Account blade.</span></span> <span data-ttu-id="2ba9c-155">**[概要]** をクリックし、**[ストレージ エクスプローラー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-155">Click **Overview**, and then click **Storage Explorer**.</span></span>
1. <span data-ttu-id="2ba9c-156">**[キュー]** の下で **[musicsharingmessages]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-156">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="2ba9c-157">メッセージのみを削除したため、ストレージ エクスプローラーにはキューが空であることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-157">The Storage Explorer shows that the queue is empty because you removed the only message.</span></span>

## <a name="cleanup"></a><span data-ttu-id="2ba9c-158">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="2ba9c-158">Cleanup</span></span>

<span data-ttu-id="2ba9c-159">この演習中に作成したリソースをすべて削除するには、Cloud Shell に次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-159">To remove all resources created during this exercise, enter the following command in Cloud Shell:</span></span> 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a><span data-ttu-id="2ba9c-160">まとめ</span><span class="sxs-lookup"><span data-stu-id="2ba9c-160">Summary</span></span>

<span data-ttu-id="2ba9c-161">今回、Azure サブスクリプションでストレージ アカウントを作成し、その中で新しいキューを作成しました。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-161">Here, you created a storage account in your Azure subscription, and created a new queue in it.</span></span> <span data-ttu-id="2ba9c-162">また、PowerShell を使用し、キューにメッセージを追加したらそれを取得し、削除することで、分散アプリケーション コンポーネントのアクションをシミュレーションしました。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-162">You also used PowerShell to simulate the actions of distributed application components by adding a message to the queue, and then retrieving and removing it.</span></span>

<span data-ttu-id="2ba9c-163">ストレージ アカウントのキューは、分散アプリケーションのコンポーネント間でメッセージを渡す場合に最適なソリューションです。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-163">Storage account queues are a good solution when you want to pass messages between the components of a distributed application.</span></span> <span data-ttu-id="2ba9c-164">イベントを発行するときはストレージ キューを選択しないでください。</span><span class="sxs-lookup"><span data-stu-id="2ba9c-164">Do not choose Storage queues when you want to publish events.</span></span>