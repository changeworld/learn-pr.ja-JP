<span data-ttu-id="de715-101">この演習では、Microsoft Azure で仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="de715-101">In this exercise, you will create a virtual network in Microsoft Azure.</span></span> <span data-ttu-id="de715-102">2 つの仮想マシンを作成し、仮想ネットワークを使用して、仮想マシンを相互に、またインターネットに接続します。</span><span class="sxs-lookup"><span data-stu-id="de715-102">You will then create two virtual machines and use the virtual network to connect the virtual machines to each other and to the internet.</span></span>

<span data-ttu-id="de715-103">このユニットを開始する前に、試用版サブスクリプション資格情報を使用して [Azure Cloud Shell](https://shell.azure.com) にログインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="de715-103">Before you begin this unit, you will need to log into [Azure Cloud Shell](https://shell.azure.com) with your trial subscription credentials.</span></span> <span data-ttu-id="de715-104">Azure Cloud Shell を使用して、リソース グループ、仮想ネットワーク、仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="de715-104">We will use Azure Cloud Shell to create the resource groups, virtual networks, and virtual machines.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="de715-105">リソース グループの作成</span><span class="sxs-lookup"><span data-stu-id="de715-105">Create a resource group</span></span>

1. <span data-ttu-id="de715-106">**[Azure Cloud Shell へようこそ]** ウィンドウで **[PowerShell (Linux)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-106">In the **Welcome to Azure Cloud Shell** window, click **PowerShell (Linux)**.</span></span>

1. <span data-ttu-id="de715-107">**[ストレージがマウントされていません]** ウィンドウで、**[ストレージの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-107">In the **you have no storage mounted** window, click **Create Storage**.</span></span>

1. <span data-ttu-id="de715-108">Azure PowerShell コマンド ラインのプロンプトで、次のコードを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="de715-108">In the Azure PowerShell command-line prompt, type the following code and press Enter.</span></span>

    ```PowerShell
    az group create --name myResourceGroup --location eastus
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="de715-109">仮想ネットワークを作成する</span><span class="sxs-lookup"><span data-stu-id="de715-109">Create a virtual network</span></span>

1. <span data-ttu-id="de715-110">仮想ネットワークを作成するには、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="de715-110">To create a virtual network, enter the following command and press Enter.</span></span>

    ```PowerShell
    az network vnet create --name myVirtualNetwork --resource-group myResourceGroup --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a><span data-ttu-id="de715-111">2 つの仮想マシンの作成</span><span class="sxs-lookup"><span data-stu-id="de715-111">Create two virtual machines</span></span>

1. <span data-ttu-id="de715-112">最初の仮想マシンを作成するには、次のコマンドを実行して、ポート 3389 (リモート デスクトップ) 経由でアクセスできるパブリック IP アドレスを持つ Windows VM を作成します。</span><span class="sxs-lookup"><span data-stu-id="de715-112">To create the first virtual machine, execute the following command to create a Windows VM with a public IP address that is accessible over port 3389 (Remote Desktop):</span></span>

    ``` PowerShell
    az vm create --name dataProcessingStage1 --resource-group MyResourceGroup --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. <span data-ttu-id="de715-113">プロンプトで、パスワードの値を指定します。</span><span class="sxs-lookup"><span data-stu-id="de715-113">Supply values for your password at the prompts.</span></span>

1. <span data-ttu-id="de715-114">パブリック IP アドレスがない Windows VM を作成するには次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="de715-114">Execute the following command to create a Windows VM without a public IP address:</span></span>

    ```PowerShell
    az vm create -n dataProcessingStage2 -g MyResourceGroup --public-ip-address '' --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. <span data-ttu-id="de715-115">完了すると、2 番目のコマンドからの出力で publicIpAddress の値が返されます。</span><span class="sxs-lookup"><span data-stu-id="de715-115">When completed, the output from the second command will return a value for publicIpAddress.</span></span>

## <a name="connect-to-dataprocessingstage1-using-remote-desktop"></a><span data-ttu-id="de715-116">リモート デスクトップを使用して dataProcessingStage1 に接続する</span><span class="sxs-lookup"><span data-stu-id="de715-116">Connect to dataProcessingStage1 using Remote Desktop</span></span>

1. <span data-ttu-id="de715-117">クライアント コンピューターで、Windows キーを押し、「RDP」と入力します。</span><span class="sxs-lookup"><span data-stu-id="de715-117">On your client computer, press the Windows key and type RDP.</span></span>

1. <span data-ttu-id="de715-118">**リモート デスクトップ接続**アプリが選択されていることを確認し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="de715-118">Ensure that **Remote Desktop Connection** app is selected, and then press Enter.</span></span>

1. <span data-ttu-id="de715-119">**[リモート デスクトップ接続]** ダイアログ ボックスの **[コンピューター]** フィールドで、dataProcessingStage1PublicIPAddress の値を入力してから、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-119">In the **Remote Desktop Connection** dialog box, in the **Computer** field, enter the value of dataProcessingStage1PublicIPAddress, and then click **Connect**.</span></span>

1. <span data-ttu-id="de715-120">**[Do you trust this remote connection?]\(このリモート接続を信頼しますか?\)** ダイアログ ボックスで、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-120">In the **Do you trust this remote connection?** dialog box, click **Connect**.</span></span>

1. <span data-ttu-id="de715-121">**[Windows セキュリティ]** ダイアログ ボックスで、dataProcessingStage1 の作成時に使用したユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="de715-121">In the **Windows Security** dialog box, enter the user name and password you used when you created dataProcessingStage1.</span></span>

1. <span data-ttu-id="de715-122">**[リモート デスクトップ接続]** ダイアログ ボックスで、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-122">In the **Remote Desktop Connection** dialog box, click **OK**.</span></span>

1. <span data-ttu-id="de715-123">Azure 内でリモート コンピューターにサインインします。</span><span class="sxs-lookup"><span data-stu-id="de715-123">Sign in to the remote computer in Azure.</span></span>

1. <span data-ttu-id="de715-124">"**ネットワーク**" というメッセージが表示されたら、**[いいえ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-124">When the **Networks** message appears, click **No**.</span></span>

1. <span data-ttu-id="de715-125">サーバー マネージャーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="de715-125">Close Server Manager.</span></span>

1. <span data-ttu-id="de715-126">リモート セッションで Windows キーを右クリックし、**[コマンド プロンプト]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-126">In the remote session, right-click the Windows key and click **Command Prompt**.</span></span>

1. <span data-ttu-id="de715-127">コマンド プロンプト ウィンドウで、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="de715-127">In the command prompt window, type the following command and press Enter.</span></span>

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. <span data-ttu-id="de715-128">リモート コンピューターからの応答はありません。</span><span class="sxs-lookup"><span data-stu-id="de715-128">There should be no response from the remote computer.</span></span> <span data-ttu-id="de715-129">これは、既定では Windows ファイアウォールが ICMP 応答を阻止するからです。</span><span class="sxs-lookup"><span data-stu-id="de715-129">This is because by default, Windows Firewall prevents ICMP responses.</span></span>

## <a name="connect-to-dataprocessingstage2-using-remote-desktop"></a><span data-ttu-id="de715-130">リモート デスクトップを使用して dataProcessingStage2 に接続する</span><span class="sxs-lookup"><span data-stu-id="de715-130">Connect to dataProcessingStage2 using Remote Desktop</span></span>

1. <span data-ttu-id="de715-131">クライアント コンピューターで、Windows キーを押し、「**RDP**」と入力します。</span><span class="sxs-lookup"><span data-stu-id="de715-131">On your client computer, press the Windows key and type **RDP**.</span></span> <span data-ttu-id="de715-132">**[リモート デスクトップ接続]** アプリを選択し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="de715-132">Select the **Remote Desktop Connection** app and press Enter.</span></span>

1. <span data-ttu-id="de715-133">**[コンピューター]** フィールドで、dataProcessingStage2PublicIPAddress の値を入力してから、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-133">In the **Computer** field, enter the value of dataProcessingStage2PublicIPAddress, and then click **Connect**.</span></span>

1. <span data-ttu-id="de715-134">**[Do you trust this remote connection?]\(このリモート接続を信頼しますか?\)** ダイアログ ボックスで、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-134">In the **Do you trust this remote connection?** dialog box, click **Connect**.</span></span>

1. <span data-ttu-id="de715-135">**[Windows セキュリティ]** ダイアログ ボックスで、dataProcessingStage2 の作成時に使用したユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="de715-135">In the **Windows Security** dialog box, enter the user name and password you used when you created dataProcessingStage2.</span></span>

1. <span data-ttu-id="de715-136">**[リモート デスクトップ接続]** ダイアログ ボックスで、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-136">In the **Remote Desktop Connection** dialog box, click **OK**.</span></span> <span data-ttu-id="de715-137">ここで、Azure 内でリモート コンピューターにサインインします。</span><span class="sxs-lookup"><span data-stu-id="de715-137">You now sign in to the remote computer in Azure.</span></span>

1. <span data-ttu-id="de715-138">"**ネットワーク**" というメッセージが表示されたら、**[いいえ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-138">When the **Networks** message appears, click **No**.</span></span>

1. <span data-ttu-id="de715-139">サーバー マネージャーを閉じます。</span><span class="sxs-lookup"><span data-stu-id="de715-139">Close Server Manager.</span></span>

1. <span data-ttu-id="de715-140">dataProcessingStage2 で、Windows キーを押して「**ファイアウォール**」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="de715-140">On dataProcessingStage2, press the Windows key, type **Firewall**, and press Enter.</span></span> <span data-ttu-id="de715-141">「**セキュリティが強化された Windows ファイアウォール**」コンソールが表示されます。</span><span class="sxs-lookup"><span data-stu-id="de715-141">The **Windows Firewall with Advanced Security** console appears.</span></span>

1. <span data-ttu-id="de715-142">左側のウィンドウで、**[受信規則]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-142">In the left-hand pane, click **Inbound Rules**.</span></span>

1. <span data-ttu-id="de715-143">右側のウィンドウで下にスクロールし、**[ファイルとプリンターの共有 (エコー要求 - ICMPv4 受信)]** を右クリックし、**[規則の有効化]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="de715-143">In the right-hand pane, scroll down and right-click **File and Printer Sharing (Echo Request - ICMPv4-In)**, and then click **Enable Rule**.</span></span>

1. <span data-ttu-id="de715-144">dataProcessingStage1 コンソールに戻り、コマンド プロンプト ウィンドウで、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="de715-144">Switch back to the dataProcessingStage1 console and in the command prompt window, type the following command and press Enter.</span></span>

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. <span data-ttu-id="de715-145">dataProcessingStage2 は、4 つの返信で応答し、2 つの VM 間の接続を示します。</span><span class="sxs-lookup"><span data-stu-id="de715-145">dataProcessingStage2 responds with four replies, demonstrating connectivity between the two VMs.</span></span>

## <a name="summary"></a><span data-ttu-id="de715-146">まとめ</span><span class="sxs-lookup"><span data-stu-id="de715-146">Summary</span></span>

<span data-ttu-id="de715-147">仮想ネットワークを正常に作成し、その仮想ネットワークに接続される 2 つの VM を作成し、一方の VM に接続して、同じ仮想ネットワーク内の他の VM へのネットワーク接続を表示しました。</span><span class="sxs-lookup"><span data-stu-id="de715-147">You have successfully created a virtual network, created two VMs that are attached to that virtual network, connected to one of the VMs and shown network connectivity to the other VM within the same virtual network.</span></span> <span data-ttu-id="de715-148">Azure Virtual Network を使用して、Azure ネットワーク内のリソースを接続できます。</span><span class="sxs-lookup"><span data-stu-id="de715-148">You can use Azure Virtual Network to connect resources within the Azure network.</span></span> <span data-ttu-id="de715-149">ただし、これらのリソースは、同じリソース グループとサブスクリプションに含まれる必要があります。</span><span class="sxs-lookup"><span data-stu-id="de715-149">However, those resources need to be within the same resource group and subscription.</span></span> <span data-ttu-id="de715-150">次に、VPN ゲートウェイを参照します。これにより、別のリソース グループ、サブスクリプション、地理的リージョン内の仮想ネットワークを接続できます。</span><span class="sxs-lookup"><span data-stu-id="de715-150">Next, we will look at VPN gateways, which enable you to connect virtual network in different resource groups, subscriptions, and even geographical regions.</span></span>
