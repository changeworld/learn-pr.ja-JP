<span data-ttu-id="cff1a-101">現在あなたのサイトは、Azure で実行されています。</span><span class="sxs-lookup"><span data-stu-id="cff1a-101">You now have your site up and running on Azure.</span></span> <span data-ttu-id="cff1a-102">しかし、毎日 24 時間、サイトが確実に稼働するようにするにはどうすればよいですか?</span><span class="sxs-lookup"><span data-stu-id="cff1a-102">But how can you help ensure your site is running 24/7?</span></span>

<span data-ttu-id="cff1a-103">たとえば、毎週のメンテナンスを行う必要がある場合は、どうなりますか?</span><span class="sxs-lookup"><span data-stu-id="cff1a-103">For instance, what happens when you need to do weekly maintenance?</span></span> <span data-ttu-id="cff1a-104">メンテナンス期間中、サービスは使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-104">Your service will still be unavailable during your maintenance window.</span></span> <span data-ttu-id="cff1a-105">また、あなたのサイトのユーザーは世界中にいるため、メンテナンスのためにシステムを停止するのに都合のよい時間帯はありません。</span><span class="sxs-lookup"><span data-stu-id="cff1a-105">And because your site reaches users all over the world, there's no good time to take down your systems for maintenance.</span></span> <span data-ttu-id="cff1a-106">同時に接続するユーザーが多すぎると、パフォーマンスの問題も発生します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-106">You may also run into performance issues if too many users connect at the same time.</span></span>

## <a name="what-are-availability-and-high-availability"></a><span data-ttu-id="cff1a-107">耐久性と高可用性とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="cff1a-107">What are availability and high availability?</span></span>

:::row:::
  :::column:::
    ![高可用性を表す高速列車](../media/3-availability.png)
  :::column-end:::
    :::column span="3":::
<span data-ttu-id="cff1a-109">_可用性_とは、サービスが中断なく稼働する長さを指します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-109">_Availability_ refers to how long your service is up and running without interruption.</span></span> <span data-ttu-id="cff1a-110">"_高可用性_" または "_可用性が高い_" とは、長期間稼働するサービスを指します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-110">_High availability_, or _highly available_, refers to a service that's up and running for a long period of time.</span></span>

<span data-ttu-id="cff1a-111">必要な情報にアクセスできないことがどれほどいら立たしいことかはおわかりでしょう。</span><span class="sxs-lookup"><span data-stu-id="cff1a-111">You know how frustrating it is when you can't access the information you need.</span></span> <span data-ttu-id="cff1a-112">あなたが毎日アクセスしているソーシャル メディアやニュース サイトを考えてみてください。</span><span class="sxs-lookup"><span data-stu-id="cff1a-112">Think of a social media or news site that you visit daily.</span></span> <span data-ttu-id="cff1a-113">そのサイトには常にアクセスできますか、それとも "503 サービス利用不可" などのエラー メッセージが頻繁に表示されますか?</span><span class="sxs-lookup"><span data-stu-id="cff1a-113">Can you always access the site, or do you often see error messages like "503 Service Unavailable"?</span></span>
  :::column-end:::
 :::row-end:::

<span data-ttu-id="cff1a-114">"ファイブ ナインの可用性" などの言葉を聞いたことがあるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="cff1a-114">You may have heard terms like "five nines availability."</span></span> <span data-ttu-id="cff1a-115">ファイブ ナインの可用性とは、サービスが 99.999% の時間実行されることを保証することを意味しています。</span><span class="sxs-lookup"><span data-stu-id="cff1a-115">Five nines availability means that the service is guaranteed to be running 99.999 percent of the time.</span></span> <span data-ttu-id="cff1a-116">100% の可用性を実現するのは困難ですが、多くのチームが少なくともファイブ ナインの実現に努めています。</span><span class="sxs-lookup"><span data-stu-id="cff1a-116">Although it's difficult to achieve 100 percent availability, many teams strive for at least five nines.</span></span>

## <a name="what-is-resiliency"></a><span data-ttu-id="cff1a-117">回復性とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="cff1a-117">What is resiliency?</span></span>

:::row:::
  :::column:::
    ![回復性を表す正常性グラフ](../media/3-resiliency.png)
  :::column-end:::
    :::column span="3":::
<span data-ttu-id="cff1a-119">_回復性_は、異常な状態でシステムが機能を維持できる能力を指します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-119">_Resiliency_ refers to a system's ability to stay operational during abnormal conditions.</span></span>

<span data-ttu-id="cff1a-120">異常な状態には次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-120">These conditions include:</span></span>

- <span data-ttu-id="cff1a-121">自然災害。</span><span class="sxs-lookup"><span data-stu-id="cff1a-121">Natural disasters.</span></span>
- <span data-ttu-id="cff1a-122">ソフトウェアの更新やセキュリティ パッチの適用を含む、計画的および計画外のシステム メンテナンス。</span><span class="sxs-lookup"><span data-stu-id="cff1a-122">System maintenance, both planned and unplanned, including software updates and security patches.</span></span>
- <span data-ttu-id="cff1a-123">サイトへのトラフィックの急増。</span><span class="sxs-lookup"><span data-stu-id="cff1a-123">Spikes in traffic to your site.</span></span>
- <span data-ttu-id="cff1a-124">分散型サービス拒否 (DDoS) 攻撃など、悪意のある第三者による脅威。</span><span class="sxs-lookup"><span data-stu-id="cff1a-124">Threats made by malicious parties, such as distributed denial of service, or DDoS, attacks.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="cff1a-125">あなたが所属するマーケティング チームでは、ビタミン サプリメントの新製品の販売促進のためのフラッシュ セールを計画しているとします。</span><span class="sxs-lookup"><span data-stu-id="cff1a-125">Imagine your marketing team wants to have a flash sale to promote a new line of vitamin supplements.</span></span> <span data-ttu-id="cff1a-126">あなたはこの期間にトラフィックが急増することを予想しています。</span><span class="sxs-lookup"><span data-stu-id="cff1a-126">You might expect a huge spike in traffic during this time.</span></span> <span data-ttu-id="cff1a-127">この急増がシステムの処理能力を上回ると、システムが遅くなったり停止したりして、ユーザーを失望させる恐れがあります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-127">This spike could overwhelm your processing system, causing it to slow down or halt, disappointing your users.</span></span> <span data-ttu-id="cff1a-128">あなた自身、こうした失望を味わった経験があるでしょう。</span><span class="sxs-lookup"><span data-stu-id="cff1a-128">You may have experienced this disappointment for yourself.</span></span> <span data-ttu-id="cff1a-129">オンライン ショップにアクセスしようとしたのに Web サイトが応答しなかったことはありませんか。</span><span class="sxs-lookup"><span data-stu-id="cff1a-129">Have you ever tried to access an online sale only to find the website wasn't responding?</span></span>

## <a name="what-is-a-load-balancer"></a><span data-ttu-id="cff1a-130">ロード バランサーとは</span><span class="sxs-lookup"><span data-stu-id="cff1a-130">What is a load balancer?</span></span>

:::row:::
  :::column:::
    ![負荷分散を表すスケール](../media/3-lb.png)
  :::column-end:::
    :::column span="3":::
<span data-ttu-id="cff1a-132">_ロード バランサー_は、プール内の各システムにトラフィックを均等に分散させます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-132">A _load balancer_ distributes traffic evenly among each system in a pool.</span></span> <span data-ttu-id="cff1a-133">ロード バランサーは、高可用性と回復性の両方を実現するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-133">A load balancer can help you achieve both high availability and resiliency.</span></span>

<span data-ttu-id="cff1a-134">たとえば、それぞれ同じように構成された VM を各階層に追加することから始めるとします。</span><span class="sxs-lookup"><span data-stu-id="cff1a-134">Say you start by adding additional VMs, each configured identically, to each tier.</span></span> <span data-ttu-id="cff1a-135">この考えは、1 つがダウンした場合、または同時にサービスを提供するユーザーの数が多すぎる場合に、追加のシステムを準備することです。</span><span class="sxs-lookup"><span data-stu-id="cff1a-135">The idea is to have additional systems ready, in case one goes down or is serving too many users at the same time.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="cff1a-136">ここでの問題は、VM ごとに独自の IP アドレスがあることです。</span><span class="sxs-lookup"><span data-stu-id="cff1a-136">The problem here is that each VM would have its own IP address.</span></span> <span data-ttu-id="cff1a-137">さらに、1 つのシステムがダウンまたはビジー状態になったときに、トラフィックを分散させる方法がないことです。</span><span class="sxs-lookup"><span data-stu-id="cff1a-137">Plus, you don't have a way to distribute traffic in case one system goes down or is busy.</span></span> <span data-ttu-id="cff1a-138">ユーザーからは 1 つのシステムと見えるように VM を接続するにはどうすればよいですか?</span><span class="sxs-lookup"><span data-stu-id="cff1a-138">How do you connect your VMs so that they appear to the user as one system?</span></span>

<span data-ttu-id="cff1a-139">答えは、ロード バランサーを使用してトラフィックを分散させることです。</span><span class="sxs-lookup"><span data-stu-id="cff1a-139">The answer is to use a load balancer to distribute traffic.</span></span> <span data-ttu-id="cff1a-140">ロード バランサーは、ユーザーへのエントリ ポイントになります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-140">The load balancer becomes the entry point to the user.</span></span> <span data-ttu-id="cff1a-141">ロード バランサーが要求を受信するのにどのシステムを選ぶかは、ユーザーにはわかりません (知る必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="cff1a-141">The user doesn't know (or need to know) which system the load balancer chooses to receive the request.</span></span>

<span data-ttu-id="cff1a-142">次の図は、ロード バランサーの役割を示したものです。</span><span class="sxs-lookup"><span data-stu-id="cff1a-142">The following illustration shows the role of a load balancer.</span></span>

![3 層アーキテクチャの Web 層を示す図。](../media/3-load-balancer.png)

<span data-ttu-id="cff1a-146">ロード バランサーが、ユーザーの要求を受信しているのがわかります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-146">You see that the load balancer receives the user's request.</span></span> <span data-ttu-id="cff1a-147">ロード バランサーは、要求を Web 層内のいずれかの VM に要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-147">The load balancer directs the request to one of the VMs in the web tier.</span></span> <span data-ttu-id="cff1a-148">VM が使用できない、または応答を停止している場合は、ロード バランサーはその VM へのトラフィックの送信を停止します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-148">If a VM is unavailable or stops responding, the load balancer stops sending traffic to it.</span></span> <span data-ttu-id="cff1a-149">次にロード バランサーは、応答しているサーバーのいずれかにトラフィックを送信します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-149">The load balancer then directs traffic to one of the responsive servers.</span></span>

<span data-ttu-id="cff1a-150">負荷分散することで、サービスを中断せずにメンテナンス タスクを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-150">Load balancing enables you to run maintenance tasks without interrupting service.</span></span> <span data-ttu-id="cff1a-151">たとえば、メンテナンス期間が重ならないように VM ごとに調整することができます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-151">For example, you can stagger the maintenance window for each VM.</span></span> <span data-ttu-id="cff1a-152">メンテナンス期間中は、ロード バランサーが VM が応答していないことを検出して、プール内の他の VM にトラフィックを送信します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-152">During the maintenance window, the load balancer detects that the VM is unresponsive, and directs traffic to other VMs in the pool.</span></span>

<span data-ttu-id="cff1a-153">この eコマース サイトの場合、アプリ層とデータ層にもロード バランサーを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-153">For your e-commerce site, the app and data tiers can also have a load balancer.</span></span> <span data-ttu-id="cff1a-154">これは提供するサービスの必要性に応じて決まります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-154">It all depends on what your service requires.</span></span>

## <a name="what-is-azure-load-balancer"></a><span data-ttu-id="cff1a-155">Azure Load Balancer とは</span><span class="sxs-lookup"><span data-stu-id="cff1a-155">What is Azure Load Balancer?</span></span>

<span data-ttu-id="cff1a-156">Azure Load Balancer は Microsoft が提供するロード バランサー サービスで、ユーザーのためにメンテナンスの面倒を見ます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-156">Azure Load Balancer is a load balancer service that Microsoft provides that helps take care of the maintenance for you.</span></span>

<span data-ttu-id="cff1a-157">ユーザーが仮想マシンに一般的なロード バランサー ソフトウェアを手動で構成すると、メンテナンスの必要なシステムが増えるという欠点があります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-157">When you manually configure typical load balancer software on a virtual machine, there's a downside: you now have an additional system that you need to maintain.</span></span> <span data-ttu-id="cff1a-158">ロード バランサーがダウンしたり定期的なメンテナンスが必要になったりすると、元の問題に戻ります。</span><span class="sxs-lookup"><span data-stu-id="cff1a-158">If your load balancer goes down or needs routine maintenance, you're back to your original problem.</span></span>

<span data-ttu-id="cff1a-159">しかし、代わりに Azure Load Balancer を使用すると、自分でメンテナンスする必要のあるインフラストラクチャやソフトウェアはありません。</span><span class="sxs-lookup"><span data-stu-id="cff1a-159">If instead, however, you use Azure Load Balancer, there's no infrastructure or software for you to maintain.</span></span>

<span data-ttu-id="cff1a-160">次の図は、多層アーキテクチャでの Azure ロード バランサーの役割です。</span><span class="sxs-lookup"><span data-stu-id="cff1a-160">The following illustration shows  the role of Azure load balancers in a multi-tier architecture.</span></span>

![3 層アーキテクチャの Web 層を示す図。](../media/3-azure-load-balancer.png)

## <a name="what-about-dns"></a><span data-ttu-id="cff1a-164">DNS とは</span><span class="sxs-lookup"><span data-stu-id="cff1a-164">What about DNS?</span></span>

:::row:::
  :::column:::
    ![DNS を表す地図上のピン](../media/3-map-pin.png)
  :::column-end:::
    :::column span="3":::
<span data-ttu-id="cff1a-166">ドメイン ネーム システム (DNS) は、わかりやすい名前をその IP アドレスにマップする方法です。</span><span class="sxs-lookup"><span data-stu-id="cff1a-166">DNS, or Domain Name System, is a way to map user-friendly names to their IP addresses.</span></span> <span data-ttu-id="cff1a-167">DNS は、インターネットの電話帳として考えることができます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-167">You can think of DNS as the phonebook of the internet.</span></span>

<span data-ttu-id="cff1a-168">たとえば、ドメイン名 contoso.com を Web 層のロード バランサーの IP アドレス 40.65.106.192 にマップできます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-168">For example, your domain name, contoso.com, might map to the IP address of the load balancer at the web tier, 40.65.106.192.</span></span>

<span data-ttu-id="cff1a-169">独自の DNS サーバーを導入することも、Azure DNS (Azure インフラストラクチャで実行される DNS ドメインのホスティング サービス) を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-169">You can bring your own DNS server or use Azure DNS, a hosting service for DNS domains that runs on Azure infrastructure.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="cff1a-170">Azure DNS の図を次に示します。</span><span class="sxs-lookup"><span data-stu-id="cff1a-170">The following illustration shows Azure DNS.</span></span> <span data-ttu-id="cff1a-171">ユーザーが contoso.com に移動すると、Azure DNS によってトラフィックがロード バランサーにルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-171">When the user navigates to contoso.com, Azure DNS routes traffic to the load balancer.</span></span>

![ロード バランサーの前に配置された Azure ドメイン ネーム システムを示す図。](../media/3-dns.png)

## <a name="summary"></a><span data-ttu-id="cff1a-173">まとめ</span><span class="sxs-lookup"><span data-stu-id="cff1a-173">Summary</span></span>

<span data-ttu-id="cff1a-174">負荷分散を配置することで、あなたの e コマース サイトの可用性と回復力がさらに向上しました。</span><span class="sxs-lookup"><span data-stu-id="cff1a-174">With load balancing in place, your e-commerce site is now more highly available and resilient.</span></span> <span data-ttu-id="cff1a-175">メンテナンスを実行したり、トラフィックが増加すると、ロード バランサーによってトラフィックを別の使用可能なシステムに分散させることができます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-175">When you perform maintenance or receive an uptick in traffic, your load balancer can distribute traffic to another available system.</span></span>

<span data-ttu-id="cff1a-176">VM に独自のロード バランサーを構成することもできますが、Azure Load Balancer を使用すると、メンテナンスするインフラストラクチャやソフトウェアがないため、維持費を削減できます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-176">Although you can configure your own load balancer on a VM, Azure Load Balancer reduces upkeep because there's no infrastructure or software to maintain.</span></span>

<span data-ttu-id="cff1a-177">DNS は、わかりやすい名前をその IP アドレスにマップします。これは、電話帳で人名または企業名を電話番号にマップする方法とよく似ています。</span><span class="sxs-lookup"><span data-stu-id="cff1a-177">DNS maps user-friendly names to their IP addresses, much like how a phonebook maps names of people or businesses to phone numbers.</span></span> <span data-ttu-id="cff1a-178">独自の DNS サーバーを導入することも、Azure DNS を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="cff1a-178">You can bring your own DNS server, or use Azure DNS.</span></span>
