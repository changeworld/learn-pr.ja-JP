<span data-ttu-id="864ee-101">あなたはプロスポーツの統計情報を記録し、結果を問い合わせる API をアプリに提供する会社で働いています。</span><span class="sxs-lookup"><span data-stu-id="864ee-101">You work at a company that tracks professional sports statistics and provides apps an API to query results.</span></span> <span data-ttu-id="864ee-102">現在行われている試合と過去の試合を問わず、ファンが試合や得点を調べるのを助けます。</span><span class="sxs-lookup"><span data-stu-id="864ee-102">It helps fans track and review games and scores, both live and historical.</span></span> <span data-ttu-id="864ee-103">"John Smith は左投手相手にホームランを何本打っているか?" など、利用者は自然言語検索でチームの統計情報を問うこともできます。</span><span class="sxs-lookup"><span data-stu-id="864ee-103">Users can also request team statistics using a natural language search, such as “How many times has John Smith hit a home run against a left-handed pitcher?”</span></span>

<span data-ttu-id="864ee-104">バックエンド サービスに需要を満たすだけの容量が与えられていないため、プレイオフ中など、需要が最も多くなるときは、サービスの応答時間が遅くなります。</span><span class="sxs-lookup"><span data-stu-id="864ee-104">During times of peak demand, such as during playoffs, your response time of your service slows down because your back-end service doesn't have the capacity to meet demand.</span></span> <span data-ttu-id="864ee-105">あなたは利用者のためにパフォーマンスを改善し、バックエンド サービスとデータ ストレージ サービスの作業負荷を減らしたいと考えます。</span><span class="sxs-lookup"><span data-stu-id="864ee-105">You want to improve performance for your users and reduce the workload on your back-end and data storage services.</span></span> <span data-ttu-id="864ee-106">数字を見ると、返されるデータの 50% から 80% は読み取り専用か、最近要求された値です。</span><span class="sxs-lookup"><span data-stu-id="864ee-106">Your metrics show that 50% to 80% of the data returned is for read-only or recently requested values.</span></span> <span data-ttu-id="864ee-107">頻繁に利用されるデータをキャッシュすれば、パフォーマンスが上がり、待ち時間が短くなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="864ee-107">Implementing a cache of commonly used data could improve performance and reduce latency.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="864ee-108">学習の目的</span><span class="sxs-lookup"><span data-stu-id="864ee-108">Learning objectives</span></span>

<span data-ttu-id="864ee-109">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="864ee-109">In this module, you will:</span></span>

- <span data-ttu-id="864ee-110">Redis Cache とは何か、何のためにそれを利用するのか説明します。</span><span class="sxs-lookup"><span data-stu-id="864ee-110">Describe what a Redis cache is, and what you can use it for.</span></span>
- <span data-ttu-id="864ee-111">Redis Cache を使用するための設計計画を作成します。</span><span class="sxs-lookup"><span data-stu-id="864ee-111">Create a design and plan to use a Redis cache.</span></span>
- <span data-ttu-id="864ee-112">Azure で Redis Cache をプロビジョニングします。</span><span class="sxs-lookup"><span data-stu-id="864ee-112">Provision a Redis cache in Azure.</span></span>
- <span data-ttu-id="864ee-113">Web アプリをキャッシュに接続します。</span><span class="sxs-lookup"><span data-stu-id="864ee-113">Connect a web app to the cache.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="864ee-114">前提条件</span><span class="sxs-lookup"><span data-stu-id="864ee-114">Prerequisites</span></span>

- <span data-ttu-id="864ee-115">アプリの開発経験</span><span class="sxs-lookup"><span data-stu-id="864ee-115">Experience with app development</span></span>
- <span data-ttu-id="864ee-116">アプリでデータを使用した経験</span><span class="sxs-lookup"><span data-stu-id="864ee-116">Experience using data in apps</span></span>