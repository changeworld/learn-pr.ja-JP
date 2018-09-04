<span data-ttu-id="e9ea9-101">この演習では、タイマー トリガーを使用して 20 秒ごとに呼び出される Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-101">In this exercise, we create an Azure function that's invoked every 20 seconds using a timer trigger.</span></span>

> [!NOTE] 
> <span data-ttu-id="e9ea9-102">この演習を完了するには、有効なアカウントを使用して [Azure Portal](https://portal.azure.com?azure-portal=true) にログインしていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-102">To complete this exercise, make sure you're logged into the [Azure portal](https://portal.azure.com?azure-portal=true) with a valid account.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="e9ea9-103">Azure 関数を作成する</span><span class="sxs-lookup"><span data-stu-id="e9ea9-103">Create an Azure function</span></span>

<span data-ttu-id="e9ea9-104">ポータルに Azure 関数を作成することから始めましょう。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-104">Let’s start by creating an Azure Function in the portal.</span></span>

1. <span data-ttu-id="e9ea9-105">左側のナビゲーションで、**[リソースの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-105">In the left navigation, select **Create a resource**.</span></span>

2. <span data-ttu-id="e9ea9-106">**[コンピューティング]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-106">Select **Compute**.</span></span>

3. <span data-ttu-id="e9ea9-107">**[Function App]** を検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-107">Locate and select **Function App**.</span></span> <span data-ttu-id="e9ea9-108">必要に応じて検索バーを使用してテンプレートを検索することもできます。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-108">You can also optionally use the search bar to locate the template.</span></span>

    ![Function App を選択する](../media-drafts/4-click-function-app.png)

4. <span data-ttu-id="e9ea9-110">一意の**アプリ名**を入力します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-110">Enter a unique **App name**.</span></span>

5. <span data-ttu-id="e9ea9-111">**サブスクリプション**を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-111">Select a **Subscription**.</span></span>

6. <span data-ttu-id="e9ea9-112">新しい**リソース グループ**を作成します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-112">Create a new **Resource Group**.</span></span>

7. <span data-ttu-id="e9ea9-113">使用する **OS** として **[Windows]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-113">Choose **Windows** as your **OS**.</span></span>

8. <span data-ttu-id="e9ea9-114">使用する**ホスティング プラン**には、**[従量課金プラン]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-114">Choose **Consumption Plan** for your **Hosting Plan**.</span></span> <span data-ttu-id="e9ea9-115">ご利用の関数を実行するたびに課金されます。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-115">You're charged for each execution of your function.</span></span> <span data-ttu-id="e9ea9-116">ご利用のアプリケーション ワークロードに基づいてリソースが自動的に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-116">Resources are automatically allocated based on your application workload.</span></span>

9. <span data-ttu-id="e9ea9-117">**[場所]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-117">Select a **Location**.</span></span>

10. <span data-ttu-id="e9ea9-118">新しい**ストレージ** アカウントを作成します。既定はアプリ名のバリエーションですが、名前は任意で変更することができます。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-118">Create a new **Storage** account, you can change the name if you like - it will default to a variation of the App name</span></span>

11. <span data-ttu-id="e9ea9-119">**[Application Insights]** をオフにします。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-119">Turn off **Application Insights**.</span></span>

12. <span data-ttu-id="e9ea9-120">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-120">Select **Create**.</span></span> <span data-ttu-id="e9ea9-121">これの完了には数分かかります。リソースの作成が完了すると、ツールバー領域の **[通知]** アイコンには、Azure Portal で開くためのボタンが作成されることが確認できます。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-121">This will take a few minutes to complete, you can watch the **Notifications** icon in the toolbar area - once it has finished creating the resource it will have a button there to open it in the Azure Portal.</span></span>

## <a name="create-a-timer-trigger"></a><span data-ttu-id="e9ea9-122">タイマー トリガーを作成する</span><span class="sxs-lookup"><span data-stu-id="e9ea9-122">Create a timer trigger</span></span>

<span data-ttu-id="e9ea9-123">ここで、Azure 関数内にタイマー トリガーを作成します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-123">Now we're going to create a timer trigger inside our Azure function.</span></span>

1. <span data-ttu-id="e9ea9-124">Azure 関数が作成されたら、左側のナビゲーションから **[すべてのリソース]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-124">After the Azure function is created, select **All resources** from the left navigation.</span></span>

2. <span data-ttu-id="e9ea9-125">使用する Azure 関数を検索し、選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-125">Locate and select your Azure function.</span></span>

3. <span data-ttu-id="e9ea9-126">新しいブレード上で、**[関数]** をポイントし、プラス (+) アイコンを選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-126">On the new blade, point to **Functions** and select the plus (+) icon.</span></span>

    ![[関数] をポイントし、プラスを選択する](../media-drafts/4-hover-function.png)

4. <span data-ttu-id="e9ea9-128">**[タイマー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-128">Select **Timer**.</span></span>

5. <span data-ttu-id="e9ea9-129">言語として **[CSharp]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-129">Select **CSharp** as the language.</span></span>

6. <span data-ttu-id="e9ea9-130">**[この関数を作成する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-130">Select **Create this function**.</span></span>

## <a name="configure-the-timer-trigger"></a><span data-ttu-id="e9ea9-131">タイマー トリガーを構成する</span><span class="sxs-lookup"><span data-stu-id="e9ea9-131">Configure the timer trigger</span></span>

<span data-ttu-id="e9ea9-132">ログ ウィンドウにメッセージを出力するロジックが備わった Azure 関数があります。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-132">We have an Azure function with logic to print a message to the log window.</span></span> <span data-ttu-id="e9ea9-133">20 秒ごとに実行されるようにタイマーのスケジュールを設定することにします。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-133">We're going to set the schedule of the timer to execute every 20 seconds.</span></span>

1. <span data-ttu-id="e9ea9-134">**[統合]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-134">Select **Integrate**.</span></span>

2. <span data-ttu-id="e9ea9-135">**[スケジュール]** ボックスに次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-135">Enter the following value into the **Schedule** box:</span></span>

    ```
    */20 * * * * *
    ```

3. <span data-ttu-id="e9ea9-136">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-136">Select **Save**.</span></span>

## <a name="start-the-timer"></a><span data-ttu-id="e9ea9-137">タイマーを開始する</span><span class="sxs-lookup"><span data-stu-id="e9ea9-137">Start the timer</span></span>

<span data-ttu-id="e9ea9-138">これで、タイマーは構成されましたので、いつでも開始することができます。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-138">Now that we've configured the timer, we're ready to start it.</span></span>

1. <span data-ttu-id="e9ea9-139">**[TimerTriggerCSharp1]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-139">Select **TimerTriggerCSharp1**.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e9ea9-140">**[TimerTriggerCSharp1]** は既定の名前です。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-140">**TimerTriggerCSharp1** is a default name.</span></span> <span data-ttu-id="e9ea9-141">これは、トリガーを作成するとき自動的に選択されます。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-141">It's automatically selected when you create the trigger.</span></span>

2. <span data-ttu-id="e9ea9-142">**[実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-142">Select **Run**.</span></span> 

<span data-ttu-id="e9ea9-143">この時点では、ログ ウィンドウに 20 秒ごとにメッセージが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-143">At this point, you should see a message every 20 seconds in the log window.</span></span>

## <a name="clean-up"></a><span data-ttu-id="e9ea9-144">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="e9ea9-144">Clean up</span></span>

<span data-ttu-id="e9ea9-145">この関数に対して課金されないようにするには、ログ ウィンドウの上にある **[一時停止]** を選択してタイマーを停止します。</span><span class="sxs-lookup"><span data-stu-id="e9ea9-145">To ensure that you aren't charged for this function, above the log window, select **Pause** to stop the timer.</span></span>

![一時停止](../media-drafts/4-pause-timer.png)

