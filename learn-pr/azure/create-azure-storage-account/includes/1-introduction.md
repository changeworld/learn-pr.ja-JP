<span data-ttu-id="96723-101">ほとんどの組織には、クラウドでホストされるデータに関してさまざまな要件があります。</span><span class="sxs-lookup"><span data-stu-id="96723-101">Most organizations have diverse requirements for their cloud-hosted data.</span></span> <span data-ttu-id="96723-102">たとえば、特定のリージョンにデータを格納したり、データ カテゴリ別に請求処理を分けたりすることが、必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="96723-102">For example, you might need to store data in a specific region or you might require separate billing for different data categories.</span></span> <span data-ttu-id="96723-103">Azure ストレージ アカウントでは、この種のポリシーを形式化して、Azure のデータに適用できます。</span><span class="sxs-lookup"><span data-stu-id="96723-103">Azure storage accounts let you formalize these types of policies and apply them to your Azure data.</span></span>

<span data-ttu-id="96723-104">ココア パウダーやチョコレート チップなどの材料を製造するチョコレート メーカーで働いているものとします。</span><span class="sxs-lookup"><span data-stu-id="96723-104">Suppose you work at a chocolate manufacturer that produces baking ingredients such as cocoa powder and chocolate chips.</span></span> <span data-ttu-id="96723-105">製品を食料品店に販売し、店はそれを消費者に売っています。</span><span class="sxs-lookup"><span data-stu-id="96723-105">You market your products to grocery stores who then sells them to consumers.</span></span>

<span data-ttu-id="96723-106">成分と製造プロセスは企業秘密です。</span><span class="sxs-lookup"><span data-stu-id="96723-106">Your formulations and manufacturing processes are trade secrets.</span></span> <span data-ttu-id="96723-107">この情報を収めたスプレッドシート、ドキュメント、説明ビデオは、ビジネスに不可欠であり、geo 冗長ストレージを必要とします。</span><span class="sxs-lookup"><span data-stu-id="96723-107">The spreadsheets, documents, and instructional videos that capture this information are critical to your business and require geographically-redundant storage.</span></span> <span data-ttu-id="96723-108">このデータは主として主工場からアクセスされるので、近くのデータ センターに格納したいと考えています。</span><span class="sxs-lookup"><span data-stu-id="96723-108">This data is primarily accessed from your main factory, so you would like to store it in a nearby datacenter.</span></span> <span data-ttu-id="96723-109">このストレージの費用は、製造部門に請求される必要があります。</span><span class="sxs-lookup"><span data-stu-id="96723-109">The expense for this storage needs to be billed to the manufacturing department.</span></span>

<span data-ttu-id="96723-110">消費者に製品を売り込むためにクッキーのレシピや調理ビデオを作成している販売グループもがあります。</span><span class="sxs-lookup"><span data-stu-id="96723-110">You also have a sales group that creates cookie recipes and baking videos to promote your products to consumers.</span></span> <span data-ttu-id="96723-111">このデータに対する優先順位は、冗長性や場所より、低コストであることです。</span><span class="sxs-lookup"><span data-stu-id="96723-111">Your priority for this data is low cost rather than redundancy or location.</span></span> <span data-ttu-id="96723-112">このストレージは、販売チームに請求する必要があります。</span><span class="sxs-lookup"><span data-stu-id="96723-112">This storage must be billed to the sales team.</span></span>

<span data-ttu-id="96723-113">ここでは、複数の Azure ストレージ アカウントを作成してこの種のビジネス要件を処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="96723-113">Here, you will see how to handle these types of business requirements by creating multiple Azure storage accounts.</span></span> <span data-ttu-id="96723-114">各ストレージ アカウントには、保持しているデータに適した設定があります。</span><span class="sxs-lookup"><span data-stu-id="96723-114">Each storage account will have the appropriate settings for the data it holds.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="96723-115">学習の目的</span><span class="sxs-lookup"><span data-stu-id="96723-115">Learning objectives</span></span>

<span data-ttu-id="96723-116">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="96723-116">In this module, you will:</span></span>
 - <span data-ttu-id="96723-117">ご自分のプロジェクトに必要なストレージ アカウントの数を決定する</span><span class="sxs-lookup"><span data-stu-id="96723-117">Decide how many storage accounts you need for your project</span></span>
 - <span data-ttu-id="96723-118">各ストレージ アカウントの適切な設定を決定する</span><span class="sxs-lookup"><span data-stu-id="96723-118">Determine the appropriate settings for each storage account</span></span>
 - <span data-ttu-id="96723-119">Azure portal を使用してストレージ アカウントを作成する</span><span class="sxs-lookup"><span data-stu-id="96723-119">Create a storage account using the Azure portal</span></span>