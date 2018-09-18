<span data-ttu-id="8fe4c-101">このユニットでは、単一の文字列を含む HTTP 要求を受け取る Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-101">In this unit, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="8fe4c-102">この関数では、成功または失敗を表す文字列が呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-102">The function returns a string back to the caller to represent success or failure.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="8fe4c-103">HTTP トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="8fe4c-103">Create an HTTP trigger</span></span>

<span data-ttu-id="8fe4c-104">既存の Azure Functions アプリケーションを引き続き使用し、HTTP トリガーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-104">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="8fe4c-105">[Azure portal](https://portal.azure.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-105">Sign into the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="8fe4c-106">**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-106">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="8fe4c-107">**[HTTP トリガー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-107">Select **HTTP trigger**.</span></span>

1. <span data-ttu-id="8fe4c-108">言語として **[C#]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-108">Select **C#** as the language.</span></span>

1. <span data-ttu-id="8fe4c-109">**[名前]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-109">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="8fe4c-110">**[承認レベル]** を **[匿名]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-110">Set the **Authorization level** to **Anonymous**.</span></span>

1. <span data-ttu-id="8fe4c-111">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-111">Select **Create**.</span></span>

1. <span data-ttu-id="8fe4c-112">自動生成されたコードを見て、実行されている処理を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-112">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="8fe4c-113">*req* パラメーターは着信要求を表し、*name* パラメーターを含みます。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-113">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="8fe4c-114">*name* に値があるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-114">We check to see if *name* has a value.</span></span> <span data-ttu-id="8fe4c-115">ある場合はあいさつを返します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-115">If it does, we return a greeting.</span></span> <span data-ttu-id="8fe4c-116">ない場合は、エラー メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-116">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="8fe4c-117">関数の URL を取得する</span><span class="sxs-lookup"><span data-stu-id="8fe4c-117">Get your function URL</span></span>

<span data-ttu-id="8fe4c-118">HTTP トリガーを作成したので、要求を開始できるように関数の URL を取得してみましょう。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-118">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="8fe4c-119">HTTP トリガーを選択してコード画面を開きます。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-119">Select your HTTP trigger to open the code screen.</span></span>

1. <span data-ttu-id="8fe4c-120">**[実行]** の右側にある **[関数の URL の取得]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-120">To the right of **Run**, select **Get function URL**.</span></span>

1. <span data-ttu-id="8fe4c-121">**[コピー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-121">Select **Copy**.</span></span>

1. <span data-ttu-id="8fe4c-122">**[実行]** を選択して関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-122">Select **Run** to start your function.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="8fe4c-123">HTTP トリガーに対して GET 要求を発行する</span><span class="sxs-lookup"><span data-stu-id="8fe4c-123">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="8fe4c-124">関数 URL はクリップボードにコピーされている状態です。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-124">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="8fe4c-125">GET 要求を発行して、応答があるかどうかを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-125">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="8fe4c-126">Web ブラウザーで新しいタブを開きます。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-126">Open a new tab in your web browser.</span></span>

1. <span data-ttu-id="8fe4c-127">アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-127">Paste the URL into the address bar.</span></span>

1. <span data-ttu-id="8fe4c-128">たとえば、`.../api/HttpTriggerCSharp1?name=Jesse` のように、*name* という名前のクエリ文字列パラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-128">Add a query string parameter called *name* with your name for example `.../api/HttpTriggerCSharp1?name=Jesse`</span></span>

1. <span data-ttu-id="8fe4c-129">Enter キーを押して要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-129">Select ENTER to submit the request.</span></span>

## <a name="clean-up"></a><span data-ttu-id="8fe4c-130">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="8fe4c-130">Clean up</span></span>
<!---TODO: Update for sandbox?--->

<span data-ttu-id="8fe4c-131">この関数に対して課金されないように、ログ ウィンドウの上にある **[一時停止]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="8fe4c-131">To ensure that you aren't charged for this function, select **Pause** above the log window.</span></span>
