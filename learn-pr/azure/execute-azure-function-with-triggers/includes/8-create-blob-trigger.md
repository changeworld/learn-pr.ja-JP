<span data-ttu-id="36773-101">このユニットでは、BLOB が作成または更新されたときの名前とサイズを表示する Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="36773-101">In this unit, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="36773-102">BLOB トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="36773-102">Create a blob trigger</span></span>

<span data-ttu-id="36773-103">ここでも、既存の Azure Functions アプリケーションを引き続き使用し、BLOB トリガーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="36773-103">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="36773-104">サンドボックスをアクティブ化したときと同じアカウントを使用して [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="36773-104">Make sure you are signed into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="36773-105">**[すべてのリソース]** 画面に移動し、関数アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-105">Navigate to the **All resources** screen and select your function app.</span></span>

1. <span data-ttu-id="36773-106">**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-106">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="36773-107">**[BLOB トリガー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-107">Select **Blob trigger**.</span></span>

1. <span data-ttu-id="36773-108">**[名前]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="36773-108">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="36773-109">**[パス]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="36773-109">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="36773-110">**[ストレージ アカウント接続]** ドロップダウンの隣にある_新しい_リンクを選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-110">Select the _new_ link next to the **Storage account connection** dropdown.</span></span> <span data-ttu-id="36773-111">ブレードが表示されたら、**[新規作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="36773-111">In the blade that pops up, click **Create new**.</span></span> <span data-ttu-id="36773-112">新しいストレージ アカウントに一意の名前を入力し、**[OK]** を選択してストレージ アカウントを作成し、ウィンドウを閉じます。</span><span class="sxs-lookup"><span data-stu-id="36773-112">Enter a unique name for the new storage account and select **OK** to create the storage account and close pane.</span></span>

1. <span data-ttu-id="36773-113">[新しい関数] 画面に戻ったら、**[作成]** を選択して関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="36773-113">Once you have returned to the New Function screen, select **Create** to create the function.</span></span>

## <a name="create-a-blob-container"></a><span data-ttu-id="36773-114">BLOB コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="36773-114">Create a blob container</span></span>

<span data-ttu-id="36773-115">BLOB トリガーが作成されたので、ストレージ エクスプローラーを使用して、BLOB を作成し、関数をトリガーしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="36773-115">Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.</span></span>

1. <span data-ttu-id="36773-116">新しいタブで使用した (または作成した) ストレージ アカウントを開きます。</span><span class="sxs-lookup"><span data-stu-id="36773-116">Open the storage account you used (or created) in a new tab.</span></span>

    > [!TIP]
    > <span data-ttu-id="36773-117">ほとんどのブラウザーでタブを複製できます。複製するタブを右クリックし、メニューが表示されたら **[複製]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-117">You can duplicate a tab in most browsers by right-clicking on the tab in question and selecting **Duplicate** from the menu that appears.</span></span> <span data-ttu-id="36773-118">今回、使用中の 2 つのサービスを切り替えることができるように新しいタブを使用します。</span><span class="sxs-lookup"><span data-stu-id="36773-118">We want to use a new tab so we can switch between the two services we are working with.</span></span>

1. <span data-ttu-id="36773-119">サイドバーにある **[ストレージ アカウント]** を選択するか、サイズバーの **[すべてのリソース]** を選択し、名前でリソースを絞り込みます。</span><span class="sxs-lookup"><span data-stu-id="36773-119">Select **Storage accounts** in the sidebar, or select **All resources** in the sidebar and then filter by the name.</span></span>

1. <span data-ttu-id="36773-120">**[ストレージ エクスプローラー (プレビュー)]** セクションをクリックすると、BLOB やファイルを操作できる新しいウィンドウが開きます。</span><span class="sxs-lookup"><span data-stu-id="36773-120">Click on the **Storage Explorer (preview)** section - this will open a new panel where you can work with blobs and files.</span></span>

<span data-ttu-id="36773-121">この BLOB トリガーは、**[パス]**  フィールドに示されている場所のみを監視しています。</span><span class="sxs-lookup"><span data-stu-id="36773-121">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="36773-122">既定では、パスは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="36773-122">By default, our path should be:</span></span>

> <span data-ttu-id="36773-123">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="36773-123">samples-workitems/{name}</span></span>

<span data-ttu-id="36773-124">**samples-workitems** という名前のコンテナーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="36773-124">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="36773-125">**[BLOB コンテナー]** を右クリックし、**[BLOB コンテナーの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-125">Right-click **BLOB CONTAINERS** and select **Create Blob Container**.</span></span>

1. <span data-ttu-id="36773-126">名前として **samples-workitems** を入力し、アクセス レベルは既定の**プライベート**の設定のままにして **[OK]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-126">Enter **samples-workitems** as the name, leave the access level at the default **Private** setting, and select **OK**.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="36773-127">BLOB トリガーを有効にする</span><span class="sxs-lookup"><span data-stu-id="36773-127">Turn on your blob trigger</span></span>

<span data-ttu-id="36773-128">これで、監視するコンテナーが作成されたので、BLOB の作成時に出力を確認するために関数を実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="36773-128">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="36773-129">Azure 関数のあるブラウザー タブに戻ります (またはもう一度開きます)。</span><span class="sxs-lookup"><span data-stu-id="36773-129">Switch back to the browser tab with your Azure Function (or reopen it).</span></span>

1. <span data-ttu-id="36773-130">BLOB トリガーを選択してコード画面を開きます。</span><span class="sxs-lookup"><span data-stu-id="36773-130">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="36773-131">画面の下部にある **[ログ]** タブを開きます。</span><span class="sxs-lookup"><span data-stu-id="36773-131">Open the **Logs** tab at the bottom of the screen.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="36773-132">BLOB を作成する</span><span class="sxs-lookup"><span data-stu-id="36773-132">Create a blob</span></span>

<span data-ttu-id="36773-133">BLOB トリガーが有効になっており、アクティビティをリッスンしています。</span><span class="sxs-lookup"><span data-stu-id="36773-133">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="36773-134">ログ メッセージを取得しているかどうかを確認するために、BLOB を作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="36773-134">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="36773-135">Storage Explorer のあるブラウザー タブに戻ります。</span><span class="sxs-lookup"><span data-stu-id="36773-135">Switch back to the browser tab with Storage Explorer.</span></span>

1. <span data-ttu-id="36773-136">Storage Explorer で、**[BLOB コンテナー]** 一覧から **[samples-workitems]** コンテナーを選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-136">In Storage Explorer, select the **samples-workitems** container from the **BLOB CONTAINERS** list.</span></span>

1. <span data-ttu-id="36773-137">ツールバーから **[アップロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-137">Select **Upload** from the toolbar.</span></span>

1. <span data-ttu-id="36773-138">コンピューターから任意のファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-138">Select any file from your computer.</span></span>

1. <span data-ttu-id="36773-139">**[認証の種類]** として **[SAS]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-139">Under **Authentication type**, select **SAS**.</span></span>

1. <span data-ttu-id="36773-140">**[アップロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="36773-140">Select **Upload**.</span></span>

1. <span data-ttu-id="36773-141">[Azure Function] タブに戻り、どのファイルがアップロードされたか表示するメッセージを出力ログで確認します。</span><span class="sxs-lookup"><span data-stu-id="36773-141">Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded.</span></span>