<span data-ttu-id="c8d30-101">Raspberry Pi ボードは、最近、テスティング セオリーや、優れたものを作るイベントで、多くの関心を集めています。</span><span class="sxs-lookup"><span data-stu-id="c8d30-101">Raspberry Pi boards have garnered much interest of late for testing theories or event making cool things.</span></span> <span data-ttu-id="c8d30-102">このボードは非常に低コストですが、投資する前に、Raspberry Pi の機能をテストしてみたい人もいるでしょう。</span><span class="sxs-lookup"><span data-stu-id="c8d30-102">While the cost on this boards are quite low, some may be interested in testing out the Raspberry Pi functionality before investing in one.</span></span>

<span data-ttu-id="c8d30-103">Microsoft は、エミュレートされたハードウェアをユーザーがコードを使用して制御することができる、オンラインの Raspberry Pi シミュレーターを構築しました。</span><span class="sxs-lookup"><span data-stu-id="c8d30-103">Microsoft has built an online Raspberry Pi simulator allowing users to control the emulated hardware via code.</span></span> <span data-ttu-id="c8d30-104">エミュレーターは、回線を 1 つにつなぐことができるブレッドボードを介して、温度、湿度、圧力センサー、および赤色 LED に接続された Raspberry Pi のグラフィックを描きます。</span><span class="sxs-lookup"><span data-stu-id="c8d30-104">The emulator portrays a graphic of a Raspberry Pi connected to a temperature, humidity, pressure sensor, and a red LED via breadboard allowing circuits to be wired together.</span></span> <span data-ttu-id="c8d30-105">表示されるサイド パネルで、ユーザーは Node.js JavaScript コードを入力して、LED を制御し、シミュレートされたセンサーからダミー データを収集することができます。</span><span class="sxs-lookup"><span data-stu-id="c8d30-105">The displayed side panel allows users to enter Node.js JavaScript code to control the LED and collect dummy data from the simulated sensor.</span></span>

<span data-ttu-id="c8d30-106">シミュレーターは、最初の実行時に、コマンド ラインを介して表示されるサンプル温度キャプチャ プログラムを操作します。</span><span class="sxs-lookup"><span data-stu-id="c8d30-106">At first run, the simulator operates a sample temperature capture program which is displayed via the command line.</span></span> <span data-ttu-id="c8d30-107">シミュレーターは実際のデバイスに転送する前にコードをテストできるように設計されているため、同じサンプル アプリケーションを実際の Pi でも実行することができます。</span><span class="sxs-lookup"><span data-stu-id="c8d30-107">The same sample application can also be run on a real Pi as the simulator is designed to allow people to test code before transferring it to a real device.</span></span>

## <a name="web-simulator"></a><span data-ttu-id="c8d30-108">Web シミュレーター</span><span class="sxs-lookup"><span data-stu-id="c8d30-108">Web simulator</span></span>

<span data-ttu-id="c8d30-109">Web シミュレーターには、次の 3 つの領域があります。</span><span class="sxs-lookup"><span data-stu-id="c8d30-109">There are three areas in the web simulator:</span></span>

1.  <span data-ttu-id="c8d30-110">アセンブリ領域 - 既定の回線は、Pi が BME280 センサーと LED に接続する回線です。</span><span class="sxs-lookup"><span data-stu-id="c8d30-110">Assembly area - The default circuit is that a Pi connects with a BME280 sensor and an LED.</span></span> <span data-ttu-id="c8d30-111">プレビュー バージョンではこの領域はロックされているため、今のところカスタマイズを行うことはできません。</span><span class="sxs-lookup"><span data-stu-id="c8d30-111">The area is locked in preview version so currently you cannot do customization.</span></span>

2.  <span data-ttu-id="c8d30-112">コーディング領域 - Raspberry Pi を使用してコーディングするためのオンライン コード エディター。</span><span class="sxs-lookup"><span data-stu-id="c8d30-112">Coding area - An online code editor for you to code with Raspberry Pi.</span></span> <span data-ttu-id="c8d30-113">既定のサンプル アプリケーションは、BME280 センサーからセンサー データを収集し、、Azure IoT Hub に送信する際に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="c8d30-113">The default sample application helps to collect sensor data from BME280 sensor and sends to your Azure IoT Hub.</span></span> <span data-ttu-id="c8d30-114">このアプリケーションは、実際の Pi デバイスとの完全に互換性があります。</span><span class="sxs-lookup"><span data-stu-id="c8d30-114">The application is fully compatible with real Pi devices.</span></span>

3.  <span data-ttu-id="c8d30-115">統合されたコンソール ウィンドウ - コードの出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c8d30-115">Integrated console window - It shows the output of your code.</span></span> <span data-ttu-id="c8d30-116">このウィンドウの上部には、3 つのボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="c8d30-116">At the top of this window, there are three buttons.</span></span>

    -   <span data-ttu-id="c8d30-117">**[実行]** - コーディング領域でアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="c8d30-117">**Run** - Run the application in the coding area.</span></span>

    -   <span data-ttu-id="c8d30-118">**[リセット]** - コーディング領域を既定のサンプル アプリケーションにリセットします。</span><span class="sxs-lookup"><span data-stu-id="c8d30-118">**Reset** - Reset the coding area to the default sample application.</span></span>

    -   <span data-ttu-id="c8d30-119">**[折りたたむ/展開する]** - 右側には、コンソール ウィンドウの折りたたみおよび展開を行うボタンがあります。</span><span class="sxs-lookup"><span data-stu-id="c8d30-119">**Fold/Expand** - On the right side there is a button for you to fold/expand the console window.</span></span>

[<span data-ttu-id="c8d30-120">Pi オンライン シミュレーターの概要図</span><span class="sxs-lookup"><span data-stu-id="c8d30-120">Overview image of Pi online simulator</span></span>](./../media-draft/image1.png)

<!-- Reference links 
-   Online Raspberry Pi Emulator:
    <https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started>

-   <https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted>-->

