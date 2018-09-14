<span data-ttu-id="cf663-101">このユニットでは、Azure でホストされている Ubuntu Linux 仮想マシンに Node.js (頭字語 MEAN の **N**) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="cf663-101">In this unit, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine.</span></span> <span data-ttu-id="cf663-102">Node.js は、HTTP トラフィックを処理し、Web アプリケーションをホストするためのランタイムとして機能します。</span><span class="sxs-lookup"><span data-stu-id="cf663-102">Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.</span></span>

## <a name="install-nodejs"></a><span data-ttu-id="cf663-103">Node.js をインストールする</span><span class="sxs-lookup"><span data-stu-id="cf663-103">Install Node.js</span></span>

1. <span data-ttu-id="cf663-104">**確信が持てないかどうかでも SSHed、前の演習から VM に**SSH でします。</span><span class="sxs-lookup"><span data-stu-id="cf663-104">**If you aren't still SSHed into your VM from the previous exercise**, SSH in.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="cf663-105">ご使用の仮想マシンにインストールできる適切なパッケージを **apt-get** で検出できるように、Node.js パッケージ リポジトリを登録します。</span><span class="sxs-lookup"><span data-stu-id="cf663-105">Register the Node.js package repository so **apt-get** can find the right package to install on your virtual machine.</span></span>

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. <span data-ttu-id="cf663-106">Linux システムに Node.js パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="cf663-106">Install the Node.js package on your Linux system.</span></span>

    ```bash
    sudo apt-get install -y Node.js
    ```

1. <span data-ttu-id="cf663-107">次の単純な Node.js コマンドを実行して、Node.js のインストールが成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="cf663-107">Verify the Node.js installation succeeded by running the following simple Node.js command.</span></span>

    ```bash
    node -v
    ```

    <span data-ttu-id="cf663-108">出力は `v8.11.4` のようになり、パッケージのインストール時点で利用できる最新の Node.js バージョンが反映されたバージョンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cf663-108">The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version that's available when you install the package.</span></span>

## <a name="summary"></a><span data-ttu-id="cf663-109">まとめ</span><span class="sxs-lookup"><span data-stu-id="cf663-109">Summary</span></span>

<span data-ttu-id="cf663-110">仮想マシンに Node.js をインストールすると、実行を担当する Web アプリケーションを構築できるようになります。</span><span class="sxs-lookup"><span data-stu-id="cf663-110">With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.</span></span>