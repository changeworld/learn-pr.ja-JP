<span data-ttu-id="0e2e4-101">このユニットでは、引き続き、Linux 管理ツールを作成している会社の例を扱います。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-101">In this unit, you will continue with the example of a company that makes Linux admin tools.</span></span> <span data-ttu-id="0e2e4-102">Linux VM を使用して自社のソフトウェアを潜在顧客にテストしてもらう計画であることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-102">Recall that you plan to use Linux VMs to let potential customers test your software.</span></span> <span data-ttu-id="0e2e4-103">リソース グループの準備はできているので、次に VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-103">You have a resource group ready and now it is time to create the VMs.</span></span>

<span data-ttu-id="0e2e4-104">会社では Linux の大規模なトレード ショーでブースを確保するための費用を支払いました。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-104">Your company has paid for a booth at a big Linux trade show.</span></span> <span data-ttu-id="0e2e4-105">デモンストレーション領域を設け、そこにはそれぞれ別々の Linux VM に接続された 3 台の端末を設置する計画です。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-105">You plan a demo area containing three terminals each connected to a separate Linux VM.</span></span> <span data-ttu-id="0e2e4-106">1 日の終わりに VM を削除して再作成します。そうすることで、VM は毎朝新しい状態で開始されます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-106">At the end of each day, you want to delete the VMs and recreate them, so they start fresh every morning.</span></span> <span data-ttu-id="0e2e4-107">仕事の後、疲れているときに手動で VM を作成すると、間違いやすくなります。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-107">Creating the VMs manually after work when you are tired would be error prone.</span></span> <span data-ttu-id="0e2e4-108">VM の作成プロセスを自動化するための PowerShell スクリプトを記述します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-108">You want to write a PowerShell script to automate the VM creation process.</span></span>

## <a name="write-a-script-that-creates-virtual-machines"></a><span data-ttu-id="0e2e4-109">仮想マシンを作成するスクリプトを記述する</span><span class="sxs-lookup"><span data-stu-id="0e2e4-109">Write a script that creates Virtual Machines</span></span>

<span data-ttu-id="0e2e4-110">右側の Cloud Shell で次の手順に従って、次のスクリプトを記述します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-110">Follow these steps in the Cloud Shell on the right to write the script:</span></span>

1. <span data-ttu-id="0e2e4-111">Cloud Shell 内のご自分のホーム フォルダーに切り替えます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-111">Switch to your home folder in the Cloud Shell.</span></span>

    ```powershell
    cd $HOME\clouddrive
    ```

1. <span data-ttu-id="0e2e4-112">**ConferenceDailyReset.ps1** という名前の新しいテキスト ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-112">Create a new text file named **ConferenceDailyReset.ps1**.</span></span>

    ```powershell
    touch "./ConferenceDailyReset.ps1"
    ```

1. <span data-ttu-id="0e2e4-113">統合エディターを開いて、**ConferenceDailyReset.ps1** ファイルを選択します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-113">Open the integrated editor and select the **ConferenceDailyReset.ps1** file.</span></span>

    ```powershell
    code "./ConferenceDailyReset.ps1"
    ```
    > [!TIP]
    > <span data-ttu-id="0e2e4-114">vim、nano、および emacs のいずれかを使用する場合、統合された Cloud Shell ではこれらのエディターもサポートされています。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-114">The integrated Cloud Shell also supports vim, nano, and emacs if you'd prefer to use one of those editors.</span></span>

1. <span data-ttu-id="0e2e4-115">変数の入力パラメーターをキャプチャすることから開始します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-115">Start by capturing the input parameter in a variable.</span></span> <span data-ttu-id="0e2e4-116">次の行をスクリプトに追加します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-116">Add the following line to your script:</span></span>

    ```powershell
    param([string]$resourceGroup)
    ```

    > [!NOTE]
    > <span data-ttu-id="0e2e4-117">通常、`Connect-AzureRmAccount` を使用する資格情報を使用して Azure を認証する必要があります。これはスクリプトで行うことができます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-117">Normally, you'd have to authenticate with Azure using your credentials using `Connect-AzureRmAccount` and this could be done in the script.</span></span> <span data-ttu-id="0e2e4-118">ただし、Cloud Shell 環境では、お客様は既に認証されているため、この認証は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-118">However, in the Cloud Shell environment you will already be authenticated so this is unnecessary.</span></span>

1. <span data-ttu-id="0e2e4-119">VM の管理者アカウント用のユーザーとパスワードを求めるプロンプトを表示し、その結果を変数内に取り込みます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-119">Prompt for a username and password for the VM's admin account and capture the result in a variable:</span></span>

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. <span data-ttu-id="0e2e4-120">3 回実行されるループを作成します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-120">Create a loop that executes three times:</span></span>

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. <span data-ttu-id="0e2e4-121">ループの本体で、各 VM の名前を作成し、それを変数内に格納してコンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-121">In the loop body, create a name for each VM and store it in a variable and output it to the console:</span></span>

    ```powershell
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    ```

1. <span data-ttu-id="0e2e4-122">次に、`$vmName` 変数を使用して VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-122">Next, create a VM using the `$vmName` variable:</span></span>

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
   ```

1. <span data-ttu-id="0e2e4-123">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-123">Save the file.</span></span> <span data-ttu-id="0e2e4-124">エディターの右上隅にある [...] メニューを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-124">You can use the "..." menu at the top right corner of the editor.</span></span> <span data-ttu-id="0e2e4-125">[保存] に対する共通のアクセス キーもあります。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-125">There are also common accelerator keys for Save.</span></span>

<span data-ttu-id="0e2e4-126">完成したスクリプトは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-126">The completed script should look like this:</span></span>

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a><span data-ttu-id="0e2e4-127">スクリプトを実行する</span><span class="sxs-lookup"><span data-stu-id="0e2e4-127">Execute the script</span></span>

<span data-ttu-id="0e2e4-128">[...] コンテキスト メニューからエディターを閉じます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-128">Close the editor through the "..." context menu.</span></span>

1. <span data-ttu-id="0e2e4-129">スクリプトを実行します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-129">Execute the script.</span></span>

    ```powershell
    .\ConferenceDailyReset.ps1 <rgn>[Sandbox resource group name]</rgn>
    ```
    
<span data-ttu-id="0e2e4-130">このスクリプトは、完了までに数分かかります。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-130">The script will take several minutes to complete.</span></span> <span data-ttu-id="0e2e4-131">完了したら、ご使用のリソース グループ内にあるリソースを確認することで、スクリプトが正常に実行されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-131">When it is finished, verify that it ran successfully by looking at the resources you now have in your resource group:</span></span>

```powershell
Get-AzureRmResource -ResourceType Microsoft.Compute/virtualMachines
```

<span data-ttu-id="0e2e4-132">それぞれに一意の名前がある、3 つの VM が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-132">You should see three VMs, each with a unique name.</span></span>

<span data-ttu-id="0e2e4-133">スクリプト パラメーターで指定したリソース グループ内に 3 つの VM を自動的に作成するスクリプトを記述しました。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-133">You wrote a script that automated the creation of three VMs in the resource group indicated by a script parameter.</span></span> <span data-ttu-id="0e2e4-134">このスクリプトは短くてシンプルですが、このスクリプトでは、ポータルを使用して手動で行うと完了するまで時間がかかるプロセスを自動化することができます。</span><span class="sxs-lookup"><span data-stu-id="0e2e4-134">The script is short and simple but automates a process that would take a long time to complete manually with the portal.</span></span>