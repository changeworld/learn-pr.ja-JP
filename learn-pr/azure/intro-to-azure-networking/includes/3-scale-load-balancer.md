<span data-ttu-id="38d90-101">現在あなたのサイトは、Azure で実行されています。</span><span class="sxs-lookup"><span data-stu-id="38d90-101">You now have your site up and running on Azure.</span></span> <span data-ttu-id="38d90-102">毎日 24 時間、サイトが確実に稼働するようにするにはどうすればよいですか?</span><span class="sxs-lookup"><span data-stu-id="38d90-102">How can you help ensure your site is running 24/7?</span></span>

<span data-ttu-id="38d90-103">たとえば、毎週のメンテナンスを行う必要がある場合は、どうなりますか?</span><span class="sxs-lookup"><span data-stu-id="38d90-103">For example, what happens when you need to do weekly maintenance?</span></span> <span data-ttu-id="38d90-104">メンテナンス期間中、サービスは使用できなくなります。</span><span class="sxs-lookup"><span data-stu-id="38d90-104">Your service will still be unavailable during your maintenance window.</span></span> <span data-ttu-id="38d90-105">また、あなたのサイトのユーザーは世界中にいるため、メンテナンスのためにシステムを停止するのに都合のよい時間帯はありません。</span><span class="sxs-lookup"><span data-stu-id="38d90-105">And because your site reaches users all over the world, there's no good time to take down your systems for maintenance.</span></span> <span data-ttu-id="38d90-106">同時に接続するユーザーが多すぎると、パフォーマンスの問題も発生します。</span><span class="sxs-lookup"><span data-stu-id="38d90-106">You may also run into performance issues if too many users connect at the same time.</span></span>

## <a name="what-are-availability-and-high-availability"></a><span data-ttu-id="38d90-107">耐久性と高可用性とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="38d90-107">What are availability and high availability?</span></span>

<span data-ttu-id="38d90-108">_可用性_とは、サービスが中断なく稼働する長さを指します。</span><span class="sxs-lookup"><span data-stu-id="38d90-108">_Availability_ refers to how long your service is up and running without interruption.</span></span> <span data-ttu-id="38d90-109">_高可用性_は _、_ 長期間稼働するサービスを指します。</span><span class="sxs-lookup"><span data-stu-id="38d90-109">_High availability_, or _highly available_, refers to a service that's up and running for a long period of time.</span></span>

<span data-ttu-id="38d90-110">あなたが毎日アクセスしているソーシャル メディアやニュース サイトを考えてみてください。</span><span class="sxs-lookup"><span data-stu-id="38d90-110">Think of a social media or news site that you visit daily.</span></span> <span data-ttu-id="38d90-111">そのサイトには常にアクセスできますか、それとも "503 サービス利用不可" などのエラー メッセージが頻繁に表示されますか?</span><span class="sxs-lookup"><span data-stu-id="38d90-111">Can you always access the site, or do you often see error messages like "503 Service Unavailable"?</span></span> <span data-ttu-id="38d90-112">必要な情報にアクセスできないことがどれほどいら立たしいことかはおわかりでしょう。</span><span class="sxs-lookup"><span data-stu-id="38d90-112">You know how frustrating it is when you can't access the information you need.</span></span>

<span data-ttu-id="38d90-113">"ファイブ ナインの可用性" などの言葉を聞いたことがあるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="38d90-113">You may have heard terms like "five nines availability".</span></span> <span data-ttu-id="38d90-114">ファイブ ナインの可用性とは、サービスが 99.999% の時間実行されることを保証することを意味しています。</span><span class="sxs-lookup"><span data-stu-id="38d90-114">Five nines availability means that the service is guaranteed to be running 99.999 percent of the time.</span></span> <span data-ttu-id="38d90-115">100% の可用性を実現するのは困難ですが、多くのチームが少なくともファイブ ナインの実現に努めています。</span><span class="sxs-lookup"><span data-stu-id="38d90-115">Although it's difficult to achieve 100 percent availability, many teams strive for at least five nines.</span></span>

## <a name="what-is-resiliency"></a><span data-ttu-id="38d90-116">回復性とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="38d90-116">What is resiliency?</span></span>

<span data-ttu-id="38d90-117">_回復性_は、異常な状態でシステムが機能を維持できる能力を指します。</span><span class="sxs-lookup"><span data-stu-id="38d90-117">_Resiliency_ refers to a system's ability to stay operational during abnormal conditions.</span></span>

<span data-ttu-id="38d90-118">異常な状態には次のようなものがあります。</span><span class="sxs-lookup"><span data-stu-id="38d90-118">These conditions include:</span></span>

- <span data-ttu-id="38d90-119">自然災害</span><span class="sxs-lookup"><span data-stu-id="38d90-119">Natural disasters</span></span>
- <span data-ttu-id="38d90-120">ソフトウェアの更新やセキュリティ パッチの適用を含む、計画的および計画外のシステム メンテナンス</span><span class="sxs-lookup"><span data-stu-id="38d90-120">System maintenance, both planned and unplanned, which include software updates and security patches</span></span>
- <span data-ttu-id="38d90-121">サイトへのトラフィックの急増</span><span class="sxs-lookup"><span data-stu-id="38d90-121">Spikes in traffic to your site</span></span>
- <span data-ttu-id="38d90-122">分散型サービス拒否 (DDoS) 攻撃など、悪意のある第三者による脅威</span><span class="sxs-lookup"><span data-stu-id="38d90-122">Threats made by malicious parties, such as distributed denial of service, or DDoS, attacks</span></span>

<span data-ttu-id="38d90-123">あなたが所属するマーケティング チームでは、新製品の販売促進のためのフラッシュ セールを計画しているとします。</span><span class="sxs-lookup"><span data-stu-id="38d90-123">Image your marketing team wants to have a flash sale to promote a line of new products.</span></span> <span data-ttu-id="38d90-124">あなたはこの期間にトラフィックが急増することを予想しています。</span><span class="sxs-lookup"><span data-stu-id="38d90-124">You might expect a huge spike in traffic during this time.</span></span> <span data-ttu-id="38d90-125">この急増がシステムの処理能力を上回ると、システムが遅くなったり停止したりして、ユーザーを失望させる恐れがあります。</span><span class="sxs-lookup"><span data-stu-id="38d90-125">This spike could overwhelm your processing system, causing it to slow down or halt, disappointing your users.</span></span> <span data-ttu-id="38d90-126">あなた自分、こうした失望を味わった経験があるでしょう。</span><span class="sxs-lookup"><span data-stu-id="38d90-126">You may have experienced this disappointment for yourself.</span></span> <span data-ttu-id="38d90-127">イベントのチケットが欲しかったのに、Web サイトが応答しなかったことはありますか?</span><span class="sxs-lookup"><span data-stu-id="38d90-127">Have you ever wanted tickets for an event only to find the web site wasn't responding?</span></span>

## <a name="what-is-a-load-balancer"></a><span data-ttu-id="38d90-128">ロード バランサーとは何ですか?</span><span class="sxs-lookup"><span data-stu-id="38d90-128">What is a load balancer?</span></span>

<span data-ttu-id="38d90-129">_ロード バランサー_は、プール内の各システムにトラフィックを均等に分散させます。</span><span class="sxs-lookup"><span data-stu-id="38d90-129">A _load balancer_ distributes traffic evenly among each system in a pool.</span></span> <span data-ttu-id="38d90-130">ロード バランサーは、高可用性と回復性の両方を実現するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="38d90-130">A load balancer can help you achieve both high availability and resiliency.</span></span>

<span data-ttu-id="38d90-131">たとえば、それぞれ同じように構成された VM を各階層に追加することから始めるとします。</span><span class="sxs-lookup"><span data-stu-id="38d90-131">Say you start by adding additional VMs, each configured identically, to each tier.</span></span> <span data-ttu-id="38d90-132">この考えは、1 つがダウンした場合、または同時にサービスを提供するユーザーの数が多すぎる場合に、追加のシステムを準備することです。</span><span class="sxs-lookup"><span data-stu-id="38d90-132">The idea is to have additional systems ready in case one goes down or is serving too many users at the same time.</span></span>

<span data-ttu-id="38d90-133">ここでの問題は、VM ごとに独自の IP アドレスがあることです。</span><span class="sxs-lookup"><span data-stu-id="38d90-133">The problem here is that each VM would have its own IP address.</span></span> <span data-ttu-id="38d90-134">さらに、1 つのシステムがダウンまたはビジー状態になったときに、トラフィックを分散させる方法がないことです。</span><span class="sxs-lookup"><span data-stu-id="38d90-134">Plus, you don't have a way to distribute traffic in case one system goes down or is busy.</span></span> <span data-ttu-id="38d90-135">ユーザーからは 1 つのシステムと見えるように VM を接続するにはどうすればよいですか?</span><span class="sxs-lookup"><span data-stu-id="38d90-135">How do you connect your VMs so that they appear to the user as one system?</span></span>

<span data-ttu-id="38d90-136">答えは、ロード バランサーを使用してトラフィックを分散させることです。</span><span class="sxs-lookup"><span data-stu-id="38d90-136">The answer is to use a load balancer to distribute traffic.</span></span> <span data-ttu-id="38d90-137">ロード バランサーは、ユーザーへのエントリ ポイントになります。</span><span class="sxs-lookup"><span data-stu-id="38d90-137">The load balancer becomes the entry point to the user.</span></span> <span data-ttu-id="38d90-138">ロード バランサーが要求を受信するのにどのシステムを選ぶかは、ユーザーにはわかりません (知る必要はありません)。</span><span class="sxs-lookup"><span data-stu-id="38d90-138">The user doesn't know (or need to know) which system the load balancer chooses to receive the request.</span></span>

<span data-ttu-id="38d90-139">ダイアグラムを次に示します。</span><span class="sxs-lookup"><span data-stu-id="38d90-139">Here's a diagram.</span></span>

![仮想マシン間で負荷を分散する](../media-draft/load-balancer.png)

<span data-ttu-id="38d90-141">ロード バランサーが、ユーザーの要求を受信しているのがわかります。</span><span class="sxs-lookup"><span data-stu-id="38d90-141">You see that the load balancer receives the user's request.</span></span> <span data-ttu-id="38d90-142">ロード バランサーは、要求を Web 層内のいずれかの VM に要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="38d90-142">The load balancer directs the request to one of the VMs in the web tier.</span></span> <span data-ttu-id="38d90-143">VM が使用できない、または応答を停止している場合は、ロード バランサーはその VM へのトラフィックの送信を停止します。</span><span class="sxs-lookup"><span data-stu-id="38d90-143">If a VM is unavailable or stops responding, the load balancer stops sending traffic to it.</span></span> <span data-ttu-id="38d90-144">次にロード バランサーは、応答しているサーバーのいずれかにトラフィックを送信します。</span><span class="sxs-lookup"><span data-stu-id="38d90-144">The load balancer then directs traffic to one of the responsive servers.</span></span>

<span data-ttu-id="38d90-145">負荷分散することで、サービスを中断せずにメンテナンス タスクを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="38d90-145">Load balancing enables you to run maintenance tasks without interrupting service.</span></span> <span data-ttu-id="38d90-146">たとえば、メンテナンス期間が重ならないように VM ごとに調整することができます。</span><span class="sxs-lookup"><span data-stu-id="38d90-146">For example, you can stagger the maintenance window for each VM.</span></span> <span data-ttu-id="38d90-147">メンテナンス期間中は、ロード バランサーが VM が応答していないことを検出して、プール内の他の VM にトラフィックを送信します。</span><span class="sxs-lookup"><span data-stu-id="38d90-147">During the maintenance window, the load balancer detects that the VM is unresponsive and directs traffic to other VMs in the pool.</span></span>

<span data-ttu-id="38d90-148">あなたの e コマース サイトの場合、アプリ層とデータ層にもロード バランサーを配置することができます。</span><span class="sxs-lookup"><span data-stu-id="38d90-148">For your e-commerce site, the app and data tiers can also have a load balancer.</span></span> <span data-ttu-id="38d90-149">これは提供するサービスの必要性に応じて決まります。</span><span class="sxs-lookup"><span data-stu-id="38d90-149">It all depends on what your service requires.</span></span>

## <a name="what-is-azure-load-balancer"></a><span data-ttu-id="38d90-150">Azure Load Balancer とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="38d90-150">What is Azure Load Balancer?</span></span>

<span data-ttu-id="38d90-151">Azure Load Balancer は、Microsoft が提供するロード バランサー サービスです。</span><span class="sxs-lookup"><span data-stu-id="38d90-151">Azure Load Balancer is a load balancer service that Microsoft provides.</span></span>

<span data-ttu-id="38d90-152">仮想マシンにロード バランサー ソフトウェアを手動で構成できます。</span><span class="sxs-lookup"><span data-stu-id="38d90-152">You can manually configure load balancer software on a virtual machine.</span></span> <span data-ttu-id="38d90-153">欠点は、メンテナンスする必要があるシステムが増えることです。</span><span class="sxs-lookup"><span data-stu-id="38d90-153">The downside is that you now have an additional system that you need to maintain.</span></span> <span data-ttu-id="38d90-154">ロード バランサーがダウンしたり定期的なメンテナンスが必要になったりすると、元の問題に戻ります。</span><span class="sxs-lookup"><span data-stu-id="38d90-154">If your load balancer goes down or needs routine maintenance, you're back to your original problem.</span></span>

<span data-ttu-id="38d90-155">代わりに、Azure Load Balancer を使用することができます。Azure によりインフラストラクチャやソフトウェアのメンテナンスが行われるため、自分で行う必要がないためです。</span><span class="sxs-lookup"><span data-stu-id="38d90-155">Instead, you can use Azure Load Balancer because there's no infrastructure or software for you to maintain; Azure takes care of maintenance for you.</span></span>

<span data-ttu-id="38d90-156">各階層に複数の VM を示す図を次に示します。</span><span class="sxs-lookup"><span data-stu-id="38d90-156">Here's a diagram that shows several VMs in each tier.</span></span> <span data-ttu-id="38d90-157">各階層には、トラフィックをプール内の VM 間に分散させる Azure Load Balancer が含まれています。</span><span class="sxs-lookup"><span data-stu-id="38d90-157">Each tier includes an Azure Load Balancer that distributes traffic among the VMs in the pool.</span></span>

![Azure Load Balancer を使用して仮想マシン間で負荷を分散する](../media-draft/azure-load-balancer.png)

## <a name="what-about-dns"></a><span data-ttu-id="38d90-159">DNS とは何ですか?</span><span class="sxs-lookup"><span data-stu-id="38d90-159">What about DNS?</span></span>

<span data-ttu-id="38d90-160">ドメイン ネーム システム (DNS) は、わかりやすい名前をその IP アドレスにマップする方法です。</span><span class="sxs-lookup"><span data-stu-id="38d90-160">DNS, or Domain Name System, is a way to map user-friendly names to their IP addresses.</span></span> <span data-ttu-id="38d90-161">DNS は、インターネットの電話帳として考えることができます。</span><span class="sxs-lookup"><span data-stu-id="38d90-161">You can think of DNS as the phonebook of the Internet.</span></span>

<span data-ttu-id="38d90-162">たとえば、ドメイン名 contoso.com を Web 層のロード バランサーの IP アドレス 40.65.106.192 にマップできます。</span><span class="sxs-lookup"><span data-stu-id="38d90-162">For example, your domain name, contoso.com, might map to the IP address of the load balancer at the web tier, 40.65.106.192.</span></span>

<span data-ttu-id="38d90-163">独自の DNS サーバーを導入することも、Azure DNS (Azure インフラストラクチャで実行される DNS ドメインのホスティング サービス) を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="38d90-163">You can bring your own DNS server or use Azure DNS, a hosting service for DNS domains that runs on Azure infrastructure.</span></span>

<span data-ttu-id="38d90-164">Azure DNS を示す図を次に示します。</span><span class="sxs-lookup"><span data-stu-id="38d90-164">Here's a diagram that shows Azure DNS.</span></span> <span data-ttu-id="38d90-165">ユーザーが contoso.com に移動すると、Azure DNS によってトラフィックがロード バランサーにルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="38d90-165">When the user navigates to contoso.com, Azure DNS routes traffic to the load balancer.</span></span>

![Azure DNS を使用して DNS 名を割り当てる](../media-draft/dns.png)

## <a name="summary"></a><span data-ttu-id="38d90-167">まとめ</span><span class="sxs-lookup"><span data-stu-id="38d90-167">Summary</span></span>

<span data-ttu-id="38d90-168">負荷分散を配置することで、あなたの e コマース サイトの可用性と回復力がさらに向上しました。</span><span class="sxs-lookup"><span data-stu-id="38d90-168">With load balancing in place, your e-commerce site is now more highly available and resilient.</span></span> <span data-ttu-id="38d90-169">メンテナンスを実行したり、トラフィックが増加すると、ロード バランサーによってトラフィックを別の使用可能なシステムに分散させることができます。</span><span class="sxs-lookup"><span data-stu-id="38d90-169">When you perform maintenance or receive an uptick in traffic, your load balancer can distribute traffic to another available system.</span></span>

<span data-ttu-id="38d90-170">VM に独自のロード バランサーを構成することもできますが、**Azure Load Balancer** を使用すると、メンテナンスするインフラストラクチャやソフトウェアがないため、維持費を削減できます。</span><span class="sxs-lookup"><span data-stu-id="38d90-170">Although you can configure your own load balancer on a VM, **Azure Load Balancer** reduces upkeep because there's no infrastructure or software to maintain.</span></span>

<span data-ttu-id="38d90-171">DNS は、わかりやすい名前をその IP アドレスにマップします。これは、電話帳で人名または企業名を電話番号にマップする方法とよく似ています。</span><span class="sxs-lookup"><span data-stu-id="38d90-171">DNS maps user-friendly names to their IP addresses, much like how a phonebook maps names of people or businesses to phone numbers.</span></span> <span data-ttu-id="38d90-172">独自の DNS サーバーを導入することも、Azure DNS を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="38d90-172">You can bring your own DNS server or use Azure DNS.</span></span>