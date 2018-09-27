<span data-ttu-id="61700-101">ロード バランサーを正常に機能させるには、ロード バランサーの動作を制御する設定を構成しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="61700-101">Before the load balancer will function correctly, you must configure settings that control the load balancer's behavior.</span></span> <span data-ttu-id="61700-102">ここでは、ネットワーク、正常性プローブ、セキュリティ規則、負荷分散規則、およびサーバー プールの構成について説明します。</span><span class="sxs-lookup"><span data-stu-id="61700-102">Here, you will look at configuring the network, health probe, security rules, load-balancing rules, and server pool.</span></span>

## <a name="steps-for-configuring-a-basic-public-load-balancer"></a><span data-ttu-id="61700-103">基本パブリック ロード バランサーの構成手順</span><span class="sxs-lookup"><span data-stu-id="61700-103">Steps for configuring a basic public load balancer</span></span>

<span data-ttu-id="61700-104">基本パブリック ロード バランサーの主要な構成手順の概要を次に示します。</span><span class="sxs-lookup"><span data-stu-id="61700-104">The following is an overview of the main configuration steps for a basic public load balancer.</span></span> <span data-ttu-id="61700-105">Standard Load Balancer と内部ロード バランサーの手順は同じになります。</span><span class="sxs-lookup"><span data-stu-id="61700-105">The steps for a standard load balancer and for an internal load balancer will be similar.</span></span> <span data-ttu-id="61700-106">基本的には、次の 4 つの項目をセットアップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="61700-106">There are essentially four things we need to setup:</span></span>

- <span data-ttu-id="61700-107">要求を処理するバックエンド サーバー</span><span class="sxs-lookup"><span data-stu-id="61700-107">Backend servers to process requests</span></span>
- <span data-ttu-id="61700-108">パブリック IP アドレス</span><span class="sxs-lookup"><span data-stu-id="61700-108">Public IP address</span></span>
- <span data-ttu-id="61700-109">正常性プローブ</span><span class="sxs-lookup"><span data-stu-id="61700-109">Health probe</span></span>
- <span data-ttu-id="61700-110">ロード バランサーの規則</span><span class="sxs-lookup"><span data-stu-id="61700-110">Load balancer rules</span></span>

### <a name="backend-servers"></a><span data-ttu-id="61700-111">バックエンド サーバー</span><span class="sxs-lookup"><span data-stu-id="61700-111">Backend servers</span></span>

<span data-ttu-id="61700-112">最初に、バックエンド VM プールを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="61700-112">First, you need to configure your backend VM pool.</span></span> <span data-ttu-id="61700-113">VM の可用性セットは同じでなければなりません。また、VM 独自のパブリック IP アドレスが必要です (ただし、これはご自身のパブリック エンドポイントでは実際には使用されません)。</span><span class="sxs-lookup"><span data-stu-id="61700-113">The VMs should be in the same availability set and have their own public IP address (although this will not actually be used by your public endpoints).</span></span>

<span data-ttu-id="61700-114">新しい仮想ネットワークを作成し、使用する VM プールに対してサブネットを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="61700-114">You must create a new virtual network and define a subnet for the VM pool to use.</span></span>

 <span data-ttu-id="61700-115">同じサービスを提供する VM が複数ある場合は、VM プール全体で同じファイアウォール規則が適用されるように、**ネットワーク セキュリティ グループ (NSG)** を使用します (ただしこれは、実際の負荷分散処理には含まれません)。</span><span class="sxs-lookup"><span data-stu-id="61700-115">When you have multiple VMs providing the same services, you should use a **network security group (NSG)** to ensure that the same firewall rules are in place across the VM pool (although this is not part of the actual load-balancing process).</span></span> <span data-ttu-id="61700-116">たとえば、Web アプリケーションをホストする VM については、ポート 80 (HTTP の場合) またはポート 8080 (HTTPS の場合) で受信セキュリティ規則を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="61700-116">For example, for VMs hosting web applications, you will need to create inbound security rules on port 80 for HTTP or port 8080 for HTTPS.</span></span>

### <a name="public-ip-address"></a><span data-ttu-id="61700-117">パブリック IP アドレス</span><span class="sxs-lookup"><span data-stu-id="61700-117">Public IP address</span></span>

<span data-ttu-id="61700-118">ポータルを使用して基本パブリック ロード バランサーを作成する場合、**パブリック IP アドレス**はロード バランサーのフロント エンドとして自動的に構成されます。</span><span class="sxs-lookup"><span data-stu-id="61700-118">When you create a public basic load balancer using the portal, the **public IP address** is automatically configured as the load balancer front end.</span></span>

<span data-ttu-id="61700-119">ロード バランサーの構成には**バックエンド アドレス プール**が含まれています。このプールには、そのロード バランサーに接続され、VM へのトラフィック分散に使用される各 VM の仮想 NIC の IP アドレスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="61700-119">Part of the configuration of the load balancer is the **backend address pool**, containing the IP addresses of each VM's virtual NICs that are connected to the load balancer and used to distribute traffic to the VMs.</span></span>

### <a name="health-probe"></a><span data-ttu-id="61700-120">正常性プローブ</span><span class="sxs-lookup"><span data-stu-id="61700-120">Health probe</span></span>

<span data-ttu-id="61700-121">正常性プローブでは、正常性チェックへの VM の応答に基づき、ロード バランサーのローテーションに対して VM が動的に追加または削除されます。</span><span class="sxs-lookup"><span data-stu-id="61700-121">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span> <span data-ttu-id="61700-122">既定では、プローブの試行の間隔は 15 秒です。</span><span class="sxs-lookup"><span data-stu-id="61700-122">By default, there are 15 seconds between probe attempts.</span></span> <span data-ttu-id="61700-123">プローブが連続して 2 回失敗すると、VM が異常と見なされ、サーバーが再びオンラインになるまでトラフィックはその VM にルーティングされません。</span><span class="sxs-lookup"><span data-stu-id="61700-123">After two consecutive probe failures, a VM is considered unhealthy and traffic won't be routed there until the server comes back online.</span></span>

### <a name="rules"></a><span data-ttu-id="61700-124">規則</span><span class="sxs-lookup"><span data-stu-id="61700-124">Rules</span></span>

<span data-ttu-id="61700-125">ロード バランサーの規則では、ロード バランサーがリッスンしているポートと、VM のバックエンド セットへのトラフィック送信に使用されるポートが識別されます。</span><span class="sxs-lookup"><span data-stu-id="61700-125">The load balancer rules identify the port(s) that the load balancer is listening on, and the port(s) used to send traffic to the back end set of VMs.</span></span> <span data-ttu-id="61700-126">トラフィックをルーティングするには、少なくとも 1 つの規則が必要です。</span><span class="sxs-lookup"><span data-stu-id="61700-126">You need at least one rule to route traffic.</span></span>
