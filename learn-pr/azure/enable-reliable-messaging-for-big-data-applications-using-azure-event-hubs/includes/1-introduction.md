<span data-ttu-id="549c7-101">ビッグ データ アプリケーションには、トランザクション ボリュームの増大に対応するために、スケール アウトしてスループットの増加を操作する機能が用意されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="549c7-101">Big data applications should have the ability to process increased throughput by scaling out to meet increased transaction volumes.</span></span>

<span data-ttu-id="549c7-102">銀行のクレジット カード部門に勤務しているとします。</span><span class="sxs-lookup"><span data-stu-id="549c7-102">Suppose you work in the credit card department of a bank.</span></span> <span data-ttu-id="549c7-103">不正行為がないかをテストして各トランザクションの承認または拒否を判断する役割を担うシステムを管理するチームに属しています。</span><span class="sxs-lookup"><span data-stu-id="549c7-103">You're part of a team that manages the system responsible for fraud testing to determine whether to approve or decline each transaction.</span></span> <span data-ttu-id="549c7-104">ご利用のシステムでは、トランザクションのストリームが受信されるため、リアルタイムでのトランザクションの処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="549c7-104">Your system receives a stream of transactions and needs to process them in real time.</span></span>

<span data-ttu-id="549c7-105">ご利用のシステム上の負荷は、週末および休日の間に急増することがあります。</span><span class="sxs-lookup"><span data-stu-id="549c7-105">The load on your system can spike during weekends and holidays.</span></span> <span data-ttu-id="549c7-106">ご利用のシステムでは、スループットの増加を効率的および正確に処理できる必要があります。</span><span class="sxs-lookup"><span data-stu-id="549c7-106">It should be able to handle increased throughput efficiently and accurately.</span></span> <span data-ttu-id="549c7-107">トランザクションの機密性が高い場合、わずかなエラーでも大きな影響を受ける可能性があります。</span><span class="sxs-lookup"><span data-stu-id="549c7-107">Given the sensitive nature of the transactions, even the slightest error can have a huge impact.</span></span>

<span data-ttu-id="549c7-108">Azure Event Hubs を使用すると、多数のトランザクションを受信し処理することができます。</span><span class="sxs-lookup"><span data-stu-id="549c7-108">Azure Event Hubs can receive and process large number of transactions.</span></span> <span data-ttu-id="549c7-109">また、スループットの増加を操作するために、必要に応じて、スケーリングを動的に行うように構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="549c7-109">It can also be configured to scale dynamically, when required, to handle increased throughput.</span></span>
<span data-ttu-id="549c7-110">このモジュールでは、Event Hubs をご利用のアプリケーションに接続し、大きなトランザクション ボリュームを確実に処理する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="549c7-110">In this module, you’ll learn how to connect Event Hubs to your application and reliably process huge transaction volumes.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="549c7-111">学習の目的</span><span class="sxs-lookup"><span data-stu-id="549c7-111">Learning objectives</span></span>

<span data-ttu-id="549c7-112">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="549c7-112">In this module, you will:</span></span>

- <span data-ttu-id="549c7-113">Azure CLI を使用してイベント ハブを作成します。</span><span class="sxs-lookup"><span data-stu-id="549c7-113">Create an event hub using the Azure CLI.</span></span>
- <span data-ttu-id="549c7-114">イベント ハブを経由してメッセージを送受信するようにアプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="549c7-114">Configure applications to send or receive messages through an event hub.</span></span>
- <span data-ttu-id="549c7-115">Azure portal を使用してイベント ハブのパフォーマンスを評価します。</span><span class="sxs-lookup"><span data-stu-id="549c7-115">Evaluate event hub performance using the Azure portal.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="549c7-116">前提条件</span><span class="sxs-lookup"><span data-stu-id="549c7-116">Prerequisites</span></span>

- <span data-ttu-id="549c7-117">Azure portal を使用してリソースを作成および管理した経験。</span><span class="sxs-lookup"><span data-stu-id="549c7-117">Experience creating and managing resources using the Azure portal.</span></span>
- <span data-ttu-id="549c7-118">Azure CLI 2.0 を使用して Azure へのサインインおよびリソースの作成を行った経験。</span><span class="sxs-lookup"><span data-stu-id="549c7-118">Experience with using Azure CLI 2.0 to sign into Azure, and to create resources.</span></span>
- <span data-ttu-id="549c7-119">ストリーミングやイベント処理など、ビッグ データの基本概念に関する知識。</span><span class="sxs-lookup"><span data-stu-id="549c7-119">Knowledge of basic big data concepts such as streaming and event processing.</span></span>
