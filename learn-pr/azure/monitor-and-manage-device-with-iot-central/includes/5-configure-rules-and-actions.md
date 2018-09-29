<span data-ttu-id="88486-101">ここまでで、コーヒー マシンを Azure IoT Central アプリケーションに接続し、コーヒー マシンを監視して管理できるようにするデータの交換を有効にしました。</span><span class="sxs-lookup"><span data-stu-id="88486-101">So far, you've connected your coffee machine to the Azure IoT Central application, enabling the exchange of data that allows you to monitor and manage your coffee machine.</span></span> <span data-ttu-id="88486-102">このユニットでは、コーヒー マシンの水温が正常な範囲を超えたときにアクションをトリガーするルールを作成します。</span><span class="sxs-lookup"><span data-stu-id="88486-102">In this unit, you create rules that trigger actions when the water temperature of the coffee machine is outside the normal range.</span></span> 

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a><span data-ttu-id="88486-103">電子メールをアクションとして IoT Central でルールを作成する</span><span class="sxs-lookup"><span data-stu-id="88486-103">Create rules in IoT Central with email as the action</span></span>

<span data-ttu-id="88486-104">Azure IoT Central には、通知を送信するネイティブなメール機能があります。</span><span class="sxs-lookup"><span data-stu-id="88486-104">Azure IoT Central has its native email capabilities to send notifications.</span></span> <span data-ttu-id="88486-105">このシナリオでは、コーヒー マシンが最適温度の範囲外にあり、保証で保護されていない場合、IoT Central によってクライアントのメンテナンス部門にメールが送信されます。</span><span class="sxs-lookup"><span data-stu-id="88486-105">In this scenario, if the coffee machine is outside the optimal temperature range and is not protected by the warranty, an email is sent by IoT Central to the client’s maintenance department.</span></span>

1. <span data-ttu-id="88486-106">このユニットの演習では **[ルール]** ページに移動し、右側の **[テンプレートの編集]** を選択して編集モードにします。</span><span class="sxs-lookup"><span data-stu-id="88486-106">Navigate to the **Rules** page for the exercises in this unit and enter edit mode by selecting **Edit Template** on the right.</span></span> 
1. <span data-ttu-id="88486-107">**[+ 新しいルール]**、**[テレメトリ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="88486-107">Select **+ New Rule** and then **Telemetry**.</span></span> 

1. <span data-ttu-id="88486-108">「`Coffee Maker Water Too Cold (Expired)`」という名前を入力します。</span><span class="sxs-lookup"><span data-stu-id="88486-108">Enter the name `Coffee Maker Water Too Cold (Expired)`</span></span>

1. <span data-ttu-id="88486-109">**[条件]** の右側にある正符号 (**+**) を選択してルールに次の条件を追加し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="88486-109">Add the following conditions to the rule by selecting the plus (**+**) sign to the right of **Conditions**, and then click **Save**:</span></span>      
    - <span data-ttu-id="88486-110">水温がコーヒー メーカーの最低温度未満である</span><span class="sxs-lookup"><span data-stu-id="88486-110">Water Temperature is less than Coffee Maker's Min Temperature</span></span>
    - <span data-ttu-id="88486-111">デバイスの保証期限切れは 1 に等しい</span><span class="sxs-lookup"><span data-stu-id="88486-111">Device Warranty Expired equals 1</span></span>

    ![ルールの使用](../media/5-flow-a.png)

1. <span data-ttu-id="88486-113">**[Configure Telemetry Rule]\(テレメトリ ルールの構成\)** パネルで下にスクロールし、**[アクション]** の横にある **[+]** を選択してから **[メール]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="88486-113">Scroll down on the **Configure Telemetry Rule** panel and choose **+** next to **Actions**, and then choose **Email**.</span></span>

1. <span data-ttu-id="88486-114">IoT Central アプリケーションへのサインインに使用したメール アドレスを入力し、「`Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.`」という注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="88486-114">Enter the email address that you used to sign in to the IoT Central application and add the note `Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.`</span></span>

1. <span data-ttu-id="88486-115">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="88486-115">Choose **Save**.</span></span> <span data-ttu-id="88486-116">使用するルールが **[ルール]** ページにリストされます。</span><span class="sxs-lookup"><span data-stu-id="88486-116">Your rule is listed on the **Rules** page.</span></span>

<span data-ttu-id="88486-117">水が熱すぎる場合は、以下の手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="88486-117">Now let's repeat these steps for the case when the water is too hot.</span></span> 

1. <span data-ttu-id="88486-118">**[+ 新しいルール]**、**[テレメトリ]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="88486-118">Select **+ New Rule** and then **Telemetry**.</span></span>

1. <span data-ttu-id="88486-119">新しいルールを追加して、`Coffee Maker Water Too Hot (Expired)` という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="88486-119">Add a new rule and give it the name `Coffee Maker Water Too Hot (Expired)`</span></span>

1. <span data-ttu-id="88486-120">**[条件]** の右側にある正符号 (**+**) を選択してルールに次の条件を追加し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="88486-120">Add the following conditions to the rule by selecting the plus (**+**) sign to the right of **Conditions**, and then click **Save**:</span></span>      
    - <span data-ttu-id="88486-121">水温がコーヒー メーカーの最高温度を超えている</span><span class="sxs-lookup"><span data-stu-id="88486-121">Water Temperature is greater than Coffee Makers Max Temperature</span></span>
    - <span data-ttu-id="88486-122">デバイスの保証期限切れは 1 に等しい</span><span class="sxs-lookup"><span data-stu-id="88486-122">Device Warranty Expired equals 1</span></span>

1. <span data-ttu-id="88486-123">**[Configure Telemetry Rule]\(テレメトリ ルールの構成\)** パネルで下にスクロールし、**[アクション]** の横にある **[+]** を選択してから **[メール]** を選びます。</span><span class="sxs-lookup"><span data-stu-id="88486-123">Scroll down on the **Configure Telemetry Rule** panel and choose **+** next to **Actions**, and then choose **Email**.</span></span>

1. <span data-ttu-id="88486-124">IoT Central アプリケーションへのサインインに使用したメール アドレスを入力し、「`Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.`」という注釈を追加します。</span><span class="sxs-lookup"><span data-stu-id="88486-124">Enter the email address that you used to sign in to the IoT Central application and add the note `Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.`</span></span>

1. <span data-ttu-id="88486-125">**[保存]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="88486-125">Choose **Save**.</span></span> <span data-ttu-id="88486-126">使用するルールが **[ルール]** ページに一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="88486-126">Your rule is listed on the **Rules** page.</span></span>

<span data-ttu-id="88486-127">ルールをトリガーするには、**[設定]** の最適温度を、**[プロパティ]** で指定した範囲から外れた値に設定します。</span><span class="sxs-lookup"><span data-stu-id="88486-127">To trigger the rule, set the optimal temperature in **Settings** outside the range that you specified under **Properties**.</span></span> <span data-ttu-id="88486-128">検証が終わったら、受信トレイがメールであふれないようにルールを無効にします。</span><span class="sxs-lookup"><span data-stu-id="88486-128">Once you are done with the validation, turn off the rules to avoid flooding your inbox with emails.</span></span>