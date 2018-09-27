所属している法律事務所は取り扱い件数を増やしており、顧客、他の法律事務所、法執行機関などのさまざまなソースの重要なドキュメントを保管するために、新しい Linux Web サーバーを設置する職務を任されました。 サーバーはアップロードを受け入れることができます。また、それをディスクに保存する必要があります。

> [!TIP]
> この演習では Linux を例に説明しますが、VM の作成とディスクの追加の基本的なプロセスは Windows の場合と同じです。 主な違いは、ディスクのパーティションとフォーマットです。このような処理は、組み込みのディスク管理ツールを使用してリモート デスクトップ セッションで行われます。

この演習の目標は、Linux 仮想マシン (VM) を作成し、"uploads" ディレクトリを保存する "uploadDataDisk1" という新しい仮想ハード ディスク (VHD) を接続することです。

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm"></a>Linux VM を作成する

Azure CLI を使用して Web サーバーをホストする Linux VM を作成しましょう。

1. まず、このセッションの既定値をいくつか設定します。 最初に、VM を配置する_場所_を決める必要があります。 これはクライアントに近い場所であることが理想的です。 この場合は、Azure サンドボックスで使用できる場所から最も近いリージョンを選択します。

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. `az configure` コマンドを使って、使用する既定の場所を設定します。 "eastus" をその場所に置き換えます。

    ```azurecli
    az configure --defaults location=eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. 既定のリソース グループ値を、Azure サンドボックス用に作成された事前構成済みリソース グループ (**<rgn>[サンドボックス リソース グループ]</rgn>**) に設定します。

    ```azurecli
    az configure --defaults group="<rgn>[sandbox Resource Group]</rgn>"
    ```

1. 次に、`vm create` コマンドを使用して新しい Ubuntu Linux VM を作成します。
    - "support-web-vm01" と名前を付けます。
    - **size** を _Standard_DS2_v2_ に設定します。
    - **admin-username** を "azureuser" (または好きなもの) に設定します。
    - `--generate-ssh-keys` パラメーターを使用して SSH キーを生成します。

    ```azurecli
    az vm create --name support-web-vm01 \
        --image UbuntuLTS \
        --size Standard_DS2_v2 \
        --admin-username azureuser \
        --generate-ssh-keys
    ```
    
    > [!TIP]
    > VM を作成して Azure にデプロイするには、数分かかることがあります。 進行状況は、Cloud Shell で確認できます。
    
1. この結果、作成された VM の詳細を含む JSON ブロックが生成されます。

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
    出力の **publicIpAddress** に注意してください。 これがリモートから VM にアクセスする方法です。

    > [!NOTE]
    > ディスクの管理方法を学習するために、この VM を使用しています。 実際は Web サーバーとして使用する予定だった場合は、`az vm open-port` コマンドで追加のポートを開き、Web サーバー ソフトウェアをインストールします。 これらのタスクはこのモジュールの対象外ですが、その手順の実行方法については、「**Azure CLI を使用して仮想マシンを管理する**」モジュールを参照してください。 

## <a name="add-an-empty-data-disk-to-our-vm"></a>空のデータ ディスクを VM に追加する

Web サーバー "uploadDataDisk1" の "uploads" ディレクトリを保存するディスクの名前を付けます。 

> [!TIP]
> `vm create` コマンドに `--data-disk-sizes-gb`パラメーターを使用して VM を作成するときに、データ ディスクを追加できます。

1. `vm disk attach` コマンドを使用して新しい空のディスクをサーバーに追加します。
    - "uploadDataDisk1" と名前を付けます。
    - 64 GB と設定します。
    - ローカルの冗長性を備えた Premium ストレージを使用するように、**sku** を "_Premium_LRS_" に設定します。ローカルの冗長性を備えた Premium ストレージを使用するように、

    ```azurecli
    az vm disk attach \
        --vm-name support-web-vm01 \
        --disk uploadDataDisk1 \
        --size-gb 64 \
        --sku Premium_LRS \
        --new
    ```
    
これで、"uploadDataDisk1" という名前のディスクが定義されます。 このディスクを使用するには、パーティションとフォーマットを実行する必要があります。

## <a name="initialize-data-disks"></a>データ ディスクの初期化

一から作成した追加ドライブは、初期化してフォーマットする必要があります。 これを行うためのプロセスは、物理ディスクと同じです。

1. まず、元の VM 作成応答から返されるパブリック IP アドレスを使用して、Linux サーバーに SSH で接続します。 まだない場合は、次のコマンドを使用して IP アドレスを取得できます。

    ```azurecli
    az vm list-ip-addresses -n support-web-vm01
    ```

1. 作成したパブリック IP とユーザー名を指定して SSH を使用します。

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. 次に、すべてのブロック デバイスを一覧表示するために `lsblk` コマンドでディスクを特定します。次のようにドライブが表示されます。

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

作成した 64 GB ドライブを探します。**sdc** が表示されており、マウントされていないことがわかります。これは初期化されていないためです。

1. 初期化する必要があるドライブ (**sdc**) がわかったら、`fdisk` を使用して行うことができます。 `sudo` でコマンドを実行し、パーティションを作成するディスクを指定する必要があります。

    ```bash
    sudo fdisk /dev/sdc
    ```

1. <kbd>n</kbd> コマンドを使用して新しいパーティションを追加します。 この例では、プライマリ パーティション用の <kbd>p</kbd> も選択し、残りの既定値はそのまま使用します。 出力は次の例のようになります。   

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

1. <kbd>p</kbd> コマンドを使ってパーティション テーブルを出力します。 これは、次のようになります。

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

1. <kbd>w<kbd> コマンドを使って変更を書き込みます。 これによってツールが終了します。

1. 次に、`mkfs` コマンドを使ってパーティションにファイル システムを書き込む必要があります。 ファイル システムの種類と、`fdisk` の出力から取得したデバイス名を指定する必要があります。
    - `-t ext4` を渡して _ext4_ ファイル システムを作成します。
    - デバイス名は `/dev/sdc` です。

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    このコマンドの完了には数分を要します。

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
    
1. 次に、マウント ポイントとして使用するディレクトリを作成します。 `uploads` フォルダーを使用するものとします。

    ```bash
    sudo mkdir /uploads
    ```
1. 最後に、`mount` を使用してディスクをマウント ポイントにアタッチします。

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```
    `lsblk` を使用し、マウントされたドライブを表示できるはずです。
    
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
    
### <a name="mounting-the-drive-automatically"></a>自動的にドライブをマウントする

再起動後にドライブが確実に自動でマウントされるようにするには、そのドライブを `/etc/fstab` ファイルに追加する必要があります。 ドライブを参照するときは、デバイス名 (`/dev/sdc1` など) だけでなく、UUID (汎用一意識別子) を `/etc/fstab` で使用することも強くお勧めします。 UUID を使用すると、OS が起動中にディスク エラーを検出した場合に、間違ったディスクが特定の場所にマウントされるのを防ぐことができます。 その後、残りのデータ ディスクは、その同じデバイス ID に割り当てられます。 新しいドライブの UUID を確認するには、`blkid` ユーティリティを使用します。

```bash
sudo -i blkid
```

次のような結果が返されます。

```output
/dev/sda1: UUID="36a59c42-c04c-4632-b83f-7015abd10358" TYPE="ext4"
/dev/sdb1: UUID="21092a62-79d4-4d8c-91de-4dd4e8b97d83" TYPE="ext4"
/dev/sdc1: UUID="e311c905-e0d9-43ab-af63-7f4ee4ef108e" TYPE="ext4"
```

1. `/dev/sdc1` ドライブの UUID をコピーし、`/etc/fstab` ファイルをテキスト エディターで開きます。

    ```bash
    sudo vi /etc/fstab
    ```

> [!WARNING]
> `/etc/fstab` ファイルを不適切に編集すると、システムが起動できなくなる可能性があります。 編集方法がはっきりわからない場合は、このファイルを適切に編集する方法について、ディストリビューションのドキュメントを参照してください。 また、運用システムで作業を行っている際は、ファイルを編集する前にバックアップの作成を推奨します。

1. <kbd>G</kbd> を押してファイルの最後の行に移動します。

1. <kbd>I</kbd> を押して挿入モードにします。 画面最下部に、モードが表示されるはずです。

1. <kbd>End</kbd> キーを押して、行の末尾に移動します。 または、矢印キーを使うこともできます。 <kbd>ENTER</kbd> を押して新規行に移動します。

1. エディタに以下のラインを入力してします。 複数の値は、スペースまたはタブで区切ることができます。 各列の詳細については、ドキュメントをご覧ください。

    ```output
    UUID=<uuid-goes-here>    /uploads    ext4    defaults,nofail    1    2
    ```
1. **Esc** キーを押し、ファイルを書き込むには「**:w!**」と入力し、 エディターを終了するには「**:q**」と入力します。

1. 最後に、マウント ポイントの更新を OS に要求することによって、入力が正しいことを確認してみましょう。

    ```bash
    sudo mount -a
    ```

    もしエラーが返された場合、ファイルを編集して問題を探します。

> [!TIP]
> 一部の Linux カーネルでは、ディスク上の未使用ブロックを破棄する TRIM がサポートされています。 この機能は Azure ディスクで利用可能で、もし大きなファイルを作成してそれを削除する場合に使用すると、コストを節約できます。 [この機能を有効にする](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure)方法については、Microsoft のドキュメントを参照してください。

VM にディスクを簡単に追加する方法がわかったので、作成するディスクの種類についてもう少し詳しく調べてみましょう。 特に、Standard と Premium のストレージの選択が重要です。