Linux VM をデプロイして実行させましたが、作業を行うようには構成していません。 SSH で VM に接続して Apache を構成し、Web サーバーを実行させましょう。

## <a name="connect-to-the-vm-with-ssh"></a>SSH を使って VM に接続する

SSH クライアントを使って Azure VM に接続するには、次が必要になります。

- SSH クライアント ソフトウェア (ほとんどの最新オペレーティング システムに存在します)
- VM のパブリック IP アドレス (または、ユーザーのネットワークに接続するように VM を構成する場合はプライベート)。

### <a name="get-the-public-ip-address"></a>パブリック IP アドレスの取得

1. [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) 内で、以前に作成した仮想マシンの**概要**パネルが開いていることを確認します。 もしパネルを開く必要がある場合は、**すべてのリソース**下で VM を見つけることができます。 概要パネルには、VM に関する多数の情報があります。

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

1. 初期化する必要があるドライブ (`sdc`) がわかったら、`fdisk` を使用して行うことができます。 `sudo` コマンドを実行し、パーティションを作成するディスクを指定する必要があります。 次のコマンドを使用して、新しいプライマリ パーティションを作成します。

    ```bash
    (echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
    ```

1. 次に、`mkfs` コマンドを使ってパーティションにファイル システムを書き込む必要があります。

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```

1. 最後に、ファイル システムにドライブをマウントする必要があります。 `data` フォルダーを使用するものとします。 マウント ポイント フォルダーを作成して、ドライブをマウントします。

    ```bash
    sudo mkdir /data & sudo mount /dev/sdc1 /data
    ```

    > [!TIP]
    > ディスクを初期化し、マウントしました。 このプロセスの詳細に関心がある場合は、「**Add and size disks in Azure virtual machines**」(Azure 仮想マシンにディスクを追加し、サイズを変更する) モジュールを参照してください。 そこで、このタスクについて詳しく説明しています。

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
    sudo systemctl status apache2 --no-pager
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