<span data-ttu-id="d6571-101">あなたは、音楽共有アプリケーション用のアーキテクチャを計画中であるとします。</span><span class="sxs-lookup"><span data-stu-id="d6571-101">Suppose you are planning the architecture for your music-sharing application.</span></span> <span data-ttu-id="d6571-102">あなたはモバイル アプリから Web API に音楽ファイルが確実にアップロードされるようにしたいと考えています。</span><span class="sxs-lookup"><span data-stu-id="d6571-102">You want to ensure that music files are uploaded to the web API reliably from the mobile app.</span></span>

<span data-ttu-id="d6571-103">この通信をメッセージのやりとりとする必要があることを理解したら、Azure Storage キューまたは Azure Service Bus のいずれを使用するかを選択する必要があります。この両方を使用すると、それらが処理を待機しているときに、メッセージのキューを格納することができます。</span><span class="sxs-lookup"><span data-stu-id="d6571-103">Having understood that this communication should be a message, you must choose whether to use Azure Storage queues or Azure Service Bus, both of which can be used to store queues of messages as they await processing.</span></span>

## <a name="delivery-guarantees"></a><span data-ttu-id="d6571-104">配信保証</span><span class="sxs-lookup"><span data-stu-id="d6571-104">Delivery Guarantees</span></span>

<span data-ttu-id="d6571-105">キュー内の各メッセージを宛先コンポーネントに配信する処理は、通常、キュー システムによって保証されます。</span><span class="sxs-lookup"><span data-stu-id="d6571-105">Queuing systems usually guarantee delivery of each message in the queue to a destination component.</span></span> <span data-ttu-id="d6571-106">ただし、このような保証は、次のようなさまざまなアプローチで実現できます。</span><span class="sxs-lookup"><span data-stu-id="d6571-106">However, these guarantees can take different approaches:</span></span>

- <span data-ttu-id="d6571-107">**At-Least-Once 配信。**</span><span class="sxs-lookup"><span data-stu-id="d6571-107">**At-Least-Once Delivery.**</span></span> <span data-ttu-id="d6571-108">このアプローチでは、キューからメッセージを取得するコンポーネントの少なくとも 1 つへ、各メッセージが配信されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="d6571-108">In this approach, each message is guaranteed to be delivered to at least one of the components that retrieve messages from the queue.</span></span> <span data-ttu-id="d6571-109">ただし、特定の状況では、同じメッセージが複数回配信される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-109">Note, however, that in certain circumstances, it is possible that the same message may be delivered more than once.</span></span> <span data-ttu-id="d6571-110">たとえば、キューからメッセージを取得する Web アプリのインスタンスが 2 つある場合、通常、各メッセージはこれらのインスタンスの 1 つだけに送信されます。</span><span class="sxs-lookup"><span data-stu-id="d6571-110">For example, if there are two instances of a web app retrieving messages from a queue, ordinarily each message goes to only one of those instances.</span></span> <span data-ttu-id="d6571-111">ただし、一方のインスタンスでメッセージの処理に時間がかかり、タイムアウトになった場合、もう一方のインスタンスにもメッセージが送信される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-111">However, if one instance takes a long time to process the message, and a time-out expires, the message may be sent to the other instance as well.</span></span> <span data-ttu-id="d6571-112">Web アプリ コードは、この可能性を考慮して設計する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-112">Your web app code should be designed with this possibility in mind.</span></span>
- <span data-ttu-id="d6571-113">**At-Most-Once 配信。**</span><span class="sxs-lookup"><span data-stu-id="d6571-113">**At-Most-Once Delivery.**</span></span> <span data-ttu-id="d6571-114">このアプローチでは、各メッセージの配信は保証されず、各メッセージが届かない可能性がごくわずかにあります。</span><span class="sxs-lookup"><span data-stu-id="d6571-114">In this approach, each message is not guaranteed to be delivered, and there is a very small chance that it may not arrive.</span></span> <span data-ttu-id="d6571-115">ただし、At-Least-Once 配信の場合とは異なり、同じメッセージが 2 回配信されることはありません。</span><span class="sxs-lookup"><span data-stu-id="d6571-115">However, unlike At-Least-Once delivery, there is no chance that the message will be delivered twice.</span></span>
- <span data-ttu-id="d6571-116">**先入れ先出し法 (FIFO)。**</span><span class="sxs-lookup"><span data-stu-id="d6571-116">**First-In-First-Out (FIFO).**</span></span> <span data-ttu-id="d6571-117">ほとんどのメッセージング システムでは、通常、メッセージはキューに入れられたのと同じ順番でキューから出されますが、この順番が保証されているかどうかを検討する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-117">In most messaging systems, messages usually leave the queue in the same order in which they were added, but you should consider whether this order is guaranteed.</span></span> <span data-ttu-id="d6571-118">分散アプリケーションにおいてメッセージが正確に正しい順序で処理される必要がある場合、FIFO 保証を含むキュー システムを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-118">If your distributed application requires that messages are processed in precisely the correct order, you must choose a queue system that includes a FIFO guarantee.</span></span>

## <a name="transactions"></a><span data-ttu-id="d6571-119">トランザクション</span><span class="sxs-lookup"><span data-stu-id="d6571-119">Transactions</span></span>

<span data-ttu-id="d6571-120">いくつかのメッセージ グループが密接に関連しているために、グループ内の 1 つのメッセージについて配信が失敗すると、問題が発生する場合があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-120">Some closely related groups of messages may cause problems when delivery fails for one message in the group.</span></span>

<span data-ttu-id="d6571-121">たとえば、e コマース アプリケーションを考えてみます。</span><span class="sxs-lookup"><span data-stu-id="d6571-121">For example, consider an e-commerce application.</span></span> <span data-ttu-id="d6571-122">ユーザーが "購入" ボタンがクリックすると、注文の詳細を含むメッセージが、クレジット カード情報を含むメッセージと共に送信されます。</span><span class="sxs-lookup"><span data-stu-id="d6571-122">When the user clicks the "Buy" button, a message with the order details is sent along with a message with the credit card details.</span></span> <span data-ttu-id="d6571-123">クレジット カード メッセージが配信されない場合、支払いなしで注文内容が出荷される場合があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-123">If the credit card message is not delivered, the order may be dispatched without payment.</span></span>

<span data-ttu-id="d6571-124">2 つのメッセージをトランザクションにグループ化することにより、このような種類の問題を回避できます。</span><span class="sxs-lookup"><span data-stu-id="d6571-124">You can avoid these kinds of problems by grouping the two messages into a transaction.</span></span> <span data-ttu-id="d6571-125">トランザクションは単一のユニットとして成功または失敗します。</span><span class="sxs-lookup"><span data-stu-id="d6571-125">Transactions succeed or fail as a single unit.</span></span> <span data-ttu-id="d6571-126">クレジット カード情報のメッセージ配信が失敗した場合は、注文詳細のメッセージ配信も失敗します。</span><span class="sxs-lookup"><span data-stu-id="d6571-126">If the credit card details message delivery fails, then so will the order details message.</span></span>

## <a name="when-to-use-storage-queues"></a><span data-ttu-id="d6571-127">ストレージ キューを使用する場合</span><span class="sxs-lookup"><span data-stu-id="d6571-127">When to use Storage queues</span></span>

<span data-ttu-id="d6571-128">キューは、メッセージに対して使用され、イベントに対しては使用されません。</span><span class="sxs-lookup"><span data-stu-id="d6571-128">Queues are used for messages, but not events.</span></span>

<span data-ttu-id="d6571-129">Azure Storage アカウントにはキューが含まれています。分散アプリケーションでは、このキューを、メッセージ用の一時的な簡易保存場所として使用することができます。</span><span class="sxs-lookup"><span data-stu-id="d6571-129">Azure storage accounts include queues, which can be used by distributed applications as simple, temporary storage locations for messages.</span></span> <span data-ttu-id="d6571-130">ソース コンポーネントでは、キューにメッセージを追加することができます。</span><span class="sxs-lookup"><span data-stu-id="d6571-130">A source component can add a message to the queue.</span></span> <span data-ttu-id="d6571-131">そのメッセージは宛先コンポーネントによって、キューの先頭で取得され、処理されます。</span><span class="sxs-lookup"><span data-stu-id="d6571-131">Destination components retrieve the message at the front of the queue for processing.</span></span> <span data-ttu-id="d6571-132">このようなキューにより、メッセージ交換の信頼性が向上します。それは混み合っているときに、宛先コンポーネント側でメッセージを処理する準備が整うまで、メッセージを単純に待機させることができるからです。</span><span class="sxs-lookup"><span data-stu-id="d6571-132">Such queues increase the reliability of the message exchange because, at times of high demand, messages can simply wait until a destination component is ready to process them.</span></span>

<span data-ttu-id="d6571-133">キューは、Service Bus メッセージングの一部としても提供されます。</span><span class="sxs-lookup"><span data-stu-id="d6571-133">Queues are also provided as part of Service Bus messaging.</span></span> <span data-ttu-id="d6571-134">Azure 内で特定の通信向けにキューを使用することにした場合は、ストレージ キューまたは Service Bus キューのいずれを使用するかを決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-134">If you have decided to use a queue in Azure for a specific communication, you must decide whether to use a Storage queue or a Service Bus queue.</span></span>

<span data-ttu-id="d6571-135">この選択を行うには、次の問題を検討してください。</span><span class="sxs-lookup"><span data-stu-id="d6571-135">To make this choice, consider the following questions:</span></span>

- <span data-ttu-id="d6571-136">At-Most-Once 配信保証が必要ですか?</span><span class="sxs-lookup"><span data-stu-id="d6571-136">Do you need an At-Most-Once delivery guarantee?</span></span>
- <span data-ttu-id="d6571-137">FIFO 保証が必要ですか?</span><span class="sxs-lookup"><span data-stu-id="d6571-137">Do you need a FIFO guarantee?</span></span>
- <span data-ttu-id="d6571-138">メッセージをトランザクションにグループ化する必要がありますか?</span><span class="sxs-lookup"><span data-stu-id="d6571-138">Do you need to group messages into transactions?</span></span>
- <span data-ttu-id="d6571-139">64 KB を超えるメッセージを格納する必要がありますか?</span><span class="sxs-lookup"><span data-stu-id="d6571-139">Do you need to store messages larger than 64 KB?</span></span>

<span data-ttu-id="d6571-140">これらの質問のいずれかに "はい" と答えた場合、Service Bus キューの使用を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-140">If the answer to any of these questions is yes, you must choose to use a Service Bus queue.</span></span>

<span data-ttu-id="d6571-141">さらに、次の質問を検討してください。</span><span class="sxs-lookup"><span data-stu-id="d6571-141">In addition, consider the following questions:</span></span>

- <span data-ttu-id="d6571-142">キューを通ったすべてのメッセージのサーバー側のログが必要ですか?</span><span class="sxs-lookup"><span data-stu-id="d6571-142">Do you need server-side logs of all messages that pass through the queue?</span></span>
- <span data-ttu-id="d6571-143">キューのサイズは 80 GB を超えると予想されますか?</span><span class="sxs-lookup"><span data-stu-id="d6571-143">Do you expect the queue to exceed 80 GB in size?</span></span>

<span data-ttu-id="d6571-144">これらの質問のいずれかに "はい" と答えた場合、ストレージ キューの使用を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d6571-144">If the answer to any of these questions is yes, you must choose to use a Storage queue.</span></span>

## <a name="summary"></a><span data-ttu-id="d6571-145">まとめ</span><span class="sxs-lookup"><span data-stu-id="d6571-145">Summary</span></span>

<span data-ttu-id="d6571-146">キューは、分散アプリケーションのコンポーネント間で送信されるメッセージ用の一時的な簡易保存場所です。</span><span class="sxs-lookup"><span data-stu-id="d6571-146">A queue is a simple, temporary storage location for messages sent between the components of a distributed application.</span></span> <span data-ttu-id="d6571-147">キューを使用すると、メッセージを整理し、予測できない需要急増を適切に対処できます。</span><span class="sxs-lookup"><span data-stu-id="d6571-147">Use a queue to organize messages and gracefully handle unpredictable surges in demand.</span></span>

<span data-ttu-id="d6571-148">簡単にコーディングできるシンプルなキュー システムを望んでいる場合は、Storage キューを使用します。</span><span class="sxs-lookup"><span data-stu-id="d6571-148">Use Storage queues when you want a simple and easy-to-code queue system.</span></span> <span data-ttu-id="d6571-149">より高度なニーズに対しては、Service Bus キューを使用します。</span><span class="sxs-lookup"><span data-stu-id="d6571-149">For more advanced needs, use Service Bus queues.</span></span>