<span data-ttu-id="cdd8e-101">Web サーバーは稼働するようになりましたが、エクスペリエンスをユーザーの役に立つものにするにはさらにコンピューティング能力が必要です。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-101">Your web server is up and running, but you realize you need more computing power to make the experience great for your users.</span></span> <span data-ttu-id="cdd8e-102">どうすれば VM をもっと速く実行させることができますか。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-102">How can you make your VM run faster?</span></span>

<span data-ttu-id="cdd8e-103">自社のデータ センターであれば、Web サーバーをいっそう強力なハードウェアに移動することで、パフォーマンスの問題を解決できるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-103">In your data center, you might move your web server to more powerful hardware to solve performance problems.</span></span> <span data-ttu-id="cdd8e-104">問題は、新しいシステムを購入し、ラックに格納し、電源を供給する必要があることです。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-104">The problem is you need to buy, rack, and power your new system.</span></span> <span data-ttu-id="cdd8e-105">Azure なら答えははるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-105">With Azure, the answer is much simpler.</span></span>

<span data-ttu-id="cdd8e-106">より強力なサイズに VM をスケールアップする前に、まずはスケーリングの意味を定義してみましょう。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-106">Before you scale up your VM to a more powerful size, let's first define what scale means.</span></span>

## <a name="what-is-scale"></a><span data-ttu-id="cdd8e-107">スケーリングとは</span><span class="sxs-lookup"><span data-stu-id="cdd8e-107">What is scale?</span></span>

<span data-ttu-id="cdd8e-108">"_スケーリング_" とは、よりよいパフォーマンスを実現するために、ネットワーク帯域幅、メモリ、ストレージ、またはコンピューティング能力を追加することです。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-108">_Scale_ refers to adding network bandwidth, memory, storage, or compute power to achieve better performance.</span></span>  

<span data-ttu-id="cdd8e-109">"_スケールアップ_" や "_スケールアウト_" という用語を聞いたことがあるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-109">You may have heard the terms _scaling up_ and _scaling out_.</span></span>

<span data-ttu-id="cdd8e-110">スケールアップまたは垂直スケーリングとは、既存の仮想マシンのメモリ、ストレージ、またはコンピューティング能力を増やすことです。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-110">Scaling up, or vertical scaling, means to increase the memory, storage, or compute power on an existing virtual machine.</span></span> <span data-ttu-id="cdd8e-111">たとえば、Web サーバーやデータベース サーバーにメモリを追加して、さらに高速で実行させることができます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-111">For example, you can add additional memory to a web or database server to make it run faster.</span></span>

<span data-ttu-id="cdd8e-112">スケールアウトまたは水平スケーリングとは、仮想マシンを追加してアプリケーションの能力を高めることです。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-112">Scaling out, or horizontal scaling, means to additional virtual machines to power your application.</span></span> <span data-ttu-id="cdd8e-113">たとえば、まったく同じ構成方法で多数の仮想マシンを作成し、ロード バランサーを使用してそれらの間に処理を分散させることができます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-113">For example, you might create many virtual machines configured in exactly the same way and use a load balancer to distribute work across them.</span></span>

> [!TIP]
> <span data-ttu-id="cdd8e-114">クラウドには柔軟性があります。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-114">The cloud is elastic.</span></span> <span data-ttu-id="cdd8e-115">スケールアップまたはスケールアウトが一時的にのみ必要であった場合は、展開を "_スケールダウン_" または "_スケールイン_" できます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-115">You can _scale down_ or _scale in_ your deployment if you needed to scale up or scale out only temporarily.</span></span> <span data-ttu-id="cdd8e-116">スケールダウンまたはスケールインはコストの削減に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-116">Scaling down or scaling in can help you save money.</span></span><br><br><span data-ttu-id="cdd8e-117">**Azure Advisor** と **Azure Cost Management** の 2 つのサービスを使用して、クラウド支出を最適化できます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-117">**Azure Advisor** and **Azure Cost Management** are two services that help you optimize cloud spend.</span></span> <span data-ttu-id="cdd8e-118">これらのサービスを利用して必要以上に使用している部分を識別し、実際に使用している量にスケールを戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-118">You can use these services to identify where you're using more than you need, and then scale back to the capacity you're actually using.</span></span>

## <a name="scale-up-your-vm"></a><span data-ttu-id="cdd8e-119">VM をスケールアップする</span><span class="sxs-lookup"><span data-stu-id="cdd8e-119">Scale up your VM</span></span>

<span data-ttu-id="cdd8e-120">VM を作成するときに、サイズ **Standard_DS2_v2** を指定したことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-120">Recall that you specified the size **Standard_DS2_v2** when you created your VM.</span></span> <span data-ttu-id="cdd8e-121">現在、VM は 2 つの仮想 CPU と 7 GB のメモリを備えています。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-121">Your VM currently has two virtual CPUs and 7 GB of memory.</span></span>

<span data-ttu-id="cdd8e-122">1 つ上のサイズ **Standard_DS3_v2** に増強してみましょう。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-122">Let's bump up to the next size, **Standard_DS3_v2**.</span></span> <span data-ttu-id="cdd8e-123">VM は 4 つの仮想 CPU と 14 GB のメモリを備えるようになります。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-123">Your VM will then have four virtual CPUs and 14 GB of memory.</span></span>

<span data-ttu-id="cdd8e-124">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="cdd8e-124">::: zone pivot="windows-cloud"</span></span>

1. <span data-ttu-id="cdd8e-125">Cloud Shell から `az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-125">From Cloud Shell, run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="cdd8e-126">更新プロセスには約 1 分かかります。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-126">The update process takes about a minute.</span></span> <span data-ttu-id="cdd8e-127">この処理中に VM が再起動します。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-127">Your VM restarts during the process.</span></span>

1. <span data-ttu-id="cdd8e-128">`az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-128">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="cdd8e-129">新しい VM サイズ **Standard_DS3_v2** が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-129">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```output
    Standard_DS3_v2
    ```

<span data-ttu-id="cdd8e-130">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="cdd8e-130">::: zone-end</span></span>

<span data-ttu-id="cdd8e-131">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="cdd8e-131">::: zone pivot="linux-cloud"</span></span>

1. <span data-ttu-id="cdd8e-132">Cloud Shell から `az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-132">From Cloud Shell, run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="cdd8e-133">更新プロセスには約 1 分かかります。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-133">The update process takes about a minute.</span></span> <span data-ttu-id="cdd8e-134">この処理中に VM が再起動します。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-134">Your VM restarts during the process.</span></span>

1. <span data-ttu-id="cdd8e-135">`az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-135">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="cdd8e-136">新しい VM サイズ **Standard_DS3_v2** が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-136">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```output
    Standard_DS3_v2
    ```

<span data-ttu-id="cdd8e-137">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="cdd8e-137">::: zone-end</span></span>

## <a name="summary"></a><span data-ttu-id="cdd8e-138">まとめ</span><span class="sxs-lookup"><span data-stu-id="cdd8e-138">Summary</span></span>

<span data-ttu-id="cdd8e-139">ごくろうさま。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-139">Nice job!</span></span> <span data-ttu-id="cdd8e-140">ほんの 1 コマンドで、VM の能力が 2 倍になりました。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-140">With just one command, your VM is now twice as powerful.</span></span>

<span data-ttu-id="cdd8e-141">スケールアップとスケールアウトは、パフォーマンスを向上させる 2 つの方法です。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-141">Scaling up and scaling out are two ways to increase performance.</span></span> <span data-ttu-id="cdd8e-142">ここでは、VM をスケールアップしてそのコンピューティング能力を増強しました。</span><span class="sxs-lookup"><span data-stu-id="cdd8e-142">Here you scaled up your VM to increase its compute power.</span></span>