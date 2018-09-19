<span data-ttu-id="9ca0d-101">Linux VM をデプロイして実行させましたが、作業を行うようには構成していません。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-101">We have our Linux VM deployed and running, but it's not configured to do any work.</span></span> <span data-ttu-id="9ca0d-102">SSH で VM に接続して Apache を構成し、Web サーバーを実行させましょう。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-102">Let's connect to it with SSH and configure Apache, so we have a running web server.</span></span>

## <a name="connect-to-the-vm-with-ssh"></a><span data-ttu-id="9ca0d-103">SSH を使って VM に接続する</span><span class="sxs-lookup"><span data-stu-id="9ca0d-103">Connect to the VM with SSH</span></span>

<span data-ttu-id="9ca0d-104">SSH クライアントを使って Azure VM に接続するには、次が必要になります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-104">To connect to an Azure VM with an SSH client, you will need:</span></span>

- <span data-ttu-id="9ca0d-105">SSH クライアント ソフトウェア (ほとんどの最新オペレーティング システムに存在します)</span><span class="sxs-lookup"><span data-stu-id="9ca0d-105">SSH client software (present on most modern operating systems)</span></span>
- <span data-ttu-id="9ca0d-106">VM のパブリック IP アドレス (または、ユーザーのネットワークに接続するように VM を構成する場合はプライベート)。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-106">The public IP address of the VM (or private if the VM is configured to connect to your network)</span></span>

### <a name="get-the-public-ip-address"></a><span data-ttu-id="9ca0d-107">パブリック IP アドレスの取得</span><span class="sxs-lookup"><span data-stu-id="9ca0d-107">Get the public IP address</span></span>

1. <span data-ttu-id="9ca0d-108">[Azure portal](https://portal.azure.com?azure-portal=true) 内で、以前に作成した仮想マシンの**概要**パネルが開いていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-108">In the [Azure portal](https://portal.azure.com?azure-portal=true), ensure the **Overview** panel for the virtual machine that you created earlier is open.</span></span> <span data-ttu-id="9ca0d-109">もしパネルを開く必要がある場合は、**すべてのリソース**下で VM を見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-109">You can find the VM under **All Resources** if you need to open it.</span></span> <span data-ttu-id="9ca0d-110">概要パネルには、VM に関する多数の情報があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-110">The overview panel has a lot of information about the VM.</span></span>

    - <span data-ttu-id="9ca0d-111">VM が実行中か否かを確認可能</span><span class="sxs-lookup"><span data-stu-id="9ca0d-111">You can see whether the VM is running</span></span>
    - <span data-ttu-id="9ca0d-112">VM の停止または再起動</span><span class="sxs-lookup"><span data-stu-id="9ca0d-112">Stop or restart it</span></span>
    - <span data-ttu-id="9ca0d-113">VM へ接続するためのパブリック IP アドレスを取得</span><span class="sxs-lookup"><span data-stu-id="9ca0d-113">Get the public IP address to connect to the VM</span></span>
    - <span data-ttu-id="9ca0d-114">CPU、ディスク、およびネットワークのアクティビティを確認</span><span class="sxs-lookup"><span data-stu-id="9ca0d-114">See the activity of the CPU, disk, and network</span></span>

1. <span data-ttu-id="9ca0d-115">ウィンドウ最上部の**接続**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-115">Click the **Connect** button at the top of the pane.</span></span>

1. <span data-ttu-id="9ca0d-116">**仮想マシンへの接続**ブレード内で、**IP アドレス**および**ポート番号**設定をメモします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-116">In the **Connect to virtual machine** blade, note the **IP address** and **Port number** settings.</span></span> <span data-ttu-id="9ca0d-117">**SSH**タブ上で、VM に接続するためにローカルで実行する必要があるコマンドも表示されています。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-117">On the **SSH** tab, you will also find the command you need to execute locally to connect to the VM.</span></span> <span data-ttu-id="9ca0d-118">これをクリップボードにコピーします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-118">Copy this to the clipboard.</span></span>

## <a name="connect-with-ssh"></a><span data-ttu-id="9ca0d-119">SSH を使用して接続する</span><span class="sxs-lookup"><span data-stu-id="9ca0d-119">Connect with SSH</span></span>

1. <span data-ttu-id="9ca0d-120">[SSH] タブから取得したコマンド ラインを Azure Cloud Shell に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-120">Paste the command line you got from the SSH tab into Azure Cloud Shell.</span></span> <span data-ttu-id="9ca0d-121">次のようになります。ただし、IP アドレスは異なります (また、**jim** を使用しなかった場合はユーザー名もおそらく異なります)。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-121">It should look something like this; however, it will have a different IP address (and perhaps a different username if you didn't use **jim**!):</span></span>

    ```bash
    ssh jim@137.117.101.249
    ```

1. <span data-ttu-id="9ca0d-122">このコマンドを使用すると、Secure Shell 接続が開き、Linux 用の従来のシェル コマンド プロンプトに移動します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-122">This command will open a Secure Shell connection and place you at a traditional shell command prompt for Linux.</span></span>

1. <span data-ttu-id="9ca0d-123">ここで Linux コマンドをいくつか実行してみてください。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-123">Try executing a few Linux commands</span></span>
    - <span data-ttu-id="9ca0d-124">`ls -la /` を使ってディスクのルートを表示します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-124">`ls -la /` to show the root of the disk</span></span>
    - <span data-ttu-id="9ca0d-125">`ps -l` を使って実行中のプロセスをすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-125">`ps -l` to show all the running processes</span></span>
    - <span data-ttu-id="9ca0d-126">`dmesg` を使ってカーネル メッセージ一覧をすべて表示します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-126">`dmesg` to list all the kernel messages</span></span>
    - <span data-ttu-id="9ca0d-127">`lsblk` を使ってブロック デバイス一覧を全て表示します。ここにお使いのドライブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-127">`lsblk` to list all the block devices - here you will see your drives</span></span>

<span data-ttu-id="9ca0d-128">ドライブの一覧でさらに興味深いことは、"_表示されていない_" デバイスです。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-128">The more interesting thing to observe in the list of drives is what is _missing_.</span></span> <span data-ttu-id="9ca0d-129">**データ** ドライブ（`sdc`）が存在しているが、ファイル システムにはマウントされていないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-129">Notice that our **Data** drive (`sdc`) is present but not mounted into the file system.</span></span> <span data-ttu-id="9ca0d-130">Azure で VHD が追加されていますが、初期化はされていません。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-130">Azure added a VHD but didn't initialize it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="9ca0d-131">データ ディスクの初期化</span><span class="sxs-lookup"><span data-stu-id="9ca0d-131">Initialize data disks</span></span>

<span data-ttu-id="9ca0d-132">一から作成した追加ドライブは、初期化してフォーマットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-132">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="9ca0d-133">これを行うためのプロセスは、物理ディスクと同じです。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-133">The process for doing this is identical to a physical disk:</span></span>

1. <span data-ttu-id="9ca0d-134">最初に、ディスクを特定します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-134">First, identify the disk.</span></span> <span data-ttu-id="9ca0d-135">これは上で行いました。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-135">We did that above.</span></span> <span data-ttu-id="9ca0d-136">`dmesg | grep SCSI` を使用して、SCSI デバイス用のカーネルからすべてのメッセージを一覧表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-136">You could also use `dmesg | grep SCSI`, which will list all the messages from the kernel for SCSI devices.</span></span>

1. <span data-ttu-id="9ca0d-137">初期化する必要があるドライブ (`sdc`) がわかったら、`fdisk` を使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-137">Once you know the drive (`sdc`) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="9ca0d-138">`sudo` でコマンドを実行し、パーティションを作成するディスクを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-138">You will need to run the command with `sudo` and supply the disk you want to partition:</span></span>

    ```bash
    sudo fdisk /dev/sdc
    ```
1. <span data-ttu-id="9ca0d-139">`n` コマンドを使用して新しいパーティションを追加します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-139">Use the `n` command to add a new partition.</span></span> <span data-ttu-id="9ca0d-140">この例では、プライマリ パーティション用の **p** も選択し、残りの既定値はそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-140">In this example, we also choose **p** for a primary partition and accept the rest of the default values.</span></span> <span data-ttu-id="9ca0d-141">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-141">The output will be similar to the following example:</span></span>   

    ```output
    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x1f2d0c46.

    Command (m for help): n
    Partition type
       p   primary (0 primary, 0 extended, 4 free)
       e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-2145386495, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} (2048-2145386495, default 2145386495):

    Created a new partition 1 of type 'Linux' and of size 1023 GiB.
    ```

1. <span data-ttu-id="9ca0d-142">`p` コマンドを使ってパーティション テーブルを出力します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-142">Print the partition table with the `p` command.</span></span> <span data-ttu-id="9ca0d-143">これは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-143">It should look something like this:</span></span>

    ```output
    Disk /dev/sdc: 1023 GiB, 1098437885952 bytes, 2145386496 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x1f2d0c46

    Device     Boot Start        End    Sectors  Size Id Type
    /dev/sdc1        2048 2145386495 2145384448 1023G 83 Linux
    ```

1. <span data-ttu-id="9ca0d-144">`w` コマンドを使って変更を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-144">Write the changes with the `w` command.</span></span> <span data-ttu-id="9ca0d-145">これによってツールが終了します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-145">This will exit the tool.</span></span>

1. <span data-ttu-id="9ca0d-146">次に、`mkfs` コマンドを使ってパーティションにファイル システムを書き込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-146">Next, we need to write a file system to the partition with the `mkfs` command.</span></span> <span data-ttu-id="9ca0d-147">ファイル システムの種類と、`fdisk` の出力から取得したデバイス名を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-147">We will need to specify the file system type and device name that we got from the `fdisk` output:</span></span>
    - <span data-ttu-id="9ca0d-148">`-t ext4` を渡して _ext4_ ファイル システムを作成します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-148">Pass `-t ext4` to create an _ext4_ filesystem.</span></span>
    - <span data-ttu-id="9ca0d-149">デバイス名は `/dev/sdc` です。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-149">The device name is `/dev/sdc`.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    <span data-ttu-id="9ca0d-150">このコマンドの完了には数分を要します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-150">This command will take a few minutes to complete.</span></span>

    ```output
    mke2fs 1.44.1 (24-Mar-2018)
    Discarding device blocks: done
    Creating filesystem with 268173056 4k blocks and 67043328 inodes
    Filesystem UUID: e311c905-e0d9-43ab-af63-7f4ee4ef108e
    Superblock backups stored on blocks:
            32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
            4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
            102400000, 214990848

    Allocating group tables: done
    Writing inode tables: done
    Creating journal (262144 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```

1. <span data-ttu-id="9ca0d-151">次に、マウント ポイントとして使用するディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-151">Next, create a directory we will use as our mount point.</span></span> <span data-ttu-id="9ca0d-152">`data` フォルダーを使用するものとします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-152">Let's assume we will have a `data` folder:</span></span>

    ```bash
    sudo mkdir /data
    ```
1. <span data-ttu-id="9ca0d-153">最後に、`mount` を使用してディスクをマウント ポイントにアタッチします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-153">Finally, use `mount` to attach the disk to the mount point:</span></span>

    ```bash
    sudo mount /dev/sdc1 /data
    ```
    <span data-ttu-id="9ca0d-154">`lsblk` を使用し、マウントされたドライブを表示できるはずです。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-154">You should be able to use `lsblk` to see the mounted drive now:</span></span>
    
    ```output
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda       8:0    0   30G  0 disk
    ├─sda1    8:1    0 29.9G  0 part /
    ├─sda14   8:14   0    4M  0 part
    └─sda15   8:15   0  106M  0 part /boot/efi
    sdb       8:16   0   16G  0 disk
    └─sdb1    8:17   0   16G  0 part /mnt
    sdc       8:32   0 1023G  0 disk
    └─sdc1    8:33   0 1023G  0 part /data
    sr0      11:0    1  628K  0 rom
    ```

### <a name="mounting-the-drive-automatically"></a><span data-ttu-id="9ca0d-155">自動的にドライブをマウントする</span><span class="sxs-lookup"><span data-stu-id="9ca0d-155">Mounting the drive automatically</span></span>

<span data-ttu-id="9ca0d-156">再起動後にドライブが確実に自動でマウントされるようにするには、そのドライブを `/etc/fstab` ファイルに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-156">To ensure that the drive is mounted automatically after a reboot, it must be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="9ca0d-157">ドライブを参照するときは、デバイス名 (`/dev/sdc1` など) だけでなく、UUID (汎用一意識別子) を `/etc/fstab` で使用することも強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-157">It is also highly recommended that the UUID (universally unique identifier) is used in `/etc/fstab` to refer to the drive rather than just the device name (such as `/dev/sdc1`).</span></span> <span data-ttu-id="9ca0d-158">UUID を使用すると、OS が起動中にディスク エラーを検出した場合に、間違ったディスクが特定の場所にマウントされるのを防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-158">If the OS detects a disk error during boot, using the UUID avoids the incorrect disk being mounted to a given location.</span></span> <span data-ttu-id="9ca0d-159">その後、残りのデータ ディスクは、その同じデバイス ID に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-159">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="9ca0d-160">新しいドライブの UUID を確認するには、`blkid` ユーティリティを使用します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-160">To find the UUID of the new drive, use the `blkid` utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="9ca0d-161">次のような結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-161">It will return something like:</span></span>

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. <span data-ttu-id="9ca0d-162">`/dev/sdc1` ドライブの UUID をコピーし、`/etc/fstab` ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-162">Copy the UUID for the `/dev/sdc1` drive and open the `/etc/fstab` file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> <span data-ttu-id="9ca0d-163">`/etc/fstab` ファイルを不適切に編集すると、システムが起動できなくなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-163">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="9ca0d-164">編集方法がはっきりわからない場合は、このファイルを適切に編集する方法について、ディストリビューションのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-164">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="9ca0d-165">また、運用システムで作業を行っている際は、ファイルを編集する前にバックアップの作成を推奨します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-165">It is also recommended that a backup of the file is created before editing when you are working with production systems.</span></span>

1. <span data-ttu-id="9ca0d-166">**G** を押してファイルの最後の行に移動します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-166">Press **G** to move to the last line in the file.</span></span>

1. <span data-ttu-id="9ca0d-167">**I** を押して挿入モードにします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-167">Press **I** to enter INSERT mode.</span></span> <span data-ttu-id="9ca0d-168">画面最下部に、モードが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-168">It should indicate the mode at the bottom of the screen.</span></span>

1. <span data-ttu-id="9ca0d-169">**End** キーを押して、行の末尾に移動します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-169">Press the **END** key to move to the end of the line.</span></span> <span data-ttu-id="9ca0d-170">または、矢印キーを使うこともできます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-170">Alternatively, you can use the arrow keys.</span></span> <span data-ttu-id="9ca0d-171">**ENTER** を押して新規行に移動します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-171">Press **ENTER** to move to a new line.</span></span>

1. <span data-ttu-id="9ca0d-172">エディタに以下のラインを入力してします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-172">Type the following line into the editor.</span></span> <span data-ttu-id="9ca0d-173">複数の値は、スペースまたはタブで区切ることができます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-173">The values can be space or tab separated.</span></span> <span data-ttu-id="9ca0d-174">各列の詳細については、ドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-174">Check the documentation for more information on each of the columns:</span></span>

    ```output
    UUID=<uuid-goes-here>    /data    ext4    defaults,nofail    1    2
    ```
1. <span data-ttu-id="9ca0d-175">**Esc** キーを押し、ファイルを書き込むには「**:w!**」と入力し、</span><span class="sxs-lookup"><span data-stu-id="9ca0d-175">Press **ESC**, then type **:w!**</span></span> <span data-ttu-id="9ca0d-176">エディターを終了するには「**:q**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-176">to write the file and **:q** to quit the editor.</span></span>

1. <span data-ttu-id="9ca0d-177">最後に、マウント ポイントの更新を OS に要求することによって、入力が正しいことを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-177">Finally, let's check to make sure the entry is correct by asking the OS to refresh the mount points:</span></span>

    ```bash
    sudo mount -a
    ```

    <span data-ttu-id="9ca0d-178">もしエラーが返された場合、ファイルを編集して問題を探します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-178">If it returns an error, edit the file to find the problem.</span></span>

> [!TIP]
> <span data-ttu-id="9ca0d-179">一部の Linux カーネルでは、ディスク上の未使用ブロックを破棄する TRIM がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-179">Some Linux kernels support TRIM to discard unused blocks on disks.</span></span> <span data-ttu-id="9ca0d-180">この機能は Azure ディスクで利用可能で、もし大きなファイルを作成してそれを削除する場合に使用すると、コストを節約できます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-180">This feature is available on Azure disks and can save you money if you create large files and then delete them.</span></span> <span data-ttu-id="9ca0d-181">[この機能を有効にする](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure)方法は、ドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-181">Learn how to [turn this feature on](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in our documentation.</span></span>

## <a name="install-software-onto-the-vm"></a><span data-ttu-id="9ca0d-182">VM 上にソフトウェアをインストールする</span><span class="sxs-lookup"><span data-stu-id="9ca0d-182">Install software onto the VM</span></span>

<span data-ttu-id="9ca0d-183">ご覧のとおり、SSH を使うことでローカルコンピューターと同様に Linux VM を操作できます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-183">As you can see, SSH allows you to work with the Linux VM just like a local computer.</span></span> <span data-ttu-id="9ca0d-184">他の Linux コンピューターと同じようにこの VM を管理することができ、ソフトウェアのインストール、ロールの構成、機能の調整、他の日常的なタスクを実行できます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-184">You can administer this VM as you would any other Linux computer: installing software, configuring roles, adjusting features, and other everyday tasks.</span></span> <span data-ttu-id="9ca0d-185">少しの間、ソフトウェアのインストールに集中しましょう。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-185">Let's focus on installing software for a moment.</span></span>

<span data-ttu-id="9ca0d-186">VM にソフトウェアをインストールするにはいくつかのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-186">You have several options to install software onto the VM.</span></span> <span data-ttu-id="9ca0d-187">最初に、前に説明したように、`scp` を使用してお使いのコンピューターから VM にローカル ファイルをコピーできます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-187">First, as mentioned, you can use `scp` to copy local files from your machine to the VM.</span></span> <span data-ttu-id="9ca0d-188">この方法では、データまたは実行するカスタム アプリケーションをコピーできます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-188">This lets you copy over data or custom applications you want to run.</span></span>

<span data-ttu-id="9ca0d-189">Secure Shell を使用してソフトウェアをインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-189">You can also install software through Secure Shell.</span></span> <span data-ttu-id="9ca0d-190">Azure マシンは既定でインターネットに接続されています。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-190">Azure machines are, by default, internet connected.</span></span> <span data-ttu-id="9ca0d-191">標準のコマンドを使用して、標準的なリポジトリから人気のあるソフトウェア パッケージを直接インストールできます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-191">You can use standard commands to install popular software packages directly from standard repositories.</span></span> <span data-ttu-id="9ca0d-192">この方法を使用して Apache をインストールしましょう。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-192">Let's use this approach to install Apache.</span></span>

### <a name="install-the-apache-web-server"></a><span data-ttu-id="9ca0d-193">Apache Web サーバーをインストールする</span><span class="sxs-lookup"><span data-stu-id="9ca0d-193">Install the Apache web server</span></span>

<span data-ttu-id="9ca0d-194">Apache は Ubuntu の既定のソフトウェア リポジトリ内で使用できるので、従来のパッケージ管理ツールを使用してインストールします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-194">Apache is available within Ubuntu's default software repositories, so we will install it using conventional package management tools:</span></span>

1. <span data-ttu-id="9ca0d-195">最初に、ローカル パッケージ インデックスを更新し、最新のアップストリームの変更を反映させます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-195">Start by updating the local package index to reflect the latest upstream changes:</span></span>

    ```bash
    sudo apt-get update
    ```
    
1. <span data-ttu-id="9ca0d-196">次に、Apache をインストールします。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-196">Next, install Apache:</span></span>

    ```bash
    sudo apt-get install apache2 -y
    ```

1. <span data-ttu-id="9ca0d-197">これは自動で開始されるはずです。状態は `systemctl` を使用して確認できます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-197">It should start automatically - we can check the status using `systemctl`:</span></span>

    ```bash
    sudo systemctl status apache2
    ```

    <span data-ttu-id="9ca0d-198">次のような結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-198">This should return something like:</span></span>

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
    > <span data-ttu-id="9ca0d-199">このようなコマンドを実行することはささいなことですが、それは手動プロセスです。常にインストールする必要があるソフトウェアがある場合は、スクリプトを使用してプロセスの自動化を検討します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-199">It's trivial to execute commands like this, however it's a manual process - if we always need to install some software, you might consider automating the process using scripting.</span></span>
    
1. <span data-ttu-id="9ca0d-200">最後に、パブリック IP アドレスを使用して既定ページの取得を試行できます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-200">Finally, we can try retrieving the default page through the public IP address.</span></span> <span data-ttu-id="9ca0d-201">ただし、Web サーバーを VM で実行している場合でも、有効な接続または応答は得られません。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-201">However, even though the web server is running on the VM, you won't get a valid connection or response.</span></span> <span data-ttu-id="9ca0d-202">それはなぜでしょうか。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-202">Do you know why?</span></span>

<span data-ttu-id="9ca0d-203">Web サーバーとやりとりするには、もう 1 つの手順を実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-203">We need to perform one more step to be able to interact with the web server.</span></span> <span data-ttu-id="9ca0d-204">今回の仮想ネットワークは受信要求をブロックしています。これは既定の動作です。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-204">Our virtual network is blocking the inbound request - this is the default behavior.</span></span> <span data-ttu-id="9ca0d-205">これを構成で変更できます。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-205">We can change that through configuration.</span></span> <span data-ttu-id="9ca0d-206">これについては次で確認します。</span><span class="sxs-lookup"><span data-stu-id="9ca0d-206">Let's look at that next.</span></span>