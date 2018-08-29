<span data-ttu-id="218ad-101">この演習では、Azure サブスクリプションで新しいストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="218ad-101">In this exercise, you will create a new Storage Account in your Azure subscription.</span></span> <span data-ttu-id="218ad-102">次に Azure Cloud Shell を使用して新しいキューを作成し、それにメッセージを追加し、そのメッセージを読み、キューから削除します。</span><span class="sxs-lookup"><span data-stu-id="218ad-102">You will then use the Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="218ad-103">これらは、分散アプリケーションのコンポーネントによって行われる同じアクションです。</span><span class="sxs-lookup"><span data-stu-id="218ad-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="218ad-104">たとえば、モバイル アプリがメッセージをキューに追加し、Web サービスがメッセージを取得して処理するまで待機することがあります。</span><span class="sxs-lookup"><span data-stu-id="218ad-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="218ad-105">ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="218ad-105">Create a Storage Account</span></span>

<span data-ttu-id="218ad-106">ストレージ キューは Azure の汎用ストレージ アカウントの一部です。</span><span class="sxs-lookup"><span data-stu-id="218ad-106">Since Storage queues are part of Azure general purpose Storage accounts.</span></span> <span data-ttu-id="218ad-107">まず、ストレージ アカウントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="218ad-107">You must start by creating a Storage account:</span></span>

1. <span data-ttu-id="218ad-108">ブラウザーで [Azure Portal](http://portal.azure.com) に移動し、通常の資格情報でサインインします。</span><span class="sxs-lookup"><span data-stu-id="218ad-108">In a browser, navigate to the [Azure Portal](http://portal.azure.com) and sign in with your normal credentials.</span></span>
1. <span data-ttu-id="218ad-109">左上の **[すべてのサービス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-109">In the top left, click **All services**.</span></span>
1. <span data-ttu-id="218ad-110">下へスクロールして **[ストレージ]** セクションを表示し、**[ストレージ アカウント]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-110">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>
1. <span data-ttu-id="218ad-111">**[ストレージ アカウント]** ブレードの左上にある **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-111">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

    ![ストレージ アカウントの作成](../images/5-create-a-storage-account-1.png)

1. <span data-ttu-id="218ad-113">**[名前]** テキスト ボックスにストレージ アカウントの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="218ad-113">In the **Name** text box, type a unique name for the storage account.</span></span>
1. <span data-ttu-id="218ad-114">**[デプロイ モデル]** で **[Resource Manager]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="218ad-114">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
1. <span data-ttu-id="218ad-115">**[アカウントの種類]** ドロップダウン リストで **[ストレージ (汎用 v2)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="218ad-115">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
1. <span data-ttu-id="218ad-116">**[場所]** ドロップダウン リストで、近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="218ad-116">In the **Location** drop-down list, select a region near you.</span></span>
1. <span data-ttu-id="218ad-117">**[レプリケーション]** ドロップダウン リストで、**[ローカル冗長ストレージ (LRS)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="218ad-117">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
1. <span data-ttu-id="218ad-118">**[パフォーマンス]** で **[Standard]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="218ad-118">Under **Performance**, select **Standard**.</span></span>
1. <span data-ttu-id="218ad-119">**[アクセス層]** で **[クール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="218ad-119">Under **Access tier**, select **Cool**.</span></span>
1. <span data-ttu-id="218ad-120">**[安全な転送が必須]** で **[無効]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="218ad-120">Under **Secure transfer required** select, **Disabled**.</span></span>
1. <span data-ttu-id="218ad-121">**[サブスクリプション]** でご使用のサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="218ad-121">Under **Subscription**, select your subscription.</span></span>

    ![ストレージ アカウントの作成](../images/5-create-a-storage-account-2.png)

1. <span data-ttu-id="218ad-123">**[リソース グループ]** で **[新規作成]** を選択し、テキスト ボックスに「**MusicSharingResourceGroup**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="218ad-123">Under **Resource group** select **Create new**, and then in the textbox type **MusicSharingResourceGroup**.</span></span>
1. <span data-ttu-id="218ad-124">**[仮想ネットワーク]** で **[無効]** を選択し、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-124">Under **Virtual networks** select **Disabled** and then click **Create**.</span></span>

    ![ストレージ アカウントの作成](../images/5-create-a-storage-account-3.png)

<span data-ttu-id="218ad-126">Azure によって新しいストレージ アカウントと新しいリソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="218ad-126">Azure creates the new storage account and the new resource group.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="218ad-127">キューを作成する</span><span class="sxs-lookup"><span data-stu-id="218ad-127">Create a Queue</span></span>

<span data-ttu-id="218ad-128">これでストレージ アカウントが作成されたので、それに新しいキューを追加できます。</span><span class="sxs-lookup"><span data-stu-id="218ad-128">Now that the Storage Account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="218ad-129">PowerShell コマンドを使用してキューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="218ad-129">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="218ad-130">ポータルの上部で、**[Cloud Shell]** リンクをクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-130">In the top right of the portal, click the **Cloud Shell** link.</span></span>

    ![Cloud Shell の起動](../images/5-create-a-storage-queue-1.png)

1. <span data-ttu-id="218ad-132">**[Azure Cloud Shell へようこそ]** 画面で **[PowerShell (Linux)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-132">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>
1. <span data-ttu-id="218ad-133">**[ストレージがマウントされていません]** 画面が表示されたら、**[ストレージの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-133">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>
1. <span data-ttu-id="218ad-134">`PS Azure` プロンプトが表示されたら、ストレージ アカウントを取得するために、次のコマンドを入力します。`<storageaccountname>` をお使いのストレージ アカウントの一意の名前に変更し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-134">When the `PS Azure` prompt appears, to obtain the storage account, type the following command, substituting `<storageaccountname>` with the unique name of your storage account, and then press Enter:</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="218ad-135">ストレージ アカウントのコンテキストを取得するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-135">To obtain the context of the storage account, type the following command and then press Enter:</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="218ad-136">新しいキューを作成するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-136">To create a new queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="218ad-137">メッセージをキューに追加する</span><span class="sxs-lookup"><span data-stu-id="218ad-137">Add a Message to the Queue</span></span>

<span data-ttu-id="218ad-138">これでストレージ アカウントにキューが作成されたので、それにメッセージを追加できます。</span><span class="sxs-lookup"><span data-stu-id="218ad-138">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="218ad-139">新しいメッセージを作成するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-139">To create a new message, type the following command and then press Enter:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="218ad-140">新しいメッセージを新しいキューに追加するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-140">To add the new message to the new queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

1. <span data-ttu-id="218ad-141">Azure Portal の左側のナビゲーションで **[すべてのリソース]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-141">In the Azure Portal, in the navigation on the left, click **All resources**.</span></span>
1. <span data-ttu-id="218ad-142">リソースの一覧で、先に作成したストレージ アカウントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-142">In the list of resources, click the storage account you created earlier.</span></span>
1. <span data-ttu-id="218ad-143">[ストレージ アカウント] ブレードで、**[ストレージ エクスプローラー (プレビュー)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-143">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>
1. <span data-ttu-id="218ad-144">ストレージ エクスプローラーの **[キュー]** の下で **[musicsharingmessages]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-144">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="218ad-145">ストレージ エクスプローラーには、追加したメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="218ad-145">The Storage Explorer displays the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="218ad-146">メッセージの取得と削除</span><span class="sxs-lookup"><span data-stu-id="218ad-146">Retrieve and Remove the Message</span></span>

<span data-ttu-id="218ad-147">ストレージ キューのメッセージに対する宛先コンポーネントでは、キューの先頭でメッセージを取得し、それを処理したら、他のコンポーネントによって取得されないようにキューから削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="218ad-147">A destination component for a message in a Storage queue, must retrieve the message at the front of the queue, process it, and then delete it from the queue so that other components do not retrieve it:</span></span>

1. <span data-ttu-id="218ad-148">Azure Cloud Shell で、キューの先頭にあるメッセージを取得するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-148">In the Azure Cloud Shell, to get the message at the front of the queue, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="218ad-149">メッセージを表示するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-149">To display the message, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="218ad-150">メッセージのプロパティをすべて表示するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-150">To display all the properties of the message, type the following command and then press Enter:</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="218ad-151">キューからメッセージを削除するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="218ad-151">To remove the message from the queue, type the following command and then press Enter:</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="218ad-152">Azure Portal でキュー ディスプレイを更新するには、[ストレージ アカウント] ブレードで、**[概要]** をクリックし、**[ストレージ エクスプローラー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-152">In the Azure Portal, to refresh the queue display, in the Storage Account blade, click **Overview** and then click **Storage Explorer**.</span></span>
1. <span data-ttu-id="218ad-153">**[キュー]** の下で **[musicsharingmessages]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="218ad-153">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="218ad-154">メッセージのみを削除したため、ストレージ エクスプローラーにはキューが空であることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="218ad-154">The Storage Explorer shows that the queue is empty because you removed the only message.</span></span>

## <a name="cleanup"></a><span data-ttu-id="218ad-155">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="218ad-155">Cleanup</span></span>

<span data-ttu-id="218ad-156">この演習中に作成したリソースをすべて削除するには、Azure Cloud Shell に次のコマンドを入力します</span><span class="sxs-lookup"><span data-stu-id="218ad-156">To remove all resources created during this exercise enter the following command in the Azure Cloud Shell</span></span> 
```powershell
Remove-AzureRmResourceGroup -Name "MusicSharingResourceGroup" -Force
```


## <a name="summary"></a><span data-ttu-id="218ad-157">まとめ</span><span class="sxs-lookup"><span data-stu-id="218ad-157">Summary</span></span>

<span data-ttu-id="218ad-158">今回、Azure サブスクリプションでストレージ アカウントを作成し、その中で新しいキューを作成しました。</span><span class="sxs-lookup"><span data-stu-id="218ad-158">Here, you created a Storage Account in your Azure subscription and created a new queue in it.</span></span> <span data-ttu-id="218ad-159">また、PowerShell を使用し、キューにメッセージを追加したらそれを取得し、削除することで、分散アプリケーション コンポーネントのアクションをシミュレーションしました。</span><span class="sxs-lookup"><span data-stu-id="218ad-159">You also used PowerShell to simulate the actions of distributed application components by adding a message to the queue and then retrieving and removing it.</span></span>

<span data-ttu-id="218ad-160">Azure Storage アカウントのキューは、分散アプリケーションのコンポーネント間でメッセージを渡す場合に最適なソリューションです。</span><span class="sxs-lookup"><span data-stu-id="218ad-160">Azure Storage Account queues are a good solution when you want to pass messages between the components of a distributed application.</span></span> <span data-ttu-id="218ad-161">イベントを発行するときはストレージ キューを選択しないでください。</span><span class="sxs-lookup"><span data-stu-id="218ad-161">Do not choose Storage queues when you want to publish events.</span></span>