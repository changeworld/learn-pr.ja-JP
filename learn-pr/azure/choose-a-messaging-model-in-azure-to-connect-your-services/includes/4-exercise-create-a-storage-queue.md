<span data-ttu-id="d00c0-101">この演習では、Azure サブスクリプションで新しいストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-101">In this exercise, you will create a new storage account in your Azure subscription.</span></span> <span data-ttu-id="d00c0-102">次に Azure Cloud Shell を使用して新しいキューを作成し、それにメッセージを追加し、そのメッセージを読み、キューから削除します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-102">You will then use Azure Cloud Shell to create a new queue, add a message to it, and then read that message and remove it from the queue.</span></span>

<span data-ttu-id="d00c0-103">これらは、分散アプリケーションのコンポーネントによって行われる同じアクションです。</span><span class="sxs-lookup"><span data-stu-id="d00c0-103">These are the same actions taken by components in a distributed application.</span></span> <span data-ttu-id="d00c0-104">たとえば、モバイル アプリがメッセージをキューに追加し、Web サービスがメッセージを取得して処理するまで待機することがあります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-104">For example, a mobile app may add a message to a queue, where it waits for a web service to retrieve it and process it.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="d00c0-105">ストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="d00c0-105">Create a storage account</span></span>

<span data-ttu-id="d00c0-106">Azure Storage キューは Azure 汎用ストレージ アカウントの一部なので、まずストレージ アカウントを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-106">Since Azure Storage queues are part of Azure general-purpose storage accounts, you must start by creating a storage account:</span></span>

1. <span data-ttu-id="d00c0-107">ブラウザーで [Azure portal](https://portal.azure.com?azure-portal=true) に移動し、通常の資格情報でサインインします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-107">In a browser, navigate to the [Azure portal](https://portal.azure.com?azure-portal=true), and sign in with your normal credentials.</span></span>

1. <span data-ttu-id="d00c0-108">左上の **[すべてのサービス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-108">In the top left, click **All services**.</span></span>

1. <span data-ttu-id="d00c0-109">下へスクロールして **[ストレージ]** セクションを表示し、**[ストレージ アカウント]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-109">Scroll down to the **Storage** section, and then click **Storage accounts**.</span></span>

1. <span data-ttu-id="d00c0-110">**[ストレージ アカウント]** ブレードの左上にある **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-110">At the top left of the **Storage accounts** blade, click **Add**.</span></span>

  ![[追加] が強調表示されたストレージ アカウントのスクリーン ショット](../media-draft/4-create-a-storage-account-1.png)

1. <span data-ttu-id="d00c0-112">表示されるダイアログに次の情報を入力します。ポータルの各オプションには `(i)` アイコンが表示されます。このアイコンをクリックすると、オプションの実行内容について詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-112">In the resulting dialog, enter the following information, each of these options has a `(i)` icon in the portal which you can use to get more information about what the option does.</span></span>

    - <span data-ttu-id="d00c0-113">**[名前]** テキスト ボックスにストレージ アカウントの一意の名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-113">In the **Name** text box, type a unique name for the storage account.</span></span>
    - <span data-ttu-id="d00c0-114">**[デプロイ モデル]** で **[Resource Manager]** が選択されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-114">Under **Deployment model**, ensure that **Resource Manager** is selected.</span></span>
    - <span data-ttu-id="d00c0-115">**[アカウントの種類]** ドロップダウン リストで **[ストレージ (汎用 v2)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-115">In the **Account kind** drop-down list, select **Storage (general purpose v2)**.</span></span>
    - <span data-ttu-id="d00c0-116">**[場所]** ドロップダウン リストで、近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-116">In the **Location** drop-down list, select a region near you.</span></span>
    - <span data-ttu-id="d00c0-117">**[レプリケーション]** ドロップダウン リストで、**[ローカル冗長ストレージ (LRS)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-117">In the **Replication** drop-down list, select **Locally-redundant storage (LRS)**.</span></span>
    - <span data-ttu-id="d00c0-118">**[パフォーマンス]** で **[Standard]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-118">Under **Performance**, select **Standard**.</span></span>
    - <span data-ttu-id="d00c0-119">**[アクセス層]** で **[クール]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-119">Under **Access tier**, select **Cool**.</span></span>
    - <span data-ttu-id="d00c0-120">**[安全な転送が必須]** で **[無効]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-120">Under **Secure transfer required**, select **Disabled**.</span></span>
    - <span data-ttu-id="d00c0-121">**[サブスクリプション]** でご使用のサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-121">Under **Subscription**, select your subscription.</span></span>
    - <span data-ttu-id="d00c0-122">**[リソース グループ]** で **[新規作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-122">Under **Resource group**, select **Create new**.</span></span> <span data-ttu-id="d00c0-123">テキスト ボックスに「**MusicSharingResourceGroup**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-123">In the text box, type **MusicSharingResourceGroup**.</span></span>
    - <span data-ttu-id="d00c0-124">**[仮想ネットワーク]** で **[無効]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-124">Under **Virtual networks**, select **Disabled**.</span></span> 

    ![[ストレージ アカウントを作成する] ダイアログ ボックスのスクリーンショット](../media-draft/4-create-a-storage-account-2.png)

1. <span data-ttu-id="d00c0-126">**[作成]** をクリックします。Azure で新しいリソース グループとそれに関連付けられた新しいストレージ アカウントが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-126">Click **Create** - Azure will create a new resource group and a new storage account associated with it.</span></span>

    ![[作成] が強調表示された [ストレージ アカウントの作成] ダイアログ ボックスのスクリーンショット](../media-draft/4-create-a-storage-account-3.png)

## <a name="create-a-queue"></a><span data-ttu-id="d00c0-128">キューを作成する</span><span class="sxs-lookup"><span data-stu-id="d00c0-128">Create a queue</span></span>

<span data-ttu-id="d00c0-129">ストレージ アカウントが作成されたので、それに新しいキューを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-129">Now that the storage account has been created, you can add a new queue to it.</span></span> <span data-ttu-id="d00c0-130">PowerShell コマンドを使用してキューを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-130">You must create the queue by using PowerShell commands:</span></span>

1. <span data-ttu-id="d00c0-131">ポータルの上部で、**Cloud Shell** リンク `(>_)` をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-131">In the top right of the portal, click the **Cloud Shell** link `(>_)`.</span></span>

    ![[Cloud Shell] アイコンが強調表示された Azure portal のスクリーンショット](../media-draft/4-create-a-storage-queue-1.png)

1. <span data-ttu-id="d00c0-133">**[Azure Cloud Shell へようこそ]** 画面で **[PowerShell (Linux)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-133">In the **Welcome to Azure Cloud Shell** screen, click **PowerShell (Linux)**.</span></span>

1. <span data-ttu-id="d00c0-134">**[ストレージがマウントされていません]** 画面が表示されたら、**[ストレージの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-134">If the **You have no storage mounted** screen appears, click **Create storage**.</span></span>

1. <span data-ttu-id="d00c0-135">`PS Azure` プロンプトが表示されたら、ストレージ アカウントを取得するために次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-135">When the `PS Azure` prompt appears, to obtain the storage account, type the following command.</span></span> <span data-ttu-id="d00c0-136">`<storageaccountname>` を、前の手順で作成したストレージ アカウントの一意の名前に置き換え、**Enter** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-136">Substitute `<storageaccountname>` with the unique name of your storage account you created above, and then press **Enter**.</span></span> <span data-ttu-id="d00c0-137">結果のオブジェクトを `$storageaccount` という名前の変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-137">We want to assign the resulting object to a variable named `$storageaccount`.</span></span>

    ```powershell
    $storageaccount = Get-AzureRmStorageAccount -Name <storageaccountname> -ResourceGroup  MusicSharingResourceGroup
    ```

1. <span data-ttu-id="d00c0-138">次に、ストレージ アカウント _context_ を取得する必要があります。これは返されたオブジェクトのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="d00c0-138">Next, we need to get the storage account _context_ - this is a property on the returned object.</span></span> <span data-ttu-id="d00c0-139">それを `$context` という別の変数に割り当てましょう。</span><span class="sxs-lookup"><span data-stu-id="d00c0-139">Let's assign it to another variable named `$context`.</span></span>

    ```powershell
    $context = $storageaccount.Context
    ```

1. <span data-ttu-id="d00c0-140">これでキューを作成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="d00c0-140">Now we are ready to create the queue.</span></span> <span data-ttu-id="d00c0-141">`New-AzureStorageQueue` コマンドを使用して `$messageQueue` 変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-141">Use the `New-AzureStorageQueue` command and assign it to a `$messageQueue` variable.</span></span>
    - <span data-ttu-id="d00c0-142">値 `musicsharingmessages` を指定して `-Name` パラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-142">Pass a `-Name` parameter with the value `musicsharingmessages`</span></span>
    - <span data-ttu-id="d00c0-143">前の手順で取得した値を指定して `-Context` パラメーターを渡します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-143">Pass a `-Context` parameter with the value you retrieved in the previous step.</span></span>

    ```powershell
    $messageQueue = New-AzureStorageQueue -Name musicsharingmessages -Context $context
    ```

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="d00c0-144">メッセージをキューに追加する</span><span class="sxs-lookup"><span data-stu-id="d00c0-144">Add a message to the queue</span></span>

<span data-ttu-id="d00c0-145">これでストレージ アカウントにキューが作成されたので、それにメッセージを追加できます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-145">Now that you have created a queue in the storage account, you can add a message to it.</span></span>

1. <span data-ttu-id="d00c0-146">新しいメッセージを作成するには、`New-Object` メソッドを使用し、文字列ベースの引数を指定して .NET `CloudQueueMessage` を作成します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-146">To create a new message, use the `New-Object` method to create a .NET `CloudQueueMessage` with a string-based argument:</span></span>

    ```powershell
    $newSongMessage = New-Object -TypeName Microsoft.WindowsAzure.Storage.Queue.CloudQueueMessage -ArgumentList "A new song has been added."
    ```

1. <span data-ttu-id="d00c0-147">新しいメッセージを新しいキューに追加するには、作成した `CloudQueueMessage` を `$messageQueue` キューの `AddMessageAsync` メソッドに渡します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-147">To add the new message to the new queue, pass the created `CloudQueueMessage` to the `AddMessageAsync` method on your `$messageQueue` queue.</span></span>

    ```powershell
    $messageQueue.CloudQueue.AddMessageAsync($newSongMessage)
    ```

## <a name="verify-the-message-was-queued"></a><span data-ttu-id="d00c0-148">メッセージがキューに追加されたことを確認する</span><span class="sxs-lookup"><span data-stu-id="d00c0-148">Verify the message was queued</span></span>

<span data-ttu-id="d00c0-149">**ストレージ エクスプローラー**を使用してキューを操作することができます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-149">We can use the **Storage Explorer** to work with our queue.</span></span> <span data-ttu-id="d00c0-150">2 種類の方法で利用できます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-150">There are two variations available:</span></span>

- <span data-ttu-id="d00c0-151">ダウンロードできる Linux、macOS、および Windows 用のクロスプラットフォーム デスクトップ アプリ。</span><span class="sxs-lookup"><span data-stu-id="d00c0-151">A cross-platform desktop app for Linux, macOS, and Windows that you can download.</span></span>
- <span data-ttu-id="d00c0-152">Azure portal のプレビュー Web バージョン。</span><span class="sxs-lookup"><span data-stu-id="d00c0-152">A preview web version in the Azure portal.</span></span> <span data-ttu-id="d00c0-153">ここではこの方法を使用しますが、好みに応じてデスクトップ バージョンをインストールすることもできます。手順は同様です。</span><span class="sxs-lookup"><span data-stu-id="d00c0-153">This is the one we will use here, but you can install the desktop version if you prefer - the instructions are very similar.</span></span>

1. <span data-ttu-id="d00c0-154">Azure portal の左側のナビゲーションで **[すべてのリソース]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-154">In the Azure portal, in the navigation on the left, click **All resources**.</span></span>

1. <span data-ttu-id="d00c0-155">リソースの一覧で、先に作成したストレージ アカウントをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-155">In the list of resources, click the storage account you created earlier.</span></span>

1. <span data-ttu-id="d00c0-156">[ストレージ アカウント] ブレードで、**[ストレージ エクスプローラー (プレビュー)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-156">In the storage account blade, click **Storage Explorer (Preview)**.</span></span>

1. <span data-ttu-id="d00c0-157">ストレージ エクスプローラーの **[キュー]** の下で **[musicsharingmessages]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-157">In the Storage Explorer, under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="d00c0-158">ストレージ エクスプローラーには、追加したメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-158">The Storage Explorer should display the message you just added.</span></span>

## <a name="retrieve-and-remove-the-message"></a><span data-ttu-id="d00c0-159">メッセージの取得と削除</span><span class="sxs-lookup"><span data-stu-id="d00c0-159">Retrieve and remove the message</span></span>

<span data-ttu-id="d00c0-160">ストレージ キュー内のメッセージの宛先コンポーネントは、キューの先頭にあるメッセージを取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-160">A destination component for a message in a Storage queue must retrieve the message at the front of the queue.</span></span> <span data-ttu-id="d00c0-161">次に、宛先コンポーネントはそのメッセージを処理してキューから削除し、他のコンポーネントがメッセージを取得しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-161">Then the destination component must process the message and delete it from the queue so that other components do not retrieve it.</span></span>

1. <span data-ttu-id="d00c0-162">最初の使用できるメッセージを取得するには、PowerShell でキューに対して `GetMessageAsync` メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-162">We can retrieve the first available message in PowerShell using the `GetMessageAsync` method on our queue.</span></span> <span data-ttu-id="d00c0-163">`Result` プロパティを使用して戻り値を取得できるように待機する必要があるため、これは非同期の .NET メソッドです。</span><span class="sxs-lookup"><span data-stu-id="d00c0-163">This is an asynchronous .NET method, since we want to wait for it we can just use the `Result` property to get the return value.</span></span> <span data-ttu-id="d00c0-164">その結果、パラメーターに割り当てることができるメッセージを表すオブジェクトが返されます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-164">This returns an object representing the message which we can assign to a parameter.</span></span>

    ```powershell
    $retrievedMessage = $messageQueue.CloudQueue.GetMessageAsync().Result
    ```

1. <span data-ttu-id="d00c0-165">`AsString` を呼び出してメッセージのテキスト バージョンを取得できます。その結果、コンソールに値が出力されます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-165">We can get a textual version of the message by calling `AsString` - this will output the value on the console.</span></span>

    ```powershell
    $retrievedMessage.AsString
    ```

1. <span data-ttu-id="d00c0-166">また、変数名を入力して **Enter** キーを押すだけで、メッセージのすべてのプロパティを表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-166">Or, we can display all the properties of the message by just typing the variable name and pressing **Enter**.</span></span>

    ```powershell
    $retrievedMessage
    ```

1. <span data-ttu-id="d00c0-167">`GetMessageAsync` を実行してもメッセージは削除*されません*。単にメッセージが返されるだけです。つまり、メッセージを再び処理することができます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-167">`GetMessageAsync` does *not* remove the message - it simply returns it, which means we could process it again.</span></span> <span data-ttu-id="d00c0-168">キューからメッセージを削除するには、キューに対して `DeleteMessageAsync` メソッドを使用できます。この場合、削除するメッセージを渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-168">To remove the message from the queue, we can use the `DeleteMessageAsync` method on the queue - this requires that we pass in the message we want to remove.</span></span>

    ```powershell
    $messageQueue.CloudQueue.DeleteMessageAsync($retrievedMessage)
    ```

1. <span data-ttu-id="d00c0-169">メッセージが削除されたことを確認するには、[ストレージ アカウント] ブレードに移動し、**[概要] > [ストレージ エクスプローラー]** の順に選択して、Azure portal のキュー表示を更新します。</span><span class="sxs-lookup"><span data-stu-id="d00c0-169">To verify that the message is gone, refresh the queue display in the Azure portal by navigating to the Storage Account blade and selecting **Overview > Storage Explorer**.</span></span> <span data-ttu-id="d00c0-170">**[キュー]** の下で **[musicsharingmessages]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d00c0-170">Under **QUEUES**, click **musicsharingmessages**.</span></span> <span data-ttu-id="d00c0-171">メッセージのみを削除したため、ストレージ エクスプローラーにはキューが空であることが表示されます。</span><span class="sxs-lookup"><span data-stu-id="d00c0-171">The Storage Explorer should now show that the queue is empty because you removed the only message.</span></span>


## <a name="summary"></a><span data-ttu-id="d00c0-172">まとめ</span><span class="sxs-lookup"><span data-stu-id="d00c0-172">Summary</span></span>
<span data-ttu-id="d00c0-173">ストレージ アカウントのキューは、分散アプリケーションのコンポーネント間で_メッセージ_を渡す場合に最適なソリューションです。</span><span class="sxs-lookup"><span data-stu-id="d00c0-173">Storage account queues are a good solution when you want to pass _messages_ between the components of a distributed application.</span></span> <span data-ttu-id="d00c0-174">これは、配信を保証したい場合や、送信した順序と同じ順序でメッセージを確実に配信したい場合に最適です。</span><span class="sxs-lookup"><span data-stu-id="d00c0-174">This is a great choice if you want to guarantee delivery or to ensure that messages are delivered in the same order you sent them.</span></span> <span data-ttu-id="d00c0-175">ただし、キューは、送信者と受信者が、渡されるデータの形式を認識していることを前提としています。つまり、2 つの通信サービス間をある程度 "結合" する暗黙のデータ コントラクトがあります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-175">However, queues imply that the sender and receiver understand the format of the data being passed - there's an implied data contract between them which adds a bit of "coupling" between the two communicating services.</span></span>

<span data-ttu-id="d00c0-176">正しい形式のデータ ブロックを渡すことは、すべてのアーキテクチャにとって必須ではありません。メッセージの処理側が何かを認識していなくても、メッセージを送信し、後の処理には関知しない単純なメッセージが必要な場合もあります。</span><span class="sxs-lookup"><span data-stu-id="d00c0-176">Not all architectures need to pass formatted blocks of data, some really just need simple messages which we want to fire-and-forget without any knowledge of what will handle the message.</span></span> <span data-ttu-id="d00c0-177">このようなシナリオの場合、キューは最適ではありません。</span><span class="sxs-lookup"><span data-stu-id="d00c0-177">In these scenarios, a queue isn't a great choice.</span></span> <span data-ttu-id="d00c0-178">このようなスタイルの通信に適した別のメッセージング戦略を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="d00c0-178">Let's look at another messaging strategy which is more suited to this style of communication.</span></span>