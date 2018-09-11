<span data-ttu-id="1abe7-101">Azure に Linux VM を作成したので、次に Azure に移行するタスク用に VM を構成します。</span><span class="sxs-lookup"><span data-stu-id="1abe7-101">Now that we have a Linux VM in Azure, the next thing you’ll want to do is configure it for the tasks we want to move to Azure.</span></span>

<span data-ttu-id="1abe7-102">Azure に対してサイト間 VPN を設定していない場合、Azure VM にはローカル ネットワークからアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="1abe7-102">Unless you’ve set up a site-to-site VPN to Azure, your Azure VMs won’t be accessible from your local network.</span></span> <span data-ttu-id="1abe7-103">Azure を使い始めたばかりの場合は、きっとサイト間 VPN を設定していないでしょう。</span><span class="sxs-lookup"><span data-stu-id="1abe7-103">If you’re just getting started with Azure, it’s unlikely that you have a working site-to-site VPN.</span></span> <span data-ttu-id="1abe7-104">それでは、どうすれば仮想マシンに接続できるでしょうか。</span><span class="sxs-lookup"><span data-stu-id="1abe7-104">So how can you connect to the virtual machine?</span></span>

## <a name="azure-virtual-machines-ip-addresses"></a><span data-ttu-id="1abe7-105">Azure 仮想マシンの IP アドレス</span><span class="sxs-lookup"><span data-stu-id="1abe7-105">Azure virtual machines IP addresses</span></span>

<span data-ttu-id="1abe7-106">少し前に説明したように、Azure VM は仮想ネットワーク経由で通信します。</span><span class="sxs-lookup"><span data-stu-id="1abe7-106">As we saw a moment ago, Azure VMs communicate on a virtual network.</span></span> <span data-ttu-id="1abe7-107">必要に応じてパブリック IP アドレスを VM に割り当てることもできます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-107">They can also have an optional public IP address assigned to them.</span></span> <span data-ttu-id="1abe7-108">パブリック IP を使用すると、インターネット経由で VM と対話できます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-108">With a public IP, we can interact with the VM over the Internet.</span></span> <span data-ttu-id="1abe7-109">または、オンプレミスのネットワークを Azure に接続する仮想プライベート ネットワーク (VPN) を設定できます。そうすれば、パブリック IP アドレスを公開することなく、安全に VM に接続できます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-109">Alternatively, we can set up a virtual private network (VPN) that connects our on-premises network to Azure - letting us securely connect to the VM without exposing a public IP.</span></span> <span data-ttu-id="1abe7-110">そのオプションを調べてみたい場合は、別のモジュールでこのアプローチが取り上げられており、詳細なドキュメントもあります。</span><span class="sxs-lookup"><span data-stu-id="1abe7-110">This approach is covered in another module and is fully documented if you are interested in exploring that option.</span></span>

<span data-ttu-id="1abe7-111">Azure でのパブリック IP アドレスに関して注意すべき 1 つの点は、多くの場合、それが動的に割り当てられることです。</span><span class="sxs-lookup"><span data-stu-id="1abe7-111">One thing to be aware of with public IP addresses in Azure is they are often dynamically allocated.</span></span> <span data-ttu-id="1abe7-112">つまり、時間が経つと IP アドレスが変わる可能性があります。VM の場合、これは VM の再起動時に行われます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-112">That means the IP address can change over time - for VMs this happens when the VM is restarted.</span></span> <span data-ttu-id="1abe7-113">名前ではなく IP アドレスに直接接続し、IP アドレスが変化しないようにしたい場合は、静的アドレスを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-113">You can pay more to assign static addresses if you want to connect directly to an IP address instead of a name and need to ensure that the IP address will not change.</span></span>

## <a name="connect-to-an-azure-linux-virtual-machines-with-ssh"></a><span data-ttu-id="1abe7-114">SSH を使用して Azure の Linux 仮想マシンに接続する</span><span class="sxs-lookup"><span data-stu-id="1abe7-114">Connect to an Azure Linux virtual machines with SSH</span></span>

<span data-ttu-id="1abe7-115">SSH を使用して Azure の VM に簡単に接続できます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-115">Connecting to a VM in Azure using SSH is straight forward.</span></span> <span data-ttu-id="1abe7-116">Azure portal で、VM のプロパティを開き、一番上にある **[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="1abe7-116">In the Azure portal, open the properties of your VM and at the top, click **Connect**.</span></span> <span data-ttu-id="1abe7-117">VM に割り当てられている IP アドレスと、SSH に関するログインの詳細がすべて表示されます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-117">This will show you the IP addresses assigned to the VM along with all the login details for SSH.</span></span> 

![Linux VM に関する SSH 接続の詳細](../media-drafts/5-connect-ssh.png)

<span data-ttu-id="1abe7-119">これらの詳細がわかれば、次のコマンドを使用して Linux VM に接続できます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-119">With these details, we can use the following command to get into our Linux VM:</span></span>

```bash
ssh jim@137.117.101.249
```

<span data-ttu-id="1abe7-120">初めて接続すると、SSH から不明なホストに対する認証について尋ねられます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-120">The first time we connect, SSH will ask us about authenticating against an unknown host.</span></span> <span data-ttu-id="1abe7-121">SSH は、ユーザーがこれまでこのサーバーに接続していないことを示します。</span><span class="sxs-lookup"><span data-stu-id="1abe7-121">SSH is telling you that you've never connected to this server before.</span></span> <span data-ttu-id="1abe7-122">それが正しい場合、それはまったく普通のことであり、"yes" と応答してサーバーのフィンガープリントを既知のホスト ファイルに保存できます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-122">If that's true, then it's perfectly normal, and you can respond with "yes" to save the fingerprint of the server off in the known host file.</span></span>

```output
The authenticity of host '137.117.101.249 (137.117.101.249)' can't be established.
ECDSA key fingerprint is SHA256:w1h08h4ie1iMq7ibIVSQM/PhcXFV7O7EEhjEqhPYMWY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '137.117.101.249' (ECDSA) to the list of known hosts.
```

## <a name="transferring-files-to-the-vm"></a><span data-ttu-id="1abe7-123">VM にファイルを転送する</span><span class="sxs-lookup"><span data-stu-id="1abe7-123">Transferring files to the VM</span></span>

<span data-ttu-id="1abe7-124">もう 1 つの一般的なタスクは、あるサーバーから別のサーバーにローカル ファイルまたはデータをコピーすることです。</span><span class="sxs-lookup"><span data-stu-id="1abe7-124">Another common task is to copy local files or data from one server to another.</span></span> <span data-ttu-id="1abe7-125">ここでは、最終的に、Web サイトのデータを Azure VM にコピーします。</span><span class="sxs-lookup"><span data-stu-id="1abe7-125">In our case, we'll eventually want to copy our website data up to our Azure VM.</span></span> <span data-ttu-id="1abe7-126">Linux VM と SSH では、`scp` コマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-126">With Linux VMs and SSH, we can use the `scp` command.</span></span> <span data-ttu-id="1abe7-127">コマンドは `cp` でのローカル ファイルのコピーに似ていますが、リモート ユーザーとホストをコマンドで指定する必要がある点が異なります。</span><span class="sxs-lookup"><span data-stu-id="1abe7-127">The command is similar to copying local files with `cp`, except you'll have to specify the remote user and host in your command.</span></span> 

<span data-ttu-id="1abe7-128">たとえば、IP アドレスが `192.168.1.25` である Linux マシン上の `~/folder` に現在のディレクトリから `somefile.txt` をコピーするには、次のように入力できます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-128">For example, to copy `somefile.txt` from our current directory to `~/folder` on a Linux machine with the IP address `192.168.1.25`, you can type:</span></span>

```bash
scp somefile.txt someuser@192.168.1.25:~/folder/
```

<span data-ttu-id="1abe7-129">これはリモート システム上で `someuser` として認証され、パスワードの入力を求められるか、またはユーザーの SSH 秘密キーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="1abe7-129">This will authenticate as `someuser` on the remote system, prompting for a password, or using your SSH private key.</span></span> <span data-ttu-id="1abe7-130">そのユーザーは、コピー先サーバー上の `~/folder/` に対する書き込みアクセス許可を持っている必要があります。</span><span class="sxs-lookup"><span data-stu-id="1abe7-130">That user will need to have write permissions to `~/folder/` on the destination server.</span></span>

> [!WARNING]
> <span data-ttu-id="1abe7-131">コマンド ラインに正確に入力しないと、`scp` はローカル ファイルのコピーを行います。</span><span class="sxs-lookup"><span data-stu-id="1abe7-131">`scp` will do local file copies if you don't get the command line quite right.</span></span> <span data-ttu-id="1abe7-132">指定し忘れることが最も多い部分は、コピー先のフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="1abe7-132">The most common missing piece is the destination folder.</span></span>

<span data-ttu-id="1abe7-133">SSH を使用して、実行中の Linux VM に接続してみましょう。</span><span class="sxs-lookup"><span data-stu-id="1abe7-133">Let's try using SSH to connect to our running Linux VM.</span></span>