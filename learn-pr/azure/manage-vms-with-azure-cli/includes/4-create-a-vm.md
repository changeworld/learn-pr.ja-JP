<span data-ttu-id="6087a-101">Azure CLI には、Azure 内の仮想マシンを操作するための `vm` コマンドが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6087a-101">The Azure CLI includes the `vm` command to work with virtual machines in Azure.</span></span> <span data-ttu-id="6087a-102">特定のタスクを実行するためのサブコマンドがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="6087a-102">We can supply several subcommands to do specific tasks.</span></span> <span data-ttu-id="6087a-103">最も一般的なものを次に示します。</span><span class="sxs-lookup"><span data-stu-id="6087a-103">The most common include:</span></span>

| <span data-ttu-id="6087a-104">サブコマンド</span><span class="sxs-lookup"><span data-stu-id="6087a-104">Sub-command</span></span> | <span data-ttu-id="6087a-105">説明</span><span class="sxs-lookup"><span data-stu-id="6087a-105">Description</span></span> |
|-------------|-------------|
| `create`    | <span data-ttu-id="6087a-106">新しい仮想マシンを作成します</span><span class="sxs-lookup"><span data-stu-id="6087a-106">Create a new virtual machine</span></span> |
| `deallocate` | <span data-ttu-id="6087a-107">仮想マシンの割り当てを解除します</span><span class="sxs-lookup"><span data-stu-id="6087a-107">Deallocate a virtual machine</span></span> |
| `delete` | <span data-ttu-id="6087a-108">仮想マシンの削除</span><span class="sxs-lookup"><span data-stu-id="6087a-108">Delete a virtual machine</span></span> |
| `list` | <span data-ttu-id="6087a-109">サブスクリプションに作成されている仮想マシンの一覧を表示します</span><span class="sxs-lookup"><span data-stu-id="6087a-109">List the created virtual machines in your subscription</span></span> |
| `open-port` | <span data-ttu-id="6087a-110">受信トラフィック用に特定のネットワーク ポートを開きます</span><span class="sxs-lookup"><span data-stu-id="6087a-110">Open a specific network port for inbound traffic</span></span> |
| `restart` | <span data-ttu-id="6087a-111">仮想マシンの再起動</span><span class="sxs-lookup"><span data-stu-id="6087a-111">Restart a virtual machine</span></span> |
| `show` | <span data-ttu-id="6087a-112">仮想マシンについての詳細を取得します</span><span class="sxs-lookup"><span data-stu-id="6087a-112">Get the details for a virtual machine</span></span> |
| `start` | <span data-ttu-id="6087a-113">停止している仮想マシンを起動します</span><span class="sxs-lookup"><span data-stu-id="6087a-113">Start a stopped virtual machine</span></span> |
| `stop` | <span data-ttu-id="6087a-114">実行している仮想マシンを停止します</span><span class="sxs-lookup"><span data-stu-id="6087a-114">Stop a running virtual machine</span></span> |
| `update` | <span data-ttu-id="6087a-115">仮想マシンのプロパティを更新します</span><span class="sxs-lookup"><span data-stu-id="6087a-115">Update a property of a virtual machine</span></span> |

> [!NOTE]
> <span data-ttu-id="6087a-116">コマンドの完全な一覧については、[Azure CLI のリファレンス ドキュメント](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="6087a-116">For a complete list of commands, you can check the [Azure CLI reference documentation](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span></span>

<span data-ttu-id="6087a-117">最初の `az vm create` から始めましょう。</span><span class="sxs-lookup"><span data-stu-id="6087a-117">Let's start with the first one: `az vm create`.</span></span> <span data-ttu-id="6087a-118">このコマンドは、リソース グループ内に仮想マシンを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="6087a-118">This command is used to create a virtual machine in a resource group.</span></span> <span data-ttu-id="6087a-119">複数のパラメーターを渡して、新しい VM のすべての面を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="6087a-119">There are several parameters you can pass to configure all the aspects of the new VM.</span></span> <span data-ttu-id="6087a-120">次の 3 つのパラメーターを必ず指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="6087a-120">The three parameters that must be supplied are:</span></span>

| <span data-ttu-id="6087a-121">パラメーター</span><span class="sxs-lookup"><span data-stu-id="6087a-121">Parameter</span></span> | <span data-ttu-id="6087a-122">説明</span><span class="sxs-lookup"><span data-stu-id="6087a-122">Description</span></span> |
|-----------|-------------|
| `resource-group` | <span data-ttu-id="6087a-123">その仮想マシンを所有するリソース グループです</span><span class="sxs-lookup"><span data-stu-id="6087a-123">The resource group that will own the virtual machine</span></span> |
| `name` | <span data-ttu-id="6087a-124">仮想マシンの名前であり、リソース グループ内で一意である必要があります</span><span class="sxs-lookup"><span data-stu-id="6087a-124">The name of the virtual machine - must be unique within the resource group</span></span> |
| `image` | <span data-ttu-id="6087a-125">VM の作成に使用するオペレーティング システム イメージです</span><span class="sxs-lookup"><span data-stu-id="6087a-125">The operating system image to use to create the VM</span></span> |

<span data-ttu-id="6087a-126">さらに、`--verbose` フラグを追加して VM の作成の進行状況を表示すると便利です。</span><span class="sxs-lookup"><span data-stu-id="6087a-126">In addition, it's helpful to add the `--verbose` flag to see progress while the VM is being created.</span></span> 

## <a name="create-a-linux-virtual-machine"></a><span data-ttu-id="6087a-127">Linux 仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="6087a-127">Create a Linux virtual machine</span></span>

<span data-ttu-id="6087a-128">新しい Linux 仮想マシンを作成してみます。</span><span class="sxs-lookup"><span data-stu-id="6087a-128">Let's create a new Linux virtual machine.</span></span> <span data-ttu-id="6087a-129">Azure Cloud Shell で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6087a-129">Execute the following command in Azure Cloud Shell:</span></span>

```azurecli
az vm create --resource-group ExerciseResources --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --verbose 
```

<span data-ttu-id="6087a-130">このコマンドでは、新しい **Debian** Linux 仮想マシンが `SampleVM` という名前で作成されます。</span><span class="sxs-lookup"><span data-stu-id="6087a-130">This command will create a new **Debian** Linux virtual machine with the name `SampleVM`.</span></span> <span data-ttu-id="6087a-131">VM が作成されている間、Azure CLI ツールがブロックされることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6087a-131">Notice that the Azure CLI tool is blocked while the VM is being created.</span></span> <span data-ttu-id="6087a-132">スクリプトでコマンドを実行している場合などで待ちたくない場合は、`--no-wait` オプションを使用して、すぐに戻るよう Azure CLI ツールに指示できます。</span><span class="sxs-lookup"><span data-stu-id="6087a-132">If you would prefer not to wait, you can use the `--no-wait` option to tell the Azure CLI tool to return immediately, for example if you're executing the command in a script.</span></span> <span data-ttu-id="6087a-133">後でスクリプトの中で `azure vm wait --name [vm-name]` コマンドを使用して、VM の作成が完了するのを待機します。</span><span class="sxs-lookup"><span data-stu-id="6087a-133">Later in the script, use the `azure vm wait --name [vm-name]` command to wait for the VM to finish being created.</span></span>

<span data-ttu-id="6087a-134">詳細な応答を見ると、VM のさまざまな依存関係の名前に `SampleVM` という名前が使用されていることもわかります。</span><span class="sxs-lookup"><span data-stu-id="6087a-134">If you look at the verbose responses, you will also see that the `SampleVM` name is used to name various dependencies for the VM.</span></span>

```
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

<span data-ttu-id="6087a-135">これらの自動生成されるリソースの名前は、省略可能なパラメーター `vm create` を使用してオーバーライドできます (`--vnet-name` や `--public-ip-address-dns-name` など)。</span><span class="sxs-lookup"><span data-stu-id="6087a-135">You can override these auto-generated resource names using optional parameters to `vm create`, such as `--vnet-name` and `--public-ip-address-dns-name`.</span></span>

<span data-ttu-id="6087a-136">`admin-username` フラグを使用して管理者アカウント名を "aldis" と指定していることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="6087a-136">Notice that we are specifying the admin account name through the `admin-username` flag to be "aldis".</span></span> <span data-ttu-id="6087a-137">これを省略した場合、`vm create` コマンドは "_現在のユーザー名_" を使用します。</span><span class="sxs-lookup"><span data-stu-id="6087a-137">If you omit this, the `vm create` command will use your _current user name_.</span></span> <span data-ttu-id="6087a-138">アカウントの命名規則は OS によって異なるため、特定の名前を指定する方が安全です。</span><span class="sxs-lookup"><span data-stu-id="6087a-138">Since the rules for account names are different for each OS, it's safer to specify a specific name.</span></span> <span data-ttu-id="6087a-139">"root" や "admin" などの一般的な名前は、ほとんどのイメージで使用できません。</span><span class="sxs-lookup"><span data-stu-id="6087a-139">Common names such as "root" and "admin" are not allowed for most images.</span></span>

<span data-ttu-id="6087a-140">ここでは、`generate-ssh-keys` フラグも使用しています。</span><span class="sxs-lookup"><span data-stu-id="6087a-140">We are also using the `generate-ssh-keys` flag.</span></span> <span data-ttu-id="6087a-141">このパラメーターは Linux ディストリビューションに対して使用され、`ssh` ツールを使用して仮想マシンにリモート アクセスできるように、セキュリティ キーのペアが作成されます。</span><span class="sxs-lookup"><span data-stu-id="6087a-141">This parameter is used for Linux distributions and creates a pair of security keys so we can use the `ssh` tool to access the virtual machine remotely.</span></span> <span data-ttu-id="6087a-142">コンピューター上と VM 内の `.ssh` フォルダーに、2 つのファイルが配置されます。</span><span class="sxs-lookup"><span data-stu-id="6087a-142">The two files are placed into the `.ssh` folder on your machine and in the VM.</span></span> <span data-ttu-id="6087a-143">ターゲット フォルダーに `id_rsa` という名前の SSH キーが既にある場合はそれが使用され、新しいキーは生成されません。</span><span class="sxs-lookup"><span data-stu-id="6087a-143">If you already have an SSH key named `id_rsa` in the target folder, then it will be used rather than having a new key generated.</span></span>

<span data-ttu-id="6087a-144">VM の作成が終了すると、JSON 応答を受け取ります。それには、仮想マシンの現在の状態と、Azure によって割り当てられたパブリックおよびプライベート IP アドレスが含まれます。</span><span class="sxs-lookup"><span data-stu-id="6087a-144">Once it finishes creating the VM, you will get a JSON response which includes the current state of the virtual machine and its public and private IP addresses assigned by Azure:</span></span>

```json
{
  "fqdns": "",
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1A-D9-74",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "168.61.54.62",
  "resourceGroup": "ExerciseResources",
  "zones": ""
}
```

