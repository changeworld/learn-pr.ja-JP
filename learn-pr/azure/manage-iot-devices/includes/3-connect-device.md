<span data-ttu-id="f2e51-101">気象は、トラフィック パターンから小売り店内の暖房、換気、空調 (HVAC) システムの運転状況に至るまで、すべてのことに影響するため、気象データの入手は重要な作業です。</span><span class="sxs-lookup"><span data-stu-id="f2e51-101">Capturing of weather data is an important task as weather can effect everything from traffic patterns to how heating, ventilation, and air conditioning (HVAC) systems in retail stores are operated.</span></span> <span data-ttu-id="f2e51-102">この演習では、前のユニットで学習したオンライン Raspberry Pi シミュレーターと対話して、Azure IoT Hub を通じてシミュレートされた気象データをキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="f2e51-102">In this exercise, you will be interacting with the online Raspberry Pi simulator you learned about in the previous unit to capture simulated weather data and via the Azure IoT Hub.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="f2e51-103">この演習はシミュレートされた環境で実行されますが、シミュレートされたデバイス上で実行されているアプリケーションは、将来、実際のデバイスに転送することができます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-103">While this exercise is being conducted in a simulated environment, the application running on the simulated device can be transferred to a real device in the future.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="f2e51-104">IoT Hub を作成する</span><span class="sxs-lookup"><span data-stu-id="f2e51-104">Create an IoT hub</span></span>
<span data-ttu-id="f2e51-105">Azure IoT Hub には、デバイスやバックエンドの開発者が堅牢なデバイス管理ソリューションを構築するために使用できる機能や拡張モデルが用意されています。</span><span class="sxs-lookup"><span data-stu-id="f2e51-105">Azure IoT Hub provides the features and an extensibility model that enable device and back-end developers to build robust device management solutions.</span></span> <span data-ttu-id="f2e51-106">デバイスは、リソースの制約の大きいセンサーをはじめ、専用マイクロコントローラー、デバイス グループの通信をルーティングする強力なゲートウェイなど、多岐にわたります。</span><span class="sxs-lookup"><span data-stu-id="f2e51-106">Devices range from constrained sensors and single purpose microcontrollers, to powerful gateways that route communications for groups of devices.</span></span> <span data-ttu-id="f2e51-107">また、用途や IoT オペレーターの要件は業界によってかなり異なります。</span><span class="sxs-lookup"><span data-stu-id="f2e51-107">In addition, the use cases and requirements for IoT operators vary significantly across industries.</span></span> <span data-ttu-id="f2e51-108">このような違いがありながら、IoT Hub によるデバイス管理で提供される機能、パターン、およびコード ライブラリは、多様なデバイスとエンド ユーザーに対応することができます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-108">Despite this variation, device management with IoT Hub provides the capabilities, patterns, and code libraries to cater to a diverse set of devices and end users.</span></span>

<span data-ttu-id="f2e51-109">Raspberry Pi シミュレーターからのデータの収集を開始するには、まず IoT ハブを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2e51-109">In order to start collecting the data from the Raspberry Pi simulator, you need to first create an IoT hub.</span></span>

1. <span data-ttu-id="f2e51-110">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="f2e51-110">Sign into the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

2. <span data-ttu-id="f2e51-111">Azure portal の左上隅にある **[リソースの作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-111">Choose **Create a resource** in the upper left-hand corner of the Azure portal.</span></span>

3. <span data-ttu-id="f2e51-112">**[モノのインターネット (IoT)]**、**[Event Hubs]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-112">Select **Internet of Things**, and then select **IoT Hub**.</span></span>

![Azure portal での IoT Hub へのナビゲーションのスクリーンショット](../media/fa40d1bc51bc4490f657e3c1a8371b5b.png)

4. <span data-ttu-id="f2e51-114">**[IoT Hub]** ウィンドウで、IoT Hub のために以下の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-114">In the **IoT hub** pane, enter the following information for your IoT hub:</span></span>

   - <span data-ttu-id="f2e51-115">**[サブスクリプション]**: この例に対する既定のサブスクリプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-115">**Subscription**: Use the default subscription for this example.</span></span>
   - <span data-ttu-id="f2e51-116">**[リソース グループ]**: 既存のリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-116">**Resource group**: Use the existing resource group.</span></span> <span data-ttu-id="f2e51-117">関連するすべてのリソースを同じグループ内に配置することで、すべてを一緒に管理できます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-117">By putting all related resources in the same group you can manage them all together.</span></span> <span data-ttu-id="f2e51-118">たとえば、リソース グループを削除すると、そのグループに含まれているすべてのリソースが削除されます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-118">For example, deleting the resource group deletes all resources contained in that group.</span></span>
   - <span data-ttu-id="f2e51-119">**[名前]**: IoT ハブの一意の名前を作成します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-119">**Name**: Create a unique name for your IoT hub.</span></span> <span data-ttu-id="f2e51-120">入力した名前が使用可能な場合は、緑色のチェック マークが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-120">If the name you enter is available, a green check mark appears.</span></span>
   - <span data-ttu-id="f2e51-121">**[リージョン]**: 次の一覧から最も近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-121">**Region**: Select the closest location to you from the following list.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

    > [!IMPORTANT]
    > <span data-ttu-id="f2e51-122">IoT ハブは DNS エンドポイントとして公開されます。そのため、名前を付けるときは機密情報を含めないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="f2e51-122">The IoT hub will be publicly discoverable as a DNS endpoint, so make sure to avoid any sensitive information while naming it.</span></span>

    ![IoT Hub の基本ウィンドウ](./../media/dbb7319388673b8ee0e0b407536156c0.png)

1. <span data-ttu-id="f2e51-124">**[Next: Size and scale]\(次へ: サイズとスケール\)** を選択して、IoT ハブの作成を続けます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-124">Select **Next: Size and scale** to continue creating your IoT hub.</span></span>
2. <span data-ttu-id="f2e51-125">**[価格とスケールティア]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-125">Choose your **Pricing and scale tier**.</span></span> <span data-ttu-id="f2e51-126">この例では、**[F1 - Free]** レベルを選択します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-126">For this example, select the **F1 - Free** tier.</span></span>

    ![IoT ハブのサイズとスケールのウィンドウ](../media/b506eb3293fa4aa9d4785ad498fc476c.png)

3. <span data-ttu-id="f2e51-128">**[Review + create]\(レビュー + 作成\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-128">Select **Review + create**.</span></span>

4. <span data-ttu-id="f2e51-129">IoT Hub の情報を確認してから、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2e51-129">Review your IoT hub information, then click **Create**.</span></span> <span data-ttu-id="f2e51-130">IoT Hub の作成には数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="f2e51-130">Your IoT hub might take a few minutes to create.</span></span> <span data-ttu-id="f2e51-131">**[通知]** ウィンドウで進行状況を監視できます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-131">You can monitor the progress in the **Notifications** pane.</span></span>

<!--STOPPED HERE-->
<!--
Now that you have created an IoT hub, it's time to locate the important information that you use to connect devices and applications to your IoT hub. In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up exercise. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media/a4b41e6ea46ccbef653c411a9829610c.png)
-->

## <a name="register-a-device"></a><span data-ttu-id="f2e51-132">デバイスの登録</span><span class="sxs-lookup"><span data-stu-id="f2e51-132">Register a device</span></span>
<span data-ttu-id="f2e51-133">デバイスを IoT ハブに接続するには、あらかじめ IoT ハブに登録しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="f2e51-133">A device must be registered with your IoT hub before it can connect.</span></span>

1. <span data-ttu-id="f2e51-134">IoT ハブ ナビゲーション メニューの **[IoT デバイス]** を開き、**[追加]** をクリックして IoT ハブにデバイスを登録します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-134">In your IoT hub navigation menu, open **IoT devices**, then click **Add** to register a device in your IoT hub.</span></span>

   ![IoT Hub の [IoT デバイス] でデバイスを追加する](../media/ee5f177abcf06b86dd007fce3b8448ad.png)

2. <span data-ttu-id="f2e51-136">新しいデバイスの **[デバイス ID]** を入力します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-136">Enter a **Device ID** for the new device.</span></span> <span data-ttu-id="f2e51-137">デバイス ID には大文字と小文字の区別があります。</span><span class="sxs-lookup"><span data-stu-id="f2e51-137">Device IDs are case sensitive.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f2e51-138">デバイス ID は、カスタマー サポートとトラブルシューティング目的で収集されたログに表示される場合があります。そのため、名前を付ける際は機密情報を含めないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="f2e51-138">The device ID may be visible in the logs collected for customer support and troubleshooting, so make sure to avoid any sensitive information while naming it.</span></span>

3. <span data-ttu-id="f2e51-139">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f2e51-139">Click **Save**.</span></span>
4. <span data-ttu-id="f2e51-140">デバイスが作成された後、**[IoT デバイス]** ウィンドウの一覧からデバイスを開きます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-140">After the device is created, open the device from the list in the **IoT devices** pane.</span></span>
5. <span data-ttu-id="f2e51-141">後で使用するために **[接続文字列 --- 主キー]** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="f2e51-141">Copy the **Connection string---primary key** to use later.</span></span>

   ![デバイスの接続文字列を取得する](../media/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="send-simulated-telemetry"></a><span data-ttu-id="f2e51-143">シミュレートされた利用統計情報の送信</span><span class="sxs-lookup"><span data-stu-id="f2e51-143">Send simulated telemetry</span></span>

1. <span data-ttu-id="f2e51-144">[Raspberry Pi Azure IoT Simulator](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true) を開きます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-144">Open the [Raspberry Pi Azure IoT Simulator](https://azure-samples.github.io/raspberry-pi-web-simulator?azure-portal=true).</span></span>
1. <span data-ttu-id="f2e51-145">15 行目のプレースホルダーを Azure IoT ハブ デバイスのコピーした接続文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-145">Replace the placeholder in Line 15 with the Azure IoT hub device connection string you just copied.</span></span>
1. <span data-ttu-id="f2e51-146">[`Run`] ボタンをクリックするか、コンソール ウィンドウで「`npm start`」と入力して、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-146">Click the `Run` button or type `npm start` in the console window to run the application.</span></span>

    ![デバイスの接続文字列を置き換える](../media/Line15.png)

1. <span data-ttu-id="f2e51-148">IoT ハブに送信されるセンサー データとメッセージを示す次の出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-148">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub.</span></span>

    ![出力 - Raspberry Pi から IoT ハブに送信されるセンサー データ](../media/96b28d30e317b04347abb0d613738117.png)

## <a name="read-the-telemetry-from-your-hub"></a><span data-ttu-id="f2e51-150">ハブから利用統計情報を読み取る</span><span class="sxs-lookup"><span data-stu-id="f2e51-150">Read the telemetry from your hub</span></span>
<span data-ttu-id="f2e51-151">どうなるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="f2e51-151">So what's happening?</span></span> <span data-ttu-id="f2e51-152">IoT ハブは、シミュレートされたデバイスから送信されたデバイスとクラウドの間のメッセージを受信します。</span><span class="sxs-lookup"><span data-stu-id="f2e51-152">IoT hub is receiving the device-to-cloud messages sent from the simulated device.</span></span> <span data-ttu-id="f2e51-153">それを確認するために、Azure IoT Hub が受信データを処理するようすを見てみます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-153">In order to see that, let's take a quick look at how Azure IoT Hub is processing the incoming data.</span></span> <span data-ttu-id="f2e51-154">対象の IoT ハブで、**[監視]** の **[メトリック]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-154">In your IoT Hub, under **Monitoring**, select **Metrics**.</span></span> <span data-ttu-id="f2e51-155">データが表示されるまで数分待ちます。</span><span class="sxs-lookup"><span data-stu-id="f2e51-155">Give it a few minutes as you wait for the data to come into the picture.</span></span>

![IoT ハブのメトリック](../media/HubMetrics.png)


<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
