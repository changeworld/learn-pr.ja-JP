<span data-ttu-id="f0297-101">あなたは、勤務する会社でソフトウェア開発者としてのスキルを伸ばす機会を得て、社内の AI チームに所属することになりました。</span><span class="sxs-lookup"><span data-stu-id="f0297-101">As a software developer at your company, you've the opportunity to grow your skills and become part of the in-house AI team.</span></span> <span data-ttu-id="f0297-102">やりがいのあるこの新しい役割に取り組みながら、日常業務もこなさなければなりません。</span><span class="sxs-lookup"><span data-stu-id="f0297-102">While you ramp-up on this exciting new role, you still have your day job to do.</span></span> <span data-ttu-id="f0297-103">チーム内の先輩 AI エンジニアが、PyTorch ベースのラボを使って画像分類モデルをトレーニングする、役に立つ Jupyter ノートブックをいくつか紹介してくれました。</span><span class="sxs-lookup"><span data-stu-id="f0297-103">The senior AI engineer on your team has told you about some useful Jupyter notebooks that have PyTorch-based labs to train an image classification model.</span></span> <span data-ttu-id="f0297-104">面白そうですが、あなたはフレームワークのセットを自分の dev rig にインストールしたくないと考えています。</span><span class="sxs-lookup"><span data-stu-id="f0297-104">Exciting stuff, but you don't want to install a set of frameworks onto your dev rig.</span></span> <span data-ttu-id="f0297-105">その代わりに、Data Science Virtual Machine (DSVM) イメージをベースにした仮想マシンを作成することにしました。</span><span class="sxs-lookup"><span data-stu-id="f0297-105">Instead, you decide to create a virtual machine based on the Data Science Virtual Machine (DSVM) image.</span></span> 

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-the-azure-cli"></a><span data-ttu-id="f0297-106">Azure CLI とは</span><span class="sxs-lookup"><span data-stu-id="f0297-106">What is the Azure CLI</span></span>

<span data-ttu-id="f0297-107">Azure CLI は、Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="f0297-107">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources.</span></span> <span data-ttu-id="f0297-108">このツールは macOS、Linux、および Windows で利用でき、ブラウザーで [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を使って利用することもできます。</span><span class="sxs-lookup"><span data-stu-id="f0297-108">It's available for macOS, Linux, and Windows, or in the browser using [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).</span></span> <span data-ttu-id="f0297-109">このツールの使い方については、**CLI を使用した Azure サービスの制御**に関するモジュールで詳しく学習します。</span><span class="sxs-lookup"><span data-stu-id="f0297-109">We have complete coverage of using this tool in the **Control Azure services with the CLI** module.</span></span>

## <a name="managing-deployments"></a><span data-ttu-id="f0297-110">デプロイの管理</span><span class="sxs-lookup"><span data-stu-id="f0297-110">Managing deployments</span></span>

<span data-ttu-id="f0297-111">Azure CLI には、Azure Resource Manager のデプロイを管理するための `az group deployment` コマンドがあります。</span><span class="sxs-lookup"><span data-stu-id="f0297-111">The Azure CLI includes the `az group deployment` command to manage Azure Resource Manager deployments.</span></span> <span data-ttu-id="f0297-112">特定のタスクを実行するためのサブコマンドがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="f0297-112">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="f0297-113">最も一般的なものを次に示します。</span><span class="sxs-lookup"><span data-stu-id="f0297-113">The most common include:</span></span>

| <span data-ttu-id="f0297-114">サブコマンド</span><span class="sxs-lookup"><span data-stu-id="f0297-114">Subcommand</span></span> | <span data-ttu-id="f0297-115">説明</span><span class="sxs-lookup"><span data-stu-id="f0297-115">Description</span></span> |
|-------------|-------------|
| `create` | <span data-ttu-id="f0297-116">デプロイを開始します。</span><span class="sxs-lookup"><span data-stu-id="f0297-116">Start a deployment.</span></span> |
| `list` | <span data-ttu-id="f0297-117">リソース グループに対して行われたすべてのデプロイを取得します。</span><span class="sxs-lookup"><span data-stu-id="f0297-117">Get all the deployments for a resource group.</span></span> |
| `export` | <span data-ttu-id="f0297-118">デプロイに使用されたテンプレートをエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="f0297-118">Export the template used for a deployment.</span></span> |

<span data-ttu-id="f0297-119">使用可能なすべてのデプロイ コマンドの一覧を確認するには、[az group deployment のコマンド リファレンス](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0297-119">For a complete list of available deployment commands, see the [az group deployment command reference](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)</span></span>

<span data-ttu-id="f0297-120">ここでは、`az group deployment create` を使用して仮想マシンをプロビジョニングします。</span><span class="sxs-lookup"><span data-stu-id="f0297-120">We'll use `az group deployment create` to provision our virtual machine.</span></span>

## <a name="create-a-json-deployment-parameters-file"></a><span data-ttu-id="f0297-121">JSON デプロイ パラメーター ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="f0297-121">Create a JSON deployment parameters file</span></span>

<span data-ttu-id="f0297-122">ここでは、Azure Resource Manager テンプレートを使用して VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="f0297-122">We're going to create our VM using an Azure Resource Manager template.</span></span> <span data-ttu-id="f0297-123">テンプレートには、プロビジョニングしようとしている Linux DSVM イメージが定義されています。</span><span class="sxs-lookup"><span data-stu-id="f0297-123">The template defines the Linux DSVM image we want to provision.</span></span> <span data-ttu-id="f0297-124">使用する VM サイズ、管理者ユーザー名とパスワード、マシンのホスト名など、いくつかのパラメーターをテンプレートに指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0297-124">We need to supply some parameters to the template, such as the VM size we want to use, the admin username and password, and the host name of our machine.</span></span> 

1. <span data-ttu-id="f0297-125">このユニットの右にある Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f0297-125">Execute the following command in Azure Cloud Shell to the right of this unit:</span></span>

    ```bash
    code parameter_file.json
    ```
    <span data-ttu-id="f0297-126"><!-- TODO add a link to official doc that explains the built-in editor when it becomes available -->このコマンドにより、`parameter_file.json` という空のファイルが組み込みエディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="f0297-126"><!-- TODO add a link to official doc that explains the built-in editor when it becomes available --> This command opens and empty file called `parameter_file.json` in the built-in editor.</span></span> 

1. <span data-ttu-id="f0297-127">次の JSON スニペットをコード エディターの空のファイルに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="f0297-127">Paste the following JSON snippet into the empty file into the code editor.</span></span>

    ```json
    { 
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
         "adminUsername": { "value" : "<USERNAME>"},
         "adminPassword": { "value" : "<PASSWORD>"},
         "vmName": { "value" : "<HOSTNAME>"},
         "vmSize": { "value" : "Standard_DS2_v2"},
      }
    }
    ```

1. <span data-ttu-id="f0297-128">エディター上で、貼り付けた JSON に含まれる次のパラメーターを更新します。</span><span class="sxs-lookup"><span data-stu-id="f0297-128">Update the following parameters in the JSON you pasted in the editor:</span></span>

    |<span data-ttu-id="f0297-129">パラメーター</span><span class="sxs-lookup"><span data-stu-id="f0297-129">Parameter</span></span>  |<span data-ttu-id="f0297-130">現在の値</span><span class="sxs-lookup"><span data-stu-id="f0297-130">Current value</span></span>  |<span data-ttu-id="f0297-131">指定する値</span><span class="sxs-lookup"><span data-stu-id="f0297-131">Your value</span></span>  |
    |---------|---------|---------|
    |<span data-ttu-id="f0297-132">adminUsername</span><span class="sxs-lookup"><span data-stu-id="f0297-132">adminUsername</span></span>     |  `<USERNAME>`       |    <span data-ttu-id="f0297-133">この新しいマシンの管理者ユーザーの名前 (*azuser* など) を選択します。</span><span class="sxs-lookup"><span data-stu-id="f0297-133">Choose a name for the admin user of this new machine, such as, *azuser*.</span></span>     |
    |<span data-ttu-id="f0297-134">adminPassword</span><span class="sxs-lookup"><span data-stu-id="f0297-134">adminPassword</span></span>     |  `<PASSWORD>`       |   <span data-ttu-id="f0297-135">この管理者ユーザー アカウントのパスワードを選択します。</span><span class="sxs-lookup"><span data-stu-id="f0297-135">Choose a password for this admin user account.</span></span> <span data-ttu-id="f0297-136">パスワードの要件の詳細については、「[Linux 仮想マシンについてのよく寄せられる質問](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0297-136">To learn more about password requirements, see [Frequently asked question about Linux Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/linux/faq?azure-portal=true)</span></span>     |
    |<span data-ttu-id="f0297-137">vmName</span><span class="sxs-lookup"><span data-stu-id="f0297-137">vmName</span></span>     |   `<HOSTNAME>`      |  <span data-ttu-id="f0297-138">新しい仮想マシンの名前を選択します。</span><span class="sxs-lookup"><span data-stu-id="f0297-138">Choose a name for the new virtual machine.</span></span> <span data-ttu-id="f0297-139">名前は文字で始め、小文字と数字のみで作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0297-139">Your name must begin with a letter and contain only lowercase letters and numbers.</span></span> <span data-ttu-id="f0297-140">ご自分のイニシャルと生まれた年を含めた名前など、一意の名前を選択するようにしてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-140">Try to choose a unique name, such as one that includes your initials and your birth year.</span></span> |
    |<span data-ttu-id="f0297-141">vmSize</span><span class="sxs-lookup"><span data-stu-id="f0297-141">vmSize</span></span>     |  <span data-ttu-id="f0297-142">Standard_DS2_v2</span><span class="sxs-lookup"><span data-stu-id="f0297-142">Standard_DS2_v2</span></span>       |  <span data-ttu-id="f0297-143">この VM サイズは、この演習用としては問題ありませんが、自由に変更できます。</span><span class="sxs-lookup"><span data-stu-id="f0297-143">This VM size will work fine for this exercise, but you are free to change it.</span></span> <span data-ttu-id="f0297-144">使用可能な VM サイズの一覧は、「[Azure の Linux 仮想マシンのサイズ](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)」で確認できます。</span><span class="sxs-lookup"><span data-stu-id="f0297-144">A list of available vm sizes can be found here [Sizes for Linux virtual machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?azure-portal=true)</span></span>       |

1. <span data-ttu-id="f0297-145">変更を `parameter_file.json` に保存し、テキスト エディターを閉じます。</span><span class="sxs-lookup"><span data-stu-id="f0297-145">Save your changes in `parameter_file.json` and close the text editor.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="f0297-146">adminUsername、adminPassword、vmName 用に選択した値を覚えておいてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-146">Remember the values you chose for adminUsername, adminPassword and vmName.</span></span> <span data-ttu-id="f0297-147">この演習でもう一度使用します。</span><span class="sxs-lookup"><span data-stu-id="f0297-147">We'll use them again in this exercise.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="f0297-148">リソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="f0297-148">Create a resource group</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="f0297-149">通常は、選択したリージョン内にリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="f0297-149">Normally you'd create a resource group in a region of your choice.</span></span> <span data-ttu-id="f0297-150">ただし、今作業しているサンドボックス セッションでは、用意されているリソース グループを使用できます。</span><span class="sxs-lookup"><span data-stu-id="f0297-150">However, the sandbox session you are currently in supplies a resource group for you to use.</span></span> <span data-ttu-id="f0297-151">このセッションで使用するリソース グループは、**<rgn>[サンドボックス リソース グループ名]</rgn>** です。</span><span class="sxs-lookup"><span data-stu-id="f0297-151">Your resource group for this session is **<rgn>[Sandbox resource group name]</rgn>**.</span></span>

## <a name="deploy-the-dsvm-to-your-resource-group"></a><span data-ttu-id="f0297-152">リソース グループに DSVM をデプロイする</span><span class="sxs-lookup"><span data-stu-id="f0297-152">Deploy the DSVM to your resource group</span></span>

<span data-ttu-id="f0297-153">ここまで、リソース グループを作成し、`parameter_file.json` というファイルに DSVM Resource Manager テンプレートのパラメーターを定義しました。</span><span class="sxs-lookup"><span data-stu-id="f0297-153">We now have a resource group and have defined parameters for the DSVM Resource Manager template in a file called `parameter_file.json`.</span></span> <span data-ttu-id="f0297-154">次に、`az group deployment create` を実行して仮想マシンをプロビジョニングします。</span><span class="sxs-lookup"><span data-stu-id="f0297-154">We'll run the `az group deployment create` next to provision our virtual machine.</span></span>

1. <span data-ttu-id="f0297-155">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f0297-155">Execute the following command in Azure Cloud Shell:</span></span>

    ```azurecli
    az group deployment create \
    --resource-group  <rgn>[Sandbox resource group name]</rgn> \
    --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json \
    --parameters parameter_file.json
    ```

    <span data-ttu-id="f0297-156">このコマンドにより、Resource Manager テンプレートと指定したパラメーターを使用して、リソース グループ内に仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="f0297-156">The command uses the Resource Manager template and our parameters to create the virtual machine in our resource group.</span></span> 

2. <span data-ttu-id="f0297-157">仮想マシンのデプロイが完了するまでに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="f0297-157">Deploying a virtual machine takes a few minutes to complete.</span></span> <span data-ttu-id="f0297-158">コンソールに ` - Running ..` と表示され、操作が完了するまでそれ以外はあまり表示されません。</span><span class="sxs-lookup"><span data-stu-id="f0297-158">The console displays ` - Running ..` and not much else until the operation completes.</span></span> <span data-ttu-id="f0297-159">操作が終了すると、JSON の応答が画面に出力されます。</span><span class="sxs-lookup"><span data-stu-id="f0297-159">When the operation finishes, a JSON response is output to the screen.</span></span> <span data-ttu-id="f0297-160">JSON の一番下までスクロールし、**"provisioningState"** フィールドに "*Succeeded*" という値が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f0297-160">Scroll to the bottom of the JSON and check that the field **"provisioningState"** has the value *Succeeded*.</span></span>

    > [!TIP]
    > <span data-ttu-id="f0297-161">DNS レコードが別のパブリック IP によって既に使用されているというエラーが表示された場合は、`parameter_file.json` 内の **vmName** を一意な別の名前に変更してみてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-161">If you get an error stating that the DNS record is already used by another public IP, try changing **vmName** in `parameter_file.json` to another name that's unique.</span></span>

3. <span data-ttu-id="f0297-162">次のコマンドを実行して VM についての情報を取得します。`<HOSTNAME>` は、ご利用の VM 用に定義したホスト名に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-162">Execute the following command to get information about the VM, replacing `<HOSTNAME>` with the host name you defined for your VM.</span></span>

    ```azurecli
    az vm get-instance-view \
    --name <HOSTNAME> \
    --resource-group <rgn>[Sandbox resource group name]</rgn> \
    --query instanceView.statuses[1] \
    --output table
    ```

    <span data-ttu-id="f0297-163">このコマンドにより、VM の状態が表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0297-163">This command displays the status of the VM.</span></span> <span data-ttu-id="f0297-164">*VM running* と表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="f0297-164">It should say *VM running*.</span></span>

<span data-ttu-id="f0297-165">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="f0297-165">Congratulations!</span></span> <span data-ttu-id="f0297-166">DSVM イメージをベースにした Linux VM の作成とデプロイが完了しました。</span><span class="sxs-lookup"><span data-stu-id="f0297-166">You've created and deployed a Linux VM based on the DSVM image.</span></span>

## <a name="open-the-vm-to-ssh-traffic-on-port-22"></a><span data-ttu-id="f0297-167">VM をポート 22 で ssh トラフィックに対して開く</span><span class="sxs-lookup"><span data-stu-id="f0297-167">Open the VM to ssh traffic on port 22</span></span>

<span data-ttu-id="f0297-168">既定では、この VM はどのポートも開かれていません。</span><span class="sxs-lookup"><span data-stu-id="f0297-168">By default, our VM doesn't have any ports open.</span></span> <span data-ttu-id="f0297-169">ここでは、リモート接続を行い、Jupyter Notebook サーバーを起動して、マシン上で他のローカル コマンドを実行することが目的です。</span><span class="sxs-lookup"><span data-stu-id="f0297-169">Our goal is to connect remotely, start a Jupyter Notebook server and run other local commands on the machine.</span></span> <span data-ttu-id="f0297-170">Secure Shell (SSH) プロトコルを使用して VM にリモート接続するには、ポートを開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0297-170">To remote into the VM using the Secure Shell (SSH) protocol, we need to open a port.</span></span> <span data-ttu-id="f0297-171">ポート 22 が ssh 用の既定のポートです。</span><span class="sxs-lookup"><span data-stu-id="f0297-171">Port 22 is the default port for ssh.</span></span>  

1. <span data-ttu-id="f0297-172">Azure Cloud Shell で次のコマンドを実行します。`<HOSTNAME>` は、セットアップ時に指定した DSV 仮想マシンの名前に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-172">Execute the following command in Azure Cloud Shell, replacing `<HOSTNAME>` wit the name you gave your DSV virtual machine during setup.</span></span> 

    ```azurecli
    az vm open-port \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 22 \
    --priority 900
    ```

<span data-ttu-id="f0297-173">このコマンドは、完了するまで 1 分ほどかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="f0297-173">This command can take up to a minute to complete.</span></span> <span data-ttu-id="f0297-174">コマンドが終了すると、JSON の応答がコマンド ラインに返されます。</span><span class="sxs-lookup"><span data-stu-id="f0297-174">When the command finishes, it returns a JSON response to the command line.</span></span> <span data-ttu-id="f0297-175">**"provisioningState"** フィールドの値が *Succeeded* になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f0297-175">Check that the field **"provisioningState"** has the value *Succeeded*.</span></span> <span data-ttu-id="f0297-176">この後すぐに、ssh が動作することをテストしますが、まず 1 つ以上のポートを開きましょう。</span><span class="sxs-lookup"><span data-stu-id="f0297-176">We'll test that ssh works shortly, but first let's open one more port.</span></span>

## <a name="open-the-vm-to-access-the-jupyter-notebook-server-remotely"></a><span data-ttu-id="f0297-177">Jupyter Notebook サーバーにリモートでアクセスするように VM を開く</span><span class="sxs-lookup"><span data-stu-id="f0297-177">Open the VM to access the Jupyter Notebook server remotely</span></span>

<span data-ttu-id="f0297-178">前述のように、DSVM イメージにはソフトウェア、ツール、サンプルがあらかじめインストールされていて、データ サイエンス プロジェクト、機械学習プロジェクト、ディープ ラーニング プロジェクトに役立てることができます。</span><span class="sxs-lookup"><span data-stu-id="f0297-178">As mentioned previously, the DSVM image comes pre-installed with software, tools, and samples to help you with your data science, machine learning, and deep learning projects.</span></span> <span data-ttu-id="f0297-179">Jupyter は、サンプル ノートブックと共にイメージにインストールされています。</span><span class="sxs-lookup"><span data-stu-id="f0297-179">Jupyter is installed in the image, along with sample notebooks.</span></span> <span data-ttu-id="f0297-180">VM 上で Jupyter Notebook サーバーを起動し、その後、ローカル マシンから Notebook サーバーにリモート接続します。</span><span class="sxs-lookup"><span data-stu-id="f0297-180">We want to start a Jupyter notebook server on the VM and then remotely connect to the notebook server from our local machine.</span></span> <span data-ttu-id="f0297-181">既定では、Notebook サーバーはポート 8888 上で稼働します。</span><span class="sxs-lookup"><span data-stu-id="f0297-181">By default, the notebook server runs on port 8888.</span></span> <span data-ttu-id="f0297-182">サーバーにリモート アクセスするには、VM 上のそのポートを開く必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0297-182">To access the server remotely, we need to open that port on our VM.</span></span> 

1. <span data-ttu-id="f0297-183">Azure Cloud Shell で次のコマンドを実行します。`<HOSTNAME>` は、セットアップ時に指定したご自分の DSVM 仮想マシンの名前に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-183">Execute the following command in Azure Cloud Shell, replacing `<HOSTNAME>` with the name you gave your DSVM virtual machine during setup.</span></span>

    ```azurecli
    az vm open-port \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --port 8888 \
    --priority 901
    ```

<span data-ttu-id="f0297-184">ここでも、このコマンドは、完了するまで 1 分ほどかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="f0297-184">Again, this command can take up to a minute to complete.</span></span> <span data-ttu-id="f0297-185">コマンドが終了すると、JSON の応答がコマンド ラインに返されます。</span><span class="sxs-lookup"><span data-stu-id="f0297-185">When the command finishes, it returns a JSON response to the command line.</span></span> <span data-ttu-id="f0297-186">**"provisioningState"** フィールドの値が *Succeeded* になっていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f0297-186">Check that the field **"provisioningState"** has the value *Succeeded*.</span></span>  

## <a name="connect-to-the-vm-with-secure-shell-ssh"></a><span data-ttu-id="f0297-187">Secure Shell (ssh) を使って VM に接続する</span><span class="sxs-lookup"><span data-stu-id="f0297-187">Connect to the VM with Secure Shell (ssh)</span></span>

1. <span data-ttu-id="f0297-188">Azure Cloud Shell で次のコマンドを実行して、VM のパブリック IP アドレスを確認します。</span><span class="sxs-lookup"><span data-stu-id="f0297-188">Execute the following command in Azure Cloud Shell to find the public IP address of the VM.</span></span> <span data-ttu-id="f0297-189">`<HOSTNAME>` は、セットアップ時に指定したご自分の DSVM 仮想マシンの名前に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-189">Replace `<HOSTNAME>` with the name you gave your DSVM virtual machine during setup.</span></span>

    ```azurecli
    az vm list-ip-addresses \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -n <HOSTNAME> \
    --output table
    ```

1. <span data-ttu-id="f0297-190">Cloud Shell で次のコマンドを実行して VM にサインインします。</span><span class="sxs-lookup"><span data-stu-id="f0297-190">Execute the following command in the Cloud Shell to sign into the VM.</span></span> <span data-ttu-id="f0297-191">`<USERNAME>` は、この演習の始めに選択したユーザー名に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-191">Replace `<USERNAME>` with the username you chose at the start of this exercise.</span></span> <span data-ttu-id="f0297-192">`<IP>` は、前のステップで確認した **PublicIPAddresses** 列の値に置き換えてください。</span><span class="sxs-lookup"><span data-stu-id="f0297-192">Replace `<IP>`  with the value from the **PublicIPAddresses** column of the previous step.</span></span>

    <span data-ttu-id="f0297-193">たとえば、選択したユーザー名が *azuser* で、PublicIPAddresses の値が 33.165.103.23 であった場合、このコマンドは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f0297-193">For example, if the username you chose was *azuser* and the PublicIPAddresses had a value of 33.165.103.23, then this command would read:</span></span>
    
    `ssh azuser@33.165.103.23`
    
    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 

1. <span data-ttu-id="f0297-194">プロンプトが表示されたら、この演習の始めに選択した管理者ユーザーのパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="f0297-194">When prompted, enter the password for the admin user you chose at the start of this exercise.</span></span> <span data-ttu-id="f0297-195">正常にサインインすると、表示されるプロンプトは `username@hostname` (たとえば `azuser@js1982`) に変わります。</span><span class="sxs-lookup"><span data-stu-id="f0297-195">When you've signed in successfully, your prompt should change to the format `username@hostname`, for example, `azuser@js1982`.</span></span>

<span data-ttu-id="f0297-196">次のステップでは、VM 上で Jupyter Notebook サーバーを起動し、リモートでノートブックを開きます。</span><span class="sxs-lookup"><span data-stu-id="f0297-196">The next step is to start the Jupyter notebook server on our VM and open a notebook remotely.</span></span>

## <a name="start-the-jupyter-notebook-server-on-the-vm"></a><span data-ttu-id="f0297-197">VM 上で Jupyter Notebook サーバーを起動する</span><span class="sxs-lookup"><span data-stu-id="f0297-197">Start the Jupyter notebook server on the VM</span></span>

<span data-ttu-id="f0297-198">この VM の `~/notebooks` フォルダーに複数のノートブックがあります。</span><span class="sxs-lookup"><span data-stu-id="f0297-198">There's a set of notebooks in the `~/notebooks` folder of your VM.</span></span> <span data-ttu-id="f0297-199">SSH セッション経由でまだログインしているという前提で、Notebook サーバーを起動し、それらのノートブックのうちのどれかを開いて、すべて正常に動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f0297-199">Assuming you are still logged in through an SSH session,  start the notebook server and open one of these notebooks to make sure everything is working.</span></span>


1. <span data-ttu-id="f0297-200">この VM のコマンド プロンプトで次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="f0297-200">Run the following command at the command prompt of your VM:</span></span>

    ```bash
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root
    ```

> [!CAUTION]
> <span data-ttu-id="f0297-201">この演習では、Notebook サーバーへのアクセスを `http://` 経由で行います。</span><span class="sxs-lookup"><span data-stu-id="f0297-201">Access to the notebook server in this exercise happens over `http://`.</span></span> <span data-ttu-id="f0297-202">公の場所で Notebook サーバーを実行する場合は、セキュリティで保護する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0297-202">If you want to run a notebook server in public, you should secure it.</span></span> <span data-ttu-id="f0297-203">Notebook サーバーをセキュリティで保護する方法の詳細については、Jupyter の公式オンライン ドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f0297-203">For more information about securing a notebook server, see the official Jupyter documentation online.</span></span> 

<span data-ttu-id="f0297-204">前述のコマンドでは、`jupyter notebook` コマンドを使用して Jupyter Notebook サーバーを起動しました。</span><span class="sxs-lookup"><span data-stu-id="f0297-204">In the preceding command, we start the Jupyter Notebook server with the `jupyter notebook` command.</span></span> <span data-ttu-id="f0297-205">3 つの重要なコマンド ライン引数を指定します。</span><span class="sxs-lookup"><span data-stu-id="f0297-205">We supply three important command-line arguments.</span></span> <span data-ttu-id="f0297-206">このマシンにコンソールからリモートにログインしていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="f0297-206">Remember, we're logged into this machine remotely through a console.</span></span> <span data-ttu-id="f0297-207">ノートブックはブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="f0297-207">Notebooks are served in a browser.</span></span> 

 - <span data-ttu-id="f0297-208">`--ip=0.0.0.0` 既定では、Notebook サーバーは 127.0.0.1:8888 でローカルに稼働し、localhost からのみアクセス可能です。</span><span class="sxs-lookup"><span data-stu-id="f0297-208">`--ip=0.0.0.0` By default, a notebook server runs locally at 127.0.0.1:8888 and is accessible only from localhost.</span></span> <span data-ttu-id="f0297-209">http://127.0.0.1:8888 を使用してブラウザーから Notebook サーバーにローカルにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f0297-209">You can access the notebook server from the browser locally using http://127.0.0.1:8888.</span></span> <span data-ttu-id="f0297-210">IP アドレスを 0.0.0.0 に設定することで、サーバーはすべての IP を対象にトラフィックをリッスンします。</span><span class="sxs-lookup"><span data-stu-id="f0297-210">Setting the IP Address to 0.0.0.0 tells the server to listen for traffic on all IPs.</span></span> <span data-ttu-id="f0297-211">Notebook サーバーが 0.0.0.0 でリッスンしている場合は、ホスト マシンの IP アドレスを介してアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="f0297-211">If the notebook server listens on 0.0.0.0, it will be reachable through the IP address of the host machine.</span></span>  
 - <span data-ttu-id="f0297-212">`--no-browser`  別のコンピューターからインターネット経由でノートブックに接続したいので、ブラウザーを開かないように Notebook サーバーを構成します。ブラウザーを開くのは既定の動作です。</span><span class="sxs-lookup"><span data-stu-id="f0297-212">`--no-browser`  Because we want to connect to the notebook from another computer over the internet, we configure the notebook server to not open the browser, which is the default behavior.</span></span> 
 - <span data-ttu-id="f0297-213">`--allow-root`  この演習では、VM 上の管理者アカウントしかないので、root としてノートブックを実行できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="f0297-213">`--allow-root`  In this exercise, we only have an admin account on the VM, so  we want to be able to run notebooks as root.</span></span>

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a><span data-ttu-id="f0297-214">リモート ブラウザーから Jupyter Notebook サーバーに接続する</span><span class="sxs-lookup"><span data-stu-id="f0297-214">Connect to the Jupyter Notebook server from a remote browser</span></span>

<span data-ttu-id="f0297-215">上記のコマンドが VM 上で実行されると、Notebook サーバーが起動し、コンソールに URL が表示されます。その URL をブラウザーで使用できます。</span><span class="sxs-lookup"><span data-stu-id="f0297-215">When the above command runs on the VM, the notebook server starts and the console displays a URL for you to use in a browser.</span></span> 

![コンソールに表示された、実行中の Notebook サーバーのメッセージのスクリーンショット。ホスト マシンからサーバーにアクセスするための URL が示されています](../media/notebook-url.png)

1. <span data-ttu-id="f0297-217">Notebook サーバーから返された URL を好きなブラウザーのアドレス バーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="f0297-217">Copy the URL the notebook server displays into the address bar of your favorite browser.</span></span> <span data-ttu-id="f0297-218">URL をクリックして既定のブラウザーで開くこともできます。</span><span class="sxs-lookup"><span data-stu-id="f0297-218">You can also click on the URL to open in your default browser.</span></span> 

    <span data-ttu-id="f0297-219">"サイトにアクセスできません" という旨のメッセージが表示されます。返された URL はホスト マシンから Notebook サーバーへの接続 URL であるためです。</span><span class="sxs-lookup"><span data-stu-id="f0297-219">You will receive a "Site can't be reached" message because the URL you were given is the connection to the notebook server from the host machine.</span></span>

1. <span data-ttu-id="f0297-220">サーバーにリモートでアクセスするには、URL のホスト名を、以前のステップで保存した VM の IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f0297-220">To access the server remotely,  replace the hostname in the URL with the IP address of the VM you saved earlier.</span></span> 

    <span data-ttu-id="f0297-221">サンプルの Notebook サーバーの URL を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f0297-221">Here's a sample notebook server URL.</span></span>

    <span data-ttu-id="f0297-222">"http://**ab-dsvm-4**:8888/?token={some token}"</span><span class="sxs-lookup"><span data-stu-id="f0297-222">"http://**ab-dsvm-4**:8888/?token={some token}"</span></span>

    <span data-ttu-id="f0297-223">この場合、**ab-dsvm-4** をマシンの IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="f0297-223">In this case, we would replace **ab-dsvm-4** with IP address of the machine.</span></span> <span data-ttu-id="f0297-224">IP アドレスが `52.175.199.43` であれば、URL は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="f0297-224">If our IP address is `52.175.199.43`, then the URL becomes:</span></span>

    <span data-ttu-id="f0297-225">"http://**52.175.199.43**:8888/?token={some token}"</span><span class="sxs-lookup"><span data-stu-id="f0297-225">"http://**52.175.199.43**:8888/?token={some token}"</span></span>

    <span data-ttu-id="f0297-226">ポート アドレス `:8888` が URL に使用されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="f0297-226">Make sure `:8888`, the port address, is kept in the URL.</span></span>

    > [!TIP]
    > <span data-ttu-id="f0297-227">IP アドレスを使用したくない場合は、サーバーの完全修飾名を使用することもできます。形式は、`<HOST NAME>.<REGION>.cloudapp.azure.com` です。</span><span class="sxs-lookup"><span data-stu-id="f0297-227">If you don't want to use the IP address, you can also use the fully qualified name of your server which is in the form `<HOST NAME>.<REGION>.cloudapp.azure.com`</span></span>

    <span data-ttu-id="f0297-228">次のスクリーンショットは、Jupyter ダッシュボードがブラウザーにどのように表示されるかを示しています。</span><span class="sxs-lookup"><span data-stu-id="f0297-228">The following  screenshot shows what the Jupyter dashboard looks like in your browser.</span></span>

    ![<span data-ttu-id="f0297-229">Jupyter Notebook のダッシュボードのスクリーンショット。</span><span class="sxs-lookup"><span data-stu-id="f0297-229">Screenshot showing Jupyter Notebooks dashboard.</span></span> ](../media/jupyter-in-browser.png)

1. <span data-ttu-id="f0297-230">**notebooks/IntroToJupyterPython.ipynb** に移動し、それを選択します。</span><span class="sxs-lookup"><span data-stu-id="f0297-230">Navigate to **notebooks/IntroToJupyterPython.ipynb** and select it.</span></span> <span data-ttu-id="f0297-231">このノートブックを使ってみて、すべてが予想どおりに動作することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="f0297-231">Try out this notebook to verify everythin  works as expected.</span></span>

    <span data-ttu-id="f0297-232">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="f0297-232">Congratulations!</span></span> <span data-ttu-id="f0297-233">DSVM ベースの稼働する仮想マシンが実行されており、Jupyter をリモートで使用できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="f0297-233">You now have a running DSVM-based virtual machine running and can work remotely with Jupyter.</span></span> <span data-ttu-id="f0297-234">この演習では、VM 上にインストールされたソフトウェアを実行しています。</span><span class="sxs-lookup"><span data-stu-id="f0297-234">In this exercise, we're running the software that was installed on the VM.</span></span> <span data-ttu-id="f0297-235">次の演習では、安心して実験できるようにソフトウェアを VM 上のコンテナーに分離します。</span><span class="sxs-lookup"><span data-stu-id="f0297-235">In the next exercise, we'll isolate the software in a container on the VM so we can experiment with confidence.</span></span>

4. <span data-ttu-id="f0297-236">ノートブックを使用し終わったら、Cloud Shell で `Control-C` を使用して Jupyter サーバーを停止できます。</span><span class="sxs-lookup"><span data-stu-id="f0297-236">When you have finished with the notebook, you can stop the Jupyter server with `Control-C` in the Cloud Shell.</span></span>
