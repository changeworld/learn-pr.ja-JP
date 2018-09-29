<span data-ttu-id="b1431-101">あなたは、大手銀行のサーバー管理者として勤務しているとします。</span><span class="sxs-lookup"><span data-stu-id="b1431-101">Suppose you work as the server administrator for a large bank.</span></span> <span data-ttu-id="b1431-102">自分の口座をインターネットやモバイル デバイスから簡単に確認できる、顧客向けの新しい銀行ポータルを立ち上げようとしています。</span><span class="sxs-lookup"><span data-stu-id="b1431-102">You are launching a new banking portal for your customers that will enable them to easily check their account from the Internet or mobile device.</span></span> <span data-ttu-id="b1431-103">これは、ビジネスに不可欠なアプリケーションで、サーバーの障害やソフトウェアのアップグレードに起因するダウンタイムをできるかぎり削減したいと考えています。</span><span class="sxs-lookup"><span data-stu-id="b1431-103">The application is business-critical, and you want to minimize the downtime that results from server failure or upgrades to the software.</span></span>

<span data-ttu-id="b1431-104">あなたはこれを Azure で起動しようとしているので、Azure が提供しているいくつかのスケーラビリティのオプションを利用したいと思っています。</span><span class="sxs-lookup"><span data-stu-id="b1431-104">Since you are launching on Azure, you want to take advantage of some of the scalability options it provides.</span></span> <span data-ttu-id="b1431-105">目標は、ロード バランサーがこの種のアプリケーションに適切か判断し、適切ならば、システムのデプロイ、構成方法を決定することです。</span><span class="sxs-lookup"><span data-stu-id="b1431-105">Your goal is to determine if a load balancer is appropriate for this type of application, and if so, how to deploy and configure the system.</span></span>

## <a name="learning-objectives"></a><span data-ttu-id="b1431-106">学習の目的:</span><span class="sxs-lookup"><span data-stu-id="b1431-106">Learning objectives:</span></span>

<span data-ttu-id="b1431-107">このモジュールでは、次のことを行います。</span><span class="sxs-lookup"><span data-stu-id="b1431-107">In this module, you will:</span></span>

- <span data-ttu-id="b1431-108">ロード バランサーを使用するタイミングを決定する</span><span class="sxs-lookup"><span data-stu-id="b1431-108">Decide when to use a load balancer</span></span>
- <span data-ttu-id="b1431-109">Basic エディションと Standard エディションのロード バランサーの違いを理解する</span><span class="sxs-lookup"><span data-stu-id="b1431-109">Identify the differences between the basic and standard Load Balancer editions</span></span>
- <span data-ttu-id="b1431-110">新しいロード バランサーを作成する</span><span class="sxs-lookup"><span data-stu-id="b1431-110">Create a new load balancer</span></span>
- <span data-ttu-id="b1431-111">ロード バランサーに関連付けた仮想ネットワークを作成する</span><span class="sxs-lookup"><span data-stu-id="b1431-111">Create virtual networks associated with a load balancer</span></span>
- <span data-ttu-id="b1431-112">ロード バランサーの規則を構成する</span><span class="sxs-lookup"><span data-stu-id="b1431-112">Configure load balancer rules</span></span>
- <span data-ttu-id="b1431-113">ロード バランサーの正常性プローブを構成する</span><span class="sxs-lookup"><span data-stu-id="b1431-113">Configure a load balancer health probe</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b1431-114">前提条件</span><span class="sxs-lookup"><span data-stu-id="b1431-114">Prerequisites</span></span>  

- <span data-ttu-id="b1431-115">Azure CLI の使用経験</span><span class="sxs-lookup"><span data-stu-id="b1431-115">Experience with the Azure CLI</span></span>
- <span data-ttu-id="b1431-116">Azure portal を使用した Azure リソースの管理経験</span><span class="sxs-lookup"><span data-stu-id="b1431-116">Experience administering Azure resources using the Azure portal</span></span>
- <span data-ttu-id="b1431-117">Azure Virtual Machines の使用経験</span><span class="sxs-lookup"><span data-stu-id="b1431-117">Experience with Azure Virtual Machines</span></span>
- <span data-ttu-id="b1431-118">仮想マシンのネットワークとパブリック IP アドレスに関するある程度の知識</span><span class="sxs-lookup"><span data-stu-id="b1431-118">Some knowledge of virtual-machine networking and public IP addresses</span></span>