<span data-ttu-id="5caea-101">データ分析企業のネットワーク管理者であるあなたは、Azure にある仮想マシンに接続し、仮想マシンが正しく構成されていることを確認する仕事を担当します。</span><span class="sxs-lookup"><span data-stu-id="5caea-101">As a network administrator at a data analysis company, you're responsible for connecting to virtual machines in Azure and checking that they're properly configured.</span></span> <span data-ttu-id="5caea-102">この仕事は、リモート デスクトップ プロトコル クライアントを使用し、VM の Windows デスクトップ UI に接続することで行います。</span><span class="sxs-lookup"><span data-stu-id="5caea-102">You do that using a Remote Desktop Protocol client to connect to the VM's Windows desktop UI.</span></span>

## <a name="what-is-the-remote-desktop-protocol"></a><span data-ttu-id="5caea-103">リモート デスクトップ プロトコルとは</span><span class="sxs-lookup"><span data-stu-id="5caea-103">What is the Remote Desktop Protocol?</span></span>

<span data-ttu-id="5caea-104">リモート デスクトップ プロトコル (RDP) は、Windows ベース コンピューターの UI へのリモート接続を提供します。</span><span class="sxs-lookup"><span data-stu-id="5caea-104">Remote Desktop Protocol (RDP) provides remote connectivity to the UI of Windows-based computers.</span></span> <span data-ttu-id="5caea-105">RDP を使用すると、離れた場所にある物理または仮想 Windows コンピューターにサインインし、まるでコンソールで操作しているかのようにそのコンピューターを制御できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-105">RDP enables you to sign in to a remote physical or virtual Windows computer and control that computer as if you were seated at the console.</span></span>

> [!Note]
> <span data-ttu-id="5caea-106">Windows Server 2016 と Windows 10 の Windows Hyper-V では RDP を利用し、ハイパーバイザーで実行されている仮想マシンに接続します。</span><span class="sxs-lookup"><span data-stu-id="5caea-106">Windows Hyper-V in Windows Server 2016 and Windows 10 uses RDP to connect to virtual machines running on the hypervisor.</span></span>

<span data-ttu-id="5caea-107">RDP 接続を利用すると、電源とハードウェアに関連する一部の機能を除き、物理コンピューターのコンソールからできる作業のほとんどを実行できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-107">An RDP connection enables you to carry out the vast majority of operations that you can do from the console of a physical computer, with the exception of some power and hardware-related functions.</span></span>

<span data-ttu-id="5caea-108">RDP 接続には RDP クライアントが必要です。</span><span class="sxs-lookup"><span data-stu-id="5caea-108">An RDP connection requires an RDP client.</span></span> <span data-ttu-id="5caea-109">Microsoft では、次のオペレーティング システム用の RDP クライアントを提供しています。</span><span class="sxs-lookup"><span data-stu-id="5caea-109">Microsoft provides RDP clients for the following operating systems:</span></span>

* <span data-ttu-id="5caea-110">Windows</span><span class="sxs-lookup"><span data-stu-id="5caea-110">Windows</span></span>
* <span data-ttu-id="5caea-111">iOS</span><span class="sxs-lookup"><span data-stu-id="5caea-111">iOS</span></span>
* <span data-ttu-id="5caea-112">MacOS</span><span class="sxs-lookup"><span data-stu-id="5caea-112">MacOS</span></span>
* <span data-ttu-id="5caea-113">Android</span><span class="sxs-lookup"><span data-stu-id="5caea-113">Android</span></span>

<span data-ttu-id="5caea-114">オープンソースの Linux クライアントもあり、たとえば、Remmina を利用すれば、Ubuntu ディストリビューションから Windows PC に接続できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-114">There are also open-source Linux clients, such as Remmina that enable you to connect to a Windows PC from an Ubuntu distribution.</span></span>

<span data-ttu-id="5caea-115">Windows 10 には RDP クライアントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="5caea-115">Windows 10 includes an RDP client.</span></span>

![Windows RDP クライアント](../images/2-rdp-client.PNG)

## <a name="what-functionality-does-an-rdp-connection-support"></a><span data-ttu-id="5caea-117">RDP 接続ではどのような機能がサポートされていますか?</span><span class="sxs-lookup"><span data-stu-id="5caea-117">What functionality does an RDP connection support?</span></span>

<span data-ttu-id="5caea-118">RDP 接続を利用すると、UI とやり取りできます。</span><span class="sxs-lookup"><span data-stu-id="5caea-118">With an RDP connection, you can interact with the UI.</span></span> <span data-ttu-id="5caea-119">もっとも、リモート コンピューター上の他のサービスにも接続できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-119">However, you can also connect to other services on the remote computer.</span></span> <span data-ttu-id="5caea-120">次のようなサービスがあります。</span><span class="sxs-lookup"><span data-stu-id="5caea-120">These services include:</span></span>

* <span data-ttu-id="5caea-121">プリンター接続。リモート コンピューターからローカル印刷デバイスに出力できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-121">Printer connections that enable the remote computer to print to your local print device.</span></span>
* <span data-ttu-id="5caea-122">オーディオ再生。ローカル コンピューターかリモート デバイスでオーディオを再生できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-122">Audio playback, where audio can play either on the local computer or on the remote device.</span></span>
* <span data-ttu-id="5caea-123">オーディオ録音。ローカル コンピューターからオーディオを録音し、そのサウンドをリモート デバイスに送ることができます。</span><span class="sxs-lookup"><span data-stu-id="5caea-123">Audio recording, where you can record audio from the local computer and direct that sound to the remote device.</span></span>
* <span data-ttu-id="5caea-124">ポート。ローカル コンピューターのポートにリダイレクトできます。</span><span class="sxs-lookup"><span data-stu-id="5caea-124">Ports, which you can redirect to the ports on the local computer.</span></span>
* <span data-ttu-id="5caea-125">デバイス。マッピングされたネットワーク ドライブを含む。リモート コンピューターに接続されているものとしてドライブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="5caea-125">Drives, including mapped network drives, where the drives will appear as connected to the remote computer.</span></span>
* <span data-ttu-id="5caea-126">ビデオ キャプチャ デバイス。一体型の Web カメラなど。</span><span class="sxs-lookup"><span data-stu-id="5caea-126">Video capture devices, such as integrated web cams.</span></span>
* <span data-ttu-id="5caea-127">サポートされているその他のプラグ アンド プレイ デバイス。</span><span class="sxs-lookup"><span data-stu-id="5caea-127">Other supported plug and play devices.</span></span>

<span data-ttu-id="5caea-128">Azure では以上のすべての機能がサポートされているわけではありません。Azure プラットフォームでは、利用できる物理アセットに制約があるためです。</span><span class="sxs-lookup"><span data-stu-id="5caea-128">Not all of these features are supported in Azure, as there are restrictions on what physical assets are available on that platform.</span></span> <span data-ttu-id="5caea-129">たとえば、Azure がホストする VM から接続クライアントの印刷デバイスに RDP 接続経由でリダイレクトできるのはソフトウェア プリンターだけです。</span><span class="sxs-lookup"><span data-stu-id="5caea-129">For example, only software printers can be redirected across an RDP connection from an Azure-hosted VM to the connecting client's print devices.</span></span>

## <a name="configure-network-settings-for-rdp-access-to-virtual-machines"></a><span data-ttu-id="5caea-130">仮想マシンに RDP アクセスするためのネットワーク設定を構成する</span><span class="sxs-lookup"><span data-stu-id="5caea-130">Configure network settings for RDP access to virtual machines</span></span>

<span data-ttu-id="5caea-131">Azure VM には 3 通りの方法で接続できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-131">Connections to Azure VMs can be made in three ways:</span></span>

1. <span data-ttu-id="5caea-132">VM のパブリック IP アドレスに直接。</span><span class="sxs-lookup"><span data-stu-id="5caea-132">Directly to the public IP address of the VM.</span></span>
2. <span data-ttu-id="5caea-133">仮想プライベート ネットワーク (VPN) 接続を通して。</span><span class="sxs-lookup"><span data-stu-id="5caea-133">Through a virtual private network (VPN) connection.</span></span>
3. <span data-ttu-id="5caea-134">ExpressRoute 接続を通して。</span><span class="sxs-lookup"><span data-stu-id="5caea-134">Through an ExpressRoute connection.</span></span>

<span data-ttu-id="5caea-135">パブリック IP アドレスは固定で割り当てるか、動的に割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="5caea-135">A public IP address can be either permanently allocated or dynamic.</span></span> <span data-ttu-id="5caea-136">ポート 3389 (RDP) で VM のパブリック アドレスにアクセスできることも確認しておく必要があります。</span><span class="sxs-lookup"><span data-stu-id="5caea-136">You also need to ensure that you have access to the VM's public address on port 3389 (RDP).</span></span> <span data-ttu-id="5caea-137">このプロビジョニングには、組織のファイアウォール セキュリティを管理し、ポート 3389 で VM のパブリック IP アドレスにルールを設定しているチームとの交渉が場合によっては必要になります。</span><span class="sxs-lookup"><span data-stu-id="5caea-137">This provision may require negotiation with the team that's managing your organization's firewall security, setting up a rule to your VM's public IP address on port 3389.</span></span>

<span data-ttu-id="5caea-138">VM が再起動するたびに、動的パブリック IP アドレスが再割り当てされます。</span><span class="sxs-lookup"><span data-stu-id="5caea-138">Dynamic public IP addresses are reallocated every time the VM restarts.</span></span> <span data-ttu-id="5caea-139">静的アドレスは固定ですが、コストが多くなります。</span><span class="sxs-lookup"><span data-stu-id="5caea-139">Static addresses are persistent, but cost more.</span></span>

<span data-ttu-id="5caea-140">VPN 経由で接続するには、ローカル ネットワークに Microsoft Azure へのアクティブな VPN 接続を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="5caea-140">To connect through a VPN, your local network must have an active VPN connection to Microsoft Azure.</span></span>

<span data-ttu-id="5caea-141">ExpressRoute リンクの手法では、VM への接続が管理されます。</span><span class="sxs-lookup"><span data-stu-id="5caea-141">The ExpressRoute link approach manages connectivity to the VM.</span></span> <span data-ttu-id="5caea-142">この手法ではまた、待ち時間が最も短く、接続の帯域幅が最も広くなります。</span><span class="sxs-lookup"><span data-stu-id="5caea-142">This approach also provides the lowest latency and highest bandwidth connection.</span></span>

> [!Note]
> <span data-ttu-id="5caea-143">Azure への接続に使用しているオプションに関係なく、RDP 経由で仮想マシンにアクセスできることを確認する必要があります。通常、既定のポート 3389 でアクセスします。</span><span class="sxs-lookup"><span data-stu-id="5caea-143">Regardless of which option you use to connect to Azure, you need to ensure the virtual machine is accessible over RDP, typically on the default port 3389.</span></span> <span data-ttu-id="5caea-144">独自のクライアント IP アドレスからのみアクセスできるように VM を構成できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-144">You can configure the VM so it can be accessed only from your own client IP address.</span></span> <span data-ttu-id="5caea-145">パブリック IP アドレスによるポート 3389 経由の VM 接続はテスト環境でのみ推奨されます。</span><span class="sxs-lookup"><span data-stu-id="5caea-145">Connecting to a VM over port 3389 on a public IP address is only recommended for test environments.</span></span> <span data-ttu-id="5caea-146">運用環境では、オプション 2 または 3 を使用してください。</span><span class="sxs-lookup"><span data-stu-id="5caea-146">In production environments, use option 2 or 3.</span></span>

## <a name="how-do-you-connect-to-a-vm-in-azure-using-rdp"></a><span data-ttu-id="5caea-147">RDP を使用して Azure の VM に接続するにはどうすればよいですか?</span><span class="sxs-lookup"><span data-stu-id="5caea-147">How do you connect to a VM in Azure using RDP?</span></span>

<span data-ttu-id="5caea-148">RDP を使用した Azure の VM への接続はシンプルなプロセスです。</span><span class="sxs-lookup"><span data-stu-id="5caea-148">Connecting to a VM in Azure using RDP is a simple process.</span></span> <span data-ttu-id="5caea-149">Azure portal で、お使いの VM のプロパティに移動し、一番上にある **[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="5caea-149">In the Azure portal, you go to the properties of your VM, and at the top, click **Connect**.</span></span> <span data-ttu-id="5caea-150">これで事前構成済みの .RDP ファイルがダウンロードされます。Windows により、このファイルが RDP クライアントで開きます。</span><span class="sxs-lookup"><span data-stu-id="5caea-150">This action downloads a preconfigured .RDP file that Windows then opens in the RDP client.</span></span> <span data-ttu-id="5caea-151">RDP ファイルの VM のパブリック IP アドレス経由で接続するように選択できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-151">You can choose to connect over the public IP address of the VM in the RDP file.</span></span> <span data-ttu-id="5caea-152">あるいは、VPN または ExpressRoute 経由で接続する場合、内部 IP アドレスを選択できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-152">Alternatively, if you're connecting over VPN or ExpressRoute, you can select the internal IP address.</span></span> <span data-ttu-id="5caea-153">接続のポート番号も選択できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-153">You can also select the port number for the connection.</span></span>

<span data-ttu-id="5caea-154">VM の静的パブリック IP アドレスを使用している場合、.RDP ファイルをお使いのデスクトップに保存できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-154">If you're using a static public IP address for the VM, you can save the .RDP file to your desktop.</span></span> <span data-ttu-id="5caea-155">動的 IP アドレスを使用している場合、VM が実行されている間のみ、.RDP ファイルは有効となります。</span><span class="sxs-lookup"><span data-stu-id="5caea-155">If you're using dynamic IP addressing, the .RDP file only remains valid while the VM is running.</span></span> <span data-ttu-id="5caea-156">VM を停止し、再起動する場合、別の .RDP ファイルをダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5caea-156">If you stop and restart the VM, you must download another .RDP file.</span></span>

> [!Note]
> <span data-ttu-id="5caea-157">Windows RDP クライアントに VM のパブリック IP アドレスを入力し、**[接続]** をクリックすることもできます。</span><span class="sxs-lookup"><span data-stu-id="5caea-157">You can also enter the public IP address of the VM into the Windows RDP client and click **Connect**.</span></span>

<span data-ttu-id="5caea-158">接続すると、通常、2 つの警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5caea-158">When you connect, you will typically receive two warnings.</span></span> <span data-ttu-id="5caea-159">次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5caea-159">These are:</span></span>

* <span data-ttu-id="5caea-160">発行元警告 - .RDP ファイルに公的な署名がないことが警告の原因です。</span><span class="sxs-lookup"><span data-stu-id="5caea-160">Publisher warning - caused by the .RDP file not being publicly signed.</span></span>
* <span data-ttu-id="5caea-161">証明書警告 - コンピューター証明書が信頼されていないことが警告の原因です。</span><span class="sxs-lookup"><span data-stu-id="5caea-161">Certificate warning - caused by the machine certificate not being trusted.</span></span>

<span data-ttu-id="5caea-162">テスト環境では、これらの警告を無視できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-162">In test environments, these warnings can be ignored.</span></span> <span data-ttu-id="5caea-163">運用環境では、.RDP ファイルに RDPSIGN.EXE を利用して署名し、コンピューター証明書をクライアントの**信頼されたルート証明機関**ストアに配置するということが可能です。</span><span class="sxs-lookup"><span data-stu-id="5caea-163">In production environments, the .RDP file could be signed using RDPSIGN.EXE and the machine certificate placed in the client's **Trusted Root Certification Authorities** store.</span></span>

## <a name="summary"></a><span data-ttu-id="5caea-164">まとめ</span><span class="sxs-lookup"><span data-stu-id="5caea-164">Summary</span></span>

<span data-ttu-id="5caea-165">これで RDP を使用して Windows VM に接続できます。</span><span class="sxs-lookup"><span data-stu-id="5caea-165">Now you can connect to a Windows VM by using RDP.</span></span> <span data-ttu-id="5caea-166">次の演習では、RDP を使用して VM に接続し、そのコンピューターにサーバーの役割をインストールします。</span><span class="sxs-lookup"><span data-stu-id="5caea-166">In the following exercise, you will use RDP to connect to a VM, then install a server role on that computer.</span></span>
