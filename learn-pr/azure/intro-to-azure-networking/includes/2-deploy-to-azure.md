<span data-ttu-id="196ed-101">最初の手順は、おそらく、オンプレミスの構成をクラウドに再作成することです。</span><span class="sxs-lookup"><span data-stu-id="196ed-101">Your first step will likely be to re-create your on-premises configuration in the cloud.</span></span>

<span data-ttu-id="196ed-102">この基本的な構成により、ネットワークがどのように構成され、ネットワーク トラフィックがどのように Azure を出入りしているかについての感覚をつかむことができます。</span><span class="sxs-lookup"><span data-stu-id="196ed-102">This basic configuration will give you a sense of how networks are configured, and how network traffic moves in and out of Azure.</span></span>

## <a name="your-e-commerce-site-at-a-glance"></a><span data-ttu-id="196ed-103">e コマース サイトの概要</span><span class="sxs-lookup"><span data-stu-id="196ed-103">Your e-commerce site at a glance</span></span>

<span data-ttu-id="196ed-104">大規模なエンタープライズ システムは、多くの場合、連動する相互接続された複数のアプリケーションとサービスで構成されます。</span><span class="sxs-lookup"><span data-stu-id="196ed-104">Larger enterprise systems are often composed of multiple inter-connected applications and services that work together.</span></span> <span data-ttu-id="196ed-105">在庫を表示し、顧客が注文できるフロントエンド Web システムを用意することがあります。</span><span class="sxs-lookup"><span data-stu-id="196ed-105">You might have a front-end web system that displays inventory and allows customers to create an order.</span></span> <span data-ttu-id="196ed-106">そのようなシステムは、在庫データを提供したり、ユーザー プロファイルを管理したり、クレジット カードを処理したり、処理された注文の受注処理を要求したりするために、さまざまな Web サービスと通信します。</span><span class="sxs-lookup"><span data-stu-id="196ed-106">That might talk to a variety of web services to provide the inventory data, manage user profiles, process credit cards, and request fulfillment of processed orders.</span></span>

<span data-ttu-id="196ed-107">これらの複雑なシステムの設計、構築、管理、保守を簡単にするために、ソフトウェア アーキテクトとデザイナーはいくつかの戦略とパターンを採用します。</span><span class="sxs-lookup"><span data-stu-id="196ed-107">There are several strategies and patterns employed by software architects and designers to make these complex systems easier to design, build, manage, and maintain.</span></span> <span data-ttu-id="196ed-108">そのうちのいくつかを見てみましょう。まずは_疎結合アーキテクチャ_です。</span><span class="sxs-lookup"><span data-stu-id="196ed-108">Let's look at a few of them, starting with _loosely coupled architectures_.</span></span>

#### <a name="benefits-of-loosely-coupled-architectures"></a><span data-ttu-id="196ed-109">疎結合アーキテクチャの利点</span><span class="sxs-lookup"><span data-stu-id="196ed-109">Benefits of Loosely Coupled Architectures</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yHrc]

### <a name="using-an-n-tier-architecture"></a><span data-ttu-id="196ed-110">n 層アーキテクチャの使用</span><span class="sxs-lookup"><span data-stu-id="196ed-110">Using an N-tier architecture</span></span>

<span data-ttu-id="196ed-111">疎結合システムの構築に使用できるアーキテクチャ パターンは、_n 層_です。</span><span class="sxs-lookup"><span data-stu-id="196ed-111">An architectural pattern that can be used to build loosely coupled systems is _N-tier_.</span></span>

<span data-ttu-id="196ed-112">[n 層アーキテクチャ](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier)では、アプリケーションが 2 つ以上の論理層に分割されます。</span><span class="sxs-lookup"><span data-stu-id="196ed-112">An [N-tier architecture](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier) divides an application into two or more logical tiers.</span></span> <span data-ttu-id="196ed-113">アーキテクチャ上、上の階層は下の階層からのサービスにアクセスすることができますが、下の階層が上の階層にアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="196ed-113">Architecturally, a higher tier can access services from a lower tier, but a lower tier should never access a higher tier.</span></span>

<span data-ttu-id="196ed-114">階層は問題を分割するのに役立ち、再利用できるように設計するのが理想的です。</span><span class="sxs-lookup"><span data-stu-id="196ed-114">Tiers help separate concerns, and are ideally designed to be reusable.</span></span> <span data-ttu-id="196ed-115">階層型アーキテクチャを使用することで、メンテナンスも簡略化されます。</span><span class="sxs-lookup"><span data-stu-id="196ed-115">Using a tiered architecture also simplifies maintenance.</span></span> <span data-ttu-id="196ed-116">階層は個別に更新したり置き換えたりできます。また、必要に応じて新しい階層を挿入することもできます。</span><span class="sxs-lookup"><span data-stu-id="196ed-116">Tiers can be updated or replaced independently, and new tiers can be inserted if needed.</span></span>

<span data-ttu-id="196ed-117">_3 層_とは、3 つの階層がある n 層アプリケーションを指します。</span><span class="sxs-lookup"><span data-stu-id="196ed-117">_Three-tier_ refers to an n-tier application that has three tiers.</span></span> <span data-ttu-id="196ed-118">あなたの e コマース Web アプリケーションは、この 3 層アーキテクチャに従っています。</span><span class="sxs-lookup"><span data-stu-id="196ed-118">Your e-commerce web application follows this three-tier architecture:</span></span>

* <span data-ttu-id="196ed-119">**Web 層**は、ブラウザーを介して、ユーザーに Web インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="196ed-119">The **web tier** provides the web interface to your users through a browser.</span></span>
* <span data-ttu-id="196ed-120">**アプリケーション層**は、ビジネス ロジックを実行します。</span><span class="sxs-lookup"><span data-stu-id="196ed-120">The **application tier** runs business logic.</span></span>
* <span data-ttu-id="196ed-121">**データ層**には、製品情報と顧客の注文を保持するデータベースとその他のストレージが含まれます。</span><span class="sxs-lookup"><span data-stu-id="196ed-121">The **data tier** includes databases and other storage that hold product information and customer orders.</span></span>

<span data-ttu-id="196ed-122">ユーザーからデータ層への要求のフローを、次の図に示します。</span><span class="sxs-lookup"><span data-stu-id="196ed-122">The following illustration shows the flow of request from the user to the data tier.</span></span>

![各層が専用の仮想マシンでホストされている 3 層アーキテクチャを示す図。](../media/2-three-tier.png)

<span data-ttu-id="196ed-124">ユーザーが発注ボタンをクリックすると、その要求が Web 層に、ユーザーの住所と支払い情報と共に送信されます。</span><span class="sxs-lookup"><span data-stu-id="196ed-124">When the user clicks the button to place the order, the request is sent to the web tier, along with the user's address and payment information.</span></span> <span data-ttu-id="196ed-125">Web 層はこの情報をアプリケーション層に渡します。そこでは支払い情報の検証とインベントリのチェックが行われます。</span><span class="sxs-lookup"><span data-stu-id="196ed-125">The web tier passes this information to the application tier, which would validate payment information and check inventory.</span></span> <span data-ttu-id="196ed-126">アプリケーション層では、後で受注処理に使用するために、注文をデータ層に格納する場合があります。</span><span class="sxs-lookup"><span data-stu-id="196ed-126">The application tier might then store the order in the data tier, to be picked up later for fulfillment.</span></span>

## <a name="your-e-commerce-site-running-on-azure"></a><span data-ttu-id="196ed-127">Azure で実行されている e コマース サイト</span><span class="sxs-lookup"><span data-stu-id="196ed-127">Your e-commerce site running on Azure</span></span>

<span data-ttu-id="196ed-128">Azure には、コードをホストする完全に事前構成された環境から、自分で構成、カスタマイズ、管理を行う仮想マシンまで、Web アプリケーションをホストするためのさまざまな方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="196ed-128">Azure provides many different ways to host your web applications, from fully pre-configured environments that host your code, to virtual machines that you configure, customize, and manage.</span></span>

<span data-ttu-id="196ed-129">たとえば、e コマース サイトを仮想マシンで実行することを選択したとします。</span><span class="sxs-lookup"><span data-stu-id="196ed-129">Let's say you choose to run your e-commerce site on virtual machines.</span></span> <span data-ttu-id="196ed-130">Azure で実行されるテスト環境は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="196ed-130">Here's what that might look like in your test environment running on Azure.</span></span> <span data-ttu-id="196ed-131">受信要求を制限するセキュリティ機能を有効にした仮想マシン上で実行される 3 層アーキテクチャを、次の図に示します。</span><span class="sxs-lookup"><span data-stu-id="196ed-131">The following illustration shows a three-tier architecture running on virtual machines with security features enabled to restrict inbound requests.</span></span> 

![各層が個別の仮想マシンで実行されている 3 層アーキテクチャを示す図。](../media/2-test-deployment.png)

<span data-ttu-id="196ed-135">これを 1 つずつ見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="196ed-135">Let's break this down.</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="196ed-136">![地球の場所にピンが留められており、Azure リージョンを表しています](../media/2-azure-region.png)</span><span class="sxs-lookup"><span data-stu-id="196ed-136">![A pinned location on the Earth representing an Azure region](../media/2-azure-region.png)</span></span>
  :::column-end:::
    <span data-ttu-id="196ed-137">:::column span="3"::: **Azure リージョンとは?**</span><span class="sxs-lookup"><span data-stu-id="196ed-137">:::column span="3"::: **What's an Azure region?**</span></span>

<span data-ttu-id="196ed-138">_リージョン_とは、特定の地理的な場所内にある Azure データ センターのことです。</span><span class="sxs-lookup"><span data-stu-id="196ed-138">A _region_ is an Azure data center within a specific geographic location.</span></span> <span data-ttu-id="196ed-139">米国東部、米国西部、北ヨーロッパなどがリージョンの例です。</span><span class="sxs-lookup"><span data-stu-id="196ed-139">East US, West US, and North Europe are examples of regions.</span></span> <span data-ttu-id="196ed-140">このインスタンスでは、アプリケーションが米国東部リージョンで実行されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="196ed-140">In this instance, you see that the application is running in the East US region.</span></span>

  :::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="196ed-141">![仮想ネットワークで実行されている 2 つの仮想マシン](../media/2-azure-vnet.png)</span><span class="sxs-lookup"><span data-stu-id="196ed-141">![Two virtual machines running on a virtual network](../media/2-azure-vnet.png)</span></span>
  :::column-end:::
    <span data-ttu-id="196ed-142">:::column span="3"::: **仮想ネットワークとは?**</span><span class="sxs-lookup"><span data-stu-id="196ed-142">:::column span="3"::: **What's a virtual network?**</span></span>

<span data-ttu-id="196ed-143">_仮想ネットワーク_とは、Azure 上で論理的に分離されたネットワークのことです。</span><span class="sxs-lookup"><span data-stu-id="196ed-143">A _virtual network_ is a logically isolated network on Azure.</span></span> <span data-ttu-id="196ed-144">Hyper-V、VMware、または他のパブリック クラウドでネットワークを設定したことがあれば、Azure 仮想ネットワークにすぐに慣れることができます。</span><span class="sxs-lookup"><span data-stu-id="196ed-144">Azure virtual networks will be familiar to you if you've set up networks on Hyper-V, VMware, or even on other public clouds.</span></span>

<span data-ttu-id="196ed-145">Web 層、アプリケーション層、およびデータ層にはそれぞれ単一の VM があります。</span><span class="sxs-lookup"><span data-stu-id="196ed-145">The web, application, and data tiers each have a single VM.</span></span> <span data-ttu-id="196ed-146">各 VM は仮想ネットワークに属しています。</span><span class="sxs-lookup"><span data-stu-id="196ed-146">Each VM belongs to a virtual network.</span></span>

<span data-ttu-id="196ed-147">ユーザーは Web 層と直接やり取りするため、Web 層の VM にはパブリック IP アドレスがあります。</span><span class="sxs-lookup"><span data-stu-id="196ed-147">Users interact with the web tier directly, so that VM has a public IP address.</span></span> <span data-ttu-id="196ed-148">ユーザーがアプリケーション層とデータ層とやり取りすることはありません。</span><span class="sxs-lookup"><span data-stu-id="196ed-148">Users don't interact with the application or data tiers.</span></span> <span data-ttu-id="196ed-149">そのため、これらの VM にはそれぞれプライベート IP アドレスがあります。</span><span class="sxs-lookup"><span data-stu-id="196ed-149">So these VMs each have a private IP address.</span></span>

<span data-ttu-id="196ed-150">物理ハードウェアは、Azure データ センターで管理されます。</span><span class="sxs-lookup"><span data-stu-id="196ed-150">Azure data centers manage the physical hardware for you.</span></span> <span data-ttu-id="196ed-151">仮想ネットワークは、ソフトウェアを通じて構成します。これにより、仮想ネットワークを独自のネットワークと同じように扱うことができます。</span><span class="sxs-lookup"><span data-stu-id="196ed-151">You configure virtual networks through software, which enables you to treat a virtual network just like your own network.</span></span> <span data-ttu-id="196ed-152">たとえば、ネットワークでの IP アドレスの割り当てをより細かく制御するため、仮想ネットワークをサブネットに分割することができます。</span><span class="sxs-lookup"><span data-stu-id="196ed-152">For example, you can divide a virtual network into subnets to better control how the network assigns IP addresses.</span></span> <span data-ttu-id="196ed-153">また、仮想ネットワークが到達できる他のネットワークを選択することもできます。それがパブリック インターネットであっても、プライベート IP アドレス空間内の他のネットワークであってもかまいません。</span><span class="sxs-lookup"><span data-stu-id="196ed-153">You also choose which other networks your virtual network can reach, whether that's the public internet or other networks in the private IP address space.</span></span>

  :::column-end:::
:::row-end:::
:::row:::
  :::column:::
    <span data-ttu-id="196ed-154">![ネットワーク セキュリティ グループを共有する 2 つの仮想マシン](../media/2-azure-nsg.png)</span><span class="sxs-lookup"><span data-stu-id="196ed-154">![Two virtual machines with a shared network security group](../media/2-azure-nsg.png)</span></span>
  :::column-end:::
    <span data-ttu-id="196ed-155">:::column span="3"::: **ネットワーク セキュリティ グループとは?**</span><span class="sxs-lookup"><span data-stu-id="196ed-155">:::column span="3"::: **What's a network security group?**</span></span>

<span data-ttu-id="196ed-156">_ネットワーク セキュリティ グループ_ (NSG) は、Azure リソースへの着信ネットワーク トラフィックを許可または拒否します。</span><span class="sxs-lookup"><span data-stu-id="196ed-156">A _network security group_, or NSG, allows or denies inbound network traffic to your Azure resources.</span></span> <span data-ttu-id="196ed-157">ネットワーク セキュリティ グループを、自分のネットワークのクラウド レベルのファイアウォールとして考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="196ed-157">Think of a network security group as a cloud-level firewall for your network.</span></span>

<span data-ttu-id="196ed-158">たとえば、Web 層の VM で、ポート 22 (SSH) とポート 80 (HTTP) での着信トラフィックが許可されていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="196ed-158">For example, notice that the VM in the web tier allows inbound traffic on ports 22 (SSH) and 80 (HTTP).</span></span> <span data-ttu-id="196ed-159">この VM のネットワーク セキュリティ グループでは、すべてのソースからのこれらのポート経由の着信トラフィックが許可されます。</span><span class="sxs-lookup"><span data-stu-id="196ed-159">This VM's network security group allows inbound traffic over these ports from all sources.</span></span> <span data-ttu-id="196ed-160">信頼できる IP アドレスなど、既知のソースからのみトラフィックを受け入れるようにネットワーク セキュリティ グループを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="196ed-160">You can configure a network security group to accept traffic only from known sources, such as IP addresses that you trust.</span></span>

> [!NOTE]
> <span data-ttu-id="196ed-161">ポート 22 では、SSH 経由で Linux システムに直接接続することができます。</span><span class="sxs-lookup"><span data-stu-id="196ed-161">Port 22 enables you to connect directly to Linux systems over SSH.</span></span> <span data-ttu-id="196ed-162">ここでは、学習目的のために、ポート 22 を開いています。</span><span class="sxs-lookup"><span data-stu-id="196ed-162">Here we show port 22 open for learning purposes.</span></span> <span data-ttu-id="196ed-163">実際には、セキュリティを強化するために、仮想ネットワークに VPN アクセスを構成します。</span><span class="sxs-lookup"><span data-stu-id="196ed-163">In practice, you might configure VPN access to your virtual network to increase security.</span></span>

  :::column-end:::
:::row-end:::

## <a name="summary"></a><span data-ttu-id="196ed-164">まとめ</span><span class="sxs-lookup"><span data-stu-id="196ed-164">Summary</span></span>

<span data-ttu-id="196ed-165">あなたの 3 層アプリケーションは現在、米国東部リージョンの Azure で実行されています。</span><span class="sxs-lookup"><span data-stu-id="196ed-165">Your three-tier application is now running on Azure in the East US region.</span></span> <span data-ttu-id="196ed-166">_リージョン_とは、特定の地理的な場所内にある Azure データ センターのことです。</span><span class="sxs-lookup"><span data-stu-id="196ed-166">A _region_ is an Azure data center within a specific geographic location.</span></span>

<span data-ttu-id="196ed-167">各階層は、下の階層からのサービスにのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="196ed-167">Each tier can access services only from a lower tier.</span></span> <span data-ttu-id="196ed-168">Web 層で実行されている VM は、インターネットからトラフィックを受信するため、パブリック IP アドレスを持っています。</span><span class="sxs-lookup"><span data-stu-id="196ed-168">The VM running in the web tier has a public IP address because it receives traffic from the internet.</span></span> <span data-ttu-id="196ed-169">それより下の階層 (アプリケーション層とデータ層) の VM は、インターネット経由で直接通信することはないため、それぞれプライベート IP アドレスを持っています。</span><span class="sxs-lookup"><span data-stu-id="196ed-169">The VMs in the lower tiers, the application and data tiers, each have private IP addresses because they don't communicate directly over the internet.</span></span>

<span data-ttu-id="196ed-170">_仮想ネットワーク_を使用すると、関連するシステムをグループ化したり分離したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="196ed-170">_Virtual networks_ enable you to group and isolate related systems.</span></span> <span data-ttu-id="196ed-171">_ネットワーク セキュリティ グループ_を定義して、仮想ネットワーク内を通過できるトラフィックを制御します。</span><span class="sxs-lookup"><span data-stu-id="196ed-171">You define _network security groups_ to control what traffic can flow through a virtual network.</span></span>

<span data-ttu-id="196ed-172">ここで説明した構成は、すぐれた出発点となります。</span><span class="sxs-lookup"><span data-stu-id="196ed-172">The configuration you saw here is a good start.</span></span> <span data-ttu-id="196ed-173">しかし、自身の e コマース サイトをクラウドの運用環境にデプロイするときに、オンプレミスのデプロイで経験したのと同じ問題が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="196ed-173">But when you deploy your e-commerce site to production in the cloud, you'll likely run into the same problems as you did in your on-premises deployment.</span></span>
