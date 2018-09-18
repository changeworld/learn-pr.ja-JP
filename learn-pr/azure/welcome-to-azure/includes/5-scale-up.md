<span data-ttu-id="f6c81-101">Web サーバーは稼働するようになりましたが、エクスペリエンスをユーザーの役に立つものにするにはさらにコンピューティング能力が必要です。</span><span class="sxs-lookup"><span data-stu-id="f6c81-101">Your web server is up and running, but you realize you need more computing power to make the experience great for your users.</span></span> <span data-ttu-id="f6c81-102">どうすれば VM をもっと速く実行させることができますか。</span><span class="sxs-lookup"><span data-stu-id="f6c81-102">How can you make your VM run faster?</span></span>

<span data-ttu-id="f6c81-103">自社のデータ センターであれば、Web サーバーをいっそう強力なハードウェアに移動することで、パフォーマンスの問題を解決できるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="f6c81-103">In your data center, you might move your web server to more powerful hardware to solve performance problems.</span></span> <span data-ttu-id="f6c81-104">問題は、新しいシステムを購入し、ラックに格納し、電源を供給する必要があることです。</span><span class="sxs-lookup"><span data-stu-id="f6c81-104">The problem is you need to buy, rack, and power your new system.</span></span> <span data-ttu-id="f6c81-105">Azure なら答えははるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="f6c81-105">With Azure, the answer is much simpler.</span></span>

<span data-ttu-id="f6c81-106">そのような場合は、より強力なサイズに VM をスケールアップします。</span><span class="sxs-lookup"><span data-stu-id="f6c81-106">Now you'll scale up your VM to a more powerful size.</span></span> <span data-ttu-id="f6c81-107">VM のサイズを変更する前に、VM をシャットダウンするか、または割り当て解除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-107">Before you change your VM's size, you must shut it down, or deallocate it.</span></span>

<span data-ttu-id="f6c81-108">最初に、スケーリングの意味を定義し、VM の割り当てを解除するとどうなるかを説明します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-108">First, let's define what scale means and what happens when you deallocate your VM.</span></span>

## <a name="what-is-scale"></a><span data-ttu-id="f6c81-109">スケーリングとは</span><span class="sxs-lookup"><span data-stu-id="f6c81-109">What is scale?</span></span>

<span data-ttu-id="f6c81-110">"_スケーリング_" とは、よりよいパフォーマンスを実現するために、ネットワーク帯域幅、メモリ、ストレージ、またはコンピューティング能力を追加することです。</span><span class="sxs-lookup"><span data-stu-id="f6c81-110">_Scale_ refers to adding network bandwidth, memory, storage, or compute power to achieve better performance.</span></span>  

<span data-ttu-id="f6c81-111">"_スケールアップ_" や "_スケールアウト_" という用語を聞いたことがあるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="f6c81-111">You may have heard the terms _scale up_ and _scale out_.</span></span>

<span data-ttu-id="f6c81-112">スケールアップまたは垂直スケーリングとは、既存の仮想マシンのメモリ、ストレージ、またはコンピューティング能力を増やすことです。</span><span class="sxs-lookup"><span data-stu-id="f6c81-112">Scaling up, or vertical scaling, means to increase the memory, storage, or compute power on an existing virtual machine.</span></span> <span data-ttu-id="f6c81-113">たとえば、Web サーバーやデータベース サーバーにメモリを追加して、さらに高速で実行させることができます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-113">For example, you can add additional memory to a web or database server to make it run faster.</span></span>

> [!TIP]
> <span data-ttu-id="f6c81-114">また、スケールアップが一時的にのみ必要であった場合は、システムを "_スケールダウン_" することもできます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-114">You can also _scale down_ your system if you needed to scale up only temporarily.</span></span>

<span data-ttu-id="f6c81-115">スケールアウトまたは水平スケーリングとは、仮想マシンを追加してアプリケーションの能力を高めることです。</span><span class="sxs-lookup"><span data-stu-id="f6c81-115">Scaling out, or horizontal scaling, means to additional virtual machines to power your application.</span></span> <span data-ttu-id="f6c81-116">たとえば、まったく同じ構成方法で多数の仮想マシンを作成し、ロード バランサーを使用してそれらの間に処理を分散させることができます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-116">For example, you might create many virtual machines configured in exactly the same way and use a load balancer to distribute work across them.</span></span>

## <a name="what-is-deallocation"></a><span data-ttu-id="f6c81-117">割り当て解除とは</span><span class="sxs-lookup"><span data-stu-id="f6c81-117">What is deallocation?</span></span>

<span data-ttu-id="f6c81-118">割り当て解除とは、VM をシャットダウンし、そのコンピューティング リソースを解放するプロセスです。</span><span class="sxs-lookup"><span data-stu-id="f6c81-118">Deallocation is the process that shuts down your VM and releases its compute resources.</span></span>

<span data-ttu-id="f6c81-119">職場や家庭の PC をシャットダウンすると、オペレーティング システムによってプログラムが終了され、電源をオフにするよう電源管理ハードウェアに通知されます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-119">When you shut down your PC at work or at home, the operating system closes your programs and then notifies the power management hardware to turn off power.</span></span>

<span data-ttu-id="f6c81-120">仮想マシンの割り当て解除もそれと似ています。</span><span class="sxs-lookup"><span data-stu-id="f6c81-120">Deallocating a virtual machine is similar.</span></span> <span data-ttu-id="f6c81-121">VM がシャットダウンすると、Azure はその VM が使用していたハードウェアを再利用します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-121">After your VM shuts down, Azure reclaims the hardware used to power it.</span></span> <span data-ttu-id="f6c81-122">データ ディスクとストレージはそのまま残ります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-122">Your data disks and storage remain intact.</span></span> <span data-ttu-id="f6c81-123">VM を再び起動すると、PC と同様に中断したところから再開します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-123">When you start your VM back up, you pick up where you left off, just like with your PC.</span></span>

<span data-ttu-id="f6c81-124">割り当てを解除すると、仮想マシンが使用するコンピューティング リソースとネットワーク リソースは課金されなくなります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-124">When deallocated, you are not billed for the compute and network resources that your virtual machine uses.</span></span> <span data-ttu-id="f6c81-125">ストレージに関連するディスクに対してはやはり課金されますが、全体的なコストは VM が実行されている場合よりはるかに低くなります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-125">You still pay for any associated disks to sit in storage, but the overall cost is much lower than it would be if the VM were running.</span></span>

<span data-ttu-id="f6c81-126">ここでは、サイズを変更できるように、VM の割り当てを短時間解除します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-126">Here, you'll deallocate your VM briefly so that you can resize it.</span></span> <span data-ttu-id="f6c81-127">ただし、コストを節約するため VM の割り当てを長期間解除することもできます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-127">But you can also deallocate your VMs for a longer period to save cost.</span></span> <span data-ttu-id="f6c81-128">営業時間中にテストのために使用する VM のバンクがあるものとします。</span><span class="sxs-lookup"><span data-stu-id="f6c81-128">Say you have a bank of VMs that you use for testing during work hours.</span></span> <span data-ttu-id="f6c81-129">夜間と週末には自動的に割り当てが解除されるように、VM をスケジュールすることができます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-129">You can schedule your VMs to be automatically deallocated on nights and weekends.</span></span> <span data-ttu-id="f6c81-130">遅くまで使用する必要がある場合は、手動で再起動できます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-130">If you need to stay late, you can manually restart them.</span></span>

## <a name="scale-up-your-vm"></a><span data-ttu-id="f6c81-131">VM をスケールアップする</span><span class="sxs-lookup"><span data-stu-id="f6c81-131">Scale up your VM</span></span>

<span data-ttu-id="f6c81-132">VM を作成するときに、サイズ **Standard_DS2_v2** を指定したことを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="f6c81-132">Recall that you specified the size **Standard_DS2_v2** when you created your VM.</span></span> <span data-ttu-id="f6c81-133">現在、VM は 2 つの仮想 CPU と 7 GB のメモリを備えています。</span><span class="sxs-lookup"><span data-stu-id="f6c81-133">Your VM currently has two virtual CPUs and 7 GB of memory.</span></span>

<span data-ttu-id="f6c81-134">1 つ上のサイズ **Standard_DS3_v2** に増強してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f6c81-134">Let's bump up to the next size, **Standard_DS3_v2**.</span></span> <span data-ttu-id="f6c81-135">VM は 4 つの仮想 CPU と 14 GB のメモリを備えるようになります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-135">Your VM will then have four virtual CPUs and 14 GB of memory.</span></span>

<span data-ttu-id="f6c81-136">::: zone pivot="windows-cloud"</span><span class="sxs-lookup"><span data-stu-id="f6c81-136">::: zone pivot="windows-cloud"</span></span>

1. <span data-ttu-id="f6c81-137">Cloud Shell から `az vm deallocate` を実行し、VM の割り当てを解除するか、または VM を停止します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-137">From Cloud Shell, run `az vm deallocate` to deallocate, or stop, your VM.</span></span>

    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myWindowsVM
    ```
    <span data-ttu-id="f6c81-138">プロセスが完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-138">The process takes a couple of minutes to complete.</span></span>
1. <span data-ttu-id="f6c81-139">`az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-139">Run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group myResourceGroup \
      --name myWindowsVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="f6c81-140">更新プロセスには約 1 分かかります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-140">The update process takes about a minute.</span></span>
1. <span data-ttu-id="f6c81-141">`az vm start` を実行して VM を再起動します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-141">Run `az vm start` to restart your VM.</span></span>

    ```azurecli
    az vm start \
      --resource-group myResourceGroup \
      --name myWindowsVM
    ```
    <span data-ttu-id="f6c81-142">プロセスには約 1 分かかります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-142">The process takes about a minute.</span></span>
1. <span data-ttu-id="f6c81-143">`az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-143">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group myResourceGroup \
      --name myWindowsVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="f6c81-144">新しい VM サイズ **Standard_DS3_v2** が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-144">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```console
    Standard_DS3_v2
    ```

<span data-ttu-id="f6c81-145">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="f6c81-145">::: zone-end</span></span>

<span data-ttu-id="f6c81-146">::: zone pivot="linux-cloud"</span><span class="sxs-lookup"><span data-stu-id="f6c81-146">::: zone pivot="linux-cloud"</span></span>

1. <span data-ttu-id="f6c81-147">Cloud Shell から `az vm deallocate` を実行し、VM の割り当てを解除するか、または VM を停止します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-147">From Cloud Shell, run `az vm deallocate` to deallocate, or stop, your VM.</span></span>

    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myLinuxVM
    ```
    <span data-ttu-id="f6c81-148">プロセスが完了するまでに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-148">The process takes a couple of minutes to complete.</span></span>
1. <span data-ttu-id="f6c81-149">`az vm resize` を実行して、VM のサイズを **Standard_DS3_v2** に上げます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-149">Run `az vm resize` to increase your VM's size to **Standard_DS3_v2**.</span></span>

    ```azurecli
    az vm resize \
      --resource-group myResourceGroup \
      --name myLinuxVM \
      --size Standard_DS3_v2
    ```
    <span data-ttu-id="f6c81-150">更新プロセスには約 1 分かかります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-150">The update process takes about a minute.</span></span>
1. <span data-ttu-id="f6c81-151">`az vm start` を実行して VM を再起動します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-151">Run `az vm start` to restart your VM.</span></span>

    ```azurecli
    az vm start \
      --resource-group myResourceGroup \
      --name myLinuxVM
    ```
    <span data-ttu-id="f6c81-152">プロセスには約 1 分かかります。</span><span class="sxs-lookup"><span data-stu-id="f6c81-152">The process takes about a minute.</span></span>
1. <span data-ttu-id="f6c81-153">`az vm show` を実行し、VM が新しいサイズで実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-153">Run `az vm show` to verify that your VM is running the new size.</span></span>

    ```azurecli
    az vm show \
      --resource-group myResourceGroup \
      --name myLinuxVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    <span data-ttu-id="f6c81-154">新しい VM サイズ **Standard_DS3_v2** が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-154">You see your new VM size, **Standard_DS3_v2**.</span></span>
    ```console
    Standard_DS3_v2
    ```

<span data-ttu-id="f6c81-155">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="f6c81-155">::: zone-end</span></span>

> [!NOTE]
> <span data-ttu-id="f6c81-156">既定では、VM を停止して再起動すると、Azure によって VM に新しいパブリック IP アドレスが割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-156">By default, Azure assigns a new public IP address to your VM when you stop and restart it.</span></span> <span data-ttu-id="f6c81-157">学習目的の場合はこれで問題ありません。</span><span class="sxs-lookup"><span data-stu-id="f6c81-157">This is OK for learning purposes.</span></span> <span data-ttu-id="f6c81-158">実際には、VM を再起動しても VM のパブリック IP アドレスが変わらないように予約しておくことができます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-158">In practice, you can reserve a public IP address that stays with your VM even when your VM is restarted.</span></span>

## <a name="summary"></a><span data-ttu-id="f6c81-159">まとめ</span><span class="sxs-lookup"><span data-stu-id="f6c81-159">Summary</span></span>

<span data-ttu-id="f6c81-160">ごくろうさま。</span><span class="sxs-lookup"><span data-stu-id="f6c81-160">Nice job!</span></span> <span data-ttu-id="f6c81-161">ほんの数コマンドで、VM の能力が 2 倍になりました。</span><span class="sxs-lookup"><span data-stu-id="f6c81-161">With just a few commands, your VM is now twice as powerful.</span></span>

<span data-ttu-id="f6c81-162">スケールアップとスケールアウトは、パフォーマンスを向上させる 2 つの方法です。</span><span class="sxs-lookup"><span data-stu-id="f6c81-162">Scaling up and scaling out are two ways to increase performance.</span></span> <span data-ttu-id="f6c81-163">ここでは、VM をスケールアップしてそのコンピューティング能力を増強しました。</span><span class="sxs-lookup"><span data-stu-id="f6c81-163">Here you scaled up your VM to increase its compute power.</span></span>

<span data-ttu-id="f6c81-164">サイズを変更する前に、VM の割り当てを解除します。</span><span class="sxs-lookup"><span data-stu-id="f6c81-164">You deallocate a VM before you can resize it.</span></span> <span data-ttu-id="f6c81-165">コストを節約するために使用しなくなった VM の割り当てを解除することもできます。</span><span class="sxs-lookup"><span data-stu-id="f6c81-165">You can also deallocate your VMs when they're not in use to save costs.</span></span>