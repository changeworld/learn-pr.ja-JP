<span data-ttu-id="303c2-101">研究チームは、火星での発見につながる可能性がある膨大な量の画像データを収集しています。</span><span class="sxs-lookup"><span data-stu-id="303c2-101">Your research team has collected massive amounts of image data that might lead to a discovery on Mars.</span></span> <span data-ttu-id="303c2-102">チームは、計算負荷の高いデータ処理を実行する必要がありますが、この作業を行うための機器は持っていません。</span><span class="sxs-lookup"><span data-stu-id="303c2-102">They need to perform computationally intense data processing but don't have the equipment to do the work.</span></span> <span data-ttu-id="303c2-103">データ分析を行うのに Azure が適切な選択である理由を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="303c2-103">Let's see why Azure is a good choice to do the data analysis.</span></span>

## <a name="what-is-azure-compute"></a><span data-ttu-id="303c2-104">Azure コンピューティングとは</span><span class="sxs-lookup"><span data-stu-id="303c2-104">What is Azure compute?</span></span>
<span data-ttu-id="303c2-105">Azure コンピューティングとは、クラウド ベースのアプリケーションを実行するためのオンデマンド コンピューティング サービスです。</span><span class="sxs-lookup"><span data-stu-id="303c2-105">Azure compute is an on-demand computing service for running cloud-based applications.</span></span> <span data-ttu-id="303c2-106">仮想マシンとコンテナーを使用して、マルチコア プロセッサやスーパーコンピューターなどのコンピューティング リソースを提供します。</span><span class="sxs-lookup"><span data-stu-id="303c2-106">It provides computing resources like multi-core processors and supercomputers via virtual machines and containers.</span></span> <span data-ttu-id="303c2-107">また、インフラストラクチャのセットアップや構成を必要とせずにアプリを実行する、サーバーレス コンピューティングも提供します。</span><span class="sxs-lookup"><span data-stu-id="303c2-107">It also provides serverless computing to run apps without requiring infrastructure setup or configuration.</span></span> <span data-ttu-id="303c2-108">オンデマンドでリソースを利用でき、通常は分単位で、場合によっては秒単位でさえ作成することができます。</span><span class="sxs-lookup"><span data-stu-id="303c2-108">The resources are available on-demand and can typically be created in minutes or even seconds.</span></span> <span data-ttu-id="303c2-109">使用したリソースに対してのみ、それらを使用している期間についてだけ、課金されます。</span><span class="sxs-lookup"><span data-stu-id="303c2-109">You pay only for the resources you use and only for as long as you're using them.</span></span>

<span data-ttu-id="303c2-110">Azure でコンピューティングを実行するには、3 つの一般的な手法があります。</span><span class="sxs-lookup"><span data-stu-id="303c2-110">There are three common techniques for performing compute in Azure:</span></span>

- <span data-ttu-id="303c2-111">仮想マシン</span><span class="sxs-lookup"><span data-stu-id="303c2-111">Virtual machines</span></span>
- <span data-ttu-id="303c2-112">Containers</span><span class="sxs-lookup"><span data-stu-id="303c2-112">Containers</span></span>
- <span data-ttu-id="303c2-113">サーバーレス コンピューティング</span><span class="sxs-lookup"><span data-stu-id="303c2-113">Serverless computing</span></span>

## <a name="what-are-virtual-machines"></a><span data-ttu-id="303c2-114">仮想マシンとは</span><span class="sxs-lookup"><span data-stu-id="303c2-114">What are virtual machines?</span></span>

<span data-ttu-id="303c2-115">**仮想マシン** (VM) は、物理コンピューターのソフトウェア エミュレーションです。</span><span class="sxs-lookup"><span data-stu-id="303c2-115">**Virtual machines**, or VMs, are software emulations of physical computers.</span></span> <span data-ttu-id="303c2-116">仮想プロセッサ、メモリ、ストレージ、およびネットワーク リソースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="303c2-116">They include a virtual processor, memory, storage, and networking resources.</span></span> <span data-ttu-id="303c2-117">オペレーティング システムをホストし、物理コンピューターと同じようにソフトウェアをインストールして実行できます。</span><span class="sxs-lookup"><span data-stu-id="303c2-117">They host an operating system, and you're able to install and run software just like a physical computer.</span></span> <span data-ttu-id="303c2-118">リモート デスクトップ クライアントを使用することにより、その前に座っているのと同じように、仮想マシンを使用および制御できます。</span><span class="sxs-lookup"><span data-stu-id="303c2-118">And by using a remote desktop client, you can use and control the virtual machine as if you were sitting in front it.</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="303c2-119">![仮想ネットワークを表すイメージ](../media/2-vm.png)</span><span class="sxs-lookup"><span data-stu-id="303c2-119">![Image representing a virtual machine](../media/2-vm.png)</span></span>
  :::column-end:::
    <span data-ttu-id="303c2-120">:::column span="3"::: **Azure での仮想マシン**</span><span class="sxs-lookup"><span data-stu-id="303c2-120">:::column span="3"::: **Virtual machines in Azure**</span></span>

<span data-ttu-id="303c2-121">仮想マシンを Azure で作成したり、ホストしたりすることができます。</span><span class="sxs-lookup"><span data-stu-id="303c2-121">Virtual machines can be created and hosted in Azure.</span></span> <span data-ttu-id="303c2-122">通常、事前構成済みの仮想マシン イメージを選択すると、数分で新しい仮想マシンを作成してプロビジョニングできます。</span><span class="sxs-lookup"><span data-stu-id="303c2-122">Typically, new virtual machines can be created and provisioned in minutes by selecting a pre-configured virtual machine image.</span></span>

<span data-ttu-id="303c2-123">イメージの選択は、VM を作成するときに行う最も重要な決定事項の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="303c2-123">Selecting an image is one of the most important decisions you'll make when creating a VM.</span></span> <span data-ttu-id="303c2-124">イメージとは、仮想マシンの作成に使用するテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="303c2-124">An image is a template used to create a virtual machine.</span></span> <span data-ttu-id="303c2-125">これらのテンプレートには、オペレーティング システム (OS) と、多くの場合は開発ツールや Web ホスティング環境などの他のソフトウェアが、既に含まれています。</span><span class="sxs-lookup"><span data-stu-id="303c2-125">These templates already include an operating system (OS) and often other software, such as development tools or web hosting environments.</span></span>

  :::column-end:::
:::row-end:::

## <a name="what-are-containers"></a><span data-ttu-id="303c2-126">コンテナーとは</span><span class="sxs-lookup"><span data-stu-id="303c2-126">What are containers?</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

<span data-ttu-id="303c2-127">コンテナーは仮想化環境ですが、仮想マシンとは異なりオペレーティング システムを含みません。</span><span class="sxs-lookup"><span data-stu-id="303c2-127">Containers are a virtualization environment but, unlike a virtual machine, they do not include an operating system.</span></span> <span data-ttu-id="303c2-128">代わりに、コンテナーを実行しているホスト環境のオペレーティング システムを参照します。</span><span class="sxs-lookup"><span data-stu-id="303c2-128">Instead, they reference the operating system of the host environment that runs the container.</span></span> <span data-ttu-id="303c2-129">たとえば、特定の Linux カーネルを使用するサーバーで 5 つのコンテナーを実行すると、5 つのコンテナーすべてがその同じカーネルで実行されます。</span><span class="sxs-lookup"><span data-stu-id="303c2-129">For example, if five containers are running on a server with a specific Linux kernel, all five containers are running on that same kernel.</span></span>

<span data-ttu-id="303c2-130">次の図では、VM 上で直接実行されているアプリケーションと VM 上のコンテナー内で実行されているアプリケーションを比較しています。</span><span class="sxs-lookup"><span data-stu-id="303c2-130">The following illustration shows a comparison between applications running directly on a VM and applications running inside containers on a VM.</span></span>

![オペレーティング システムは仮想マシンの一部であって、コンテナーの一部ではないことを示す図](../media/2-vm-versus-containers.png)

<span data-ttu-id="303c2-132">通常、コンテナーにはユーザーが作成したアプリケーションと、ホスト環境のカーネルでそのアプリケーションを実行するために必要なすべてのライブラリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="303c2-132">Containers typically contain an application that you write &mdash; along with any libraries required for your application to run on the host environment's kernel.</span></span>

<span data-ttu-id="303c2-133">コンテナーは軽量になるように意図され、動的に作成、スケールアウト、停止されるように設計されています。</span><span class="sxs-lookup"><span data-stu-id="303c2-133">Containers are meant to be lightweight and are designed to be created, scaled out, and stopped dynamically.</span></span> <span data-ttu-id="303c2-134">これにより、需要の変化に対応し、クラッシュやハードウェアの中断が発生した場合はすぐに再起動することができます。</span><span class="sxs-lookup"><span data-stu-id="303c2-134">This allows you to respond to changes on demand and quickly restart in case of a crash or hardware interruption.</span></span>

<span data-ttu-id="303c2-135">コンテナーを使用するその他の利点は、複数の独立したアプリケーションを仮想マシンで実行できることです。</span><span class="sxs-lookup"><span data-stu-id="303c2-135">An additional benefit of using containers is the ability to run multiple isolated applications on a virtual machine.</span></span> <span data-ttu-id="303c2-136">コンテナー自体がセキュリティで保護されて分離されているので、異なるワークロードごとに VM を分ける必要は必ずしもありません。</span><span class="sxs-lookup"><span data-stu-id="303c2-136">Since containers themselves are secured and isolated, you don't necessarily need separate VMs for separate workloads.</span></span>

<span data-ttu-id="303c2-137">Azure では、Docker コンテナーと、それらのコンテナーを管理するための複数の方法がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="303c2-137">Azure supports Docker containers and several ways to manage those containers.</span></span> <span data-ttu-id="303c2-138">手動で、または Azure Kubernetes Service などの Azure サービスで、コンテナーを管理できます。</span><span class="sxs-lookup"><span data-stu-id="303c2-138">Containers can be managed manually or with Azure services such as Azure Kubernetes Service.</span></span>

### <a name="what-is-serverless-computing"></a><span data-ttu-id="303c2-139">サーバーレス コンピューティングとは</span><span class="sxs-lookup"><span data-stu-id="303c2-139">What is serverless computing?</span></span>

<span data-ttu-id="303c2-140">サーバーレス コンピューティングとは、ユーザーのコードを実行するクラウドでホストされた実行環境ですが、基になるホスティング環境が完全に抽象化されています。</span><span class="sxs-lookup"><span data-stu-id="303c2-140">Serverless computing is a cloud-hosted execution environment that runs your code but completely abstracts the underlying hosting environment.</span></span> <span data-ttu-id="303c2-141">サービスのインスタンスを作成して、コードを追加します。インフラストラクチャの構成やメンテナンスは必要はありませんが、許可されていることもあります。</span><span class="sxs-lookup"><span data-stu-id="303c2-141">You create an instance of the service, and you add your code; no infrastructure configuration or maintenance is required, or even allowed.</span></span>

#### <a name="what-is-serverless-computing"></a><span data-ttu-id="303c2-142">サーバーレス コンピューティングとは</span><span class="sxs-lookup"><span data-stu-id="303c2-142">What is Serverless Computing?</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yzjL]

<span data-ttu-id="303c2-143">_イベント_ に応答するようにサーバーレス アプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="303c2-143">You configure your serverless apps to respond to _events_.</span></span> <span data-ttu-id="303c2-144">REST エンドポイント、定期的なタイマー、別の Azure サービスから受信したメッセージなどのイベントがあります。</span><span class="sxs-lookup"><span data-stu-id="303c2-144">This could be a REST endpoint, a periodic timer, or even a message received from another Azure service.</span></span> <span data-ttu-id="303c2-145">イベントでトリガーされた場合にのみ、サーバーレス アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="303c2-145">The serverless app runs only when it's triggered by an event.</span></span>

<span data-ttu-id="303c2-146">基本的に、ユーザーにはインフラストラクチャに関する責任はありません。</span><span class="sxs-lookup"><span data-stu-id="303c2-146">Essentially, infrastructure isn't your responsibility.</span></span> <span data-ttu-id="303c2-147">スケーリングとパフォーマンスは自動的に処理され、使用した正確なリソースに対してのみ課金されます。</span><span class="sxs-lookup"><span data-stu-id="303c2-147">Scaling and performance are handled automatically, and you are billed only for the exact resources you use.</span></span> <span data-ttu-id="303c2-148">容量を予約する必要すらありません。</span><span class="sxs-lookup"><span data-stu-id="303c2-148">There's no need to even reserve capacity.</span></span>

:::row:::
  :::column:::
    <span data-ttu-id="303c2-149">![サーバーレス コンピューティングを表すイメージ](../media/2-serverless.png)</span><span class="sxs-lookup"><span data-stu-id="303c2-149">![Image representing serverless computing](../media/2-serverless.png)</span></span>
  :::column-end:::
    <span data-ttu-id="303c2-150">:::column span="3"::: **Azure でのサーバーレス コンピューティング**</span><span class="sxs-lookup"><span data-stu-id="303c2-150">:::column span="3"::: **Serverless computing in Azure**</span></span>

<span data-ttu-id="303c2-151">Azure には、サーバーレス コンピューティングの 2 つの実装があります。</span><span class="sxs-lookup"><span data-stu-id="303c2-151">Azure has two implementations of serverless compute:</span></span>

- <span data-ttu-id="303c2-152">**Azure Functions**: ほぼすべての最新の言語でコードを実行できます。</span><span class="sxs-lookup"><span data-stu-id="303c2-152">**Azure Functions** which can execute code in almost any modern language.</span></span>
- <span data-ttu-id="303c2-153">**Azure Logic Apps**: Web ベースのデザイナーで設計されており、コードを記述せずに Azure サービスによってトリガーされるロジックを実行できます。</span><span class="sxs-lookup"><span data-stu-id="303c2-153">**Azure Logic Apps** which are designed in a web-based designer and can execute logic triggered by Azure services without writing any code.</span></span>

  :::column-end:::
:::row-end:::