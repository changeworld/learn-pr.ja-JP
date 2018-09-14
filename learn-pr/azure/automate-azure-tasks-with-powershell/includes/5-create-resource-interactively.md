<span data-ttu-id="97da1-101">PowerShell は、コマンドを記述し、すぐに実行できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-101">PowerShell lets you write commands and execute them immediately.</span></span> <span data-ttu-id="97da1-102">これは**対話モード**と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="97da1-102">This is known as **interactive mode**.</span></span>

<span data-ttu-id="97da1-103">顧客関係管理 (CRM) の例の全体的な目標は VM を含む 3 つのテスト環境を作成することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="97da1-103">Recall that the overall goal in the Customer Relationship Management (CRM) example is to create three test environments containing VMs.</span></span> <span data-ttu-id="97da1-104">リソース グループを利用して VM を別々の環境に整理します。単体テスト用に 1 つ、統合テスト用に 1 つ、受け入れテスト用に 1 つとなります。</span><span class="sxs-lookup"><span data-stu-id="97da1-104">You will use resource groups to ensure the VMs are organized into separate environments: one for unit testing, one for integration testing, and one for acceptance testing.</span></span> <span data-ttu-id="97da1-105">リソース グループは一度だけ作成すれば十分です。そこで、PowerShell の対話モードを使用することが良い選択肢となります。</span><span class="sxs-lookup"><span data-stu-id="97da1-105">You only need to create the resource groups once, which means using the interactive mode of PowerShell is a good choice.</span></span>

<span data-ttu-id="97da1-106">一致を PowerShell コマンドを入力すると、_コマンドレット_し、要求されたアクションを実行します。</span><span class="sxs-lookup"><span data-stu-id="97da1-106">When you type a command into PowerShell, it matches it to a _cmdlet_ which then performs the requested action.</span></span> <span data-ttu-id="97da1-107">を使用して PowerShell の Azure サポートのインストールを確認し、一般的なコマンドのいくつか見ていきます。</span><span class="sxs-lookup"><span data-stu-id="97da1-107">We're going to look at some of the common commands you can use, and then look at installing the Azure support for PowerShell.</span></span>

## <a name="what-are-powershell-cmdlets"></a><span data-ttu-id="97da1-108">PowerShell コマンドレットとは</span><span class="sxs-lookup"><span data-stu-id="97da1-108">What are PowerShell cmdlets?</span></span>
<span data-ttu-id="97da1-109">PowerShell コマンドは**コマンドレット**と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="97da1-109">A PowerShell command is called a **cmdlet** (pronounced "command-let").</span></span> <span data-ttu-id="97da1-110">コマンドレットは 1 つの機能を操作するコマンドです。</span><span class="sxs-lookup"><span data-stu-id="97da1-110">A cmdlet is a command that manipulates a single feature.</span></span> <span data-ttu-id="97da1-111">**コマンドレット**と用語は "小さなコマンド" を意味します。</span><span class="sxs-lookup"><span data-stu-id="97da1-111">The term **cmdlet** is intended to imply "small command".</span></span> <span data-ttu-id="97da1-112">慣例で、コマンドレットの作成者には、コマンドレットをシンプルにすること、目的を 1 つだけにすることが推奨されています。</span><span class="sxs-lookup"><span data-stu-id="97da1-112">By convention, cmdlet authors are encouraged to keep cmdlets simple and single-purpose.</span></span>

<span data-ttu-id="97da1-113">基本の PowerShell 製品には、セッションやバックグラウンド ジョブなどの機能で使用できるコマンドレットが付属します。</span><span class="sxs-lookup"><span data-stu-id="97da1-113">The base PowerShell product ships with cmdlets that work with features such as sessions and background jobs.</span></span> <span data-ttu-id="97da1-114">他の機能を操作するコマンドレットを得るには、PowerShell インストールにモジュールを追加します。</span><span class="sxs-lookup"><span data-stu-id="97da1-114">You add modules to your PowerShell installation to get cmdlets that manipulate other features.</span></span> <span data-ttu-id="97da1-115">たとえば、サードパーティ モジュールには、FTP と連動するモジュール、オペレーティング システムを管理するモジュール、ファイル システムにアクセスするモジュールなどがあります。</span><span class="sxs-lookup"><span data-stu-id="97da1-115">For example, there are third-party modules to work with ftp, administer your operating system, access the file system, and so on.</span></span>

<span data-ttu-id="97da1-116">コマンドレットには、動詞-名詞の命名規則が採用されています。たとえば、**Get-Process**、**Format-Table**、**Start-Service** のようになります。</span><span class="sxs-lookup"><span data-stu-id="97da1-116">Cmdlets follow a verb-noun naming convention; for example, **Get-Process**, **Format-Table**, and **Start-Service**.</span></span> <span data-ttu-id="97da1-117">動詞の選択にも規則があります。たとえば、データの取得は "get"、データの挿入または更新は "set"、データの書式設定は "format"、宛先への直接出力は "out" になります。</span><span class="sxs-lookup"><span data-stu-id="97da1-117">There is also a convention for verb choice: "get" to retrieve data, "set" to insert or update data, "format" to format data, "out" to direct output to a destination, and so on.</span></span>

<span data-ttu-id="97da1-118">コマンドレットの作成者には、コマンドレットごとにヘルプ ファイルを含めることが推奨されています。</span><span class="sxs-lookup"><span data-stu-id="97da1-118">Cmdlet authors are encouraged to include a help file for each cmdlet.</span></span> <span data-ttu-id="97da1-119">**Get-help**コマンドレットは、いずれかのコマンドレットのヘルプ ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="97da1-119">The **Get-Help** cmdlet displays the help file for any cmdlet.</span></span> <span data-ttu-id="97da1-120">などヘルプを取得でした、`Get-ChildItem`コマンドレットは、次のステートメントを使用します。</span><span class="sxs-lookup"><span data-stu-id="97da1-120">For example, we could get help on the `Get-ChildItem` cmdlet with the following statement:</span></span>

```powershell
Get-Help Get-ChildItem -detailed
```

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="97da1-121">PowerShell モジュールとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="97da1-121">What is a PowerShell module?</span></span>

<span data-ttu-id="97da1-122">コマンドレットが付属して_モジュール_します。</span><span class="sxs-lookup"><span data-stu-id="97da1-122">Cmdlets are shipped in _modules_.</span></span> <span data-ttu-id="97da1-123">PowerShell モジュールは、使用可能な各コマンドレットのプロセスにコードを含む DLL です。</span><span class="sxs-lookup"><span data-stu-id="97da1-123">A PowerShell Module is a DLL that includes the code to proces each available cmdlet.</span></span> <span data-ttu-id="97da1-124">内のモジュールを読み込むには、PowerShell にコマンドレットを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="97da1-124">You load cmdlets into PowerShell by loading the module they are contained in.</span></span> <span data-ttu-id="97da1-125">使用して読み込まれたモジュールの一覧を取得することができます、`Get-Module`コマンド。</span><span class="sxs-lookup"><span data-stu-id="97da1-125">You can get a list of loaded modules using the `Get-Module` command:</span></span>

```powershell
Get-Module
```

<span data-ttu-id="97da1-126">これは、以下のような出力します。</span><span class="sxs-lookup"><span data-stu-id="97da1-126">This will output something like:</span></span>

```output
ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Find-PackageProvider, Get-Package, Get-Pack...
Script     1.0.0.1    PowerShellGet                       {Find-Command, Find-DscResource, Find-Module, Find-RoleCap...
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

## <a name="what-is-azurerm"></a><span data-ttu-id="97da1-127">AzureRM とは</span><span class="sxs-lookup"><span data-stu-id="97da1-127">What is AzureRM?</span></span>
<span data-ttu-id="97da1-128">**AzureRM** は、Azure 機能で使用できるコマンドレットを含む Azure PowerShell コマンドレットの正式名称です (名前の中の **RM** は **Resource Manager** という意味です)。</span><span class="sxs-lookup"><span data-stu-id="97da1-128">**AzureRM** is the formal name for the Azure PowerShell module containing cmdlets to work with Azure features (the **RM** in the name stands for **Resource Manager**).</span></span> <span data-ttu-id="97da1-129">数百単位のコマンドレットが含まれ、あらゆる Azure リソースのほぼすべての面を制御できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-129">It contains hundreds of cmdlets that let you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="97da1-130">リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-130">You can work with resource groups, storage, virtual machines, Azure Active Directory, containers, machine learning, and so on.</span></span> <span data-ttu-id="97da1-131">このモジュールは、オープン ソース コンポーネントで[GitHub で入手できます](https://github.com/Azure/azure-powershell)します。</span><span class="sxs-lookup"><span data-stu-id="97da1-131">This module is an open source component [available on GitHub](https://github.com/Azure/azure-powershell).</span></span>

> [!NOTE]
> <span data-ttu-id="97da1-132">Azure PowerShell モジュールは、オプションのインストール - コマンドレットは、モジュールをインポートするまで使用できません。</span><span class="sxs-lookup"><span data-stu-id="97da1-132">The Azure PowerShell module is an optional install - the cmdlets aren't available until you import the module.</span></span>

### <a name="install-the-azurerm-module"></a><span data-ttu-id="97da1-133">AzureRM モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="97da1-133">Install the AzureRM module</span></span>

<span data-ttu-id="97da1-134">AzureRM モジュールは、PowerShell ギャラリーと呼ばれるグローバル リポジトリから使用できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-134">The AzureRM module is available from a global repository called the PowerShell Gallery.</span></span> <span data-ttu-id="97da1-135">使ってローカル コンピューター上に、モジュールをインストールすることができます、`Install-Module`コマンド。</span><span class="sxs-lookup"><span data-stu-id="97da1-135">You can install the module onto your local machine through the `Install-Module` command.</span></span> <span data-ttu-id="97da1-136">管理者特権で PowerShell シェルを PowerShell ギャラリーからモジュールをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="97da1-136">You need an elevated PowerShell shell to install modules from the PowerShell Gallery.</span></span> 

<span data-ttu-id="97da1-137">::: zone pivot="windows"</span><span class="sxs-lookup"><span data-stu-id="97da1-137">::: zone pivot="windows"</span></span>

<span data-ttu-id="97da1-138">最新の Azure PowerShell モジュールをインストールするには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="97da1-138">To install the latest Azure PowerShell module, run the following commands:</span></span>

1. <span data-ttu-id="97da1-139">**[スタート]** メニューを開き、「**Windows PowerShell**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="97da1-139">Open the **Start** menu and type **Windows PowerShell**.</span></span>

1. <span data-ttu-id="97da1-140">**[Windows PowerShell]** アイコンを右クリックし、**[管理者として実行]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="97da1-140">Right-click the **Windows PowerShell** icon and select **Run as administrator**.</span></span>

1. <span data-ttu-id="97da1-141">**[ユーザー アカウント制御]** ダイアログで、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="97da1-141">In the **User Account Control** dialog, select **Yes**.</span></span>

1. <span data-ttu-id="97da1-142">次のコマンドを入力して、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="97da1-142">Type the following command, and then press Enter:</span></span>

    ```powershell
    Install-Module -Name AzureRM
    ```

<span data-ttu-id="97da1-143">これにより、既定で (スコープのパラメーターによって制御されます) すべてのユーザーに対してモジュールがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="97da1-143">This installs the module for all users by default (controlled by the scope parameter).</span></span> 

<span data-ttu-id="97da1-144">コマンドは、コンポーネントを取得する NuGet に依存しています、インストールされている NuGet のバージョンに応じてをダウンロードして、最新バージョンの NuGet のインストールを求めるメッセージを取得する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="97da1-144">The command relies on NuGet to retrieve components, depending on the version of NuGet you have installed you might get a prompt to download and install the latest version of NuGet.</span></span>

```output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
 provider must be available in 'C:\Program Files (x86)\PackageManagement\ProviderAssemblies' or
'C:\Users\<username>\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
 the NuGet provider now?
```

<span data-ttu-id="97da1-145">既定では、PowerShell ギャラリーは、PowerShellGet の信頼できるリポジトリとしては構成されません。</span><span class="sxs-lookup"><span data-stu-id="97da1-145">By default, the PowerShell gallery isn't configured as a trusted repository for PowerShellGet.</span></span> <span data-ttu-id="97da1-146">PSGallery の初回使用時には、次のプロンプトが表示されます。</span><span class="sxs-lookup"><span data-stu-id="97da1-146">The first time you use the PSGallery you see the following prompt:</span></span>

```output
You are installing the modules from an untrusted repository. If you trust this repository, change its
InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
'PSGallery'?
```

#### <a name="script-execution-failed"></a><span data-ttu-id="97da1-147">スクリプトの実行に失敗しました</span><span class="sxs-lookup"><span data-stu-id="97da1-147">Script execution failed</span></span>
<span data-ttu-id="97da1-148">セキュリティ構成によって`Import-Module`で次のような失敗する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="97da1-148">Depending on your security configuration, `Import-Module` might fail with something like the following.</span></span>

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

<span data-ttu-id="97da1-149">通常、このエラーは、"restricted"、つまりモジュールが PowerShell ギャラリーなどの外部ソースからダウンロードすることを実行することはできません、実行ポリシーであることを示します。</span><span class="sxs-lookup"><span data-stu-id="97da1-149">This normally indicates that the execution policy is "restricted", meaning you can't execute modules you download from an external source - including the PowerShell gallery.</span></span> <span data-ttu-id="97da1-150">コマンドを実行して、ケースになるかどうかをチェックする`Get-ExecutionPolicy`します。</span><span class="sxs-lookup"><span data-stu-id="97da1-150">You can check whether this is the case by executing the command `Get-ExecutionPolicy`.</span></span> <span data-ttu-id="97da1-151">"Restricted"が返された場合、次を方法します。</span><span class="sxs-lookup"><span data-stu-id="97da1-151">If it returns "Restricted", then do the following:</span></span>

1. <span data-ttu-id="97da1-152">管理者特権の PowerShell コマンド プロンプトを開きます。</span><span class="sxs-lookup"><span data-stu-id="97da1-152">Open an elevated PowerShell command prompt.</span></span>
1. <span data-ttu-id="97da1-153">使用して、 `SetExecutionPolicy` "RemoteSigned"にポリシーを変更するコマンドレット。</span><span class="sxs-lookup"><span data-stu-id="97da1-153">Use the `SetExecutionPolicy` cmdlet to change the policy to "RemoteSigned":</span></span>

```powershell
Set-ExecutionPolicy RemoteSigned
```

<span data-ttu-id="97da1-154">これで、アクセス許可が要求されます。</span><span class="sxs-lookup"><span data-stu-id="97da1-154">This will prompt you for permission:</span></span>

```output
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

<span data-ttu-id="97da1-155">使用できる必要がありますし、`Import-Module`コマンドレットを読み込めません。</span><span class="sxs-lookup"><span data-stu-id="97da1-155">You should then be able to use `Import-Module` to load the cmdlets.</span></span>

<span data-ttu-id="97da1-156">ゾーン終了</span><span class="sxs-lookup"><span data-stu-id="97da1-156">:::zone-end</span></span>

<span data-ttu-id="97da1-157">ゾーン ピボット"linux、macos"を =</span><span class="sxs-lookup"><span data-stu-id="97da1-157">::: zone pivot="linux,macos"</span></span>

<span data-ttu-id="97da1-158">私たちは、同じコマンドを使用して、Linux または macOS で Azure PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="97da1-158">We use the same commands to install the Azure PowerShell on either Linux or macOS.</span></span>

1. <span data-ttu-id="97da1-159">ターミナルで次のコマンドを入力し、管理者特権で PowerShell Core を起動します。</span><span class="sxs-lookup"><span data-stu-id="97da1-159">In a terminal, type the following command to launch PowerShell Core with elevated privileges.</span></span>

    ```bash
    sudo pwsh
    ```

1. <span data-ttu-id="97da1-160">PowerShell Core プロンプトで次のコマンドを実行して、Azure PowerShell をインストールします。</span><span class="sxs-lookup"><span data-stu-id="97da1-160">Run the following command at the PowerShell Core prompt to install Azure PowerShell.</span></span>

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. <span data-ttu-id="97da1-161">**PSGallery** からのモジュールを信頼するかどうかの確認を求められたら、**[はい]** または **[すべてはい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="97da1-161">If you are asked whether you trust modules from **PSGallery**, answer **Yes** or **Yes to All**.</span></span>

<span data-ttu-id="97da1-162">ゾーン終了</span><span class="sxs-lookup"><span data-stu-id="97da1-162">:::zone-end</span></span>

### <a name="update-a-module"></a><span data-ttu-id="97da1-163">モジュールを更新します。</span><span class="sxs-lookup"><span data-stu-id="97da1-163">Update a module</span></span>

<span data-ttu-id="97da1-164">警告または Azure PowerShell モジュールのバージョンが既にインストールされていることを示すエラー メッセージを取得する場合を更新することができます、_最新_コマンドを実行してバージョン。</span><span class="sxs-lookup"><span data-stu-id="97da1-164">If you get a warning or error message indicating that a version of the Azure PowerShell module is already installed, you can update to the _latest_ version by issuing the command:</span></span>

<span data-ttu-id="97da1-165">ゾーン pivot ="windows"</span><span class="sxs-lookup"><span data-stu-id="97da1-165">:::zone pivot="windows"</span></span>

```powershell
Update-Module -Name AzureRM
```

<span data-ttu-id="97da1-166">ゾーン終了</span><span class="sxs-lookup"><span data-stu-id="97da1-166">:::zone-end</span></span>

<span data-ttu-id="97da1-167">ゾーン ピボット"linux、macos"を =</span><span class="sxs-lookup"><span data-stu-id="97da1-167">::: zone pivot="linux,macos"</span></span>

```powershell
Update-Module -Name AzureRM.NetCore
```

<span data-ttu-id="97da1-168">ゾーン終了</span><span class="sxs-lookup"><span data-stu-id="97da1-168">:::zone-end</span></span>

<span data-ttu-id="97da1-169">`Install-Module` コマンドと同じように、モジュールの信頼の確認を求められたら **[はい]** または **[すべてはい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="97da1-169">As with the `Install-Module` command, answer **Yes** or **Yes to All** when prompted to trust the module.</span></span> <span data-ttu-id="97da1-170">使用することも、`Update-Module`をそれに問題がある場合、モジュールを再インストールするコマンド。</span><span class="sxs-lookup"><span data-stu-id="97da1-170">You can also use the `Update-Module` command to re-install a module if you are having trouble with it.</span></span>

## <a name="example-how-to-create-a-resource-group-with-azure-powershell"></a><span data-ttu-id="97da1-171">例: Azure PowerShell を使用したリソース グループを作成する方法</span><span class="sxs-lookup"><span data-stu-id="97da1-171">Example: How to create a resource group with Azure PowerShell</span></span>
<span data-ttu-id="97da1-172">Azure モジュールをロードしたら、Azure との連携を開始することができます。</span><span class="sxs-lookup"><span data-stu-id="97da1-172">Once you have the Azure module loaded, you can begin working with Azure.</span></span> <span data-ttu-id="97da1-173">一般的なタスク - リソース グループを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="97da1-173">Let's do a common task - create a Resource Group.</span></span> <span data-ttu-id="97da1-174">ご存知のように、関連リソースをまとめて管理するのにリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="97da1-174">As you know, we use resource groups to administer related resources together.</span></span> <span data-ttu-id="97da1-175">新しいリソース グループを作成すると、新しい Azure ソリューションを開始するときに行います最初のタスクの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="97da1-175">Creating a new resource group is one of the first tasks you'll do when starting a new Azure solution.</span></span>

<span data-ttu-id="97da1-176">次の 4 つの手順を実行する必要がありますがあります。</span><span class="sxs-lookup"><span data-stu-id="97da1-176">There are four steps we need to perform:</span></span>

1. <span data-ttu-id="97da1-177">Azure コマンドレットをインポートします。</span><span class="sxs-lookup"><span data-stu-id="97da1-177">Import the Azure cmdlets.</span></span>

1. <span data-ttu-id="97da1-178">Azure サブスクリプションに接続します。</span><span class="sxs-lookup"><span data-stu-id="97da1-178">Connect to your Azure subscription.</span></span>

1. <span data-ttu-id="97da1-179">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="97da1-179">Create the resource group.</span></span>

1. <span data-ttu-id="97da1-180">正常に作成できたことを確認します (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="97da1-180">Verify that creation was successful (see below).</span></span>

<span data-ttu-id="97da1-181">次の図では、この手順の概要を示します。</span><span class="sxs-lookup"><span data-stu-id="97da1-181">The following illustration shows an overview of these steps.</span></span>

![リソース グループを作成する手順を示す図。](../media/5-create-resource-overview.png)

<span data-ttu-id="97da1-183">各手順は異なるコマンドレットに対応しています。</span><span class="sxs-lookup"><span data-stu-id="97da1-183">Each step corresponds to a different cmdlet.</span></span>

### <a name="import-the-azure-cmdlets"></a><span data-ttu-id="97da1-184">Azure のコマンドレットをインポートします。</span><span class="sxs-lookup"><span data-stu-id="97da1-184">Import the Azure cmdlets</span></span>
<span data-ttu-id="97da1-185">起動時、PowerShell は既定で中心的なコマンドレットのみを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="97da1-185">At startup, PowerShell loads only the core cmdlets by default.</span></span> <span data-ttu-id="97da1-186">つまり、Azure で使用する必要があるコマンドレットは読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="97da1-186">This means the cmdlets you need to work with Azure won't be loaded.</span></span> <span data-ttu-id="97da1-187">必要なコマンドレットを最も確実に読み込む方法は、PowerShell セッションの開始時にコマンドレットを手動でインポートすることです。</span><span class="sxs-lookup"><span data-stu-id="97da1-187">The most reliable way to load the cmdlets you need is to import them manually at the start of your PowerShell session.</span></span>

<span data-ttu-id="97da1-188">モジュールの読み込みには **Import-Module** コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="97da1-188">You use the **Import-Module** cmdlet to load modules.</span></span> <span data-ttu-id="97da1-189">このコマンドレットには、さまざまな状況に対処するためのパラメーターがたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="97da1-189">This cmdlet has many parameters to handle a variety of situations.</span></span> <span data-ttu-id="97da1-190">たとえば、複数のモジュールを読み込んだり、特定のモジュール バージョンを読み込んだり、モジュールの一部を読み込んだりできます。</span><span class="sxs-lookup"><span data-stu-id="97da1-190">For example, it can load multiple modules, a specific module version, part of a module, and so on.</span></span>

<span data-ttu-id="97da1-191">次のコマンドで AzureRM のすべてのコマンドレットを読み込むたとえば、**管理者特権の PowerShell セッションで**:</span><span class="sxs-lookup"><span data-stu-id="97da1-191">For example, we can load all the cmdlets for AzureRM with the following command **in an elevated PowerShell session**:</span></span>

<span data-ttu-id="97da1-192">ゾーン pivot ="windows"</span><span class="sxs-lookup"><span data-stu-id="97da1-192">:::zone pivot="windows"</span></span>

```powershell
Import-Module AzureRM
```

<span data-ttu-id="97da1-193">ゾーン終了</span><span class="sxs-lookup"><span data-stu-id="97da1-193">:::zone-end</span></span>

<span data-ttu-id="97da1-194">ゾーン ピボット"linux、macos"を =</span><span class="sxs-lookup"><span data-stu-id="97da1-194">::: zone pivot="linux,macos"</span></span>

```powershell
Import-Module AzureRM.NetCore
```

<span data-ttu-id="97da1-195">ゾーン終了</span><span class="sxs-lookup"><span data-stu-id="97da1-195">:::zone-end</span></span>

> [!TIP]
> <span data-ttu-id="97da1-196">Azure PowerShell を頻繁に使用するようであれば、2 とおりの方法でモジュール読み込みプロセスを自動化できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-196">If you find that you work with Azure PowerShell frequently, there are two ways you can automate the module-loading process.</span></span> <span data-ttu-id="97da1-197">PowerShell プロファイルにエントリを追加し、起動時に Azure モジュールをインポートするか、PowerShell の最新版を使用し、コマンドレットの使用時にコマンドレットを含むモジュールを自動的に読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="97da1-197">You can add an entry to your PowerShell profile to import the Azure module at startup or use the latest versions of PowerShell, which loads the containing module automatically when you use a cmdlet.</span></span>

### <a name="connect"></a><span data-ttu-id="97da1-198">接続</span><span class="sxs-lookup"><span data-stu-id="97da1-198">Connect</span></span>
<span data-ttu-id="97da1-199">Azure PowerShell のローカル インストールを使用する場合は、Azure コマンドを実行する前に認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="97da1-199">When you are working with a local install of Azure PowerShell, you will need to authenticate before you can execute Azure commands.</span></span> <span data-ttu-id="97da1-200">**Connect-AzureRmAccount** コマンドレットを実行すると、Azure 資格情報が求められ、Azure サブスクリプションに接続されます。</span><span class="sxs-lookup"><span data-stu-id="97da1-200">The **Connect-AzureRmAccount** cmdlet prompts for your Azure credentials and then connects to your Azure subscription.</span></span> <span data-ttu-id="97da1-201">さまざまなオプション パラメーターがありますが、対話的プロンプトだけが必要であれば、パラメーターは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="97da1-201">It has many optional parameters, but if all you need is an interactive prompt, no parameters are needed:</span></span>

```powershell
Connect-AzureRmAccount
```

<span data-ttu-id="97da1-202">このモジュールは、コア セットの一部ではないために、開始するすべての新しい PowerShell セッションにこれらの手順を繰り返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="97da1-202">You'll need to repeat these steps for every new PowerShell session you start since this module is not part of the core set.</span></span>


### <a name="working-with-subscriptions"></a><span data-ttu-id="97da1-203">サブスクリプションの使用</span><span class="sxs-lookup"><span data-stu-id="97da1-203">Working with subscriptions</span></span>
<span data-ttu-id="97da1-204">Azure に慣れていない場合をおそらく 1 つのサブスクリプションがあるだけです。</span><span class="sxs-lookup"><span data-stu-id="97da1-204">If you are new to Azure, you probably only have a single subscription.</span></span> <span data-ttu-id="97da1-205">一方、しばらく前から Azure を使用している場合は、複数の Azure サブスクリプションを作成済みであることも考えられます。</span><span class="sxs-lookup"><span data-stu-id="97da1-205">But if you have been using Azure for a while, you may have created multiple Azure subscriptions.</span></span> <span data-ttu-id="97da1-206">その場合は、特定のサブスクリプションに対してコマンドを実行するように Azure PowerShell を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="97da1-206">You can configure Azure PowerShell to execute commands against a particular subscription.</span></span>

<span data-ttu-id="97da1-207">一度に 1 つのサブスクリプションでのみできます。</span><span class="sxs-lookup"><span data-stu-id="97da1-207">You can only be in one subscription at a time.</span></span> <span data-ttu-id="97da1-208">使用して、`Get-AzureRmContext`コマンドレットをどのサブスクリプションがアクティブかを判断します。</span><span class="sxs-lookup"><span data-stu-id="97da1-208">Use the `Get-AzureRmContext` cmdlet to determine which subscription is active.</span></span> <span data-ttu-id="97da1-209">しいものではない場合は、変更を適用することができます。</span><span class="sxs-lookup"><span data-stu-id="97da1-209">If it's not the correct one, you can change it.</span></span>

1. <span data-ttu-id="97da1-210">使用してアカウントのすべてのサブスクリプション名の一覧を取得、`Get-AzureRmSubscription`コマンド。</span><span class="sxs-lookup"><span data-stu-id="97da1-210">Get a list of all subscription names in your account with the `Get-AzureRmSubscription` command.</span></span> 

2. <span data-ttu-id="97da1-211">選択する 1 つの名前を渡すことによって、サブスクリプションを変更します。</span><span class="sxs-lookup"><span data-stu-id="97da1-211">Change the subscription by passing the name of the one to select.</span></span>

```powershell
Select-AzureRmSubscription -Subscription "Visual Studio Enterprise"
```

### <a name="get-a-list-of-all-resource-groups"></a><span data-ttu-id="97da1-212">すべてのリソース グループの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="97da1-212">Get a list of all Resource Groups</span></span>

<span data-ttu-id="97da1-213">アクティブなサブスクリプション内のすべてのリソース グループの一覧を取得できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-213">You can retrieve a list of all Resource Groups in the active subscription:</span></span>

```powershell
Get-AzureRmResourceGroup
```

<span data-ttu-id="97da1-214">簡潔なビューを取得するからの出力を送信することができます、`Get-AzureRmResourceGroup`を`Format-Table`コマンドレットにパイプを使用して ' |'。</span><span class="sxs-lookup"><span data-stu-id="97da1-214">To get a more concise view, you can send the output from the `Get-AzureRmResourceGroup` to the `Format-Table` cmdlet using a pipe '|'.</span></span>

```powershell
Get-AzureRmResourceGroup | Format-Table
```

<span data-ttu-id="97da1-215">これは、以下のような出力します。</span><span class="sxs-lookup"><span data-stu-id="97da1-215">This will output something like:</span></span>

```output
ResourceGroupName                  Location       ProvisioningState Tags TagsTable ResourceId
-----------------                  --------       ----------------- ---- --------- ----------
cloud-shell-storage-southcentralus southcentralus Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
ExerciseResources                  eastus         Succeeded                        /subscriptions/xxxxxxxx-d3ce-4172...
```

### <a name="create-a-resource-group"></a><span data-ttu-id="97da1-216">リソース グループを作成します</span><span class="sxs-lookup"><span data-stu-id="97da1-216">Create a Resource Group</span></span>

<span data-ttu-id="97da1-217">Azure では、リソースを作成する場合、ご存知のように管理目的でリソース グループに配置するが常に。</span><span class="sxs-lookup"><span data-stu-id="97da1-217">As you know, when you are creating resources in Azure, you will always place them into a resource group for management purposes.</span></span> <span data-ttu-id="97da1-218">リソース グループは、多くの場合は、まず、新しいアプリケーションを開始するときに作成のいずれかです。</span><span class="sxs-lookup"><span data-stu-id="97da1-218">A resource group is often one of the first things you will create when starting a new application.</span></span>

<span data-ttu-id="97da1-219">持つリソース グループを作成することができます、`New-AzureRmResourceGroup`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="97da1-219">You can create resource groups with the `New-AzureRmResourceGroup` cmdlet.</span></span> <span data-ttu-id="97da1-220">名前と場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="97da1-220">You must specify a name and location.</span></span> <span data-ttu-id="97da1-221">この名前はサブスクリプション内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="97da1-221">The name must be unique within your subscription.</span></span> <span data-ttu-id="97da1-222">この場所によって、お使いのリソース グループのメタデータが保存される場所が決定されます (コンプライアンス上の理由から重要となる場合があります)。</span><span class="sxs-lookup"><span data-stu-id="97da1-222">The location determines where the metadata for your resource group will be stored (which may be important to you for compliance reasons).</span></span> <span data-ttu-id="97da1-223">"West US"、"North Europe"、"West India" などの文字列を使用して場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="97da1-223">You use strings like "West US", "North Europe", or "West India" to specify the location.</span></span> <span data-ttu-id="97da1-224">Azure のコマンドレットのほとんどと同様に`New-AzureRmResourceGroup`は多くの省略可能なパラメーターがあります。 ただし、でも core 構文は。</span><span class="sxs-lookup"><span data-stu-id="97da1-224">As with most of the Azure cmdlets, `New-AzureRmResourceGroup` has many optional parameters; however, the core syntax is:</span></span>

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

> [!NOTE]
> <span data-ttu-id="97da1-225">リソース グループを作成する Azure のサンド ボックス内で取り組む予定に注意してください。</span><span class="sxs-lookup"><span data-stu-id="97da1-225">Remember, we will be working in the Azure Sandbox which creates the Resource Group for you.</span></span> <span data-ttu-id="97da1-226">上記のコマンドは、自分のサブスクリプションで作業する場合に使用されます。</span><span class="sxs-lookup"><span data-stu-id="97da1-226">The above command would be used if you work in your own subscription.</span></span>

### <a name="verify-the-resources"></a><span data-ttu-id="97da1-227">リソースを確認します</span><span class="sxs-lookup"><span data-stu-id="97da1-227">Verify the resources</span></span>
<span data-ttu-id="97da1-228">`Get-AzureRmResource` Azure リソースを一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="97da1-228">The `Get-AzureRmResource` lists your Azure resources.</span></span> <span data-ttu-id="97da1-229">リソース グループが正常に作成されたことを確認するためにここで役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="97da1-229">This is useful here to verify whether creation of the resource group was successful.</span></span>

```powershell
Get-AzureRmResource
```

<span data-ttu-id="97da1-230">ように、`Get-AzureRmResourceGroup`コマンドをより簡潔なビュー取得できます、`Format-Table`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="97da1-230">Like the `Get-AzureRmResourceGroup` command, you can get a more concise view through the `Format-Table` cmdlet.</span></span> <span data-ttu-id="97da1-231">ここでは、短縮形バージョンを使用して、 `ft`:</span><span class="sxs-lookup"><span data-stu-id="97da1-231">Here we will use a shorthand version `ft`:</span></span>

```powershell
Get-AzureRmResource | ft
```

<span data-ttu-id="97da1-232">そのグループに関連付けられているリソースのみを一覧表示する特定のリソース グループにフィルターできます。</span><span class="sxs-lookup"><span data-stu-id="97da1-232">You can also filter it to specific resource groups to only list resources associated with that group:</span></span>

```powershell
Get-AzureRmResource -ResourceGroup ExerciseResources
```

### <a name="creating-an-azure-virtual-machine"></a><span data-ttu-id="97da1-233">Azure の仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="97da1-233">Creating an Azure Virtual Machine</span></span>

<span data-ttu-id="97da1-234">PowerShell で行うことがもう 1 つの一般的なタスクでは、Vm を作成します。</span><span class="sxs-lookup"><span data-stu-id="97da1-234">Another common task that could be done with PowerShell is to create VMs.</span></span>

<span data-ttu-id="97da1-235">Azure PowerShell では、`New-AzureRmVm`コマンドレットで仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="97da1-235">Azure PowerShell provides the `New-AzureRmVm` cmdlet to create a virtual machine.</span></span> <span data-ttu-id="97da1-236">コマンドレットには多くのパラメーターがあり、それを使用して数多くの VM 構成設定を処理することができます。</span><span class="sxs-lookup"><span data-stu-id="97da1-236">The cmdlet has many parameters to let it handle the large number of VM configuration settings.</span></span> <span data-ttu-id="97da1-237">ほとんどのパラメーターに適切な既定値があるため、指定する必要があるのは次の 5 つのみです。</span><span class="sxs-lookup"><span data-stu-id="97da1-237">Most of the parameters have reasonable default values so we only need to specify five things:</span></span>

- <span data-ttu-id="97da1-238">**ResourceGroupName**: 新しい VM が配置されるリソース グループ。</span><span class="sxs-lookup"><span data-stu-id="97da1-238">**ResourceGroupName**: The resource group into which the new VM will be placed.</span></span>
- <span data-ttu-id="97da1-239">**Name**: Azure での VM の名前。</span><span class="sxs-lookup"><span data-stu-id="97da1-239">**Name**: The name of the VM in Azure.</span></span>
- <span data-ttu-id="97da1-240">**Location**: VM がプロビジョニングされる地理的な場所。</span><span class="sxs-lookup"><span data-stu-id="97da1-240">**Location**: Geographic location where the VM will be provisioned.</span></span>
- <span data-ttu-id="97da1-241">**Credential**: VM 管理者アカウント用のユーザー名とパスワードが含まれているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="97da1-241">**Credential**: An object containing the username and password for the VM admin account.</span></span> <span data-ttu-id="97da1-242">使用して、`Get-Credential`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="97da1-242">We will use the `Get-Credential` cmdlet.</span></span> <span data-ttu-id="97da1-243">このコマンドレットはユーザー名とパスワードを確認し、資格情報オブジェクトにパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="97da1-243">This cmdlet will prompt for a username and password and package it into a credential object.</span></span>
- <span data-ttu-id="97da1-244">**イメージ**: VM に使用するオペレーティング システム イメージ。</span><span class="sxs-lookup"><span data-stu-id="97da1-244">**Image**: The operating system image to use for the VM.</span></span> <span data-ttu-id="97da1-245">これは多くの場合、Linux ディストリビューション、または Windows Server です。</span><span class="sxs-lookup"><span data-stu-id="97da1-245">This is often a Linux distribution, or Windows Server.</span></span>

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

<span data-ttu-id="97da1-246">上記のように、これらのパラメーターをコマンドレットに直接を指定できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-246">You can supply these parameters directly to the cmdlet as shown above.</span></span> <span data-ttu-id="97da1-247">他のコマンドレットを使用してなど、仮想マシンを構成する代わりに、 `Set-AzureRmVMOperatingSystem`、 `Set-AzureRmVMSourceImage`、 `Add-AzureRmVMNetworkInterface`、および`Set-AzureRmVMOSDisk`します。</span><span class="sxs-lookup"><span data-stu-id="97da1-247">Alternatively, other cmdlets can be used to configure the virtual machine, such as `Set-AzureRmVMOperatingSystem`, `Set-AzureRmVMSourceImage`, `Add-AzureRmVMNetworkInterface`, and `Set-AzureRmVMOSDisk`.</span></span>

<span data-ttu-id="97da1-248">文字列の例を次に示します、`Get-Credential`コマンドレットと共に、`-Credential`パラメーター。</span><span class="sxs-lookup"><span data-stu-id="97da1-248">Here's an example that strings the `Get-Credential` cmdlet together with the `-Credential` parameter:</span></span>

```powershell
New-AzureRmVM -Name MyVm -ResourceGroupName ExerciseResources -Credential (Get-Credential) ...
```

<span data-ttu-id="97da1-249">`AzureRmVM`サフィックスは、PowerShell で VM ベースのコマンドを特定します。</span><span class="sxs-lookup"><span data-stu-id="97da1-249">The `AzureRmVM` suffix is specific to VM-based commands in PowerShell.</span></span> <span data-ttu-id="97da1-250">使用できるその他いくつかあります。</span><span class="sxs-lookup"><span data-stu-id="97da1-250">There are several others you can use:</span></span>

| <span data-ttu-id="97da1-251">コマンド</span><span class="sxs-lookup"><span data-stu-id="97da1-251">Command</span></span> | <span data-ttu-id="97da1-252">説明</span><span class="sxs-lookup"><span data-stu-id="97da1-252">Description</span></span> |
|---------|-------------|
| `Remove-AzureRmVM` | <span data-ttu-id="97da1-253">Azure VM を削除します。</span><span class="sxs-lookup"><span data-stu-id="97da1-253">Deletes an Azure VM.</span></span> |
| `Start-AzureRmVM` | <span data-ttu-id="97da1-254">停止した VM を起動します。</span><span class="sxs-lookup"><span data-stu-id="97da1-254">Start a stopped VM.</span></span> |
| `Stop-AzureRmVM` | <span data-ttu-id="97da1-255">実行中の VM を停止します。</span><span class="sxs-lookup"><span data-stu-id="97da1-255">Stop a running VM.</span></span> |
| `Restart-AzureRmVM` | <span data-ttu-id="97da1-256">VM を再起動します。</span><span class="sxs-lookup"><span data-stu-id="97da1-256">Restart a VM.</span></span> |
| `Update-AzureRmVM` | <span data-ttu-id="97da1-257">VM の構成を更新します。</span><span class="sxs-lookup"><span data-stu-id="97da1-257">Updates the configuration for a VM.</span></span> |

#### <a name="example-getting-the-information-for-a-vm"></a><span data-ttu-id="97da1-258">例: VM の情報を取得します。</span><span class="sxs-lookup"><span data-stu-id="97da1-258">Example: getting the information for a VM</span></span>

<span data-ttu-id="97da1-259">サブスクリプション内の Vm を一覧表示、`Get-AzureRmVM -Status`コマンド。</span><span class="sxs-lookup"><span data-stu-id="97da1-259">You can list the VMs in your subscription with the `Get-AzureRmVM -Status` command.</span></span> <span data-ttu-id="97da1-260">これを使用して VM を指定することも、`-Name`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="97da1-260">This can also specify a VM with the `-Name` property.</span></span> <span data-ttu-id="97da1-261">ここで、PowerShell 変数に割り当てます。</span><span class="sxs-lookup"><span data-stu-id="97da1-261">Here we assign it to a PowerShell variable:</span></span>

```powershell
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName ExerciseResources
```

<span data-ttu-id="97da1-262">これは興味深いは、_オブジェクト_と対話できます。</span><span class="sxs-lookup"><span data-stu-id="97da1-262">The interesting thing is this is an _object_ you can interact with.</span></span> <span data-ttu-id="97da1-263">たとえば、そのオブジェクトを取得、変更を加えるして Azure にバックアップの変更をプッシュ、、`Update-AzureRmVM`コマンド。</span><span class="sxs-lookup"><span data-stu-id="97da1-263">For example, you can take that object, make changes and then push changes back to Azure with the `Update-AzureRmVM` command:</span></span>

```powershell
$ResourceGroupName = "ExerciseResources"
$vm = Get-AzureRmVM  -Name MyVM -ResourceGroupName $ResourceGroupName
$vm.HardwareProfile.vmSize = "Standard_DS3_v2"

Update-AzureRmVM -ResourceGroupName $ResourceGroupName  -VM $vm
```

## <a name="summary"></a><span data-ttu-id="97da1-264">まとめ</span><span class="sxs-lookup"><span data-stu-id="97da1-264">Summary</span></span>
<span data-ttu-id="97da1-265">PowerShell の対話モードは 1 回限りのタスクに適しています。</span><span class="sxs-lookup"><span data-stu-id="97da1-265">PowerShell's interactive mode is appropriate for one-off tasks.</span></span> <span data-ttu-id="97da1-266">例では、可能性があります、同じリソース グループ、つまり妥当な対話形式で作成することは、プロジェクトの有効期間にわたって使用します。</span><span class="sxs-lookup"><span data-stu-id="97da1-266">In our example, we'll likely use the same resource group for the lifetime of the project, which means creating it interactively is reasonable.</span></span> <span data-ttu-id="97da1-267">このタスクでは多くの場合、スクリプトを記述してそのスクリプトを正確に 1 回実行するより、対話モードの方が簡単で速くなります。</span><span class="sxs-lookup"><span data-stu-id="97da1-267">Interactive mode is often quicker and easier for this task than writing a script and executing that script exactly once.</span></span>