<span data-ttu-id="247bf-101">所属している法律事務所は取り扱い件数を増やしており、顧客、他の法律事務所、法執行機関などのさまざまなソースの重要なドキュメントを保管するために、新しい Linux Web サーバーを設置する職務を任されました。</span><span class="sxs-lookup"><span data-stu-id="247bf-101">Our law firm is expanding it's case load and you have been tasked with putting up a new Linux web server to store critical documents from a variety of sources - clients, other law firms, and law enforcement offices.</span></span> <span data-ttu-id="247bf-102">サーバーはアップロードを受け入れることができます。また、それをディスクに保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-102">Our server can accept uploads and needs to store them on disk.</span></span>

> [!TIP]
> <span data-ttu-id="247bf-103">この演習では Linux を例に説明しますが、VM の作成とディスクの追加の基本的なプロセスは Windows の場合と同じです。</span><span class="sxs-lookup"><span data-stu-id="247bf-103">This exercise uses Linux as the example, but the basic process of creating VMs and adding disks is the same for Windows.</span></span> <span data-ttu-id="247bf-104">主な違いは、ディスクのパーティションとフォーマットです。このような処理は、組み込みのディスク管理ツールを使用してリモート デスクトップ セッションで行われます。</span><span class="sxs-lookup"><span data-stu-id="247bf-104">The primary difference would be in partitioning and formatting the disk - that would be done in a Remote Desktop session using the built-in Disk Management tools.</span></span>

<span data-ttu-id="247bf-105">この演習の目標は、Linux 仮想マシン (VM) を作成し、"uploads" ディレクトリを保存する "uploadDataDisk1" という新しい仮想ハード ディスク (VHD) を接続することです。</span><span class="sxs-lookup"><span data-stu-id="247bf-105">The goal of the exercise is to create a Linux virtual machine (VM) and attach a new virtual hard disk (VHD) named "uploadDataDisk1" to store the "uploads" directory.</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm"></a><span data-ttu-id="247bf-106">Linux VM を作成する</span><span class="sxs-lookup"><span data-stu-id="247bf-106">Create a Linux VM</span></span>

<span data-ttu-id="247bf-107">Azure CLI を使用して Web サーバーをホストする Linux VM を作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="247bf-107">Let's create a Linux VM to host our web server using the Azure CLI.</span></span>

1. <span data-ttu-id="247bf-108">まず、このセッションの既定値をいくつか設定します。</span><span class="sxs-lookup"><span data-stu-id="247bf-108">Start by setting some defaults for this session.</span></span> <span data-ttu-id="247bf-109">最初に、VM を配置する_場所_を決める必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-109">The first thing you need to decide is the _location_ to place the VM.</span></span> <span data-ttu-id="247bf-110">これはクライアントに近い場所であることが理想的です。</span><span class="sxs-lookup"><span data-stu-id="247bf-110">Ideally this would be close to your clients.</span></span> <span data-ttu-id="247bf-111">この場合は、Azure サンドボックスで使用できる場所から最も近いリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="247bf-111">In this case, select the closest region from the locations available to the Azure sandbox.</span></span>

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. <span data-ttu-id="247bf-112">`az configure` コマンドを使って、使用する既定の場所を設定します。</span><span class="sxs-lookup"><span data-stu-id="247bf-112">Use the `az configure` command to set the default location you want to use.</span></span> <span data-ttu-id="247bf-113">"eastus" をその場所に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="247bf-113">Replace "eastus" with the location.</span></span>

    ```azurecli
    az configure --defaults location=eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="247bf-114">既定のリソース グループ値を、Azure サンドボックス用に作成された事前構成済みリソース グループ (**<rgn>[サンドボックス リソース グループ]</rgn>**) に設定します。</span><span class="sxs-lookup"><span data-stu-id="247bf-114">Set the default resource group value to the pre-configured resource group created for the Azure sandbox: **<rgn>[sandbox resource group]</rgn>**</span></span>

    ```azurecli
    az configure --defaults group="<rgn>[sandbox Resource Group]</rgn>"
    ```

1. <span data-ttu-id="247bf-115">次に、`vm create` コマンドを使用して新しい Ubuntu Linux VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="247bf-115">Next, use the `vm create` command to create a new Ubuntu Linux VM.</span></span>
    - <span data-ttu-id="247bf-116">"support-web-vm01" と名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="247bf-116">Name it "support-web-vm01".</span></span>
    - <span data-ttu-id="247bf-117">**size** を _Standard_DS2_v2_ に設定します。</span><span class="sxs-lookup"><span data-stu-id="247bf-117">Set the **size** to _Standard_DS2_v2_.</span></span>
    - <span data-ttu-id="247bf-118">**admin-username** を "azureuser" (または好きなもの) に設定します。</span><span class="sxs-lookup"><span data-stu-id="247bf-118">Set the **admin-username** to "azureuser" (or whatever you prefer).</span></span>
    - <span data-ttu-id="247bf-119">`--generate-ssh-keys` パラメーターを使用して SSH キーを生成します。</span><span class="sxs-lookup"><span data-stu-id="247bf-119">Generate SSH keys with the `--generate-ssh-keys` parameter.</span></span>

    ```azurecli
    az vm create --name support-web-vm01 \
        --image UbuntuLTS \
        --size Standard_DS2_v2 \
        --admin-username azureuser \
        --generate-ssh-keys
    ```
    
    > [!TIP]
    > <span data-ttu-id="247bf-120">VM を作成して Azure にデプロイするには、数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="247bf-120">Creating your VM and deploying it in Azure can take a few minutes.</span></span> <span data-ttu-id="247bf-121">進行状況は、Cloud Shell で確認できます。</span><span class="sxs-lookup"><span data-stu-id="247bf-121">You can watch the progress in the Cloud Shell.</span></span>
    
1. <span data-ttu-id="247bf-122">この結果、作成された VM の詳細を含む JSON ブロックが生成されます。</span><span class="sxs-lookup"><span data-stu-id="247bf-122">This will result in a JSON block with the details about the created VM.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/xxx/resourceGroups/<rgn>[sandbox resource group]</rgn>/providers/Microsoft.Compute/virtualMachines/support-web-vm01",
        "location": "eastus",
        "macAddress": "00-0D-3A-18-DE-B4",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "40.76.193.249",
        "resourceGroup": "<rgn>[sandbox resource group]</rgn>",
        "zones": ""
    }
    ```
    <span data-ttu-id="247bf-123">出力の **publicIpAddress** に注意してください。</span><span class="sxs-lookup"><span data-stu-id="247bf-123">Note of the **publicIpAddress** in your output.</span></span> <span data-ttu-id="247bf-124">これがリモートから VM にアクセスする方法です。</span><span class="sxs-lookup"><span data-stu-id="247bf-124">This is how we can access the VM remotely.</span></span>

    > [!NOTE]
    > <span data-ttu-id="247bf-125">ディスクの管理方法を学習するために、この VM を使用しています。</span><span class="sxs-lookup"><span data-stu-id="247bf-125">We are using this VM to learn how to manage disks.</span></span> <span data-ttu-id="247bf-126">実際は Web サーバーとして使用する予定だった場合は、`az vm open-port` コマンドで追加のポートを開き、Web サーバー ソフトウェアをインストールします。</span><span class="sxs-lookup"><span data-stu-id="247bf-126">If we were truly going to use it as a web server, we'd want to open up additional ports with the `az vm open-port` command, and install web server software.</span></span> <span data-ttu-id="247bf-127">これらのタスクはこのモジュールの対象外ですが、その手順の実行方法については、「**Azure CLI を使用して仮想マシンを管理する**」モジュールを参照してください。</span><span class="sxs-lookup"><span data-stu-id="247bf-127">These tasks are outside the scope of this module, but you can go through the **Manage virtual machines with the Azure CLI** module to learn how to do these steps.</span></span> 

## <a name="add-an-empty-data-disk-to-our-vm"></a><span data-ttu-id="247bf-128">空のデータ ディスクを VM に追加する</span><span class="sxs-lookup"><span data-stu-id="247bf-128">Add an empty data disk to our VM</span></span>

<span data-ttu-id="247bf-129">Web サーバー "uploadDataDisk1" の "uploads" ディレクトリを保存するディスクの名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="247bf-129">We're going to name the disk that stores the "uploads" directory for your web server "uploadDataDisk1".</span></span> 

> [!TIP]
> <span data-ttu-id="247bf-130">`vm create` コマンドに `--data-disk-sizes-gb`パラメーターを使用して VM を作成するときに、データ ディスクを追加できます。</span><span class="sxs-lookup"><span data-stu-id="247bf-130">You can add data disks when the VM is created using the `--data-disk-sizes-gb` parameter on the `vm create` command.</span></span>

1. <span data-ttu-id="247bf-131">`vm disk attach` コマンドを使用して新しい空のディスクをサーバーに追加します。</span><span class="sxs-lookup"><span data-stu-id="247bf-131">Add a new empty disk to the server with the `vm disk attach` command.</span></span>
    - <span data-ttu-id="247bf-132">"uploadDataDisk1" と名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="247bf-132">Name it "uploadDataDisk1"</span></span>
    - <span data-ttu-id="247bf-133">64 GB と設定します。</span><span class="sxs-lookup"><span data-stu-id="247bf-133">Set it to be 64 GB.</span></span>
    - <span data-ttu-id="247bf-134">ローカルの冗長性を備えた Premium ストレージを使用するように、**sku** を "_Premium_LRS_" に設定します。ローカルの冗長性を備えた Premium ストレージを使用するように、</span><span class="sxs-lookup"><span data-stu-id="247bf-134">Set the **sku** to "_Premium_LRS_" so we use premium storage with local redundancy.</span></span>

    ```azurecli
    az vm disk attach \
        --vm-name support-web-vm01 \
        --disk uploadDataDisk1 \
        --size-gb 64 \
        --sku Premium_LRS \
        --new
    ```
    
<span data-ttu-id="247bf-135">これで、"uploadDataDisk1" という名前のディスクが定義されます。</span><span class="sxs-lookup"><span data-stu-id="247bf-135">We've now defined a disk named "uploadDataDisk1" .</span></span> <span data-ttu-id="247bf-136">このディスクを使用するには、パーティションとフォーマットを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-136">To use the disk, we'll need to partition and format it.</span></span>

## <a name="initialize-data-disks"></a><span data-ttu-id="247bf-137">データ ディスクの初期化</span><span class="sxs-lookup"><span data-stu-id="247bf-137">Initialize data disks</span></span>

<span data-ttu-id="247bf-138">一から作成した追加ドライブは、初期化してフォーマットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-138">Any additional drives you create from scratch will need to be initialized and formatted.</span></span> <span data-ttu-id="247bf-139">これを行うためのプロセスは、物理ディスクと同じです。</span><span class="sxs-lookup"><span data-stu-id="247bf-139">The process for doing this is identical to a physical disk:</span></span>

1. <span data-ttu-id="247bf-140">まず、元の VM 作成応答から返されるパブリック IP アドレスを使用して、Linux サーバーに SSH で接続します。</span><span class="sxs-lookup"><span data-stu-id="247bf-140">First, SSH into the Linux server using the public IP address you got back from the original VM creation response.</span></span> <span data-ttu-id="247bf-141">まだない場合は、次のコマンドを使用して IP アドレスを取得できます。</span><span class="sxs-lookup"><span data-stu-id="247bf-141">If you don't have that, you can use the following command to get the IP address:</span></span>

    ```azurecli
    az vm list-ip-addresses -n support-web-vm01
    ```

1. <span data-ttu-id="247bf-142">作成したパブリック IP とユーザー名を指定して SSH を使用します。</span><span class="sxs-lookup"><span data-stu-id="247bf-142">Use SSH with the public IP and the username you created.</span></span>

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. <span data-ttu-id="247bf-143">次に、すべてのブロック デバイスを一覧表示するために `lsblk` コマンドでディスクを特定します。次のようにドライブが表示されます。</span><span class="sxs-lookup"><span data-stu-id="247bf-143">Next, identify the disk with the `lsblk` command to list all the block devices - here you will see your drives.</span></span>

    ```bash
    lsblk
    ```

    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom
    sdc      8:32   0   64G  0 disk
    sda      8:0    0   30G  0 disk
    └─sda1   8:1    0   30G  0 part /
    ```

<span data-ttu-id="247bf-144">作成した 64 GB ドライブを探します。**sdc** が表示されており、マウントされていないことがわかります。これは初期化されていないためです。</span><span class="sxs-lookup"><span data-stu-id="247bf-144">Look for the 64 GB drive we created - here you can see it's **sdc** and it's not mounted - this is because it's not been initialized.</span></span>

1. <span data-ttu-id="247bf-145">初期化する必要があるドライブ (**sdc**) がわかったら、`fdisk` を使用して行うことができます。</span><span class="sxs-lookup"><span data-stu-id="247bf-145">Once you know the drive (**sdc**) you need to initialize, you can use `fdisk` to do that.</span></span> <span data-ttu-id="247bf-146">`sudo` でコマンドを実行し、パーティションを作成するディスクを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-146">You will need to run the command with `sudo` and supply the disk you want to partition:</span></span>

    ```bash
    sudo fdisk /dev/sdc
    ```

1. <span data-ttu-id="247bf-147"><kbd>n</kbd> コマンドを使用して新しいパーティションを追加します。</span><span class="sxs-lookup"><span data-stu-id="247bf-147">Use the <kbd>n</kbd> command to add a new partition.</span></span> <span data-ttu-id="247bf-148">この例では、プライマリ パーティション用の <kbd>p</kbd> も選択し、残りの既定値はそのまま使用します。</span><span class="sxs-lookup"><span data-stu-id="247bf-148">In this example, we also choose <kbd>p</kbd> for a primary partition and accept the rest of the default values.</span></span> <span data-ttu-id="247bf-149">出力は次の例のようになります。</span><span class="sxs-lookup"><span data-stu-id="247bf-149">The output will be similar to the following example:</span></span>   

    ```output
    Device does not contain a recognized partition table.
    Created a new DOS disklabel with disk identifier 0x3b44089f.
    
    Command (m for help): n
    Partition type
       p   primary (0 primary, 0 extended, 4 free)
       e   extended (container for logical partitions)
    Select (default p): p
    Partition number (1-4, default 1): 1
    First sector (2048-268435455, default 2048):
    Last sector, +sectors or +size{K,M,G,T,P} ...
    Created a new partition 1 of type 'Linux' and of size 64 GiB.    
    ```

1. <span data-ttu-id="247bf-150"><kbd>p</kbd> コマンドを使ってパーティション テーブルを出力します。</span><span class="sxs-lookup"><span data-stu-id="247bf-150">Print the partition table with the <kbd>p</kbd> command.</span></span> <span data-ttu-id="247bf-151">これは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="247bf-151">It should look something like this:</span></span>

    ```output
    Disk /dev/sdc: 64 GiB ...
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 4096 bytes
    I/O size (minimum/optimal): 4096 bytes / 4096 bytes
    Disklabel type: dos
    Disk identifier: 0x3b44089f
    
    Device     Boot Start ... Size Id Type
    /dev/sdc1        2048 ... 64G 83 Linux
    ```

1. <span data-ttu-id="247bf-152"><kbd>w<kbd> コマンドを使って変更を書き込みます。</span><span class="sxs-lookup"><span data-stu-id="247bf-152">Write the changes with the <kbd>w<kbd> command.</span></span> <span data-ttu-id="247bf-153">これによってツールが終了します。</span><span class="sxs-lookup"><span data-stu-id="247bf-153">This will exit the tool.</span></span>

1. <span data-ttu-id="247bf-154">次に、`mkfs` コマンドを使ってパーティションにファイル システムを書き込む必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-154">Next, we need to write a file system to the partition with the `mkfs` command.</span></span> <span data-ttu-id="247bf-155">ファイル システムの種類と、`fdisk` の出力から取得したデバイス名を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-155">We will need to specify the file system type and device name that we got from the `fdisk` output:</span></span>
    - <span data-ttu-id="247bf-156">`-t ext4` を渡して _ext4_ ファイル システムを作成します。</span><span class="sxs-lookup"><span data-stu-id="247bf-156">Pass `-t ext4` to create an _ext4_ filesystem.</span></span>
    - <span data-ttu-id="247bf-157">デバイス名は `/dev/sdc` です。</span><span class="sxs-lookup"><span data-stu-id="247bf-157">The device name is `/dev/sdc`.</span></span>

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    <span data-ttu-id="247bf-158">このコマンドの完了には数分を要します。</span><span class="sxs-lookup"><span data-stu-id="247bf-158">This command will take a minute or so to complete.</span></span>

    ```output
    mke2fs 1.42.13 (17-May-2015)
    Discarding device blocks: done
    Creating filesystem with 16777088 4k blocks and 4194304 inodes
    ...
    Allocating group tables: done
    Writing inode tables: done
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done
    ```
    
1. <span data-ttu-id="247bf-159">次に、マウント ポイントとして使用するディレクトリを作成します。</span><span class="sxs-lookup"><span data-stu-id="247bf-159">Next, create a directory we will use as our mount point.</span></span> <span data-ttu-id="247bf-160">`uploads` フォルダーを使用するものとします。</span><span class="sxs-lookup"><span data-stu-id="247bf-160">Let's assume we will have a `uploads` folder:</span></span>

    ```bash
    sudo mkdir /uploads
    ```
1. <span data-ttu-id="247bf-161">最後に、`mount` を使用してディスクをマウント ポイントにアタッチします。</span><span class="sxs-lookup"><span data-stu-id="247bf-161">Finally, use `mount` to attach the disk to the mount point:</span></span>

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```
    <span data-ttu-id="247bf-162">`lsblk` を使用し、マウントされたドライブを表示できるはずです。</span><span class="sxs-lookup"><span data-stu-id="247bf-162">You should be able to use `lsblk` to see the mounted drive now:</span></span>
    
    ```output
    NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sdb      8:16   0   14G  0 disk
    └─sdb1   8:17   0   14G  0 part /mnt
    sr0     11:0    1  628K  0 rom
    sdc      8:32   0   64G  0 disk
    └─sdc1   8:33   0   64G  0 part /uploads
    sda      8:0    0   30G  0 disk
    └─sda1   8:1    0   30G  0 part /
    ```
    
### <a name="mounting-the-drive-automatically"></a><span data-ttu-id="247bf-163">自動的にドライブをマウントする</span><span class="sxs-lookup"><span data-stu-id="247bf-163">Mounting the drive automatically</span></span>

<span data-ttu-id="247bf-164">再起動後にドライブが確実に自動でマウントされるようにするには、そのドライブを `/etc/fstab` ファイルに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-164">To ensure that the drive is mounted automatically after a reboot, it must be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="247bf-165">ドライブを参照するときは、デバイス名 (`/dev/sdc1` など) だけでなく、UUID (汎用一意識別子) を `/etc/fstab` で使用することも強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="247bf-165">It is also highly recommended that the UUID (universally unique identifier) is used in `/etc/fstab` to refer to the drive rather than just the device name (such as `/dev/sdc1`).</span></span> <span data-ttu-id="247bf-166">UUID を使用すると、OS が起動中にディスク エラーを検出した場合に、間違ったディスクが特定の場所にマウントされるのを防ぐことができます。</span><span class="sxs-lookup"><span data-stu-id="247bf-166">If the OS detects a disk error during boot, using the UUID avoids the incorrect disk being mounted to a given location.</span></span> <span data-ttu-id="247bf-167">その後、残りのデータ ディスクは、その同じデバイス ID に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="247bf-167">Remaining data disks would then be assigned those same device IDs.</span></span> <span data-ttu-id="247bf-168">新しいドライブの UUID を確認するには、`blkid` ユーティリティを使用します。</span><span class="sxs-lookup"><span data-stu-id="247bf-168">To find the UUID of the new drive, use the `blkid` utility:</span></span>

```bash
sudo -i blkid
```

<span data-ttu-id="247bf-169">次のような結果が返されます。</span><span class="sxs-lookup"><span data-stu-id="247bf-169">It will return something like:</span></span>

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. <span data-ttu-id="247bf-170">`/dev/sdc1` ドライブの UUID をコピーし、`/etc/fstab` ファイルをテキスト エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="247bf-170">Copy the UUID for the `/dev/sdc1` drive and open the `/etc/fstab` file in a text editor:</span></span>

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> <span data-ttu-id="247bf-171">`/etc/fstab` ファイルを不適切に編集すると、システムが起動できなくなる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="247bf-171">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="247bf-172">編集方法がはっきりわからない場合は、このファイルを適切に編集する方法について、ディストリビューションのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="247bf-172">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="247bf-173">また、運用システムで作業を行っている際は、ファイルを編集する前にバックアップの作成を推奨します。</span><span class="sxs-lookup"><span data-stu-id="247bf-173">It is also recommended that a backup of the file is created before editing when you are working with production systems.</span></span>

1. <span data-ttu-id="247bf-174"><kbd>G</kbd> を押してファイルの最後の行に移動します。</span><span class="sxs-lookup"><span data-stu-id="247bf-174">Press <kbd>G</kbd> to move to the last line in the file.</span></span>

1. <span data-ttu-id="247bf-175"><kbd>I</kbd> を押して挿入モードにします。</span><span class="sxs-lookup"><span data-stu-id="247bf-175">Press <kbd>I</kbd> to enter INSERT mode.</span></span> <span data-ttu-id="247bf-176">画面最下部に、モードが表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="247bf-176">It should indicate the mode at the bottom of the screen.</span></span>

1. <span data-ttu-id="247bf-177"><kbd>End</kbd> キーを押して、行の末尾に移動します。</span><span class="sxs-lookup"><span data-stu-id="247bf-177">Press the <kbd>END</kbd> key to move to the end of the line.</span></span> <span data-ttu-id="247bf-178">または、矢印キーを使うこともできます。</span><span class="sxs-lookup"><span data-stu-id="247bf-178">Alternatively, you can use the arrow keys.</span></span> <span data-ttu-id="247bf-179"><kbd>ENTER</kbd> を押して新規行に移動します。</span><span class="sxs-lookup"><span data-stu-id="247bf-179">Press <kbd>ENTER</kbd> to move to a new line.</span></span>

1. <span data-ttu-id="247bf-180">エディタに以下のラインを入力してします。</span><span class="sxs-lookup"><span data-stu-id="247bf-180">Type the following line into the editor.</span></span> <span data-ttu-id="247bf-181">複数の値は、スペースまたはタブで区切ることができます。</span><span class="sxs-lookup"><span data-stu-id="247bf-181">The values can be space or tab separated.</span></span> <span data-ttu-id="247bf-182">各列の詳細については、ドキュメントをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="247bf-182">Check the documentation for more information on each of the columns:</span></span>

    ```output
    UUID=<uuid-goes-here>    /uploads    ext4    defaults,nofail    1    2
    ```
1. <span data-ttu-id="247bf-183">**Esc** キーを押し、ファイルを書き込むには「**:w!**」と入力し、</span><span class="sxs-lookup"><span data-stu-id="247bf-183">Press **ESC**, then type **:w!**</span></span> <span data-ttu-id="247bf-184">エディターを終了するには「**:q**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="247bf-184">to write the file and **:q** to quit the editor.</span></span>

1. <span data-ttu-id="247bf-185">最後に、マウント ポイントの更新を OS に要求することによって、入力が正しいことを確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="247bf-185">Finally, let's check to make sure the entry is correct by asking the OS to refresh the mount points:</span></span>

    ```bash
    sudo mount -a
    ```

    <span data-ttu-id="247bf-186">もしエラーが返された場合、ファイルを編集して問題を探します。</span><span class="sxs-lookup"><span data-stu-id="247bf-186">If it returns an error, edit the file to find the problem.</span></span>

> [!TIP]
> <span data-ttu-id="247bf-187">一部の Linux カーネルでは、ディスク上の未使用ブロックを破棄する TRIM がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="247bf-187">Some Linux kernels support TRIM to discard unused blocks on disks.</span></span> <span data-ttu-id="247bf-188">この機能は Azure ディスクで利用可能で、もし大きなファイルを作成してそれを削除する場合に使用すると、コストを節約できます。</span><span class="sxs-lookup"><span data-stu-id="247bf-188">This feature is available on Azure disks and can save you money if you create large files and then delete them.</span></span> <span data-ttu-id="247bf-189">[この機能を有効にする](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure)方法については、Microsoft のドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="247bf-189">Learn how to [turn this feature on](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure) in our documentation.</span></span>

<span data-ttu-id="247bf-190">VM にディスクを簡単に追加する方法がわかったので、作成するディスクの種類についてもう少し詳しく調べてみましょう。</span><span class="sxs-lookup"><span data-stu-id="247bf-190">Now that you've seen how easy it is to add disks to your VMs, let's explore a bit more about the types of disks you might create.</span></span> <span data-ttu-id="247bf-191">特に、Standard と Premium のストレージの選択が重要です。</span><span class="sxs-lookup"><span data-stu-id="247bf-191">In particular the choice between Standard and Premium storage.</span></span>