<span data-ttu-id="101bd-101">Azure IoT Central は、グローバルなモノのインターネット (IoT) 資産の接続、監視、管理を簡単に行えるフルマネージド IoT ソリューションです。</span><span class="sxs-lookup"><span data-stu-id="101bd-101">Azure IoT Central is a fully managed Internet of Things (IoT) solution that makes it easy to connect, monitor, and manage your global IoT assets.</span></span>

<span data-ttu-id="101bd-102">ここでは、リモートのコーヒー メーカーが問題の監視と管理のために Azure IoT Central に接続されているというシナリオに従って進めます。</span><span class="sxs-lookup"><span data-stu-id="101bd-102">Here, you'll follow the scenario in which a remote coffee machine is connected to Azure IoT Central for monitoring and management of issues.</span></span> <span data-ttu-id="101bd-103">水温や湿度などのテレメトリの監視、マシン状態の観察、最適な温度の設定、保証状態の受信、コマンドの送信を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="101bd-103">You can monitor telemetry such as water temperature and humidity, observe the state of your machine, set optimal temperature, receive warranty status, and send commands.</span></span> <span data-ttu-id="101bd-104">保証が期限切れになった後で水温が想定範囲を超えた場合は、IoT Central からクライアントのメンテナンス部門に処置を求める電子メールが送信されます。</span><span class="sxs-lookup"><span data-stu-id="101bd-104">If the warranty is expired when the water temperature is outside the expected range, an email from IoT Central is sent to the client’s maintenance department for further action.</span></span>

<span data-ttu-id="101bd-105">IoT デバイスとやり取りできるデータおよびコマンドを定義する Azure IoT Central 内のデバイスを作成することから始めます。</span><span class="sxs-lookup"><span data-stu-id="101bd-105">You'll begin by a creating a device in Azure IoT Central that defines the data and commands that can be exchanged with the IoT device.</span></span>

<span data-ttu-id="101bd-106">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="101bd-106">In this module, you will:</span></span>
  - <span data-ttu-id="101bd-107">Azure IoT Central カスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="101bd-107">Create an Azure IoT Central custom application</span></span>
  - <span data-ttu-id="101bd-108">デバイス テンプレートを作成して定義する</span><span class="sxs-lookup"><span data-stu-id="101bd-108">Create and define your device template</span></span>
  - <span data-ttu-id="101bd-109">コーヒー メーカー シミュレーターを Azure IoT Central 内のご使用のアプリケーションに接続する</span><span class="sxs-lookup"><span data-stu-id="101bd-109">Connect a coffee machine simulator to your application in Azure IoT Central</span></span>
  - <span data-ttu-id="101bd-110">接続とデータ フローを検証する</span><span class="sxs-lookup"><span data-stu-id="101bd-110">Validate your connection and data flow</span></span>
  - <span data-ttu-id="101bd-111">メンテナンス通知の規則を構成する</span><span class="sxs-lookup"><span data-stu-id="101bd-111">Configure rules for maintenance notifications</span></span>
 
## <a name="sign-in-to-azure-iot-central"></a><span data-ttu-id="101bd-112">Azure IoT Central にサインインする</span><span class="sxs-lookup"><span data-stu-id="101bd-112">Sign in to Azure IoT Central</span></span>
<span data-ttu-id="101bd-113">このユニットでは、IoT Central にサインインして新しいカスタム アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="101bd-113">In this unit, you sign in to IoT Central to create a new custom application.</span></span> <span data-ttu-id="101bd-114">このモジュールを完了するには、7 日間の試用版で十分です。</span><span class="sxs-lookup"><span data-stu-id="101bd-114">A 7-day trial is sufficient to complete this module.</span></span> 

1. <span data-ttu-id="101bd-115">Azure IoT Central の[アプリケーション マネージャー](https://aka.ms/iotcentral?azure-portal=true) ページに移動します。</span><span class="sxs-lookup"><span data-stu-id="101bd-115">Navigate to the Azure IoT Central [Application Manager](https://aka.ms/iotcentral?azure-portal=true) page.</span></span> 

1. <span data-ttu-id="101bd-116">サインイン ページで、Microsoft アカウントへのアクセスに使用するメール アドレスとパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="101bd-116">On the sign-in page, enter the email address and password that you use to access your Microsoft account.</span></span>

## <a name="create-a-new-custom-application"></a><span data-ttu-id="101bd-117">新しいカスタム アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="101bd-117">Create a new custom application</span></span>

1. <span data-ttu-id="101bd-118">新しい Azure IoT Central アプリケーションを作成するには、**[新しいアプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="101bd-118">To create a new Azure IoT Central application, choose **New Application**.</span></span> 

1. <span data-ttu-id="101bd-119">**[アプリケーションの作成]** ページで:</span><span class="sxs-lookup"><span data-stu-id="101bd-119">On the **Create Application** page:</span></span> 
    * <span data-ttu-id="101bd-120">支払プランに **[無料]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="101bd-120">Choose **Free** for the payment plan</span></span>
    * <span data-ttu-id="101bd-121">アプリケーション テンプレートには **[カスタム アプリケーション]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="101bd-121">Select **Custom Application** as the application template</span></span>
    * <span data-ttu-id="101bd-122">アプリケーションにわかりやすい名前を付けます (**コーヒー メーカー 01-A** など)。</span><span class="sxs-lookup"><span data-stu-id="101bd-122">Choose a friendly application name, like **Coffee Maker 01-A**</span></span>
    * <span data-ttu-id="101bd-123">(オプション) URL を編集します。この操作は、選択した名前が既に使用されている場合に必要になります。</span><span class="sxs-lookup"><span data-stu-id="101bd-123">(Optionally) edit the URL - this will be required if the name you selected is already in use</span></span>
    * <span data-ttu-id="101bd-124">**[作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="101bd-124">Choose **Create**</span></span>
