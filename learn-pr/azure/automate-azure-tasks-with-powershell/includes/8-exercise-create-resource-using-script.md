<span data-ttu-id="cc533-101">この演習では、引き続き、Linux 管理ツールを作成している会社の例を扱います。</span><span class="sxs-lookup"><span data-stu-id="cc533-101">In this exercise, you will continue with the example of a company that makes Linux admin tools.</span></span> <span data-ttu-id="cc533-102">Linux VM を使用して自社のソフトウェアを潜在顧客にテストしてもらう計画であることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="cc533-102">Recall that you plan to use Linux VMs to let potential customers test your software.</span></span> <span data-ttu-id="cc533-103">リソース グループの準備はできているので、次に VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="cc533-103">You have a resource group ready and now it is time to create the VMs.</span></span>

<span data-ttu-id="cc533-104">会社では Linux の大規模なトレード ショーでブースを確保するための費用を支払いました。</span><span class="sxs-lookup"><span data-stu-id="cc533-104">Your company has paid for a booth at a big Linux trade show.</span></span> <span data-ttu-id="cc533-105">デモンストレーション領域を設け、そこにはそれぞれ別々の Linux VM に接続された 3 台の端末を設置する計画です。</span><span class="sxs-lookup"><span data-stu-id="cc533-105">You plan a demo area containing three terminals each connected to a separate Linux VM.</span></span> <span data-ttu-id="cc533-106">1 日の終わりに VM を削除して再作成します。そうすることで、VM は毎朝新しい状態で開始されます。</span><span class="sxs-lookup"><span data-stu-id="cc533-106">At the end of each day, you want to delete the VMs and recreate them, so they start fresh every morning.</span></span> <span data-ttu-id="cc533-107">仕事の後、疲れているときに手動で VM を作成すると、間違いやすくなります。</span><span class="sxs-lookup"><span data-stu-id="cc533-107">Creating the VMs manually after work when you are tired would be error prone.</span></span> <span data-ttu-id="cc533-108">VM の作成プロセスを自動化するための PowerShell スクリプトを記述します。</span><span class="sxs-lookup"><span data-stu-id="cc533-108">You want to write a PowerShell script to automate the VM creation process.</span></span>

## <a name="write-a-script-that-creates-virtual-machines"></a><span data-ttu-id="cc533-109">仮想マシンを作成するスクリプトを記述する</span><span class="sxs-lookup"><span data-stu-id="cc533-109">Write a script that creates Virtual Machines</span></span>

<span data-ttu-id="cc533-110">次の手順に従って、スクリプトを記述します。</span><span class="sxs-lookup"><span data-stu-id="cc533-110">Follow these steps to write the script:</span></span>

1. <span data-ttu-id="cc533-111">**ConferenceDailyReset.ps1** という名前の新しいテキスト ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="cc533-111">Create a new text file named **ConferenceDailyReset.ps1**.</span></span>

1. <span data-ttu-id="cc533-112">変数内にパラメーターを取り込みます。</span><span class="sxs-lookup"><span data-stu-id="cc533-112">Capture the parameter in a variable:</span></span>

    ```powershell
    param([string]$resourceGroup)
    ```

1. <span data-ttu-id="cc533-113">ご自分の資格情報を使用して Azure に対する認証を受けます。</span><span class="sxs-lookup"><span data-stu-id="cc533-113">Authenticate with Azure using your credentials:</span></span>

    ```powershell
    Connect-AzureRmAccount
    ```

1. <span data-ttu-id="cc533-114">VM の管理者アカウント用のユーザーとパスワードを求めるプロンプトを表示し、その結果を変数内に取り込みます。</span><span class="sxs-lookup"><span data-stu-id="cc533-114">Prompt for a username and password for the VM's admin account and capture the result in a variable:</span></span>

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. <span data-ttu-id="cc533-115">3 回実行されるループを作成します。</span><span class="sxs-lookup"><span data-stu-id="cc533-115">Create a loop that executes three times:</span></span>

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. <span data-ttu-id="cc533-116">ループの本体で、各 VM の名前を作成し、それを変数内に格納します。</span><span class="sxs-lookup"><span data-stu-id="cc533-116">In the loop body, create a name for each VM and store it in a variable:</span></span>

    ```powershell
    $vmName = "ConferenceDemo" + $i
    ```

1. <span data-ttu-id="cc533-117">次に、`$vmName` 変数を使用して VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="cc533-117">Next, create a VM using the `$vmName` variable:</span></span>

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
   ```

1. <span data-ttu-id="cc533-118">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cc533-118">Save the file.</span></span>

<span data-ttu-id="cc533-119">完成したスクリプトは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="cc533-119">The completed script should look like this:</span></span>

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

Connect-AzureRmAccount

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i

    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a><span data-ttu-id="cc533-120">スクリプトを実行する</span><span class="sxs-lookup"><span data-stu-id="cc533-120">Execute the script</span></span>

<span data-ttu-id="cc533-121">PowerShell を起動し、スクリプト ファイルを保存してあるディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="cc533-121">Launch PowerShell and change to the directory where you saved the script file.</span></span> <span data-ttu-id="cc533-122">スクリプトを実行するには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="cc533-122">To run the script, execute the following command:</span></span>

```powershell
.\ConferenceDailyReset.ps1 TrialsResourceGroup
```

<span data-ttu-id="cc533-123">スクリプトが終了するまでに数分かかる場合があります。</span><span class="sxs-lookup"><span data-stu-id="cc533-123">The script may take a few minutes to complete.</span></span> <span data-ttu-id="cc533-124">スクリプトが終了したら、正常に実行されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="cc533-124">When it is finished, verify that it ran successfully:</span></span>

1. <span data-ttu-id="cc533-125">ブラウザー内で、Azure Portal にサインインします。</span><span class="sxs-lookup"><span data-stu-id="cc533-125">In a browser, sign into the Azure portal.</span></span>

1. <span data-ttu-id="cc533-126">左側のナビゲーションで、**[リソース グループ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc533-126">In the navigation on the left, click **Resource Groups**.</span></span>

1. <span data-ttu-id="cc533-127">リソース グループの一覧で、**[TrialsResourceGroup]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="cc533-127">In the list of resource groups, click **TrialsResourceGroup**.</span></span> <span data-ttu-id="cc533-128">リソースの一覧に、新しく作成された VM とそれに関連するリソースが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cc533-128">In the list of resources, you should see the newly created VMs and their associated resources.</span></span>

## <a name="summary"></a><span data-ttu-id="cc533-129">まとめ</span><span class="sxs-lookup"><span data-stu-id="cc533-129">Summary</span></span>
<span data-ttu-id="cc533-130">スクリプト パラメーターで指定したリソース グループ内に 3 つの VM を自動的に作成するスクリプトを記述しました。</span><span class="sxs-lookup"><span data-stu-id="cc533-130">You wrote a script that automated the creation of three VMs in the resource group indicated by a script parameter.</span></span> <span data-ttu-id="cc533-131">このスクリプトは短くてシンプルですが、このスクリプトでは、ポータルを使用して手動で行うと完了するまで時間がかかるプロセスを自動化することができます。</span><span class="sxs-lookup"><span data-stu-id="cc533-131">The script is short and simple but automates a process that would take a long time to complete manually with the portal.</span></span>