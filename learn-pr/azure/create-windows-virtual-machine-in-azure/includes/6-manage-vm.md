<span data-ttu-id="c7b5e-101">ここまでで、カスタム ソフトウェアのインストール、FTP サーバーの設定、ビデオ ファイルを受信する VM の構成が完了しました。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-101">We've installed our custom software, set up an FTP server, and configured the VM to receive our video files.</span></span> <span data-ttu-id="c7b5e-102">ただし、FTP を使用してパブリック IP アドレスに接続しようとすると、その接続がブロックされることがわかります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-102">However, if we try to connect to our public IP address with FTP, we'll find that it's blocked.</span></span> 

<span data-ttu-id="c7b5e-103">オンプレミス環境内の機器では、サーバーの構成を調整することは、一般によく行われています。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-103">Making adjustments to server configuration is commonly performed with equipment in your on-premises environment.</span></span> <span data-ttu-id="c7b5e-104">この意味で、Azure VM はその環境の拡張機能だと考えることができます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-104">In this sense, you can consider Azure VMs to be an extension of that environment.</span></span> <span data-ttu-id="c7b5e-105">構成の変更、ネットワークの管理、トラフィックの開放またはブロックなどを、Azure portal、Azure CLI、または Azure PowerShell ツールを使って行うことができます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-105">You can make configuration changes, manage networks, open or block traffic, and more through the Azure portal, Azure CLI, or Azure PowerShell tools.</span></span>

<span data-ttu-id="c7b5e-106">仮想マシンの **[概要]** パネルに示される基本的な情報と管理オプションについては、既に確認しました。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-106">You've already seen some of the basic information and management options in the **Overview** panel for the virtual machine.</span></span> <span data-ttu-id="c7b5e-107">ネットワーク構成をもう少し詳しく見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-107">Let's explore network configuration a bit more.</span></span>

## <a name="opening-ports-in-azure-vms"></a><span data-ttu-id="c7b5e-108">Azure VM でポートを開く</span><span class="sxs-lookup"><span data-stu-id="c7b5e-108">Opening ports in Azure VMs</span></span>

<!-- TODO: Azure portal is inconsistent here in applying the NSG.
By default, new VMs are locked down. 

Apps can make outgoing requests, but the only inbound traffic allowed is from the virtual network (e.g. other resources on the same local network), and from Azure's Load Balancer (probe checks). -->

<span data-ttu-id="c7b5e-109">FTP をサポートする構成を調整するには、2 つのステップがあります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-109">There are two steps to adjusting the configuration to support FTP.</span></span> <span data-ttu-id="c7b5e-110">新しい VM を作成するときに、いくつかの一般的なポート (RDP、HTTP、HTTPS、SSH) を開く機会があります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-110">When you create a new VM you have an opportunity to open a few common ports (RDP, HTTP, HTTPS, and SSH).</span></span> <span data-ttu-id="c7b5e-111">ただし、ファイアウォールに対するその他の変更が必要な場合は、自分で行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-111">However, if you require other changes to the firewall, you will need to do them yourself.</span></span>

<span data-ttu-id="c7b5e-112">このプロセスには、2 つのステップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-112">The process for this involves two steps:</span></span>

1. <span data-ttu-id="c7b5e-113">ネットワーク セキュリティ グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-113">Create a Network Security Group.</span></span>
2. <span data-ttu-id="c7b5e-114">アクティブ FTP をサポートするために、ポート 20 と 21 のトラフィックを許可する受信規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-114">Create an inbound rule allowing traffic on port 20 and 21 for active FTP support.</span></span>

### <a name="what-is-a-network-security-group"></a><span data-ttu-id="c7b5e-115">ネットワーク セキュリティ グループについて</span><span class="sxs-lookup"><span data-stu-id="c7b5e-115">What is a Network Security Group?</span></span>

<span data-ttu-id="c7b5e-116">Azure ネットワーク モデルの基盤である仮想ネットワーク (VNet) によって、分離と保護が提供されます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-116">Virtual networks (VNets) are the foundation of the Azure networking model and provide isolation and protection.</span></span> <span data-ttu-id="c7b5e-117">ネットワーク セキュリティ グループ (NSG) は、ネットワーク レベルでネットワーク トラフィック規則を適用するために使う主なツールです。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-117">Network Security Groups (NSGs) are the main tool you use to enforce and control network traffic rules at the networking level.</span></span> <span data-ttu-id="c7b5e-118">NSG は、VNet 上で受信トラフィックと送信トラフィックをフィルター処理することによってソフトウェア ファイアウォールを提供する、オプションのセキュリティ レイヤーです。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-118">NSGs are an optional security layer that provides a software firewall by filtering inbound and outbound traffic on the VNet.</span></span> 

<span data-ttu-id="c7b5e-119">セキュリティ グループは、ネットワーク インターフェイス (ホストごとの規則) に関連付けるか、仮想ネットワーク内のサブネットに関連付けるか (複数のリソースに適用するため)、または両方のレベルに関連付けることができます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-119">Security groups can be associated to a network interface (for per-host rules), a subnet in the virtual network (to apply to multiple resources), or both levels.</span></span> 

#### <a name="security-group-rules"></a><span data-ttu-id="c7b5e-120">セキュリティ グループ規則</span><span class="sxs-lookup"><span data-stu-id="c7b5e-120">Security group rules</span></span>

<span data-ttu-id="c7b5e-121">NGS では、"_規則_" を使って、ネットワークを経由して移動するトラフィックを許可または拒否します。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-121">NGSs use _rules_ to allow or deny traffic moving through the network.</span></span> <span data-ttu-id="c7b5e-122">各規則は、ソースと宛先のアドレス (または範囲)、プロトコル、ポート (または範囲)、方向 (受信または送信)、数値の優先度を識別し、規則に合致するトラフィックを許可するか拒否するかを決定します。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-122">Each rule identifies the source and destination address (or range), protocol, port (or range), direction (inbound or outbound), a numeric priority, and whether to allow or deny the traffic that matches the rule.</span></span> <span data-ttu-id="c7b5e-123">次の図は、サブネットとネットワーク インターフェイスのレベルで適用された NSG 規則を示したものです。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-123">The following illustration shows NSG rules applied at the subnet and network interface levels.</span></span>

![2 つの異なるサブネットでのネットワーク セキュリティ グループのアーキテクチャを示す図。](../media/7-nsg-rules.png)

<span data-ttu-id="c7b5e-127">各セキュリティ グループには既定のセキュリティ規則があり、上記で説明した既定のネットワーク規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-127">Each security group has a set of default security rules to apply the default network rules described above.</span></span> <span data-ttu-id="c7b5e-128">これらの既定の規則は変更できませんが、"_オーバーライド_" することはできます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-128">These default rules cannot be modified, but _can_ be overridden.</span></span>

#### <a name="how-azure-uses-network-rules"></a><span data-ttu-id="c7b5e-129">Azure によってネットワーク規則が使用される方法</span><span class="sxs-lookup"><span data-stu-id="c7b5e-129">How Azure uses network rules</span></span>

<span data-ttu-id="c7b5e-130">受信トラフィックの場合は、サブネットに関連付けられているセキュリティ グループが処理され、次にネットワーク インターフェイスに適用されているセキュリティ グループが処理されます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-130">For inbound traffic, Azure processes the security group associated to the subnet, then the security group applied to the network interface.</span></span> <span data-ttu-id="c7b5e-131">送信トラフィックは、逆の順番に処理されます (最初にネットワーク インターフェイス、その後にサブネット)。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-131">Outbound traffic is processed in the opposite order (the network interface first, followed by the subnet).</span></span>

> [!WARNING]
> <span data-ttu-id="c7b5e-132">両方のレベルでセキュリティ グループはオプションであることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-132">Keep in mind that security groups are optional at both levels.</span></span> <span data-ttu-id="c7b5e-133">セキュリティ グループが適用されていない場合、Azure によって**すべてのトラフィックが許可されます**。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-133">If no security group is applied then **all traffic is allowed** by Azure.</span></span> <span data-ttu-id="c7b5e-134">これは、VM にパブリック IP がある場合、特に OS に何らかのファイアウォールが用意されていない場合に、重大なリスクになる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-134">If the VM has a public IP, this could be a serious risk particularly if the OS doesn't provide some sort of firewall.</span></span>

<span data-ttu-id="c7b5e-135">規則は "_優先度順_" に評価されます (**最も低い優先度**の規則から開始されます)。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-135">The rules are evaluated in _priority-order_, starting with the **lowest priority** rule.</span></span> <span data-ttu-id="c7b5e-136">拒否規則は常に評価を**停止**します。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-136">Deny rules always **stop** the evaluation.</span></span> <span data-ttu-id="c7b5e-137">たとえば、送信要求がネットワーク インターフェイス規則によってブロックされている場合、サブネットに適用されている規則はチェックされません。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-137">For example, if an outbound request is blocked by a network interface rule, any rules applied to the subnet will not be checked.</span></span> <span data-ttu-id="c7b5e-138">セキュリティ グループを経由するトラフィックが許可されるためには、適用されている "_すべての_" グループをトラフィックが通過する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-138">In order for traffic to be allowed through the security group, it must pass through _all_ applied groups.</span></span>

<span data-ttu-id="c7b5e-139">最後の規則は常に**すべて拒否**の規則です。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-139">The last rule is always a **Deny All** rule.</span></span> <span data-ttu-id="c7b5e-140">これは、受信トラフィックと送信トラフィックの両方を対象に、すべてのセキュリティ グループに優先度 65500 として追加される既定の規則です。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-140">This is a default rule added to every security group for both inbound and outbound traffic with a priority of 65500.</span></span> <span data-ttu-id="c7b5e-141">つまり、トラフィックがセキュリティ グループを通過するようにするには、"_許可規則_" を設定する必要があります。そうしないと、既定の最後の規則によってブロックされます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-141">That means to have traffic pass through the security group _you must have an allow rule_ or it will be blocked by the default final rule.</span></span>

> [!NOTE]
> <span data-ttu-id="c7b5e-142">SMTP (ポート 25) は特殊なケースであり、サブスクリプション レベルによって、およびアカウントがいつ作成されたかによって、送信 SMTP トラフィックがブロックされる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-142">SMTP (port 25) is a special case, depending on your subscription level and when your account was created, outbound SMTP traffic may be blocked.</span></span> <span data-ttu-id="c7b5e-143">ビジネス上の妥当性がある場合には、この制限の解除を要求することができます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-143">You can make a request to remove this restriction with business justification.</span></span>

<span data-ttu-id="c7b5e-144">この VM ではセキュリティ グループを作成しなかったため、作成して適用してみましょう。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-144">Since we didn't create a security group for this VM, let's do that and apply it.</span></span>

## <a name="creating-network-security-groups"></a><span data-ttu-id="c7b5e-145">ネットワーク セキュリティ グループの作成</span><span class="sxs-lookup"><span data-stu-id="c7b5e-145">Creating Network Security Groups</span></span>

<span data-ttu-id="c7b5e-146">セキュリティ グループは、Azure のほとんどすべてと同様の管理対象リソースです。Azure portal またはコマンド ライン スクリプト ツールを使用して作成することができます。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-146">Security groups are managed resources like most everything in Azure, you can create them in the Azure portal or through command-line scripting tools.</span></span> <span data-ttu-id="c7b5e-147">規則を定義することが課題になります。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-147">The challenge is in defining the rules.</span></span> <span data-ttu-id="c7b5e-148">FTP アクセスを許可する新しい規則の定義を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="c7b5e-148">Let's look at defining a new rule to allow FTP access.</span></span>