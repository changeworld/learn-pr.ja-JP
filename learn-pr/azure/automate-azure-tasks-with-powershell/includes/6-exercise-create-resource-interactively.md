<span data-ttu-id="18967-101">Linux 管理ツールのスイートを作成する会社で働いているものとします。</span><span class="sxs-lookup"><span data-stu-id="18967-101">Suppose you work at a company that makes a suite of Linux admin tools.</span></span> <span data-ttu-id="18967-102">仕事は、見込みのあるお客様が購入する前にソフトウェアを試すのを支援することです。</span><span class="sxs-lookup"><span data-stu-id="18967-102">Your job is to help potential customers try your software before they buy it.</span></span> <span data-ttu-id="18967-103">ソフトウェアでは OS に対してルート レベルの変更を加えるため、試用版のお客様ごとに Linux VM を作成することにしました。</span><span class="sxs-lookup"><span data-stu-id="18967-103">Because the software makes root-level changes to the OS, you have decided to create a Linux VM for each trial customer.</span></span> <span data-ttu-id="18967-104">必要に応じて VM を作成し、試用サブスクリプションの最後にそれらを削除します。</span><span class="sxs-lookup"><span data-stu-id="18967-104">You create the VMs as needed and delete them at the end of the trial subscription.</span></span> <span data-ttu-id="18967-105">このようにすると、各お客様はクリーンなバージョンの OS で始めることができます。</span><span class="sxs-lookup"><span data-stu-id="18967-105">This way, each customer starts with a clean version of the OS.</span></span> 

<span data-ttu-id="18967-106">これらの VM を、内部テスト用に会社で使用されている VM から分離しておくため、それらを格納するための専用のリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="18967-106">To keep these VMs separate from the VMs your company uses for internal testing, you will create a dedicated resource group to house them.</span></span> <span data-ttu-id="18967-107">必要なリソース グループは 1 つだけなので、このタスクには対話モードの Azure PowerShell を使用するのが妥当です。</span><span class="sxs-lookup"><span data-stu-id="18967-107">You only need one resource group so using Azure PowerShell in interactive mode is a reasonable choice for this task.</span></span>

## <a name="steps-to-create-a-resource-group"></a><span data-ttu-id="18967-108">リソース グループの作成手順</span><span class="sxs-lookup"><span data-stu-id="18967-108">Steps to create a resource group</span></span>

1. <span data-ttu-id="18967-109">PowerShell を起動します。</span><span class="sxs-lookup"><span data-stu-id="18967-109">Launch PowerShell.</span></span>

1. <span data-ttu-id="18967-110">Azure コマンドレットにアクセスできるように、現在のセッションにモジュールをインポートします。</span><span class="sxs-lookup"><span data-stu-id="18967-110">Import the module into the current session so you have access to the Azure cmdlets.</span></span>

   ```powershell
   Import-Module AzureRM
   ```

1. <span data-ttu-id="18967-111">次に示すコマンドを使用して、Azure に接続します。</span><span class="sxs-lookup"><span data-stu-id="18967-111">Connect to Azure using the command shown below.</span></span> <span data-ttu-id="18967-112">コマンドを入力した後、Azure 資格情報を指定して認証を行います。</span><span class="sxs-lookup"><span data-stu-id="18967-112">After entering the command, authenticate by providing your Azure credentials.</span></span>

   ```powershell
   Connect-AzureRmAccount
   ```

1. <span data-ttu-id="18967-113">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="18967-113">Create a resource group.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name "TrialsResourceGroup" -Location "East US"
    ```

1. <span data-ttu-id="18967-114">リソース グループが正常に作成されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="18967-114">Verify the resource group was created successfully.</span></span>

    ```powershell
    Get-AzureRmResource | Format-Table
    ```
<span data-ttu-id="18967-115">リソース グループが正常に作成されたかどうかは、Azure portal を使用して確認することもできます。</span><span class="sxs-lookup"><span data-stu-id="18967-115">Another way to check whether the resource group was created successfully is to use the Azure Portal.</span></span> <span data-ttu-id="18967-116">そのためには、ポータルにログインし、**[リソース グループ]** セクション (下記参照) に移動します。</span><span class="sxs-lookup"><span data-stu-id="18967-116">To do this, login to the Portal and navigate to the **Resource Groups** section (see below).</span></span> <span data-ttu-id="18967-117">新しいリソース グループが一覧に表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="18967-117">The new resource group should be displayed in the list.</span></span>

![ポータルを使用したリソース グループの一覧表示](../media-drafts/6-listing-resource-groups.png)

## <a name="summary"></a><span data-ttu-id="18967-119">まとめ</span><span class="sxs-lookup"><span data-stu-id="18967-119">Summary</span></span>
<span data-ttu-id="18967-120">この演習では、対話型 PowerShell セッションの一般的なパターンを示します。</span><span class="sxs-lookup"><span data-stu-id="18967-120">This exercise shows a common pattern for an interactive PowerShell session.</span></span> <span data-ttu-id="18967-121">標準的なコマンドレットを使用して AzureRM モジュールをインポートしてから、Azure PowerShell コマンドレットを使用して特定のタスクを実行しました。</span><span class="sxs-lookup"><span data-stu-id="18967-121">You used a standard cmdlet to import the AzureRM module and then the Azure PowerShell cmdlets to perform a specific task.</span></span> <span data-ttu-id="18967-122">サブスクリプションにリソース グループが作成されて、VM を作成する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="18967-122">You now have a resource group in your subscription and are ready to create VMs.</span></span>