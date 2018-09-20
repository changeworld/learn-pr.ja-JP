<span data-ttu-id="3a78c-101">Linux VM をデプロイして実行させましたが、作業を行うようには構成していません。</span><span class="sxs-lookup"><span data-stu-id="3a78c-101">We have our Linux VM deployed and running, but it's not configured to do any work.</span></span> <span data-ttu-id="3a78c-102">SSH で VM に接続して Apache を構成し、Web サーバーを実行させましょう。</span><span class="sxs-lookup"><span data-stu-id="3a78c-102">Let's connect to it with SSH and configure Apache, so we have a running web server.</span></span>

## <a name="connect-to-the-vm-with-ssh"></a><span data-ttu-id="3a78c-103">SSH を使って VM に接続する</span><span class="sxs-lookup"><span data-stu-id="3a78c-103">Connect to the VM with SSH</span></span>

<span data-ttu-id="3a78c-104">SSH クライアントを使って Azure VM に接続するには、次が必要になります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-104">To connect to an Azure VM with an SSH client, you will need:</span></span>

- <span data-ttu-id="3a78c-105">SSH クライアント ソフトウェア (ほとんどの最新オペレーティング システムに存在します)</span><span class="sxs-lookup"><span data-stu-id="3a78c-105">SSH client software (present on most modern operating systems)</span></span>
- <span data-ttu-id="3a78c-106">VM のパブリック IP アドレス (または、ユーザーのネットワークに接続するように VM を構成する場合はプライベート)。</span><span class="sxs-lookup"><span data-stu-id="3a78c-106">The public IP address of the VM (or private if the VM is configured to connect to your network)</span></span>

### <a name="get-the-public-ip-address"></a><span data-ttu-id="3a78c-107">パブリック IP アドレスの取得</span><span class="sxs-lookup"><span data-stu-id="3a78c-107">Get the public IP address</span></span>

1. <span data-ttu-id="3a78c-108">[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) 内で、以前に作成した仮想マシンの**概要**パネルが開いていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-108">In the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true), ensure the **Overview** panel for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="3a78c-109">もしパネルを開く必要がある場合は、**すべてのリソース**下で VM を見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-109">You can find the VM under **All Resources** if you need to open it.</span></span> <span data-ttu-id="3a78c-110">概要パネルには、VM に関する多数の情報があります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-110">The overview panel has a lot of information about the VM.</span></span>

    - <span data-ttu-id="3a78c-111">VM が実行中か否かを確認可能</span><span class="sxs-lookup"><span data-stu-id="3a78c-111">You can see whether the VM is running</span></span>
    - <span data-ttu-id="3a78c-112">VM の停止または再起動</span><span class="sxs-lookup"><span data-stu-id="3a78c-112">Stop or restart it</span></span>
    - <span data-ttu-id="3a78c-113">VM へ接続するためのパブリック IP アドレスを取得</span><span class="sxs-lookup"><span data-stu-id="3a78c-113">Get the public IP address to connect to the VM</span></span>
    - <span data-ttu-id="3a78c-114">CPU、ディスク、およびネットワークのアクティビティを確認</span><span class="sxs-lookup"><span data-stu-id="3a78c-114">See the activity of the CPU, disk, and network</span></span>

1. <span data-ttu-id="3a78c-115">ウィンドウ最上部の**接続**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3a78c-115">Click the **Connect** button at the top of the pane.</span></span>

1. <span data-ttu-id="3a78c-116">**仮想マシンへの接続**ブレード内で、**IP アドレス**および**ポート番号**設定をメモします。</span><span class="sxs-lookup"><span data-stu-id="3a78c-116">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings.</span></span> <span data-ttu-id="3a78c-117">**SSH**タブ上で、VM に接続するためにローカルで実行する必要があるコマンドも表示されています。</span><span class="sxs-lookup"><span data-stu-id="3a78c-117">On the **SSH** tab, you will also find the command you need to execute locally to connect to the VM.</span></span> <span data-ttu-id="3a78c-118">これをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="3a78c-118">Copy this to the clipboard.</span></span>

## <a name="connect-with-ssh"></a><span data-ttu-id="3a78c-119">SSH を使用して接続する</span><span class="sxs-lookup"><span data-stu-id="3a78c-119">Connect with SSH</span></span>

1. <span data-ttu-id="3a78c-120">[SSH] タブから取得したコマンド ラインを Azure Cloud Shell に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-120">Paste the command line you got from the SSH tab into Azure Cloud Shell.</span></span> <span data-ttu-id="3a78c-121">次のようになります。ただし、IP アドレスは異なります (また、**jim** を使用しなかった場合はユーザー名もおそらく異なります)。</span><span class="sxs-lookup"><span data-stu-id="3a78c-121">It should look something like this; however, it will have a different IP address (and perhaps a different username if you didn't use **jim**!):</span></span>

    ```bash
    ssh jim@137.117.101.249
    ```

1. <span data-ttu-id="3a78c-122">このコマンドを使用すると、Secure Shell 接続が開き、Linux 用の従来のシェル コマンド プロンプトに移動します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-122">This command will open a Secure Shell connection and place you at a traditional shell command prompt for Linux.</span></span>

1. <span data-ttu-id="3a78c-123">ここで Linux コマンドをいくつか実行してみてください。</span><span class="sxs-lookup"><span data-stu-id="3a78c-123">Try executing a few Linux commands</span></span>
    - <span data-ttu-id="3a78c-124">`ls -la /` を使ってディスクのルートを表示します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-124">`ls -la /` to show the root of the disk</span></span>
    - <span data-ttu-id="3a78c-125">`ps -l` を使って実行中のプロセスをすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-125">`ps -l` to show all the running processes</span></span>
    - <span data-ttu-id="3a78c-126">`dmesg` を使ってカーネル メッセージ一覧をすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-126">`dmesg` to list all the kernel messages</span></span>
    - <span data-ttu-id="3a78c-127">`lsblk` を使ってブロック デバイス一覧を全て表示します。ここにお使いのドライブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-127">`lsblk` to list all the block devices - here you will see your drives</span></span>

<span data-ttu-id="3a78c-128">ドライブの一覧でさらに興味深いことは、"_表示されていない_" デバイスです。</span><span class="sxs-lookup"><span data-stu-id="3a78c-128">The more interesting thing to observe in the list of drives is what is _missing_.</span></span> <span data-ttu-id="3a78c-129">**データ** ドライブ（`sdc`）が存在しているが、ファイル システムにはマウントされていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="3a78c-129">Notice that our **Data** drive (`sdc`) is present but not mounted into the file system.</span></span> <span data-ttu-id="3a78c-130">Azure で VHD が追加されていますが、初期化はされていません。</span><span class="sxs-lookup"><span data-stu-id="3a78c-130">Azure added a VHD but didn't initialize it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="3a78c-131">データ ディスクの初期化</span><span class="sxs-lookup"><span data-stu-id="3a78c-131">Initialize data disks</span></span>

<span data-ttu-id="3a78c-132">一から作成した追加ドライブは、初期化してフォーマットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-132">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="3a78c-133">これを行うためのプロセスは、物理ディスクと同じです。</span><span class="sxs-lookup"><span data-stu-id="3a78c-133">The process for doing this is identical to a physical disk:</span></span>

1. <span data-ttu-id="3a78c-134">最初に、ディスクを特定します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-134">First, identify the disk.</span></span> <span data-ttu-id="3a78c-135">これは上で行いました。</span><span class="sxs-lookup"><span data-stu-id="3a78c-135">We did that above.</span></span> <span data-ttu-id="3a78c-136">`dmesg | grep SCSI` を使用して、SCSI デバイス用のカーネルからすべてのメッセージを一覧表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-136">You could also use `dmesg | grep SCSI`, which will list all the messages from the kernel for SCSI devices.</span></span>

1. <span data-ttu-id="3a78c-137">初期化する必要があるドライブ (`sdc`) がわかったら、`fdisk` を使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-137">Once you know the drive (`sdc`) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="3a78c-138">`sudo` コマンドを実行し、パーティションを作成するディスクを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-138">You will need to run the command with `sudo` and supply the disk you want to partition.</span></span> <span data-ttu-id="3a78c-139">次のコマンドを使用して、新しいプライマリ パーティションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-139">We can use the following command to create a new primary partition:</span></span>

    ```bash
    (echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
    ```

1. <span data-ttu-id="3a78c-140">次に、`mkfs` コマンドを使ってパーティションにファイル システムを書き込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-140">Next, we need to write a file system to the partition with the `mkfs` command.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```

1. <span data-ttu-id="3a78c-141">最後に、ファイル システムにドライブをマウントする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-141">Finally, we need to mount the drive to the file system.</span></span> <span data-ttu-id="3a78c-142">`data` フォルダーを使用するものとします。</span><span class="sxs-lookup"><span data-stu-id="3a78c-142">Let's assume we will have a `data` folder.</span></span> <span data-ttu-id="3a78c-143">マウント ポイント フォルダーを作成して、ドライブをマウントします。</span><span class="sxs-lookup"><span data-stu-id="3a78c-143">Let's create the mount point folder and mount the drive.</span></span>

    ```bash
    sudo mkdir /data & sudo mount /dev/sdc1 /data
    ```

    > [!TIP]
    > <span data-ttu-id="3a78c-144">ディスクを初期化し、マウントしました。</span><span class="sxs-lookup"><span data-stu-id="3a78c-144">We initialized the disk and mounted it.</span></span> <span data-ttu-id="3a78c-145">このプロセスの詳細に関心がある場合は、「**Add and size disks in Azure virtual machines**」(Azure 仮想マシンにディスクを追加し、サイズを変更する) モジュールを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3a78c-145">If you are interested in more details on this process go through the **Add and size disks in Azure virtual machines** module.</span></span> <span data-ttu-id="3a78c-146">そこで、このタスクについて詳しく説明しています。</span><span class="sxs-lookup"><span data-stu-id="3a78c-146">This task is covered in more detail there.</span></span>

## <a name="install-software-onto-the-vm"></a><span data-ttu-id="3a78c-147">VM 上にソフトウェアをインストールする</span><span class="sxs-lookup"><span data-stu-id="3a78c-147">Install software onto the VM</span></span>

<span data-ttu-id="3a78c-148">ご覧のとおり、SSH を使うことでローカルコンピューターと同様に Linux VM を操作できます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-148">As you can see, SSH allows you to work with the Linux VM just like a local computer.</span></span> <span data-ttu-id="3a78c-149">他の Linux コンピューターと同じようにこの VM を管理することができ、ソフトウェアのインストール、ロールの構成、機能の調整、他の日常的なタスクを実行できます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-149">You can administer this VM as you would any other Linux computer: installing software, configuring roles, adjusting features, and other everyday tasks.</span></span> <span data-ttu-id="3a78c-150">少しの間、ソフトウェアのインストールに集中しましょう。</span><span class="sxs-lookup"><span data-stu-id="3a78c-150">Let's focus on installing software for a moment.</span></span>

<span data-ttu-id="3a78c-151">VM にソフトウェアをインストールするにはいくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-151">You have several options to install software onto the VM.</span></span> <span data-ttu-id="3a78c-152">最初に、前に説明したように、`scp` を使用してお使いのコンピューターから VM にローカル ファイルをコピーできます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-152">First, as mentioned, you can use `scp` to copy local files from your machine to the VM.</span></span> <span data-ttu-id="3a78c-153">この方法では、データまたは実行するカスタム アプリケーションをコピーできます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-153">This lets you copy over data or custom applications you want to run.</span></span>

<span data-ttu-id="3a78c-154">Secure Shell を使用してソフトウェアをインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-154">You can also install software through Secure Shell.</span></span> <span data-ttu-id="3a78c-155">Azure マシンは既定でインターネットに接続されています。</span><span class="sxs-lookup"><span data-stu-id="3a78c-155">Azure machines are, by default, internet connected.</span></span> <span data-ttu-id="3a78c-156">標準のコマンドを使用して、標準的なリポジトリから人気のあるソフトウェア パッケージを直接インストールできます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-156">You can use standard commands to install popular software packages directly from standard repositories.</span></span> <span data-ttu-id="3a78c-157">この方法を使用して Apache をインストールしましょう。</span><span class="sxs-lookup"><span data-stu-id="3a78c-157">Let's use this approach to install Apache.</span></span>

### <a name="install-the-apache-web-server"></a><span data-ttu-id="3a78c-158">Apache Web サーバーをインストールする</span><span class="sxs-lookup"><span data-stu-id="3a78c-158">Install the Apache web server</span></span>

<span data-ttu-id="3a78c-159">Apache は Ubuntu の既定のソフトウェア リポジトリ内で使用できるので、従来のパッケージ管理ツールを使用してインストールします。</span><span class="sxs-lookup"><span data-stu-id="3a78c-159">Apache is available within Ubuntu's default software repositories, so we will install it using conventional package management tools:</span></span>

1. <span data-ttu-id="3a78c-160">最初に、ローカル パッケージ インデックスを更新し、最新のアップストリームの変更を反映させます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-160">Start by updating the local package index to reflect the latest upstream changes:</span></span>

    ```bash
    sudo apt-get update
    ```
    
1. <span data-ttu-id="3a78c-161">次に、Apache をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3a78c-161">Next, install Apache:</span></span>

    ```bash
    sudo apt-get install apache2 -y
    ```

1. <span data-ttu-id="3a78c-162">これは自動で開始されるはずです。状態は `systemctl` を使用して確認できます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-162">It should start automatically - we can check the status using `systemctl`:</span></span>

    ```bash
    sudo systemctl status apache2 --no-pager
    ```

    <span data-ttu-id="3a78c-163">次のような結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-163">This should return something like:</span></span>

    ```output
    apache2.service - The Apache HTTP Server
       Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
      Drop-In: /lib/systemd/system/apache2.service.d
               └─apache2-systemd.conf
       Active: active (running) since Mon 2018-09-03 21:00:03 UTC; 1min 34s ago
     Main PID: 11156 (apache2)
        Tasks: 55 (limit: 4915)
       CGroup: /system.slice/apache2.service
               ├─11156 /usr/sbin/apache2 -k start
               ├─11158 /usr/sbin/apache2 -k start
               └─11159 /usr/sbin/apache2 -k start

    test-web-eus-vm1 systemd[1]: Starting The Apache HTTP Server...
    test-web-eus-vm1 apachectl[11129]: AH00558: apache2: Could not reliably determine the server's fully qua
    test-web-eus-vm1 systemd[1]: Started The Apache HTTP Server.
    ```
    > [!NOTE]
    > <span data-ttu-id="3a78c-164">このようなコマンドを実行することはささいなことですが、それは手動プロセスです。常にインストールする必要があるソフトウェアがある場合は、スクリプトを使用してプロセスの自動化を検討します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-164">It's trivial to execute commands like this, however it's a manual process - if we always need to install some software, you might consider automating the process using scripting.</span></span>
    
1. <span data-ttu-id="3a78c-165">最後に、パブリック IP アドレスを使用して既定ページの取得を試行できます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-165">Finally, we can try retrieving the default page through the public IP address.</span></span> <span data-ttu-id="3a78c-166">ただし、Web サーバーを VM で実行している場合でも、有効な接続または応答は得られません。</span><span class="sxs-lookup"><span data-stu-id="3a78c-166">However, even though the web server is running on the VM, you won't get a valid connection or response.</span></span> <span data-ttu-id="3a78c-167">それはなぜでしょうか。</span><span class="sxs-lookup"><span data-stu-id="3a78c-167">Do you know why?</span></span>

<span data-ttu-id="3a78c-168">Web サーバーとやりとりするには、もう 1 つの手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3a78c-168">We need to perform one more step to be able to interact with the web server.</span></span> <span data-ttu-id="3a78c-169">今回の仮想ネットワークは受信要求をブロックしています。これは既定の動作です。</span><span class="sxs-lookup"><span data-stu-id="3a78c-169">Our virtual network is blocking the inbound request - this is the default behavior.</span></span> <span data-ttu-id="3a78c-170">これを構成で変更できます。</span><span class="sxs-lookup"><span data-stu-id="3a78c-170">We can change that through configuration.</span></span> <span data-ttu-id="3a78c-171">これについては次で確認します。</span><span class="sxs-lookup"><span data-stu-id="3a78c-171">Let's look at that next.</span></span>