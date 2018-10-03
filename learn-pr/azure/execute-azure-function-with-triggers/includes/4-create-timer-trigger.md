<span data-ttu-id="bde82-101">このユニットでは、タイマー トリガーを使用して 20 秒ごとに呼び出される Azure 関数アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="bde82-101">In this unit, we create an Azure function app that's invoked every 20 seconds using a timer trigger.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="bde82-102">Azure 関数アプリの作成</span><span class="sxs-lookup"><span data-stu-id="bde82-102">Create an Azure function app</span></span>

<span data-ttu-id="bde82-103">それではまず、ポータルで Azure 関数アプリを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="bde82-103">Let’s start by creating an Azure Function app in the portal.</span></span>

1. <span data-ttu-id="bde82-104">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="bde82-104">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="bde82-105">左側のナビゲーションで、**[リソースの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-105">In the left navigation, select **Create a resource**.</span></span>

1. <span data-ttu-id="bde82-106">**[コンピューティング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-106">Select **Compute**.</span></span>

1. <span data-ttu-id="bde82-107">**[Function App]** を検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-107">Locate and select **Function App**.</span></span> <span data-ttu-id="bde82-108">必要に応じて検索バーを使用してテンプレートを検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="bde82-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Azure portal のスクリーンショット。[リソースの作成] ブレードで Function App が強調表示されています。](../media/4-click-function-app.png)

1. <span data-ttu-id="bde82-110">グローバルに一意の**アプリ名**を入力します。</span><span class="sxs-lookup"><span data-stu-id="bde82-110">Enter a globally unique **App name**.</span></span>

1. <span data-ttu-id="bde82-111">**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-111">Select a **Subscription**.</span></span>

1. <span data-ttu-id="bde82-112"><rgn>[サンドボックス リソース グループ名]</rgn> という既存の**リソース グループ**を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-112">Select the existing **Resource group** <rgn>[sandbox resource group name]</rgn>.</span></span>

1. <span data-ttu-id="bde82-113">使用する **OS** として **[Windows]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-113">Choose **Windows** as your **OS**.</span></span>

1. <span data-ttu-id="bde82-114">使用する**ホスティング プラン**には、**[従量課金プラン]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="bde82-115">ご利用の関数を実行するたびに課金されます。</span><span class="sxs-lookup"><span data-stu-id="bde82-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="bde82-116">ご利用のアプリケーション ワークロードに基づいてリソースが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="bde82-116">Resources are automatically allocated based on your application workload.</span></span>

1. <span data-ttu-id="bde82-117">次の使用可能なリストから**場所**を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-117">Select a **Location** from the available list below.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

1. <span data-ttu-id="bde82-118">**[ランタイム スタック]** は、既定の *[.NET]* のままにしておきます。これは、この演習で関数の例を実装する際に使用する言語です。</span><span class="sxs-lookup"><span data-stu-id="bde82-118">For **Runtime Stack**, leave as default *.NET*, which is the language in which we implement the function examples in this exercise.</span></span>

1. <span data-ttu-id="bde82-119">新しい**ストレージ** アカウントを作成します。名前は必要に応じて変更できます。既定では、アプリ名に基づく名前になります。</span><span class="sxs-lookup"><span data-stu-id="bde82-119">Create a new **Storage** account, you can change the name if you like - it will default to a variation of the App name.</span></span>

1. <span data-ttu-id="bde82-120">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-120">Select **Create**.</span></span> <span data-ttu-id="bde82-121">関数アプリがデプロイされたら、ポータルの **[すべてのリソース]** に移動します。</span><span class="sxs-lookup"><span data-stu-id="bde82-121">Once the function app is deployed, go to **All resources** in the portal.</span></span> <span data-ttu-id="bde82-122">関数アプリは、**[App Service]** という種類でリストされ、指定した名前になります。</span><span class="sxs-lookup"><span data-stu-id="bde82-122">The function app will be listed with type **App Service** and has the name you gave it.</span></span>
 
<!-- Start temporary fix for issue #2498. -->
> [!IMPORTANT]
> <span data-ttu-id="bde82-123">現時点では、このモジュールの演習は Azure Functions V1 を使用して行います。</span><span class="sxs-lookup"><span data-stu-id="bde82-123">The exercises in this module currently work with Azure Functions V1.</span></span> <span data-ttu-id="bde82-124">以下の手順に慎重に従って、関数アプリで V1 ランタイム バージョンが使用されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="bde82-124">Please follow these steps carefully to make sure your function app uses the V1 runtime version.</span></span> 

1. <span data-ttu-id="bde82-125">関数アプリが作成されたら、左側のナビゲーションから **[すべてのリソース]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-125">After the function app is created, select **All resources** from the left navigation.</span></span>

1. <span data-ttu-id="bde82-126">**[Function Apps]** リストで、関数アプリを選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-126">Select your function app in the **Function Apps** list.</span></span>
1. <span data-ttu-id="bde82-127">**[プラットフォーム機能]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-127">Select **Platform features**.</span></span>
1. <span data-ttu-id="bde82-128">**[プラットフォーム機能]** 画面の **[全般設定]** で、**[Function App の設定]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-128">In the **Platform features** screen, select **Function app settings** under **General Settings**.</span></span>
1. <span data-ttu-id="bde82-129">**[ランタイム バージョン]** で *[~1]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-129">Select *~1* in the **Runtime version** .</span></span>
1. <span data-ttu-id="bde82-130">**[Function App の設定]** を閉じます。</span><span class="sxs-lookup"><span data-stu-id="bde82-130">Close **Function app settings**.</span></span>

<span data-ttu-id="bde82-131">これで、Azure Functions V1 ランタイムを使用するように関数アプリが構成されました。</span><span class="sxs-lookup"><span data-stu-id="bde82-131">Our function app is now configured to use the Azure Functions V1 runtime.</span></span> <span data-ttu-id="bde82-132">最初の関数の作成を続行できます。</span><span class="sxs-lookup"><span data-stu-id="bde82-132">We can now continue to create our first function.</span></span>
<!-- End temporary fix for issue #2498. --> 

## <a name="create-a-timer-trigger"></a><span data-ttu-id="bde82-133">タイマー トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="bde82-133">Create a timer trigger</span></span>

<span data-ttu-id="bde82-134">次に、関数内にタイマー トリガーを作成します。</span><span class="sxs-lookup"><span data-stu-id="bde82-134">Now we're going to create a timer trigger inside our function.</span></span>



1. <span data-ttu-id="bde82-135">新しいブレード上で、**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-135">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![Azure portal のスクリーンショット。[関数アプリ] ブレードで [関数] サブメニューの追加 (+) ボタンが強調表示されています。](../media/4-hover-function.png)

1. <span data-ttu-id="bde82-137">**[タイマー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-137">Select **Timer**.</span></span>

1. <span data-ttu-id="bde82-138">**[この関数を作成する]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-138">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="bde82-139">タイマー トリガーを構成する</span><span class="sxs-lookup"><span data-stu-id="bde82-139">Configure the timer trigger</span></span>

<span data-ttu-id="bde82-140">ログ ウィンドウにメッセージを出力するロジックを持つ Azure 関数アプリを用意しています。</span><span class="sxs-lookup"><span data-stu-id="bde82-140">We have an Azure function app with logic to print a message to the log window.</span></span> <span data-ttu-id="bde82-141">20 秒ごとに実行されるようにタイマーのスケジュールを設定することにします。</span><span class="sxs-lookup"><span data-stu-id="bde82-141">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="bde82-142">**[統合]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-142">Select **Integrate**.</span></span>

1. <span data-ttu-id="bde82-143">**[スケジュール]** ボックスに次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="bde82-143">Enter the following value into the **Schedule** box:</span></span>

    ```log
    */20 * * * * *
    ```

1. <span data-ttu-id="bde82-144">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-144">Select **Save**.</span></span>

## <a name="test-the-timer"></a><span data-ttu-id="bde82-145">タイマーをテストする</span><span class="sxs-lookup"><span data-stu-id="bde82-145">Test the timer</span></span>

<span data-ttu-id="bde82-146">タイマーを設定したので、定義した間隔で関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="bde82-146">Now that we've configured the timer, it will invoke the function on the interval we defined.</span></span>

1. <span data-ttu-id="bde82-147">**[TimerTriggerCSharp1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="bde82-147">Select **TimerTriggerCSharp1**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bde82-148">**[TimerTriggerCSharp1]** は既定の名前です。</span><span class="sxs-lookup"><span data-stu-id="bde82-148">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="bde82-149">これは、トリガーを作成するとき自動的に選択されます。</span><span class="sxs-lookup"><span data-stu-id="bde82-149">It's automatically selected when you create the trigger.</span></span>

1. <span data-ttu-id="bde82-150">画面の下部にある **[ログ]** パネルを開きます。</span><span class="sxs-lookup"><span data-stu-id="bde82-150">Open the **Logs** panel at the bottom of the screen.</span></span>

1. <span data-ttu-id="bde82-151">20 秒ごとに新しいメッセージが到着するのをログ ウィンドウで観察してください。</span><span class="sxs-lookup"><span data-stu-id="bde82-151">Observe new messages arrive every 20 seconds in the log window.</span></span>

1. <span data-ttu-id="bde82-152">関数の実行を停止するには、**[管理]** を選択し、**[関数の状態]** を *[無効]* に切り替えます。</span><span class="sxs-lookup"><span data-stu-id="bde82-152">To stop the function from running, select **Manage** and then switch **Function State** to *Disabled*.</span></span>
