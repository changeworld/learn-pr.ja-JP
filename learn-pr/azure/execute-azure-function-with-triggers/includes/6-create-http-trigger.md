<span data-ttu-id="f8214-101">この演習では、単一の文字列を含む HTTP 要求を受け取る Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="f8214-101">In this exercise, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="f8214-102">この関数は、成功または失敗を表す文字列を呼び出し元に返します。</span><span class="sxs-lookup"><span data-stu-id="f8214-102">The function returns a string back to the caller to represent success or failure.</span></span>

> [!NOTE]
> <span data-ttu-id="f8214-103">この演習を完了するには、有効なアカウントで [Azure portal](https://portal.azure.com?azure-portal=true) にサインインしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="f8214-103">To complete this exercise, make sure you're signed into the [Azure portal](https://portal.azure.com?azure-portal=true) with a valid account.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="f8214-104">HTTP トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="f8214-104">Create an HTTP trigger</span></span>

<span data-ttu-id="f8214-105">既存の Azure Functions アプリケーションを引き続き使用し、HTTP トリガーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f8214-105">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="f8214-106">**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="f8214-106">Point to **Functions** and select the plus (+) icon.</span></span>

    ![[関数] をポイントし、プラス ホバーを選択する](../media-drafts/4-hover-function.png)

2. <span data-ttu-id="f8214-108">**[HTTP トリガー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8214-108">Select **HTTP trigger**.</span></span>

3. <span data-ttu-id="f8214-109">言語として **[C#]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8214-109">Select **C#** as the language.</span></span> 

4. <span data-ttu-id="f8214-110">**[名前]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="f8214-110">Leave the **Name** set to the default value.</span></span>

5. <span data-ttu-id="f8214-111">**[承認レベル]** を **[匿名]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="f8214-111">Set the **Authorization level** to **Anonymous**.</span></span>

6. <span data-ttu-id="f8214-112">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8214-112">Select **Create**.</span></span>

7. <span data-ttu-id="f8214-113">自動生成されたコードを見て、実行されている処理を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="f8214-113">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="f8214-114">*req* パラメーターは着信要求を表し、*name* パラメーターを含みます。</span><span class="sxs-lookup"><span data-stu-id="f8214-114">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="f8214-115">*name* に値があるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="f8214-115">We check to see if *name* has a value.</span></span> <span data-ttu-id="f8214-116">ある場合はあいさつを返します。</span><span class="sxs-lookup"><span data-stu-id="f8214-116">If it does, we return a greeting.</span></span> <span data-ttu-id="f8214-117">ない場合は、エラー メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="f8214-117">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="f8214-118">関数の URL を取得する</span><span class="sxs-lookup"><span data-stu-id="f8214-118">Get your function URL</span></span>

<span data-ttu-id="f8214-119">HTTP トリガーを作成したので、要求を開始できるように関数の URL を取得してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f8214-119">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="f8214-120">HTTP トリガーを選択してコード画面を開きます。</span><span class="sxs-lookup"><span data-stu-id="f8214-120">Select your HTTP trigger to open the code screen.</span></span>

2. <span data-ttu-id="f8214-121">**[実行]** の右側にある **[関数の URL の取得]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8214-121">To the right of **Run**, select **Get function URL**.</span></span>

3. <span data-ttu-id="f8214-122">**[コピー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8214-122">Select **Copy**.</span></span>

4. <span data-ttu-id="f8214-123">**[実行]** を選択して関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="f8214-123">Select **Run** to start your function.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="f8214-124">HTTP トリガーに対して GET 要求を発行する</span><span class="sxs-lookup"><span data-stu-id="f8214-124">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="f8214-125">関数 URL はクリップボードにコピーされている状態です。</span><span class="sxs-lookup"><span data-stu-id="f8214-125">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="f8214-126">GET 要求を発行して、応答があるかどうかを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="f8214-126">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="f8214-127">Web ブラウザーで新しいタブを開きます。</span><span class="sxs-lookup"><span data-stu-id="f8214-127">Open a new tab in your web browser.</span></span>

2. <span data-ttu-id="f8214-128">アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f8214-128">Paste the URL into the address bar.</span></span>

3. <span data-ttu-id="f8214-129">たとえば、`.../api/HttpTriggerCSharp1?name=Jesse` のように、*name* という名前のクエリ文字列パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="f8214-129">Add a query string parameter called *name* with your name for example `.../api/HttpTriggerCSharp1?name=Jesse`</span></span>

4. <span data-ttu-id="f8214-130">Enter キーを押して要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="f8214-130">Select ENTER to submit the request.</span></span>

## <a name="clean-up"></a><span data-ttu-id="f8214-131">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="f8214-131">Clean up</span></span>

<span data-ttu-id="f8214-132">この関数に対して課金されないように、ログ ウィンドウの上にある **[一時停止]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8214-132">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>

![一時停止](../media-drafts/4-pause-timer.png)