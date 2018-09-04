<span data-ttu-id="94164-101">この演習では、BLOB が作成または更新された際のサイズと名前を表示する Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="94164-101">In this exercise, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span> 

> [!NOTE]
> <span data-ttu-id="94164-102">この演習を完了するには、有効なアカウントで [Azure portal](https://portal.azure.com?azure-portal=true) にサインインしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="94164-102">To complete this exercise, make sure you're signed into the [Azure portal](https://portal.azure.com?azure-portal=true) with a valid account.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="94164-103">BLOB トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="94164-103">Create a blob trigger</span></span>

<span data-ttu-id="94164-104">既存の Azure Functions アプリケーションを引き続き使用し、BLOB トリガーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="94164-104">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="94164-105">**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-105">Point to **Functions** and select the plus (+) icon.</span></span>

    ![[関数] をポイントし、プラスを選択する](../media-drafts/4-hover-function.png)

2. <span data-ttu-id="94164-107">**[カスタム関数]**、**[BLOB トリガー]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-107">Select **Custom Function** and then **Blob trigger**.</span></span>

3. <span data-ttu-id="94164-108">言語として **[C#]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-108">Select **C#** as the language.</span></span> 

4. <span data-ttu-id="94164-109">**[名前]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="94164-109">Leave the **Name** set to the default value.</span></span>

5. <span data-ttu-id="94164-110">**[パス]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="94164-110">Leave the **Path** set to the default value.</span></span>

6. <span data-ttu-id="94164-111">既存の Azure Storage アカウントを選択するか、または Azure で新しいアカウントを作成する場合は **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-111">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="94164-112">BLOB コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="94164-112">Create a blob container</span></span>

<span data-ttu-id="94164-113">BLOB トリガーが作成されたので、ストレージ エクスプローラーを使用して、BLOB を作成し、関数をトリガーしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="94164-113">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="94164-114">新しいタブで使用した (または作成した) ストレージ アカウントを開きます。これを簡単に行うには、新しい Azure Portal タブを開き、サイドバーの **[ストレージ アカウント]** をクリックするか、サイドバーの **[すべてのサービス]** を使用して、名前を使用してフィルタリングを行います。</span><span class="sxs-lookup"><span data-stu-id="94164-114">Open the storage account you used (or created) in a new tab. An easy way to do this is to open a new Azure Portal tab and click on **Storage accounts** in the sidebar, or to use **All services** in the sidebar and then filter by the name.</span></span> <span data-ttu-id="94164-115">新しいタブを使用して、使用している 2 つのサービスを切り替えることができるようにすることが希望です。</span><span class="sxs-lookup"><span data-stu-id="94164-115">We want to use a new tab so we can switch between the two services we are working with.</span></span>

2. <span data-ttu-id="94164-116">**[ストレージ エクスプローラー (プレビュー)]** セクションをクリックすると、BLOB やファイルを操作できる、新しいブレードが開きます。</span><span class="sxs-lookup"><span data-stu-id="94164-116">Click on the **Storage Explorer (preview)** section - this will open a new blade where you can work with blobs and files.</span></span>

<span data-ttu-id="94164-117">この BLOB トリガーは、**[パス]**  フィールドに示されている場所のみを監視しています。</span><span class="sxs-lookup"><span data-stu-id="94164-117">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="94164-118">既定では、パスは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="94164-118">By default, our path should be:</span></span>

> <span data-ttu-id="94164-119">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="94164-119">samples-workitems/{name}</span></span>

<span data-ttu-id="94164-120">**samples-workitems** という名前のコンテナーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="94164-120">We need to create a container called **samples-workitems**.</span></span>

3. <span data-ttu-id="94164-121">**[BLOB コンテナー]** を右クリックし、**[BLOB コンテナーの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-121">Right-click **BLOB CONTAINERS** and select **Create blob container**.</span></span>

4. <span data-ttu-id="94164-122">名前として **samples-workitems** を入力し、アクセス レベルは、既定の**プライベート**の設定のままにします。</span><span class="sxs-lookup"><span data-stu-id="94164-122">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="94164-123">BLOB トリガーを有効にする</span><span class="sxs-lookup"><span data-stu-id="94164-123">Turn on your blob trigger</span></span>

<span data-ttu-id="94164-124">これで、監視するコンテナーが作成されたので、BLOB の作成時に出力を確認するために関数を実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="94164-124">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="94164-125">Azure Function タブに戻ります (またはそれを再度開きます)。</span><span class="sxs-lookup"><span data-stu-id="94164-125">Switch back to the Azure Function tab (or reopen it).</span></span>

2. <span data-ttu-id="94164-126">BLOB トリガーを選択してコード画面を開きます。</span><span class="sxs-lookup"><span data-stu-id="94164-126">Select your blob trigger to open the code screen.</span></span>

3. <span data-ttu-id="94164-127">**[実行]** を選択します。これにより出力ウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="94164-127">Select **Run** - this will open the output window.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="94164-128">BLOB を作成する</span><span class="sxs-lookup"><span data-stu-id="94164-128">Create a blob</span></span>

<span data-ttu-id="94164-129">BLOB トリガーが有効になっており、アクティビティをリッスンしています。</span><span class="sxs-lookup"><span data-stu-id="94164-129">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="94164-130">ログ メッセージを取得しているかどうかを確認するために、BLOB を作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="94164-130">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="94164-131">ストレージ エクスプローラーで、**[samples-workitems]** コンテナーを選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-131">In Storage explorer, select the **samples-workitems** container.</span></span>

2. <span data-ttu-id="94164-132">ツールバーから **[アップロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-132">Select **Upload** from the toolbar.</span></span>

3. <span data-ttu-id="94164-133">コンピューターから任意のファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-133">Select any file from your computer.</span></span>

4. <span data-ttu-id="94164-134">**[アップロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="94164-134">Select **Upload**.</span></span>

5. <span data-ttu-id="94164-135">[Azure Function] タブに戻り、どのファイルがアップロードされたか表示するメッセージを出力ログで確認します。</span><span class="sxs-lookup"><span data-stu-id="94164-135">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>

## <a name="pause-the-function"></a><span data-ttu-id="94164-136">関数を一時停止する</span><span class="sxs-lookup"><span data-stu-id="94164-136">Pause the Function</span></span>

<span data-ttu-id="94164-137">追加の要求に対して課金されないように、ログ ウィンドウの **[一時停止]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="94164-137">To ensure that you aren't charged for additional requests, you can click **Pause** above the log window.</span></span>

![関数を一時停止する](../media-drafts/4-pause-timer.png)


