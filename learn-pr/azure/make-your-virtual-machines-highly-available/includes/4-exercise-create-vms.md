<span data-ttu-id="9ef76-101">受信インターネット トラフィックをルーティングするためのロード バランサーの準備ができました。ここで、トラフィックをルーティングするための Azure VM がいくつか必要になります。</span><span class="sxs-lookup"><span data-stu-id="9ef76-101">We have a load balancer to take inbound Internet traffic and route it, now we need some Azure VMs to route traffic to.</span></span> <span data-ttu-id="9ef76-102">この例では Linux を使用しますが、どの仮想マシンのイメージでもまったく同じ手法を適用します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-102">We'll use Linux for this example, but the exact same technique would work with any virtual machine image.</span></span>

## <a name="create-a-set-of-vms"></a><span data-ttu-id="9ef76-103">一連の VM を作成する</span><span class="sxs-lookup"><span data-stu-id="9ef76-103">Create a set of VMs</span></span>

<span data-ttu-id="9ef76-104">3 つの VM を作成し、Azure CLI を使用してそれらを可用性セットにグループ化します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-104">We're going to create three VMs and group them into an availability set with the Azure CLI.</span></span> <span data-ttu-id="9ef76-105">このタスクではポータルを使用できましたが、ここでは複数のリソースを作成するため、スクリプト ツールを使用して実行する方がはるかに簡単です。</span><span class="sxs-lookup"><span data-stu-id="9ef76-105">We could have used the portal for this task - but because we're creating multiple resources, it's much easier to do this through a scripting tool.</span></span>

<span data-ttu-id="9ef76-106">_本当に_ Azure portal を使用したい場合の代替方法は、_テンプレートを_利用することです。</span><span class="sxs-lookup"><span data-stu-id="9ef76-106">An alternative approach if you _really_ prefer to use the Azure portal would be to use a _template_.</span></span> <span data-ttu-id="9ef76-107">これらは JSON の手順であり、リソースをデプロイするために使用できます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-107">These are JSON instructions which can be used to deploy resources.</span></span> <span data-ttu-id="9ef76-108">仮想マシン用のテンプレートを定義し、Azure portal で複数回テンプレートをデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-108">We could define a template for the virtual machine and then deploy the template multiple times in the Azure portal.</span></span>

### <a name="create-some-azure-cli-defaults"></a><span data-ttu-id="9ef76-109">Azure CLI の既定値をいくつか作成する</span><span class="sxs-lookup"><span data-stu-id="9ef76-109">Create some Azure CLI defaults</span></span>

1. <span data-ttu-id="9ef76-110">まず、Azure CLI で既定値をいくつか設定します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-110">Start by setting some defaults in the Azure CLI.</span></span> <span data-ttu-id="9ef76-111">使用するすべてのコマンドには、_リソース グループ_と省略可能な_場所_が必要です。</span><span class="sxs-lookup"><span data-stu-id="9ef76-111">Every command we use requires a _resource group_ and an optional _location_.</span></span> <span data-ttu-id="9ef76-112">毎回入力するのではなく、既定のパラメーターとして設定できます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-112">Rather than typing that every time, we can set it as a default parameter.</span></span> <span data-ttu-id="9ef76-113">事前に作成した Azure のサンドボックス リソース グループと、Azure Load Balancer に対して選択した同じ場所を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-113">Use the pre-created Azure sandbox resource group, and the same location you selected for the Azure Load Balancer.</span></span> <span data-ttu-id="9ef76-114">繰り返しますが、利用可能な場所は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="9ef76-114">As a reminder, here are the available locations:</span></span>

    [!include[](../../../includes/azure-sandbox-regions-note.md)]

1. <span data-ttu-id="9ef76-115">右側にある Cloud Shell にこのコマンドを入力し、`<location>` プレースホルダーを、Azure Load Balancer で使用した場所に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-115">Type this command into the Cloud Shell on the right, replacing the `<location>` placeholder with the location you used for your Azure Load Balancer.</span></span>

    ```azurecli
    az configure --defaults group=<rgn>[sandbox Resource Group</rgn> location=<location>
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

### <a name="create-an-availability-set"></a><span data-ttu-id="9ef76-116">可用性セットを作成する</span><span class="sxs-lookup"><span data-stu-id="9ef76-116">Create an availability set</span></span>

<span data-ttu-id="9ef76-117">最初に、_可用性セット_を作成します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-117">Let's start by creating an _availability set_.</span></span>

<span data-ttu-id="9ef76-118">仮想マシンをグループ化するための可用性セットが必要になります。</span><span class="sxs-lookup"><span data-stu-id="9ef76-118">We will need an availability set to group our virtual machines into.</span></span> <span data-ttu-id="9ef76-119">これは `vm availability-set` コマンドを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-119">We can use the `vm availability-set` command to create one.</span></span> <span data-ttu-id="9ef76-120">必要なのは名前だけです。ここでは **woodgrove-avail-set** を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-120">It just needs a name, we'll use **woodgrove-avail-set**.</span></span>

```azurecli
az vm availability-set create --name woodgrove-avail-set
```

### <a name="create-a-configuration-script"></a><span data-ttu-id="9ef76-121">構成スクリプトを作成する</span><span class="sxs-lookup"><span data-stu-id="9ef76-121">Create a configuration script</span></span>

<span data-ttu-id="9ef76-122">VM にはそれぞれ同じ基本構成を使用します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-122">We want to use the same basic configuration for each of the VMs.</span></span>

    - <span data-ttu-id="9ef76-123">Ubuntu Linux</span><span class="sxs-lookup"><span data-stu-id="9ef76-123">Ubuntu Linux</span></span>
    - <span data-ttu-id="9ef76-124">Nginx Web サーバー</span><span class="sxs-lookup"><span data-stu-id="9ef76-124">nginx web server</span></span>
    - <span data-ttu-id="9ef76-125">Node.js</span><span class="sxs-lookup"><span data-stu-id="9ef76-125">Node.js</span></span>
    - <span data-ttu-id="9ef76-126">テスト用の単一ページを含む基本的な Web サイト</span><span class="sxs-lookup"><span data-stu-id="9ef76-126">Basic website with a single page for testing</span></span>

<span data-ttu-id="9ef76-127">OS は選択するイメージに基づいて選びますが、他の構成要素ではスクリプトがいくつか必要になります。</span><span class="sxs-lookup"><span data-stu-id="9ef76-127">The choice of OS is based on the image we select, but the other configuration elements require some scripting.</span></span> <span data-ttu-id="9ef76-128">Cloud Shell でローカル ファイルを作成して、Web サーバーをインストールし、基本的なサイトで構成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9ef76-128">Let's create a local file in the Cloud Shell to install a web server and configure it with a basic site.</span></span>

1. <span data-ttu-id="9ef76-129">ターミナルで「`code`」と入力して、Cloud Shell エディターを開きます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-129">Open the Cloud Shell Editor by typing `code` in the terminal.</span></span>

    ```bash
    code
    ```

    <span data-ttu-id="9ef76-130">これは組み込みエディターです。これを使用して、Cloud Shell 環境ですぐにファイルを作成して編集することができます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-130">This is a built-in editor you can use to create and edit files right in the Cloud Shell environment.</span></span> <span data-ttu-id="9ef76-131">ファイルは、Cloud Shell 環境を起動したときに作成される、Azure の Storage アカウントに保存されます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-131">The files are saved in Azure in a Storage account created when you launched the Cloud Shell environment.</span></span>

1. <span data-ttu-id="9ef76-132">次のスクリプトをコピーし、エディター ウィンドウに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-132">Copy the following script and paste it into the editor window.</span></span>

    ```script
    #cloud-config
    package_upgrade: true
    packages:
      - nginx
      - nodejs
      - npm
    write_files:
      - owner: www-data:www-data
      - path: /etc/nginx/sites-available/default
        content: |
          server {
            listen 80;
            location / {
              proxy_pass http://localhost:3000;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection keep-alive;
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
            }
          }
      - owner: azureuser:azureuser
      - path: /home/azureuser/myapp/index.js
        content: |
          var express = require('express')
          var app = express()
          var os = require('os');
          app.get('/', function (req, res) {
            res.send('Hello World from host ' + os.hostname() + '!')
          })
          app.listen(3000, function () {
            console.log('Hello world app listening on port 3000!')
          })
    runcmd:
      - service nginx restart
      - cd "/home/azureuser/myapp"
      - npm init
      - npm install express -y
      - nodejs index.js
    ```

1. <span data-ttu-id="9ef76-133">ファイルを保存します。ファイルには **cloud-init.txt** という名前を付けます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-133">Save the file - give it the name **cloud-init.txt**.</span></span> <span data-ttu-id="9ef76-134">エディターの右上隅にある [...] コンテキスト メニューを使用できます。また、Windows と Linux の場合は <kbd>Ctrl + S</kbd> キー、macOS の場合は <kbd>Cmd + S</kbd> キーを使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-134">You can use the "..." context menu on the top/right corner of the editor, or use <kbd>Ctrl+S</kbd> in Windows and Linux, and <kbd>Cmd+S</kbd> on macOS.</span></span>

1. <span data-ttu-id="9ef76-135">エディターを終了します。ここでも [...] コンテキスト メニューを使用できます。また、アクセラレータ キー (<kbd>Ctrl + Q</kbd>) を使用することもできます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-135">Exit the editor - again you can use the "..." context menu, or an accelerator key (<kbd>Ctrl+Q</kbd>).</span></span>

1. <span data-ttu-id="9ef76-136">ファイルはファイル システム上に存在する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9ef76-136">The file should be on the file system.</span></span> <span data-ttu-id="9ef76-137">`ls` コマンドを使用して、フォルダーの内容をリストしてみます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-137">Try using the `ls` command to list the contents of the folder.</span></span> <span data-ttu-id="9ef76-138">ファイルに対して `cat` を実行して、内容を確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-138">You can also `cat` the file to verify the contents.</span></span>

### <a name="create-the-virtual-machines"></a><span data-ttu-id="9ef76-139">仮想マシンを作成する</span><span class="sxs-lookup"><span data-stu-id="9ef76-139">Create the virtual machines</span></span>

<span data-ttu-id="9ef76-140">次は、Azure CLI を使用して 3 つの Ubuntu Linux 仮想マシンを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="9ef76-140">Next, let's create three Ubuntu Linux virtual machines with the Azure CLI.</span></span> <span data-ttu-id="9ef76-141">ここではオプションについて説明しませんが、CLI での VM 管理の詳細に関心をお持ちの場合は、「**Azure CLI を使用して仮想マシンを管理する**」モジュールをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9ef76-141">We won't cover the options here, but if you are interested in learning more about VM management with the CLI, check out the **Manage virtual machines with the Azure CLI** module.</span></span>

<span data-ttu-id="9ef76-142">各 VM の仮想ネットワーク インターフェイスを作成する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="9ef76-142">We also need to create a virtual network interface for each VM.</span></span> <span data-ttu-id="9ef76-143">ループを使用して、それらをまとめて作成することができます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-143">We can use a loop and create them together.</span></span>

1. <span data-ttu-id="9ef76-144">`vm-create` コマンドを使用して、ループ内に 3 つの仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-144">Use the `vm-create` command to create three virtual machines in a loop.</span></span> <span data-ttu-id="9ef76-145">以下のコードを使用して、次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-145">You can use the following code to:</span></span>
    - <span data-ttu-id="9ef76-146">_woodgrove_NicX_ という名前の NIC を作成します。ここで、X は [1,2,3] となります。</span><span class="sxs-lookup"><span data-stu-id="9ef76-146">Create a NIC named _woodgrove_NicX_ where X is [1,2,3].</span></span>
    - <span data-ttu-id="9ef76-147">_woodgrove-VMX_ という名前の VM を作成します。ここで、X は [1,2,3] となります。</span><span class="sxs-lookup"><span data-stu-id="9ef76-147">Create a VM named _woodgrove-VMX_ where X is [1,2,3].</span></span>
    - <span data-ttu-id="9ef76-148">作成された VNET に各 NIC を関連付ける</span><span class="sxs-lookup"><span data-stu-id="9ef76-148">Associate each NIC to the created VNET</span></span>
    - <span data-ttu-id="9ef76-149">各 VM を NIC と可用性セットに関連付けます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-149">Associate each VM to the NIC and availability set.</span></span>

    ```azurecli
    for i in `seq 1 3`; do

        az network nic create --name woodgrove-Nic$i \
            --vnet-name woodgrove-VNET \
            --subnet backendSubnet

        az vm create --name woodgrove-VM$i \
            --availability-set woodgrove-avail-set \
            --nics woodgrove-Nic$i \
            --image UbuntuLTS \
            --admin-username azureuser \
            --generate-ssh-keys \
            --custom-data cloud-init.txt \
            --no-wait
    done
    ```
    > [!TIP]
    > <span data-ttu-id="9ef76-150">ここでは、コマンドによって管理者名が "azureuser" に設定されます。これを任意の名前に変更することができます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-150">The command is setting the administrator name to "azureuser" here, you can change that to anything you like.</span></span> <span data-ttu-id="9ef76-151">SSH キーよりユーザー名/パスワードの方がよい場合は、パスワードを指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-151">You can also supply a password if you prefer username/password over SSH keys.</span></span>

1. <span data-ttu-id="9ef76-152">この実行にはしばらく時間がかかります。ループが完了しても、VM は完全には使用できません。</span><span class="sxs-lookup"><span data-stu-id="9ef76-152">This will take a while to run - and even when the loop is finished, the VMs won't quite be available.</span></span> <span data-ttu-id="9ef76-153">Azure portal に切り替え、**[すべてのリソース]** 表示を使用して、作成されたリソースを確認することができます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-153">You can switch over to the Azure portal and use the **All Resources** display to see the created resources.</span></span>

1. <span data-ttu-id="9ef76-154">また、`vm list` コマンドを使用して、デプロイされた VM の数を確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="9ef76-154">You can also use the `vm list` command to see how many VMs have been deployed.</span></span>

    ```azurecli
    az vm list -o table
    ```

<span data-ttu-id="9ef76-155">次は、ロード バランサーの構成方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9ef76-155">Next we'll talk about how to configure the load balancer.</span></span>