<span data-ttu-id="88bdf-101">Azure PowerShell では、コマンドを記述し、すぐに実行できます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-101">Azure PowerShell lets you write commands and execute them immediately.</span></span> <span data-ttu-id="88bdf-102">これは**対話モード**と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="88bdf-102">This is known as **interactive mode**.</span></span>

<span data-ttu-id="88bdf-103">顧客関係管理 (CRM) の例の全体的な目標は VM を含む 3 つのテスト環境を作成することであることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="88bdf-103">Recall that the overall goal in the Customer Relationship Management (CRM) example is to create three test environments containing VMs.</span></span> <span data-ttu-id="88bdf-104">リソース グループを利用して VM を別々の環境に整理します。単体テスト用に 1 つ、統合テスト用に 1 つ、受け入れテスト用に 1 つとなります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-104">You will use resource groups to ensure the VMs are organized into separate environments: one for unit testing, one for integration testing, and one for acceptance testing.</span></span> <span data-ttu-id="88bdf-105">リソース グループは一度だけ作成すれば十分です。そこで、PowerShell の対話モードを使用することが良い選択肢となります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-105">You only need to create the resource groups once, which means using the interactive mode of PowerShell is a good choice.</span></span>

<span data-ttu-id="88bdf-106">このセクションでは、PowerShell を対話的に使用し、Azure サブスクリプションにログオンしてリソース グループを作成する例をいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-106">This section shows some examples of using PowerShell interactively to log on to your Azure subscription and create resource groups.</span></span>

## <a name="what-are-powershell-cmdlets"></a><span data-ttu-id="88bdf-107">PowerShell コマンドレットとは</span><span class="sxs-lookup"><span data-stu-id="88bdf-107">What are PowerShell cmdlets?</span></span>
<span data-ttu-id="88bdf-108">PowerShell コマンドは**コマンドレット**と呼ばれています。</span><span class="sxs-lookup"><span data-stu-id="88bdf-108">A PowerShell command is called a **cmdlet** (pronounced "command-let").</span></span> <span data-ttu-id="88bdf-109">コマンドレットは 1 つの機能を操作するコマンドです。</span><span class="sxs-lookup"><span data-stu-id="88bdf-109">A cmdlet is a command that manipulates a single feature.</span></span> <span data-ttu-id="88bdf-110">**コマンドレット**と用語は "小さなコマンド" を意味します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-110">The term **cmdlet** is intended to imply "small command".</span></span> <span data-ttu-id="88bdf-111">慣例で、コマンドレットの作成者には、コマンドレットをシンプルにすること、目的を 1 つだけにすることが推奨されています。</span><span class="sxs-lookup"><span data-stu-id="88bdf-111">By convention, cmdlet authors are encouraged to keep cmdlets simple and single-purpose.</span></span>

<span data-ttu-id="88bdf-112">基本の PowerShell 製品には、セッションやバックグラウンド ジョブなどの機能で使用できるコマンドレットが付属します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-112">The base PowerShell product ships with cmdlets that work with features such as sessions and background jobs.</span></span> <span data-ttu-id="88bdf-113">他の機能を操作するコマンドレットを得るには、PowerShell インストールにモジュールを追加します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-113">You add modules to your PowerShell installation to get cmdlets that manipulate other features.</span></span> <span data-ttu-id="88bdf-114">たとえば、サードパーティ モジュールには、FTP と連動するモジュール、オペレーティング システムを管理するモジュール、ファイル システムにアクセスするモジュールなどがあります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-114">For example, there are third-party modules to work with ftp, administer your operating system, access the file system, and so on.</span></span>

<span data-ttu-id="88bdf-115">コマンドレットには、動詞-名詞の命名規則が採用されています。たとえば、**Get-Process**、**Format-Table**、**Start-Service** のようになります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-115">Cmdlets follow a verb-noun naming convention; for example, **Get-Process**, **Format-Table**, and **Start-Service**.</span></span> <span data-ttu-id="88bdf-116">動詞の選択にも規則があります。たとえば、データの取得は "get"、データの挿入または更新は "set"、データの書式設定は "format"、宛先への直接出力は "out" になります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-116">There is also a convention for verb choice: "get" to retrieve data, "set" to insert or update data, "format" to format data, "out" to direct output to a destination, and so on.</span></span>

<span data-ttu-id="88bdf-117">コマンドレットの作成者には、コマンドレットごとにヘルプ ファイルを含めることが推奨されています。</span><span class="sxs-lookup"><span data-stu-id="88bdf-117">Cmdlet authors are encouraged to include a help file for each cmdlet.</span></span> <span data-ttu-id="88bdf-118">**Get-Help** コマンドレットを実行すると、コマンドレットのヘルプ ファイルが表示されます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-118">The **Get-Help** cmdlet displays the help file for any cmdlet:</span></span>

```powershell
Get-Help <cmdlet-name> -detailed
```

## <a name="what-is-azurerm"></a><span data-ttu-id="88bdf-119">AzureRM とは</span><span class="sxs-lookup"><span data-stu-id="88bdf-119">What is AzureRM?</span></span>
<span data-ttu-id="88bdf-120">**AzureRM** は、Azure 機能で使用できるコマンドレットを含む Azure PowerShell コマンドレットの正式名称です (名前の中の **RM** は **Resource Manager** という意味です)。</span><span class="sxs-lookup"><span data-stu-id="88bdf-120">**AzureRM** is the formal name for the Azure PowerShell module containing cmdlets to work with Azure features (the **RM** in the name stands for **Resource Manager**).</span></span> <span data-ttu-id="88bdf-121">数百単位のコマンドレットが含まれ、あらゆる Azure リソースのほぼすべての面を制御できます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-121">It contains hundreds of cmdlets that let you control nearly every aspect of every Azure resource.</span></span> <span data-ttu-id="88bdf-122">リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-122">You can work with resource groups, storage, virtual machines, Azure Active Directory, containers, machine learning, and so on.</span></span>

## <a name="how-to-create-a-resource-group"></a><span data-ttu-id="88bdf-123">リソース グループの作成方法</span><span class="sxs-lookup"><span data-stu-id="88bdf-123">How to create a resource group</span></span>
<span data-ttu-id="88bdf-124">次に、ローカルにインストールした Azure PowerShell を使用してリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-124">Next, we'll create a resource group using a local installation of Azure PowerShell.</span></span> 

<span data-ttu-id="88bdf-125">次の 4 つのステップがあります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-125">There are four steps:</span></span> 
1. <span data-ttu-id="88bdf-126">Azure コマンドレットをインポートします。</span><span class="sxs-lookup"><span data-stu-id="88bdf-126">Import the Azure cmdlets.</span></span>
1. <span data-ttu-id="88bdf-127">Azure サブスクリプションに接続します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-127">Connect to your Azure subscription.</span></span>
1. <span data-ttu-id="88bdf-128">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-128">Create the resource group.</span></span>
1. <span data-ttu-id="88bdf-129">正常に作成できたことを確認します (下記参照)。</span><span class="sxs-lookup"><span data-stu-id="88bdf-129">Verify that creation was successful (see below).</span></span>

![Azure PowerShell を使用して Azure にリソースを作成する手順](../media-drafts/5-create-resource-overview.png)

<span data-ttu-id="88bdf-131">各手順は異なるコマンドレットに対応しています。</span><span class="sxs-lookup"><span data-stu-id="88bdf-131">Each step corresponds to a different cmdlet.</span></span>

### <a name="import"></a><span data-ttu-id="88bdf-132">[インポート]</span><span class="sxs-lookup"><span data-stu-id="88bdf-132">Import</span></span>
<span data-ttu-id="88bdf-133">起動時、PowerShell は既定で中心的なコマンドレットのみを読み込みます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-133">At startup, PowerShell loads only the core cmdlets by default.</span></span> <span data-ttu-id="88bdf-134">つまり、Azure で使用する必要があるコマンドレットは読み込まれません。</span><span class="sxs-lookup"><span data-stu-id="88bdf-134">This means the cmdlets you need to work with Azure won't be loaded.</span></span> <span data-ttu-id="88bdf-135">必要なコマンドレットを最も確実に読み込む方法は、PowerShell セッションの開始時にコマンドレットを手動でインポートすることです。</span><span class="sxs-lookup"><span data-stu-id="88bdf-135">The most reliable way to load the cmdlets you need is to import them manually at the start of your PowerShell session.</span></span>

<span data-ttu-id="88bdf-136">モジュールの読み込みには **Import-Module** コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-136">You use the **Import-Module** cmdlet to load modules.</span></span> <span data-ttu-id="88bdf-137">このコマンドレットには、さまざまな状況に対処するためのパラメーターがたくさんあります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-137">This cmdlet has many parameters to handle a variety of situations.</span></span> <span data-ttu-id="88bdf-138">たとえば、複数のモジュールを読み込んだり、特定のモジュール バージョンを読み込んだり、モジュールの一部を読み込んだりできます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-138">For example, it can load multiple modules, a specific module version, part of a module, and so on.</span></span> <span data-ttu-id="88bdf-139">1 つのモジュールを完全に読み込む場合、構文は次のように単純になります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-139">To load the entirety of one module, the syntax is simply:</span></span>

```powershell
Import-Module <module-name>
```

> [!TIP]
> <span data-ttu-id="88bdf-140">Azure PowerShell を頻繁に使用するようであれば、2 とおりの方法でモジュール読み込みプロセスを自動化できます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-140">If you find that you work with Azure PowerShell frequently, there are two ways you can automate the module-loading process.</span></span> <span data-ttu-id="88bdf-141">PowerShell プロファイルにエントリを追加し、起動時に Azure モジュールをインポートするか、PowerShell の最新版を使用し、コマンドレットの使用時にコマンドレットを含むモジュールを自動的に読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-141">You can add an entry to your PowerShell profile to import the Azure module at startup or use the latest versions of PowerShell, which loads the containing module automatically when you use a cmdlet.</span></span>

### <a name="connect"></a><span data-ttu-id="88bdf-142">接続</span><span class="sxs-lookup"><span data-stu-id="88bdf-142">Connect</span></span>
<span data-ttu-id="88bdf-143">Azure PowerShell のローカル インストールを使用している場合、Azure コマンドを実行する前に認証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-143">Since you are working with a local install of Azure PowerShell, you will need to authenticate before you can execute Azure commands.</span></span> <span data-ttu-id="88bdf-144">**Connect-AzureRmAccount** コマンドレットを実行すると、Azure 資格情報が求められ、Azure サブスクリプションに接続されます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-144">The **Connect-AzureRmAccount** cmdlet prompts for your Azure credentials and then connects to your Azure subscription.</span></span> <span data-ttu-id="88bdf-145">さまざまなオプション パラメーターがありますが、対話的プロンプトだけが必要であれば、パラメーターは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="88bdf-145">It has many optional parameters, but if all you need is an interactive prompt, no parameters are needed:</span></span>

```powershell
Connect-AzureRmAccount
```

### <a name="create"></a><span data-ttu-id="88bdf-146">Create</span><span class="sxs-lookup"><span data-stu-id="88bdf-146">Create</span></span>
<span data-ttu-id="88bdf-147">**New-AzureRmResourceGroup** コマンドレットを実行すると、リソース グループが作成されます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-147">The **New-AzureRmResourceGroup** cmdlet creates a resource group.</span></span> <span data-ttu-id="88bdf-148">名前と場所を指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-148">You must specify a name and location.</span></span> <span data-ttu-id="88bdf-149">この名前はサブスクリプション内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-149">The name must be unique within your subscription.</span></span> <span data-ttu-id="88bdf-150">この場所によって、お使いのリソース グループのメタデータが保存される場所が決定されます (コンプライアンス上の理由から重要となる場合があります)。</span><span class="sxs-lookup"><span data-stu-id="88bdf-150">The location determines where the metadata for your resource group will be stored (which may be important to you for compliance reasons).</span></span> <span data-ttu-id="88bdf-151">"West US"、"North Europe"、"West India" などの文字列を使用して場所を指定します。</span><span class="sxs-lookup"><span data-stu-id="88bdf-151">You use strings like "West US", "North Europe", or "West India" to specify the location.</span></span> <span data-ttu-id="88bdf-152">ほとんどの Azure コマンドレットと同様に、**New-AzureRmResourceGroup** にはオプション パラメーターがたくさんありますが、中心的な構文は次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-152">As with most of the Azure cmdlets, **New-AzureRmResourceGroup** has many optional parameters; however, the core syntax is:</span></span>

```powershell
New-AzureRmResourceGroup -Name <name> -Location <location>
```

### <a name="verify"></a><span data-ttu-id="88bdf-153">確認</span><span class="sxs-lookup"><span data-stu-id="88bdf-153">Verify</span></span>
<span data-ttu-id="88bdf-154">**Get-AzureRmResource** を実行すると、Azure リソースが一覧表示されます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-154">The **Get-AzureRmResource** lists your Azure resources.</span></span> <span data-ttu-id="88bdf-155">リソース グループが正常に作成されたことを確認するためにここで役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-155">This is useful here to verify whether creation of the resource group was successful.</span></span>

```powershell
Get-AzureRmResource
```

<span data-ttu-id="88bdf-156">もっと簡潔な一覧を表示するには、パイプ '|' を利用して **Get-AzureRmResource** から **Format-Table** コマンドレットに出力を送信できます。</span><span class="sxs-lookup"><span data-stu-id="88bdf-156">To get a more concise view, you can send the output from the **Get-AzureRmResource** to the **Format-Table** cmdlet using a pipe '|'.</span></span>

```powershell
Get-AzureRmResource | Format-Table
```

## <a name="summary"></a><span data-ttu-id="88bdf-157">まとめ</span><span class="sxs-lookup"><span data-stu-id="88bdf-157">Summary</span></span>
<span data-ttu-id="88bdf-158">PowerShell の対話モードは 1 回限りのタスクに適しています。</span><span class="sxs-lookup"><span data-stu-id="88bdf-158">PowerShell's interactive mode is appropriate for one-off tasks.</span></span> <span data-ttu-id="88bdf-159">例では、プロジェクトのライフタイムに対して同じリソース グループを使用します。つまり、対話で作成することが道理にかなっています。</span><span class="sxs-lookup"><span data-stu-id="88bdf-159">In our example, we'll use the same resource group for the lifetime of the project, which means creating it interactively is reasonable.</span></span> <span data-ttu-id="88bdf-160">このタスクでは多くの場合、スクリプトを記述してそのスクリプトを正確に 1 回実行するより、対話モードの方が簡単で速くなります。</span><span class="sxs-lookup"><span data-stu-id="88bdf-160">Interactive mode is often quicker and easier for this task than writing a script and executing that script exactly once.</span></span>
