コンテナーを実行、一覧表示、削除する一般的な方法について説明する前に、Ubuntu 仮想マシン (VM) でホストされている Docker コンテナーで実行される基本的な Nginx の構成を見てみましょう。

このパートで学習する内容は次のとおりです。

1. VM の初回起動時に Docker をインストールするように構成された Linux VM を作成する。
1. VM に接続し、Nginx を実行する Docker コンテナーを開始する。
1. ポート バインドを使用して、VM の外部から Web サーバーにアクセスできるようにする。

## <a name="what-are-containers"></a>コンテナーとは

コンテナーは仮想化環境ですが、仮想マシンとは異なり、完全なオペレーティング システムを常に含むとは限りません。 代わりに、そのコンテナーを実行しているホスト環境のオペレーティング システムを参照します。 たとえば、特定の Linux カーネルを使用するサーバーで 5 つのコンテナーを実行すると、5 つのコンテナーすべてがその同じカーネルで実行されます。

コンテナーを使用すると、アプリケーションとその実行に必要なすべてのものを、"_コンテナー イメージ_" と呼ばれるものにパッケージ化できます。 コンテナーは分離されており、これは同じシステムで実行している他のコンテナーに干渉しないことを意味します。 コンテナー イメージはポータブルでもあります。 Docker Hub や Azure Container Registry などのコンテナー レジストリに、イメージをアップロードできます。 その後、クラウド内でコンテナーを実行でき、開発環境のときとまったく同じように動作することを期待できます。

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

コンテナーを使用すると繰り返しを速くできます。 コンテナーは、オペレーティング システムを含む必要がないため、通常、完全な VM よりかなり小さくなります。 これにより、コンテナーの開始と停止を、多くの場合は秒単位で、迅速に行うことができます。

## <a name="understand-the-setup"></a>セットアップについて

学習が目的なので、作業のためのサンドボックス環境は提供されています。 この環境には、Azure リソースを管理および開発するためのブラウザー ベースのコマンド ライン エクスペリエンスである Azure Cloud Shell が含まれます。

Cloud Shell から Docker を含む Linux VM を作成します。 Docker コンテナーを対話的に作成および使用できるように、SSH 経由で Linux VM に接続します。

この Linux VM を、開発環境およびコンテナーについて学習する場所と考えてください。 実際には、アプリの開発に使用するコンピューターに Docker をインストールします。 Docker は、Windows、macOS、Linux で動作します。

このモジュールの最後には、ローカル開発環境で Docker の実行についてさらに学習できるリソースを提供します。

## <a name="what-is-nginx"></a>Nginx とは

Nginx (発音は "エンジンエックス") は、UNIX、Linux、macOS、および Windows で実行される、一般的な無料のオープン ソースの Web サーバーです。 Web サーバーを構成するのは、コンテナーの動作を確認するのによい方法です。 ここでは、Nginx を使用して基本的な Web ページを提供します。

## <a name="what-is-cloud-init"></a>cloud-init とは

cloud-Init を使用すると、初回起動時の Linux VM をカスタマイズできます。 cloud-init を使って、パッケージをインストールしてファイルを書き込んだり、ユーザーとセキュリティを構成したりすることができます。 ここでは、cloud-init スクリプトを使って、VM に Docker をインストールします。

## <a name="what-is-port-binding"></a>ポートのバインドとは

ポートのバインドを使用すると、ホストからコンテナーにネットワーク トラフィックを転送することができます。

Docker コンテナーは、コンテナーのホスト システム上のローカル ネットワークで実行されます。 この演習では、コンテナーのネットワークは Linux VM 上にあります。

通常、Web 要求はポート 80 (HTTP) で動作することはご存じでしょう。 コンテナーは VM 内部のローカル ネットワーク上で動作しているため、コンテナーは外部にはアクセス可できません。

Docker を使用すると、ポートを発行つまり公開して、VM の外部からポートにアクセスできるようにすることができます。 ここでは、VM 上のポート 8080 へのネットワーク トラフィックが、コンテナーのポート 80 に転送されるように、コンテナーを構成します。

## <a name="create-a-virtual-machine-to-host-docker"></a>Docker をホストする仮想マシンを作成する

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="create-the-cloud-init-script"></a>cloud-init スクリプトを作成する

1. **clouddrive** フォルダーに移動します。

    ```azurecli
    cd clouddrive
    ```

1. **vm-config** という名前の新しいフォルダーを作成します。

    ```azurecli
    mkdir vm-config
    ```

1. 新しいフォルダーに移動します。

    ```azurecli
    cd vm-config
    ```

1. Cloud Shell の統合されたエディターを使用して、新しいファイルを作成します。

    ```azurecli
    code .
    ```

1. この cloud-init スクリプトをエディターに貼り付けます。

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```yaml
    #cloud-config
    package_upgrade: true
    write_files:
      - path: /etc/systemd/system/docker.service.d/docker.conf
        content: |
          [Service]
          ExecStart=
          ExecStart=/usr/bin/dockerd
      - path: /etc/docker/daemon.json
        content: |
          {
            "hosts": ["fd://","tcp://127.0.0.1:2375"]
          }
    runcmd:
      - curl -sSL https://get.docker.com/ | sh
      - usermod -aG docker azureuser
    ```

1. ファイルを **docker-vm-config.txt** として保存します (<kbd>Ctrl + S</kbd> キーを押すか、右クリックして **[保存]** を選択)。

1. エディターを閉じます (<kbd>Ctrl + Q</kbd> キーを押すか、右クリックして **[終了]** を選択)。

### <a name="create-the-vm"></a>VM を作成する

> [!IMPORTANT]
> 通常なら、`az group create` コマンドを使用して関連するすべての Azure リソースに対してリソース グループを作成しますが、以下の演習では、使用するリソース グループが既に作成されています。 すべての手順で、自分のリソース グループとして、"**<rgn>[サンドボックス リソース グループ名]</rgn>**" を使用してください。

1. この `az vm create` コマンドを実行して、Linux VM を作成します。

    ```azurecli
    az vm create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data docker-vm-config.txt
    ```

`--custom-data` 引数では、VM の初回起動時に Docker をインストールする cloud-init スクリプトを指定します。 この処理が完了するまでに数分かかります。

## <a name="connect-to-the-vm"></a>VM に接続する

ここでは、VM に SSH で接続して VM を構成できるようにします。

1. この `az vm show` コマンドを実行して、VM の IP アドレスを取得します。

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. VM に SSH で接続します。 **ip-address** を、実際に使用する値に置換します。

    ```bash
    ssh azureuser@ip-address
    ```

1. Docker のバージョンを確認します。

    ```bash
    docker version
    ```

    > [!NOTE]
    > コマンドが失敗する場合、または Docker デーモン ソケットへの接続に関するエラー メッセージが表示される場合は、cloud-init スクリプトがまだ完了していないことを意味します。 スクリプトが完了するまで待つ方法もありますが、ここでは、`exit` を実行して SSH セッションを終了し、1、2 分してから再接続してみます。

## <a name="start-nginx"></a>Nginx を開始する

ここでは、Nginx を実行する Docker コンテナーを開始します。

1. この `docker run` コマンドを実行して、Nginx を実行する Docker コンテナーを開始します。

    ```bash
    docker run -d -p 8080:80 --name nginx nginx
    ```

    このコマンドは、Nginx で構成済みの Docker イメージを [Docker Hub](https://hub.docker.com/_/nginx?azure-portal=true) からダウンロードします。

    `--name` 引数は、後で操作できるように、Docker コンテナーに "nginx" という名前を割り当てます。

    `-p` 引数は、VM のポート 8080 へのネットワーク トラフィックを、コンテナーのポート 80 に転送します。

1. `curl` を実行し、Nginx が実行していてアクセス可能であることを確認します。

    ```bash
    curl http://localhost:8080
    ```

1. SSH セッションを終了します。

    ```bash
    exit
    ```

## <a name="access-your-web-server-from-a-web-browser"></a>Web ブラウザーから Web サーバーにアクセスする

ポート 8080 へのネットワーク トラフィックがコンテナーのポート 80 に転送されるようにコンテナーを構成したことを、思い出してください。

ポート マッピングが動作していることを確認するため、ここでは Azure ファイアウォールを通して VM へのポート 8080 を開きます。 その後、ブラウザーから Web サーバーにアクセスします。

1. この `az vm open-port` コマンドを実行し、ファイアウォールを経由して VM のポート 8080 を開きます。

    ```azurecli
    az vm open-port \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --port 8080 \
      --priority 1001
    ```

1. Web ブラウザーから、自分の VM の IP アドレスに移動します。 必ずポート 8080 を含めます (例: **http://40.121.106.164:8080**)。

    既定のホーム ページが表示されます。

    ![ブラウザーで実行されている Web サイト](../media/2-nginx-browser.png)

## <a name="stop-the-container"></a>コンテナーを停止する

ここではコンテナーを停止します。

1. まず、VM に再びログインします。 念のためその方法を次に示します。

    IP アドレスを取得します。

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

    VM に SSH で接続します。 **ip-address** を、実際に使用する値に置換します。

    ```bash
    ssh azureuser@ip-address
    ```

1. `docker stop nginx` を実行して、"nginx" という名前のコンテナーを停止します。

    ```bash
    docker stop nginx
    ```

次のパートのため、SSH 接続は開いたままにします。

お疲れさまでした。 仮想マシンが正常に作成され、その仮想マシンを使用して、Nginx でコンテナー化された Web サーバーをホストしました。
