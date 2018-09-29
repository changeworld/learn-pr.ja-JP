<span data-ttu-id="6e26a-101">コンポーネントの MEAN スタックには、サーバーが必要です。</span><span class="sxs-lookup"><span data-stu-id="6e26a-101">The MEAN stack of components requires a server.</span></span> <span data-ttu-id="6e26a-102">これは、ご使用のサーバー ルームで実行している Linux マシンや仮想マシンでも、それをクラウドベースの仮想マシンに構成するのでもかまいません。</span><span class="sxs-lookup"><span data-stu-id="6e26a-102">It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine.</span></span> <span data-ttu-id="6e26a-103">このモジュールでは、Azure で実行されている Ubuntu Linux 仮想マシンで実行するスタックを設定します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-103">In this module, we will set up the stack to run on an Ubuntu Linux virtual machine running on Azure.</span></span>

<span data-ttu-id="6e26a-104">このユニットでは、Azure にホストされる、新しい Ubuntu Linux 仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-104">In this unit, you will be creating a new Ubuntu Linux virtual machine hosted on Azure.</span></span> <span data-ttu-id="6e26a-105">MEAN スタック コンポーネントは、既存の仮想マシンや物理ホスト マシンにインストールすることも可能です。</span><span class="sxs-lookup"><span data-stu-id="6e26a-105">You could also install your MEAN stack components on an existing virtual machine or physical host machine.</span></span> <span data-ttu-id="6e26a-106">この演習では新しいものを作成することにより、すべてのコンポーネントを Azure リソース グループにまとめて、演習の完了後の管理とクリーンアップを簡単にすることができます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-106">By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.</span></span>

## <a name="provision-an-ubuntu-linux-vm"></a><span data-ttu-id="6e26a-107">Ubuntu Linux VM をプロビジョニングする</span><span class="sxs-lookup"><span data-stu-id="6e26a-107">Provision an Ubuntu Linux VM</span></span>

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="creating-a-resource-group"></a><span data-ttu-id="6e26a-108">リソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="6e26a-108">Creating a resource group</span></span>

<span data-ttu-id="6e26a-109">通常、新しい一連のリソースを作成するときにまず行うことは、_リソース グループ_を作成してそれらすべてを所有することです。</span><span class="sxs-lookup"><span data-stu-id="6e26a-109">Normally, the first thing you'll do when creating a new set of resources is create a _resource group_ to own them all.</span></span> <span data-ttu-id="6e26a-110">これは Azure サンドボックスでは不要な手順ですが、独自のサブスクリプションで作業している場合は、次のコマンドを使用して、ご自分の近くの場所にリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-110">This is an unnecessary step in the Azure sandbox, but when you are working in your own subscription use the following command to create a resource group in a location near you.</span></span>

```azurecli
az group create --name <resource-group-name> --location <resource-group-location>
```

> [!IMPORTANT]
> <span data-ttu-id="6e26a-111">Azure サンドボックスでは、リソース グループを作成する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="6e26a-111">You do not need to create a resource group with the Azure sandbox.</span></span> <span data-ttu-id="6e26a-112">代わりに、**<rgn>[サンドボックス リソース グループ名]</rgn>** という名前の事前に作成されたリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-112">Instead use the pre-created resource group named **<rgn>[sandbox Resource Group]</rgn>**.</span></span>

1. <span data-ttu-id="6e26a-113">右側にある Cloud Shell で、次のコマンドを入力して、新しい Ubuntu Linux VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-113">In the Cloud Shell on the right, type the following command to create a new Ubuntu Linux VM.</span></span> <span data-ttu-id="6e26a-114">`<vm-admin-username>` および `<vm-admin-password>` をご希望の管理ユーザー名とパスワードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-114">Substitute your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.</span></span>

    ```azurecli
    az vm create \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    <span data-ttu-id="6e26a-115">この VM に後で接続できるよう、管理者名とパスワードをメモしておきます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-115">Take note of your admin username and password to allow you to connect to this VM later.</span></span>

    <span data-ttu-id="6e26a-116">このコマンドの完了には約 2 分かかります。</span><span class="sxs-lookup"><span data-stu-id="6e26a-116">This command takes about two minutes to complete.</span></span> <span data-ttu-id="6e26a-117">コマンドの完了の結果の出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="6e26a-117">When the command finishes, the resulting output will look similar to this.</span></span>

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<resource group location>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<rgn>[sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    <span data-ttu-id="6e26a-118">VM に接続するには、新たに作成した VM のパブリック IP アドレスも保存したおいた方がよいでしょう。</span><span class="sxs-lookup"><span data-stu-id="6e26a-118">You will also want to save the public IP address of the newly created VM in order to connect to the VM.</span></span>

1. <span data-ttu-id="6e26a-119">新しい VM への接続を試します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-119">Try connecting to your new VM.</span></span>

    <span data-ttu-id="6e26a-120">Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-120">From the Cloud Shell, run the following command.</span></span> <span data-ttu-id="6e26a-121">`<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご利用の VM のパブリック IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-121">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    <span data-ttu-id="6e26a-122">マシンに最初に接続したとき、そのリモート マシンを信頼するかたずねられます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-122">The first time you connect to the machine, you'll be asked if you trust the remote machine.</span></span> <span data-ttu-id="6e26a-123">`yes` と回答すると、マシンの ECDSA キー フィンガープリントがローカルに保存されるので、それ以降の接続は信頼されるものとなります。</span><span class="sxs-lookup"><span data-stu-id="6e26a-123">By answering `yes`, the machine's ECDSA key fingerprint will be saved locally, so subsequent connections will be trusted.</span></span> <span data-ttu-id="6e26a-124">パスワードの入力が求められます。これは接続するたびに表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-124">You'll then be prompted for your password, which you'll see every time you connect.</span></span>

    <span data-ttu-id="6e26a-125">すべてに問題がないようであれば、`exit` と入力し、SSH セッションを閉じます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-125">If everything looks fine, type `exit` to close the SSH session.</span></span>

1. <span data-ttu-id="6e26a-126">VM でポート 80 を開き、作成する新しい Web アプリケーションへの受信 HTTP トラフィックを許可します。</span><span class="sxs-lookup"><span data-stu-id="6e26a-126">Open port 80 on the VM to allow incoming HTTP traffic to the new web application that you will create.</span></span>

    ```azurecli
    az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name MeanDemo
    ```

    <span data-ttu-id="6e26a-127">このコマンドにより、作成時に "MeanDemo" と命名された HTTP ポートがご利用の VM で開きます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-127">This command will open up the HTTP port on your VM that was named "MeanDemo" when it was created.</span></span>

## <a name="summary"></a><span data-ttu-id="6e26a-128">まとめ</span><span class="sxs-lookup"><span data-stu-id="6e26a-128">Summary</span></span>

<span data-ttu-id="6e26a-129">新しい Ubuntu Linux VM の準備が完了したら、それに接続して、MEAN スタックのさまざまなコンポーネントのインストールを開始できます。</span><span class="sxs-lookup"><span data-stu-id="6e26a-129">With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.</span></span>