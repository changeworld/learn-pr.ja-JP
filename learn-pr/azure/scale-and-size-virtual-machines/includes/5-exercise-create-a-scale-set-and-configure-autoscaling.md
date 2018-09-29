<span data-ttu-id="d3e8c-101">この課題では Azure portal を使用し、仮想マシン スケール セットと自動スケール規則を作成します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-101">In this exercise, you will use the Azure portal to create a virtual machine scale set with rules for autoscaling.</span></span>

## <a name="create-a-virtual-machine-scale-set"></a><span data-ttu-id="d3e8c-102">仮想マシン スケール セットを作成する</span><span class="sxs-lookup"><span data-stu-id="d3e8c-102">Create a virtual machine scale set</span></span>

1. <span data-ttu-id="d3e8c-103">[Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) で、**[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-103">In the [Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), click **Create a resource**.</span></span>

1. <span data-ttu-id="d3e8c-104">検索ボックスに「**scale set**」と入力し、<kbd>Enter</kbd> キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-104">In the search box, type **scale set** and press <kbd>ENTER</kbd>.</span></span> <span data-ttu-id="d3e8c-105">**[結果]** ブレードで **Virtual machine scale set** をクリックし、**[Virtual machine scale set]** ブレードで **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-105">On the **Results** blade, click **Virtual machine scale set**, and on the **Virtual machine scale set** blade, click **Create**.</span></span>

1. <span data-ttu-id="d3e8c-106">**[仮想マシン スケール セットの作成]** ブレードで、次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-106">On the **Create virtual machine scale set** blade, enter the following values.</span></span>
    - <span data-ttu-id="d3e8c-107">**[名前]** には _WebServerSS_ を使用します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-107">Use _WebServerSS_ for the **Name**.</span></span>
    - <span data-ttu-id="d3e8c-108">**[オペレーティング システムのディスク イメージ]** は _Windows Server 2016 Datacenter_ のままにします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-108">Leave _Windows Server 2016 Datacenter_ for the **Operating system disk image**.</span></span>
    - <span data-ttu-id="d3e8c-109">**[サブスクリプション]** には "_コンシェルジェ サブスクリプション_" を使用します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-109">Use _Concierge Subscription_ for the **Subscription**.</span></span>
    - <span data-ttu-id="d3e8c-110">**[リソース グループ]** では **<rgn>[サンドボックス リソース グループ名]</rgn>** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-110">Select **<rgn>[sandbox resource group name]</rgn>** for the **Resource group**.</span></span>
    - <span data-ttu-id="d3e8c-111">次の一覧から場所を選択します。[!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]</span><span class="sxs-lookup"><span data-stu-id="d3e8c-111">Pick a location from the following list:   [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]</span></span>

    - <span data-ttu-id="d3e8c-112">有効な **[ユーザー名]** と **[パスワード]** を設定します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-112">Set a valid **Username** and **Password**.</span></span> <span data-ttu-id="d3e8c-113">(後で使うのでメモしておきます)</span><span class="sxs-lookup"><span data-stu-id="d3e8c-113">(Note these for later use)</span></span>
    - <span data-ttu-id="d3e8c-114">**[インスタンス数]** は _2_ に設定します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-114">Set the **Instance count** to _2_.</span></span>
    - <span data-ttu-id="d3e8c-115">**[インスタンス サイズ]** は _D2s_v3_ に設定します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-115">Set the **Instance size** to _D2s_v3_.</span></span>
    - <span data-ttu-id="d3e8c-116">**[自動スケール]** は _[無効]_ のままにします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-116">Leave **Autoscale** as _Disabled_.</span></span>
    - <span data-ttu-id="d3e8c-117">**[ロード バランサーLoad balancer]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-117">Select **Load balancer**.</span></span>
    - <span data-ttu-id="d3e8c-118">**[パブリック IP アドレス名]** に「_WebServerPubIP_」と入力します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-118">Enter _WebServerPubIP_ for the **Public IP address name**.</span></span>
    - <span data-ttu-id="d3e8c-119">**[ドメイン名ラベル]** は、一意になるように "イニシャル + 日付 + 時刻" を使用します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-119">Use your initials + date + time for the **Domain name label** so it's unique.</span></span>
    - <span data-ttu-id="d3e8c-120">**[仮想ネットワーク]** では **[新規作成]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-120">For **Virtual network** select **Create new**.</span></span> <span data-ttu-id="d3e8c-121">名前を「**SSVNet**」にして **[作成]** をクリックし、VNet を作成します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-121">Name it **SSVNet** and click **Create** to create the VNet.</span></span>

1. <span data-ttu-id="d3e8c-122">**[作成]** をクリックして、スケール セットをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-122">Click **Create** to deploy the scale set.</span></span>

1. <span data-ttu-id="d3e8c-123">スケール セットが作成されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-123">Wait for the scale set to be created.</span></span>

## <a name="create-and-apply-autoscale-rules"></a><span data-ttu-id="d3e8c-124">自動スケール規則を作成して適用する</span><span class="sxs-lookup"><span data-stu-id="d3e8c-124">Create and apply autoscale rules</span></span>

1. <span data-ttu-id="d3e8c-125">左サイドバーで **[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-125">Click **Resource Groups** in the left sidebar.</span></span>

1. <span data-ttu-id="d3e8c-126">**<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-126">Select the **<rgn>[sandbox resource group name]</rgn>** resource group.</span></span>

1. <span data-ttu-id="d3e8c-127">**<rgn>[サンド ボックス リソース グループ名]</rgn>** ブレードで、**WebServerSS** オブジェクトをクリックしてスケール セットを開きます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-127">On the **<rgn>[sandbox resource group name]</rgn>** blade, click the **WebServerSS** object to open the scale set.</span></span>

1. <span data-ttu-id="d3e8c-128">**[WebServerSS]** ブレードで、**[設定]** の **[インスタンス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-128">On the **WebServerSS** blade, under **Settings**, click **Instances**.</span></span> <span data-ttu-id="d3e8c-129">スケール セット内で 2 つの仮想マシン インスタンスが実行されることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-129">Note the two virtual machine instances running within the scale set.</span></span>

1. <span data-ttu-id="d3e8c-130">**[WebServerSS]** ブレードで、**[スケーリング]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-130">On the **WebServerSS** blade, click **Scaling**.</span></span>

1. <span data-ttu-id="d3e8c-131">**[WebServerSS - スケーリング]** ブレードで **[自動スケールの有効化]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-131">On the **WebServerSS - Scaling** blade, click **Enable autoscale**.</span></span>

1. <span data-ttu-id="d3e8c-132">構成画面で、名前を **WebAutoscaleSetting** に設定します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-132">On configuration screen, set the name to **WebAutoscaleSetting**.</span></span>

1. <span data-ttu-id="d3e8c-133">既定の条件で **[インスタンスの制限]** を検索し、次の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-133">In the default condition, locate the **Instance limits**, set the following values.</span></span>

    |<span data-ttu-id="d3e8c-134">設定</span><span class="sxs-lookup"><span data-stu-id="d3e8c-134">Setting</span></span>|<span data-ttu-id="d3e8c-135">値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-135">Value</span></span>|
    |---|---|
    |<span data-ttu-id="d3e8c-136">最小値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-136">Minimum</span></span>|<span data-ttu-id="d3e8c-137">2</span><span class="sxs-lookup"><span data-stu-id="d3e8c-137">2</span></span>|
    |<span data-ttu-id="d3e8c-138">最大値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-138">Maximum</span></span>|<span data-ttu-id="d3e8c-139">8</span><span class="sxs-lookup"><span data-stu-id="d3e8c-139">8</span></span>|
    |<span data-ttu-id="d3e8c-140">既定値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-140">Default</span></span>|<span data-ttu-id="d3e8c-141">2</span><span class="sxs-lookup"><span data-stu-id="d3e8c-141">2</span></span>|

1. <span data-ttu-id="d3e8c-142">**[ルールの追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-142">Click **Add a rule**.</span></span>

1. <span data-ttu-id="d3e8c-143">**[スケール ルール]** ブレードで次の情報を入力し、**[追加]** をクリックします。これにより、平均 CPU 使用率が 5 分以上にわたって 75% を上回ったら、さらに 2 つの仮想マシンをスケールアウトするルールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-143">On the **Scale rule** blade, enter the following information to create a rule to scale out an extra two virtual machines when average CPU usage is more than 75% over a five-minute period, and then click **Add**.</span></span>

    |<span data-ttu-id="d3e8c-144">設定</span><span class="sxs-lookup"><span data-stu-id="d3e8c-144">Setting</span></span>|<span data-ttu-id="d3e8c-145">値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-145">Value</span></span>|
    |---|---|
    |<span data-ttu-id="d3e8c-146">総時間</span><span class="sxs-lookup"><span data-stu-id="d3e8c-146">Time aggregation</span></span>|<span data-ttu-id="d3e8c-147">平均</span><span class="sxs-lookup"><span data-stu-id="d3e8c-147">Average</span></span>|
    |<span data-ttu-id="d3e8c-148">メトリックの名前</span><span class="sxs-lookup"><span data-stu-id="d3e8c-148">Metric name</span></span>|<span data-ttu-id="d3e8c-149">CPU 使用率</span><span class="sxs-lookup"><span data-stu-id="d3e8c-149">Percentage CPU</span></span>|
    |<span data-ttu-id="d3e8c-150">統計時間単位</span><span class="sxs-lookup"><span data-stu-id="d3e8c-150">Time grain statistic</span></span>|<span data-ttu-id="d3e8c-151">平均</span><span class="sxs-lookup"><span data-stu-id="d3e8c-151">Average</span></span>|
    |<span data-ttu-id="d3e8c-152">演算子</span><span class="sxs-lookup"><span data-stu-id="d3e8c-152">Operator</span></span>|<span data-ttu-id="d3e8c-153">より大きい</span><span class="sxs-lookup"><span data-stu-id="d3e8c-153">Greater than</span></span>|
    |<span data-ttu-id="d3e8c-154">しきい値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-154">Threshold</span></span>|<span data-ttu-id="d3e8c-155">75</span><span class="sxs-lookup"><span data-stu-id="d3e8c-155">75</span></span>|
    |<span data-ttu-id="d3e8c-156">時間 (分単位)</span><span class="sxs-lookup"><span data-stu-id="d3e8c-156">Duration (in minutes)</span></span>|<span data-ttu-id="d3e8c-157">5</span><span class="sxs-lookup"><span data-stu-id="d3e8c-157">5</span></span>|
    |<span data-ttu-id="d3e8c-158">Operation (運用)</span><span class="sxs-lookup"><span data-stu-id="d3e8c-158">Operation</span></span>|<span data-ttu-id="d3e8c-159">増加単位</span><span class="sxs-lookup"><span data-stu-id="d3e8c-159">Increase count by</span></span>|
    |<span data-ttu-id="d3e8c-160">インスタンス数</span><span class="sxs-lookup"><span data-stu-id="d3e8c-160">Instance count</span></span>|<span data-ttu-id="d3e8c-161">2</span><span class="sxs-lookup"><span data-stu-id="d3e8c-161">2</span></span>|
    |<span data-ttu-id="d3e8c-162">クールダウン (分)</span><span class="sxs-lookup"><span data-stu-id="d3e8c-162">Cool down (minutes)</span></span>|<span data-ttu-id="d3e8c-163">5</span><span class="sxs-lookup"><span data-stu-id="d3e8c-163">5</span></span>|

1. <span data-ttu-id="d3e8c-164">次の情報を入力して **[追加]** をクリックし、別のルールを追加します。今度は、平均 CPU 使用率が 5 分以上にわたって 30% を下回ったら、サーバーを 1 台ずつスケールインするルールが作成されます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-164">Add another rule, this time, enter the following information to create a rule to scale in one server at a time when average CPU usage is below 30% over a five-minute period, and then click **Add**.</span></span>

    |<span data-ttu-id="d3e8c-165">設定</span><span class="sxs-lookup"><span data-stu-id="d3e8c-165">Setting</span></span>|<span data-ttu-id="d3e8c-166">値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-166">Value</span></span>|
    |---|---|
    |<span data-ttu-id="d3e8c-167">総時間</span><span class="sxs-lookup"><span data-stu-id="d3e8c-167">Time aggregation</span></span>|<span data-ttu-id="d3e8c-168">平均</span><span class="sxs-lookup"><span data-stu-id="d3e8c-168">Average</span></span>|
    |<span data-ttu-id="d3e8c-169">メトリックの名前</span><span class="sxs-lookup"><span data-stu-id="d3e8c-169">Metric name</span></span>|<span data-ttu-id="d3e8c-170">CPU 使用率</span><span class="sxs-lookup"><span data-stu-id="d3e8c-170">Percentage CPU</span></span>|
    |<span data-ttu-id="d3e8c-171">統計時間単位</span><span class="sxs-lookup"><span data-stu-id="d3e8c-171">Time grain statistic</span></span>|<span data-ttu-id="d3e8c-172">平均</span><span class="sxs-lookup"><span data-stu-id="d3e8c-172">Average</span></span>|
    |<span data-ttu-id="d3e8c-173">演算子</span><span class="sxs-lookup"><span data-stu-id="d3e8c-173">Operator</span></span>|<span data-ttu-id="d3e8c-174">より小さい</span><span class="sxs-lookup"><span data-stu-id="d3e8c-174">Less than</span></span>|
    |<span data-ttu-id="d3e8c-175">しきい値</span><span class="sxs-lookup"><span data-stu-id="d3e8c-175">Threshold</span></span>|<span data-ttu-id="d3e8c-176">30</span><span class="sxs-lookup"><span data-stu-id="d3e8c-176">30</span></span>|
    |<span data-ttu-id="d3e8c-177">時間 (分単位)</span><span class="sxs-lookup"><span data-stu-id="d3e8c-177">Duration (in minutes)</span></span>|<span data-ttu-id="d3e8c-178">5</span><span class="sxs-lookup"><span data-stu-id="d3e8c-178">5</span></span>|
    |<span data-ttu-id="d3e8c-179">Operation (運用)</span><span class="sxs-lookup"><span data-stu-id="d3e8c-179">Operation</span></span>|<span data-ttu-id="d3e8c-180">減少単位</span><span class="sxs-lookup"><span data-stu-id="d3e8c-180">Decrease count by</span></span>|
    |<span data-ttu-id="d3e8c-181">インスタンス数</span><span class="sxs-lookup"><span data-stu-id="d3e8c-181">Instance count</span></span>|<span data-ttu-id="d3e8c-182">1</span><span class="sxs-lookup"><span data-stu-id="d3e8c-182">1</span></span>|
    |<span data-ttu-id="d3e8c-183">クールダウン (分)</span><span class="sxs-lookup"><span data-stu-id="d3e8c-183">Cool down (minutes)</span></span>|<span data-ttu-id="d3e8c-184">5</span><span class="sxs-lookup"><span data-stu-id="d3e8c-184">5</span></span>|

1. <span data-ttu-id="d3e8c-185">ルールは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-185">Your rules should look something like this:</span></span>

    ![VM スケール セットに適用される自動スケール ルール セットのスクリーンショット](../media/5-scale-rules.png)

1. <span data-ttu-id="d3e8c-187">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-187">Click **Save**.</span></span>

## <a name="generate-load-to-demonstrate-autoscaling"></a><span data-ttu-id="d3e8c-188">負荷を生成して自動スケールのデモをする</span><span class="sxs-lookup"><span data-stu-id="d3e8c-188">Generate load to demonstrate autoscaling</span></span>

<span data-ttu-id="d3e8c-189">ここでは、SysInternals の **CPUStress.exe** ツールを利用して、スケール セット内の仮想マシンに負荷を生成し、自動スケール アウトのデモをします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-189">Now you will use the SysInternals **CPUStress.exe** tool to generate load on the virtual machines in the scale set and demonstrate automatic scaling out.</span></span>

<span data-ttu-id="d3e8c-190">これを VM で実行します。このために Windows を用意する必要はありませんが、リモート デスクトップ クライアントが "_必要です_"。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-190">We will run this on our VM - you do not need to have Windows for this, however you _do_ need a Remote Desktop Client.</span></span> <span data-ttu-id="d3e8c-191">Microsoft には Windows 用と macOS 用のリモート デスクトップ クライアントがあり、さまざまな Linux ディストリビューションにもクライアントが含まれます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-191">Microsoft has one for macOS and Windows, and various distributions of Linux include a client as well.</span></span>

1. <span data-ttu-id="d3e8c-192">接続先のパブリック IP アドレスを求めるには、**<rgn>[サンドボックス リソース グループ名]</rgn>** リソース グループを参照します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-192">To determine the public IP address to connect to, browse to the **<rgn>[sandbox resource group name]</rgn>** resource group.</span></span> <span data-ttu-id="d3e8c-193">**<rgn>[サンド ボックス リソース グループ名]</rgn>** ブレードで、**WebServerSSlb** オブジェクトをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-193">On the **<rgn>[sandbox resource group name]</rgn>** blade, click the **WebServerSSlb** object.</span></span>

1. <span data-ttu-id="d3e8c-194">**[WebServerSSlb]** ブレードで **[フロントエンド IP 構成]** をクリックして、表示されたパブリック IP アドレスを書き留めます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-194">On the **WebServerSSlb** blade, click **Frontend IP configuration** and make a note of the public IP address shown.</span></span> <span data-ttu-id="d3e8c-195">**[LoadBalancerFrontEnd]** ブレードを閉じます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-195">Close the **LoadBalancerFrontEnd** blade.</span></span>

1. <span data-ttu-id="d3e8c-196">接続先のポート番号を求めるには､**[WebServerSSlb]** ブレードで **[受信 NAT 規則]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-196">To determine the port number to connect to, on the **WebServerSSlb** blade, click **Inbound NAT rules**.</span></span> <span data-ttu-id="d3e8c-197">各仮想マシンの [サービス] 列にあるカスタム ポート番号を書き留めます｡</span><span class="sxs-lookup"><span data-stu-id="d3e8c-197">Note down the custom port number in the Service column for each virtual machine.</span></span>

1. <span data-ttu-id="d3e8c-198">ローカル コンピューター上で **[リモート デスクトップ接続]** を起動します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-198">On your local computer, start the **Remote Desktop Connection**.</span></span> <span data-ttu-id="d3e8c-199">**[コンピューター]** ボックスにロード バランサーのパブリック IP アドレスを入力し、コロンを入力してから、インスタンス 0 のポート番号値 (例:40.67.191.221:50000 ) を入力して、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-199">In the **Computer** box, type the public IP address of the load balancer, followed by a colon and then the port number value for Instance 0 (for example, 40.67.191.221:50000), and then click **Connect**.</span></span>

1. <span data-ttu-id="d3e8c-200">**[Windows セキュリティ]** ダイアログ ボックスで **[その他]** をクリックし、**[別のアカウントを使う]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-200">In the **Windows Security** dialog box, click **More choices**, and then click **Use a different account**.</span></span> <span data-ttu-id="d3e8c-201">**[ユーザー名]** ボックスと **[パスワード]** ボックスに、スケール セットを作成するときに指定した資格情報を入力します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-201">In the **User name** and **Password** boxes, type enter the credentials you specified above when you created the scale set.</span></span>

1. <span data-ttu-id="d3e8c-202">**[リモート デスクトップ接続]** ダイアログ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-202">In the **Remote Desktop Connection** dialog box, click **Yes**.</span></span>

1. <span data-ttu-id="d3e8c-203">仮想マシンのリモート デスクトップで Internet Explorer を開き、**[Internet Explorer の設定]** ダイアログ ボックスで **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-203">In the virtual machine remote desktop, open Internet Explorer, and in the **Set up Internet Explorer** dialog box, click **OK**.</span></span>

1. <span data-ttu-id="d3e8c-204">**Internet Explorer** のアドレス バーに「**http://download.sysinternals.com/files/CPUSTRES.zip**」と入力して、<kbd>Enter</kbd> キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-204">In **Internet Explorer**, in the address bar, type **http://download.sysinternals.com/files/CPUSTRES.zip** and press <kbd>ENTER</kbd>.</span></span> <span data-ttu-id="d3e8c-205">**[Internet Explorer]** ダイアログ ボックスで、**[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-205">In the **Internet Explorer** dialog box, click **Add**.</span></span> <span data-ttu-id="d3e8c-206">**[信頼済みサイト]** ダイアログ ボックスで、**[追加]** をクリックし、**[閉じる]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-206">In the **Trusted Sites** dialog box, click **Add** and then click **Close**.</span></span>

1. <span data-ttu-id="d3e8c-207">ダウンロードのプロンプトが自動的に表示されない場合は、再度 URL を入力して、<kbd>Enter</kbd> キーを押します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-207">If the download prompt does not appear automatically, you may need to enter the URL again and press <kbd>ENTER</kbd>.</span></span> <span data-ttu-id="d3e8c-208">**[Internet Explorer]** ダイアログ ボックスで **[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-208">In the **Internet Explorer** dialog box, click **Save**.</span></span> <span data-ttu-id="d3e8c-209">ダウンロードが完了したら、**[フォルダーを開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-209">Once the download is complete, click **Open folder**.</span></span>

1. <span data-ttu-id="d3e8c-210">**[ダウンロード]** フォルダーで **CPUSTRES** をダブルクリックし、**CPUSTRES** アプリケーションをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-210">In the **Downloads** folder, double-click **CPUSTRES**, and then double-click the **CPUSTRES** application.</span></span> <span data-ttu-id="d3e8c-211">**[圧縮 (zip 形式) フォルダー]** ダイアログ ボックスで **[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-211">In the **Compressed (zipped) folders** dialog box, click **Run**.</span></span>

1. <span data-ttu-id="d3e8c-212">**[CPU Stress]\(CPU ストレス\)** ウィンドウの **[Thread 1]\(スレッド 1\)** で、**[Activity]\(アクティビティ\)** ドロップダウンから **[Maximum]\(最大\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-212">In the **CPU Stress** window, under **Thread 1**, in the **Activity** drop-down, select **Maximum**.</span></span> <span data-ttu-id="d3e8c-213">**[Thread 2]\(スレッド 2\)** で **[Active]\(アクティブ\)** チェック ボックスをオンにし、**[Activity]\(アクティビティ\)** ドロップダウン内部の下向き矢印をクリックして **[Maximum]\(最大\)** を選択します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-213">Under **Thread 2**, click the **Active** checkbox, and in the **Activity** drop-down, click the down arrorw inside the drop-down and select **Maximum**.</span></span> <span data-ttu-id="d3e8c-214">これらの設定はすぐに反映されます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-214">These settings take effect immediately.</span></span>

1. <span data-ttu-id="d3e8c-215">Windows の [スタート] ボタンをクリックし、**[タスク マネージャー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-215">Click the Windows Start button, then click **Task Manager**.</span></span> <span data-ttu-id="d3e8c-216">タスク マネージャーで **[詳細]** をクリックし、CPU が 75% を超えていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-216">In Task Manager, click **More details**, and confirm that the CPU is over 75%.</span></span>

1. <span data-ttu-id="d3e8c-217">スケール セット内の他の仮想マシンで **CPUSTRES** のインストールと起動を**繰り返し**、5 分間待ちます。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-217">**Repeat** the installation and launch of **CPUSTRES** on the other virtual machine in the scale set, and then wait for five minutes.</span></span>

1. <span data-ttu-id="d3e8c-218">Azure portal で、**<rgn>[サンドボックス リソース グループ名]</rgn>** リソース グループを参照します。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-218">In the Azure portal, browse to the **<rgn>[sandbox resource group name]</rgn>** resource group.</span></span> <span data-ttu-id="d3e8c-219">**<rgn>[サンド ボックス リソース グループ名]</rgn>** ブレードで、**WebServerSS** オブジェクトをクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-219">On the **<rgn>[sandbox resource group name]</rgn>** blade, click the **WebServerSS** object.</span></span> <span data-ttu-id="d3e8c-220">**[WebServerSS]** ブレードで、**[インスタンス]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-220">On the **WebServerSS** blade, click **Instances**.</span></span> <span data-ttu-id="d3e8c-221">スケール セット内に表示されている仮想マシン インスタンス数とそれらのステータスをメモします。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-221">Note how many virtual machine instances are displayed within the scale set and the status.</span></span>

<span data-ttu-id="d3e8c-222">ここでは、仮想マシンが 2 つの仮想マシン スケールセットを作成しました。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-222">Here, you created a virtual machine scale set with two virtual machines.</span></span> <span data-ttu-id="d3e8c-223">さらに、そのスケールセットで自動的にスケールアウトとスケールインする規則を作成し、それら規則を自動スケール プロファイルに追加しました。</span><span class="sxs-lookup"><span data-stu-id="d3e8c-223">You then created rules to automatically scale out and scale in the scale set, and you added the rules to an autoscale profile.</span></span>
