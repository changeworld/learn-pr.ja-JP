<span data-ttu-id="e82ea-101">この演習では、RDP クライアントを使用して、前の演習で作成した Windows VM に接続します。</span><span class="sxs-lookup"><span data-stu-id="e82ea-101">In this exercise, you'll use the RDP client to connect to the Windows VM that you created in the previous unit.</span></span> <span data-ttu-id="e82ea-102">Azure portal から RDP ファイルをダウンロードして実行することで接続できます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-102">You can connect by downloading and running an RDP file from the Azure portal.</span></span> <span data-ttu-id="e82ea-103">この RDP ファイルには次のものが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-103">This RDP file will have:</span></span>

* <span data-ttu-id="e82ea-104">VM のパブリック IP アドレス。</span><span class="sxs-lookup"><span data-stu-id="e82ea-104">The public IP address of the VM.</span></span>
* <span data-ttu-id="e82ea-105">ポート番号。</span><span class="sxs-lookup"><span data-stu-id="e82ea-105">The port number.</span></span>

## <a name="motivation"></a><span data-ttu-id="e82ea-106">目的</span><span class="sxs-lookup"><span data-stu-id="e82ea-106">Motivation</span></span>

<span data-ttu-id="e82ea-107">この演習シナリオでは、皆さんは学生であり、Windows Server コンピューターにロールと機能を追加する方法について学習する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e82ea-107">From our exercise scenario, you're now a student and you want to learn about adding roles and features to a Windows Server computer.</span></span> <span data-ttu-id="e82ea-108">しかし、ネットワーク管理者は学生がネットワーク上の物理コンピューターでこれを行うことを望んでおらず、学校のコンピューターは Windows Hyper-V を実行するように適切に指定されていません。</span><span class="sxs-lookup"><span data-stu-id="e82ea-108">However, your network administrator doesn't want you to do this on a physical computer on the network, and the school's computers are too poorly specified to run Windows Hyper-V.</span></span> <span data-ttu-id="e82ea-109">Azure では最適なソリューションが提供されます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-109">Azure provides the perfect solution.</span></span>

## <a name="configure-network-and-public-ip-address-settings"></a><span data-ttu-id="e82ea-110">ネットワークとパブリック IP アドレスの設定を構成する</span><span class="sxs-lookup"><span data-stu-id="e82ea-110">Configure network and public IP address settings</span></span>

1. <span data-ttu-id="e82ea-111">Azure portal で、先ほど作成した仮想マシン用のブレードが開いていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="e82ea-111">In the Azure portal, ensure the blade for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="e82ea-112">ブレードを開く必要がある場合は、**[すべてのリソース]** で見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-112">You can find the blade under **All Resources** if you need to open it.</span></span>

1. <span data-ttu-id="e82ea-113">**[ネットワーク]** セクションに移動します。</span><span class="sxs-lookup"><span data-stu-id="e82ea-113">Go to the **Networking** section.</span></span> <span data-ttu-id="e82ea-114">このセクションの上部には、仮想サブネットおよび動的 IP アドレスへのリンクがあります。既定値を使用したため、これらは VM とともに作成されています。</span><span class="sxs-lookup"><span data-stu-id="e82ea-114">The top of this section has links to the virtual subnet and dynamic IP address that were created along with our VM since we used the defaults.</span></span> <span data-ttu-id="e82ea-115">リソースを変更する (たとえば、静的 IP アドレスを変更する) 必要がある場合は、これらのリンクに従います。</span><span class="sxs-lookup"><span data-stu-id="e82ea-115">We would follow these links if we wanted to change those resources (for example, changing to a static IP address).</span></span>

1. <span data-ttu-id="e82ea-116">**[受信ポート規則を追加する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-116">Click **Add inbound port rule**.</span></span>

1. <span data-ttu-id="e82ea-117">**[受信セキュリティ規則の追加]** ダイアログ ボックスの上部にある **[basic]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-117">At the top of the **Add inbound security rule** dialog box, click **basic**.</span></span>

1. <span data-ttu-id="e82ea-118">**[サービス]** で、**[RDP]** を選択してから **[追加]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-118">Under **Service**, select **RDP**, and then click **Add**.</span></span>

## <a name="connect-to-the-vm-by-using-rdp"></a><span data-ttu-id="e82ea-119">RDP を使用して VM に接続する</span><span class="sxs-lookup"><span data-stu-id="e82ea-119">Connect to the VM by using RDP</span></span>

<span data-ttu-id="e82ea-120">RDP ファイルをダウンロードして VM に接続するには、次の手順を行います。</span><span class="sxs-lookup"><span data-stu-id="e82ea-120">To download the RDP file and connect to the VM, carry out the following steps.</span></span>

### <a name="download-the-rdp-file"></a><span data-ttu-id="e82ea-121">RDP ファイルのダウンロード</span><span class="sxs-lookup"><span data-stu-id="e82ea-121">Download the RDP file</span></span>

1. <span data-ttu-id="e82ea-122">仮想マシンのブレードの **[概要]** セクションで、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-122">In the **Overview** section of the virtual machine's blade, click **Connect**.</span></span>

1. <span data-ttu-id="e82ea-123">**[仮想マシンへの接続]** ブレードで、**IP アドレス**と**ポート番号**の設定内容をメモしてから、**[RDP ファイルのダウンロード]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-123">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings, then click **Download RDP File**.</span></span>

1. <span data-ttu-id="e82ea-124">ブラウザーで、**[開く]** または **[実行]** をクリックして RDP ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-124">In your browser, click **Open** or **Run** to open the RDP file.</span></span>

### <a name="connect-to-the-windows-vm"></a><span data-ttu-id="e82ea-125">Windows VM に接続する</span><span class="sxs-lookup"><span data-stu-id="e82ea-125">Connect to the Windows VM</span></span>

1. <span data-ttu-id="e82ea-126">**[リモート デスクトップ接続]** ダイアログ ボックスで、セキュリティに関する警告とリモート コンピューターの IP アドレスをメモしてから **[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-126">In the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then click **Connect**.</span></span>

1. <span data-ttu-id="e82ea-127">**[Windows セキュリティ]** ダイアログ ボックスに、手順 6 と 7 で使用したユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="e82ea-127">In the **Windows Security** dialog box, enter your user name and password that you used in steps 6 and 7.</span></span>

1. <span data-ttu-id="e82ea-128">2 番目の **[リモート デスクトップ接続]** ダイアログ ボックスで、証明書に関するエラーをメモしてから **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-128">In the second **Remote Desktop Connection** dialog box, note the certificate errors, then click **Yes**.</span></span>

   > [!Note]
   > <span data-ttu-id="e82ea-129">仮想マシンのデスクトップが表示されるまでしばらく時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="e82ea-129">The virtual machine desktop takes a while to appear.</span></span> <span data-ttu-id="e82ea-130">これは、B1 イメージの指定が不完全であるためです。</span><span class="sxs-lookup"><span data-stu-id="e82ea-130">This effect is because the B1 image is under-specified.</span></span> <span data-ttu-id="e82ea-131">低メモリの割り当てに関するメッセージが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="e82ea-131">You may receive a message about low memory allocation.</span></span>

1. <span data-ttu-id="e82ea-132">**[ネットワーク]** ダイアログで、**[いいえ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-132">In the **Networks** dialog, click **No**.</span></span>

### <a name="resize-the-vm-in-the-azure-portal"></a><span data-ttu-id="e82ea-133">Azure portal で VM のサイズを変更する</span><span class="sxs-lookup"><span data-stu-id="e82ea-133">Resize the VM in the Azure portal</span></span>

1. <span data-ttu-id="e82ea-134">Azure portal に戻ります。</span><span class="sxs-lookup"><span data-stu-id="e82ea-134">Switch back to the Azure portal.</span></span> <span data-ttu-id="e82ea-135">仮想マシンのプロパティ ページで、**[設定]** の下にある **[サイズ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-135">On the virtual machine properties page, under **Settings**, click **Size**.</span></span>

1. <span data-ttu-id="e82ea-136">**[D2s_v3]** (2 vCPU、8 GB RAM)、**[選択]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-136">Click **D2s_v3** (2 vCPUs, 8-GB RAM), and then click **Select**.</span></span> <span data-ttu-id="e82ea-137">仮想マシンのサイズ変更に関するメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-137">A resizing virtual machine message will display.</span></span> <span data-ttu-id="e82ea-138">また、RDP ウィンドウ内の VM が閉じられます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-138">The VM in the RDP window will also close down.</span></span>

1. <span data-ttu-id="e82ea-139">Azure portal に戻ります。</span><span class="sxs-lookup"><span data-stu-id="e82ea-139">Switch back to the Azure portal.</span></span> <span data-ttu-id="e82ea-140">左側のウィンドウで、**[仮想マシン]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-140">In the left pane, click **Virtual machines**.</span></span>

1. <span data-ttu-id="e82ea-141">**[仮想マシン]** で、ご利用の VM の状態が **[実行中]** になるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-141">Under **Virtual machines**, wait until your VM shows a status of **Running**.</span></span> <span data-ttu-id="e82ea-142">**[更新]** をクリックする必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="e82ea-142">You may need to click **Refresh**.</span></span>

1. <span data-ttu-id="e82ea-143">仮想マシンの名前をクリックしてから、**[設定]** で **[サイズ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-143">Click the virtual machine's name, then in **Settings**, click **Size**.</span></span> <span data-ttu-id="e82ea-144">これで VM のサイズが **[D2s_v3]** に設定されたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e82ea-144">Note the VM size is now set to **D2s_v3**.</span></span>

### <a name="reconnect-to-the-resized-vm"></a><span data-ttu-id="e82ea-145">サイズを変更した VM に再接続する</span><span class="sxs-lookup"><span data-stu-id="e82ea-145">Reconnect to the resized VM</span></span>

1. <span data-ttu-id="e82ea-146">**[仮想マシン]**、ご利用の VM の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-146">Click **Virtual machines**, then click your VM.</span></span> <span data-ttu-id="e82ea-147">**[パブリック IP アドレス]** の値が変更された可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e82ea-147">Note the **Public IP Address** value has probably changed.</span></span> <span data-ttu-id="e82ea-148">**[接続]** をクリックします。次に、ブラウザーで **[実行]** または **[開く]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-148">Click **Connect**, and then click **Run** or **Open** in your browser.</span></span>

1. <span data-ttu-id="e82ea-149">**[リモート デスクトップ接続]** ダイアログ ボックスで、セキュリティに関する警告とリモート コンピューターの IP アドレスをメモしてから **[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-149">In the **Remote Desktop Connection** dialog box, note the security warning and the remote computer IP address, then click **Connect**.</span></span>

1. <span data-ttu-id="e82ea-150">**[Windows セキュリティ]** ダイアログ ボックスに、手順 6 と 7 で使用したユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="e82ea-150">In the **Windows Security** dialog box, enter your user name and password that you used in steps 6 and 7.</span></span>

1. <span data-ttu-id="e82ea-151">2 番目の **[リモート デスクトップ接続]** ダイアログ ボックスで、証明書に関するエラーをメモしてから **[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-151">In the second **Remote Desktop Connection** dialog box, note the certificate errors, then click **Yes**.</span></span> <span data-ttu-id="e82ea-152">仮想マシンでのサインイン プロセスに対する応答性がどれだけ向上するかに注目してください。</span><span class="sxs-lookup"><span data-stu-id="e82ea-152">Notice how much more responsive the virtual machine is to the sign-in process.</span></span>

1. <span data-ttu-id="e82ea-153">タスク バーを右クリックし、**[タスク マネージャー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-153">Right-click the taskbar and click **Task Manager**.</span></span>

1. <span data-ttu-id="e82ea-154">**[タスク マネージャー]** ウィンドウで、**[詳細]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="e82ea-154">In the **Task Manager** window, click **More details**.</span></span>

1. <span data-ttu-id="e82ea-155">**[パフォーマンス]** タブをクリックします。利用可能なメモリの合計が、使用される約 1.2 GB のうち 8 GB になっていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e82ea-155">Click the **Performance** tab. Note the total available memory is 8 GB of which about 1.2 GB will be in use.</span></span> <span data-ttu-id="e82ea-156">**[タスク マネージャー]** を閉じます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-156">Close **Task Manager**.</span></span>

## <a name="summary"></a><span data-ttu-id="e82ea-157">まとめ</span><span class="sxs-lookup"><span data-stu-id="e82ea-157">Summary</span></span>

<span data-ttu-id="e82ea-158">RDP を使用して Windows VM に接続しました。</span><span class="sxs-lookup"><span data-stu-id="e82ea-158">You have connected to a Windows VM by using RDP.</span></span> <span data-ttu-id="e82ea-159">デスクトップ UI アクセスでは、すべての Windows コンピューターの場合と同じように、この VM を管理できます。</span><span class="sxs-lookup"><span data-stu-id="e82ea-159">With Desktop UI access, you can administer this VM as you would any Windows computer.</span></span>
