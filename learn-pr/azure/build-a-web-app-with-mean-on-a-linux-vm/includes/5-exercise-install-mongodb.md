<span data-ttu-id="aa2cb-101">このユニットでは、今後のサンプル Web アプリケーションのデータ ストアとして動作する MongoDB を Ubuntu Linux 仮想マシンにインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-101">In this unit, you will install MongoDB on your Ubuntu Linux virtual machine to act as a data store for your upcoming sample web application.</span></span>

## <a name="install-mongodb"></a><span data-ttu-id="aa2cb-102">MongoDB をインストールする</span><span class="sxs-lookup"><span data-stu-id="aa2cb-102">Install MongoDB</span></span>

1. <span data-ttu-id="aa2cb-103">Cloud Shell から SSH で VM に接続します。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-103">From Cloud Shell, SSH into your VM.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="aa2cb-104">MongoDB リポジトリの暗号化キーをインポートします。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-104">Import the encryption key for the MongoDB repository.</span></span> <span data-ttu-id="aa2cb-105">これで、インストールする mongodb パッケージが MongoDB Inc. からリリースされたものであることをパッケージ マネージャーで確認できるようになります。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-105">This will allow the package manager to verify that the mongodb packages you install are coming from MongoDB Inc.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    <span data-ttu-id="aa2cb-106">**sudo** コマンドは、管理特権を使用して、指定されたコマンドを実行することを意味します。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-106">The **sudo** command means that we want to run the specified command with administrative privileges.</span></span>

1. <span data-ttu-id="aa2cb-107">パッケージ マネージャーで mongodb パッケージを検出できるように、MongoDB Ubuntu リポジトリを登録します。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-107">Register the MongoDB Ubuntu repository so the package manager can locate the mongodb packages.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa2cb-108">このコマンドは Ubuntu のバージョンによって異なります。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-108">This command is different for different versions of Ubuntu.</span></span> <span data-ttu-id="aa2cb-109">使用している Ubuntu のバージョンを確認するには、`uname -v` を実行します。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-109">To find out which version of Ubuntu you're using, run: `uname -v`.</span></span>
    > <span data-ttu-id="aa2cb-110">このコマンドの出力は、`#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018` のようになります。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-110">This command will output something like this: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.</span></span>
    >
    > <span data-ttu-id="aa2cb-111">この出力は、Ubuntu バージョン 16.04.1 を実行していることを示しています。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-111">This output indicates that we're running Ubuntu version 16.04.1.</span></span>
    > <span data-ttu-id="aa2cb-112">使用しているバージョンの正確なコマンドを確認するには、[MongoDB Community Edition を Ubuntu にインストールする方法](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)に関するドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-112">Refer to the [Install MongoDB Community Edition on Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) documentation to get the exact command for your version.</span></span>

    <span data-ttu-id="aa2cb-113">Ubuntu 16.04 上で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-113">On Ubuntu 16.04, we run this command:</span></span>

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. <span data-ttu-id="aa2cb-114">最新のパッケージ情報が得られるように、パッケージ データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-114">Update the package database so we have the latest package information.</span></span>

    ```bash
    sudo apt-get update
    ```

1. <span data-ttu-id="aa2cb-115">MongoDB パッケージを VM にインストールします。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-115">Install the MongoDB package onto our VM.</span></span>

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. <span data-ttu-id="aa2cb-116">MongoDB サービスを開始して、後で接続できるようにします。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-116">Start the MongoDB service so you can connect to it later.</span></span>

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a><span data-ttu-id="aa2cb-117">まとめ</span><span class="sxs-lookup"><span data-stu-id="aa2cb-117">Summary</span></span>

<span data-ttu-id="aa2cb-118">Ubuntu Linux VM に MongoDB をインストールしました。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-118">We now have MongoDB installed on our Ubuntu Linux VM.</span></span> <span data-ttu-id="aa2cb-119">MongoDB は、Web アプリケーションで保存および取得した情報のバッキング データ ストアとして機能します。</span><span class="sxs-lookup"><span data-stu-id="aa2cb-119">MongoDB will serve as your backing data store for the information you save and retrieve in your web application.</span></span>