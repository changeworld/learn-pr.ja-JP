<span data-ttu-id="cc56e-101">Azure Redis Cache はメモリ内データベースであるため、メモリは Azure Redis Cache の最も重要なリソースです。</span><span class="sxs-lookup"><span data-stu-id="cc56e-101">Memory is the most critical resource for Azure Redis Cache, because it's an in-memory database.</span></span> <span data-ttu-id="cc56e-102">使用可能なメモリの量を超えるデータを追加しようとすると、問題が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="cc56e-102">You can run into problems when you begin adding data that exceeds the amount of memory available.</span></span> <span data-ttu-id="cc56e-103">Azure Redis Cache では、メモリが不足した場合のデータの処理方法を示す削除ポリシーがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="cc56e-103">Azure Redis Cache supports eviction policies, which indicate how data should be handled when you run out of memory.</span></span>

<span data-ttu-id="cc56e-104">ここでは、メモリの最大量を超えた場合のデータの処理方法を決定するために、削除ポリシーを設定します。</span><span class="sxs-lookup"><span data-stu-id="cc56e-104">Here, you'll set an eviction policy to determine what your data should do when you exceed the maximum amount of memory.</span></span>

## <a name="what-is-an-eviction-policy"></a><span data-ttu-id="cc56e-105">削除ポリシーとは</span><span class="sxs-lookup"><span data-stu-id="cc56e-105">What is an eviction policy?</span></span>

<span data-ttu-id="cc56e-106">削除ポリシーは、使用可能なメモリの最大量を超えた場合にどのようにデータを管理するかを決定する計画です。</span><span class="sxs-lookup"><span data-stu-id="cc56e-106">An eviction policy is a plan that determines how your data should be managed when you exceed the maximum amount of memory available.</span></span> <span data-ttu-id="cc56e-107">削除ポリシーを使用すると、たとえば、新たに挿入するデータ用にスペースを確保するためにランダム キーを削除するよう、Azure Redis Cache に指示することができます。</span><span class="sxs-lookup"><span data-stu-id="cc56e-107">For example, using an eviction policy, you could tell Azure Redis Cache to delete a random key to make room for the new data being inserted.</span></span>

### <a name="types-of-eviction-policies"></a><span data-ttu-id="cc56e-108">削除ポリシーの種類</span><span class="sxs-lookup"><span data-stu-id="cc56e-108">Types of eviction policies</span></span>

<span data-ttu-id="cc56e-109">Azure Redis Cache で提供される削除ポリシーには 6 つの異なる種類があります。</span><span class="sxs-lookup"><span data-stu-id="cc56e-109">There are six different eviction policies provided by Azure Redis Cache.</span></span> <span data-ttu-id="cc56e-110">メモリが不足しているときにデータを挿入しようとすると、次のすべての値によってアクションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="cc56e-110">All of these values perform an action when you attempt to insert data when you're out of memory.</span></span>

* <span data-ttu-id="cc56e-111">**noeviction:** 削除ポリシーはありません。</span><span class="sxs-lookup"><span data-stu-id="cc56e-111">**noeviction:** No eviction policy.</span></span> <span data-ttu-id="cc56e-112">データを挿入しようとすると、エラー メッセージが返されます。</span><span class="sxs-lookup"><span data-stu-id="cc56e-112">Returns an error message if you attempt to insert data.</span></span>

* <span data-ttu-id="cc56e-113">**allkeys-lru:** 最も長く使われていないキーを削除します。</span><span class="sxs-lookup"><span data-stu-id="cc56e-113">**allkeys-lru:** Removes the least recently used key.</span></span>

* <span data-ttu-id="cc56e-114">**allkeys-random:** ランダム キーを削除します。</span><span class="sxs-lookup"><span data-stu-id="cc56e-114">**allkeys-random:** Removes a random key.</span></span>

* <span data-ttu-id="cc56e-115">**volatile-lru:** 有効期限が指定されたすべてのキーの中で、最も長く使われていないキーを削除します。</span><span class="sxs-lookup"><span data-stu-id="cc56e-115">**volatile-lru:** Removes the least recently used key out of all the keys with an expiration set.</span></span>

* <span data-ttu-id="cc56e-116">**volatile ttl:** 設定された有効期限に基づいて、存続時間が最短のキーを削除します。</span><span class="sxs-lookup"><span data-stu-id="cc56e-116">**volatile-ttl:** Removes the key with the shortest time to live based on the expiration set for it.</span></span>

* <span data-ttu-id="cc56e-117">**volatile-random:** 有効期限が設定されたランダム キーを削除します。</span><span class="sxs-lookup"><span data-stu-id="cc56e-117">**volatile-random:** Removes a random key that has an expiration set.</span></span>

## <a name="how-to-set-an-eviction-policy"></a><span data-ttu-id="cc56e-118">削除ポリシーを設定する方法</span><span class="sxs-lookup"><span data-stu-id="cc56e-118">How to set an eviction policy</span></span>

<span data-ttu-id="cc56e-119">Azure では、Azure Redis Cache の削除ポリシーを設定するための簡単なドロップダウン メニューが提供されています。</span><span class="sxs-lookup"><span data-stu-id="cc56e-119">Azure provides a simple drop-down menu to set the eviction policy for Azure Redis Cache.</span></span> <span data-ttu-id="cc56e-120">**[詳細設定]** を選択し、**[maxmemory-policy]** ドロップダウン メニューを使用します。</span><span class="sxs-lookup"><span data-stu-id="cc56e-120">Select **Advanced Settings**, and use the **maxmemory-policy** drop-down menu.</span></span>

<span data-ttu-id="cc56e-121">メモリは Azure Redis Cache にとって重要であるため、削除ポリシーがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="cc56e-121">Since memory is critical to Azure Redis Cache, there is support for eviction policies.</span></span> <span data-ttu-id="cc56e-122">メモリが不足している場合に新しいデータを挿入しようとすると、削除ポリシーにより、既存のデータをどのように処理するかが決定されます。</span><span class="sxs-lookup"><span data-stu-id="cc56e-122">An eviction policy determines what should be done with existing data when you're out of memory and attempt to insert new data.</span></span>