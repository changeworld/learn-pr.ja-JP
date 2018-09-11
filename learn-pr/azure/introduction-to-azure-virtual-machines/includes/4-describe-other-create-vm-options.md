<span data-ttu-id="f286a-101">VM などのリソースを初めて作成する場合には、Azure portal が最も簡単です。</span><span class="sxs-lookup"><span data-stu-id="f286a-101">The Azure portal is the easiest way to create resources such as VMs when you are getting started.</span></span> <span data-ttu-id="f286a-102">ただし、いくつかのリソースをまとめて作成する場合は、Azure を使用する方法が、最も効率的または早いわけではありません。</span><span class="sxs-lookup"><span data-stu-id="f286a-102">However, it's not necessarily the most efficient or quickest way to work with Azure, particularly if you need to create several resources together.</span></span> <span data-ttu-id="f286a-103">ここでは、さまざまなタスクを処理する数十の VM を最終的に作成します。</span><span class="sxs-lookup"><span data-stu-id="f286a-103">In our case, we will eventually be creating dozens of VMs to handle different tasks.</span></span> <span data-ttu-id="f286a-104">Azure portal でそれらを手動で作成するのは、楽しい作業ではありません。</span><span class="sxs-lookup"><span data-stu-id="f286a-104">Creating them manually in the Azure portal wouldn't be a fun task!</span></span>

<span data-ttu-id="f286a-105">Azure でリソースを作成および管理する方法には、他に次などもあります。</span><span class="sxs-lookup"><span data-stu-id="f286a-105">Let's look at some other ways to create and administer resources in Azure:</span></span>

- [<span data-ttu-id="f286a-106">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f286a-106">Azure Resource Manager</span></span>](#Azure_RM)
- [<span data-ttu-id="f286a-107">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f286a-107">Azure PowerShell</span></span>](#Azure_PowerShell)
- [<span data-ttu-id="f286a-108">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f286a-108">Azure CLI</span></span>](#Azure_CLI)
- [<span data-ttu-id="f286a-109">Azure REST API</span><span class="sxs-lookup"><span data-stu-id="f286a-109">Azure REST API</span></span>](#Azure_REST_API)
- [<span data-ttu-id="f286a-110">Azure クライアント SDK</span><span class="sxs-lookup"><span data-stu-id="f286a-110">Azure Client SDK</span></span>](#Azure_Client_SDK)
- [<span data-ttu-id="f286a-111">Azure VM 拡張機能</span><span class="sxs-lookup"><span data-stu-id="f286a-111">Azure VM Extensions</span></span>](#Azure_VMExtensions)
- [<span data-ttu-id="f286a-112">Azure Automation サービス</span><span class="sxs-lookup"><span data-stu-id="f286a-112">Azure Automation Services</span></span>](#Azure_Automation)

<a name="Azure_RM" />

## <a name="azure-resource-manager"></a><span data-ttu-id="f286a-113">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f286a-113">Azure Resource Manager</span></span>

<span data-ttu-id="f286a-114">VM のコピーを、同じ設定で作成したいとします。</span><span class="sxs-lookup"><span data-stu-id="f286a-114">Let's assume you want to create a copy of a VM with the same settings.</span></span> <span data-ttu-id="f286a-115">その場合、VM のイメージを作成し、それを Azure にアップロードし、新しい VM の基として参照します。</span><span class="sxs-lookup"><span data-stu-id="f286a-115">You could create a VM image, upload it to Azure and reference it as the basis for your new VM.</span></span> <span data-ttu-id="f286a-116">この手順は、非効率的で時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="f286a-116">This process is inefficient and time-consuming.</span></span> <span data-ttu-id="f286a-117">Azure には、VM の正確なコピーを作成する、テンプレートを作成するためのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="f286a-117">Azure provides you with the option to create a template from which to create an exact copy of a VM.</span></span>

<span data-ttu-id="f286a-118">通常、お使いの Azure のインフラストラクチャには、多くのリソースが含まれており、そのほとんどが他のものに何らかの方法で関連しています。</span><span class="sxs-lookup"><span data-stu-id="f286a-118">Typically, your Azure infrastructure will contain many resources, many of them related to one another in some way.</span></span> <span data-ttu-id="f286a-119">たとえば、作成した VM には、WordPress サイトを実行するために、まとめてすべて作成した、仮想マシン自体、ストレージ、ネットワーク インターフェイス、Web サーバーおよびデータベースがあります。</span><span class="sxs-lookup"><span data-stu-id="f286a-119">For example, the VM we created has the virtual machine itself, storage, network interface, web server, and a database - all created together to run the WordPress site.</span></span> <span data-ttu-id="f286a-120">**Azure Resource Manager** は、これらの関連リソースをより効率的に操作できるようにします。</span><span class="sxs-lookup"><span data-stu-id="f286a-120">**Azure Resource Manager** makes working with these related resources more efficient.</span></span> <span data-ttu-id="f286a-121">これでは、リソースが名前付きの**リソース グループに**まとめられるため、ユーザーはすべてのリソースを、まとめてデプロイ、更新または削除できるようになります。</span><span class="sxs-lookup"><span data-stu-id="f286a-121">It organizes resources into named **Resource Groups** that let you deploy, update, or delete all of the resources together.</span></span> <span data-ttu-id="f286a-122">WordPress サイトの作成時、VM を作成する一貫としてリソース グループを作成しました。そして Resource Manager によって関連するリソースが同じグループに入れられました。</span><span class="sxs-lookup"><span data-stu-id="f286a-122">When we created the WordPress site, we identified the Resource Group as part of the VM creation, and Resource Manager placed the associated resources into the same group.</span></span>

<span data-ttu-id="f286a-123">Resource Manager では、特定の構成を作成およびデプロイするために使用できる_テンプレート_も作成できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-123">Resource Manager also allows you to create _templates_ which can be used to create and deploy specific configurations.</span></span>

### <a name="what-are-resource-manager-templates"></a><span data-ttu-id="f286a-124">Resource Manager テンプレートとは何ですか?</span><span class="sxs-lookup"><span data-stu-id="f286a-124">What are Resource Manager templates?</span></span>

<span data-ttu-id="f286a-125">**Resource Manager テンプレート**とは、ソリューションに対してデプロイが必要なリソースを定義した JSON ファイルのことです。</span><span class="sxs-lookup"><span data-stu-id="f286a-125">**Resource Manager templates** are JSON files that define the resources you need to deploy for your solution.</span></span>

<span data-ttu-id="f286a-126">リソース テンプレートは、特定の VM の **[設定]** セクションで [Automation スクリプト] オプションを選択することによって、作成することができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-126">You can create resource templates from the **Settings** section for a specific VM by selecting the Automation script option.</span></span>

![VM 用の Automation スクリプト](../media-draft/4-automation-script.png)

<span data-ttu-id="f286a-128">ユーザーには、リソース テンプレートを後で使用するためのオプション、またはこのテンプレートを使用して新しい VM をすぐにデプロイするオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="f286a-128">You have the option to save the resource template for later use or immediately deploy a new VM based on this template.</span></span> <span data-ttu-id="f286a-129">たとえば、テスト環境でテンプレートから作成した VM が、お使いのオンプレミスのマシンと置き換えるとき、うまく機能しないことがわかったとします。</span><span class="sxs-lookup"><span data-stu-id="f286a-129">For example, you might create a VM from a template in a test environment and find it doesn’t quite work to replace your on-premise machine.</span></span> <span data-ttu-id="f286a-130">リソース グループを削除し (このとき、すべてのリソースが削除されます)、テンプレートを調整して、再度試すことができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-130">You can delete the resource group, which deletes all of the resources, tweak the template and try again.</span></span> <span data-ttu-id="f286a-131">デプロイ済みの既存のリソースのみを変更したい場合、それを作成するために使用したテンプレートを変更して、再度デプロイできます。</span><span class="sxs-lookup"><span data-stu-id="f286a-131">If you only want to make changes to the existing deployed resources, you can change the template used to create it and deploy it again.</span></span> <span data-ttu-id="f286a-132">Resource Manager により、新しいテンプレートに合うようリソースが変更されます。</span><span class="sxs-lookup"><span data-stu-id="f286a-132">Resource Manager will change the resources to match the new template.</span></span>

<span data-ttu-id="f286a-133">一度、それが希望どおりに動作するようになると、そのテンプレートを使用し、ステージングや運用などインフラストラクチャの複数のバージョンを簡単に何度も作成することができるようになります。</span><span class="sxs-lookup"><span data-stu-id="f286a-133">Once you have it working the way you want it, you can take that template and easily re-create multiple versions of your infrastructure, such as staging and production.</span></span> <span data-ttu-id="f286a-134">VM 名、ネットワーク名、ストレージ アカウント名などのフィールドをパラメーター化し、繰り返しテンプレートを読み込み、さまざまなパラメーターを使用し、各環境用にカスタマイズすることができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-134">You can parameterize fields such as the VM name, network name, storage account name, etc., and load the template repeatedly, using different parameters to customize each environment.</span></span>

<span data-ttu-id="f286a-135">お好きなプログラミング言語を、Azure CLI、Azure PowerShell などの Automation スクリプト ツールや Azure REST API で使用して、リソース テンプレートを処理して、この強力なツールでインフラストラクチャを迅速に起動させることができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-135">You can use automation scripting tools such as the Azure CLI, Azure PowerShell, or even the Azure REST APIs with your favorite programming language to process resource templates making this a powerful tool for quickly spinning up your infrastructure.</span></span>

<a name="Azure_PowerShell" />

## <a name="azure-powershell"></a><span data-ttu-id="f286a-136">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="f286a-136">Azure PowerShell</span></span>

<span data-ttu-id="f286a-137">管理スクリプトを作成すると、毎日繰り返すタスクを自動化して、ワークフローを強力に最適化できます。一度検証されたそのスクリプトは、一貫して実行されるようになり、エラーを減らすことができるようになります。</span><span class="sxs-lookup"><span data-stu-id="f286a-137">Creating administration scripts is a powerful way to optimize your workflow, you can automate everyday, repetitive tasks, and once a script has been verified, it will run consistently, likely reducing errors.</span></span> <span data-ttu-id="f286a-138">**Azure PowerShell** は、1 回限りの対話型のタスクや、繰り返し実行するタスクを自動化するのに最適です。</span><span class="sxs-lookup"><span data-stu-id="f286a-138">**Azure PowerShell** is ideal for one-off interactive tasks and or the automate of repeated tasks.</span></span>

> [!NOTE]
> <span data-ttu-id="f286a-139">PowerShell とは、シェル ウィンドウやコマンド解析などのサービスを提供する、クロスプラットフォームのシェルです。</span><span class="sxs-lookup"><span data-stu-id="f286a-139">PowerShell is a cross-platform shell that provides services like the shell window and command parsing.</span></span> <span data-ttu-id="f286a-140">Azure PowerShell は、(**コマンドレット**と呼ばれる) Azure 固有のコマンドを追加する、オプションのアドオン パッケージです。</span><span class="sxs-lookup"><span data-stu-id="f286a-140">Azure PowerShell is an optional add-on package which adds the Azure-specific commands (referred to as **cmdlets**).</span></span> <span data-ttu-id="f286a-141">Azure PowerShell のインストールと使用の詳細は、別のトレーニング モジュールで説明しています。</span><span class="sxs-lookup"><span data-stu-id="f286a-141">You can learn more about installing and using Azure PowerShell in a separate training module.</span></span>

<span data-ttu-id="f286a-142">たとえば、`New-AzureRmVM` コマンドレットを使用すると、Azure の新しい仮想マシンを作成できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-142">For example, you can use the `New-AzureRmVM` cmdlet to create a new Azure Virtual Machine.</span></span> 

```powershell
New-AzureRmVm `
    -ResourceGroupName "TestResourceGroup" `
    -Name "test-wp1-eus-vm" `
    -Location "East US" `
    -VirtualNetworkName "test-wp1-eus-network" `
    -SubnetName "default" `
    -SecurityGroupName "test-wp1-eus-nsg" `
    -PublicIpAddressName "test-wp1-eus-pubip" `
    -OpenPorts 80,3389
```

<span data-ttu-id="f286a-143">ここで示すさまざまなパラメーターを提供し、利用できる多数の VM の構成設定を行えます。</span><span class="sxs-lookup"><span data-stu-id="f286a-143">As shown here, you supply various parameters to handle the large number of VM configuration settings available.</span></span> <span data-ttu-id="f286a-144">ほとんどのパラメーターには妥当な値があるため、必須パラメーターのみを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="f286a-144">Most of the parameters have reasonable values; you only need to specify the required parameters.</span></span> <span data-ttu-id="f286a-145">Azure PowerShell で VM を作成および管理する方法の詳細については、「**Automate Azure tasks using scripts with PowerShell**」(PowerShell でスクリプトを使用して Azure タスクを自動化する) のモジュールを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f286a-145">Learn more about creating and managing VMs with Azure PowerShell in the **Automate Azure tasks using scripts with PowerShell** module.</span></span>
<a name="Azure_CLI" />

## <a name="azure-cli"></a><span data-ttu-id="f286a-146">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f286a-146">Azure CLI</span></span>

<span data-ttu-id="f286a-147">スクリプトとコマンド ラインで Azure とやり取りする別のオプションに、**Azure CLI** があります。</span><span class="sxs-lookup"><span data-stu-id="f286a-147">Another option for scripting and command-line Azure interaction is the **Azure CLI**.</span></span>

<span data-ttu-id="f286a-148">Azure CLI は、コマンド ラインから仮想マシンやディスクなどの Azure リソースを管理するための、Microsoft のクロスプラットフォーム コマンド ライン ツールです。</span><span class="sxs-lookup"><span data-stu-id="f286a-148">The Azure CLI is Microsoft's cross-platform command-line tool for managing Azure resources such as virtual machines and disks from the command line.</span></span> <span data-ttu-id="f286a-149">このツールは、macOS、Linux、および Windows で使用できます。またはブラウザーで Cloud Shell を使用して使用できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-149">It's available for macOS, Linux, and Windows, or in the browser using the Cloud Shell.</span></span> <span data-ttu-id="f286a-150">Azure PowerShell のように、Azure CLI は、管理ワークフローを効率化する強力な方法です。</span><span class="sxs-lookup"><span data-stu-id="f286a-150">Like Azure PowerShell, the Azure CLI is a powerful way to streamline your administrative workflow.</span></span> <span data-ttu-id="f286a-151">Azure CLI では、Azure PowerShell とは異なり、機能に PowerShell は不要です。</span><span class="sxs-lookup"><span data-stu-id="f286a-151">Unlike Azure PowerShell, the Azure CLI does not need PowerShell to function.</span></span>

<span data-ttu-id="f286a-152">たとえば、`az vm create` コマンドを使用して Azure VM を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-152">For example, you can create an Azure VM with the `az vm create` command.</span></span>

```bash
az vm create \
    --resource-group TestResourceGroup \
    --name test-wp1-eus-vm \
    --image win2016datacenter \
    --admin-username jonc \
    --admin-password aReallyGoodPasswordHere
```

<span data-ttu-id="f286a-153">Azure CLI は、Ruby や Python などのその他のスクリプト言語でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-153">The Azure CLI can be used with other scripting languages, for example, Ruby and Python.</span></span> <span data-ttu-id="f286a-154">いずれの言語も、管理者が PowerShell に精通していない場合に、Windows 以外のコンピューターでよく使用されます。</span><span class="sxs-lookup"><span data-stu-id="f286a-154">Both languages are commonly used on non-windows based machines where the developer may not be familiar with PowerShell.</span></span>

<span data-ttu-id="f286a-155">VM を作成および管理する方法の詳細については、「**Manage virtual machines with the Azure CLI tool**」(Azure CLI ツールを使用して仮想マシンを管理する) モジュールを参照してください。</span><span class="sxs-lookup"><span data-stu-id="f286a-155">Learn more about creating and managing VMs in the **Manage virtual machines with the Azure CLI tool** module.</span></span>

## <a name="programmatic-apis"></a><span data-ttu-id="f286a-156">プログラム (API)</span><span class="sxs-lookup"><span data-stu-id="f286a-156">Programmatic (APIs)</span></span>

<span data-ttu-id="f286a-157">一般的に、実行するスクリプトがシンプルであり、コマンドライン ツールを使用したい場合、Azure PowerShell と Azure CLI はいずれも適した選択肢です。</span><span class="sxs-lookup"><span data-stu-id="f286a-157">Generally speaking, both Azure PowerShell and Azure CLI are good options if you have simple scripts to run and want to stick to command-line tools.</span></span> <span data-ttu-id="f286a-158">複雑なロジックの大規模なアプリケーションの一部として VM を作成および管理する場合のより複雑なシナリオでは、別のアプローチをとる必要があります。</span><span class="sxs-lookup"><span data-stu-id="f286a-158">When it comes to more complex scenarios where the creation and management of VM form part of a larger application with complex logic, another approach is needed.</span></span>

<span data-ttu-id="f286a-159">Azure では、プログラムを使用して、さまざまな種類のリソースとやり取りすることができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-159">You can interact with every type of resource in Azure programmatically.</span></span>

<a name="Azure_REST_API" />

### <a name="azure-rest-api"></a><span data-ttu-id="f286a-160">Azure REST API</span><span class="sxs-lookup"><span data-stu-id="f286a-160">Azure REST API</span></span>

<span data-ttu-id="f286a-161">開発者は、Azure REST API により、リソースでカテゴリ化された操作を実行できるようになり、また VM を作成および管理できるようになります。</span><span class="sxs-lookup"><span data-stu-id="f286a-161">The Azure REST API provides developers operations categorized by resource as well as the ability to create and manage VMs.</span></span> <span data-ttu-id="f286a-162">操作は、対応する HTTP メソッド (`GET`、`PUT`、`POST``DELETE`、および `PATCH`) と対応する応答と共に、URI として公開されます。</span><span class="sxs-lookup"><span data-stu-id="f286a-162">Operations are exposed as URIs with corresponding HTTP methods (`GET`, `PUT`, `POST`, `DELETE`, and `PATCH`) and a corresponding response.</span></span>

<span data-ttu-id="f286a-163">Azure コンピューティング API によって、仮想マシンおよび仮想マシンでサポートされるリソースに、プログラムからアクセスできるようになります。</span><span class="sxs-lookup"><span data-stu-id="f286a-163">The Azure Compute APIs give you programmatic access to virtual machines and their supporting resources.</span></span> <span data-ttu-id="f286a-164">この API を使用すると、次の操作を実行できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-164">With this API, you have operations to:</span></span>

- <span data-ttu-id="f286a-165">可用性セットの作成および管理</span><span class="sxs-lookup"><span data-stu-id="f286a-165">Create and manage availability sets</span></span>
- <span data-ttu-id="f286a-166">仮想マシンの拡張機能の追加と管理</span><span class="sxs-lookup"><span data-stu-id="f286a-166">Add and manage virtual machine extensions</span></span>
- <span data-ttu-id="f286a-167">マネージド ディスク、スナップショットおよびイメージの作成と管理</span><span class="sxs-lookup"><span data-stu-id="f286a-167">Create and manage managed disks, snapshots, and images</span></span>
- <span data-ttu-id="f286a-168">Azure で利用できるプラットフォーム イメージへのアクセス</span><span class="sxs-lookup"><span data-stu-id="f286a-168">Access the platform images available in Azure</span></span>
- <span data-ttu-id="f286a-169">リソースの使用量情報の取得</span><span class="sxs-lookup"><span data-stu-id="f286a-169">Retrieve usage information of your resources</span></span>
- <span data-ttu-id="f286a-170">仮想マシンの作成と管理</span><span class="sxs-lookup"><span data-stu-id="f286a-170">Create and manage virtual machines</span></span>
- <span data-ttu-id="f286a-171">仮想マシン スケールセットの作成と管理</span><span class="sxs-lookup"><span data-stu-id="f286a-171">Create and manage virtual machine scale sets</span></span>

<a name="Azure_Client_SDK" />

### <a name="azure-client-sdk"></a><span data-ttu-id="f286a-172">Azure クライアント SDK</span><span class="sxs-lookup"><span data-stu-id="f286a-172">Azure Client SDK</span></span>

<span data-ttu-id="f286a-173">REST API はプラットフォームと言語に依存しませんが、開発者はしばしばより高いレベルの抽象化を求めます。</span><span class="sxs-lookup"><span data-stu-id="f286a-173">Even though the REST API is platform and language agnostic, most often developers will look towards a higher level of abstraction.</span></span> <span data-ttu-id="f286a-174">Azure クライアント SDK では、Azure REST API をカプセル化できるため、開発者は Azure とより簡単にやり取りすることができるようになります。</span><span class="sxs-lookup"><span data-stu-id="f286a-174">The Azure Client SDK encapsulates the Azure REST API making it much easier for developers to interact with Azure.</span></span>

<span data-ttu-id="f286a-175">Azure クライアント SDK は、C#、Java、Node.js、PHP、Python、Ruby、および Go など .NET ベースの言語を含む、さまざまな言語やフレームワークで使用できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-175">The Azure Client SDKs are available for a variety of languages and frameworks including .NET based languages such as C#, Java, Node.js, PHP, Python, Ruby, and Go.</span></span>

<span data-ttu-id="f286a-176">`Microsoft.Azure.Management.Fluent` NuGet パッケージが使用された Azure VM を作成する C# コードのスニペット例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="f286a-176">Here's an example snippet of C# code to create an Azure VM using the `Microsoft.Azure.Management.Fluent` NuGet package:</span></span>

```csharp
var azure = Azure
    .Configure()
    .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
    .Authenticate(credentials)
    .WithDefaultSubscription();
// ...
var vmName = "test-wp1-eus-vm";

azure.VirtualMachines.Define(vmName)
    .WithRegion(Region.USEast)
    .WithExistingResourceGroup("TestResourceGroup")
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("jonc")
    .WithAdminPassword("aReallyGoodPasswordHere")
    .WithComputerName(vmName)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

<span data-ttu-id="f286a-177">**Azure Java SDK** が使用された Java の同じスニペットは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="f286a-177">Here's the same snippet in Java using the **Azure Java SDK**:</span></span>

```java
String vmName = "test-wp1-eus-vm";
// ...
VirtualMachine virtualMachine = azure.virtualMachines()
    .define(vmName)
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("TestResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("jonc")
    .withAdminPassword("aReallyGoodPasswordHere")
    .withComputerName(vmName)
    .withSize("Standard_DS1")
    .create();
```

<a name="Azure_VMExtensions" />

## <a name="azure-vm-extensions"></a><span data-ttu-id="f286a-178">Azure VM 拡張機能</span><span class="sxs-lookup"><span data-stu-id="f286a-178">Azure VM Extensions</span></span>

<span data-ttu-id="f286a-179">仮想マシンの最初のデプロイ後に、ソフトウェアを追加で構成およびインストールしたいとします。</span><span class="sxs-lookup"><span data-stu-id="f286a-179">Let's assume you want to configure and install additional software on your virtual machine after the initial deployment.</span></span> <span data-ttu-id="f286a-180">このタスクで、自動的に監視および実行される特定の構成を使用したいとします。</span><span class="sxs-lookup"><span data-stu-id="f286a-180">You want this task to use a specific configuration, monitored and executed automatically.</span></span>

<span data-ttu-id="f286a-181">**Azure VM 拡張機能**は、Azure VM の最初のデプロイ後にタスクを構成および自動化したい場合に使用できる、小規模なアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="f286a-181">**Azure VM extensions** are small applications that allow you to configure and automate tasks on Azure VMs after initial deployment.</span></span> <span data-ttu-id="f286a-182">**Azure VM 拡張機能**は、Azure CLI、PowerShell、Azure Resource Manager テンプレート、および Azure portal を使って実行できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-182">**Azure VM extensions** can be run with the Azure CLI, PowerShell, Azure Resource Manager templates, and the Azure portal.</span></span>

<span data-ttu-id="f286a-183">拡張機能は、新しい VM のデプロイにバンドルすることも、既存のシステムに対して実行することもできます。</span><span class="sxs-lookup"><span data-stu-id="f286a-183">You bundle extensions with a new VM deployment or run them against an existing system.</span></span>

<a name="Azure_Automation" />

## <a name="azure-automation-services"></a><span data-ttu-id="f286a-184">Azure Automation サービス</span><span class="sxs-lookup"><span data-stu-id="f286a-184">Azure Automation Services</span></span>

<span data-ttu-id="f286a-185">リモート インフラストラクチャを管理する場合の運用上の最大の課題は、時間の短縮、エラーの削減、効率性の向上です。</span><span class="sxs-lookup"><span data-stu-id="f286a-185">Saving time, reducing errors, and increasing efficiency are some of the most significant operational management challenges faced when managing remote infrastructure.</span></span> <span data-ttu-id="f286a-186">多くのインフラストラクチャ サービスがある場合、高いレベルから操作できるように、Azure でレベルの高いサービスを使用することを検討したい場合があるでしょう。</span><span class="sxs-lookup"><span data-stu-id="f286a-186">If you have a lot of infrastructure services, you might want to consider using higher-level services in Azure to help you operate from a higher level.</span></span>

<span data-ttu-id="f286a-187">**Azure Automation** では、頻繁に発生する、時間のかかる、エラーが発生しやすいタスクを簡単自動化して、サービスを統合できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-187">**Azure Automation** allows you to integrate services that allow you to automate frequent, time-consuming and error-prone management tasks with ease.</span></span> <span data-ttu-id="f286a-188">これらのサービスには、**プロセスの自動化**、**構成管理、および**更新管理\*\*が含まれます。</span><span class="sxs-lookup"><span data-stu-id="f286a-188">These services include **process automation**, \*\*configuration management, and **update management**.</span></span>

- <span data-ttu-id="f286a-189">**プロセス管理**。</span><span class="sxs-lookup"><span data-stu-id="f286a-189">**Process Management**.</span></span> <span data-ttu-id="f286a-190">特定のエラー イベントを監視している VM があるとします。</span><span class="sxs-lookup"><span data-stu-id="f286a-190">Let's assume you have a VM that is monitored for a specific error event.</span></span> <span data-ttu-id="f286a-191">それが報告されたら、できるだけ早くアクションをとり、問題を解決したいと思っています。</span><span class="sxs-lookup"><span data-stu-id="f286a-191">You want to take action and fix the problem as soon as it's reported.</span></span> <span data-ttu-id="f286a-192">プロセス オートメーションを使用すると、データセンターで発生する可能性のあるイベントに対応する監視タスクを設定できます。</span><span class="sxs-lookup"><span data-stu-id="f286a-192">Process automation allows you to set up watcher tasks that can respond to events that may occur in your datacenter.</span></span>

- <span data-ttu-id="f286a-193">**構成管理**。</span><span class="sxs-lookup"><span data-stu-id="f286a-193">**Configuration Management**.</span></span>  <span data-ttu-id="f286a-194">ユーザーが、VM で実行しているオペレーティング システムで利用可能なソフトウェアの更新をトラッキングしたいとします。</span><span class="sxs-lookup"><span data-stu-id="f286a-194">Perhaps you want to track software updates that become available for the operating system that runs on your VM.</span></span> <span data-ttu-id="f286a-195">ユーザーには、インストールしたいまたはしたくない、特定の更新プログラムがあります。</span><span class="sxs-lookup"><span data-stu-id="f286a-195">There are specific updates you may want to include or exclude.</span></span> <span data-ttu-id="f286a-196">構成管理では、これらの更新プログラムを追跡して、必要に応じてアクションをとることができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-196">Configuration management allows you to track these updates and take action as required.</span></span> <span data-ttu-id="f286a-197">会社の PC、サーバー、モバイル デバイスの管理には、**System Center Configuration Manager** を使用します。</span><span class="sxs-lookup"><span data-stu-id="f286a-197">You use **System Center Configuration Manager** to manage your company's PC, servers, and mobile devices.</span></span> <span data-ttu-id="f286a-198">Configuration Manager を使用すると、このサポートを、Azure VM まで拡張することができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-198">You can extend this support to your Azure VMs with Configuration Manager.</span></span>

- <span data-ttu-id="f286a-199">**Update Management**。</span><span class="sxs-lookup"><span data-stu-id="f286a-199">**Update Management**.</span></span> <span data-ttu-id="f286a-200">これは、ご使用の VM の更新プログラムおよび修正プログラムの管理に使用します。</span><span class="sxs-lookup"><span data-stu-id="f286a-200">This is used to manage updates and patches for your VMs.</span></span> <span data-ttu-id="f286a-201">このサービスでは、利用可能な更新プログラムの状態を評価したり、インストールをスケジュールしたり、デプロイの結果を確認して、更新プログラムが正常に適用されたことを検証したりすることができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-201">With this service, you're able to assess the status of available updates, schedule installation, and review deployment results to verify updates applied successfully.</span></span> <span data-ttu-id="f286a-202">Update Management には、プロセスおよび構成を管理するサービスが組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="f286a-202">Update management incorporates services that provide process and configuration management.</span></span> <span data-ttu-id="f286a-203">VM の Update Management は、**Azure Automation** アカウントから直接有効にできます。</span><span class="sxs-lookup"><span data-stu-id="f286a-203">You enable update management for a VM directly from your **Azure Automation** account.</span></span> <span data-ttu-id="f286a-204">また、単一の仮想マシンで Update Management を有効にするには、ポータルの仮想マシン ブレードから行うことができます。</span><span class="sxs-lookup"><span data-stu-id="f286a-204">You can also allow update management for a single virtual machine from the virtual machine blade in the portal.</span></span>

<span data-ttu-id="f286a-205">ご覧のとおり、Azure には、_ご自分に有効な_プロセスに管理操作を統合できる、リソースを作成および管理するツールが多数あります。</span><span class="sxs-lookup"><span data-stu-id="f286a-205">As you can see, Azure provides a variety of tools to create and administer resources so you can integrate management operations into a process _that works for you_.</span></span> <span data-ttu-id="f286a-206">インフラストラクチャのリソースがスムーズに実行されるよう、その他の Azure のサービスを検証してみましょう。</span><span class="sxs-lookup"><span data-stu-id="f286a-206">Let's examine some of the other services Azure to make sure your infrastructure resources are running smoothly.</span></span>
