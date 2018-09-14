<span data-ttu-id="87df6-101">この演習では、BLOB が作成または更新された際のサイズと名前を表示する Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="87df6-101">In this exercise, we're going to create an Azure function that displays the name and size of a blob when it's created or updated.</span></span> 

> [!NOTE]
> <span data-ttu-id="87df6-102">この演習を完了するには、有効なアカウントで [Azure portal](https://portal.azure.com/) にサインインしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="87df6-102">To complete this exercise, make sure you're signed in to the [Azure portal](https://portal.azure.com/) with a valid account.</span></span>

## <a name="create-a-blob-trigger"></a><span data-ttu-id="87df6-103">BLOB トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="87df6-103">Create a blob trigger</span></span>

<span data-ttu-id="87df6-104">既存の Azure Functions アプリケーションを引き続き使用し、BLOB トリガーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="87df6-104">Again, let's continue using our existing Azure Functions application and add a blob trigger.</span></span>

1. <span data-ttu-id="87df6-105">**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-105">Point to **Functions** and select the plus (+) icon.</span></span>

    ![[関数] をポイントし、プラスを選択する](../media-drafts/4-hover-function.png)

1. <span data-ttu-id="87df6-107">**[BLOB トリガー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-107">Select **Blob trigger**.</span></span>

1. <span data-ttu-id="87df6-108">言語として **[C#]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-108">Select **C#** as the language.</span></span> 

1. <span data-ttu-id="87df6-109">**[名前]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="87df6-109">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="87df6-110">**[パス]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="87df6-110">Leave the **Path** set to the default value.</span></span>

1. <span data-ttu-id="87df6-111">既存の Azure Storage アカウントを選択するか、または Azure で新しいアカウントを作成する場合は **[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-111">Select an existing Azure Storage account, or select **Create** if you want Azure to create a new account for you.</span></span>

## <a name="download-storage-explorer"></a><span data-ttu-id="87df6-112">Storage Explorer をダウンロードする</span><span class="sxs-lookup"><span data-stu-id="87df6-112">Download Storage explorer</span></span>

<span data-ttu-id="87df6-113">これで BLOB トリガーが作成されたので Storage Explorer をダウンロードしてみましょう。これにより BLOB を簡単に作成することができます。</span><span class="sxs-lookup"><span data-stu-id="87df6-113">Now that we've created a blob trigger, let's download Storage explorer, which will allow us to easily create a blob.</span></span>

- <span data-ttu-id="87df6-114">[Storage Explorer](http://storageexplorer.com) をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="87df6-114">Download [Storage explorer](http://storageexplorer.com).</span></span>

## <a name="connect-to-your-azure-storage-account"></a><span data-ttu-id="87df6-115">Azure Storage アカウントに接続する</span><span class="sxs-lookup"><span data-stu-id="87df6-115">Connect to your Azure Storage account</span></span>

<span data-ttu-id="87df6-116">これで、Storage Explorer がダウンロードされました。</span><span class="sxs-lookup"><span data-stu-id="87df6-116">We now have Storage explorer downloaded.</span></span> <span data-ttu-id="87df6-117">指定された資格情報を使用してサインインしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="87df6-117">Let's sign in using the credentials that were supplied.</span></span>

1. <span data-ttu-id="87df6-118">Storage Explorer の左側にあるプラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-118">In Storage explorer, select the plus (+) icon on the left.</span></span>

1. <span data-ttu-id="87df6-119">**[Use a storage account name and key]\(ストレージ アカウント名とキーを使用\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-119">Select **Use a storage account name and key**.</span></span>

1. <span data-ttu-id="87df6-120">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-120">Select **Next**.</span></span>

1. <span data-ttu-id="87df6-121">Azure の BLOB トリガーで、**[統合]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-121">In Azure, under your blob trigger, select **Integrate**.</span></span>

1. <span data-ttu-id="87df6-122">**[ドキュメント]** を選択して、ビューを展開します。</span><span class="sxs-lookup"><span data-stu-id="87df6-122">Select **Documentation** to expand the view.</span></span>

1. <span data-ttu-id="87df6-123">**[アカウント名]** と **[アカウント キー]** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="87df6-123">Copy the **Account Name** and **Account Key**.</span></span>

1. <span data-ttu-id="87df6-124">Storage Explorer に戻って、**[アカウント名]** と **[アカウント キー]** に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="87df6-124">Back in Storage explorer, paste in the **Account Name** and **Account Key**.</span></span>

1. <span data-ttu-id="87df6-125">**[表示名]** を入力します。</span><span class="sxs-lookup"><span data-stu-id="87df6-125">Enter a **Display name**.</span></span> <span data-ttu-id="87df6-126">この値は、Storage Explorer の接続の名前です。</span><span class="sxs-lookup"><span data-stu-id="87df6-126">This value is the name of the connection in Storage explorer.</span></span>

1. <span data-ttu-id="87df6-127">**[次へ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-127">Select **Next**.</span></span>

1. <span data-ttu-id="87df6-128">**[接続]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-128">Select **Connect**.</span></span> 

## <a name="create-a-blob-container"></a><span data-ttu-id="87df6-129">BLOB コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="87df6-129">Create a blob container</span></span>

<span data-ttu-id="87df6-130">ここでは、Azure Storage アカウントに接続されていません。</span><span class="sxs-lookup"><span data-stu-id="87df6-130">We aren't connected to our Azure Storage account.</span></span> <span data-ttu-id="87df6-131">この BLOB トリガーは、**[パス]** フィールドに示されている場所だけを監視しています。</span><span class="sxs-lookup"><span data-stu-id="87df6-131">Remember that our blob trigger is monitoring only the location described in the **Path** field.</span></span> <span data-ttu-id="87df6-132">既定では、パスは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="87df6-132">By default, our path should be:</span></span>

> <span data-ttu-id="87df6-133">samples-workitems/{name}</span><span class="sxs-lookup"><span data-stu-id="87df6-133">samples-workitems/{name}</span></span>

<span data-ttu-id="87df6-134">**samples-workitems** という名前のコンテナーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="87df6-134">We need to create a container called **samples-workitems**.</span></span>

1. <span data-ttu-id="87df6-135">Storage Explorer で、ストレージ アカウントを展開します。</span><span class="sxs-lookup"><span data-stu-id="87df6-135">In Storage explorer, expand your storage account.</span></span> <span data-ttu-id="87df6-136">名前は、接続プロセス中に指定した **[表示名]** となります。</span><span class="sxs-lookup"><span data-stu-id="87df6-136">The name should be the **Display name** that you provided during the connection process.</span></span>

1. <span data-ttu-id="87df6-137">**[BLOB コンテナー]** を右クリックし、**[BLOB コンテナーの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-137">Right-click **Blob Containers** and select **Create blob container**.</span></span>

1. <span data-ttu-id="87df6-138">「**samples-workitems**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="87df6-138">Enter **samples-workitems**.</span></span>

## <a name="turn-on-your-blob-trigger"></a><span data-ttu-id="87df6-139">BLOB トリガーを有効にする</span><span class="sxs-lookup"><span data-stu-id="87df6-139">Turn on your blob trigger</span></span>

<span data-ttu-id="87df6-140">これで、監視するコンテナーが作成されたので、BLOB の作成時に出力を確認するために関数を実行してみましょう。</span><span class="sxs-lookup"><span data-stu-id="87df6-140">Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.</span></span>

1. <span data-ttu-id="87df6-141">BLOB トリガーを選択してコード画面を開きます。</span><span class="sxs-lookup"><span data-stu-id="87df6-141">Select your blob trigger to open the code screen.</span></span>

1. <span data-ttu-id="87df6-142">**[実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-142">Select **Run**.</span></span>

## <a name="create-a-blob"></a><span data-ttu-id="87df6-143">BLOB を作成する</span><span class="sxs-lookup"><span data-stu-id="87df6-143">Create a blob</span></span>

<span data-ttu-id="87df6-144">BLOB トリガーが有効になっており、アクティビティをリッスンしています。</span><span class="sxs-lookup"><span data-stu-id="87df6-144">Our blob trigger is now up and listening for activity.</span></span> <span data-ttu-id="87df6-145">ログ メッセージを取得しているかどうかを確認するために、BLOB を作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="87df6-145">Let's create a blob to see if we get a log message.</span></span>

1. <span data-ttu-id="87df6-146">Storage Explorer で、**[samples-workitems]** コンテナーを選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-146">In Storage explorer, select the **samples-workitems** container.</span></span>

1. <span data-ttu-id="87df6-147">**[アップロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-147">Select **Upload**.</span></span> 

1. <span data-ttu-id="87df6-148">**[ファイルのアップロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-148">Select **Upload Files**.</span></span>

1. <span data-ttu-id="87df6-149">コンピューターから任意のファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-149">Select any file from your computer.</span></span>

1. <span data-ttu-id="87df6-150">**[アップロード]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-150">Select **Upload**.</span></span>

1. <span data-ttu-id="87df6-151">Azure に戻ります。</span><span class="sxs-lookup"><span data-stu-id="87df6-151">Go back to Azure.</span></span> <span data-ttu-id="87df6-152">ログを確認して、アップロードされたファイルを示すメッセージを見つけます。</span><span class="sxs-lookup"><span data-stu-id="87df6-152">Check your logs for a message that displays what file was uploaded.</span></span>

## <a name="clean-up"></a><span data-ttu-id="87df6-153">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="87df6-153">Clean up</span></span>

<span data-ttu-id="87df6-154">この関数に対して課金されないように、ログ ウィンドウの上にある **[一時停止]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="87df6-154">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>

![一時停止](../media-drafts/4-pause-timer.png)


