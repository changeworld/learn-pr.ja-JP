<span data-ttu-id="f7149-101">気象は、トラフィック パターンから小売り店内の HVAC システムの運転状況に至るまで、すべてのことに影響するため、気象データの入手は重要な作業です。</span><span class="sxs-lookup"><span data-stu-id="f7149-101">Capturing of weather data is an important task as weather can effect everything from traffic patterns to how HVAC systems in retail stores are operated.</span></span> <span data-ttu-id="f7149-102">この演習では、オンライン Raspberry Pi シミュレーターを利用してシミュレートされた気象データをキャプチャし、Azure IoT Hub を通じて上記のデータをキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="f7149-102">In this exercise, you will be harnessing the online Raspberry Pi simulator to capture simulated weather data and capture said data via the Azure IoT Hub.</span></span>

<span data-ttu-id="f7149-103">この演習はシミュレートされた環境で実行されますが、シミュレートされたデバイス上で実行されているアプリケーションは、将来、実際のデバイスに転送することができます。</span><span class="sxs-lookup"><span data-stu-id="f7149-103">While this exercise is being conducted in a simulated environment, the application running on the simulated device can be transferred to a real device in future.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="f7149-104">IoT Hub を作成する</span><span class="sxs-lookup"><span data-stu-id="f7149-104">Create an IoT hub</span></span>

1. <span data-ttu-id="f7149-105">[Azure portal](https://portal.azure.com/) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="f7149-105">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>

1. <span data-ttu-id="f7149-106">**[リソースの作成]** \>**[モノのインターネット]** \>**[IoT Hub]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="f7149-106">Select **Create a resource** \> **Internet of Things** \> **IoT Hub**.</span></span>

![Azure portal での IoT Hub へのナビゲーションのスクリーンショット](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

1. <span data-ttu-id="f7149-108">**[IoT Hub]** ウィンドウで、IoT Hub のために以下の情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="f7149-108">In the **IoT hub** pane, enter the following information for your IoT hub:</span></span>

 - <span data-ttu-id="f7149-109">**[サブスクリプション]**: この IoT Hub を作成するために使用するサブスクリプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="f7149-109">**Subscription**: Choose the subscription that you want to use to create this IoT hub.</span></span>
 - <span data-ttu-id="f7149-110">**[リソース グループ]**: IoT Hub をホストするリソース グループを作成するか、既存のリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="f7149-110">**Resource group**: Create a resource group to host the IoT hub or use an existing one.</span></span> <span data-ttu-id="f7149-111">詳細については、[リソース グループを使用した Azure リソースの管理](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f7149-111">For more information, see [Use resource groups to manage your Azure resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).</span></span>
 - <span data-ttu-id="f7149-112">**[リージョン]**: 最も近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="f7149-112">**Region**: Select the closest location to you.</span></span>
 - <span data-ttu-id="f7149-113">**[名前]**: IoT Hub の名前を作成します。</span><span class="sxs-lookup"><span data-stu-id="f7149-113">**Name**: Create a name for your IoT hub.</span></span> <span data-ttu-id="f7149-114">入力した名前が使用可能な場合は、緑色のチェック マークが表示されます。</span><span class="sxs-lookup"><span data-stu-id="f7149-114">If the name you enter is available, a green check mark appears.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f7149-115">IoT Hub は DNS エンドポイントとして公開されます。そのため、名前を付ける際には機密情報を含めないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="f7149-115">The IoT hub will be publicly discoverable as a DNS endpoint, so make sure to avoid any sensitive information while naming it.</span></span>

   ![IoT Hub の基本ウィンドウ](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

1.  <span data-ttu-id="f7149-117">**[Next: Size and scale]\(次へ: サイズとスケール\)** を選択して、IoT ハブの作成を続けます。</span><span class="sxs-lookup"><span data-stu-id="f7149-117">Select **Next: Size and scale** to continue creating your IoT hub.</span></span>

1.  <span data-ttu-id="f7149-118">**[価格とスケールティア]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f7149-118">Choose your **Pricing and scale tier**.</span></span> <span data-ttu-id="f7149-119">この記事では、**[F1 - Free]** レベルを選択します (サブスクリプションで引き続き使用可能な場合)。</span><span class="sxs-lookup"><span data-stu-id="f7149-119">For this article, select the **F1 - Free** tier if it's still available on your subscription.</span></span> <span data-ttu-id="f7149-120">詳細については、[料金とスケール レベル](https://azure.microsoft.com/pricing/details/iot-hub/)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f7149-120">For more information, see the [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

   ![IoT Hub のサイズとスケールのウィンドウ](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

1.  <span data-ttu-id="f7149-122">**[Review + create]\(レビュー + 作成\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="f7149-122">Select **Review + create**.</span></span>

1.  <span data-ttu-id="f7149-123">IoT Hub の情報を確認してから、**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7149-123">Review your IoT hub information, then click **Create**.</span></span> <span data-ttu-id="f7149-124">IoT Hub の作成には数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="f7149-124">Your IoT hub might take a few minutes to create.</span></span> <span data-ttu-id="f7149-125">**[通知]** ウィンドウで進行状況を監視できます。</span><span class="sxs-lookup"><span data-stu-id="f7149-125">You can monitor the progress in the **Notifications** pane.</span></span>

<span data-ttu-id="f7149-126">IoT Hub を作成できたので、その IoT Hub にデバイスとアプリケーションを接続するために必要な重要な情報を確認します。</span><span class="sxs-lookup"><span data-stu-id="f7149-126">Now that you have created an IoT hub, locate the important information that you use to connect devices and applications to your IoT hub.</span></span>

<span data-ttu-id="f7149-127">IoT Hub ナビゲーション メニューで**共有アクセス ポリシー**を開きます。</span><span class="sxs-lookup"><span data-stu-id="f7149-127">In your IoT hub navigation menu, open **Shared access policies**.</span></span> <span data-ttu-id="f7149-128">**[iothubowner]** ポリシーを選択し、IoT Hub の **[接続文字列 --- 主キー]** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="f7149-128">Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub.</span></span> <span data-ttu-id="f7149-129">詳細については、「[IoT Hub へのアクセスの制御](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f7149-129">For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).</span></span>

> [!NOTE]
> <span data-ttu-id="f7149-130">このセットアップ チュートリアルでは、この iothubowner 接続文字列は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="f7149-130">You do not need this iothubowner connection string for this set-up tutorial.</span></span> <span data-ttu-id="f7149-131">ただし、このセットアップの完了後、一部のチュートリアルや異なる IoT シナリオでは必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f7149-131">However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.</span></span>

![IoT Hub の接続文字列を取得する](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a><span data-ttu-id="f7149-133">デバイスの IoT Hub にデバイスを登録する</span><span class="sxs-lookup"><span data-stu-id="f7149-133">Register a device in the IoT hub for your device</span></span>
------------------------------------------------

1.  <span data-ttu-id="f7149-134">IoT Hub ナビゲーション メニューの **[IoT デバイス]** を開き、**[追加]** をクリックして IoT Hub にデバイスを登録します。</span><span class="sxs-lookup"><span data-stu-id="f7149-134">In your IoT hub navigation menu, open **IoT devices**, then click **Add** to register a device in your IoT hub.</span></span>

   ![IoT Hub の [IoT デバイス] でデバイスを追加する](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

1.  <span data-ttu-id="f7149-136">新しいデバイスの **[デバイス ID]** を入力します。</span><span class="sxs-lookup"><span data-stu-id="f7149-136">Enter a **Device ID** for the new device.</span></span> <span data-ttu-id="f7149-137">デバイス ID には大文字と小文字の区別があります。</span><span class="sxs-lookup"><span data-stu-id="f7149-137">Device IDs are case sensitive.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f7149-138">デバイス ID は、カスタマー サポートとトラブルシューティング目的で収集されたログに表示される場合があります。そのため、名前を付ける際は機密情報を含めないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="f7149-138">The device ID may be visible in the logs collected for customer support and troubleshooting, so make sure to avoid any sensitive information while naming it.</span></span>

1.  <span data-ttu-id="f7149-139">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="f7149-139">Click **Save**.</span></span>

1.  <span data-ttu-id="f7149-140">デバイスが作成された後、**[IoT デバイス]** ウィンドウの一覧からデバイスを開きます。</span><span class="sxs-lookup"><span data-stu-id="f7149-140">After the device is created, open the device from the list in the **IoT devices** pane.</span></span>

1.  <span data-ttu-id="f7149-141">後で使用するために **[接続文字列 --- 主キー]** をコピーします。</span><span class="sxs-lookup"><span data-stu-id="f7149-141">Copy the **Connection string---primary key** to use later.</span></span>

   ![デバイスの接続文字列を取得する](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## <a name="run-a-sample-application-on-pi-web-simulator"></a><span data-ttu-id="f7149-143">Pi Web シミュレーターでサンプル アプリケーションを実行する</span><span class="sxs-lookup"><span data-stu-id="f7149-143">Run a sample application on Pi web simulator</span></span>

1. <span data-ttu-id="f7149-144">コーディング領域で、既定のサンプル アプリケーションで作業していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f7149-144">In coding area, make sure you are working on the default sample application.</span></span> <span data-ttu-id="f7149-145">行 15 のプレースホルダーを Azure IoT Hub デバイスの接続文字列に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f7149-145">Replace the placeholder in Line 15 with the Azure IoT hub device connection string.</span></span>

    ![デバイスの接続文字列を置き換える](../media-draft/92ea2c31d42f5b939fb5512e7220e957.png)

2.  <span data-ttu-id="f7149-147">**[実行]** をクリックするか、「npm start」と入力して、アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="f7149-147">Click **Run** or type npm start to run the application.</span></span>

<span data-ttu-id="f7149-148">IoT Hub に送信されるセンサー データとメッセージを示す次の出力が表示されます</span><span class="sxs-lookup"><span data-stu-id="f7149-148">You should see the following output that shows the sensor data and the messages that are sent to your IoT hub</span></span>

   ![出力 - Raspberry Pi から IoT Hub に送信されるセンサー データ](../media-draft/96b28d30e317b04347abb0d613738117.png)

<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
