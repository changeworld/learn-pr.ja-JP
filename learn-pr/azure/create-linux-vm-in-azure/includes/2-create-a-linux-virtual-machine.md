<span data-ttu-id="7bc11-101">ローカルの Ubuntu Linux サーバー上で実行されている既存の Web サイトがあります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-101">We have an existing website running on a local Ubuntu Linux server.</span></span> <span data-ttu-id="7bc11-102">目標は、最新の Ubuntu イメージを使用して Azure VM を作成し、サイトをクラウドに移行することです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-102">Our goal is to create an Azure VM, using the latest Ubuntu image and migrate the site to the cloud.</span></span> <span data-ttu-id="7bc11-103">このユニットでは、Azure で仮想マシンを作成するときに決定する必要があるオプションについて学習します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-103">In this unit, you will learn about the options you will need to decide when creating a virtual machine in Azure.h</span></span>

## <a name="introduction-to-azure-virtual-machines"></a><span data-ttu-id="7bc11-104">Azure Virtual Machines の概要</span><span class="sxs-lookup"><span data-stu-id="7bc11-104">Introduction to Azure Virtual Machines</span></span>

<span data-ttu-id="7bc11-105">Azure VM は、オンデマンドのスケーラブルなクラウド コンピューティング リソースです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-105">Azure VMs are an on-demand scalable cloud computing resource.</span></span> <span data-ttu-id="7bc11-106">プロセッサ、メモリ、ストレージ、およびネットワーク リソースが含まれます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-106">They include processor, memory, storage, and networking resources.</span></span> <span data-ttu-id="7bc11-107">自由に仮想マシンを開始および停止し、Azure portal から、または Azure CLI を使用して仮想マシンを管理できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-107">You can start and stop virtual machines at will, and manage them from the Azure portal or with the Azure CLI.</span></span> <span data-ttu-id="7bc11-108">また、リモート Secure Shell (SSH) を使用して実行中の VM に直接接続し、ローカル コンピューターの場合と同じようにコマンドを実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-108">You can also use a remote secure shell (SSH) to connect directly to the running VM and execute commands as if you are on a local computer.</span></span>

### <a name="running-linux-in-azure"></a><span data-ttu-id="7bc11-109">Azure での Linux の実行</span><span class="sxs-lookup"><span data-stu-id="7bc11-109">Running Linux in Azure</span></span>

<span data-ttu-id="7bc11-110">Azure で Linux ベースの VM を作成することは簡単です。</span><span class="sxs-lookup"><span data-stu-id="7bc11-110">Creating Linux-based VMs in Azure is easy.</span></span> <span data-ttu-id="7bc11-111">Microsoft は主要な Linux ベンダーと提携し、ディストリビューションが Azure プラットフォーム用に確実に最適化されるようにしています。</span><span class="sxs-lookup"><span data-stu-id="7bc11-111">Microsoft has partnered with prominent Linux vendors to ensure their distributions are optimized for the Azure platform.</span></span> <span data-ttu-id="7bc11-112">SUSE、Red Hat、Ubuntu などのさまざまな人気のある Linux ディストリビューションの事前構築済みイメージから仮想マシンを作成することも、クラウドで実行するように独自の Linux ディストリビューションを構築することもできます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-112">You can create virtual machines from pre-built images for a variety of popular Linux distributions such as SUSE, Red Hat, and Ubuntu or build your own Linux distribution to run in the cloud.</span></span>

## <a name="creating-an-azure-vm"></a><span data-ttu-id="7bc11-113">Azure VM の作成</span><span class="sxs-lookup"><span data-stu-id="7bc11-113">Creating an Azure VM</span></span>

<span data-ttu-id="7bc11-114">VM を定義して Azure にデプロイするには、いくつかの方法があります。つまり、Azure portal、スクリプト (Azure CLI または Azure PowerShell を使用)、Azure Resource Manager テンプレートのいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-114">VMs can be defined and deployed on Azure in several ways: the Azure portal, a script (using the Azure CLI or Azure PowerShell), or through an Azure Resource Manager template.</span></span> <span data-ttu-id="7bc11-115">いずれの場合も、いくつかの情報を指定する必要があります。それについては、この後すぐに説明します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-115">In all cases, you will need to supply several pieces of information which we'll cover shortly.</span></span>

<span data-ttu-id="7bc11-116">Azure Marketplace には、OS と特定のシナリオ用にインストールされた人気のあるソフトウェア ツールの両方を含む、事前構成済みのイメージも用意されています。</span><span class="sxs-lookup"><span data-stu-id="7bc11-116">The Azure Marketplace also provides pre-configured images that include both an OS and favorite software tools installed for specific scenarios.</span></span>

![Azure Marketplace の仮想マシン](../media-drafts/2-marketplace-vm-choices.png)

## <a name="resources-used-in-a-linux-vm"></a><span data-ttu-id="7bc11-118">Linux VM で使用されるリソース</span><span class="sxs-lookup"><span data-stu-id="7bc11-118">Resources used in a Linux VM</span></span>

<span data-ttu-id="7bc11-119">Azure 内に Linux VM を作成する場合、VM をホストするためのリソースも作成します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-119">When creating a Linux VM in Azure, you also create resources to host the VM.</span></span> <span data-ttu-id="7bc11-120">これらのリソースが連携することによってコンピューターが仮想化され、Linux オペレーティング システムが実行されます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-120">These resources work together to virtualize a computer and run the Linux operating system.</span></span> <span data-ttu-id="7bc11-121">これらのリソースは、存在しているか (VM の作成時に選択します)、または VM と共に作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-121">These must either exist (and be selected during VM creation), or they will be created with the VM.</span></span>

- <span data-ttu-id="7bc11-122">CPU とメモリ リソースを提供する仮想マシン。</span><span class="sxs-lookup"><span data-stu-id="7bc11-122">A virtual machine that provides CPU and memory resources.</span></span>
- <span data-ttu-id="7bc11-123">仮想ハード ディスクを保持する Azure Storage アカウント。</span><span class="sxs-lookup"><span data-stu-id="7bc11-123">An Azure Storage account to hold the virtual hard disks.</span></span>
- <span data-ttu-id="7bc11-124">OS、アプリケーション、データを保持する仮想ディスク。</span><span class="sxs-lookup"><span data-stu-id="7bc11-124">Virtual disks to hold the OS, applications, and data.</span></span>
- <span data-ttu-id="7bc11-125">他の Azure サービスやオンプレミスのハードウェアに VM を接続する仮想ネットワーク (VNet)。</span><span class="sxs-lookup"><span data-stu-id="7bc11-125">A virtual network (VNet) to connect the VM to other Azure services or your on-premise hardware.</span></span>
- <span data-ttu-id="7bc11-126">VNet と通信するためのネットワーク インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="7bc11-126">A network interface to communicate with the VNet.</span></span>
- <span data-ttu-id="7bc11-127">VM にアクセスできるようにするための省略可能なパブリック IP アドレス。</span><span class="sxs-lookup"><span data-stu-id="7bc11-127">An optional public IP address so you can access the VM.</span></span>

<span data-ttu-id="7bc11-128">他の Azure サービスと同じように、VM を格納する (および、必要に応じて管理のためにこれらのリソースをグループ化する) ための**リソース グループ**が必要です。</span><span class="sxs-lookup"><span data-stu-id="7bc11-128">Like other Azure services, you'll need a **Resource Group** to contain the VM (and optionally group these resources for administration).</span></span> <span data-ttu-id="7bc11-129">新しい VM を作成する場合、既存のリソース グループを使用することも、新しいリソース グループを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-129">When you create a new VM, you can either use an existing resource group or create a new one.</span></span>

## <a name="choose-the-vm-image"></a><span data-ttu-id="7bc11-130">VM イメージを選択する</span><span class="sxs-lookup"><span data-stu-id="7bc11-130">Choose the VM image</span></span>

<span data-ttu-id="7bc11-131">イメージの選択は、VM を作成するときに行う最初の最も重要な決定事項の 1 つです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-131">Selecting an image is one of the first and most important decisions you'll make when creating a VM.</span></span> <span data-ttu-id="7bc11-132">イメージとは、VM の作成に使用するテンプレートです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-132">An image is a template that's used to create a VM.</span></span> <span data-ttu-id="7bc11-133">これらのテンプレートには、OS と、多くの場合はその他のソフトウェア (開発ツールや Web ホスティング環境など) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="7bc11-133">These templates include an OS and often other software, such as development tools or web hosting environments.</span></span>

<span data-ttu-id="7bc11-134">コンピューターにインストールできるものはすべて、イメージに含めることができます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-134">Anything that a computer can have installed can be included in an image.</span></span> <span data-ttu-id="7bc11-135">Apache サーバーでの Web アプリのホスティングなど、必要なタスクに正確に対応するように事前構成されたイメージから、VM を作成できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-135">You can create a VM from an image that's pre-configured to precisely the tasks you need, such as hosting a web app on Apache server.</span></span>

> [!TIP]
> <span data-ttu-id="7bc11-136">カスタム ディスク イメージを作成してアップロードすることもできます。詳しくは、ドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="7bc11-136">You can also create and upload custom disk images, check the documentation for more information.</span></span>

## <a name="sizing-your-vm"></a><span data-ttu-id="7bc11-137">VM のサイズ設定</span><span class="sxs-lookup"><span data-stu-id="7bc11-137">Sizing your VM</span></span>
<span data-ttu-id="7bc11-138">物理マシンに一定量のメモリや CPU 電源があるように、仮想マシンにも同様にあります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-138">Just as a physical machine has a certain amount of memory and CPU power, so does a virtual machine.</span></span> <span data-ttu-id="7bc11-139">Azure では、さまざまなサイズの VM が、異なる価格ポイントで提供されています。</span><span class="sxs-lookup"><span data-stu-id="7bc11-139">Azure offers a range of VMs of differing sizes at different price points.</span></span> <span data-ttu-id="7bc11-140">選択したサイズにより、VM の処理能力、メモリ、最大ストレージ容量が決まります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-140">The size that you choose will determine the VMs processing power, memory, and max storage capacity.</span></span>

> [!WARNING]
> <span data-ttu-id="7bc11-141">各サブスクリプションには、VM の作成に影響するクォータ制限があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-141">There are quota limits on each subscription that can impact VM creation.</span></span> <span data-ttu-id="7bc11-142">既定では、リージョン内のすべての VM で保持できる仮想 "_コア_" の数は最大 20 個です。</span><span class="sxs-lookup"><span data-stu-id="7bc11-142">By default, you cannot have more than 20 virtual _cores_ across all VMs within a region.</span></span> <span data-ttu-id="7bc11-143">VM を複数のリージョンに分割するか、制限を引き上げる[オンライン要求](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request)を申請することができます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-143">You can either split up VMs across regions or file an [online request](https://docs.microsoft.com/en-us/azure/azure-supportability/resource-manager-core-quotas-request) to increase your limits.</span></span>

<span data-ttu-id="7bc11-144">VM のサイズは複数のカテゴリにグループ化されており、基本的なテストと実行のための B シリーズから、大規模なコンピューティング タスク向けの H シリーズまであります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-144">VM sizes are grouped into categories, starting with the B-series for basic testing and running up to the H-series for massive computing tasks.</span></span> <span data-ttu-id="7bc11-145">実行するワークロードに基づいて VM のサイズを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-145">You should select the size of the VM based on the workload you want to perform.</span></span> <span data-ttu-id="7bc11-146">VM 作成後に VM のサイズを変更することは可能ですが、そのためにはまず VM を停止する必要があるので、可能であれば最初から適切なサイズを選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="7bc11-146">It is possible to change the size of a VM after it's been created, but the VM must be stopped first, so it's best to size it appropriately from the start if possible.</span></span>

#### <a name="here-are-some-guidelines-based-on-the-scenario-you-are-targeting"></a><span data-ttu-id="7bc11-147">対象とするシナリオに基づいたガイドラインを以下に示します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-147">Here are some guidelines based on the scenario you are targeting.</span></span>

| <span data-ttu-id="7bc11-148">操作内容</span><span class="sxs-lookup"><span data-stu-id="7bc11-148">What are you doing?</span></span> | <span data-ttu-id="7bc11-149">考慮するサイズ</span><span class="sxs-lookup"><span data-stu-id="7bc11-149">Consider these sizes</span></span>
|-------|------------------|
| <span data-ttu-id="7bc11-150">**汎用コンピューティング/Web**: テストと開発、小規模から中規模のデータベース、軽度から中程度のトラフィックの Web サーバー。</span><span class="sxs-lookup"><span data-stu-id="7bc11-150">**General use computing/web** Testing and development, small to medium databases, or low to medium traffic web servers.</span></span> | <span data-ttu-id="7bc11-151">B、Dsv3、Dv3、DSv2、Dv2</span><span class="sxs-lookup"><span data-stu-id="7bc11-151">B, Dsv3, Dv3, DSv2, Dv2</span></span> |
| <span data-ttu-id="7bc11-152">**高負荷の計算タスク**: トラフィックが中程度の Web サーバー、ネットワーク アプライアンス、バッチ処理、アプリケーション サーバー。</span><span class="sxs-lookup"><span data-stu-id="7bc11-152">**Heavy computational tasks** Medium traffic web servers, network appliances, batch processes, and application servers.</span></span> | <span data-ttu-id="7bc11-153">Fsv2、Fs、F</span><span class="sxs-lookup"><span data-stu-id="7bc11-153">Fsv2, Fs, F</span></span> |
| <span data-ttu-id="7bc11-154">**大容量のメモリ使用量**: リレーショナル データベース サーバー、中規模から大規模のキャッシュ、メモリ内分析。</span><span class="sxs-lookup"><span data-stu-id="7bc11-154">**Large memory usage** Relational database servers, medium to large caches, and in-memory analytics.</span></span> | <span data-ttu-id="7bc11-155">Esv3、Ev3、M、GS、G、DSv2、Dv2</span><span class="sxs-lookup"><span data-stu-id="7bc11-155">Esv3, Ev3, M, GS, G, DSv2, Dv2</span></span> |
| <span data-ttu-id="7bc11-156">**データのストレージと処理**: 高いディスク スループットと IO を必要とするビッグ データ、SQL、および NoSQL データベース。</span><span class="sxs-lookup"><span data-stu-id="7bc11-156">**Data storage and processing** Big Data, SQL, and NoSQL databases which need high disk throughput and IO.</span></span> | <span data-ttu-id="7bc11-157">Ls</span><span class="sxs-lookup"><span data-stu-id="7bc11-157">Ls</span></span> |
| <span data-ttu-id="7bc11-158">**負荷の高いグラフィックスのレンダリング**やビデオ編集、およびディープ ラーニングを使用したモデル トレーニングと推論 (ND)。</span><span class="sxs-lookup"><span data-stu-id="7bc11-158">**Heavy graphics rendering** or video editing, as well as model training and inferencing (ND) with deep learning.</span></span> | <span data-ttu-id="7bc11-159">NV、NC、NCv2、NCv3、ND</span><span class="sxs-lookup"><span data-stu-id="7bc11-159">NV, NC, NCv2, NCv3, ND</span></span> |
| <span data-ttu-id="7bc11-160">**ハイ パフォーマンス コンピューティング (HPC)**: 高スループットのネットワーク インターフェイスのオプションを備えた、最も高速かつ強力な CPU 仮想マシンが必要な場合。</span><span class="sxs-lookup"><span data-stu-id="7bc11-160">**High-performance computing (HPC)** If you need the fastest and most powerful CPU virtual machines with optional high-throughput network interfaces.</span></span> | <span data-ttu-id="7bc11-161">H</span><span class="sxs-lookup"><span data-stu-id="7bc11-161">H</span></span> |

## <a name="choosing-storage-options"></a><span data-ttu-id="7bc11-162">ストレージ オプションの選択</span><span class="sxs-lookup"><span data-stu-id="7bc11-162">Choosing storage options</span></span>

<span data-ttu-id="7bc11-163">次の一連の決定では、ストレージに関して解決します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-163">The next set of decisions revolve around storage.</span></span> <span data-ttu-id="7bc11-164">最初に、ディスク テクノロジを選択できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-164">First, you can choose the disk technology.</span></span> <span data-ttu-id="7bc11-165">オプションには、従来の円盤状の記憶媒体ベースのハード ディスク ドライブ (HDD) や、より最新のソリッドステート ドライブ (SSD) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-165">Options include a traditional platter-based hard disk drive (HDD) or a more modern solid-state drive (SSD).</span></span> <span data-ttu-id="7bc11-166">ハードウェアを購入する場合と同じように、SSD ストレージは高コストですがパフォーマンスに優れています。</span><span class="sxs-lookup"><span data-stu-id="7bc11-166">Just like the hardware you purchase, SSD storage costs more but provides better performance.</span></span>

> [!TIP]
> <span data-ttu-id="7bc11-167">SSD ストレージでは、Standard と Premium の 2 つのレベルを利用できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-167">There are two levels of SSD storage available: standard and premium.</span></span> <span data-ttu-id="7bc11-168">通常のワークロードで優れたパフォーマンスが必要な場合は、Standard SSD ディスクを選択します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-168">Choose Standard SSD disks if you have normal workloads but want better performance.</span></span> <span data-ttu-id="7bc11-169">I/O 集中型ワークロードの場合、または非常に高速のデータ処理が必要なミッション クリティカルなシステムの場合は、Premium SSD ディスクを選択します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-169">Choose Premium SSD disks if you have I/O intensive workloads or mission-critical systems that need to process data very quickly.</span></span>

### <a name="mapping-storage-to-disks"></a><span data-ttu-id="7bc11-170">ディスクへのストレージのマッピング</span><span class="sxs-lookup"><span data-stu-id="7bc11-170">Mapping storage to disks</span></span>

<span data-ttu-id="7bc11-171">Azure では、VM 用の物理ディスクを表すために仮想ハード ディスク (VHD) が使用されます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-171">Azure uses Virtual hard disks (VHDs) to represent physical disks for the VM.</span></span> <span data-ttu-id="7bc11-172">VHD は、ディスク ドライブの論理形式とデータをレプリケートしますが、Azure Storage アカウント内のページ BLOB として格納されます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-172">VHDs replicate the logical format and data of a disk drive but are stored as page blobs in an Azure Storage account.</span></span> <span data-ttu-id="7bc11-173">ディスクごとに、使用するストレージの種類 (SSD または HDD) を選択できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-173">You can choose on a per-disk basis what type of storage it should use (SSD or HDD).</span></span> <span data-ttu-id="7bc11-174">これにより、一般にはディスクで実行する予定の I/O に基づいて、各ディスクのパフォーマンスを制御することができます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-174">This allows you to control the performance of each disk, likely based on the I/O you plan to perform on it.</span></span>

<span data-ttu-id="7bc11-175">既定では、2 個の仮想ハード ディスク (VHD) が Linux VM に作成されます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-175">By default, two virtual hard disks (VHDs) will be created for your Linux VM:</span></span>

1. <span data-ttu-id="7bc11-176">**オペレーティング システム ディスク**。</span><span class="sxs-lookup"><span data-stu-id="7bc11-176">The **Operating System disk**.</span></span> <span data-ttu-id="7bc11-177">これはプライマリ ドライブであり、最大容量は 2,048 GB です。</span><span class="sxs-lookup"><span data-stu-id="7bc11-177">This is your primary drive and has a maximum capacity of 2048 GB.</span></span> <span data-ttu-id="7bc11-178">既定のラベルは `/dev/sda` です。</span><span class="sxs-lookup"><span data-stu-id="7bc11-178">It will be labeled as `/dev/sda` by default.</span></span>

1. <span data-ttu-id="7bc11-179">**一時ディスク**。</span><span class="sxs-lookup"><span data-stu-id="7bc11-179">A **Temporary disk**.</span></span> <span data-ttu-id="7bc11-180">このディスクは、OS またはアプリ用の一時的なストレージを提供します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-180">This provides temporary storage for the OS or any apps.</span></span> <span data-ttu-id="7bc11-181">Linux 仮想マシンでは、このディスクは `/dev/sdb` であり、Azure Linux エージェントによりフォーマットされて、`/mnt` にマウントされます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-181">On Linux virtual machines, the disk is `/dev/sdb` and is formatted and mounted to `/mnt` by the Azure Linux Agent.</span></span> <span data-ttu-id="7bc11-182">サイズは、VM のサイズに基づいて設定され、スワップ ファイルの格納に使用されます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-182">It is sized based on the VM size and is used to store the swap file.</span></span> 

> [!WARNING]
> <span data-ttu-id="7bc11-183">一時ディスクは永続的ではありません。</span><span class="sxs-lookup"><span data-stu-id="7bc11-183">The temporary disk is not persistent.</span></span> <span data-ttu-id="7bc11-184">このディスクには、システムにとって重大ではないデータのみを書き込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-184">You should only write data to this disk that is not critical to the system.</span></span>

#### <a name="what-about-data"></a><span data-ttu-id="7bc11-185">データについて</span><span class="sxs-lookup"><span data-stu-id="7bc11-185">What about data?</span></span>

<span data-ttu-id="7bc11-186">プライマリ ドライブに OS だけでなくデータを格納することもできますが、専用の "_データ ディスク_" を作成する方がよい方法です。</span><span class="sxs-lookup"><span data-stu-id="7bc11-186">You can store data on the primary drive along with the OS, but a better approach is to create dedicated _data disks_.</span></span> <span data-ttu-id="7bc11-187">追加のディスクを作成して VM にアタッチできます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-187">You can create and attach additional disks to the VM.</span></span> <span data-ttu-id="7bc11-188">各ディスクには最大 4,095 GB のデータを保持でき、ストレージの最大量は選択した VM のサイズによって決まります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-188">Each disk can hold up to 4095 GB of data, with the maximum amount of storage determined by the VM size you select.</span></span>

> [!NOTE]
> <span data-ttu-id="7bc11-189">興味深い機能として、実際のディスクから VHD イメージを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-189">An interesting capability is to create a VHD image from a real disk.</span></span> <span data-ttu-id="7bc11-190">これにより、オンプレミスのコンピューターからクラウドに "_既存の_" 情報を簡単に移行できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-190">This allows you to easily migrate _existing_ information from an on-premise computer to the cloud.</span></span>

### <a name="unmanaged-vs-managed-disks"></a><span data-ttu-id="7bc11-191">アンマネージド ディスクとマネージド ディスク</span><span class="sxs-lookup"><span data-stu-id="7bc11-191">Unmanaged vs. Managed disks</span></span>

<span data-ttu-id="7bc11-192">ストレージに関して最後に行う選択は、**アンマネージド** ディスクと**マネージド** ディスクのどちらを使用するかです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-192">The final storage choice you'll make is whether to use **unmanaged** or **managed** disks.</span></span>

<span data-ttu-id="7bc11-193">アンマネージド ディスクを使用する場合、VM ディスクに対応する VHD を保持するために使用するストレージ アカウントを自分で管理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-193">With unmanaged disks, you are responsible for the storage accounts that are used to hold the VHDs that correspond to your VM disks.</span></span> <span data-ttu-id="7bc11-194">使用する領域の容量に対応したストレージ アカウントの料金を負担します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-194">You pay the storage account rates for the amount of space you use.</span></span> <span data-ttu-id="7bc11-195">1 つのストレージ アカウントには、I/O 操作数 20,000/秒という固定レート制限があります。これは、1 つのストレージ アカウントがフル スロットルで 40 個の Standard 仮想ハード ディスクをサポートできることを意味します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-195">A single storage account has a fixed rate limit of 20,000 I/O operations/sec. This means that a single storage account is capable of supporting 40 standard virtual hard disks at full throttle.</span></span> <span data-ttu-id="7bc11-196">スケールアウトする必要がある場合は、複数のストレージ アカウントが必要であり、複雑になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-196">If you need to scale out, then you need more than one storage account, which can get complicated.</span></span>

<span data-ttu-id="7bc11-197">マネージド ディスクは、より新しい、お勧めのディスク ストレージ モデルです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-197">Managed disks are the newer and recommended disk storage model.</span></span> <span data-ttu-id="7bc11-198">ストレージ アカウントを管理する負担を Azure に移すことで、この複雑さを適切に解決します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-198">They elegantly solve this complexity by putting the burden of managing the storage accounts onto Azure.</span></span> <span data-ttu-id="7bc11-199">ユーザーはディスクの種類 (Premium または Standard) とディスクのサイズを指定し、ディスクとそれが使用するストレージ "_両方_" の作成と管理は Azure によって行われます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-199">You specify the disk type (Premium or Standard), and the size of the disk and Azure creates and manages both the disk _and_ the storage it uses.</span></span> <span data-ttu-id="7bc11-200">ストレージ アカウント制限について気にする必要がないため、簡単にスケールアウトできます。その他にもいくつかの利点があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-200">You don't have to worry about storage account limits, which makes them easier to scale out. They also offer several other benefits:</span></span>

- <span data-ttu-id="7bc11-201">**信頼性の向上**: Azure では、高信頼性の VM に関連付けられた VHD が Azure ストレージのさまざまな部分に配置されて、同等のレベルの回復力が提供されます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-201">**Increased reliability**: Azure ensures that VHDs associated with high-reliability VMs will be placed in different parts of Azure storage to provide similar levels of resilience.</span></span>
- <span data-ttu-id="7bc11-202">**セキュリティの向上**: マネージド ディスクは、リソース グループ内の実際の管理対象リソースです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-202">**Better security**: Managed disks are real managed resources in the resource group.</span></span> <span data-ttu-id="7bc11-203">つまり、ロールベースのアクセス制御を使用して、VHD データを使用できるユーザーを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-203">This means they can use role-based access control to restrict who can work with the VHD data.</span></span>
- <span data-ttu-id="7bc11-204">**スナップショットのサポート**: スナップショットを使用して、VHD の読み取り専用コピーを作成できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-204">**Snapshot support**: Snapshots can be used to create a read-only copy of a VHD.</span></span> <span data-ttu-id="7bc11-205">所有している VM をシャットダウンする必要がありますが、スナップショットの作成に要する時間はわずか数秒です。</span><span class="sxs-lookup"><span data-stu-id="7bc11-205">You have to shut down the owning VM but creating the snapshot only takes a few seconds.</span></span> <span data-ttu-id="7bc11-206">完了したら、VM の電源を入れ、スナップショットを使用して重複する VM を作成し、運用環境の問題をトラブルシューティングしたり、スナップショットが作成された時点に VM をロールバックしたりできます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-206">Once it's done, you can power on the VM and use the snapshot to create a duplicate VM to troubleshoot a production issue or rollback the VM to the point in time that the snapshot was taken.</span></span>
- <span data-ttu-id="7bc11-207">**バックアップのサポート**: Azure Backup を使用して、ディザスター リカバリーのために、マネージド ディスクを異なるリージョンに自動的にバックアップできます。VM のサービスに影響はありません。</span><span class="sxs-lookup"><span data-stu-id="7bc11-207">**Backup support**: Managed disks can be automatically backed up to different regions for disaster recovery with Azure Backup all without affecting the service of the VM.</span></span>

## <a name="network-communication"></a><span data-ttu-id="7bc11-208">ネットワーク通信</span><span class="sxs-lookup"><span data-stu-id="7bc11-208">Network communication</span></span>

<span data-ttu-id="7bc11-209">仮想マシンは、仮想ネットワーク (VNet) を使用して外部のリソースと通信します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-209">Virtual machines communicate with external resources using a virtual network (VNet).</span></span> <span data-ttu-id="7bc11-210">VNet は、リソースが通信を行う 1 つのリージョン内のプライベート ネットワークを表します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-210">The VNet represents a private network in a single region that your resources communicate on.</span></span> <span data-ttu-id="7bc11-211">仮想ネットワークは、ユーザーがオンプレミスで管理するネットワークと同じようなものです。</span><span class="sxs-lookup"><span data-stu-id="7bc11-211">A virtual network is just like the networks you manage on-premises.</span></span> <span data-ttu-id="7bc11-212">サブネットに分割してリソースを分離したり、他のネットワーク (オンプレミスのネットワークを含む) に接続したり、トラフィック規則を適用して受信および送信接続を管理したりできます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-212">You can divide them up with subnets to isolate resources, connect them to other networks (including your on-premises networks), and apply traffic rules to govern inbound and outbound connections.</span></span>

### <a name="planning-your-network"></a><span data-ttu-id="7bc11-213">ネットワークの計画</span><span class="sxs-lookup"><span data-stu-id="7bc11-213">Planning your network</span></span>

<span data-ttu-id="7bc11-214">新しい VM を作成するときは、新しい仮想ネットワークの作成、またはリージョン内の既存の VNet の使用を選択できます。</span><span class="sxs-lookup"><span data-stu-id="7bc11-214">When you create a new VM, you will have the option of creating a new virtual network or using an existing VNet in your region.</span></span>

<span data-ttu-id="7bc11-215">VM と一緒に Azure でネットワークを自動的に作成するとシンプルですが、ほとんどのシナリオに適していない可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-215">Having Azure create the network together with the VM is simple, but it's likely not ideal for most scenarios.</span></span> <span data-ttu-id="7bc11-216">アーキテクチャ内のすべてのコンポーネントに対するネットワーク要件を "_事前に_" 計画し、必要な VNet の構造を別に作成することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="7bc11-216">It's better to plan your network requirements _up-front_ for all the components in your architecture and create the VNet structure you will need separately.</span></span> <span data-ttu-id="7bc11-217">その後、VM を作成して、既に作成されている VNet に配置します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-217">Then create the VMs and place them into the already-created VNets.</span></span> <span data-ttu-id="7bc11-218">このモジュールでは、後でもう少し詳しく仮想ネットワークについて説明します。</span><span class="sxs-lookup"><span data-stu-id="7bc11-218">We'll look more at virtual networks a bit later in this module.</span></span>

<span data-ttu-id="7bc11-219">仮想マシンを作成する前に、VM の望ましい管理方法を決定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7bc11-219">Before we create a virtual machine, we need to decide how we'd like to administer the VM.</span></span> <span data-ttu-id="7bc11-220">そのオプションを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="7bc11-220">Let's look at our options.</span></span>