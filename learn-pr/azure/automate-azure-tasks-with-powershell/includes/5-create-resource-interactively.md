<span data-ttu-id="dd8d6-101">PowerShell では、コマンドを記述し、すぐに実行できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-101">PowerShell lets you write commands and execute them immediately.</span></span> <span data-ttu-id="dd8d6-102">これは**対話モード**と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-102">This is known as **interactive mode**.</span></span>

<span data-ttu-id="dd8d6-103">顧客関係管理 (CRM) の例の全体的な目標は VM を含む 3 つのテスト環境を作成することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-103">Recall that the overall goal in the Customer Relationship Management (CRM) example is to create three test environments containing VMs.</span></span> <span data-ttu-id="dd8d6-104">リソース グループを利用して VM を別々の環境に整理します。単体テスト用に 1 つ、統合テスト用に 1 つ、受け入れテスト用に 1 つとなります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-104">You will use resource groups to ensure the VMs are organized into separate environments: one for unit testing, one for integration testing, and one for acceptance testing.</span></span> <span data-ttu-id="dd8d6-105">リソース グループは一度だけ作成すれば十分です。その場合、PowerShell の対話モードを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-105">You only need to create the resource groups once, which means using the interactive mode of PowerShell is a good choice.</span></span>

<span data-ttu-id="dd8d6-106">PowerShell にコマンドを入力すると、_コマンドレット_と照合され、その後、要求されたアクションが実行されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-106">When you type a command into PowerShell, it matches it to a _cmdlet_ which then performs the requested action.</span></span> <span data-ttu-id="dd8d6-107">使用できる一般的なコマンドをいくつか見てから、PowerShell 用の Azure サポートのインストールについて確認します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-107">We're going to look at some of the common commands you can use, and then look at installing the Azure support for PowerShell.</span></span>

## <a name="what-are-powershell-cmdlets"></a><span data-ttu-id="dd8d6-108">PowerShell コマンドレットとは</span><span class="sxs-lookup"><span data-stu-id="dd8d6-108">What are PowerShell cmdlets?</span></span>
<span data-ttu-id="dd8d6-109">PowerShell コマンドは**コマンドレット**と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-109">A PowerShell command is called a **cmdlet** (pronounced "command-let").</span></span> <span data-ttu-id="dd8d6-110">コマンドレットは 1 つの機能を操作するコマンドです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-110">A cmdlet is a command that manipulates a single feature.</span></span> <span data-ttu-id="dd8d6-111">**コマンドレット**と用語は "小さなコマンド" を意味します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-111">The term **cmdlet** is intended to imply "small command".</span></span> <span data-ttu-id="dd8d6-112">慣例で、コマンドレットの作成者には、コマンドレットをシンプルにすること、目的を 1 つだけにすることが推奨されています。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-112">By convention, cmdlet authors are encouraged to keep cmdlets simple and single-purpose.</span></span>

<span data-ttu-id="dd8d6-113">基本の PowerShell 製品には、セッションやバックグラウンド ジョブなどの機能で使用できるコマンドレットが付属します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-113">The base PowerShell product ships with cmdlets that work with features such as sessions and background jobs.</span></span> <span data-ttu-id="dd8d6-114">他の機能を操作するコマンドレットを得るには、PowerShell インストールにモジュールを追加します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-114">You add modules to your PowerShell installation to get cmdlets that manipulate other features.</span></span> <span data-ttu-id="dd8d6-115">たとえば、サードパーティ モジュールには、FTP と連動するモジュール、オペレーティング システムを管理するモジュール、ファイル システムにアクセスするモジュールなどがあります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-115">For example, there are third-party modules to work with ftp, administer your operating system, access the file system, and so on.</span></span>

<span data-ttu-id="dd8d6-116">コマンドレットには、動詞-名詞の命名規則が採用されています。たとえば、**Get-Process**、**Format-Table**、**Start-Service** のようになります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-116">Cmdlets follow a verb-noun naming convention; for example, **Get-Process**, **Format-Table**, and **Start-Service**.</span></span> <span data-ttu-id="dd8d6-117">動詞の選択にも規則があります。たとえば、データの取得は "get"、データの挿入または更新は "set"、データの書式設定は "format"、宛先への直接出力は "out" になります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-117">There is also a convention for verb choice: "get" to retrieve data, "set" to insert or update data, "format" to format data, "out" to direct output to a destination, and so on.</span></span>

<span data-ttu-id="dd8d6-118">コマンドレットの作成者には、コマンドレットごとにヘルプ ファイルを含めることが推奨されています。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-118">Cmdlet authors are encouraged to include a help file for each cmdlet.</span></span> <span data-ttu-id="dd8d6-119">**Get-Help** コマンドレットを実行すると、コマンドレットのヘルプ ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-119">The **Get-Help** cmdlet displays the help file for any cmdlet.</span></span> <span data-ttu-id="dd8d6-120">たとえば、次のステートメントで `Get-ChildItem` コマンドレットに関するヘルプを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-120">For example, we could get help on the `Get-ChildItem` cmdlet with the following statement:</span></span>

```powershell
Get-Help Get-ChildItem -detailed
```

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="dd8d6-121">PowerShell モジュールとは</span><span class="sxs-lookup"><span data-stu-id="dd8d6-121">What is a PowerShell module?</span></span>

<span data-ttu-id="dd8d6-122">コマンドレットは_モジュール_に付属しています。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-122">Cmdlets are shipped in _modules_.</span></span> <span data-ttu-id="dd8d6-123">PowerShell モジュールは、利用可能な各コマンドレットを処理するコードを含む DLL です。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-123">A PowerShell Module is a DLL that includes the code to proces each available cmdlet.</span></span> <span data-ttu-id="dd8d6-124">PowerShell にコマンドレットを読み込む場合は、そのコマンドレットが含まれるモジュールを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-124">You load cmdlets into PowerShell by loading the module they are contained in.</span></span> <span data-ttu-id="dd8d6-125">`Get-Module` コマンドを使用して、読み込まれたモジュールのリストを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-125">You can get a list of loaded modules using the `Get-Module` command:</span></span>

```powershell
Get-Module
```

<span data-ttu-id="dd8d6-126">その出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-126">This will output something like:</span></span>

```output
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Find-PackageProvider, Get-Package, Get-Pack...
Script     1.0.0.1    PowerShellGet                       {Find-Command, Find-DscResource, Find-Module, Find-RoleCap...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

## <a name="what-is-azurerm"></a><span data-ttu-id="dd8d6-127">AzureRM とは</span><span class="sxs-lookup"><span data-stu-id="dd8d6-127">What is AzureRM?</span></span>
<span data-ttu-id="dd8d6-128">**AzureRM** は、Azure 機能で使用できるコマンドレットを含む Azure PowerShell コマンドレットの正式名称です (名前の中の **RM** は **Resource Manager** という意味です)。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-128">**AzureRM** is the formal name for the Azure PowerShell module containing cmdlets to work with Azure features (the **RM** in the name stands for **Resource Manager**).</span></span> <span data-ttu-id="dd8d6-129">数百単位のコマンドレットが含まれ、あらゆる Azure リソースのほぼすべての面を制御できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-129">It contains hundreds of cmdlets that let you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="dd8d6-130">リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-130">You can work with resource groups, storage, virtual machines, Azure Active Directory, containers, machine learning, and so on.</span></span> <span data-ttu-id="dd8d6-131">このモジュールは、[GitHub で入手できる](https://github.com/Azure/azure-powershell)オープン ソースのコンポーネントです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-131">This module is an open source component [available on GitHub](https://github.com/Azure/azure-powershell).</span></span>

> [!NOTE]
> <span data-ttu-id="dd8d6-132">Azure PowerShell モジュールのインストールは任意です。モジュールをインポートするまで、コマンドレットは使用できません。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-132">The Azure PowerShell module is an optional install - the cmdlets aren't available until you import the module.</span></span>

### <a name="install-the-azurerm-module"></a><span data-ttu-id="dd8d6-133">AzureRM モジュールをインストールする</span><span class="sxs-lookup"><span data-stu-id="dd8d6-133">Install the AzureRM module</span></span>

<span data-ttu-id="dd8d6-134">AzureRM モジュールは、PowerShell ギャラリーと呼ばれるグローバル リポジトリから入手できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-134">The AzureRM module is available from a global repository called the PowerShell Gallery.</span></span> <span data-ttu-id="dd8d6-135">`Install-Module` コマンドを使用して、ローカル コンピューター上にモジュールをインストールすることができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-135">You can install the module onto your local machine through the `Install-Module` command.</span></span> <span data-ttu-id="dd8d6-136">PowerShell ギャラリーからモジュールをインストールするには、管理者特権の PowerShell シェルが必要です。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-136">You need an elevated PowerShell shell to install modules from the PowerShell Gallery.</span></span> 

<span data-ttu-id="dd8d6-137">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="dd8d6-137">::: zone pivot="windows"</span></span>

<span data-ttu-id="dd8d6-138">最新の Azure PowerShell モジュールをインストールするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-138">To install the latest Azure PowerShell module, run the following commands:</span></span>

1. <span data-ttu-id="dd8d6-139">**[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-139">Open the **Start** menu and type **Windows PowerShell**.</span></span>

1. <span data-ttu-id="dd8d6-140">**[Windows PowerShell]** アイコンを右クリックし、**[管理者として実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-140">Right-click the **Windows PowerShell** icon and select **Run as administrator**.</span></span>

1. <span data-ttu-id="dd8d6-141">**[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-141">In the **User Account Control** dialog, select **Yes**.</span></span>

1. <span data-ttu-id="dd8d6-142">次のコマンドを入力して、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-142">Type the following command, and then press Enter:</span></span>

    ```powershell
    Install-Module -Name AzureRM
    ```

<span data-ttu-id="dd8d6-143">これにより、既定ですべてのユーザーに対してモジュールがインストールされます (スコープ パラメーターで制御)。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-143">This installs the module for all users by default (controlled by the scope parameter).</span></span> 

<span data-ttu-id="dd8d6-144">コマンドでは NuGet を利用してコンポーネントを取得します。インストールした NuGet のバージョンによっては、最新バージョンの NuGet をダウンロードしてインストールするように求められる場合があります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-144">The command relies on NuGet to retrieve components, depending on the version of NuGet you have installed you might get a prompt to download and install the latest version of NuGet.</span></span>

```output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
 provider must be available in 'C:\Program Files (x86)\PackageManagement\ProviderAssemblies' or
'C:\Users\<username>\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
 the NuGet provider now?
```

<span data-ttu-id="dd8d6-145">既定では、PowerShell ギャラリーは、PowerShellGet 用の信頼できるリポジトリとしては構成されません。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-145">By default, the PowerShell gallery isn't configured as a trusted repository for PowerShellGet.</span></span> <span data-ttu-id="dd8d6-146">PSGallery の初回使用時には、次のプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-146">The first time you use the PSGallery you see the following prompt:</span></span>

```output
You are installing the modules from an untrusted repository. If you trust this repository, change its
InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
'PSGallery'?
```

#### <a name="script-execution-failed"></a><span data-ttu-id="dd8d6-147">スクリプトの実行に失敗しました</span><span class="sxs-lookup"><span data-stu-id="dd8d6-147">Script execution failed</span></span>
<span data-ttu-id="dd8d6-148">セキュリティの構成によっては、`Import-Module` が失敗し、次のように表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-148">Depending on your security configuration, `Import-Module` might fail with something like the following.</span></span>

```output
import-module : File C:\Program Files (x86)\WindowsPowerShell\Modules\azurerm\6.8.1\AzureRM.psm1 cannot be loaded
because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ import-module azurerm
+ ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [Import-Module], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess,Microsoft.PowerShell.Commands.ImportModuleCommand
```

<span data-ttu-id="dd8d6-149">これは通常、実行ポリシーが "restricted" であることを示しており、PowerShell ギャラリーを含む、外部ソースからダウンロードするモジュールを実行できないことを意味します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-149">This normally indicates that the execution policy is "restricted", meaning you can't execute modules you download from an external source - including the PowerShell gallery.</span></span> <span data-ttu-id="dd8d6-150">これに該当するかどうかは、`Get-ExecutionPolicy` というコマンドを実行することで確認できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-150">You can check whether this is the case by executing the command `Get-ExecutionPolicy`.</span></span> <span data-ttu-id="dd8d6-151">"Restricted" が返された場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-151">If it returns "Restricted", then do the following:</span></span>

1. <span data-ttu-id="dd8d6-152">管理者特権の PowerShell コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-152">Open an elevated PowerShell command prompt.</span></span>
1. <span data-ttu-id="dd8d6-153">`SetExecutionPolicy` コマンドレットを使用して、ポリシーを "RemoteSigned" に変更します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-153">Use the `SetExecutionPolicy` cmdlet to change the policy to "RemoteSigned":</span></span>

```powershell
Set-ExecutionPolicy RemoteSigned
```

<span data-ttu-id="dd8d6-154">これにより、アクセス許可が求められます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-154">This will prompt you for permission:</span></span>

```output
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

<span data-ttu-id="dd8d6-155">その後、`Import-Module` を使用して、コマンドレットを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-155">You should then be able to use `Import-Module` to load the cmdlets.</span></span>

<span data-ttu-id="dd8d6-156">:::zone-end</span><span class="sxs-lookup"><span data-stu-id="dd8d6-156">:::zone-end</span></span>

<span data-ttu-id="dd8d6-157">::: zone pivot="linux,macos"</span><span class="sxs-lookup"><span data-stu-id="dd8d6-157">::: zone pivot="linux,macos"</span></span>

<span data-ttu-id="dd8d6-158">同じコマンドを使用して、Linux または macOS に Azure PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-158">We use the same commands to install the Azure PowerShell on either Linux or macOS.</span></span>

1. <span data-ttu-id="dd8d6-159">ターミナルで次のコマンドを入力し、管理者特権で PowerShell Core を起動します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-159">In a terminal, type the following command to launch PowerShell Core with elevated privileges.</span></span>

    ```bash
    sudo pwsh
    ```

1. <span data-ttu-id="dd8d6-160">PowerShell Core プロンプトで次のコマンドを実行して、Azure PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-160">Run the following command at the PowerShell Core prompt to install Azure PowerShell.</span></span>

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. <span data-ttu-id="dd8d6-161">**PSGallery** からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** で答えます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-161">If you are asked whether you trust modules from **PSGallery**, answer **Yes** or **Yes to All**.</span></span>

<span data-ttu-id="dd8d6-162">:::zone-end</span><span class="sxs-lookup"><span data-stu-id="dd8d6-162">:::zone-end</span></span>

### <a name="update-a-module"></a><span data-ttu-id="dd8d6-163">モジュールを更新する</span><span class="sxs-lookup"><span data-stu-id="dd8d6-163">Update a module</span></span>

<span data-ttu-id="dd8d6-164">Azure PowerShell モジュールの何らかのバージョンが既にインストールされていることを示す警告またはエラー メッセージが表示された場合は、次のコマンドを実行して、_最新_のバージョンに更新できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-164">If you get a warning or error message indicating that a version of the Azure PowerShell module is already installed, you can update to the _latest_ version by issuing the command:</span></span>

<span data-ttu-id="dd8d6-165">:::zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="dd8d6-165">:::zone pivot="windows"</span></span>

```powershell
Update-Module -Name AzureRM
```

<span data-ttu-id="dd8d6-166">:::zone-end</span><span class="sxs-lookup"><span data-stu-id="dd8d6-166">:::zone-end</span></span>

<span data-ttu-id="dd8d6-167">::: zone pivot="linux,macos"</span><span class="sxs-lookup"><span data-stu-id="dd8d6-167">::: zone pivot="linux,macos"</span></span>

```powershell
Update-Module -Name AzureRM.NetCore
```

<span data-ttu-id="dd8d6-168">:::zone-end</span><span class="sxs-lookup"><span data-stu-id="dd8d6-168">:::zone-end</span></span>

<span data-ttu-id="dd8d6-169">`Install-Module` コマンドと同じように、モジュールの信頼の確認を求められたら **[はい]** または **[すべてはい]** で答えます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-169">As with the `Install-Module` command, answer **Yes** or **Yes to All** when prompted to trust the module.</span></span> <span data-ttu-id="dd8d6-170">問題がある場合は、`Update-Module` コマンドを使用してモジュールを再インストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-170">You can also use the `Update-Module` command to re-install a module if you are having trouble with it.</span></span>

## <a name="example-how-to-create-a-resource-group-with-azure-powershell"></a><span data-ttu-id="dd8d6-171">例: Azure PowerShell を使用してリソース グループを作成する方法</span><span class="sxs-lookup"><span data-stu-id="dd8d6-171">Example: How to create a resource group with Azure PowerShell</span></span>
<span data-ttu-id="dd8d6-172">Azure モジュールを読み込んだら、Azure の操作を開始できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-172">Once you have the Azure module loaded, you can begin working with Azure.</span></span> <span data-ttu-id="dd8d6-173">ここでは一般的なタスク (リソース グループの作成) を行います。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-173">Let's do a common task - create a Resource Group.</span></span> <span data-ttu-id="dd8d6-174">ご存知のように、関連リソースをまとめて管理するにはリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-174">As you know, we use resource groups to administer related resources together.</span></span> <span data-ttu-id="dd8d6-175">新しいリソース グループの作成は、新しい Azure ソリューションを開始するときに行う最初のタスクの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-175">Creating a new resource group is one of the first tasks you'll do when starting a new Azure solution.</span></span>

<span data-ttu-id="dd8d6-176">行う必要がある手順は次の 4 つです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-176">There are four steps we need to perform:</span></span>

1. <span data-ttu-id="dd8d6-177">Azure コマンドレットをインポートします。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-177">Import the Azure cmdlets.</span></span>

1. <span data-ttu-id="dd8d6-178">Azure サブスクリプションに接続します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-178">Connect to your Azure subscription.</span></span>

1. <span data-ttu-id="dd8d6-179">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-179">Create the resource group.</span></span>

1. <span data-ttu-id="dd8d6-180">正常に作成できたことを確認します (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-180">Verify that creation was successful (see below).</span></span>

<span data-ttu-id="dd8d6-181">次の図では、この手順の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-181">The following illustration shows an overview of these steps.</span></span>

![リソース グループを作成する手順を示す図。](../media/5-create-resource-overview.png)

<span data-ttu-id="dd8d6-183">各手順は異なるコマンドレットに対応しています。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-183">Each step corresponds to a different cmdlet.</span></span>

### <a name="import-the-azure-cmdlets"></a><span data-ttu-id="dd8d6-184">Azure コマンドレットをインポートする</span><span class="sxs-lookup"><span data-stu-id="dd8d6-184">Import the Azure cmdlets</span></span>
<span data-ttu-id="dd8d6-185">PowerShell では、起動時に中心的なコマンドレットのみが既定で読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-185">At startup, PowerShell loads only the core cmdlets by default.</span></span> <span data-ttu-id="dd8d6-186">つまり、Azure で使用する必要があるコマンドレットは読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-186">This means the cmdlets you need to work with Azure won't be loaded.</span></span> <span data-ttu-id="dd8d6-187">必要なコマンドレットを最も確実に読み込む方法は、PowerShell セッションの開始時にコマンドレットを手動でインポートすることです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-187">The most reliable way to load the cmdlets you need is to import them manually at the start of your PowerShell session.</span></span>

<span data-ttu-id="dd8d6-188">モジュールの読み込みには **Import-Module** コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-188">You use the **Import-Module** cmdlet to load modules.</span></span> <span data-ttu-id="dd8d6-189">このコマンドレットには、さまざまな状況に対処するためのパラメーターがたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-189">This cmdlet has many parameters to handle a variety of situations.</span></span> <span data-ttu-id="dd8d6-190">たとえば、複数のモジュール、特定のバージョンのモジュール、モジュールの一部などを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-190">For example, it can load multiple modules, a specific module version, part of a module, and so on.</span></span>

<span data-ttu-id="dd8d6-191">たとえば、**管理者特権の PowerShell セッションで**は、次のコマンドで AzureRM 用のすべてのコマンドレットを読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-191">For example, we can load all the cmdlets for AzureRM with the following command **in an elevated PowerShell session**:</span></span>

<span data-ttu-id="dd8d6-192">:::zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="dd8d6-192">:::zone pivot="windows"</span></span>

```powershell
Import-Module AzureRM
```

<span data-ttu-id="dd8d6-193">:::zone-end</span><span class="sxs-lookup"><span data-stu-id="dd8d6-193">:::zone-end</span></span>

<span data-ttu-id="dd8d6-194">::: zone pivot="linux,macos"</span><span class="sxs-lookup"><span data-stu-id="dd8d6-194">::: zone pivot="linux,macos"</span></span>

```powershell
Import-Module AzureRM.NetCore
```

<span data-ttu-id="dd8d6-195">:::zone-end</span><span class="sxs-lookup"><span data-stu-id="dd8d6-195">:::zone-end</span></span>

> [!TIP]
> <span data-ttu-id="dd8d6-196">Azure PowerShell を頻繁に操作するようであれば、2 とおりの方法でモジュール読み込みプロセスを自動化できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-196">If you find that you work with Azure PowerShell frequently, there are two ways you can automate the module-loading process.</span></span> <span data-ttu-id="dd8d6-197">PowerShell プロファイルにエントリを追加し、起動時に Azure モジュールをインポートするか、PowerShell の最新版を使用し、コマンドレットの使用時にコマンドレットを含むモジュールを自動的に読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-197">You can add an entry to your PowerShell profile to import the Azure module at startup or use the latest versions of PowerShell, which loads the containing module automatically when you use a cmdlet.</span></span>

### <a name="connect"></a><span data-ttu-id="dd8d6-198">接続する</span><span class="sxs-lookup"><span data-stu-id="dd8d6-198">Connect</span></span>
<span data-ttu-id="dd8d6-199">Azure PowerShell のローカル インストールを使用している場合、Azure コマンドを実行する前に認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-199">When you are working with a local install of Azure PowerShell, you will need to authenticate before you can execute Azure commands.</span></span> <span data-ttu-id="dd8d6-200">**Connect-AzureRmAccount** コマンドレットを実行すると、Azure 資格情報が求められ、その後、Azure サブスクリプションに接続されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-200">The **Connect-AzureRmAccount** cmdlet prompts for your Azure credentials and then connects to your Azure subscription.</span></span> <span data-ttu-id="dd8d6-201">さまざまなオプション パラメーターがありますが、対話型プロンプトのみが必要な場合、パラメーターは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-201">It has many optional parameters, but if all you need is an interactive prompt, no parameters are needed:</span></span>

```powershell
Connect-AzureRmAccount
```

<span data-ttu-id="dd8d6-202">このモジュールはコア セットの一部ではないため、新しい PowerShell セッションを開始するたびに、この手順を繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-202">You'll need to repeat these steps for every new PowerShell session you start since this module is not part of the core set.</span></span>


### <a name="working-with-subscriptions"></a><span data-ttu-id="dd8d6-203">サブスクリプションの使用</span><span class="sxs-lookup"><span data-stu-id="dd8d6-203">Working with subscriptions</span></span>
<span data-ttu-id="dd8d6-204">Azure の利用が初めての場合、所有しているサブスクリプションは 1 つのみと思われます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-204">If you are new to Azure, you probably only have a single subscription.</span></span> <span data-ttu-id="dd8d6-205">しかし、Azure をしばらく利用している場合は、複数の Azure サブスクリプションを作成した可能性があります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-205">But if you have been using Azure for a while, you may have created multiple Azure subscriptions.</span></span> <span data-ttu-id="dd8d6-206">特定のサブスクリプションに対してコマンドを実行するように Azure PowerShell を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-206">You can configure Azure PowerShell to execute commands against a particular subscription.</span></span>

<span data-ttu-id="dd8d6-207">これは一度に 1 つのサブスクリプションでのみ行うことができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-207">You can only be in one subscription at a time.</span></span> <span data-ttu-id="dd8d6-208">アクティブなサブスクリプションを判別するには、`Get-AzureRmContext` コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-208">Use the `Get-AzureRmContext` cmdlet to determine which subscription is active.</span></span> <span data-ttu-id="dd8d6-209">これが適切でない場合は、変更できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-209">If it's not the correct one, you can change it.</span></span>

1. <span data-ttu-id="dd8d6-210">`Get-AzureRmSubscription` コマンドで、アカウントのすべてのサブスクリプション名のリストを取得します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-210">Get a list of all subscription names in your account with the `Get-AzureRmSubscription` command.</span></span> 

2. <span data-ttu-id="dd8d6-211">選択するサブスクリプションの名前を渡して、そのサブスクリプションを変更します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-211">Change the subscription by passing the name of the one to select.</span></span>

```powershell
Select-AzureRmSubscription -Subscription "Visual Studio Enterprise"
```

### <a name="get-a-list-of-all-resource-groups"></a><span data-ttu-id="dd8d6-212">すべてのリソース グループのリストを取得する</span><span class="sxs-lookup"><span data-stu-id="dd8d6-212">Get a list of all Resource Groups</span></span>

<span data-ttu-id="dd8d6-213">アクティブなサブスクリプションのすべてのリソース グループのリストを取得できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-213">You can retrieve a list of all Resource Groups in the active subscription:</span></span>

```powershell
Get-AzureRmResourceGroup
```

<span data-ttu-id="dd8d6-214">より簡潔なリストを表示するには、パイプ '|' を使用して `Get-AzureRmResourceGroup` から `Format-Table` コマンドレットに出力を送信できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-214">To get a more concise view, you can send the output from the `Get-AzureRmResourceGroup` to the `Format-Table` cmdlet using a pipe '|'.</span></span>

```powershell
Get-AzureRmResourceGroup | Format-Table
```

<span data-ttu-id="dd8d6-215">その出力は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-215">This will output something like:</span></span>

```output
ResourceGroupName                  Location       ProvisioningState Tags TagsTable ResourceId
-----------------                  --------       ----------------- ---- --------- ----------
cloud-shell-storage-southcentralus southcentralus Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
ExerciseResources                  eastus         Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
```

### <a name="create-a-resource-group"></a><span data-ttu-id="dd8d6-216">リソース グループを作成する</span><span class="sxs-lookup"><span data-stu-id="dd8d6-216">Create a Resource Group</span></span>

<span data-ttu-id="dd8d6-217">ご存知のように、Azure でリソースを作成する場合は、常に管理目的でリソース グループにリソースを配置します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-217">As you know, when you are creating resources in Azure, you will always place them into a resource group for management purposes.</span></span> <span data-ttu-id="dd8d6-218">リソース グループは、多くの場合、新しいアプリケーションを開始するときに作成する最初のものの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-218">A resource group is often one of the first things you will create when starting a new application.</span></span>

<span data-ttu-id="dd8d6-219">`New-AzureRmResourceGroup` コマンドレットを使用して、リソース グループを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-219">You can create resource groups with the `New-AzureRmResourceGroup` cmdlet.</span></span> <span data-ttu-id="dd8d6-220">名前と場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-220">You must specify a name and location.</span></span> <span data-ttu-id="dd8d6-221">この名前はサブスクリプション内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-221">The name must be unique within your subscription.</span></span> <span data-ttu-id="dd8d6-222">この場所によって、お使いのリソース グループのメタデータが保存される場所が決定されます (コンプライアンス上の理由から重要となる場合があります)。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-222">The location determines where the metadata for your resource group will be stored (which may be important to you for compliance reasons).</span></span> <span data-ttu-id="dd8d6-223">"West US"、"North Europe"、"West India" などの文字列を使用して場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-223">You use strings like "West US", "North Europe", or "West India" to specify the location.</span></span> <span data-ttu-id="dd8d6-224">ほとんどの Azure コマンドレットと同様に、`New-AzureRmResourceGroup` には多くのオプション パラメーターがありますが、中心的な構文は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-224">As with most of the Azure cmdlets, `New-AzureRmResourceGroup` has many optional parameters; however, the core syntax is:</span></span>

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

> [!NOTE]
> <span data-ttu-id="dd8d6-225">リソース グループが自動的に作成される Azure サンドボックスで作業を行うことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-225">Remember, we will be working in the Azure Sandbox which creates the Resource Group for you.</span></span> <span data-ttu-id="dd8d6-226">上記のコマンドは、ご自身のサブスクリプションで作業を行う場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-226">The above command would be used if you work in your own subscription.</span></span>

### <a name="verify-the-resources"></a><span data-ttu-id="dd8d6-227">リソースを確認する</span><span class="sxs-lookup"><span data-stu-id="dd8d6-227">Verify the resources</span></span>
<span data-ttu-id="dd8d6-228">`Get-AzureRmResource` では Azure リソースがリストされます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-228">The `Get-AzureRmResource` lists your Azure resources.</span></span> <span data-ttu-id="dd8d6-229">リソース グループが正常に作成されたことを確認するためにここで役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-229">This is useful here to verify whether creation of the resource group was successful.</span></span>

```powershell
Get-AzureRmResource
```

<span data-ttu-id="dd8d6-230">`Get-AzureRmResourceGroup` コマンドと同様に、`Format-Table` コマンドレットを使用してより簡潔なリストを表示できます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-230">Like the `Get-AzureRmResourceGroup` command, you can get a more concise view through the `Format-Table` cmdlet.</span></span> <span data-ttu-id="dd8d6-231">ここでは、短縮版の `ft` を使用します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-231">Here we will use a shorthand version `ft`:</span></span>

```powershell
Get-AzureRmResource | ft
```

<span data-ttu-id="dd8d6-232">これで特定のリソース グループに絞り込み、そのグループに関連付けられているリソースのみをリストすることもできます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-232">You can also filter it to specific resource groups to only list resources associated with that group:</span></span>

```powershell
Get-AzureRmResource -ResourceGroup ExerciseResources
```

### <a name="creating-an-azure-virtual-machine"></a><span data-ttu-id="dd8d6-233">Azure 仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="dd8d6-233">Creating an Azure Virtual Machine</span></span>

<span data-ttu-id="dd8d6-234">PowerShell で行うことができる一般的なタスクには、VM の作成もあります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-234">Another common task that could be done with PowerShell is to create VMs.</span></span>

<span data-ttu-id="dd8d6-235">Azure PowerShell では、仮想マシンを作成するための `New-AzureRmVm` コマンドレットが提供されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-235">Azure PowerShell provides the `New-AzureRmVm` cmdlet to create a virtual machine.</span></span> <span data-ttu-id="dd8d6-236">コマンドレットには多くのパラメーターがあり、それを使用して数多くの VM 構成設定を処理することができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-236">The cmdlet has many parameters to let it handle the large number of VM configuration settings.</span></span> <span data-ttu-id="dd8d6-237">ほとんどのパラメーターに適切な既定値があるため、指定する必要があるのは次の 5 つのみです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-237">Most of the parameters have reasonable default values so we only need to specify five things:</span></span>

- <span data-ttu-id="dd8d6-238">**ResourceGroupName**: 新しい VM が配置されるリソース グループ。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-238">**ResourceGroupName**: The resource group into which the new VM will be placed.</span></span>
- <span data-ttu-id="dd8d6-239">**Name**: Azure での VM の名前。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-239">**Name**: The name of the VM in Azure.</span></span>
- <span data-ttu-id="dd8d6-240">**Location**: VM がプロビジョニングされる地理的な場所。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-240">**Location**: Geographic location where the VM will be provisioned.</span></span>
- <span data-ttu-id="dd8d6-241">**Credential**: VM 管理者アカウント用のユーザー名とパスワードが含まれているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-241">**Credential**: An object containing the username and password for the VM admin account.</span></span> <span data-ttu-id="dd8d6-242">ここでは `Get-Credential` コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-242">We will use the `Get-Credential` cmdlet.</span></span> <span data-ttu-id="dd8d6-243">このコマンドレットでは、ユーザー名とパスワードが求められ、それが資格情報オブジェクトにパッケージ化されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-243">This cmdlet will prompt for a username and password and package it into a credential object.</span></span>
- <span data-ttu-id="dd8d6-244">**Image**: VM で使用するオペレーティング システム イメージです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-244">**Image**: The operating system image to use for the VM.</span></span> <span data-ttu-id="dd8d6-245">これは多くの場合、Linux ディストリビューション、または Windows Server となります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-245">This is often a Linux distribution, or Windows Server.</span></span>

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

<span data-ttu-id="dd8d6-246">上記のように、これらのパラメーターをコマンドレットに直接指定することができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-246">You can supply these parameters directly to the cmdlet as shown above.</span></span> <span data-ttu-id="dd8d6-247">あるいは、`Set-AzureRmVMOperatingSystem`、`Set-AzureRmVMSourceImage`、`Add-AzureRmVMNetworkInterface`、`Set-AzureRmVMOSDisk` などの他のコマンドレットを使用して、仮想マシンを構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-247">Alternatively, other cmdlets can be used to configure the virtual machine, such as `Set-AzureRmVMOperatingSystem`, `Set-AzureRmVMSourceImage`, `Add-AzureRmVMNetworkInterface`, and `Set-AzureRmVMOSDisk`.</span></span>

<span data-ttu-id="dd8d6-248">`Get-Credential` コマンドレットと `-Credential` パラメーターを一緒に使用する例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-248">Here's an example that strings the `Get-Credential` cmdlet together with the `-Credential` parameter:</span></span>

```powershell
New-AzureRmVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...
```

<span data-ttu-id="dd8d6-249">`AzureRmVM` サフィックスは、PowerShell の VM ベースのコマンドに固有のものです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-249">The `AzureRmVM` suffix is specific to VM-based commands in PowerShell.</span></span> <span data-ttu-id="dd8d6-250">使用できるものは他にもいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-250">There are several others you can use:</span></span>

| <span data-ttu-id="dd8d6-251">コマンド</span><span class="sxs-lookup"><span data-stu-id="dd8d6-251">Command</span></span> | <span data-ttu-id="dd8d6-252">説明</span><span class="sxs-lookup"><span data-stu-id="dd8d6-252">Description</span></span> |
|---------|-------------|
| `Remove-AzureRmVM` | <span data-ttu-id="dd8d6-253">Azure VM が削除されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-253">Deletes an Azure VM.</span></span> |
| `Start-AzureRmVM` | <span data-ttu-id="dd8d6-254">停止した VM を起動します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-254">Start a stopped VM.</span></span> |
| `Stop-AzureRmVM` | <span data-ttu-id="dd8d6-255">実行中の VM を停止します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-255">Stop a running VM.</span></span> |
| `Restart-AzureRmVM` | <span data-ttu-id="dd8d6-256">VM を再起動します。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-256">Restart a VM.</span></span> |
| `Update-AzureRmVM` | <span data-ttu-id="dd8d6-257">VM の構成が更新されます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-257">Updates the configuration for a VM.</span></span> |

#### <a name="example-getting-the-information-for-a-vm"></a><span data-ttu-id="dd8d6-258">例: VM の情報の取得</span><span class="sxs-lookup"><span data-stu-id="dd8d6-258">Example: getting the information for a VM</span></span>

<span data-ttu-id="dd8d6-259">`Get-AzureRmVM -Status` コマンドを使用して、サブスクリプション内の VM をリストすることができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-259">You can list the VMs in your subscription with the `Get-AzureRmVM -Status` command.</span></span> <span data-ttu-id="dd8d6-260">`-Name` プロパティで VM を指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-260">This can also specify a VM with the `-Name` property.</span></span> <span data-ttu-id="dd8d6-261">ここでは、PowerShell 変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-261">Here we assign it to a PowerShell variable:</span></span>

```powershell
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName ExerciseResources
```

<span data-ttu-id="dd8d6-262">興味深いことは、これがやりとりできる_オブジェクト_であることです。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-262">The interesting thing is this is an _object_ you can interact with.</span></span> <span data-ttu-id="dd8d6-263">たとえば、そのオブジェクトを取得し、変更を加えてから、`Update-AzureRmVM` コマンドを使用してその変更を Azure にプッシュして戻すことができます。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-263">For example, you can take that object, make changes and then push changes back to Azure with the `Update-AzureRmVM` command:</span></span>

```powershell
$ResourceGroupName = "ExerciseResources"
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName $ResourceGroupName
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"

Update-AzureRmVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

<span data-ttu-id="dd8d6-264">PowerShell の対話モードは 1 回限りのタスクに適しています。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-264">PowerShell's interactive mode is appropriate for one-off tasks.</span></span> <span data-ttu-id="dd8d6-265">この例では、プロジェクトのライフタイムに対して同じリソース グループを使用することが考えられます。つまり、対話形式で作成するのが妥当です。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-265">In our example, we'll likely use the same resource group for the lifetime of the project, which means creating it interactively is reasonable.</span></span> <span data-ttu-id="dd8d6-266">このタスクでは多くの場合、スクリプトを記述し、そのスクリプトを 1 回だけ実行するよりも、対話モードの方が早くて簡単です。</span><span class="sxs-lookup"><span data-stu-id="dd8d6-266">Interactive mode is often quicker and easier for this task than writing a script and executing that script exactly once.</span></span>