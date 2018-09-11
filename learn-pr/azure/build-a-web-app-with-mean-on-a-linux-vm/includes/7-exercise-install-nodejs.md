<span data-ttu-id="8f869-101">このユニットでは、Azure でホストされている Ubuntu Linux 仮想マシンに Node.js (頭字語 MEAN の **N**) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="8f869-101">In this unit, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine.</span></span> <span data-ttu-id="8f869-102">Node.js は、HTTP トラフィックを処理し、Web アプリケーションをホストするためのランタイムとして機能します。</span><span class="sxs-lookup"><span data-stu-id="8f869-102">Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="8f869-103">VM に接続する</span><span class="sxs-lookup"><span data-stu-id="8f869-103">Connect to the VM</span></span>

<span data-ttu-id="8f869-104">Node.js をインストールするには、**ssh** を使用して VM に接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8f869-104">In order to install Node.js, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="8f869-105">まだ VM に接続していない場合は、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="8f869-105">If you aren't still connected to your VM, run the following command.</span></span> <span data-ttu-id="8f869-106">`<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご自分の VM のパブリック IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8f869-106">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-nodejs"></a><span data-ttu-id="8f869-107">Node.js をインストールする</span><span class="sxs-lookup"><span data-stu-id="8f869-107">Install Node.js</span></span>

> [!Important]
> <span data-ttu-id="8f869-108">Ubuntu では、**Node.js-legacy** という名前の非公式のパッケージを提供しています。</span><span class="sxs-lookup"><span data-stu-id="8f869-108">Ubuntu provides an unofficial package called **Node.js-legacy**.</span></span> <span data-ttu-id="8f869-109">このパッケージは、期限が切れており、Node.js では管理されていません。</span><span class="sxs-lookup"><span data-stu-id="8f869-109">This package is not maintained by Node.js and is outdated.</span></span>

1. <span data-ttu-id="8f869-110">ご使用の仮想マシンにインストールできる適切なパッケージを **apt-get** で検出できるように、Node.js パッケージ リポジトリを登録します。</span><span class="sxs-lookup"><span data-stu-id="8f869-110">Register the Node.js package repository, so **apt-get** can find the right package to install on your virtual machine.</span></span>

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. <span data-ttu-id="8f869-111">Linux システムに Node.js パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8f869-111">Install the Node.js package on your Linux system.</span></span>

    ```bash
    sudo apt-get install -y Node.js
    ```

1. <span data-ttu-id="8f869-112">次の単純な Node.js コマンドを実行して、Node.js のインストールが成功したことを確認します。</span><span class="sxs-lookup"><span data-stu-id="8f869-112">Verify the Node.js installation succeeded by running the following simple Node.js command.</span></span>

    ```bash
    node -v
    ```

    <span data-ttu-id="8f869-113">出力は `v8.11.4` のようになり、パッケージのインストール時点で利用できる最新の Node.js バージョンが反映されたバージョンが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8f869-113">The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version that's available when you install the package.</span></span>

## <a name="summary"></a><span data-ttu-id="8f869-114">まとめ</span><span class="sxs-lookup"><span data-stu-id="8f869-114">Summary</span></span>

<span data-ttu-id="8f869-115">仮想マシンに Node.js をインストールすると、実行を担当する Web アプリケーションを構築できるようになります。</span><span class="sxs-lookup"><span data-stu-id="8f869-115">With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.</span></span>