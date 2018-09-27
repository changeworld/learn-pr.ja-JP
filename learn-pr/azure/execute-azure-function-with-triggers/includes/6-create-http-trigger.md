<span data-ttu-id="dadcb-101">このユニットでは、単一の文字列を含む HTTP 要求を受け取る Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-101">In this unit, we're going to create an Azure function that accepts an HTTP request with a single string.</span></span> <span data-ttu-id="dadcb-102">この関数では、成功または失敗を表す文字列が呼び出し元に返されます。</span><span class="sxs-lookup"><span data-stu-id="dadcb-102">The function returns a string back to the caller to represent success or failure.</span></span> <span data-ttu-id="dadcb-103">前の演習からの関数に引き続き取り組みます。</span><span class="sxs-lookup"><span data-stu-id="dadcb-103">We'll continue working on the function from the previous exercise.</span></span>

## <a name="create-an-http-trigger"></a><span data-ttu-id="dadcb-104">HTTP トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="dadcb-104">Create an HTTP trigger</span></span>

<span data-ttu-id="dadcb-105">既存の Azure Functions アプリケーションを引き続き使用し、HTTP トリガーを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="dadcb-105">Let's continue using our existing Azure Functions application and add an HTTP trigger.</span></span>

1. <span data-ttu-id="dadcb-106">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="dadcb-106">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="dadcb-107">**[すべてのリソース]** 画面に移動し、関数アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-107">Navigate to the **All resources** screen and select your function app.</span></span>

1. <span data-ttu-id="dadcb-108">**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-108">Point to **Functions** and select the plus (+) icon.</span></span>

1. <span data-ttu-id="dadcb-109">**[HTTP トリガー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-109">Select **HTTP trigger**.</span></span>

1. <span data-ttu-id="dadcb-110">言語として **[C#]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-110">Select **C#** as the language.</span></span>

1. <span data-ttu-id="dadcb-111">**[名前]** は既定値のままにします。</span><span class="sxs-lookup"><span data-stu-id="dadcb-111">Leave the **Name** set to the default value.</span></span>

1. <span data-ttu-id="dadcb-112">**[承認レベル]** を **[匿名]** に設定します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-112">Set the **Authorization level** to **Anonymous**.</span></span>

1. <span data-ttu-id="dadcb-113">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-113">Select **Create**.</span></span>

1. <span data-ttu-id="dadcb-114">自動生成されたコードを見て、実行されている処理を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="dadcb-114">Take a quick look at the auto-generated code to get an idea about what's going on.</span></span> <span data-ttu-id="dadcb-115">*req* パラメーターは着信要求を表し、*name* パラメーターを含みます。</span><span class="sxs-lookup"><span data-stu-id="dadcb-115">The *req* parameter represents the incoming request and contains a *name* parameter.</span></span> <span data-ttu-id="dadcb-116">*name* に値があるかどうかを確認します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-116">We check to see if *name* has a value.</span></span> <span data-ttu-id="dadcb-117">ある場合はあいさつを返します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-117">If it does, we return a greeting.</span></span> <span data-ttu-id="dadcb-118">ない場合は、エラー メッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-118">Otherwise, we return an error message.</span></span>

## <a name="get-your-function-url"></a><span data-ttu-id="dadcb-119">関数の URL を取得する</span><span class="sxs-lookup"><span data-stu-id="dadcb-119">Get your function URL</span></span>

<span data-ttu-id="dadcb-120">HTTP トリガーを作成したので、要求を開始できるように関数の URL を取得してみましょう。</span><span class="sxs-lookup"><span data-stu-id="dadcb-120">Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.</span></span>

1. <span data-ttu-id="dadcb-121">HTTP トリガーを選択してコード画面を開きます。</span><span class="sxs-lookup"><span data-stu-id="dadcb-121">Select your HTTP trigger to open the code screen.</span></span>

1. <span data-ttu-id="dadcb-122">**[実行]** の右側にある **[関数の URL の取得]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-122">To the right of **Run**, select **Get function URL**.</span></span>

1. <span data-ttu-id="dadcb-123">**[コピー]** を選択し、関数 URL ポップアップを閉じます。</span><span class="sxs-lookup"><span data-stu-id="dadcb-123">Select **Copy**, then close the function URL popup.</span></span>

## <a name="issue-a-get-request-to-your-http-trigger"></a><span data-ttu-id="dadcb-124">HTTP トリガーに対して GET 要求を発行する</span><span class="sxs-lookup"><span data-stu-id="dadcb-124">Issue a GET request to your HTTP trigger</span></span>

<span data-ttu-id="dadcb-125">関数 URL はクリップボードにコピーされている状態です。</span><span class="sxs-lookup"><span data-stu-id="dadcb-125">We now have our function URL copied to our clipboard.</span></span> <span data-ttu-id="dadcb-126">GET 要求を発行して、応答があるかどうかを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="dadcb-126">Let's issue a GET request to see if we get a response.</span></span>

1. <span data-ttu-id="dadcb-127">Web ブラウザーで新しいタブを開きます。</span><span class="sxs-lookup"><span data-stu-id="dadcb-127">Open a new tab in your web browser.</span></span>

1. <span data-ttu-id="dadcb-128">アドレス バーに URL を貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="dadcb-128">Paste the URL into the address bar.</span></span>

1. <span data-ttu-id="dadcb-129">たとえば、*name* というクエリ文字列パラメーターを自分の名前で追加します。たとえば、`.../api/HttpTriggerCSharp1?name=Jesse` にします。</span><span class="sxs-lookup"><span data-stu-id="dadcb-129">Add a query string parameter called *name* with your name for example `.../api/HttpTriggerCSharp1?name=Jesse`</span></span>

1. <span data-ttu-id="dadcb-130"><kbd>Enter</kbd> キーを押して要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="dadcb-130">Press <kbd>ENTER</kbd> to submit the request.</span></span>
