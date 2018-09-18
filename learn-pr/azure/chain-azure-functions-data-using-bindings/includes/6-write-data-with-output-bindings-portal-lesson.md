<span data-ttu-id="d00ec-101">入力バインディングと同様に、複数の種類の出力バインディングがあります。</span><span class="sxs-lookup"><span data-stu-id="d00ec-101">Similar to input bindings, there are multiple types of output bindings.</span></span>

<span data-ttu-id="d00ec-102">出力バインディングには複数の種類がありますが、すべての種類で入力と出力の両方がサポートされているわけではありません。</span><span class="sxs-lookup"><span data-stu-id="d00ec-102">There are multiple types of output bindings, however not all types support both input and output.</span></span> <span data-ttu-id="d00ec-103">出力バインディングは、データを送信または保存するときにいつでも使用できます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-103">You'll use them anytime you want to send or store data.</span></span> <span data-ttu-id="d00ec-104">ここでは、出力バインディングをサポートする種類と、どのような場合にそれらを使用するのかを見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="d00ec-104">Here, we'll look at the types that support output bindings and when to use them.</span></span>

## <a name="output-binding-types"></a><span data-ttu-id="d00ec-105">出力バインディングの種類</span><span class="sxs-lookup"><span data-stu-id="d00ec-105">Output binding types</span></span>

- <span data-ttu-id="d00ec-106">**Blob Storage**: BLOB 出力バインディングを使用して BLOB を書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-106">**Blob Storage** You can use the blob output binding to write blobs.</span></span>

- <span data-ttu-id="d00ec-107">**Cosmos DB**: Azure Cosmos DB 出力バインディングでは、SQL API を使用して Azure Cosmos DB データベースに新しいドキュメントを書き込むことができます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-107">**Cosmos DB** The Azure Cosmos DB output binding lets you write a new document to an Azure Cosmos DB database using the SQL API.</span></span>

- <span data-ttu-id="d00ec-108">**Event Hubs**: Event Hubs 出力バインディングを使用して、イベント ストリームにイベントを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-108">**Event Hubs** Use the Event Hubs output binding to write events to an event stream.</span></span> <span data-ttu-id="d00ec-109">イベントを書き込むには、イベント ハブへの送信アクセス許可が必要です。</span><span class="sxs-lookup"><span data-stu-id="d00ec-109">You must have send permission to an event hub to write events to it.</span></span>

- <span data-ttu-id="d00ec-110">**HTTP**: HTTP 出力バインディングを使用して、HTTP 要求送信元に応答します。</span><span class="sxs-lookup"><span data-stu-id="d00ec-110">**HTTP** Use the HTTP output binding to respond to the HTTP request sender.</span></span> <span data-ttu-id="d00ec-111">このバインディングには HTTP トリガーが必要です。このバインディングを使用すると、トリガーの要求に関連付けられている応答をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-111">This binding requires an HTTP trigger and allows you to customize the response associated with the trigger's request.</span></span>

- <span data-ttu-id="d00ec-112">**Microsoft Graph**: Microsoft Graph 出力バインディングを使用すると、OneDrive 内のファイルへの書き込み、Excel データの変更、Outlook からの電子メールの送信が可能になります。</span><span class="sxs-lookup"><span data-stu-id="d00ec-112">**Microsoft Graph** Microsoft Graph output bindings allow you to write to files in OneDrive, modify Excel data, and send email through Outlook.</span></span>

- <span data-ttu-id="d00ec-113">**Mobile Apps**: Mobile Apps 出力バインディングでは、Mobile Apps テーブルに新しいレコードを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-113">**Mobile Apps** The Mobile Apps output binding writes a new record to a Mobile Apps table.</span></span>

- <span data-ttu-id="d00ec-114">**Notification Hubs**: Notification Hubs 出力バインディングを使用して、プッシュ通知を送信できます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-114">**Notification Hubs** You can send push notifications with Notification Hubs output bindings.</span></span>

- <span data-ttu-id="d00ec-115">**Queue Storage**: Azure Queue Storage 出力バインディングを使用して、キューにメッセージを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-115">**Queue Storage** Use the Azure Queue storage output binding to write messages to a queue.</span></span>

- <span data-ttu-id="d00ec-116">**SendGrid**: SendGrid バインディングを使用して電子メールを送信します。</span><span class="sxs-lookup"><span data-stu-id="d00ec-116">**Send Grid** Send emails using SendGrid bindings.</span></span>

- <span data-ttu-id="d00ec-117">**Service Bus**: Azure Service Bus 出力バインディングを使用して、キュー メッセージまたはトピック メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="d00ec-117">**Service Bus** Use Azure Service Bus output binding to send queue or topic messages.</span></span>

- <span data-ttu-id="d00ec-118">**Table Storage**: Azure Table Storage 出力バインディングを使用して、Azure Storage アカウント内のテーブルに書き込みます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-118">**Table storage** Use an Azure Table storage output binding to write to a table in an Azure Storage account.</span></span>

- <span data-ttu-id="d00ec-119">**Twilio**: Twilio を使用してテキスト メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="d00ec-119">**Twilio** Send text messages with Twilio.</span></span>

- <span data-ttu-id="d00ec-120">**Webhook**: HTTP 出力バインディングを使用して、HTTP 要求送信元に応答します。</span><span class="sxs-lookup"><span data-stu-id="d00ec-120">**Webhooks** Use the HTTP output binding to respond to the HTTP request sender.</span></span> <span data-ttu-id="d00ec-121">このバインディングには HTTP トリガーが必要です。このバインディングを使用すると、トリガーの要求に関連付けられている応答をカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-121">This binding requires an HTTP trigger and allows you to customize the response associated with the trigger's request.</span></span>


## <a name="how-to-create-an-output-binding"></a><span data-ttu-id="d00ec-122">出力バインディングを作成する方法</span><span class="sxs-lookup"><span data-stu-id="d00ec-122">How to create an output binding?</span></span>
<span data-ttu-id="d00ec-123">バインディングを入力として定義するには、`direction` を `out` として定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d00ec-123">In order to define a binding an input, you must define the `direction` as `out`.</span></span>
<span data-ttu-id="d00ec-124">パラメーターはバインディングの種類によって異なる場合があります。パラメーターの詳細については、[Microsoft のドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="d00ec-124">The parameters for each type of binding may differ, those are well documented in [Microsoft's Documentation](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)</span></span>

## <a name="combining-input-and-output-bindings"></a><span data-ttu-id="d00ec-125">入力バインディングと出力バインディングの組み合わせ</span><span class="sxs-lookup"><span data-stu-id="d00ec-125">Combining input and output bindings</span></span> 
<span data-ttu-id="d00ec-126">1 つの関数に複数のバインディングを適用できます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-126">it's possible to apply multiple bindings to a single function.</span></span> <span data-ttu-id="d00ec-127">これにより、入力バインディングと出力バインディングの両方を定義できます。</span><span class="sxs-lookup"><span data-stu-id="d00ec-127">This allows you to define both input and output bindings.</span></span>

<span data-ttu-id="d00ec-128">また、入力と出力のバインディングの種類は同じであっても構いません。</span><span class="sxs-lookup"><span data-stu-id="d00ec-128">And the input and output can even be the same binding type .....</span></span>