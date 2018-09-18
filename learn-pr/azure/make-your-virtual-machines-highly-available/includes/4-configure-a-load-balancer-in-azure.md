<span data-ttu-id="30bd9-101">ロード バランサーを正常に機能させるには、ロード バランサーの動作を制御する設定を構成しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="30bd9-101">Before the load balancer will function correctly, you must configure settings that control the load balancer's behavior.</span></span> <span data-ttu-id="30bd9-102">ここでは、ネットワーク、正常性プローブ、セキュリティ規則、負荷分散規則、およびサーバー プールの構成について説明します。</span><span class="sxs-lookup"><span data-stu-id="30bd9-102">Here, you will look at configuring the network, health probe, security rules, load-balancing rules, and server pool.</span></span>

## <a name="steps-to-configure-a-basic-public-load-balancer"></a><span data-ttu-id="30bd9-103">パブリック Basic Load Balancer の構成手順</span><span class="sxs-lookup"><span data-stu-id="30bd9-103">Steps to configure a Basic Public Load Balancer</span></span>

<span data-ttu-id="30bd9-104">パブリック Basic Load balancer の主な構成手順の概要を次に示します。Standard Load Balancer および内部 Load Balancer については、同様の手順で構成できます。</span><span class="sxs-lookup"><span data-stu-id="30bd9-104">The following is an overview of the main configuration steps for a Basic Public Load balancer; the steps for a Standard Load Balancer, and for an internal Load Balancer will be similar.</span></span>

### <a name="backend-servers"></a><span data-ttu-id="30bd9-105">バックエンド サーバー</span><span class="sxs-lookup"><span data-stu-id="30bd9-105">Backend servers</span></span>

<span data-ttu-id="30bd9-106">最初に、バックエンド VM プールを構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="30bd9-106">First, you need to configure your backend VM pool.</span></span> <span data-ttu-id="30bd9-107">VM の可用性セットは同じでなければなりません。また、VM 独自のパブリック IP アドレスが必要です (ただし、これはご自身のパブリック エンドポイントでは実際には使用されません)。</span><span class="sxs-lookup"><span data-stu-id="30bd9-107">The VMs should be in the same availability set and have their own public IP address (although this will not actually be used by your public endpoints).</span></span>

<span data-ttu-id="30bd9-108">新しい仮想ネットワーク (vNet) を作成し、使用する VM プールに対してサブネットを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="30bd9-108">You must create a new virtual network (vNet), and define a subnet for the VM pool to use.</span></span>

<span data-ttu-id="30bd9-109">負荷分散に直接は含まれませんが、同じサービスを提供する VM が複数ある場合は、VM プール全体で同じファイアウォール規則が適用されるように、**ネットワーク セキュリティ グループ (NSG)** を使用します。</span><span class="sxs-lookup"><span data-stu-id="30bd9-109">While not part of load balancing directly, when you have multiple VMs providing the same services, you should use a **Network Security Group (NSG)** to ensure that the same firewall rules are in place across the VM pool.</span></span> <span data-ttu-id="30bd9-110">たとえば、Web アプリケーションをホストする VM については、ポート 80 (HTTP の場合) またはポート 8080 (HTTPS の場合) で受信セキュリティ規則を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="30bd9-110">For example, for VMs hosting web applications, you will need to create inbound security rules on port 80 for HTTP or port 8080 for HTTPS.</span></span>

### <a name="public-ip-address"></a><span data-ttu-id="30bd9-111">パブリック IP アドレス</span><span class="sxs-lookup"><span data-stu-id="30bd9-111">Public IP address</span></span>

<span data-ttu-id="30bd9-112">ポータルを使用してパブリック Basic ロード バランサーを作成する場合、**パブリック IP アドレス**はロード バランサーのフロント エンドとして自動的に構成されます。</span><span class="sxs-lookup"><span data-stu-id="30bd9-112">When you create a public Basic load balancer using the portal, the **public IP address** is automatically configured as the load balancer's front end.</span></span>

<span data-ttu-id="30bd9-113">ロード バランサーの構成には**バックエンド アドレス プール**が含まれています。このプールには、そのロード バランサーに接続され、VM へのトラフィック分散に使用される各 VM の仮想 NIC の IP アドレスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="30bd9-113">Part of the configuration of the load balancer is the **backend address pool**, containing the IP addresses of each VM's virtual NICs that are connected to the load balancer and used to distribute traffic to the VMs.</span></span> 

### <a name="health-probe"></a><span data-ttu-id="30bd9-114">正常性プローブ</span><span class="sxs-lookup"><span data-stu-id="30bd9-114">Health probe</span></span>

<span data-ttu-id="30bd9-115">正常性プローブでは、正常性チェックへの VM の応答に基づき、ロード バランサーのローテーションに対して VM が動的に追加または削除されます。</span><span class="sxs-lookup"><span data-stu-id="30bd9-115">The health probe dynamically adds or removes VMs from the load balancer rotation based on their response to health checks.</span></span>
<span data-ttu-id="30bd9-116">既定では、プローブの試行の間隔は 15 秒で、プローブが連続して 2 回失敗すると、VM が異常と見なされます。</span><span class="sxs-lookup"><span data-stu-id="30bd9-116">By default, there are 15 seconds between probe attempts, and after two consecutive probe failures a VM is considered unhealthy.</span></span>

### <a name="rules"></a><span data-ttu-id="30bd9-117">ルール</span><span class="sxs-lookup"><span data-stu-id="30bd9-117">Rules</span></span>

<span data-ttu-id="30bd9-118">ロード バランサーの規則では、フロントエンドがリッスンしているポートと、バックエンドへのトラフィック送信に使用されるポートが指定されます。</span><span class="sxs-lookup"><span data-stu-id="30bd9-118">The load balancer rule specifies the port that the frontend is listening on, and the port used to send traffic to the backend.</span></span>