<span data-ttu-id="62daf-101">多くの場合、複雑なタスクや反復的なタスクの管理にはかなりの時間がかかります。</span><span class="sxs-lookup"><span data-stu-id="62daf-101">Complex or repetitive tasks often take a great deal of administrative time.</span></span> <span data-ttu-id="62daf-102">組織は、コストを削減してエラーを回避するためにこれらのタスクを自動化したいと考えます。</span><span class="sxs-lookup"><span data-stu-id="62daf-102">Organizations prefer to automate these tasks to reduce costs and avoid errors.</span></span>

<span data-ttu-id="62daf-103">これは、カスタマー リレーションシップ マネジメント (CRM) 会社の例では重要なことです。</span><span class="sxs-lookup"><span data-stu-id="62daf-103">This is important in the Customer Relationship Management (CRM) company example.</span></span> <span data-ttu-id="62daf-104">そのため、継続的に削除して再作成する必要がある、複数の Linux 仮想マシン (VM) でソフトウェアをテストします。</span><span class="sxs-lookup"><span data-stu-id="62daf-104">There, you test your software on multiple Linux Virtual Machines (VMs) that you need to continuously delete and recreate.</span></span> <span data-ttu-id="62daf-105">VM の作成を自動化するには PowerShell スクリプトを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="62daf-105">You want to use a PowerShell script to automate the creation of the VMs.</span></span>

<span data-ttu-id="62daf-106">VM の作成の主な操作以外にも、スクリプトに関する追加要件がいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="62daf-106">Beyond the core operation of creating a VM you have a few additional requirements for your script.</span></span> 
- <span data-ttu-id="62daf-107">複数の VM を作成するため、ループ内に作成を配置する必要があります</span><span class="sxs-lookup"><span data-stu-id="62daf-107">You will create multiple VMs, so you want to put the creation inside a loop</span></span>
- <span data-ttu-id="62daf-108">3 つの異なるリソース グループで VM を作成する必要があるため、そのリソース グループの名前をパラメーターとしてスクリプトに渡す必要があります</span><span class="sxs-lookup"><span data-stu-id="62daf-108">You need to create VMs in three different resource groups, so the name of the resource group should be passed to the script as a parameter</span></span>

<span data-ttu-id="62daf-109">このセクションでは、これらの要件を満たす Azure PowerShell スクリプトを記述して実行する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="62daf-109">In this section, you will see how to write and execute an Azure PowerShell script that meets these requirements.</span></span>

## <a name="what-is-a-powershell-script"></a><span data-ttu-id="62daf-110">PowerShell スクリプトとは</span><span class="sxs-lookup"><span data-stu-id="62daf-110">What is a PowerShell script?</span></span>
<span data-ttu-id="62daf-111">PowerShell スクリプトは、コマンドと制御コンストラクトを含むテキスト ファイルです。</span><span class="sxs-lookup"><span data-stu-id="62daf-111">A PowerShell script is a text file containing commands and control constructs.</span></span> <span data-ttu-id="62daf-112">コマンドはコマンドレットの呼び出しのことです。</span><span class="sxs-lookup"><span data-stu-id="62daf-112">The commands are invocations of cmdlets.</span></span> <span data-ttu-id="62daf-113">制御コンストラクトでは、PowerShell によって提供されるループ、変数、パラメーター、コメントなどの機能をプログラミングします。</span><span class="sxs-lookup"><span data-stu-id="62daf-113">The control constructs are programming features like loops, variables, parameters, comments, etc. supplied by PowerShell.</span></span>

<span data-ttu-id="62daf-114">PowerShell スクリプト ファイルには **.ps1** ファイル拡張子が付いています。</span><span class="sxs-lookup"><span data-stu-id="62daf-114">PowerShell script files have a **.ps1** file extension.</span></span> <span data-ttu-id="62daf-115">任意のテキスト エディターでこれらのファイルを作成して保存することができます。</span><span class="sxs-lookup"><span data-stu-id="62daf-115">You can create and save these files with any text editor.</span></span> 

> [!TIP]
> <span data-ttu-id="62daf-116">Windows で PowerShell スクリプトを記述する場合は、Windows PowerShell Integrated Scripting Environment (ISE) を使用できます。</span><span class="sxs-lookup"><span data-stu-id="62daf-116">If you’re writing PowerShell scripts under Windows, you can use the Windows PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="62daf-117">このエディターでは、構文の色分けなどの機能と利用可能なコマンドレットの一覧が提供されます。</span><span class="sxs-lookup"><span data-stu-id="62daf-117">This editor provides features such as syntax coloring and a list of available cmdlets.</span></span>
>
<span data-ttu-id="62daf-118">次のスクリーンショットは、Windows PowerShell Integrated Scripting Environment (ISE) でサンプル スクリプトを使用して Azure に接続し、Azure で仮想マシンを作成する様子を示しています。</span><span class="sxs-lookup"><span data-stu-id="62daf-118">The following screenshot shows the Windows PowerShell Integrated Scripting Environment (ISE) with a sample script to connect to Azure and create a virtual machine in Azure.</span></span>

>![仮想マシンを作成するためのスクリプトが編集ウィンドウで開かれている Windows PowerShell Integrated Scripting Environment のスクリーンショット。](../media/7-windows-powershell-ise-screenshot.png)

<span data-ttu-id="62daf-120">スクリプトを記述したら、前にドットとバックスラッシュが付いたファイルの名前を渡して、PowerShell コマンド ラインから実行します。</span><span class="sxs-lookup"><span data-stu-id="62daf-120">Once you have written the script, execute it from the PowerShell command line by passing the name of the file preceded by a dot and a backslash:</span></span>

```powershell
.\myScript.ps1
```

## <a name="powershell-techniques"></a><span data-ttu-id="62daf-121">PowerShell 手法</span><span class="sxs-lookup"><span data-stu-id="62daf-121">PowerShell techniques</span></span>
<span data-ttu-id="62daf-122">PowerShell には、一般的なプログラミング言語で見られる多くの機能があります。</span><span class="sxs-lookup"><span data-stu-id="62daf-122">PowerShell has many features found in typical programming languages.</span></span> <span data-ttu-id="62daf-123">変数の定義、ブランチとループの使用、コマンド ライン パラメーターのキャプチャ、関数の記述、コメントの追加などを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="62daf-123">You can define variables, use branches and loops, capture command-line parameters, write functions, add comments, and so on.</span></span> <span data-ttu-id="62daf-124">このスクリプトには、変数、ループ、パラメーターという 3 つの機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="62daf-124">We will need three features for our script: variables, loops, and parameters.</span></span>

### <a name="variables"></a><span data-ttu-id="62daf-125">variables</span><span class="sxs-lookup"><span data-stu-id="62daf-125">Variables</span></span>
<span data-ttu-id="62daf-126">PowerShell では変数がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="62daf-126">PowerShell supports variables.</span></span> <span data-ttu-id="62daf-127">**$** を使用して、変数と **=** を宣言し、値を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="62daf-127">Use **$** to declare a variable and **=** to assign a value.</span></span> <span data-ttu-id="62daf-128">例: </span><span class="sxs-lookup"><span data-stu-id="62daf-128">For example:</span></span>

```powershell
$loc = "East US"
$iterations = 3
```

<span data-ttu-id="62daf-129">変数でオブジェクトを保持することができます。</span><span class="sxs-lookup"><span data-stu-id="62daf-129">Variables can hold objects.</span></span> <span data-ttu-id="62daf-130">たとえば、次の定義では、**Get-Credential** コマンドレットによって返されるオブジェクトに **adminCredential** 変数が設定されます。</span><span class="sxs-lookup"><span data-stu-id="62daf-130">For example, the following definition sets the **adminCredential** variable to the object returned by the **Get-Credential** cmdlet.</span></span>

```powershell
$adminCredential = Get-Credential
```

<span data-ttu-id="62daf-131">変数に格納されている値を取得するには、次のように **$** プレフィックスとその名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="62daf-131">To obtain the value stored in a variable, use the **$** prefix and its name as shown below:</span></span> 

```powershell
$loc = "East US"
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location $loc
```

### <a name="loops"></a><span data-ttu-id="62daf-132">ループ</span><span class="sxs-lookup"><span data-stu-id="62daf-132">Loops</span></span>
<span data-ttu-id="62daf-133">PowerShell には、**For**、**Do...While**、**For...Each** などのループがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="62daf-133">PowerShell has several loops: **For**, **Do...While**, **For...Each**, and so on.</span></span> <span data-ttu-id="62daf-134">ここではコマンドレットを一定の回数実行するため、**For** ループが最適です。</span><span class="sxs-lookup"><span data-stu-id="62daf-134">The **For** loop is the best match for our needs because we will execute a cmdlet a fixed number of times.</span></span>

<span data-ttu-id="62daf-135">中心的な構文は次のとおりです。この例では 2 回反復され、毎回 **i** の値が出力されます。</span><span class="sxs-lookup"><span data-stu-id="62daf-135">The core syntax is shown below; the example runs for two iterations and prints the value of **i** each time.</span></span> <span data-ttu-id="62daf-136">比較演算子が記述され、"より小さい" 場合は **-lt**、"以下" の場合は **-le**、"等しい" 場合は **eq**、"等しくない" 場合は **ne** などとなります。</span><span class="sxs-lookup"><span data-stu-id="62daf-136">The comparison operators are written **-lt** for "less than", **-le** for "less than or equal", **eq** for "equal", **ne** for "not equal", etc.</span></span>

```powershell
For ($i = 1; $i -lt 3; $i++)
{
    $i
}
```

### <a name="parameters"></a><span data-ttu-id="62daf-137">parameters</span><span class="sxs-lookup"><span data-stu-id="62daf-137">Parameters</span></span>
<span data-ttu-id="62daf-138">スクリプトを実行するときに、コマンド ラインで引数を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="62daf-138">When you execute a script, you can pass arguments on the command line.</span></span> <span data-ttu-id="62daf-139">パラメーターごとに名前を指定でき、スクリプトで値を抽出するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="62daf-139">You can provide names for each parameter to help the script extract the values.</span></span> <span data-ttu-id="62daf-140">例: </span><span class="sxs-lookup"><span data-stu-id="62daf-140">For example:</span></span>

```powershell
.\setupEnvironment.ps1 -size 5 -location "East US"
```

<span data-ttu-id="62daf-141">スクリプト内で、変数に値をキャプチャします。</span><span class="sxs-lookup"><span data-stu-id="62daf-141">Inside the script, you capture the values into variables.</span></span> <span data-ttu-id="62daf-142">この例では、パラメーターは名前と一致します。</span><span class="sxs-lookup"><span data-stu-id="62daf-142">In this example, the parameters are matched by name:</span></span>

```powershell
param([string]$location, [int]$size)
```

<span data-ttu-id="62daf-143">コマンド ラインでは名前を省略することができます。</span><span class="sxs-lookup"><span data-stu-id="62daf-143">You can omit the names from the command line.</span></span> <span data-ttu-id="62daf-144">例: </span><span class="sxs-lookup"><span data-stu-id="62daf-144">For example:</span></span>

```powershell
.\setupEnvironment.ps1 5 "East US"
```

<span data-ttu-id="62daf-145">スクリプト内では、パラメーターに名前がない場合の照合に位置を使用します。</span><span class="sxs-lookup"><span data-stu-id="62daf-145">Inside the script, you rely on position for matching when the parameters are unnamed:</span></span>

```powershell
param([int]$size, [string]$location)
```

## <a name="how-to-create-a-linux-virtual-machine"></a><span data-ttu-id="62daf-146">Linux 仮想マシンを作成する方法</span><span class="sxs-lookup"><span data-stu-id="62daf-146">How to create a Linux Virtual Machine</span></span>
<span data-ttu-id="62daf-147">Azure PowerShell では、仮想マシンを作成するための **New-AzureRmVm** コマンドレットが提供されます。</span><span class="sxs-lookup"><span data-stu-id="62daf-147">Azure PowerShell provides the **New-AzureRmVm** cmdlet to create a Virtual Machine.</span></span> <span data-ttu-id="62daf-148">コマンドレットには多くのパラメーターがあり、それを使用して数多くの VM 構成設定を処理することができます。</span><span class="sxs-lookup"><span data-stu-id="62daf-148">The cmdlet has many parameters to let it handle the large number of VM configuration settings.</span></span> <span data-ttu-id="62daf-149">ほとんどのパラメーターに適切な既定値があるため、指定する必要があるのは次の 5 つのみです。</span><span class="sxs-lookup"><span data-stu-id="62daf-149">Most of the parameters have reasonable default values so we only need to specify five things:</span></span>

- <span data-ttu-id="62daf-150">**ResourceGroupName**: 新しい VM が配置されるリソース グループ。</span><span class="sxs-lookup"><span data-stu-id="62daf-150">**ResourceGroupName**: The resource group into which the new VM will be placed.</span></span>
- <span data-ttu-id="62daf-151">**Name**: Azure での VM の名前。</span><span class="sxs-lookup"><span data-stu-id="62daf-151">**Name**: The name of the VM in Azure.</span></span>
- <span data-ttu-id="62daf-152">**Location**: VM がプロビジョニングされる地理的な場所。</span><span class="sxs-lookup"><span data-stu-id="62daf-152">**Location**: Geographic location where the VM will be provisioned.</span></span>
- <span data-ttu-id="62daf-153">**Credential**: VM 管理者アカウント用のユーザー名とパスワードが含まれているオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="62daf-153">**Credential**: An object containing the username and password for the VM admin account.</span></span> <span data-ttu-id="62daf-154">ここでは **Get-Credential** を使用します。これはユーザー名とパスワードの入力を求めるコマンドレットです。</span><span class="sxs-lookup"><span data-stu-id="62daf-154">We will use the **Get-Credential** The cmdlet to prompt for a username and password.</span></span> <span data-ttu-id="62daf-155">**Get-Credential** ではユーザー名とパスワードが、結果として返される資格情報オブジェクトにパッケージ化されます。</span><span class="sxs-lookup"><span data-stu-id="62daf-155">**Get-Credential** packages the username and password into a credential object, which it returns as its result.</span></span>
- <span data-ttu-id="62daf-156">**Image**: VM で使用するオペレーティング システムの ID。</span><span class="sxs-lookup"><span data-stu-id="62daf-156">**Image**: Identity of the operating system to use for the VM.</span></span> <span data-ttu-id="62daf-157">ここでは "UbuntuLTS" を使用します。</span><span class="sxs-lookup"><span data-stu-id="62daf-157">We will use "UbuntuLTS".</span></span>

<span data-ttu-id="62daf-158">コマンドレットの構文は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="62daf-158">The syntax for the cmdlet is shown below:</span></span>

```powershell
   New-AzureRmVm 
       -ResourceGroupName <resource group name> 
       -Name <machine name> 
       -Credential <credentials object> 
       -Location <location> 
       -Image <image name>
```

## <a name="summary"></a><span data-ttu-id="62daf-159">まとめ</span><span class="sxs-lookup"><span data-stu-id="62daf-159">Summary</span></span>
<span data-ttu-id="62daf-160">PowerShell と Azure PowerShell を組み合わせることで、Azure の自動化に必要なすべてのツールを利用できます。</span><span class="sxs-lookup"><span data-stu-id="62daf-160">The combination of PowerShell and Azure PowerShell gives you all the tools you need to automate Azure.</span></span> <span data-ttu-id="62daf-161">CRM の例では、スクリプトを汎用的にするためのパラメーターとコードの繰り返しを避けるためのループを使用して、複数の Linux VM を作成することができます。</span><span class="sxs-lookup"><span data-stu-id="62daf-161">In our CRM example, we will be able to create multiple Linux VMs using a parameter to keep the script generic and a loop to avoid repeated code.</span></span> <span data-ttu-id="62daf-162">つまり、これまで複雑だった操作が、1 つの手順で行えるようになります。</span><span class="sxs-lookup"><span data-stu-id="62daf-162">This means that a formerly complex operation can now be executed in a single step.</span></span>