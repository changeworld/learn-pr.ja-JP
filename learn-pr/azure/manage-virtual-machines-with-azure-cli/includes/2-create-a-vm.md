<span data-ttu-id="5978f-101">最もわかりやすいタスクから始めましょう: Azure 仮想マシンの作成。</span><span class="sxs-lookup"><span data-stu-id="5978f-101">Let's start with the most obvious task: creating an Azure Virtual Machine.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="logins-subscriptions-and-resource-groups"></a><span data-ttu-id="5978f-102">ログイン、サブスクリプション、リソース グループ</span><span class="sxs-lookup"><span data-stu-id="5978f-102">Logins, subscriptions, and resource groups</span></span>

<span data-ttu-id="5978f-103">この後の作業には右側の Azure Cloud Shell を使用します。</span><span class="sxs-lookup"><span data-stu-id="5978f-103">You'll be working in the Azure Cloud Shell on the right.</span></span> <span data-ttu-id="5978f-104">サンドボックスをアクティブにした後、Microsoft Learn によって管理されている無料のサブスクリプションで Azure にログインします。</span><span class="sxs-lookup"><span data-stu-id="5978f-104">Once you activate the sandbox, you'll be logged into Azure with a free subscription managed by Microsoft Learn.</span></span> <span data-ttu-id="5978f-105">自分で Azure にログインしたり、サブスクリプションを選択したりする必要はありません。これらは自動的に行われます。</span><span class="sxs-lookup"><span data-stu-id="5978f-105">You don't have to log into Azure on your own, or select a subscription - this will be done for you.</span></span> <span data-ttu-id="5978f-106">さらに、通常であれば、新しいリソースを保持するための "_リソース グループ_" を作成します。</span><span class="sxs-lookup"><span data-stu-id="5978f-106">In addition, normally you would create a _resource group_ to hold new resources.</span></span> <span data-ttu-id="5978f-107">このモジュールでは、すべてのコマンドの実行に使用されるリソース グループが Azure サンドボックスによって自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="5978f-107">In this module, the Azure sandbox will create a resource group for you which will be used to execute all the commands.</span></span>

## <a name="create-a-linux-vm-with-the-azure-cli"></a><span data-ttu-id="5978f-108">Azure CLI を使用して Linux VM を作成する</span><span class="sxs-lookup"><span data-stu-id="5978f-108">Create a Linux VM with the Azure CLI</span></span>

<span data-ttu-id="5978f-109">Azure CLI には、Azure 内の仮想マシンを操作するための `vm` コマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5978f-109">The Azure CLI includes the `vm` command to work with virtual machines in Azure.</span></span> <span data-ttu-id="5978f-110">特定のタスクを実行するためのサブコマンドがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="5978f-110">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="5978f-111">最も一般的なものを次に示します。</span><span class="sxs-lookup"><span data-stu-id="5978f-111">The most common include:</span></span>

| <span data-ttu-id="5978f-112">サブコマンド</span><span class="sxs-lookup"><span data-stu-id="5978f-112">Sub-command</span></span> | <span data-ttu-id="5978f-113">説明</span><span class="sxs-lookup"><span data-stu-id="5978f-113">Description</span></span> |
|-------------|-------------|
| `create`    | <span data-ttu-id="5978f-114">新しい仮想マシンを作成します</span><span class="sxs-lookup"><span data-stu-id="5978f-114">Create a new virtual machine</span></span> |
| `deallocate` | <span data-ttu-id="5978f-115">仮想マシンの割り当てを解除します</span><span class="sxs-lookup"><span data-stu-id="5978f-115">Deallocate a virtual machine</span></span> |
| `delete` | <span data-ttu-id="5978f-116">仮想マシンの削除</span><span class="sxs-lookup"><span data-stu-id="5978f-116">Delete a virtual machine</span></span> |
| `list` | <span data-ttu-id="5978f-117">サブスクリプションに作成されている仮想マシンの一覧を表示します</span><span class="sxs-lookup"><span data-stu-id="5978f-117">List the created virtual machines in your subscription</span></span> |
| `open-port` | <span data-ttu-id="5978f-118">受信トラフィック用に特定のネットワーク ポートを開きます</span><span class="sxs-lookup"><span data-stu-id="5978f-118">Open a specific network port for inbound traffic</span></span> |
| `restart` | <span data-ttu-id="5978f-119">仮想マシンの再起動</span><span class="sxs-lookup"><span data-stu-id="5978f-119">Restart a virtual machine</span></span> |
| `show` | <span data-ttu-id="5978f-120">仮想マシンについての詳細を取得します</span><span class="sxs-lookup"><span data-stu-id="5978f-120">Get the details for a virtual machine</span></span> |
| `start` | <span data-ttu-id="5978f-121">停止している仮想マシンを起動します</span><span class="sxs-lookup"><span data-stu-id="5978f-121">Start a stopped virtual machine</span></span> |
| `stop` | <span data-ttu-id="5978f-122">実行している仮想マシンを停止します</span><span class="sxs-lookup"><span data-stu-id="5978f-122">Stop a running virtual machine</span></span> |
| `update` | <span data-ttu-id="5978f-123">仮想マシンのプロパティを更新します</span><span class="sxs-lookup"><span data-stu-id="5978f-123">Update a property of a virtual machine</span></span> |

> [!NOTE]
> <span data-ttu-id="5978f-124">コマンドの完全な一覧については、[Azure CLI のリファレンス ドキュメント](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="5978f-124">For a complete list of commands, you can check the [Azure CLI reference documentation](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span></span>

<span data-ttu-id="5978f-125">最初の `az vm create` から始めましょう。</span><span class="sxs-lookup"><span data-stu-id="5978f-125">Let's start with the first one: `az vm create`.</span></span> <span data-ttu-id="5978f-126">このコマンドは、リソース グループ内に仮想マシンを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="5978f-126">This command is used to create a virtual machine in a resource group.</span></span> <span data-ttu-id="5978f-127">複数のパラメーターを渡して、新しい VM のすべての面を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="5978f-127">There are several parameters you can pass to configure all the aspects of the new VM.</span></span> <span data-ttu-id="5978f-128">次の 3 つのパラメーターを必ず指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5978f-128">The three parameters that must be supplied are:</span></span>

> [!div class="mx-tableFixed"]
> | <span data-ttu-id="5978f-129">パラメーター</span><span class="sxs-lookup"><span data-stu-id="5978f-129">Parameter</span></span> | <span data-ttu-id="5978f-130">説明</span><span class="sxs-lookup"><span data-stu-id="5978f-130">Description</span></span> |
> |-----------|-------------|
> | `resource-group` | <span data-ttu-id="5978f-131">仮想マシンを所有するリソース グループです。**<rgn>[サンドボックス リソース グループ]</rgn>** を使用します。</span><span class="sxs-lookup"><span data-stu-id="5978f-131">The resource group that will own the virtual machine, use **<rgn>[sandbox Resource Group]</rgn>**.</span></span> |
> | `name` | <span data-ttu-id="5978f-132">仮想マシンの名前であり、リソース グループ内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="5978f-132">The name of the virtual machine - must be unique within the resource group.</span></span> |
> | `image` | <span data-ttu-id="5978f-133">VM の作成に使用するオペレーティング システム イメージです。</span><span class="sxs-lookup"><span data-stu-id="5978f-133">The operating system image to use to create the VM.</span></span> |
> | `location` | <span data-ttu-id="5978f-134">VM を配置するリージョンです。</span><span class="sxs-lookup"><span data-stu-id="5978f-134">The region to place the VM in.</span></span> <span data-ttu-id="5978f-135">通常は、VM のコンシューマーの近くになります。</span><span class="sxs-lookup"><span data-stu-id="5978f-135">Typically this would be close to the consumer of the VM.</span></span> <span data-ttu-id="5978f-136">この演習では、次の一覧から近くの場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="5978f-136">In this exercise, choose a location nearby from the following list.</span></span> |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

<span data-ttu-id="5978f-137">さらに、`--verbose` フラグを追加して VM の作成の進行状況を表示すると便利です。</span><span class="sxs-lookup"><span data-stu-id="5978f-137">In addition, it's helpful to add the `--verbose` flag to see progress while the VM is being created.</span></span> 

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="5978f-138">Linux 仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="5978f-138">Create a Linux virtual machine</span></span>

<span data-ttu-id="5978f-139">新しい Linux 仮想マシンを作成してみます。</span><span class="sxs-lookup"><span data-stu-id="5978f-139">Let's create a new Linux virtual machine.</span></span> <span data-ttu-id="5978f-140">"米国西部" の場所に Debian Linux マシンを作成するには、Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5978f-140">Execute the following command in Azure Cloud Shell to create a Debian Linux machine in the "West US" location.</span></span> <span data-ttu-id="5978f-141">米国西部が近くではない場合は、場所を変更してください。</span><span class="sxs-lookup"><span data-stu-id="5978f-141">Change the location if that one isn't nearby.</span></span>

```azurecli
az vm create --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --location westus --verbose 
```

[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


<span data-ttu-id="5978f-142">このコマンドでは、新しい **Debian** Linux 仮想マシンが `SampleVM` という名前で作成されます。</span><span class="sxs-lookup"><span data-stu-id="5978f-142">This command will create a new **Debian** Linux virtual machine with the name `SampleVM`.</span></span> <span data-ttu-id="5978f-143">VM が作成されている間、Azure CLI ツールは待っていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="5978f-143">Notice that the Azure CLI tool waits while the VM is being created.</span></span> <span data-ttu-id="5978f-144">`--no-wait` オプションを追加することで、Azure CLI ツールに対して、すぐに戻り、VM の作成を Azure にバックグラウンドで続けさせるように指示できます。</span><span class="sxs-lookup"><span data-stu-id="5978f-144">You can add the `--no-wait` option to tell the Azure CLI tool to return immediately and have Azure continue creating the VM in the background.</span></span> <span data-ttu-id="5978f-145">これは、スクリプトでコマンドを実行している場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="5978f-145">This is useful if you're executing the command in a script.</span></span> <span data-ttu-id="5978f-146">後でスクリプトの中で `azure vm wait --name [vm-name]` コマンドを使用して、VM の作成が完了するのを待機します。</span><span class="sxs-lookup"><span data-stu-id="5978f-146">Later in the script, use the `azure vm wait --name [vm-name]` command to wait for the VM to finish being created.</span></span>

<span data-ttu-id="5978f-147">詳細な応答を見ると、VM のさまざまな依存関係の名前に `SampleVM` という名前が使用されていることもわかります。</span><span class="sxs-lookup"><span data-stu-id="5978f-147">If you look at the verbose responses, you will also see that the `SampleVM` name is used to name various dependencies for the VM.</span></span>

```output
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

<span data-ttu-id="5978f-148">これらの自動生成されるリソースの名前は、省略可能なパラメーター `vm create` を使用してオーバーライドできます (`--vnet-name` や `--public-ip-address-dns-name` など)。</span><span class="sxs-lookup"><span data-stu-id="5978f-148">You can override these auto-generated resource names using optional parameters to `vm create`, such as `--vnet-name` and `--public-ip-address-dns-name`.</span></span>

<span data-ttu-id="5978f-149">`admin-username` フラグを使用して管理者アカウント名を **"aldis"** と指定しています。</span><span class="sxs-lookup"><span data-stu-id="5978f-149">We are specifying the administrator account name through the `admin-username` flag to be **"aldis"**.</span></span> <span data-ttu-id="5978f-150">これを省略した場合、`vm create` コマンドは "_現在のユーザー名_" を使用します。</span><span class="sxs-lookup"><span data-stu-id="5978f-150">If you omit this, the `vm create` command will use your _current user name_.</span></span> <span data-ttu-id="5978f-151">アカウントの命名規則は OS によって異なるため、特定の名前を指定する方が安全です。</span><span class="sxs-lookup"><span data-stu-id="5978f-151">Since the rules for account names are different for each OS, it's safer to specify a specific name.</span></span> 

> [!NOTE]
> <span data-ttu-id="5978f-152">"root" や "admin" などの一般的な名前は、ほとんどのイメージで使用できません。</span><span class="sxs-lookup"><span data-stu-id="5978f-152">Common names such as "root" and "admin" are not allowed for most images.</span></span>

<span data-ttu-id="5978f-153">ここでは、`generate-ssh-keys` フラグも使用しています。</span><span class="sxs-lookup"><span data-stu-id="5978f-153">We are also using the `generate-ssh-keys` flag.</span></span> <span data-ttu-id="5978f-154">このパラメーターは Linux ディストリビューションに対して使用され、`ssh` ツールを使用して仮想マシンにリモート アクセスできるように、セキュリティ キーのペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5978f-154">This parameter is used for Linux distributions and creates a pair of security keys so we can use the `ssh` tool to access the virtual machine remotely.</span></span> <span data-ttu-id="5978f-155">コンピューター上と VM 内の `.ssh` フォルダーに、2 つのファイルが配置されます。</span><span class="sxs-lookup"><span data-stu-id="5978f-155">The two files are placed into the `.ssh` folder on your machine and in the VM.</span></span> <span data-ttu-id="5978f-156">ターゲット フォルダーに `id_rsa` という名前の SSH キーが既にある場合はそれが使用され、新しいキーは生成されません。</span><span class="sxs-lookup"><span data-stu-id="5978f-156">If you already have an SSH key named `id_rsa` in the target folder, then it will be used rather than having a new key generated.</span></span>

<span data-ttu-id="5978f-157">VM の作成が終了すると、JSON 応答を受け取ります。それには、仮想マシンの現在の状態と、Azure によって割り当てられたパブリックおよびプライベート IP アドレスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5978f-157">Once it finishes creating the VM, you will get a JSON response which includes the current state of the virtual machine and its public and private IP addresses assigned by Azure:</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "2568d0d0-efe3-4d04-a08f-df7f009f822a",
  "zones": ""
}
```
