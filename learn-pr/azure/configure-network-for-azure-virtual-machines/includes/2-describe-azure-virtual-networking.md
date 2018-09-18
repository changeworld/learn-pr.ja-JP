<span data-ttu-id="549bb-101">維持する予定のオンプレミスのデータセンターがありますが、Azure でホストされる仮想マシン (VM) を使用してピーク時のトラフィックをオフロードするために、Azure を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="549bb-101">You have an on-premises datacenter that you plan to keep, but you want to use Azure to offload peak traffic using virtual machines (VMs) hosted in Azure.</span></span> <span data-ttu-id="549bb-102">既存の IP アドレス指定スキームとネットワーク アプライアンスを維持できるのかどうか把握する必要がある一方、すべてのデータ転送がセキュリティで保護される必要があります。</span><span class="sxs-lookup"><span data-stu-id="549bb-102">You need to know if you can keep your existing IP addressing scheme and network appliances, while ensuring that any data transfer is secure.</span></span>

## <a name="what-is-azure-virtual-networking"></a><span data-ttu-id="549bb-103">Azure 仮想ネットワークとは</span><span class="sxs-lookup"><span data-stu-id="549bb-103">What is Azure virtual networking?</span></span>

<span data-ttu-id="549bb-104">**Azure 仮想ネットワーク**を使用すると、仮想マシン、Web アプリ、データベースなどの Azure リソースが、各 Azure リソース、インターネット上のユーザー、オンプレミスのクライアント コンピューターと通信できるようになります。</span><span class="sxs-lookup"><span data-stu-id="549bb-104">**Azure virtual networks** enable Azure resources, such as virtual machines, web apps, and databases, to communicate with: each other, users on the internet, and on-premises client computers.</span></span> <span data-ttu-id="549bb-105">Azure ネットワークは、他の Azure リソースとリンクするリソースのセットと見なすことができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-105">You can think of an Azure network as a set of resources that links other Azure resources.</span></span>

<span data-ttu-id="549bb-106">Azure 仮想ネットワークには、次の主要なネットワーク機能が含まれます。</span><span class="sxs-lookup"><span data-stu-id="549bb-106">Azure virtual networks provide key networking capabilities:</span></span>

- <span data-ttu-id="549bb-107">分離とセグメント化</span><span class="sxs-lookup"><span data-stu-id="549bb-107">Isolation and segmentation</span></span>
- <span data-ttu-id="549bb-108">インターネット通信</span><span class="sxs-lookup"><span data-stu-id="549bb-108">Internet communications</span></span>
- <span data-ttu-id="549bb-109">Azure リソース間の通信</span><span class="sxs-lookup"><span data-stu-id="549bb-109">Communicate between Azure resources</span></span>
- <span data-ttu-id="549bb-110">オンプレミス リソースとの通信</span><span class="sxs-lookup"><span data-stu-id="549bb-110">Communicate with on-premises resources</span></span>
- <span data-ttu-id="549bb-111">ネットワーク トラフィックのルーティング</span><span class="sxs-lookup"><span data-stu-id="549bb-111">Route network traffic</span></span>
- <span data-ttu-id="549bb-112">ネットワーク トラフィックのフィルター処理</span><span class="sxs-lookup"><span data-stu-id="549bb-112">Filter network traffic</span></span>
- <span data-ttu-id="549bb-113">仮想ネットワークの接続</span><span class="sxs-lookup"><span data-stu-id="549bb-113">Connect virtual networks</span></span>

### <a name="isolation-and-segmentation"></a><span data-ttu-id="549bb-114">分離とセグメント化</span><span class="sxs-lookup"><span data-stu-id="549bb-114">Isolation and segmentation</span></span>

<span data-ttu-id="549bb-115">Azure では、分離された仮想ネットワークを複数作成できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-115">Azure allows you to create multiple isolated virtual networks.</span></span> <span data-ttu-id="549bb-116">仮想ネットワークを設定するときに、パブリックまたはプライベートの IP アドレス範囲のいずれかを使用して、プライベートのインターネット プロトコル (IP) アドレス空間を定義します。</span><span class="sxs-lookup"><span data-stu-id="549bb-116">When you set up a virtual network, you define a private Internet Protocol (IP) address space, using either public or private IP address ranges.</span></span> <span data-ttu-id="549bb-117">次に、その IP アドレス空間をサブネットに分割し、定義されたアドレス空間の一部をそれぞれの名前付きサブネットに割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-117">You can then segment that IP address space into subnets, and allocate part of the defined address space to each named subnet.</span></span>

<span data-ttu-id="549bb-118">名前を解決するためには、Azure の組み込みの名前解決サービスを使用するか、または仮想ネットワークを構成して、内部または外部のドメイン ネーム システム (DNS) サーバーを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-118">For name resolution, you can use the name resolution service that's built in to Azure, or you can configure the virtual network to use either an internal or an external Domain Name System (DNS) server.</span></span>

### <a name="internet-communications"></a><span data-ttu-id="549bb-119">インターネット通信</span><span class="sxs-lookup"><span data-stu-id="549bb-119">Internet communications</span></span>

<span data-ttu-id="549bb-120">Azure 内の VM は、既定でインターネットに接続できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-120">A VM in Azure can connect to the internet by default.</span></span> <span data-ttu-id="549bb-121">ただし、Azure CLI、リモート デスクトップ プロトコル (RDP)、または Secure Shell (SSH) のいずれかを使用して、その VM に接続して制御する必要があります。</span><span class="sxs-lookup"><span data-stu-id="549bb-121">However, you need to connect to and control that VM, with either the Azure CLI, Remote Desktop Protocol (RDP), or Secure Shell (SSH).</span></span> <span data-ttu-id="549bb-122">パブリック IP アドレスまたはパブリック ロード バランサーを定義することで、受信の通信を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-122">You can enable incoming communications by defining a public IP address or a public load balancer.</span></span>

### <a name="communicate-between-azure-resources"></a><span data-ttu-id="549bb-123">Azure リソース間の通信</span><span class="sxs-lookup"><span data-stu-id="549bb-123">Communicate between Azure resources</span></span>

<span data-ttu-id="549bb-124">Azure リソースが相互に安全に通信できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="549bb-124">You will want to enable Azure resources to communicate securely with each other.</span></span> <span data-ttu-id="549bb-125">次の 2 つの方法のいずれかでこれを実現します。</span><span class="sxs-lookup"><span data-stu-id="549bb-125">You can do that in one of two ways:</span></span>

- <span data-ttu-id="549bb-126">**仮想ネットワーク**</span><span class="sxs-lookup"><span data-stu-id="549bb-126">**Virtual networks**</span></span>
    
    <span data-ttu-id="549bb-127">仮想ネットワークでは、VM だけではなく、App Service 環境、Azure Kubernetes Service、Azure 仮想マシン スケール セットなど、その他の Azure リソースも接続できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-127">Virtual networks can connect not only VMs, but other Azure resources, such as the App Service Environment, Azure Kubernetes Service, and Azure virtual machine scale sets.</span></span>

- <span data-ttu-id="549bb-128">**サービス エンドポイント**</span><span class="sxs-lookup"><span data-stu-id="549bb-128">**Service endpoints**</span></span>
     
     <span data-ttu-id="549bb-129">サービス エンドポイントを使用して、Azure SQL データベースやストレージ アカウントなど、他の Azure リソースの種類に接続することができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-129">You can use service endpoints to connect to other Azure resource types, such as Azure SQL databases and storage accounts.</span></span> <span data-ttu-id="549bb-130">この方法を使用すると、複数の Azure リソースを仮想ネットワークにリンクすることが可能になり、セキュリティの向上とリソース間の最適なルーティングを実現できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-130">This approach enables you to link multiple Azure resources to virtual networks, thereby improving security and providing optimal routing between resources.</span></span>

### <a name="communicate-with-on-premises-resources"></a><span data-ttu-id="549bb-131">オンプレミス リソースとの通信</span><span class="sxs-lookup"><span data-stu-id="549bb-131">Communicate with on-premises resources</span></span>

<span data-ttu-id="549bb-132">Azure 仮想ネットワークを使用すると、オンプレミス環境と Azure サブスクリプション内でリソースをリンクできるため、実質的にローカル環境とクラウド環境にまたがるネットワークを作成できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-132">Azure virtual networks enable you to link resources together in your on-premises environment and within your Azure subscription, in effect creating a network that spans both your local and cloud environments.</span></span> <span data-ttu-id="549bb-133">この接続を実現するためのメカニズムが 3 つあります。</span><span class="sxs-lookup"><span data-stu-id="549bb-133">There are three mechanisms for you to achieve this connectivity:</span></span>

- <span data-ttu-id="549bb-134">**ポイント対サイト VPN**</span><span class="sxs-lookup"><span data-stu-id="549bb-134">**Point-to-site VPNs**</span></span>

   <span data-ttu-id="549bb-135">この方法は、組織外のコンピューターが反対方向で動作することを除いて、企業ネットワークに戻る、VPN 接続に似ています。</span><span class="sxs-lookup"><span data-stu-id="549bb-135">This approach is like a VPN connection that a computer outside your organization makes back into your corporate network, except that it's working in the opposite direction.</span></span> <span data-ttu-id="549bb-136">この場合、クライアント コンピューターが、そのコンピューターを Azure 仮想ネットワークに接続して、Azure への暗号化された VPN 接続を開始します。</span><span class="sxs-lookup"><span data-stu-id="549bb-136">In this case, the client computer initiates an encrypted VPN connection to Azure, connecting that computer to the Azure virtual network.</span></span>

- <span data-ttu-id="549bb-137">**サイト間 VPN** では、オンプレミスの VPN デバイスまたはゲートウェイが、仮想ネットワーク内で Azure VPN Gateway にリンクされます。</span><span class="sxs-lookup"><span data-stu-id="549bb-137">**Site-to-site VPNs** A site-to-site VPN links your on-premises VPN device or gateway to the Azure VPN gateway in a virtual network.</span></span> <span data-ttu-id="549bb-138">実際には、Azure 内のデバイスはローカル ネットワーク上にあるものとして表示できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-138">In effect, the devices in Azure can appear as being on the local network.</span></span> <span data-ttu-id="549bb-139">接続は暗号化され、インターネット経由で動作します。</span><span class="sxs-lookup"><span data-stu-id="549bb-139">The connection is encrypted and works over the internet.</span></span>

- <span data-ttu-id="549bb-140">**Azure ExpressRoute**</span><span class="sxs-lookup"><span data-stu-id="549bb-140">**Azure ExpressRoute**</span></span>

    <span data-ttu-id="549bb-141">より広い帯域幅とさらに高いレベルのセキュリティが必要な環境に対しては、Azure ExpressRoute が最適な方法です。</span><span class="sxs-lookup"><span data-stu-id="549bb-141">For environments where you need greater bandwidth and even higher levels of security, Azure ExpressRoute is the best approach.</span></span> <span data-ttu-id="549bb-142">Azure ExpressRoute には、インターネットを経由しない Azure への専用プライベート接続が用意されています。</span><span class="sxs-lookup"><span data-stu-id="549bb-142">Azure ExpressRoute provides dedicated private connectivity to Azure that does not travel over the internet.</span></span>

### <a name="route-network-traffic"></a><span data-ttu-id="549bb-143">ネットワーク トラフィックのルーティング</span><span class="sxs-lookup"><span data-stu-id="549bb-143">Route network traffic</span></span>

<span data-ttu-id="549bb-144">Azure では、サブネット、接続されている仮想ネットワーク、オンプレミス ネットワーク、インターネット間で、トラフィックが既定でルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="549bb-144">By default, Azure will route traffic between subnets on any connected virtual networks, on-premises networks, and the internet.</span></span> <span data-ttu-id="549bb-145">ただし、次のようにルーティングを制御して各設定をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-145">However, you can control routing and override those settings as follows:</span></span>

- <span data-ttu-id="549bb-146">**ルート テーブル**</span><span class="sxs-lookup"><span data-stu-id="549bb-146">**Route tables**</span></span>

    <span data-ttu-id="549bb-147">ルート テーブルでは、トラフィックが送信される方向に関する規則を定義できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-147">A route table allows you to define rules as to how traffic should be directed.</span></span> <span data-ttu-id="549bb-148">サブネット間でパケットがルーティングされる方法を制御する、カスタム ルート テーブルを作成できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-148">You can create custom route tables that control how packets are routed between subnets.</span></span>

- <span data-ttu-id="549bb-149">**Border Gateway Protocol**</span><span class="sxs-lookup"><span data-stu-id="549bb-149">**Border Gateway Protocol**</span></span>

    <span data-ttu-id="549bb-150">Border Gateway Protocol (BGP) は、Azure VPN Gateway または ExpressRoute と共に動作して、オンプレミスの BGP ルートを Azure 仮想ネットワークに反映させます。</span><span class="sxs-lookup"><span data-stu-id="549bb-150">Border Gateway Protocol (BGP) works with Azure VPN gateways or ExpressRoute to propagate on-premises BGP routes to Azure virtual networks.</span></span>

### <a name="filter-network-traffic"></a><span data-ttu-id="549bb-151">ネットワーク トラフィックのフィルター処理</span><span class="sxs-lookup"><span data-stu-id="549bb-151">Filter network traffic</span></span>

<span data-ttu-id="549bb-152">Azure 仮想ネットワークでは、次の方法を使用してサブネット間のトラフィックをフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-152">Azure virtual networks enable you to filter traffic between subnets by using the following approaches:</span></span>

- <span data-ttu-id="549bb-153">**ネットワーク セキュリティ グループ**</span><span class="sxs-lookup"><span data-stu-id="549bb-153">**Network security groups**</span></span>

    <span data-ttu-id="549bb-154">ネットワーク セキュリティ グループは、受信と送信に関するセキュリティ規則を複数含めることができる Azure リソースです。</span><span class="sxs-lookup"><span data-stu-id="549bb-154">A network security group is an Azure resource that can contain multiple inbound and outbound security rules.</span></span> <span data-ttu-id="549bb-155">送信元と送信先の IP アドレス、ポート、プロトコルなどの要素に基づいて、トラフィックを許可またはブロックする各規則を定義できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-155">You can define these rules to allow or block traffic, based on factors such as source and destination IP address, port, and protocol.</span></span>

- <span data-ttu-id="549bb-156">**ネットワーク仮想アプライアンス**</span><span class="sxs-lookup"><span data-stu-id="549bb-156">**Network virtual appliances**</span></span>
    
    <span data-ttu-id="549bb-157">ネットワーク仮想アプライアンスは、堅牢化されたネットワーク アプライアンスに対応する特殊な VM です。</span><span class="sxs-lookup"><span data-stu-id="549bb-157">A network virtual appliance is a specialized VM that can be compared to a hardened network appliance.</span></span> <span data-ttu-id="549bb-158">ネットワーク仮想アプライアンスでは、ファイアウォールの実行、WAN の最適化の実行など、特定のネットワーク機能が実行されます。</span><span class="sxs-lookup"><span data-stu-id="549bb-158">A network virtual appliance carries out a particular network function, such as running a firewall or performing WAN optimization.</span></span>

## <a name="connect-virtual-networks"></a><span data-ttu-id="549bb-159">仮想ネットワークの接続</span><span class="sxs-lookup"><span data-stu-id="549bb-159">Connect virtual networks</span></span>

<span data-ttu-id="549bb-160">仮想ネットワークの "_ピアリング_" を使用して、仮想ネットワークをリンクできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-160">You can link virtual networks together using virtual network _peering_.</span></span> <span data-ttu-id="549bb-161">ピアリングを使用すると、各仮想ネットワーク内のリソースを相互に通信させることができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-161">Peering enables resources in each virtual network to communicate with each other.</span></span> <span data-ttu-id="549bb-162">異なるリージョンに各仮想ネットワークを配置し、Azure を通じてグローバルに相互接続されたネットワークを作成できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-162">These virtual networks can be in separate regions, allowing you to create a global interconnected network through Azure.</span></span>

## <a name="azure-virtual-network-settings"></a><span data-ttu-id="549bb-163">Azure 仮想ネットワークの設定</span><span class="sxs-lookup"><span data-stu-id="549bb-163">Azure virtual network settings</span></span>

<span data-ttu-id="549bb-164">ローカル コンピューター上の Azure portal や Azure PowerShell から、または Azure Cloud Shell から Azure 仮想ネットワークの作成と構成を実行できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-164">You can create and configure Azure virtual networks from the Azure portal, Azure PowerShell on your local computer, or Azure Cloud Shell.</span></span>

### <a name="create-a-virtual-network"></a><span data-ttu-id="549bb-165">仮想ネットワークの作成</span><span class="sxs-lookup"><span data-stu-id="549bb-165">Create a virtual network</span></span>

<span data-ttu-id="549bb-166">Azure 仮想ネットワークを作成するときに、基本的な設定をいくつか構成します。</span><span class="sxs-lookup"><span data-stu-id="549bb-166">When you create an Azure virtual network, you configure a number of basic settings.</span></span> <span data-ttu-id="549bb-167">また、複数のサブネット、分散型サービス拒否 (DDoS) の保護、サービス エンドポイントなど、高度な設定を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-167">You also have the option to configure advanced settings, such as multiple subnets, distributed denial of service (DDoS) protection, and service endpoints.</span></span>

![[仮想ネットワークの作成] ブレード フィールドの例を示す Azure portal のスクリーンショット。](../media/2-create-virtual-network.PNG)

<span data-ttu-id="549bb-169">基本的な仮想ネットワークでは、次の設定を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="549bb-169">Settings that you need to configure for a basic virtual network are:</span></span>

- <span data-ttu-id="549bb-170">**ネットワーク名**</span><span class="sxs-lookup"><span data-stu-id="549bb-170">**Network name**</span></span>

    <span data-ttu-id="549bb-171">この名前は、サブスクリプション内で一意である必要がありますが、グローバルに一意である必要はありません。</span><span class="sxs-lookup"><span data-stu-id="549bb-171">This name must be unique in your subscription but does not need to be globally unique.</span></span> <span data-ttu-id="549bb-172">記憶したり、他の仮想ネットワークから区別したりするのが簡単になる、説明的な名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="549bb-172">Make the name a descriptive one that is easy to remember and distinguish from other virtual networks.</span></span>

- <span data-ttu-id="549bb-173">**アドレス空間**</span><span class="sxs-lookup"><span data-stu-id="549bb-173">**Address space**</span></span>
    
    <span data-ttu-id="549bb-174">仮想ネットワークを設定するときに、クラスレス ドメイン間ルーティング (CIDR) 形式で内部のアドレス空間を定義します。</span><span class="sxs-lookup"><span data-stu-id="549bb-174">When you set up a virtual network, you define the internal address space in Classless Interdomain Routing (CIDR) format.</span></span> <span data-ttu-id="549bb-175">このアドレス空間はサブスクリプション内で一意である必要があります。そのため、たとえば、ある仮想ネットワークに対して 10.0.0.0/24 のアドレス空間を定義した後、別の仮想ネットワークに対して 10.1.0.0/8 を定義することはできません。10.1.0.0/8 のネットワークが 10.0.0.0/24 と重複するためです。</span><span class="sxs-lookup"><span data-stu-id="549bb-175">This address space needs to be unique within your subscription, so you cannot define an address space of, say, 10.0.0.0/24 for one virtual network and then 10.1.0.0/8 for another, as the 10.1.0.0/8 network will overlap 10.0.0.0/24.</span></span> <span data-ttu-id="549bb-176">しかし、10.0.0.0/16 と 10.1.0.0/16 などは指定できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-176">However, you can have 10.0.0.0/16 and 10.1.0.0/16, etc.</span></span> 
    > [!NOTE] 
    > <span data-ttu-id="549bb-177">仮想ネットワークを作成したら、アドレス空間を追加できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-177">You can add address spaces after creating the virtual network.</span></span>

- <span data-ttu-id="549bb-178">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="549bb-178">**Subscription**</span></span>

    <span data-ttu-id="549bb-179">選択するサブスクリプションが複数ある場合にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="549bb-179">Only applies if you have multiple subscriptions to choose from.</span></span>

- <span data-ttu-id="549bb-180">**[リソース グループ]**</span><span class="sxs-lookup"><span data-stu-id="549bb-180">**Resource group**</span></span>
    
    <span data-ttu-id="549bb-181">他の Azure リソースと同様、仮想ネットワークはリソース グループ内に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="549bb-181">Like any other Azure resource, a virtual network needs to exist in a resource group.</span></span> <span data-ttu-id="549bb-182">既存の RG を選択するか、新しく作成することができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-182">You can either select an existing RG or create a new one.</span></span>
    
- <span data-ttu-id="549bb-183">**場所**</span><span class="sxs-lookup"><span data-stu-id="549bb-183">**Location**</span></span>

    <span data-ttu-id="549bb-184">仮想ネットワークを配置する場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="549bb-184">Select the location where you want the virtual network to exist.</span></span>

- <span data-ttu-id="549bb-185">**サブネット**</span><span class="sxs-lookup"><span data-stu-id="549bb-185">**Subnet**</span></span>
    
    <span data-ttu-id="549bb-186">各仮想ネットワークのアドレス範囲内で、仮想ネットワークのアドレス空間をパーティション分割するサブネットを、1 つまたは複数作成できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-186">Within each virtual network address range, you can create one or more subnets that partition the virtual network's address space.</span></span> <span data-ttu-id="549bb-187">次に、サブネット間のルーティングは既定のトラフィックのルーティングに依存するか、またはカスタム ルートを定義できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-187">Routing between subnets will then depend on the default traffic routes, or you can define custom routes.</span></span> <span data-ttu-id="549bb-188">または、すべての仮想ネットワークのアドレス範囲を含むサブネットを 1 つ定義できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-188">Alternatively, you can define one subnet that encompasses all the virtual networks' address ranges.</span></span>

    > [!NOTE]
    > <span data-ttu-id="549bb-189">サブネット名は、先頭にはアルファベットまたは数字、末尾にはアルファベット、数字、またはアンダースコアを使用する必要があります。また、使用できるのは、アルファベット、数字、アンダースコア、ピリオド、ハイフンのみです。</span><span class="sxs-lookup"><span data-stu-id="549bb-189">Subnet names must begin with a letter or number, end with a letter, number or underscore, and may contain only letters, numbers, underscores, periods, or hyphens.</span></span>

- <span data-ttu-id="549bb-190">**DDoS 保護**</span><span class="sxs-lookup"><span data-stu-id="549bb-190">**DDoS protection**</span></span>

    <span data-ttu-id="549bb-191">Basic または Standard のいずれかの DDoS 保護を選択できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-191">You can select either Basic or Standard DDoS protection.</span></span> <span data-ttu-id="549bb-192">Standard の DDoS Protection はプレミアム サービスです。</span><span class="sxs-lookup"><span data-stu-id="549bb-192">Standard DDoS Protection is a premium service.</span></span> <span data-ttu-id="549bb-193">Standard の DDoS 保護の詳細。</span><span class="sxs-lookup"><span data-stu-id="549bb-193">For more information on Standard DDoS protection.</span></span>

- <span data-ttu-id="549bb-194">**サービス エンドポイント**</span><span class="sxs-lookup"><span data-stu-id="549bb-194">**Service Endpoints**</span></span>
    
    <span data-ttu-id="549bb-195">ここでは、サービス エンドポイントを有効にした後、有効にする Azure サービス エンドポイントを一覧から選択します。</span><span class="sxs-lookup"><span data-stu-id="549bb-195">Here, you enable service endpoints, and then select from the list which Azure service endpoints you want to enable.</span></span> <span data-ttu-id="549bb-196">オプションには、Azure Cosmos DB、Azure Service Bus、Azure Key Vault などが含まれます。</span><span class="sxs-lookup"><span data-stu-id="549bb-196">Options include Azure Cosmos DB, Azure Service Bus, Azure Key Vault, and so on.</span></span>

<span data-ttu-id="549bb-197">これらの設定を構成したら、**[作成]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="549bb-197">When you have configured these settings, click the **Create** button.</span></span>

### <a name="define-additional-settings"></a><span data-ttu-id="549bb-198">追加設定の定義</span><span class="sxs-lookup"><span data-stu-id="549bb-198">Define additional settings</span></span>

<span data-ttu-id="549bb-199">仮想ネットワークを作成したら、さらに追加設定を定義できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-199">After creating a virtual network, you can then define further settings.</span></span> <span data-ttu-id="549bb-200">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="549bb-200">These include:</span></span>

- <span data-ttu-id="549bb-201">**ネットワーク セキュリティ グループ**</span><span class="sxs-lookup"><span data-stu-id="549bb-201">**Network security group**</span></span>
    
    <span data-ttu-id="549bb-202">ネットワーク セキュリティ グループのセキュリティ規則を使用して、仮想ネットワーク サブネットとネットワーク インターフェイスに出入りできるネットワーク トラフィックの種類をフィルター処理できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-202">Network security groups have security rules that enable you to filter the type of network traffic that can flow in and out of virtual network subnets and network interfaces.</span></span> <span data-ttu-id="549bb-203">ネットワーク セキュリティ グループを個別に作成した後、これを仮想ネットワークに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="549bb-203">You create the network security group separately, and then associate it with the virtual network.</span></span>

- <span data-ttu-id="549bb-204">**ルート テーブル**</span><span class="sxs-lookup"><span data-stu-id="549bb-204">**Route table**</span></span>

    <span data-ttu-id="549bb-205">Azure では、Azure 仮想ネットワークのサブネットごとにルート テーブルが自動的に作成され、既定のシステム ルートがテーブルに追加されます。</span><span class="sxs-lookup"><span data-stu-id="549bb-205">Azure automatically creates a route table for each subnet within an Azure virtual network and adds system default routes to the table.</span></span> <span data-ttu-id="549bb-206">ただし、カスタム ルート テーブルを追加して、仮想ネットワーク間のトラフィックを変更できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-206">However, you can add custom route tables to modify traffic between virtual networks.</span></span>

<span data-ttu-id="549bb-207">また、サービス エンドポイントを修正することもできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-207">You can also amend the service endpoints.</span></span>

![仮想ネットワークの設定を編集するためのブレードの例を示す Azure portal のスクリーンショット。](../media/2-virtual-network-additional-settings.PNG)

### <a name="configure-virtual-networks"></a><span data-ttu-id="549bb-209">仮想ネットワークを構成する</span><span class="sxs-lookup"><span data-stu-id="549bb-209">Configure virtual networks</span></span>

<span data-ttu-id="549bb-210">仮想ネットワークを作成したら、さらなる設定はすべて Azure portal の [仮想ネットワーク] ブレードから変更できます。</span><span class="sxs-lookup"><span data-stu-id="549bb-210">When you have created a virtual network, you can change any further settings from the Virtual Networks blade in the Azure portal.</span></span> <span data-ttu-id="549bb-211">または、PowerShell コマンドや Cloud Shell のコマンドを使用して変更を加えることもできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-211">Alternatively, you can use PowerShell commands or commands in Cloud Shell to make changes.</span></span>

![仮想ネットワークを構成するためのブレードの例を示す Azure portal のスクリーンショット。](../media/2-configure-virtual-network.PNG)

<span data-ttu-id="549bb-213">次に、さらなるサブ ブレードで設定の確認と変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-213">You can then review and change settings in further sub-blades.</span></span>
<span data-ttu-id="549bb-214">これらの設定には、以下が含まれます。</span><span class="sxs-lookup"><span data-stu-id="549bb-214">These settings include:</span></span>

- <span data-ttu-id="549bb-215">アドレス空間</span><span class="sxs-lookup"><span data-stu-id="549bb-215">Address spaces</span></span>

    <span data-ttu-id="549bb-216">最初の定義にさらなるアドレス空間を追加できます</span><span class="sxs-lookup"><span data-stu-id="549bb-216">You can add further address spaces to the initial definition</span></span>

- <span data-ttu-id="549bb-217">接続されているデバイス</span><span class="sxs-lookup"><span data-stu-id="549bb-217">Connected devices</span></span>

    <span data-ttu-id="549bb-218">仮想ネットワークを使用してマシンを接続します</span><span class="sxs-lookup"><span data-stu-id="549bb-218">Use the virtual network to connect machines</span></span>

- <span data-ttu-id="549bb-219">サブネット</span><span class="sxs-lookup"><span data-stu-id="549bb-219">Subnets</span></span>

    <span data-ttu-id="549bb-220">さらにサブネットを追加します</span><span class="sxs-lookup"><span data-stu-id="549bb-220">Add further subnets</span></span>

- <span data-ttu-id="549bb-221">ピアリング</span><span class="sxs-lookup"><span data-stu-id="549bb-221">Peerings</span></span>

    <span data-ttu-id="549bb-222">ピアリングの配置で仮想ネットワークをリンクします</span><span class="sxs-lookup"><span data-stu-id="549bb-222">Link virtual networks in peering arrangements</span></span>

<span data-ttu-id="549bb-223">また、仮想ネットワークの監視やトラブルシューティングを行ったり、現在の仮想ネットワークを生成するために自動化スクリプトを作成したりすることもできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-223">You can also monitor and troubleshoot virtual networks, or create an automation script to generate the current virtual network.</span></span>

## <a name="summary"></a><span data-ttu-id="549bb-224">まとめ</span><span class="sxs-lookup"><span data-stu-id="549bb-224">Summary</span></span>

<span data-ttu-id="549bb-225">仮想ネットワークは、Azure 内のエンティティを接続するための強力なメカニズムであり、自由に構成することができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-225">Virtual networks are powerful and highly configurable mechanisms for connecting entities in Azure.</span></span> <span data-ttu-id="549bb-226">Azure リソースを相互に接続したり、またはオンプレミスのリソースに接続したりできます。</span><span class="sxs-lookup"><span data-stu-id="549bb-226">You can connect Azure resources to one another or to resources you have on-premises.</span></span> <span data-ttu-id="549bb-227">ネットワーク トラフィックの分離、フィルター処理、ルーティングを実行できます。また、必要に応じて Azure でセキュリティを強化することができます。</span><span class="sxs-lookup"><span data-stu-id="549bb-227">You can isolate, filter and route your network traffic, and Azure allows you to increase security where you feel you need it.</span></span>
