<span data-ttu-id="fde22-101">オンライン バンク アーキテクチャの主要なリソースを作成しました。</span><span class="sxs-lookup"><span data-stu-id="fde22-101">You have created the key resources for your online bank architecture.</span></span> <span data-ttu-id="fde22-102">この演習では、負荷分散規則を構成し、セットアップを完了します。</span><span class="sxs-lookup"><span data-stu-id="fde22-102">In this exercise, you will complete the setup by configuring the load-balancing rules.</span></span>

<span data-ttu-id="fde22-103">セットアップが完了したら、プールから VM を削除し、その VM にクライアント要求が送信されなくなったことを確認するという手順でロード バランサーをテストします。</span><span class="sxs-lookup"><span data-stu-id="fde22-103">Once setup is complete, you will test the load balancer by removing a VM from the pool and verifying that client requests are no longer sent to this VM.</span></span>

<span data-ttu-id="fde22-104">最初にロード バランサーでバックエンド プールを定義します。</span><span class="sxs-lookup"><span data-stu-id="fde22-104">We'll start by defining our backend pool in the load balancer.</span></span> <span data-ttu-id="fde22-105">これにより受信要求の経路が決定されます。</span><span class="sxs-lookup"><span data-stu-id="fde22-105">This determines where inbound requests are routed.</span></span>

## <a name="create-a-backend-address-pool"></a><span data-ttu-id="fde22-106">バックエンド アドレス プールの作成</span><span class="sxs-lookup"><span data-stu-id="fde22-106">Create a backend address pool</span></span>

1. <span data-ttu-id="fde22-107">[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) で、左側のメニューにある **[すべてのリソース]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="fde22-107">In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), in the left menu, Select **All resources**.</span></span> <span data-ttu-id="fde22-108">次に、リソースの一覧で、ロード バランサー (**woodgrove-LB**) を選択します。</span><span class="sxs-lookup"><span data-stu-id="fde22-108">Then, in the resource list, select your load balancer (**woodgrove-LB**).</span></span>

1. <span data-ttu-id="fde22-109">**[設定]**、**[バックエンド プール]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="fde22-109">Select **Settings** > **Backend pools**.</span></span> <span data-ttu-id="fde22-110">まだ何も定義されていないことに注目してください。</span><span class="sxs-lookup"><span data-stu-id="fde22-110">Notice we don't have any defined yet.</span></span>

1. <span data-ttu-id="fde22-111">**[追加]** をクリックし、新しいバックエンド プールを追加します。</span><span class="sxs-lookup"><span data-stu-id="fde22-111">Click **Add** to add a new backend pool.</span></span>

    ![バックエンド プールの追加方法を示すスクリーンショット。](../media/6-backend-pools.png)

1. <span data-ttu-id="fde22-113">**[バックエンド プールの追加]** ブレードで、次の情報を追加します。</span><span class="sxs-lookup"><span data-stu-id="fde22-113">On the **Add backend pool** blade, add the following information:</span></span>
    - <span data-ttu-id="fde22-114">それに _woodgrove-BEP_ という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="fde22-114">Name it _woodgrove-BEP_.</span></span>
    - <span data-ttu-id="fde22-115">**可用性セット**にプールを関連付けます。</span><span class="sxs-lookup"><span data-stu-id="fde22-115">Associated the pool to an **Availability set**.</span></span>
    - <span data-ttu-id="fde22-116">可用性セットについては、_[woodgrove-AS]_ を選択します。</span><span class="sxs-lookup"><span data-stu-id="fde22-116">For the availability set, select _woodgrove-AS_.</span></span>

1. <span data-ttu-id="fde22-117">**[ターゲット ネットワーク IP 構成の追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fde22-117">Click **Add a target network IP configuration**.</span></span>

1. <span data-ttu-id="fde22-118">**[ターゲット仮想マシン]** 一覧で、_[woodgrove-VM1]_ を選択します。</span><span class="sxs-lookup"><span data-stu-id="fde22-118">In the **Target virtual machine** list, select _woodgrove-VM1_.</span></span>

1. <span data-ttu-id="fde22-119">VM の **[ネットワーク IP 構成]** 一覧で、VM の IP アドレス プールを選択します。</span><span class="sxs-lookup"><span data-stu-id="fde22-119">In the **Network IP configuration** list for the VM, select the VM's IP address pool.</span></span>

1. <span data-ttu-id="fde22-120">以上の手順を繰り返し、_[woodgrove-VM2]_ と _[woodgrove-VM3]_ をバックエンド プールに追加します。</span><span class="sxs-lookup"><span data-stu-id="fde22-120">Repeat those steps to add _woodgrove-VM2_ and _woodgrove-VM3_ to the backend pool.</span></span>

    ![[バックエンド プールの追加] ブレードのスクリーンショット。VM が 3 つすべて選択されています。](../media/6-add-backend-pool.png)

1. <span data-ttu-id="fde22-122">**[OK]** をクリックしてブレードを閉じます。</span><span class="sxs-lookup"><span data-stu-id="fde22-122">Click **OK** to close the blade.</span></span>

<span data-ttu-id="fde22-123">ロード バランサーの構成が更新されるのを待ち、更新後、演習に進みます。</span><span class="sxs-lookup"><span data-stu-id="fde22-123">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="create-a-health-probe-for-the-load-balancer"></a><span data-ttu-id="fde22-124">ロード バランサーの正常性プローブを作成する</span><span class="sxs-lookup"><span data-stu-id="fde22-124">Create a health probe for the load balancer</span></span>

<span data-ttu-id="fde22-125">次に、ポート 80 で HTTP に対して正常性プローブを追加します。</span><span class="sxs-lookup"><span data-stu-id="fde22-125">Next, add a health probe for HTTP over port 80.</span></span>

1. <span data-ttu-id="fde22-126">**[woodgrove-LB]** ブレードの **[設定]** で **[正常性プローブ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fde22-126">On the **woodgrove-LB** blade, under **Settings**, click **Health probes**.</span></span> <span data-ttu-id="fde22-127">現在のところ、空になっていることに注目してください。</span><span class="sxs-lookup"><span data-stu-id="fde22-127">Notice it's currently empty.</span></span>

1. <span data-ttu-id="fde22-128">**[追加]** をクリックして新しい正常性プローブを追加します。</span><span class="sxs-lookup"><span data-stu-id="fde22-128">Click **Add** to add a new health probe.</span></span>

1. <span data-ttu-id="fde22-129">**[正常性プローブの追加]** ブレードで、次の構成を設定します。</span><span class="sxs-lookup"><span data-stu-id="fde22-129">On the **Add health probe** blade, set the following configuration:</span></span>
    - <span data-ttu-id="fde22-130">**名前:** _woodgrove-HP_</span><span class="sxs-lookup"><span data-stu-id="fde22-130">**Name:** _woodgrove-HP_</span></span>
    - <span data-ttu-id="fde22-131">**プロトコル:** _HTTP_</span><span class="sxs-lookup"><span data-stu-id="fde22-131">**Protocol:** _HTTP_</span></span>
    - <span data-ttu-id="fde22-132">**ポート:** _80_</span><span class="sxs-lookup"><span data-stu-id="fde22-132">**Port:** _80_</span></span>
    - <span data-ttu-id="fde22-133">**パス:** _/_</span><span class="sxs-lookup"><span data-stu-id="fde22-133">**Path:** _/_</span></span>
    - <span data-ttu-id="fde22-134">**間隔:** _15_</span><span class="sxs-lookup"><span data-stu-id="fde22-134">**Interval:** _15_</span></span>
    - <span data-ttu-id="fde22-135">**異常なしきい値:** _2_</span><span class="sxs-lookup"><span data-stu-id="fde22-135">**Unhealthy threshold:** _2_</span></span>

1. <span data-ttu-id="fde22-136">**[OK]** をクリックして変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="fde22-136">Click **OK** to save the changes.</span></span>

<span data-ttu-id="fde22-137">ロード バランサーの構成が更新されるのを待ち、更新後、演習に進みます。</span><span class="sxs-lookup"><span data-stu-id="fde22-137">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="create-a-load-balancer-rule"></a><span data-ttu-id="fde22-138">ロード バランサー規則の作成</span><span class="sxs-lookup"><span data-stu-id="fde22-138">Create a load balancer rule</span></span>

<span data-ttu-id="fde22-139">最後に、バックエンド プールを正常性プローブに関連付ける HTTP の負荷分散規則をポート 80 で作成します。</span><span class="sxs-lookup"><span data-stu-id="fde22-139">Finally, create a load-balancing rule for the HTTP over port 80, that associates the backend pool with the health probe.</span></span>

1. <span data-ttu-id="fde22-140">**[woodgrove-LB]** ブレードの **[設定]** で、**[負荷分散規則]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fde22-140">On the **woodgrove-LB** blade, under **Settings**, click **Load balancing rules**.</span></span>

1. <span data-ttu-id="fde22-141">**[追加]** をクリックして新しい規則を追加します。</span><span class="sxs-lookup"><span data-stu-id="fde22-141">Click **Add** to add a new rule.</span></span>

1. <span data-ttu-id="fde22-142">**[負荷分散規則の追加]** ブレードで **[名前]** を _woodgrove-HTTP-LBRule_ にします。</span><span class="sxs-lookup"><span data-stu-id="fde22-142">On the **Add load balancing rule** blade, set the the **Name** to _woodgrove-HTTP-LBRule_.</span></span>

1. <span data-ttu-id="fde22-143">以下の情報が自動的に入力されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="fde22-143">Verify that the following information has been automatically entered:</span></span>
    - <span data-ttu-id="fde22-144">IP バージョン: **IPv4**</span><span class="sxs-lookup"><span data-stu-id="fde22-144">IP Version: **IPv4**</span></span>
    - <span data-ttu-id="fde22-145">フロントエンド IP アドレス: **LoadBalancerFrontEnd**</span><span class="sxs-lookup"><span data-stu-id="fde22-145">Frontend IP address: **LoadBalancerFrontEnd**</span></span>
    - <span data-ttu-id="fde22-146">プロトコル: **TCP**</span><span class="sxs-lookup"><span data-stu-id="fde22-146">Protocol: **TCP**</span></span>
    - <span data-ttu-id="fde22-147">ポート: **80**</span><span class="sxs-lookup"><span data-stu-id="fde22-147">Port: **80**</span></span>
    - <span data-ttu-id="fde22-148">バックエンド ポート: **80**</span><span class="sxs-lookup"><span data-stu-id="fde22-148">Backend port: **80**</span></span>
    - <span data-ttu-id="fde22-149">バックエンド プール: **woodgrove-BEP**</span><span class="sxs-lookup"><span data-stu-id="fde22-149">Backend pool: **woodgrove-BEP**</span></span>
    - <span data-ttu-id="fde22-150">正常性プローブ: **woodgrove-HP**</span><span class="sxs-lookup"><span data-stu-id="fde22-150">Health probe: **woodgrove-HP**</span></span>
    - <span data-ttu-id="fde22-151">セッション永続化: **なし**</span><span class="sxs-lookup"><span data-stu-id="fde22-151">Session persistence: **None**</span></span>
    - <span data-ttu-id="fde22-152">アイドル タイムアウト: **4 分**</span><span class="sxs-lookup"><span data-stu-id="fde22-152">Idle timeout: **4 minutes**</span></span>
    - <span data-ttu-id="fde22-153">Floating IP: **無効**</span><span class="sxs-lookup"><span data-stu-id="fde22-153">Floating IP: **Disabled**</span></span>

1. <span data-ttu-id="fde22-154">**[OK]** をクリックして規則を保存します。</span><span class="sxs-lookup"><span data-stu-id="fde22-154">Click **OK** to save the rule.</span></span>

<span data-ttu-id="fde22-155">ロード バランサーの構成が更新されるのを待ち、更新後、演習に進みます。</span><span class="sxs-lookup"><span data-stu-id="fde22-155">Wait until the load balancer configuration has updated before proceeding with the exercise.</span></span>

## <a name="test-the-load-balancer"></a><span data-ttu-id="fde22-156">ロード バランサーをテストする</span><span class="sxs-lookup"><span data-stu-id="fde22-156">Test the load balancer</span></span>

1. <span data-ttu-id="fde22-157">ロード バランサーの **[概要]** ページに切り替え、そのページの **[パブリック IP アドレス]** リンクをクリックし、割り当てられている IP アドレスを表示します。</span><span class="sxs-lookup"><span data-stu-id="fde22-157">Switch back to the **Overview** page of the load balancer and click the **Public IP address** link on the page to get to the IP address assigned.</span></span> <span data-ttu-id="fde22-158">あるいは、**[すべてのリソース]** を使用し、リソースの一覧で **[woodgrove-LB-ip]** を選択できます。</span><span class="sxs-lookup"><span data-stu-id="fde22-158">Alternatively, you can use **All resources** and then in the resource list, select **woodgrove-LB-ip**.</span></span>

1. <span data-ttu-id="fde22-159">**[概要]** ブレードで **[IP アドレス]** を選択し、その隣にある **[コピー]** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fde22-159">On the **Overview** blade, select the **IP address**, and click the **Copy** button next to it.</span></span> <span data-ttu-id="fde22-160">これはロード バランサーの_パブリック_ IP アドレスです。</span><span class="sxs-lookup"><span data-stu-id="fde22-160">This is the _public_ IP address for the load balancer.</span></span>

    ![パブリック IP アドレスの概要パネルを示すスクリーンショット](../media/6-public-ip.png)

1. <span data-ttu-id="fde22-162">新しいブラウザー タブを開き、ブラウザーのアドレス バーに IP アドレスを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="fde22-162">Open a new browser tab and paste the IP address into the address bar of your browser.</span></span> <span data-ttu-id="fde22-163">サーバーの名前を書き留めます。このタブは開いたままにしておいてください。</span><span class="sxs-lookup"><span data-stu-id="fde22-163">Note the name of the server and keep this tab open.</span></span>

1. <span data-ttu-id="fde22-164">Azure portal で、左側のメニューの **[すべてのリソース]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fde22-164">In the Azure portal, in the left menu, click **All resources**.</span></span> <span data-ttu-id="fde22-165">次に、リソース リストで、先ほど書き留めたサーバーをクリックします。</span><span class="sxs-lookup"><span data-stu-id="fde22-165">Then, in the resource list, click the server you noted above.</span></span>

1. <span data-ttu-id="fde22-166">**[概要]** ブレードで **[停止]** をクリックし、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fde22-166">On the **Overview** blade, click **Stop**, and then click **Yes**.</span></span>

1. <span data-ttu-id="fde22-167">VM が停止するまで待って、手順 3. で表示したタブに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="fde22-167">Wait until the VM has stopped, and then switch to the tab you viewed in step 3.</span></span> <span data-ttu-id="fde22-168">ページを更新します。</span><span class="sxs-lookup"><span data-stu-id="fde22-168">Refresh the page.</span></span>

1. <span data-ttu-id="fde22-169">これでロード バランサーは HTTP 要求を他のいずれかの VM に送信するようになりました。</span><span class="sxs-lookup"><span data-stu-id="fde22-169">The load balancer will now send your HTTP request to one of your other VMs.</span></span>

<span data-ttu-id="fde22-170">この演習では、バックエンド VM とロード バランサーのデプロイを完了しました。</span><span class="sxs-lookup"><span data-stu-id="fde22-170">In this exercise, you completed the deployment of your backend VMs and the load balancer.</span></span>
