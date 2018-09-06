<span data-ttu-id="6b1f2-101">データのある部分を変更すると、データの別の部分の変更が必要になるため、一連のデータ更新をグループにまとめる必要があることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-101">There are many times when you need to group a series of data updates together because a change to one piece of data needs to result in a change to another piece of data.</span></span> <span data-ttu-id="6b1f2-102">トランザクションを使用すると、これらの更新をグループ化して、一連の更新の 1 つのイベントが失敗した場合に、グループ全体をロールバックする、つまり元に戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-102">Transactions enable you to group these updates so that if one event in a series of updates fails, then entire series can be rolled back, or undone.</span></span> <span data-ttu-id="6b1f2-103">たとえば、オンラインの小売店では、発注と支払いの確認にトランザクションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-103">For example, an online retailer could use a transaction for the placement of an order along with payment verification.</span></span> <span data-ttu-id="6b1f2-104">関連するイベントをグループ化すると、支払いの承認済みフォームを受け取るまで、在庫レベルが減りません。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-104">The grouping of the related events ensures that you don't reduce your inventory levels until an approved form of payment is received.</span></span>

<span data-ttu-id="6b1f2-105">ここでは、トランザクションとはどのようなもので、どのようなときにデータに対してトランザクションが必要になるかについて説明します。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-105">Here, you'll learn what a transaction is and when they're required for your data.</span></span>

## <a name="what-is-a-transaction"></a><span data-ttu-id="6b1f2-106">トランザクションとは</span><span class="sxs-lookup"><span data-stu-id="6b1f2-106">What is a transaction?</span></span>

<span data-ttu-id="6b1f2-107">トランザクションとは、データの取得や更新に対して独立して実行される論理単位です。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-107">A transaction is a logical unit that is independently executed for data retrieval or updates.</span></span>

<span data-ttu-id="6b1f2-108">トランザクション データベースが必要かどうか確認するには、次のように自問してみてください。データセット内のデータのある部分を変更すると、別の部分に影響がありますか。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-108">Here's the question to ask yourself regarding whether you need a transactional database: Will a change to one piece of data in your dataset impact another?</span></span> <span data-ttu-id="6b1f2-109">答えが "はい" の場合は、データベース サービスでトランザクションをサポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-109">If the answer is yes, then you'll need transaction support in your database service.</span></span>

<span data-ttu-id="6b1f2-110">トランザクションは、多くの場合、ACID 保証と呼ばれる 4 つの要件のセットによって定義されます。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-110">Transactions are often defined by a set of four requirements, referred to as ACID guarantees.</span></span> <span data-ttu-id="6b1f2-111">ACID は、原子性 (Atomicity)、一貫性 (Consistency)、分離性 (Isolation)、持続性 (Durability) の頭文字です。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-111">ACID stands for Atomicity, Consistency, Isolation, and Durability:</span></span>

* <span data-ttu-id="6b1f2-112">原子性とは、すべてのデータが更新される、またはすべてのデータが元の状態にロールバックされることを意味します。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-112">Atomicity means all the data is updated, or all the data is rolled back to its original state.</span></span>
* <span data-ttu-id="6b1f2-113">一貫性は、トランザクションの途中で何かが起きた場合に、その部分のデータが更新されないことを保証します。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-113">Consistency ensures that if something happens partway through the transaction, that part of the data isn't updated and part of it isn't.</span></span> <span data-ttu-id="6b1f2-114">データは一貫して、トランザクションを適用されるか、または適用されません。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-114">The data is consistent in applying the transaction or not.</span></span>
* <span data-ttu-id="6b1f2-115">分離性は、あるトランザクションが別のトランザクションの影響を受けないことを保証します。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-115">Isolation ensures that one transaction is not impacted by another transaction.</span></span>
* <span data-ttu-id="6b1f2-116">持続性とは、トランザクションによって行われた変更がシステムに永続的に保存されることを意味します。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-116">Durability means that the changes made due to the transaction are permanently saved in the system.</span></span> <span data-ttu-id="6b1f2-117">コミットされたデータは、障害が発生したりシステムが再起動した場合でも、データを正しい状態で使用できるように、システムによって保存されます。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-117">Committed data is saved by the system such that, even in the event of a failure and system restart, the data is available in its correct state.</span></span>

<span data-ttu-id="6b1f2-118">ACID が保証されたデータベースでは、トランザクションに対してこれらの原則が適用され、トランザクションが一貫した方法で適用されることが保証されます。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-118">When a database has ACID guarantees, it applies these principles to its transactions, and you can be assured that your transactions will be applied in a consistent manner.</span></span>

## <a name="oltp-vs-olap"></a><span data-ttu-id="6b1f2-119">OLTP と OLAP</span><span class="sxs-lookup"><span data-stu-id="6b1f2-119">OLTP vs OLAP</span></span>

<span data-ttu-id="6b1f2-120">トランザクション データベースは、OLTP (オンライン トランザクション処理) システムと呼ばれることがよくあります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-120">Transactional databases are often called OLTP (On-line Transaction Processing) systems.</span></span> <span data-ttu-id="6b1f2-121">一般に、OLTP システムは、多数のユーザーをサポートし、応答時間が速く、大量のデータを処理し、高可用性であり (つまり、最小限のダウンタイム)、通常は小規模または比較的単純なトランザクションを処理します。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-121">OLTP systems commonly support lots of users, have quick response times, handle large volumes of data, are highly available (meaning they have very minimal downtime), and typically handle small or relatively simple transactions.</span></span>

<span data-ttu-id="6b1f2-122">一方、OLAP (オンライン分析処理) システムは、一般に、少数のユーザーをサポート、応答時間が長く、可用性が低いことがあり、通常は大規模で複雑なトランザクションを処理します。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-122">On the contrary, OLAP (On-line Analytical Processing) systems commonly support fewer users, have longer response times, can be less available, and typically handle large and complex transactions.</span></span>

<span data-ttu-id="6b1f2-123">OLTP と OLAP は一時期ほど頻繁には使用されなくなりましたが、両者を比較するとアプリケーションのニーズの分類が容易になるので、理解しておくべき重要な概念です。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-123">OLTP and OLAP aren't used as frequently as they used to be, but the comparison does make it easier to categorize the needs of your application, so it's an important concept to be aware of.</span></span> 

<span data-ttu-id="6b1f2-124">トランザクション、OLTP、OLAP のことがわかったので、オンライン小売りシナリオの各データ セットを調べて、トランザクションが必要かどうかを判断しましょう。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-124">Now that we're familiar with transactions, OLTP, and OLAP, let's walk through each of the data sets in the online retail scenario and determine the need for transactions.</span></span>

### <a name="product-catalog-data"></a><span data-ttu-id="6b1f2-125">製品カタログ データ</span><span class="sxs-lookup"><span data-stu-id="6b1f2-125">Product catalog data</span></span>

<span data-ttu-id="6b1f2-126">製品カタログ データは、トランザクション データベースに格納する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-126">Product catalog data should be stored in a transactional database.</span></span> <span data-ttu-id="6b1f2-127">ユーザーが注文を出して支払いが確認されたら、品目の在庫を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-127">When users place an order and the payment is verified, the inventory for the item should be updated.</span></span> <span data-ttu-id="6b1f2-128">同様に、客のクレジット カードが拒否された場合は、注文をロールバックし、在庫を更新しないようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-128">Likewise, if the customer's credit card is declined, the order should be rolled back, and the inventory should not be updated.</span></span> <span data-ttu-id="6b1f2-129">これらの関係はすべて、トランザクションを必要とします。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-129">These relationships all require transactions.</span></span>

### <a name="photos-and-videos"></a><span data-ttu-id="6b1f2-130">写真とビデオ</span><span class="sxs-lookup"><span data-stu-id="6b1f2-130">Photos and videos</span></span>

<span data-ttu-id="6b1f2-131">製品カタログの写真やビデオについては、トランザクションのサポートは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-131">Photos and videos in a product catalog don't require transaction support.</span></span> <span data-ttu-id="6b1f2-132">写真やビデオが変更される唯一の理由は、更新が行われたり、新しいファイルが追加されたりした場合です。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-132">The only reason a change would be made to a photo or video is if an update was made or new files were added.</span></span> <span data-ttu-id="6b1f2-133">画像と実際の製品データの間には関係がありますが、本質的にトランザクションではありません。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-133">Even though there is a relationship between the image and the actual product data, it's not transactional in nature.</span></span>

### <a name="business-data"></a><span data-ttu-id="6b1f2-134">ビジネス データ</span><span class="sxs-lookup"><span data-stu-id="6b1f2-134">Business data</span></span>

<span data-ttu-id="6b1f2-135">ビジネス データの場合、すべてのデータは過去のもので変化しないので、トランザクションのサポートは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-135">For the business data, because all of the data is historical and unchanging, transaction support is not required.</span></span> <span data-ttu-id="6b1f2-136">データを扱うビジネス アナリストの場合、他の小さいデータ ポイントの合計を処理できるように、クエリで集計の操作がしばしば必要になるという、独自のニーズもあります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-136">The business analysts working with the data also have unique needs in that they often require working with aggregates in their queries, so that they can work with the totals of other smaller data points.</span></span>

## <a name="summary"></a><span data-ttu-id="6b1f2-137">まとめ</span><span class="sxs-lookup"><span data-stu-id="6b1f2-137">Summary</span></span>

<span data-ttu-id="6b1f2-138">データを正しい状態に維持することは、簡単な作業ではない場合があります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-138">Ensuring that your data is in the correct state is not always an easy task.</span></span> <span data-ttu-id="6b1f2-139">トランザクションを使用すると、データに対してデータ整合性要件が適用されるので、データの維持に役立つ場合があります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-139">Transactions can help by enforcing data integrity requirements on your data.</span></span> <span data-ttu-id="6b1f2-140">ACID の原則にメリットがあるデータの場合は、トランザクションをサポートするストレージ ソリューションを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6b1f2-140">If your data would benefit from the principles of ACID, you should choose a storage solution that supports transactions.</span></span>