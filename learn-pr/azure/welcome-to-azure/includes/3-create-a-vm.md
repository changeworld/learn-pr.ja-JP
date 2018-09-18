<span data-ttu-id="0181c-101">あなたが技術の専門家なら、おそらく特定の分野についての専門知識をお持ちでしょう。</span><span class="sxs-lookup"><span data-stu-id="0181c-101">As a technology professional, you likely have expertise in a specific area.</span></span> <span data-ttu-id="0181c-102">ストレージの管理者か仮想化のエキスパート、それとも最新のセキュリティ プラクティスの専門家でしょうか。</span><span class="sxs-lookup"><span data-stu-id="0181c-102">Perhaps you're a storage admin or virtualization expert, or maybe you focus on the latest security practices.</span></span> <span data-ttu-id="0181c-103">あなたが学生なら、最も興味を引かれるものをまだ探しているところかもしれません。</span><span class="sxs-lookup"><span data-stu-id="0181c-103">If you're a student, you may still be exploring what interests you most.</span></span>

<span data-ttu-id="0181c-104">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-104">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="0181c-105">役割に関係なく、ほとんどの人は仮想マシンを作成することによってクラウドを使い始めます。</span><span class="sxs-lookup"><span data-stu-id="0181c-105">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="0181c-106">ここでは、Windows Server 2016 を実行する仮想マシンを稼働させます。</span><span class="sxs-lookup"><span data-stu-id="0181c-106">Here you'll bring up a virtual machine running Windows Server 2016.</span></span>

<span data-ttu-id="0181c-107">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-107">::: zone-end</span></span>

<span data-ttu-id="0181c-108">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-108">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="0181c-109">役割に関係なく、ほとんどの人は仮想マシンを作成することによってクラウドを使い始めます。</span><span class="sxs-lookup"><span data-stu-id="0181c-109">No matter your role, most people get started with the cloud by creating a virtual machine.</span></span> <span data-ttu-id="0181c-110">ここでは、Ubuntu 16.04 を実行する仮想マシンを稼働させます。</span><span class="sxs-lookup"><span data-stu-id="0181c-110">Here you'll bring up a virtual machine running Ubuntu 16.04.</span></span>

<span data-ttu-id="0181c-111">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-111">::: zone-end</span></span>

<span data-ttu-id="0181c-112">Azure で仮想マシンを作成するには多くの方法があります。</span><span class="sxs-lookup"><span data-stu-id="0181c-112">There are many ways to create a virtual machine on Azure.</span></span> <span data-ttu-id="0181c-113">ここでは、Cloud Shell と呼ばれる対話型ターミナルを使用して、Windows または Linux の仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="0181c-113">Here, you'll bring up a Windows or Linux virtual machine using an interactive terminal called Cloud Shell.</span></span> <span data-ttu-id="0181c-114">日常的にターミナルから作業していれば、多くの場合はこれが作業を行う最も速い方法であることをご存知でしょう。</span><span class="sxs-lookup"><span data-stu-id="0181c-114">If you work from the terminal on a daily basis, you know this is often the fastest way to get the job done.</span></span>

<span data-ttu-id="0181c-115">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-115">::: zone pivot="windows-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="0181c-116">Linux でよろしいですか、それとも何か新しいものを試したいですか。</span><span class="sxs-lookup"><span data-stu-id="0181c-116">Prefer Linux or want to try something new?</span></span> <span data-ttu-id="0181c-117">Linux 仮想マシンを実行するには、このページの上部で **Linux** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0181c-117">Select **Linux** from the top of this page to run a Linux virtual machine.</span></span>

<span data-ttu-id="0181c-118">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-118">::: zone-end</span></span>

<span data-ttu-id="0181c-119">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-119">::: zone pivot="linux-cloud"</span></span>

> [!TIP]
> <span data-ttu-id="0181c-120">Windows でよろしいですか、それとも何か新しいものを試したいですか。</span><span class="sxs-lookup"><span data-stu-id="0181c-120">Prefer Windows or want to try something new?</span></span> <span data-ttu-id="0181c-121">Windows Server 仮想マシンを実行するには、このページの上部で **Windows** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0181c-121">Select **Windows** from the top of this page to run a Windows Server virtual machine.</span></span>

<span data-ttu-id="0181c-122">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-122">::: zone-end</span></span>

<span data-ttu-id="0181c-123">いくつかの基本的な用語を確認し、初めての仮想マシンを稼働させてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0181c-123">Let's review some basic terms and get your first virtual machine up and running.</span></span>

## <a name="what-is-a-virtual-machine"></a><span data-ttu-id="0181c-124">仮想マシンとは</span><span class="sxs-lookup"><span data-stu-id="0181c-124">What is a virtual machine?</span></span>

<span data-ttu-id="0181c-125">仮想マシン (VM) とは、物理コンピューターのソフトウェア エミュレーションです。</span><span class="sxs-lookup"><span data-stu-id="0181c-125">A virtual machine, or VM, is a software emulation of a physical computer.</span></span> <span data-ttu-id="0181c-126">VM はソフトウェアとして存在するので、数十、数百、あるいは数千もの Azure VM を分単位で生成し、不要になったら削除することができます。</span><span class="sxs-lookup"><span data-stu-id="0181c-126">Because VMs exist as software, dozens, hundreds, or even thousands of Azure VMs can be generated in minutes, then deleted when you don't need them anymore.</span></span> <span data-ttu-id="0181c-127">低コストで分単位の課金なので、使用している間だけ、使用したコンピューティング リソースに対してのみ料金を支払います。</span><span class="sxs-lookup"><span data-stu-id="0181c-127">With low-cost, per-minute billing, you pay only for the compute resources you use, for as long as you are using them.</span></span> <span data-ttu-id="0181c-128">さらに、ニーズに合わせて VM を構成する多くの方法があります。</span><span class="sxs-lookup"><span data-stu-id="0181c-128">Plus, there are many ways to configure the VMs to fit your needs.</span></span>

<span data-ttu-id="0181c-129">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-129">::: zone pivot="windows-cloud"</span></span>

<span data-ttu-id="0181c-130">実行中の VM のスナップショットは、"_イメージ_" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="0181c-130">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="0181c-131">Azure では、Windows および Linux の複数のエディションに対するイメージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-131">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="0181c-132">独自の事前構成済みイメージを作成し、デプロイの時間を短縮することもできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-132">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="0181c-133">ここでは、Microsoft によって提供される Windows Server 2016 VM を稼働させます。</span><span class="sxs-lookup"><span data-stu-id="0181c-133">Here you'll bring up a Windows Server 2016 VM, provided by Microsoft.</span></span>

<span data-ttu-id="0181c-134">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-134">::: zone-end</span></span>

<span data-ttu-id="0181c-135">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-135">::: zone pivot="linux-cloud"</span></span>

<span data-ttu-id="0181c-136">実行中の VM のスナップショットは、"_イメージ_" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="0181c-136">A snapshot of a running VM is called an _image_.</span></span> <span data-ttu-id="0181c-137">Azure では、Windows および Linux の複数のエディションに対するイメージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-137">Azure provides images for Windows and several flavors of Linux.</span></span> <span data-ttu-id="0181c-138">独自の事前構成済みイメージを作成し、デプロイの時間を短縮することもできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-138">You can also create your own preconfigured images to make deployments go faster.</span></span> <span data-ttu-id="0181c-139">ここでは、Canonical によって提供される Ubuntu 16.04 VM を稼働させます。</span><span class="sxs-lookup"><span data-stu-id="0181c-139">Here you'll bring up an Ubuntu 16.04 VM, provided by Canonical.</span></span>

<span data-ttu-id="0181c-140">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-140">::: zone-end</span></span>

## <a name="what-defines-a-virtual-machine-on-azure"></a><span data-ttu-id="0181c-141">Azure 上の仮想マシンを定義するもの</span><span class="sxs-lookup"><span data-stu-id="0181c-141">What defines a virtual machine on Azure?</span></span>

<span data-ttu-id="0181c-142">仮想マシンは、サイズや場所など、さまざまな要素によって定義されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-142">A virtual machine is defined by a number of factors, including its size and location.</span></span> <span data-ttu-id="0181c-143">VM を稼働させる前に、関係するものを簡単に説明しておきます。</span><span class="sxs-lookup"><span data-stu-id="0181c-143">Before you bring up your VM, let's briefly cover what's involved.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="0181c-144">**サイズ**</span><span class="sxs-lookup"><span data-stu-id="0181c-144">**Size**</span></span>
    :::column-end:::
    <span data-ttu-id="0181c-145">:::column span="3"::: VM の "_サイズ_" によって、プロセッサの速度、メモリの量、ストレージの初期量、および予想されるネットワーク帯域幅が定義されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-145">:::column span="3"::: A VM's _size_ defines its processor speed, amount of memory, initial amount of storage, and expected network bandwidth.</span></span> <span data-ttu-id="0181c-146">一部のサイズには、大量のグラフィックス レンダリングやビデオ編集用の GPU などの特殊なハードウェアも含まれます。</span><span class="sxs-lookup"><span data-stu-id="0181c-146">Some sizes even include specialized hardware such as GPUs for heavy graphics rendering and video editing.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="0181c-147">**リージョン**</span><span class="sxs-lookup"><span data-stu-id="0181c-147">**Region**</span></span>
    :::column-end:::
    <span data-ttu-id="0181c-148">:::column span="3"::: Azure は、世界中に分散するデータ センターによって構成されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-148">:::column span="3"::: Azure is made up of data centers distributed throughout the world.</span></span> <span data-ttu-id="0181c-149">"_リージョン_" とは、名前付きの地理的な場所における Azure データ センターのセットです。</span><span class="sxs-lookup"><span data-stu-id="0181c-149">A _region_ is a set of Azure data centers in a named geographic location.</span></span> <span data-ttu-id="0181c-150">仮想マシンを含むすべての Azure リソースには、リージョンが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="0181c-150">Every Azure resource, including virtual machines, is assigned a region.</span></span> <span data-ttu-id="0181c-151">米国東部や北ヨーロッパなどがリージョンの例です。</span><span class="sxs-lookup"><span data-stu-id="0181c-151">East US and North Europe are examples of regions.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="0181c-152">**ネットワーク**</span><span class="sxs-lookup"><span data-stu-id="0181c-152">**Network**</span></span>
    :::column-end:::
    <span data-ttu-id="0181c-153">:::column span="3"::: "_仮想ネットワーク_" とは、Azure 上で論理的に分離されたネットワークのことです。</span><span class="sxs-lookup"><span data-stu-id="0181c-153">:::column span="3"::: A _virtual network_ is a logically isolated network on Azure.</span></span> <span data-ttu-id="0181c-154">Azure 上の各仮想マシンには、仮想ネットワークが関連付けられます。</span><span class="sxs-lookup"><span data-stu-id="0181c-154">Each virtual machine on Azure is associated with a virtual network.</span></span> <span data-ttu-id="0181c-155">Azure では、"_ネットワーク セキュリティ グループ_" と呼ばれる仮想ネットワークに対して、クラウド レベルのファイアウォールが提供されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-155">Azure provides cloud-level firewalls for your virtual networks called _network security groups_.</span></span>
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        <span data-ttu-id="0181c-156">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="0181c-156">**Resource groups**</span></span>
    :::column-end:::
    <span data-ttu-id="0181c-157">:::column span="3"::: 仮想マシンと他のクラウド リソースは、"_リソース グループ_" と呼ばれる論理的なコンテナーにグループ化されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-157">:::column span="3"::: Virtual machines and other cloud resources are grouped into logical containers called _resource groups_.</span></span> <span data-ttu-id="0181c-158">グループは、通常、アプリケーションまたはサービスの一部としてまとめてデプロイされるリソースの整理に使用されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-158">Groups are typically used to organize sets of resources that are deployed together as part of an application or service.</span></span> <span data-ttu-id="0181c-159">リソース グループを参照するにはその名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="0181c-159">You refer to a resource group by its name.</span></span>
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a><span data-ttu-id="0181c-160">Azure Cloud Shell とは</span><span class="sxs-lookup"><span data-stu-id="0181c-160">What is Azure Cloud Shell?</span></span>

<span data-ttu-id="0181c-161">Azure Cloud Shell とは、Azure リソースを管理および開発するための、ブラウザー ベースのコマンド ライン エクスペリエンスです。</span><span class="sxs-lookup"><span data-stu-id="0181c-161">Azure Cloud Shell is a browser-based command-line experience for managing and developing Azure resources.</span></span> <span data-ttu-id="0181c-162">Cloud Shell は、クラウド上で動作する対話型コンソールと考えてください。</span><span class="sxs-lookup"><span data-stu-id="0181c-162">Think of Cloud Shell as an interactive console that you run in the cloud.</span></span>

<span data-ttu-id="0181c-163">Cloud Shell では、2 つのエクスペリエンス Bash と PowerShell のどちらかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="0181c-163">Cloud Shell provides two experiences to choose from: Bash and PowerShell.</span></span> <span data-ttu-id="0181c-164">どちらからも、Azure 用のコマンド ライン インターフェイスである Azure CLI にアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-164">Both include access to the Azure CLI, the command-line interface for Azure.</span></span>

<span data-ttu-id="0181c-165">Azure portal、Azure CLI、Azure PowerShell などの Azure 管理インターフェイスを使用して、すべての種類の VM を管理できます。</span><span class="sxs-lookup"><span data-stu-id="0181c-165">You can use any Azure management interface, including the Azure portal, Azure CLI, and Azure PowerShell, to manage any kind of VM.</span></span> <span data-ttu-id="0181c-166">学習が目的なので、ここでは Azure CLI を使用して Windows または Linux の VM の作成と管理を行います。</span><span class="sxs-lookup"><span data-stu-id="0181c-166">For learning purposes, here you'll use the Azure CLI to create and manage either a Windows or Linux VM.</span></span>

<span data-ttu-id="0181c-167">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-167">::: zone pivot="windows-cloud"</span></span>

## <a name="create-a-windows-vm"></a><span data-ttu-id="0181c-168">Windows VM を作成する</span><span class="sxs-lookup"><span data-stu-id="0181c-168">Create a Windows VM</span></span>

<span data-ttu-id="0181c-169">Windows VM を稼働させてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0181c-169">Let's get your Windows VM up and running.</span></span>

1. <span data-ttu-id="0181c-170">このページの右側にある Cloud Shell から `az group create` コマンドを実行し、**myResourceGroup** という名前のリソース グループを米国東部リージョンに作成します。</span><span class="sxs-lookup"><span data-stu-id="0181c-170">From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.</span></span>

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```

1. <span data-ttu-id="0181c-171">`az vm create` コマンドを実行して、VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="0181c-171">Run the `az vm create` command to create your VM.</span></span> <span data-ttu-id="0181c-172">次に示すパスワードを、後で覚えやすいものに変更することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0181c-172">We recommend you change the password shown below to one you'll remember later.</span></span>

      > [!NOTE]
    > <span data-ttu-id="0181c-173">大文字と小文字の英字、数字、記号を組み合わせて 8 文字以上のパスワードを選択します。</span><span class="sxs-lookup"><span data-stu-id="0181c-173">Choose a password that contains at least 8 characters with a combination of upper and lowercase letters, numbers, and symbols.</span></span>

    ```azurecli
    az vm create \
      --name myWindowsVM \
      --resource-group myResourceGroup \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --location eastus \
      --admin-username azureuser \
      --admin-password "Password1234&"
    ```

    > [!TIP]
    > <span data-ttu-id="0181c-174">**[コピー]** ボタンを使用して、各コマンドをコピーできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-174">You can use the **Copy** button to copy each command.</span></span> <span data-ttu-id="0181c-175">コマンドを貼り付けるには、Cloud Shell ウィンドウで新しい行を右クリックし、**[貼り付け]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0181c-175">To paste it, right click on the new line in the Cloud Shell window and select **Paste**.</span></span>

    <span data-ttu-id="0181c-176">VM の作成には 4 から 5 分かかります。</span><span class="sxs-lookup"><span data-stu-id="0181c-176">Your VM will take four to five minutes to come up.</span></span> <span data-ttu-id="0181c-177">それを、データ センター内のシステムの購入、ラック設置、構成にかかる時間と比べてみてください。</span><span class="sxs-lookup"><span data-stu-id="0181c-177">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="0181c-178">まったく違います。</span><span class="sxs-lookup"><span data-stu-id="0181c-178">Quite a difference!</span></span>

<span data-ttu-id="0181c-179">待っている間に、実行したコマンドを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0181c-179">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="0181c-180">VM の名前は **myWindowsVM** です。</span><span class="sxs-lookup"><span data-stu-id="0181c-180">The VM is named **myWindowsVM**.</span></span> <span data-ttu-id="0181c-181">この名前によって Azure 内で VM が識別されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-181">This name identifies the VM in Azure.</span></span> <span data-ttu-id="0181c-182">これはまた、VM の内部ホスト名またはコンピューター名にもなります。</span><span class="sxs-lookup"><span data-stu-id="0181c-182">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="0181c-183">リソース グループつまり VM の論理的なコンテナーの名前は **myResourceGroup** です。</span><span class="sxs-lookup"><span data-stu-id="0181c-183">The resource group, or the VM's logical container, is named **myResourceGroup**.</span></span>
* <span data-ttu-id="0181c-184">**Win2016Datacenter** は、Windows Server 2016 VM イメージを指定します。</span><span class="sxs-lookup"><span data-stu-id="0181c-184">**Win2016Datacenter** specifies the Windows Server 2016 VM image.</span></span>
* <span data-ttu-id="0181c-185">**Standard_DS2_v2** は、VM のサイズを示します。</span><span class="sxs-lookup"><span data-stu-id="0181c-185">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="0181c-186">このサイズには、2 つの仮想 CPU と 7 GB のメモリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0181c-186">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="0181c-187">VM が存在する場所つまりリージョンは、**米国東部**です。</span><span class="sxs-lookup"><span data-stu-id="0181c-187">The VM exists in the **East US** location, or region.</span></span>
* <span data-ttu-id="0181c-188">後でユーザー名とパスワードを使用して、VM に接続できます。</span><span class="sxs-lookup"><span data-stu-id="0181c-188">The username and password enable you to connect to your VM later.</span></span> <span data-ttu-id="0181c-189">たとえば、リモート デスクトップまたは WinRM 経由で接続し、システムを使用したり構成したりできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-189">For example, you can connect over Remote Desktop or WinRM to work with and configure the system.</span></span>

<span data-ttu-id="0181c-190">既定では、VM にはパブリック IP アドレスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="0181c-190">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="0181c-191">インターネットからアクセスできるように、または内部ネットワークからのみアクセスできるように、VM を構成できます。</span><span class="sxs-lookup"><span data-stu-id="0181c-191">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="0181c-192">この短いビデオで、VM の作成と管理に使用できるオプションについて確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-192">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="0181c-193">VM の準備ができたら、それについての情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="0181c-193">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="0181c-194">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0181c-194">Here's an example.</span></span>

```console
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myWindowsVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

<span data-ttu-id="0181c-195">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-195">::: zone-end</span></span>

<span data-ttu-id="0181c-196">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="0181c-196">::: zone pivot="linux-cloud"</span></span>

## <a name="create-a-linux-vm"></a><span data-ttu-id="0181c-197">Linux VM を作成する</span><span class="sxs-lookup"><span data-stu-id="0181c-197">Create a Linux VM</span></span>

<span data-ttu-id="0181c-198">Linux VM を稼働させてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0181c-198">Let's get your Linux VM up and running.</span></span>

1. <span data-ttu-id="0181c-199">このページの右側にある Cloud Shell から `az group create` コマンドを実行し、**myResourceGroup** という名前のリソース グループを米国東部リージョンに作成します。</span><span class="sxs-lookup"><span data-stu-id="0181c-199">From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.</span></span>

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```

1. <span data-ttu-id="0181c-200">`az vm create` コマンドを実行して、VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="0181c-200">Run the `az vm create` command to create your VM.</span></span>

    ```azurecli
    az vm create \
      --name myLinuxVM \
      --resource-group myResourceGroup \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --location eastus \
      --generate-ssh-keys
    ```

    > [!TIP]
    > <span data-ttu-id="0181c-201">**[コピー]** ボタンを使用して、各コマンドをコピーできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-201">You can use the **Copy** button to copy each command.</span></span> <span data-ttu-id="0181c-202">コマンドを貼り付けるには、Cloud Shell ウィンドウで新しい行を右クリックし、**[貼り付け]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0181c-202">To paste it, right click on the new line in the Cloud Shell window and select **Paste**.</span></span>

    <span data-ttu-id="0181c-203">VM の作成には約 2 分かかります。</span><span class="sxs-lookup"><span data-stu-id="0181c-203">Your VM will take about two minutes to come up.</span></span> <span data-ttu-id="0181c-204">それを、データ センター内のシステムの購入、ラック設置、構成にかかる時間と比べてみてください。</span><span class="sxs-lookup"><span data-stu-id="0181c-204">Compare that to the time it takes to purchase, rack, and configure a system in your data center.</span></span> <span data-ttu-id="0181c-205">まったく違います。</span><span class="sxs-lookup"><span data-stu-id="0181c-205">Quite a difference!</span></span>

<span data-ttu-id="0181c-206">待っている間に、実行したコマンドを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0181c-206">While you're waiting, let's review the command you just ran.</span></span>

* <span data-ttu-id="0181c-207">VM の名前は **myLinuxVM** です。</span><span class="sxs-lookup"><span data-stu-id="0181c-207">The VM is named **myLinuxVM**.</span></span> <span data-ttu-id="0181c-208">この名前によって Azure 内で VM が識別されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-208">This name identifies the VM in Azure.</span></span> <span data-ttu-id="0181c-209">これはまた、VM の内部ホスト名またはコンピューター名にもなります。</span><span class="sxs-lookup"><span data-stu-id="0181c-209">It also becomes the VM's internal hostname, or computer name.</span></span>
* <span data-ttu-id="0181c-210">リソース グループつまり VM の論理的なコンテナーの名前は **myResourceGroup** です。</span><span class="sxs-lookup"><span data-stu-id="0181c-210">The resource group, or the VM's logical container, is named **myResourceGroup**.</span></span>
* <span data-ttu-id="0181c-211">**UbuntuLTS** は、Ubuntu 16.04 LTS VM イメージを指定します。</span><span class="sxs-lookup"><span data-stu-id="0181c-211">**UbuntuLTS** specifies the Ubuntu 16.04 LTS VM image.</span></span>
* <span data-ttu-id="0181c-212">**Standard_DS2_v2** は、VM のサイズを示します。</span><span class="sxs-lookup"><span data-stu-id="0181c-212">**Standard_DS2_v2** refers to the size of the VM.</span></span> <span data-ttu-id="0181c-213">このサイズには、2 つの仮想 CPU と 7 GB のメモリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0181c-213">This size has two virtual CPUs and 7 GB of memory.</span></span>
* <span data-ttu-id="0181c-214">VM が存在する場所つまりリージョンは、**米国東部**です。</span><span class="sxs-lookup"><span data-stu-id="0181c-214">The VM exists in the **East US** location, or region.</span></span>
* <span data-ttu-id="0181c-215">`--generate-ssh-keys` オプションでは、VM にログインできる SSH キー ペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="0181c-215">The `--generate-ssh-keys` option creates an SSH key pair to enable you to log in to the VM.</span></span>

<span data-ttu-id="0181c-216">既定では、VM にはパブリック IP アドレスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="0181c-216">By default, Azure assigns a public IP address to your VM.</span></span> <span data-ttu-id="0181c-217">インターネットからアクセスできるように、または内部ネットワークからのみアクセスできるように、VM を構成できます。</span><span class="sxs-lookup"><span data-stu-id="0181c-217">You can configure a VM to be accessible from the Internet or only from the internal network.</span></span>

<span data-ttu-id="0181c-218">この短いビデオで、VM の作成と管理に使用できるオプションについて確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="0181c-218">You can also check out this short video about some of the options you have to create and manage VMs.</span></span>

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

<span data-ttu-id="0181c-219">VM の準備ができたら、それについての情報を表示します。</span><span class="sxs-lookup"><span data-stu-id="0181c-219">When the VM is ready, you see information about it.</span></span> <span data-ttu-id="0181c-220">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="0181c-220">Here's an example.</span></span>

```console
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myLinuxVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

<span data-ttu-id="0181c-221">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="0181c-221">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="0181c-222">まとめ</span><span class="sxs-lookup"><span data-stu-id="0181c-222">Summary</span></span>

<span data-ttu-id="0181c-223">いくつかの概念を理解するだけで、わずか数分で Azure 上で VM を稼働させることができます。</span><span class="sxs-lookup"><span data-stu-id="0181c-223">With just a few concepts under your belt, you're able to spin up a VM on Azure in just a few minutes.</span></span> <span data-ttu-id="0181c-224">VM のサイズやファイアウォール規則などのこれらの概念の多くは、既に見慣れているはずです。</span><span class="sxs-lookup"><span data-stu-id="0181c-224">Many of these concepts, such as a VM's size and firewall rules, are likely familiar to you already.</span></span>

<span data-ttu-id="0181c-225">次に、VM に Web サーバーをインストールし、基本的な Web サイトを提供するように Web サーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="0181c-225">Next, you'll install a web server on your VM and configure your web server to serve up a basic web site.</span></span>