<span data-ttu-id="48575-101">あなたが技術の専門家なら、おそらく特定の分野についての専門知識をお持ちでしょう。</span><span class="sxs-lookup"><span data-stu-id="48575-101">As a technology professional, you likely have expertise in a specific area.</span></span> <span data-ttu-id="48575-102">ストレージの管理者か仮想化のエキスパート、それとも最新のセキュリティ プラクティスの専門家でしょうか。</span><span class="sxs-lookup"><span data-stu-id="48575-102">Perhaps you're a storage admin or virtualization expert, or maybe you focus on the latest security practices.</span></span> <span data-ttu-id="48575-103">あなたが学生なら、最も興味を引かれるものをまだ探しているところかもしれません。</span><span class="sxs-lookup"><span data-stu-id="48575-103">If you're a student, you may still be exploring what interests you most.</span></span>

<span data-ttu-id="48575-104">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-104">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="48575-105">役割に関係なく、ほとんどの人は仮想マシンを作成することによってクラウドを使い始めます。</span><span class="sxs-lookup"><span data-stu-id="48575-105">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="48575-106">ここでは、Windows Server 2016 を実行する仮想マシンを稼働させます。</span><span class="sxs-lookup"><span data-stu-id="48575-106">Here you'll bring up a virtual machine running Windows Server 2016.</span></span>

<span data-ttu-id="48575-107">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-107">::: zone-end</span></span>

<span data-ttu-id="48575-108">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-108">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="48575-109">役割に関係なく、ほとんどの人は仮想マシンを作成することによってクラウドを使い始めます。</span><span class="sxs-lookup"><span data-stu-id="48575-109">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="48575-110">ここでは、Ubuntu 16.04 を実行する仮想マシンを稼働させます。</span><span class="sxs-lookup"><span data-stu-id="48575-110">Here you'll bring up a virtual machine running Ubuntu 16.04.</span></span>

<span data-ttu-id="48575-111">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-111">::: zone-end</span></span>

<span data-ttu-id="48575-112">Azure で仮想マシンを作成するには多くの方法があります。</span><span class="sxs-lookup"><span data-stu-id="48575-112">There are many ways to create a virtual machine on Azure.</span></span> <span data-ttu-id="48575-113">ここでは、Cloud Shell と呼ばれる対話型ターミナルを使用して、Windows または Linux の仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="48575-113">Here, you'll bring up a Windows or Linux virtual machine using an interactive terminal called Cloud Shell.</span></span> <span data-ttu-id="48575-114">日常的にターミナルから作業していれば、多くの場合はこれが作業を行う最も速い方法であることをご存知でしょう。</span><span class="sxs-lookup"><span data-stu-id="48575-114">If you work from the terminal on a daily basis, you know this is often the fastest way to get the job done.</span></span>

<span data-ttu-id="48575-115">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-115">::: zone pivot="windows-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="48575-116">Linux でよろしいですか、それとも何か新しいものを試したいですか。</span><span class="sxs-lookup"><span data-stu-id="48575-116">Prefer Linux or want to try something new?</span></span> <span data-ttu-id="48575-117">Linux 仮想マシンを実行するには、このページの上部で **Linux** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48575-117">Select **Linux** from the top of this page to run a Linux virtual machine.</span></span>

<span data-ttu-id="48575-118">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-118">::: zone-end</span></span>

<span data-ttu-id="48575-119">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-119">::: zone pivot="linux-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="48575-120">Windows でよろしいですか、それとも何か新しいものを試したいですか。</span><span class="sxs-lookup"><span data-stu-id="48575-120">Prefer Windows or want to try something new?</span></span> <span data-ttu-id="48575-121">Windows Server 仮想マシンを実行するには、このページの上部で **Windows** を選択します。</span><span class="sxs-lookup"><span data-stu-id="48575-121">Select **Windows** from the top of this page to run a Windows Server virtual machine.</span></span>

<span data-ttu-id="48575-122">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-122">::: zone-end</span></span>

<span data-ttu-id="48575-123">いくつかの基本的な用語を確認し、初めての仮想マシンを稼働させてみましょう。</span><span class="sxs-lookup"><span data-stu-id="48575-123">Let's review some basic terms and get your first virtual machine up and running.</span></span>

## <a name="what-is-a-virtual-machine"></a><span data-ttu-id="48575-124">仮想マシンとは</span><span class="sxs-lookup"><span data-stu-id="48575-124">What is a virtual machine?</span></span>

<span data-ttu-id="48575-125">仮想マシン (VM) とは、物理コンピューターのソフトウェア エミュレーションです。</span><span class="sxs-lookup"><span data-stu-id="48575-125">A virtual machine, or VM, is a software emulation of a physical computer.</span></span> <span data-ttu-id="48575-126">VM はソフトウェアとして存在するので、数十、数百、あるいは数千もの Azure VM を分単位で生成し、不要になったら削除することができます。</span><span class="sxs-lookup"><span data-stu-id="48575-126">Because VMs exist as software, dozens, hundreds, or even thousands of Azure VMs can be generated in minutes, then deleted when you don't need them anymore.</span></span> <span data-ttu-id="48575-127">低コストで分単位の課金なので、使用している間だけ、使用したコンピューティング リソースに対してのみ料金を支払います。</span><span class="sxs-lookup"><span data-stu-id="48575-127">With low-cost, per-minute billing, you pay only for the compute resources you use, for as long as you are using them.</span></span> <span data-ttu-id="48575-128">さらに、ニーズに合わせて VM を構成する多くの方法があります。</span><span class="sxs-lookup"><span data-stu-id="48575-128">Plus, there are many ways to configure the VMs to fit your needs.</span></span>

<span data-ttu-id="48575-129">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-129">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="48575-130">実行中の VM のスナップショットは、"_イメージ_" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="48575-130">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="48575-131">Azure では、Windows および Linux の複数のエディションに対するイメージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="48575-131">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="48575-132">独自の事前構成済みイメージを作成し、デプロイの時間を短縮することもできます。</span><span class="sxs-lookup"><span data-stu-id="48575-132">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="48575-133">ここでは、Microsoft によって提供される Windows Server 2016 VM を稼働させます。</span><span class="sxs-lookup"><span data-stu-id="48575-133">Here you'll bring up a Windows Server 2016 VM, provided by Microsoft.</span></span>

<span data-ttu-id="48575-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-134">::: zone-end</span></span>

<span data-ttu-id="48575-135">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-135">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="48575-136">実行中の VM のスナップショットは、"_イメージ_" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="48575-136">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="48575-137">Azure では、Windows および Linux の複数のエディションに対するイメージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="48575-137">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="48575-138">独自の事前構成済みイメージを作成し、デプロイの時間を短縮することもできます。</span><span class="sxs-lookup"><span data-stu-id="48575-138">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="48575-139">ここでは、Canonical によって提供される Ubuntu 16.04 VM を稼働させます。</span><span class="sxs-lookup"><span data-stu-id="48575-139">Here you'll bring up an Ubuntu 16.04 VM, provided by Canonical.</span></span>

<span data-ttu-id="48575-140">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-140">::: zone-end</span></span>

## <a name="what-defines-a-virtual-machine-on-azure"></a><span data-ttu-id="48575-141">Azure 上の仮想マシンを定義するもの</span><span class="sxs-lookup"><span data-stu-id="48575-141">What defines a virtual machine on Azure?</span></span>

<span data-ttu-id="48575-142">仮想マシンは、サイズや場所など、さまざまな要素によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="48575-142">A virtual machine is defined by a number of factors, including its size and location.</span></span> <span data-ttu-id="48575-143">VM を稼働させる前に、関係するものを簡単に説明しておきます。</span><span class="sxs-lookup"><span data-stu-id="48575-143">Before you bring up your VM, let's briefly cover what's involved.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="48575-144">**サイズ**</span><span class="sxs-lookup"><span data-stu-id="48575-144">**Size**</span></span>
    :::column-end:::
    <span data-ttu-id="48575-145">:::column span="3"::: VM の "_サイズ_" によって、プロセッサの速度、メモリの量、ストレージの初期量、および予想されるネットワーク帯域幅が定義されます。</span><span class="sxs-lookup"><span data-stu-id="48575-145">:::column span="3"::: A VM's _size_ defines its processor speed, amount of memory, initial amount of storage, and expected network bandwidth.</span></span> <span data-ttu-id="48575-146">一部のサイズには、大量のグラフィックス レンダリングやビデオ編集用の GPU などの特殊なハードウェアも含まれます。</span><span class="sxs-lookup"><span data-stu-id="48575-146">Some sizes even include specialized hardware such as GPUs for heavy graphics rendering and video editing.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="48575-147">**リージョン**</span><span class="sxs-lookup"><span data-stu-id="48575-147">**Region**</span></span>
    :::column-end:::
    <span data-ttu-id="48575-148">:::column span="3"::: Azure は、世界中に分散するデータ センターによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="48575-148">:::column span="3"::: Azure is made up of data centers distributed throughout the world.</span></span> <span data-ttu-id="48575-149">"_リージョン_" とは、名前付きの地理的な場所における Azure データ センターのセットです。</span><span class="sxs-lookup"><span data-stu-id="48575-149">A _region_ is a set of Azure data centers in a named geographic location.</span></span> <span data-ttu-id="48575-150">仮想マシンを含むすべての Azure リソースには、リージョンが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="48575-150">Every Azure resource, including virtual machines, is assigned a region.</span></span> <span data-ttu-id="48575-151">米国東部や北ヨーロッパなどがリージョンの例です。</span><span class="sxs-lookup"><span data-stu-id="48575-151">East US and North Europe are examples of regions.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="48575-152">**ネットワーク**</span><span class="sxs-lookup"><span data-stu-id="48575-152">**Network**</span></span>
    :::column-end:::
    <span data-ttu-id="48575-153">:::column span="3"::: "_仮想ネットワーク_" とは、Azure 上で論理的に分離されたネットワークのことです。</span><span class="sxs-lookup"><span data-stu-id="48575-153">:::column span="3"::: A _virtual network_ is a logically isolated network on Azure.</span></span> <span data-ttu-id="48575-154">Azure 上の各仮想マシンには、仮想ネットワークが関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="48575-154">Each virtual machine on Azure is associated with a virtual network.</span></span> <span data-ttu-id="48575-155">Azure では、"_ネットワーク セキュリティ グループ_" と呼ばれる仮想ネットワークに対して、クラウド レベルのファイアウォールが提供されます。</span><span class="sxs-lookup"><span data-stu-id="48575-155">Azure provides cloud-level firewalls for your virtual networks called _network security groups_.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="48575-156">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="48575-156">**Resource groups**</span></span>
    :::column-end:::
    <span data-ttu-id="48575-157">:::column span="3"::: 仮想マシンと他のクラウド リソースは、"_リソース グループ_" と呼ばれる論理的なコンテナーにグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="48575-157">:::column span="3"::: Virtual machines and other cloud resources are grouped into logical containers called _resource groups_.</span></span> <span data-ttu-id="48575-158">グループは、通常、アプリケーションまたはサービスの一部としてまとめてデプロイされるリソースの整理に使用されます。</span><span class="sxs-lookup"><span data-stu-id="48575-158">Groups are typically used to organize sets of resources that are deployed together as part of an application or service.</span></span> <span data-ttu-id="48575-159">リソース グループを参照するにはその名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="48575-159">You refer to a resource group by its name.</span></span>
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="48575-160">Azure Cloud Shell とは</span><span class="sxs-lookup"><span data-stu-id="48575-160">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="48575-161">Azure Cloud Shell とは、Azure リソースを管理および開発するための、ブラウザー ベースのコマンド ライン エクスペリエンスです。</span><span class="sxs-lookup"><span data-stu-id="48575-161">Azure Cloud Shell is a browser-based command-line experience for managing and developing Azure resources.</span></span> <span data-ttu-id="48575-162">Cloud Shell は、クラウド上で動作する対話型コンソールと考えてください。</span><span class="sxs-lookup"><span data-stu-id="48575-162">Think of Cloud Shell as an interactive console that you run in the cloud.</span></span>

<span data-ttu-id="48575-163">Cloud Shell では、2 つのエクスペリエンス Bash と PowerShell のどちらかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="48575-163">Cloud Shell provides two experiences to choose from: Bash and PowerShell.</span></span> <span data-ttu-id="48575-164">どちらからも、Azure 用のコマンド ライン インターフェイスである Azure CLI にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="48575-164">Both include access to the Azure CLI, the command-line interface for Azure.</span></span>

<span data-ttu-id="48575-165">Azure portal、Azure CLI、Azure PowerShell などの Azure 管理インターフェイスを使用して、すべての種類の VM を管理できます。</span><span class="sxs-lookup"><span data-stu-id="48575-165">You can use any Azure management interface, including the Azure portal, Azure CLI, and Azure PowerShell, to manage any kind of VM.</span></span> <span data-ttu-id="48575-166">学習のため、ここでは Azure CLI を使用して、Windows VM または Linux VM の作成と管理を行います。</span><span class="sxs-lookup"><span data-stu-id="48575-166">For learning purposes, here you'll use the Azure CLI to create and manage either a Windows or Linux VM.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="creating-resources-in-azure"></a><span data-ttu-id="48575-167">Azure でのリソースの作成</span><span class="sxs-lookup"><span data-stu-id="48575-167">Creating resources in Azure</span></span>

<span data-ttu-id="48575-168">通常は、まず、作成する必要があるすべてのものを保持する "_リソース グループ_" を作成します。</span><span class="sxs-lookup"><span data-stu-id="48575-168">Normally, the first thing we'd do is to create a _resource group_ to hold all the things that we need to create.</span></span> <span data-ttu-id="48575-169">これにより、ソリューションを構成する VM、ディスク、ネットワーク インターフェイスなどのすべての要素を 1 つのユニットとして管理できます。</span><span class="sxs-lookup"><span data-stu-id="48575-169">This allows us to administer all the VMs, disks, network interfaces, and other elements that make up our solution as a unit.</span></span> <span data-ttu-id="48575-170">リソース グループは、Azure CLI で `az group create` コマンドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="48575-170">We can use the Azure CLI to create a resource group with the `az group create` command.</span></span> <span data-ttu-id="48575-171">このコマンドには、リソース グループにサブスクリプション内で一意の名前を付ける `--name` と、リソースを既定で配置する場所を Azure に指示する `--location` が必要です。</span><span class="sxs-lookup"><span data-stu-id="48575-171">It takes a `--name` to give it a unique name in our subscription, and a `--location` to tell Azure what area of the world we want the resources to be located by default.</span></span>

<span data-ttu-id="48575-172">ここでは無料の Azure サンドボックス環境を使用しているので、この手順を実行する必要はありません。代わりに、作成済みのリソース グループ **<rgn>[リソース グループ名]</rgn>**  を使用します。</span><span class="sxs-lookup"><span data-stu-id="48575-172">Since we are in the free Azure Sandbox environment, you don't need to do this step, instead, you will use the pre-created resource group **<rgn>[Resource Group Name]</rgn>**.</span></span>

## <a name="choosing-a-location"></a><span data-ttu-id="48575-173">場所の選択</span><span class="sxs-lookup"><span data-stu-id="48575-173">Choosing a location</span></span>

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="48575-174">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-174">::: zone pivot="windows-cloud"</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="48575-175">Windows VM を作成する</span><span class="sxs-lookup"><span data-stu-id="48575-175">Create a Windows VM</span></span>

<span data-ttu-id="48575-176">Windows VM を稼働させてみましょう。</span><span class="sxs-lookup"><span data-stu-id="48575-176">Let's get your Windows VM up and running.</span></span>

1. <span data-ttu-id="48575-177">このページの右側にある Cloud Shell で、次の `az vm create` コマンドを実行して VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="48575-177">In the Cloud Shell on the right side of this page, run the following `az vm create` command to create your VM.</span></span> <span data-ttu-id="48575-178">このコマンドでは "米国東部" に VM が作成されますが、これを上記のいずれかの場所に変更できます。最寄りの場所を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="48575-178">The command creates the VM in the "East US" location, you can change that to any of the locations listed above &mdash; we recommend you select one close to you.</span></span>

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username azureuser
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    <span data-ttu-id="48575-179">コマンドによって Windows 管理者パスワードを指定できますが、ここではパスワードの入力を求められます。</span><span class="sxs-lookup"><span data-stu-id="48575-179">Although you can specify the Windows admin password through the command, here you'll be prompted to enter one.</span></span> <span data-ttu-id="48575-180">大文字と小文字の英字、数字、記号を組み合わせた 12 文字以上のパスワードを選択します。</span><span class="sxs-lookup"><span data-stu-id="48575-180">Choose a password that contains at least 12 characters with a combination of upper and lowercase letters, numbers, and symbols.</span></span> <span data-ttu-id="48575-181">他の場所で使用しているパスワードは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="48575-181">Don't use a password you use elsewhere.</span></span>

    <span data-ttu-id="48575-182">VM の作成には 4 から 5 分かかります。</span><span class="sxs-lookup"><span data-stu-id="48575-182">Your VM will take four to five minutes to come up.</span></span> <span data-ttu-id="48575-183">それを、データ センター内のシステムの購入、ラック設置、構成にかかる時間と比べてみてください。</span><span class="sxs-lookup"><span data-stu-id="48575-183">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="48575-184">まったく違います。</span><span class="sxs-lookup"><span data-stu-id="48575-184">Quite a difference!</span></span>

<span data-ttu-id="48575-185">待っている間に、先ほど実行したコマンドを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="48575-185">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="48575-186">VM の名前は **myVM** です。</span><span class="sxs-lookup"><span data-stu-id="48575-186">The VM is named **myVM**.</span></span> <span data-ttu-id="48575-187">この名前によって Azure 内で VM が識別されます。</span><span class="sxs-lookup"><span data-stu-id="48575-187">This name identifies the VM in Azure.</span></span> <span data-ttu-id="48575-188">これは、VM の内部ホスト名 (コンピューター名) にもなります。</span><span class="sxs-lookup"><span data-stu-id="48575-188">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="48575-189">リソース グループ (VM の論理的なコンテナー) の名前は **<rgn>[サンドボックス リソース グループ名]</rgn>** です。</span><span class="sxs-lookup"><span data-stu-id="48575-189">The resource group, or the VM's logical container, is named **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
* <span data-ttu-id="48575-190">**Win2016Datacenter** は、Windows Server 2016 VM イメージを示します。</span><span class="sxs-lookup"><span data-stu-id="48575-190">**Win2016Datacenter** specifies the Windows Server 2016 VM image.</span></span>
* <span data-ttu-id="48575-191">**Standard_DS2_v2** は、VM のサイズを示します。</span><span class="sxs-lookup"><span data-stu-id="48575-191">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="48575-192">このサイズには、2 つの仮想 CPU と 7 GB のメモリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="48575-192">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="48575-193">ユーザー名とパスワードを使用して、後で VM に接続できます。</span><span class="sxs-lookup"><span data-stu-id="48575-193">The username and password enable you to connect to your VM later.</span></span> <span data-ttu-id="48575-194">たとえば、リモート デスクトップまたは WinRM 経由で接続し、システムを使用したり構成したりできます。</span><span class="sxs-lookup"><span data-stu-id="48575-194">For example, you can connect over Remote Desktop or WinRM to work with and configure the system.</span></span>

<span data-ttu-id="48575-195">既定では、VM にはパブリック IP アドレスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="48575-195">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="48575-196">インターネットからアクセスできるように、または内部ネットワークからのみアクセスできるように、VM を構成できます。</span><span class="sxs-lookup"><span data-stu-id="48575-196">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="48575-197">この短いビデオで、VM の作成と管理に使用できるオプションについて確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="48575-197">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

#### <a name="options-to-create-and-manage-vms"></a><span data-ttu-id="48575-198">VM を作成および管理するためのオプション</span><span class="sxs-lookup"><span data-stu-id="48575-198">Options to create and manage VMs</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="48575-199">VM の準備ができると、VM に関する情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48575-199">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="48575-200">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="48575-200">Here's an example.</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

<span data-ttu-id="48575-201">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-201">::: zone-end</span></span>

<span data-ttu-id="48575-202">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="48575-202">::: zone pivot="linux-cloud"</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="48575-203">Linux VM を作成する</span><span class="sxs-lookup"><span data-stu-id="48575-203">Create a Linux VM</span></span>

<span data-ttu-id="48575-204">Linux VM を稼働させてみましょう。</span><span class="sxs-lookup"><span data-stu-id="48575-204">Let's get your Linux VM up and running.</span></span>

1. <span data-ttu-id="48575-205">このページの右側にある Cloud Shell で、`az vm create` コマンドを実行して VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="48575-205">From Cloud Shell on the right side of this page, run the `az vm create` command to create your VM.</span></span> <span data-ttu-id="48575-206">次のコマンドでは "米国東部" に VM が作成されますが、これを上記のいずれかの場所に変更できます。最寄りの場所を選択することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="48575-206">The following command creates the VM in the "East US" location, you can change that to any of the locations listed above - we recommend you select one close to you.</span></span>

    ```azurecli
    az vm create \
      --name myVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --location eastus \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    <span data-ttu-id="48575-207">VM の作成には約 2 分かかります。</span><span class="sxs-lookup"><span data-stu-id="48575-207">Your VM will take about two minutes to come up.</span></span> <span data-ttu-id="48575-208">それを、データ センター内のシステムの購入、ラック設置、構成にかかる時間と比べてみてください。</span><span class="sxs-lookup"><span data-stu-id="48575-208">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="48575-209">まったく違います。</span><span class="sxs-lookup"><span data-stu-id="48575-209">Quite a difference!</span></span>

<span data-ttu-id="48575-210">待っている間に、先ほど実行したコマンドを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="48575-210">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="48575-211">VM の名前は **myVM** です。</span><span class="sxs-lookup"><span data-stu-id="48575-211">The VM is named **myVM**.</span></span> <span data-ttu-id="48575-212">この名前によって Azure 内で VM が識別されます。</span><span class="sxs-lookup"><span data-stu-id="48575-212">This name identifies the VM in Azure.</span></span> <span data-ttu-id="48575-213">これは、VM の内部ホスト名 (コンピューター名) にもなります。</span><span class="sxs-lookup"><span data-stu-id="48575-213">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="48575-214">リソース グループ (VM の論理的なコンテナー) の名前は **<rgn>[サンドボックス リソース グループ名]</rgn>** です。</span><span class="sxs-lookup"><span data-stu-id="48575-214">The resource group, or the VM's logical container, is named **<rgn>[Sandbox resource group name]</rgn>**.</span></span>
* <span data-ttu-id="48575-215">**UbuntuLTS** は、Ubuntu 16.04 LTS VM イメージを示します。</span><span class="sxs-lookup"><span data-stu-id="48575-215">**UbuntuLTS** specifies the Ubuntu 16.04 LTS VM image.</span></span>
* <span data-ttu-id="48575-216">**Standard_DS2_v2** は、VM のサイズを示します。</span><span class="sxs-lookup"><span data-stu-id="48575-216">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="48575-217">このサイズには、2 つの仮想 CPU と 7 GB のメモリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="48575-217">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="48575-218">`--generate-ssh-keys` オプションでは、VM にログインできるようにするための SSH キー ペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="48575-218">The `--generate-ssh-keys` option creates an SSH key pair to enable you to log in to the VM.</span></span>

<span data-ttu-id="48575-219">既定では、VM にはパブリック IP アドレスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="48575-219">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="48575-220">インターネットからアクセスできるように、または内部ネットワークからのみアクセスできるように、VM を構成できます。</span><span class="sxs-lookup"><span data-stu-id="48575-220">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="48575-221">この短いビデオで、VM の作成と管理に使用できるオプションについて確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="48575-221">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

#### <a name="options-to-create-and-manage-vms"></a><span data-ttu-id="48575-222">VM を作成および管理するためのオプション</span><span class="sxs-lookup"><span data-stu-id="48575-222">Options to create and manage VMs</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="48575-223">VM の準備ができると、VM に関する情報が表示されます。</span><span class="sxs-lookup"><span data-stu-id="48575-223">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="48575-224">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="48575-224">Here's an example.</span></span>

```json
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

<span data-ttu-id="48575-225">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="48575-225">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="48575-226">まとめ</span><span class="sxs-lookup"><span data-stu-id="48575-226">Summary</span></span>

<span data-ttu-id="48575-227">いくつかの概念を理解するだけで、わずか数分で Azure 上で VM を稼働させることができます。</span><span class="sxs-lookup"><span data-stu-id="48575-227">With just a few concepts under your belt, you're able to spin up a VM on Azure in just a few minutes.</span></span> <span data-ttu-id="48575-228">VM のサイズやファイアウォール規則などのこれらの概念の多くは、既に見慣れているはずです。</span><span class="sxs-lookup"><span data-stu-id="48575-228">Many of these concepts, such as a VM's size and firewall rules, are likely familiar to you already.</span></span>

<span data-ttu-id="48575-229">次に、VM に Web サーバーをインストールし、基本的な Web サイトを提供するように Web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="48575-229">Next, you'll install a web server on your VM and configure your web server to serve up a basic web site.</span></span>