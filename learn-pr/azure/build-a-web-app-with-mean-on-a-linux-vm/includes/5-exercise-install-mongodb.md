<span data-ttu-id="ee7a9-101">このユニットでは、今後のサンプル Web アプリケーションのデータ ストアとして動作する MongoDB を Ubuntu Linux 仮想マシンにインストールします。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-101">In this unit, you will install MongoDB on your Ubuntu Linux virtual machine to act as a data store for your upcoming sample web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="ee7a9-102">VM に接続する</span><span class="sxs-lookup"><span data-stu-id="ee7a9-102">Connect to the VM</span></span>

<span data-ttu-id="ee7a9-103">MongoDB をインストールするには、**ssh** を使用して VM に接続する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-103">In order to install MongoDB, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="ee7a9-104">`<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご自分の VM のパブリック IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-104">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-mongodb"></a><span data-ttu-id="ee7a9-105">MongoDB をインストールする</span><span class="sxs-lookup"><span data-stu-id="ee7a9-105">Install MongoDB</span></span>

> [!Important]
> <span data-ttu-id="ee7a9-106">Ubuntu では、**mongodb** という名前の非公式のパッケージを提供しています。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-106">Ubuntu provides an unofficial package called **mongodb**.</span></span> <span data-ttu-id="ee7a9-107">このパッケージは MongoDB Inc. では管理されていません。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-107">This package is not maintained by MongoDB Inc.</span></span>

1. <span data-ttu-id="ee7a9-108">MongoDB リポジトリの暗号化キーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-108">Import the encryption key for the MongoDB repository.</span></span> <span data-ttu-id="ee7a9-109">これで、インストールする mongodb パッケージが MongoDB Inc. からリリースされたものであることをパッケージ マネージャーで確認できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-109">This will allow the package manager to verify that the mongodb packages you install are coming from MongoDB Inc.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    <span data-ttu-id="ee7a9-110">**sudo** コマンドは、管理特権を使用して、指定されたコマンドを実行することを意味します。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-110">The **sudo** command means that we want to run the specified command with administrative privileges.</span></span>

1. <span data-ttu-id="ee7a9-111">パッケージ マネージャーで mongodb パッケージを検出できるように、MongoDB Ubuntu リポジトリを登録します。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-111">Register the MongoDB Ubuntu repository so the package manager can locate the mongodb packages.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ee7a9-112">このコマンドは Ubuntu のバージョンによって異なります。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-112">This command is different for different versions of Ubuntu.</span></span> <span data-ttu-id="ee7a9-113">使用している Ubuntu のバージョンを確認するには、`uname -v` を実行します。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-113">To find out which version of Ubuntu you're using, run: `uname -v`.</span></span>
    > <span data-ttu-id="ee7a9-114">このコマンドの出力は、`#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018` のようになります。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-114">This command will output something like this: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span></span>
    >
    > <span data-ttu-id="ee7a9-115">この出力は、Ubuntu バージョン 16.04.1 を実行していることを示しています。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-115">This output indicates that we're running Ubuntu version 16.04.1.</span></span>
    > <span data-ttu-id="ee7a9-116">使用しているバージョンの正確なコマンドを確認するには、[MongoDB Community Edition を Ubuntu にインストールする方法](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)に関するドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-116">Refer to the [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) documentation to get the exact command for your version.</span></span>

    <span data-ttu-id="ee7a9-117">Ubuntu 16.04 上で以下のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-117">On Ubuntu 16.04, we run this command:</span></span>

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. <span data-ttu-id="ee7a9-118">最新のパッケージ情報が得られるように、パッケージ データベースをリロードします。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-118">Reload the package database so we have the latest package information.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="ee7a9-119">MongoDB パッケージを VM にインストールします。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-119">Install the MongoDB package onto our VM.</span></span>

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. <span data-ttu-id="ee7a9-120">MongoDB サービスを開始して、後で接続できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-120">Start the MongoDB service so you can connect to it later.</span></span>

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a><span data-ttu-id="ee7a9-121">まとめ</span><span class="sxs-lookup"><span data-stu-id="ee7a9-121">Summary</span></span>

<span data-ttu-id="ee7a9-122">Ubuntu Linux VM に MongoDB をインストールしました。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-122">We now have MongoDB installed on our Ubuntu Linux VM.</span></span> <span data-ttu-id="ee7a9-123">MongoDB は、Web アプリケーションで保存および取得した情報のバッキング データ ストアとして機能します。</span><span class="sxs-lookup"><span data-stu-id="ee7a9-123">MongoDB will serve as your backing data store for the information you save and retrieve in your web application.</span></span>