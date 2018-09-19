Linux VM をデプロイして実行させましたが、作業を行うようには構成していません。 SSH で VM に接続して Apache を構成し、Web サーバーを実行させましょう。

## <a name="connect-to-the-vm-with-ssh"></a>SSH を使って VM に接続する

SSH クライアントを使って Azure VM に接続するには、次が必要になります。

- SSH クライアント ソフトウェア (ほとんどの最新オペレーティング システムに存在します)
- VM のパブリック IP アドレス (または、ユーザーのネットワークに接続するように VM を構成する場合はプライベート)。

### <a name="get-the-public-ip-address"></a>パブリック IP アドレスの取得

1. [Azure portal](https://portal.azure.com?azure-portal=true) 内で、以前に作成した仮想マシンの**概要**パネルが開いていることを確認します。 もしパネルを開く必要がある場合は、**すべてのリソース**下で VM を見つけることができます。 概要パネルには、VM に関する多数の情報があります。

    - VM が実行中か否かを確認可能
    - VM の停止または再起動
    - VM へ接続するためのパブリック IP アドレスを取得
    - CPU、ディスク、およびネットワークのアクティビティを確認

1. ウィンドウ最上部の**接続**ボタンをクリックします。

1. **仮想マシンへの接続**ブレード内で、**IP アドレス**および**ポート番号**設定をメモします。 **SSH**タブ上で、VM に接続するためにローカルで実行する必要があるコマンドも表示されています。 これをクリップボードにコピーします。

## <a name="connect-with-ssh"></a>SSH を使用して接続する

1. [SSH] タブから取得したコマンド ラインを Azure Cloud Shell に貼り付けます。 次のようになります。ただし、IP アドレスは異なります (また、**jim** を使用しなかった場合はユーザー名もおそらく異なります)。

    ```bash
    ssh jim@137.117.101.249
    ```

1. このコマンドを使用すると、Secure Shell 接続が開き、Linux 用の従来のシェル コマンド プロンプトに移動します。

1. ここで Linux コマンドをいくつか実行してみてください。
    - `ls -la /` を使ってディスクのルートを表示します。
    - `ps -l` を使って実行中のプロセスをすべて表示します。
    - `dmesg` を使ってカーネル メッセージ一覧をすべて表示します。
    - `lsblk` を使ってブロック デバイス一覧を全て表示します。ここにお使いのドライブが表示されます。

ドライブの一覧でさらに興味深いことは、"_表示されていない_" デバイスです。 **データ** ドライブ（`sdc`）が存在しているが、ファイル システムにはマウントされていないことに注意してください。 Azure で VHD が追加されていますが、初期化はされていません。

## <a name="initialize-data-disks"></a>データ ディスクの初期化

一から作成した追加ドライブは、初期化してフォーマットする必要があります。 これを行うためのプロセスは、物理ディスクと同じです。

1. 最初に、ディスクを特定します。 これは上で行いました。 `dmesg | grep SCSI` を使用して、SCSI デバイス用のカーネルからすべてのメッセージを一覧表示することもできます。

1. 初期化する必要があるドライブ (`sdc`) がわかったら、`fdisk` を使用して行うことができます。 `sudo` でコマンドを実行し、パーティションを作成するディスクを指定する必要があります。

    ```bash
    sudo fdisk /dev/sdc
    ```
1. `n` コマンドを使用して新しいパーティションを追加します。 この例では、プライマリ パーティション用の **p** も選択し、残りの既定値はそのまま使用します。 出力は次の例のようになります。   

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

1. `p` コマンドを使ってパーティション テーブルを出力します。 これは、次のようになります。

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

1. `w` コマンドを使って変更を書き込みます。 これによってツールが終了します。

1. 次に、`mkfs` コマンドを使ってパーティションにファイル システムを書き込む必要があります。 ファイル システムの種類と、`fdisk` の出力から取得したデバイス名を指定する必要があります。
    - `-t ext4` を渡して _ext4_ ファイル システムを作成します。
    - デバイス名は `/dev/sdc` です。

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
    
    このコマンドの完了には数分を要します。

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

1. 次に、マウント ポイントとして使用するディレクトリを作成します。 `data` フォルダーを使用するものとします。

    ```bash
    sudo mkdir /data
    ```
1. 最後に、`mount` を使用してディスクをマウント ポイントにアタッチします。

    ```bash
    sudo mount /dev/sdc1 /data
    ```
    `lsblk` を使用し、マウントされたドライブを表示できるはずです。
    
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

1. **G** を押してファイルの最後の行に移動します。

1. **I** を押して挿入モードにします。 画面最下部に、モードが表示されるはずです。

1. **End** キーを押して、行の末尾に移動します。 または、矢印キーを使うこともできます。 **ENTER** を押して新規行に移動します。

1. エディタに以下のラインを入力してします。 複数の値は、スペースまたはタブで区切ることができます。 各列の詳細については、ドキュメントをご覧ください。

    ```output
    UUID=<uuid-goes-here>    /data    ext4    defaults,nofail    1    2
    ```
1. **Esc** キーを押し、ファイルを書き込むには「**:w!**」と入力し、 エディターを終了するには「**:q**」と入力します。

1. 最後に、マウント ポイントの更新を OS に要求することによって、入力が正しいことを確認してみましょう。

    ```bash
    sudo mount -a
    ```

    もしエラーが返された場合、ファイルを編集して問題を探します。

> [!TIP]
> 一部の Linux カーネルでは、ディスク上の未使用ブロックを破棄する TRIM がサポートされています。 この機能は Azure ディスクで利用可能で、もし大きなファイルを作成してそれを削除する場合に使用すると、コストを節約できます。 [この機能を有効にする](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal#trimunmap-support-for-linux-in-azure)方法は、ドキュメントをご覧ください。

## <a name="install-software-onto-the-vm"></a>VM 上にソフトウェアをインストールする

ご覧のとおり、SSH を使うことでローカルコンピューターと同様に Linux VM を操作できます。 他の Linux コンピューターと同じようにこの VM を管理することができ、ソフトウェアのインストール、ロールの構成、機能の調整、他の日常的なタスクを実行できます。 少しの間、ソフトウェアのインストールに集中しましょう。

VM にソフトウェアをインストールするにはいくつかのオプションがあります。 最初に、前に説明したように、`scp` を使用してお使いのコンピューターから VM にローカル ファイルをコピーできます。 この方法では、データまたは実行するカスタム アプリケーションをコピーできます。

Secure Shell を使用してソフトウェアをインストールすることもできます。 Azure マシンは既定でインターネットに接続されています。 標準のコマンドを使用して、標準的なリポジトリから人気のあるソフトウェア パッケージを直接インストールできます。 この方法を使用して Apache をインストールしましょう。

### <a name="install-the-apache-web-server"></a>Apache Web サーバーをインストールする

Apache は Ubuntu の既定のソフトウェア リポジトリ内で使用できるので、従来のパッケージ管理ツールを使用してインストールします。

1. 最初に、ローカル パッケージ インデックスを更新し、最新のアップストリームの変更を反映させます。

    ```bash
    sudo apt-get update
    ```
    
1. 次に、Apache をインストールします。

    ```bash
    sudo apt-get install apache2 -y
    ```

1. これは自動で開始されるはずです。状態は `systemctl` を使用して確認できます。

    ```bash
    sudo systemctl status apache2
    ```

    次のような結果が返されます。

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
    > このようなコマンドを実行することはささいなことですが、それは手動プロセスです。常にインストールする必要があるソフトウェアがある場合は、スクリプトを使用してプロセスの自動化を検討します。
    
1. 最後に、パブリック IP アドレスを使用して既定ページの取得を試行できます。 ただし、Web サーバーを VM で実行している場合でも、有効な接続または応答は得られません。 それはなぜでしょうか。

Web サーバーとやりとりするには、もう 1 つの手順を実行する必要があります。 今回の仮想ネットワークは受信要求をブロックしています。これは既定の動作です。 これを構成で変更できます。 これについては次で確認します。