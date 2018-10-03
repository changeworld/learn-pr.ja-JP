<span data-ttu-id="f28bc-101">利用できる Azure コンピューティング サービスの概要がわかったので、各サービスをどのようなときに使用すればよいか決められるように各サービスを詳しく見ていきましょう。</span><span class="sxs-lookup"><span data-stu-id="f28bc-101">Now that you've been introduced to the Azure compute services available, let's look at each more closely to help you decide when to use each service.</span></span>

## <a name="azure-virtual-machines"></a><span data-ttu-id="f28bc-102">Azure 仮想マシン</span><span class="sxs-lookup"><span data-stu-id="f28bc-102">Azure virtual machines</span></span>

:::row:::
  :::column:::
    ![Azure 仮想マシンを表すイメージ](../media/3-azure-vms.png)
  :::column-end:::
  :::column span="3":::
<span data-ttu-id="f28bc-104">オペレーティング システムと環境を完全に制御する必要がある場合は、仮想マシンが理想的な選択肢です。</span><span class="sxs-lookup"><span data-stu-id="f28bc-104">When you need total control over the operating system and environment, virtual machines are an ideal choice.</span></span> <span data-ttu-id="f28bc-105">物理コンピューターと同じように、VM で実行されているすべてのソフトウェアをカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-105">You're able to customize all of the software running on the VM, just like a physical computer.</span></span> <span data-ttu-id="f28bc-106">これは特に、カスタム ソフトウェアまたはカスタム ホスト構成を実行するときに適しています。</span><span class="sxs-lookup"><span data-stu-id="f28bc-106">This is especially ideal when running custom software or custom hosting configurations.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="f28bc-107">物理サーバーからクラウドに移行するときも、VM が優れた選択肢です。</span><span class="sxs-lookup"><span data-stu-id="f28bc-107">VMs are also an excellent choice when moving from a physical server to the cloud.</span></span> <span data-ttu-id="f28bc-108">多くの場合、物理サーバーのイメージを作成し、仮想マシン内でそれをホストできます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-108">You can often create an image of the physical server and host it within a virtual machine.</span></span> <span data-ttu-id="f28bc-109">これにより、オペレーティング システムとアプリケーション実行環境を完全に自由に選択でき、好みのツールを使用するほぼすべての言語で開発することができます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-109">This gives you complete freedom to choose operating systems and application execution environments, meaning you can develop in almost any language that uses the tools of your choice.</span></span>

<span data-ttu-id="f28bc-110">ただし、仮想マシンを維持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f28bc-110">However, you'll be required to maintain the virtual machine.</span></span> <span data-ttu-id="f28bc-111">これは、オペレーティング システムとそこで実行されているソフトウェアを更新することを意味します。</span><span class="sxs-lookup"><span data-stu-id="f28bc-111">This means updating the operating system and the software it runs.</span></span> 

<span data-ttu-id="f28bc-112">また、スケーリングするときもいっそうの考慮が必要です。</span><span class="sxs-lookup"><span data-stu-id="f28bc-112">It also requires more consideration when scaling.</span></span> <span data-ttu-id="f28bc-113">仮想マシンをスケールアップできます。つまり、コンピューティング リソースとメモリ リソースを追加できます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-113">You can scale up the virtual machine, meaning you can add more compute and memory resources.</span></span> <span data-ttu-id="f28bc-114">ただし、複数のインスタンスを並列で実行したり、スケールアウトしたりする必要がある場合、ロード バランサーのようなサービスを追加する必要があることがあります。</span><span class="sxs-lookup"><span data-stu-id="f28bc-114">But if you need to run multiple instances in parallel, or scale out, you may need to add additional services, such as load balancers.</span></span>

<span data-ttu-id="f28bc-115">そこで、処理する必要がある天文学の画像を科学者がアップロードできるようにする Web サイトを運用していることを想像してください。</span><span class="sxs-lookup"><span data-stu-id="f28bc-115">So, imagine you're running a web site that enables scientists to upload astronomy images that need to be processed.</span></span> <span data-ttu-id="f28bc-116">VM を複製した場合、複数の Web サイト VM インスタンス間で要求をルーティングするサービスを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f28bc-116">If you duplicated the VM, you'd need an additional service to route requests between multiple instances of the web site VM.</span></span>

<span data-ttu-id="f28bc-117">Azure では高度な仮想マシン サービスも利用できます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-117">There are also advanced virtual machine services available in Azure:</span></span>

- <span data-ttu-id="f28bc-118">同じように構成された VM で同じアプリまたはアプリ セットの常に使用可能なインスタンスを実行する必要がある場合は、**Virtual Machine Scale Sets** オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="f28bc-118">Use the **Virtual Machine Scale Sets** option when you need to run consistently available instances of the same app, or sets of apps, on similarly configured VMs.</span></span> <span data-ttu-id="f28bc-119">何千もの同一の VM が自動的に生成されてアプリケーションが分単位で読み込まれるので、ユーザーは待つ必要がありません。</span><span class="sxs-lookup"><span data-stu-id="f28bc-119">It automatically generates thousands of identical VMs loaded with your application in minutes, so your users never have to wait.</span></span> <span data-ttu-id="f28bc-120">また、仮想マシンを事前にプロビジョニングする必要がないため、いつでもアプリケーションで必要なコンピューティング リソースだけが使用されます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-120">And, since you don't have to pre-provision virtual machines, you use only the compute resources your application needs at any time.</span></span>

- <span data-ttu-id="f28bc-121">何十、何百、または数千もの VM にスケーリングする機能を備えたクラウド規模のジョブ スケジューリングおよびコンピューティング管理が必要な場合は、**Batch** オプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="f28bc-121">Use the **Batch** option when you need cloud-scale job scheduling and compute management with the ability to scale to tens, hundreds, or thousands of VMs.</span></span> <span data-ttu-id="f28bc-122">手が加えられていないコンピューティング能力またはスーパーコンピューター レベルのコンピューティング パワーが必要な場合があります。Azure ではこれらの機能が提供されます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-122">There may be situations in which you need raw computing power or supercomputer level compute power &mdash; Azure provides these capabilities.</span></span>

## <a name="azure-containers"></a><span data-ttu-id="f28bc-123">Azure コンテナー</span><span class="sxs-lookup"><span data-stu-id="f28bc-123">Azure containers</span></span>

:::row:::
  :::column:::
    ![Azure コンテナーを表すイメージ](../media/3-azure-containers.png)
  :::column-end:::
  :::column span="3":::
<span data-ttu-id="f28bc-125">単一の仮想マシンでアプリケーションの複数のインスタンスを実行したい場合は、コンテナーが優れた選択肢です。</span><span class="sxs-lookup"><span data-stu-id="f28bc-125">If you wish to run multiple instances of an application on a single virtual machine, containers are an excellent choice.</span></span> <span data-ttu-id="f28bc-126">コンテナー オーケストレーターで、必要に応じてアプリケーション インスタンスを開始、停止、スケールアウトできます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-126">The container orchestrator can start, stop, and scale out application instances as needed.</span></span>
  :::column-end:::
:::row-end:::

#### <a name="vms-versus-containers"></a><span data-ttu-id="f28bc-127">コンテナーと VM</span><span class="sxs-lookup"><span data-stu-id="f28bc-127">VMs versus containers</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuaq]

<span data-ttu-id="f28bc-128">コンテナーは、ソリューションを小さな部分に分割するためによく使用されるため、マイクロサービス アーキテクチャを使用してソリューションを作成するのによく使用されます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-128">Containers are often used to create solutions using a microservice architecture, as they are often used to break solutions into smaller pieces.</span></span> <span data-ttu-id="f28bc-129">たとえば、1 つの Web サイトを、フロントエンドをホストするコンテナー、バックエンドをホストするコンテナー、ストレージ用の 3 つ目のコンテナーに分割できます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-129">For example, you may split a web site into a container hosting your front end, another hosting your back end, and a third for storage.</span></span> <span data-ttu-id="f28bc-130">これにより、独立して維持、スケーリング、更新できる論理的なセクションに、アプリの部分を分離することができます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-130">This allows you to separate portions of your app into logical sections that can be maintained, scaled, or updated independently.</span></span>

#### <a name="what-is-a-microservice"></a><span data-ttu-id="f28bc-131">マイクロサービスとは</span><span class="sxs-lookup"><span data-stu-id="f28bc-131">What is a microservice?</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yual]

<span data-ttu-id="f28bc-132">Web サイトのバックエンドは容量に達したけれども、フロントエンドとストレージはまだ余裕がある場合を想像してください。</span><span class="sxs-lookup"><span data-stu-id="f28bc-132">Imagine your web site back end has reached capacity but the front end and storage aren't being stressed.</span></span> <span data-ttu-id="f28bc-133">バックエンドを個別にスケーリングして、パフォーマンスを向上させることも、別のストレージ サービスを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-133">You could scale the back end separately to improve performance, or you could decide to use a different storage service.</span></span> <span data-ttu-id="f28bc-134">または、アプリケーションの他の部分に影響を与えずに、ストレージ コンテナーを置き換えることもできます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-134">Or you could even replace the storage container without affecting the rest of the application.</span></span>

<span data-ttu-id="f28bc-135">チームが Kubernetes コンテナー オーケストレーションを使い慣れている場合は、**Azure Kubernetes Service (AKS)** オプションを検討します。</span><span class="sxs-lookup"><span data-stu-id="f28bc-135">If your team is comfortable with using Kubernetes container orchestration, consider the **Azure Kubernetes Service (AKS)** option.</span></span> <span data-ttu-id="f28bc-136">このオプションは、Kubernetes オーケストレーションの管理、展開、操作を簡素化および自動化します。</span><span class="sxs-lookup"><span data-stu-id="f28bc-136">It simplifies and automates the management, deployment, and operations of Kubernetes orchestration.</span></span>

#### <a name="what-is-kubernetes"></a><span data-ttu-id="f28bc-137">Kubernetes とは</span><span class="sxs-lookup"><span data-stu-id="f28bc-137">What is Kubernetes?</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuX]

## <a name="azure-functions"></a><span data-ttu-id="f28bc-138">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="f28bc-138">Azure Functions</span></span>

:::row:::
  :::column:::
    ![Azure Functions を表すイメージ](../media/3-azure-functions.png)
  :::column-end:::
  :::column span="3":::
<span data-ttu-id="f28bc-140">サービスを実行しているコードのみに関心があり、基になるプラットフォームやインフラストラクチャには関心がない場合は、Azure Functions が最適です。</span><span class="sxs-lookup"><span data-stu-id="f28bc-140">When you're concerned only about the code running your service, and not the underlying platform or infrastructure, Azure Functions are ideal.</span></span> <span data-ttu-id="f28bc-141">REST 要求、タイマー、または別の Azure サービスからのメッセージによるイベントに応答して処理を実行する必要があり、数秒以内にすばやく処理を完了できる場合に、よく使用されます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-141">They're commonly used when you need to perform work in response to an event, often via a REST request, timer, or message from another Azure service and when that work can be completed quickly, within seconds or less.</span></span>
  :::column-end:::
:::row-end:::

<span data-ttu-id="f28bc-142">Azure Functions は自動的にスケーリングされるので、需要が変化する場合に確実な選択肢であり、関数がトリガーされたときにのみ課金されます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-142">Azure Functions scale automatically, so they're a solid choice when demand is variable, and you're charged only when a function is triggered.</span></span> <span data-ttu-id="f28bc-143">たとえば、配送車群を監視するために使用されている IoT ソリューションからのメッセージを受信するような場合があります。</span><span class="sxs-lookup"><span data-stu-id="f28bc-143">For example, you may be receiving messages from an IoT solution used to monitor a fleet of delivery vehicles.</span></span> <span data-ttu-id="f28bc-144">業務時間中は受信データが増える可能性があります。</span><span class="sxs-lookup"><span data-stu-id="f28bc-144">You'll likely have more data arriving during business hours.</span></span>

<span data-ttu-id="f28bc-145">さらに、Azure Functions はステートレスであり、イベントに応答するたびに再起動されたかのように動作します。</span><span class="sxs-lookup"><span data-stu-id="f28bc-145">Furthermore, Azure Functions are stateless; they behave as if they're restarted every time they respond to an event.</span></span> <span data-ttu-id="f28bc-146">これは、受信データを処理するために最適です。</span><span class="sxs-lookup"><span data-stu-id="f28bc-146">This is ideal for processing incoming data.</span></span> <span data-ttu-id="f28bc-147">状態が必要な場合は、Azure ストレージ サービスに接続できます。</span><span class="sxs-lookup"><span data-stu-id="f28bc-147">And if state is required, they can be connected to an Azure storage service.</span></span>
