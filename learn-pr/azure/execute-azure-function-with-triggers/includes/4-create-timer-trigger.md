<span data-ttu-id="c9588-101">このユニットでは、タイマー トリガーを使用して 20 秒ごとに呼び出される Azure 関数アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="c9588-101">In this unit, we create an Azure function app that's invoked every 20 seconds using a timer trigger.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="c9588-102">Azure 関数アプリの作成</span><span class="sxs-lookup"><span data-stu-id="c9588-102">Create an Azure function app</span></span>

<span data-ttu-id="c9588-103">それではまず、ポータルで Azure 関数アプリを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="c9588-103">Let’s start by creating an Azure Function app in the portal.</span></span>

1. <span data-ttu-id="c9588-104">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="c9588-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="c9588-105">左側のナビゲーションで、**[リソースの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-105">In the left navigation, select **Create a resource**.</span></span>

1. <span data-ttu-id="c9588-106">**[コンピューティング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-106">Select **Compute**.</span></span>

1. <span data-ttu-id="c9588-107">**[Function App]** を検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-107">Locate and select **Function App**.</span></span> <span data-ttu-id="c9588-108">必要に応じて検索バーを使用してテンプレートを検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="c9588-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Azure portal のスクリーンショット。[リソースの作成] ブレードで Function App が強調表示されています。](../media/4-click-function-app.png)

1. <span data-ttu-id="c9588-110">グローバルに一意の**アプリ名**を入力します。</span><span class="sxs-lookup"><span data-stu-id="c9588-110">Enter a globally unique **App name**.</span></span>

1. <span data-ttu-id="c9588-111">**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-111">Select a **Subscription**.</span></span>

1. <span data-ttu-id="c9588-112"><rgn>[サンドボックス リソース グループ名]</rgn> という既存の**リソース グループ**を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-112">Select the existing **Resource group** <rgn>[sandbox resource group name]</rgn>.</span></span>

1. <span data-ttu-id="c9588-113">使用する **OS** として **[Windows]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-113">Choose **Windows** as your **OS**.</span></span>

1. <span data-ttu-id="c9588-114">使用する**ホスティング プラン**には、**[従量課金プラン]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="c9588-115">ご利用の関数を実行するたびに課金されます。</span><span class="sxs-lookup"><span data-stu-id="c9588-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="c9588-116">ご利用のアプリケーション ワークロードに基づいてリソースが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="c9588-116">Resources are automatically allocated based on your application workload.</span></span>

1. <span data-ttu-id="c9588-117">次の利用可能な一覧から**場所**を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-117">Select a **Location** from the available list below.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="c9588-118">新しい**ストレージ** アカウントを作成します。名前は自由に変更できます。変更しない場合、アプリ名に基づく名前になります。</span><span class="sxs-lookup"><span data-stu-id="c9588-118">Create a new **Storage** account, you can change the name if you like - it will default to a variation of the App name.</span></span>

1. <span data-ttu-id="c9588-119">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-119">Select **Create**.</span></span> <span data-ttu-id="c9588-120">関数アプリがデプロイされたら、ポータルの **[すべてのリソース]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="c9588-120">Once the function app is deployed, go to **All resources** in the portal.</span></span> <span data-ttu-id="c9588-121">関数アプリはタイプ **[App Service]** の一覧に表示され、指定した名前が付けられます。</span><span class="sxs-lookup"><span data-stu-id="c9588-121">The function app will be listed with type **App Service** and has the name you gave it.</span></span>

## <a name="create-a-timer-trigger"></a><span data-ttu-id="c9588-122">タイマー トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="c9588-122">Create a timer trigger</span></span>

<span data-ttu-id="c9588-123">次に、関数内にタイマー トリガーを作成します。</span><span class="sxs-lookup"><span data-stu-id="c9588-123">Now we're going to create a timer trigger inside our function.</span></span>

1. <span data-ttu-id="c9588-124">関数が作成されたら、左側のナビゲーションから **[すべてのリソース]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-124">After the function is created, select **All resources** from the left navigation.</span></span>

1. <span data-ttu-id="c9588-125">一覧で関数アプリを見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-125">Find your function app in the list and select it.</span></span>

1. <span data-ttu-id="c9588-126">新しいブレード上で、**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-126">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![Azure portal のスクリーンショット。[関数アプリ] ブレードで [関数] サブメニューの追加 (+) ボタンが強調表示されています。](../media/4-hover-function.png)

1. <span data-ttu-id="c9588-128">**[タイマー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-128">Select **Timer**.</span></span>

1. <span data-ttu-id="c9588-129">言語として **[CSharp]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-129">Select **CSharp** as the language.</span></span>

1. <span data-ttu-id="c9588-130">**[この関数を作成する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="c9588-130">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="c9588-131">タイマー トリガーを構成する</span><span class="sxs-lookup"><span data-stu-id="c9588-131">Configure the timer trigger</span></span>

<span data-ttu-id="c9588-132">ログ ウィンドウにメッセージを出力するロジックを持つ Azure 関数アプリを用意しています。</span><span class="sxs-lookup"><span data-stu-id="c9588-132">We have an Azure function app with logic to print a message to the log window.</span></span> <span data-ttu-id="c9588-133">20 秒ごとに実行されるようにタイマーのスケジュールを設定することにします。</span><span class="sxs-lookup"><span data-stu-id="c9588-133">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="c9588-134">**[統合]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-134">Select **Integrate**.</span></span>

1. <span data-ttu-id="c9588-135">**[スケジュール]** ボックスに次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="c9588-135">Enter the following value into the **Schedule** box:</span></span>

    ```log
    */20 * * * * *
    ```

1. <span data-ttu-id="c9588-136">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-136">Select **Save**.</span></span>

## <a name="test-the-timer"></a><span data-ttu-id="c9588-137">タイマーをテストする</span><span class="sxs-lookup"><span data-stu-id="c9588-137">Test the timer</span></span>

<span data-ttu-id="c9588-138">タイマーを設定したので、定義した間隔で関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="c9588-138">Now that we've configured the timer, it will invoke the function on the interval we defined.</span></span>

1. <span data-ttu-id="c9588-139">**[TimerTriggerCSharp1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="c9588-139">Select **TimerTriggerCSharp1**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c9588-140">**[TimerTriggerCSharp1]** は既定の名前です。</span><span class="sxs-lookup"><span data-stu-id="c9588-140">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="c9588-141">これは、トリガーを作成するとき自動的に選択されます。</span><span class="sxs-lookup"><span data-stu-id="c9588-141">It's automatically selected when you create the trigger.</span></span>

1. <span data-ttu-id="c9588-142">画面の下部にある **[ログ]** パネルを開きます。</span><span class="sxs-lookup"><span data-stu-id="c9588-142">Open the **Logs** panel at the bottom of the screen.</span></span>

1. <span data-ttu-id="c9588-143">20 秒ごとに新しいメッセージが到着するのをログ ウィンドウで観察してください。</span><span class="sxs-lookup"><span data-stu-id="c9588-143">Observe new messages arrive every 20 seconds in the log window.</span></span>

1. <span data-ttu-id="c9588-144">関数の実行を停止するには、**[管理]** を選択し、**[関数の状態]** を *[無効]* に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="c9588-144">To stop the function from running, select **Manage** and then switch **Function State** to *Disabled*.</span></span>
