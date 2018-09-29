<span data-ttu-id="fdea5-101">Azure IoT Central アプリケーションを使用し、コーヒー マシンを Azure IoT Central に接続しました。</span><span class="sxs-lookup"><span data-stu-id="fdea5-101">You’ve now worked with the Azure IoT Central application and connected the coffee machine to Azure IoT Central.</span></span> <span data-ttu-id="fdea5-102">リモート コーヒー マシンの監視と管理を開始するまでの手順を順調に進んでいます。</span><span class="sxs-lookup"><span data-stu-id="fdea5-102">You are well on your way to begin to monitor and manage your remote coffee machine.</span></span> <span data-ttu-id="fdea5-103">このユニットでは、少し時間を取って、先ほど定義した接続されているコーヒー メーカー テンプレートを使用して、セットアップと接続を検証します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-103">In this unit, you take a moment to validate your setup and connection by using the Connected Coffee Maker template that you defined earlier.</span></span> <span data-ttu-id="fdea5-104">設定の最適温度を更新し、コマンドを実行してマシンの状態を確認し、ダッシュボードで接続されているコーヒー マシンを表示します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-104">You update the optimal temperature in settings, run commands to check for the state of your machine, and view your connected coffee machine in the dashboard.</span></span> 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a><span data-ttu-id="fdea5-105">設定を更新してアプリケーションとコーヒー マシンを同期させる</span><span class="sxs-lookup"><span data-stu-id="fdea5-105">Update settings to sync your application with the coffee machine</span></span>

<span data-ttu-id="fdea5-106">**[設定]** ページで、アプリケーションからコーヒー マシンに構成データを送信します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-106">On the **Settings** page, you send configuration data to the coffee machine from your application.</span></span> 

<span data-ttu-id="fdea5-107">このシナリオでは、最適温度を変更して、**[更新]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-107">In this scenario, change the optimal temperature and choose **Update**.</span></span> <span data-ttu-id="fdea5-108">設定が変更されると、コーヒー マシンで設定変更に対する応答が確認されるまで、設定は UI で保留中としてマークされます。</span><span class="sxs-lookup"><span data-stu-id="fdea5-108">When the setting is changed, the setting is marked as pending in the UI until the coffee machine acknowledges that it has responded to the setting change.</span></span> 

> [!NOTE]
> <span data-ttu-id="fdea5-109">設定が正常に更新されると、データ フローが示され、接続が検証されます。</span><span class="sxs-lookup"><span data-stu-id="fdea5-109">Successful updates in the setting indicate data flow and validate your  connection.</span></span> <span data-ttu-id="fdea5-110">テレメトリの測定値は、最適温度の更新に応答します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-110">The telemetry measurements will respond to the update in optimal temperature.</span></span> <span data-ttu-id="fdea5-111">**[測定]** ページで変更を確認できます。</span><span class="sxs-lookup"><span data-stu-id="fdea5-111">You can observe the change on the **Measurements** page.</span></span> 

## <a name="run-commands-on-the-coffee-machine"></a><span data-ttu-id="fdea5-112">コーヒー マシンに対してコマンドを実行する</span><span class="sxs-lookup"><span data-stu-id="fdea5-112">Run commands on the coffee machine</span></span> 
<span data-ttu-id="fdea5-113">次の演習では、**[コマンド]** ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-113">Navigate to the **Commands** page for the following exercise.</span></span> <span data-ttu-id="fdea5-114">コマンドのセットアップを検証するには、IoT Central からコーヒー マシンに対してリモートでコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-114">To validate the commands setup, you remotely run commands on the coffee machine from IoT Central.</span></span> <span data-ttu-id="fdea5-115">コマンドが正常に実行された場合は、コーヒー マシンから確認メッセージが送信されます。</span><span class="sxs-lookup"><span data-stu-id="fdea5-115">If the commands are successful, confirmation messages are sent from the coffee machine.</span></span>

1. <span data-ttu-id="fdea5-116">**[実行]** を選択し、リモートで抽出を開始します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-116">Start brewing remotely by choosing **Run**.</span></span> 
    
    <span data-ttu-id="fdea5-117">次の 3 つの条件が満たされている場合、コーヒー マシンが起動します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-117">The coffee machine will start if these three conditions are satisfied:</span></span>
    - <span data-ttu-id="fdea5-118">カップが検出された</span><span class="sxs-lookup"><span data-stu-id="fdea5-118">Cup detected</span></span>
    - <span data-ttu-id="fdea5-119">メンテナンス中ではない</span><span class="sxs-lookup"><span data-stu-id="fdea5-119">Not in maintenance</span></span>
    - <span data-ttu-id="fdea5-120">まだ抽出していない</span><span class="sxs-lookup"><span data-stu-id="fdea5-120">Not brewing already</span></span>  

    > [!NOTE]
    > <span data-ttu-id="fdea5-121">抽出が正常に開始されると、**[測定]** > **[状態]** で示されるマシンの状態が黄色に変わります。</span><span class="sxs-lookup"><span data-stu-id="fdea5-121">When you've successfully started brewing, the state of the machine changes to yellow as indicated in **Measurements** > **State**.</span></span> 
    
    <span data-ttu-id="fdea5-122">node.js でシミュレートされたコーヒー マシンのコンソール ログで確認メッセージを探します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-122">Look for confirmation messages in the console log on the node.js simulated coffee machine.</span></span> 

    ![コマンドを実行する](../media/4-commands-brewing.png)

1. <span data-ttu-id="fdea5-124">**[実行]** を選択して、メンテナンス モードを設定します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-124">Set maintenance mode by choosing **Run**.</span></span> <span data-ttu-id="fdea5-125">まだメンテナンスになって*いない*場合は、コーヒー マシンがメンテナンスに設定されます。</span><span class="sxs-lookup"><span data-stu-id="fdea5-125">The coffee machine will set to maintenance if it's *not* already in maintenance.</span></span>
    
    <span data-ttu-id="fdea5-126">コーヒー マシンのコンソール ログで確認メッセージを探します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-126">Look for confirmation messages in the console log on the coffee machine.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="fdea5-127">実際に技術者がマシンをオフラインにして必要な修理を行ってからオンラインに戻すのと同じように、クライアント コードを再起動するまで、コーヒー マシンはメンテナンス モードのままになっています。</span><span class="sxs-lookup"><span data-stu-id="fdea5-127">As in real life, when the technician takes the machine offline to perform necessary repairs before switching it back online, the coffee machine continues to stay in the maintenance mode until you reboot the client code.</span></span>

    ![コマンドを実行する](../media/4-commands-maintenance.png)

> [!IMPORTANT]
> <span data-ttu-id="fdea5-129">Node.js アプリケーションを 60 分を超えない程度で実行し、アプリケーションから不要な通知や電子メールが送信されないようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="fdea5-129">It's recommended that you run the Node.js application no more than 60 minutes or so to prevent the application from sending you unwanted notifications/emails.</span></span> <span data-ttu-id="fdea5-130">モジュールで操作していない場合はアプリケーションを停止することで、1 日のメッセージ クォータを使い切ることもなくなります。</span><span class="sxs-lookup"><span data-stu-id="fdea5-130">Stopping the application when you're not working on the module also prevents you from exhausting the daily message quota.</span></span>

## <a name="view-the-coffee-machine-in-the-dashboard"></a><span data-ttu-id="fdea5-131">ダッシュボードでコーヒー マシンを表示する</span><span class="sxs-lookup"><span data-stu-id="fdea5-131">View the coffee machine in the dashboard</span></span>

1. <span data-ttu-id="fdea5-132">**[ダッシュボード]** タブに移動します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-132">Navigate to the **Dashboard** tab.</span></span>

1. <span data-ttu-id="fdea5-133">**[テンプレートの編集]** を選択してダッシュボードを編集します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-133">Select **Edit Template** to edit the dashboard.</span></span>

1. <span data-ttu-id="fdea5-134">サイド メニューから **[折れ線グラフ]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-134">Choose **Line Chart** from the side menu.</span></span>

1. <span data-ttu-id="fdea5-135">**[グラフの構成]** の **[タイトル]** フィールドに「`Telemetry`」と入力します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-135">In **Configure Chart**  enter `Telemetry` in the **Title** field.</span></span> <span data-ttu-id="fdea5-136">このグラフで利用統計情報を確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="fdea5-136">We'll view our telemetry data with this chart.</span></span> 

1. <span data-ttu-id="fdea5-137">**[時間範囲]** で **[過去 30 分]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-137">Select **Past 30 minutes** for **Time Range**.</span></span> 

1. <span data-ttu-id="fdea5-138">**[測定]** 領域で、各測定値の右側にある表示アイコンを選択して、測定値がグラフに表示されるようにします。</span><span class="sxs-lookup"><span data-stu-id="fdea5-138">In the **Measures** area, select the visibility icon to the right of each measurement to make that measurement visible for our chart.</span></span> 

1. <span data-ttu-id="fdea5-139">**[保存]** を選択して構成を保存し、折れ線グラフを表示します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-139">Select **Save** to save our configuration and display the line chart.</span></span> 

    ![ダッシュボードの表示](../media/4-dashboard-a.png)

1. <span data-ttu-id="fdea5-141">左側のメニューにある **[Settings and Properties]\(設定とプロパティ\)** を選択し、**[Configure Device Details]\(デバイスの詳細の構成\)** パネルを開きます。</span><span class="sxs-lookup"><span data-stu-id="fdea5-141">Choose **Settings and Properties** in the left-hand menu to open the **Configure Device Details** panel.</span></span> 

1. <span data-ttu-id="fdea5-142">**[タイトル]** フィールドに「`Device properties`」と入力します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-142">Enter `Device properties` in the **Title** filed.</span></span>

1. <span data-ttu-id="fdea5-143">**[追加/削除]** で、**[Coffee Makers Max Temperature]\(コーヒー メーカー最高温度\)**、**[Coffee Makers Min Temperature]\(コーヒー メーカー最低温度\)**、**[Device Warranty Expired]\(デバイスの保証期限切れ\)** を選択し、**[OK]** を選択して選択内容を確定します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-143">In **Add/Remove**, choose **Coffee Makers Max Temperature**, **Coffee Makers Min Temperature**, **Device Warranty Expired** and then select **OK** to complete the selection.</span></span>

1. <span data-ttu-id="fdea5-144">**[保存]** を選択して、ダッシュボードに新しい *[デバイスのプロパティ]* パネルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-144">Select **Save** to create a new *Device properties* panel in our dashboard.</span></span> 

1. <span data-ttu-id="fdea5-145">**[Settings and Properties]\(設定とプロパティ\)** をもう一度選択し、タイトルに「`Optimal Temperature`」と入力します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-145">Choose **Settings and Properties** again,  and enter `Optimal Temperature` as the title.</span></span> <span data-ttu-id="fdea5-146">**[追加/削除]** で、**[Optimal Temperature]\(最適温度\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-146">In **Add/Remove**, choose **Optimal  Temperature**.</span></span>

1. <span data-ttu-id="fdea5-147">**[保存]** を選択して作業内容を保存し、ダッシュボードに新しいウィンドウを表示します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-147">Select **Save** to save your work and display a new pane in our dashboard.</span></span> 

1. <span data-ttu-id="fdea5-148">**[完了]** を選択して編集モードを終了し、新しいダッシュボードを表示します。</span><span class="sxs-lookup"><span data-stu-id="fdea5-148">Select **Done** to exit edit mode and display the new dashboard.</span></span> 