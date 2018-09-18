<span data-ttu-id="c6a36-101">コンポーネントの MEAN スタックには、サーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="c6a36-101">The MEAN stack of components requires a server.</span></span> <span data-ttu-id="c6a36-102">これは、ご使用のサーバー ルームで実行している Linux マシンや仮想マシンでも、それをクラウドベースの仮想マシンに構成するのでもかまいません。</span><span class="sxs-lookup"><span data-stu-id="c6a36-102">It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine.</span></span> <span data-ttu-id="c6a36-103">このモジュールでは、Azure で実行されている Ubuntu Linux 仮想マシンで実行するスタックを設定します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-103">In this module, we will set up the stack to run on an Ubuntu Linux virtual machine running on Azure.</span></span>

<span data-ttu-id="c6a36-104">このユニットでは、Azure にホストされる、新しい Ubuntu Linux 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-104">In this unit, you will be creating a new Ubuntu Linux virtual machine hosted on Azure.</span></span> <span data-ttu-id="c6a36-105">MEAN スタック コンポーネントは、既存の仮想マシンや物理ホスト マシンにインストールすることも可能です。</span><span class="sxs-lookup"><span data-stu-id="c6a36-105">You could also install your MEAN stack components on an existing virtual machine or physical host machine.</span></span> <span data-ttu-id="c6a36-106">この演習では新しいものを作成することにより、すべてのコンポーネントを Azure リソース グループにまとめて、演習の完了後の管理とクリーンアップを簡単にすることができます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-106">By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.</span></span>

<span data-ttu-id="c6a36-107">Linux VM は、Azure portal に統合されている Cloud Shell コマンドラインを使用して作成します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-107">We will use the Cloud Shell command line that's integrated into the Azure portal to create the Linux VM.</span></span>

## <a name="provision-an-ubuntu-linux-vm"></a><span data-ttu-id="c6a36-108">Ubuntu Linux VM をプロビジョニングする</span><span class="sxs-lookup"><span data-stu-id="c6a36-108">Provision an Ubuntu Linux VM</span></span>

1. <span data-ttu-id="c6a36-109">[Azure portal](https://portal.azure.com?azure-portal=true) にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="c6a36-109">Go to the [Azure portal](https://portal.azure.com?azure-portal=true).</span></span>

1. <span data-ttu-id="c6a36-110">Azure portal のツールバーの山かっこ (>_) アイコンから Cloud Shell を開きます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-110">Open Cloud Shell from the angle bracket (>_) icon in the Azure portal toolbar.</span></span>

<!---TODO: Update for sandbox--->
1. <span data-ttu-id="c6a36-111">Cloud Shell でコマンドを実行し、使用する VM が含まれる Azure リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-111">In Cloud Shell, execute the command to create an Azure resource group, which will include our VM.</span></span> <span data-ttu-id="c6a36-112">`<resource-group-name>` をご自分のリソース グループ名に、また `<resource-group-location>` をご希望の Azure の場所 (`westus` など) に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-112">Substitute your own resource group name for `<resource-group-name>` and your desired Azure location for `<resource-group-location>` (`westus`, for example).</span></span>


    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    <span data-ttu-id="c6a36-113">ご自分のリソース グループ名は、その他のコマンドでも使用するので覚えておきます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-113">Remember your resource group name, as we will use it in other commands.</span></span>

1. <span data-ttu-id="c6a36-114">Cloud Shell で次のコマンドを実行して、新しい Ubuntu Linux VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-114">In Cloud Shell, run the following command to create a new Ubuntu Linux VM.</span></span> <span data-ttu-id="c6a36-115">`<resource-group-name>` をご自分のリソース グループ名に、また `<vm-admin-username>` と `<vm-admin-password>` をご希望の管理ユーザー名とパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-115">Substitute your own resource group name for `<resource-group-name>` and your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.</span></span>

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    <span data-ttu-id="c6a36-116">この VM に後で接続できるよう、管理者名とパスワードをメモしておきます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-116">Take note of your admin username and password to allow you to connect to this VM later.</span></span>

    <span data-ttu-id="c6a36-117">このコマンドの完了には約 2 分かかります。</span><span class="sxs-lookup"><span data-stu-id="c6a36-117">This command takes about two minutes to complete.</span></span> <span data-ttu-id="c6a36-118">コマンドの完了の結果の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="c6a36-118">When the command finishes, the resulting output will look similar to this.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<location you chose for the resource group>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<name you chose for thr resource group>",
        "zones": ""
    }
    ```

    <span data-ttu-id="c6a36-119">VM に接続するには、新たに作成した VM のパブリック IP アドレスも保存したおいた方がよいでしょう。</span><span class="sxs-lookup"><span data-stu-id="c6a36-119">You will also want to save the public IP address of the newly created VM in order to connect to the VM.</span></span>

1. <span data-ttu-id="c6a36-120">ご利用の新しい VM への接続を試します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-120">Try connecting to your new VM.</span></span>

    <span data-ttu-id="c6a36-121">コマンド プロンプトまたはターミナル ウィンドウを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-121">Open a command prompt/terminal window and run the following command.</span></span> <span data-ttu-id="c6a36-122">`<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご利用の VM のパブリック IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-122">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    <span data-ttu-id="c6a36-123">マシンに最初に接続したとき、そのリモート マシンを信頼するかたずねられます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-123">The first time you connect to the machine, you'll be asked if you trust the remote machine.</span></span> <span data-ttu-id="c6a36-124">`yes` と回答すると、マシンの ECDSA キー フィンガープリントがローカルに保存されるので、それ以降の接続は信頼されるものとなります。</span><span class="sxs-lookup"><span data-stu-id="c6a36-124">By answering `yes`, the machine's ECDSA key fingerprint will be saved locally, so subsequent connections will be trusted.</span></span>

    <span data-ttu-id="c6a36-125">すべてに問題がないようであれば、`exit` と入力し、SSH セッションを閉じます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-125">If everything looks fine, type `exit` to close the SSH session.</span></span>

1. <span data-ttu-id="c6a36-126">ポート 80 を開き、作成する新しい Web アプリケーションへの受信 HTTP トラフィックを許可します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-126">Open port 80 to allow incoming HTTP traffic to the new web application that you will create.</span></span>

    <span data-ttu-id="c6a36-127">Azure portal 上で、Cloud Shell に戻ります。</span><span class="sxs-lookup"><span data-stu-id="c6a36-127">Go back to Cloud Shell on the Azure portal.</span></span> <span data-ttu-id="c6a36-128">`<resource-group-name>` の元のリソース グループ名を使用して、次のコマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="c6a36-128">Issue the following command using your original resource group name for `<resource-group-name>`.</span></span>

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    <span data-ttu-id="c6a36-129">このコマンドにより、作成時に "MeanDemo" と命名された HTTP ポートがご利用の VM で開きます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-129">This command will open up the HTTP port on your VM that was named "MeanDemo" when it was created.</span></span>

## <a name="summary"></a><span data-ttu-id="c6a36-130">まとめ</span><span class="sxs-lookup"><span data-stu-id="c6a36-130">Summary</span></span>

<span data-ttu-id="c6a36-131">新しい Ubuntu Linux VM の準備が完了したら、それに接続して、MEAN スタックのさまざまなコンポーネントのインストールを開始できます。</span><span class="sxs-lookup"><span data-stu-id="c6a36-131">With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.</span></span>