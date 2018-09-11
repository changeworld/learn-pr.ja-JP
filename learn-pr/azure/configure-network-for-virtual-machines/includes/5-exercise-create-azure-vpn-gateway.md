<span data-ttu-id="ab690-101">パブリック インターネット経由で暗号化されたトンネルを使用して環境内のクライアントまたはサイトを Azure に接続できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ab690-101">You want to ensure that you can connect clients or sites within your environment into Azure using encrypted tunnels across the public Internet.</span></span> <span data-ttu-id="ab690-102">この演習では、ポイント対サイト VPN ゲートウェイを作成し、クライアント コンピューターからそのゲートウェイに接続します。</span><span class="sxs-lookup"><span data-stu-id="ab690-102">In this exercise, you'll create a point-to-site VPN gateway, and then connect to that gateway from a client computer.</span></span> <span data-ttu-id="ab690-103">セキュリティを確保するためにネイティブの Azure 証明書認証接続を使用します。</span><span class="sxs-lookup"><span data-stu-id="ab690-103">You'll use native Azure certificate authentication connections for security.</span></span>

<span data-ttu-id="ab690-104">次のプロセスを実行します。</span><span class="sxs-lookup"><span data-stu-id="ab690-104">You will carry out the following process:</span></span>

1. <span data-ttu-id="ab690-105">RouteBased VPN ゲートウェイを作成する</span><span class="sxs-lookup"><span data-stu-id="ab690-105">Create a RouteBased VPN gateway</span></span>

1. <span data-ttu-id="ab690-106">認証の目的でルート証明書の公開キーをアップロードする</span><span class="sxs-lookup"><span data-stu-id="ab690-106">Upload the public key for a root certificate for authentication purposes</span></span>

1. <span data-ttu-id="ab690-107">ルート証明書からクライアント証明書を生成し、認証の目的で、VNet に接続する各クライアント コンピューターにクライアント証明書をインストールする</span><span class="sxs-lookup"><span data-stu-id="ab690-107">Generate a client certificate from the root certificate, then install the client certificate on each client computer that will connect to the VNet for authentication purposes</span></span>

1. <span data-ttu-id="ab690-108">クライアントが VNet に接続するために必要な情報を含む VPN クライアント構成ファイルを作成する。</span><span class="sxs-lookup"><span data-stu-id="ab690-108">Create VPN client configuration files, which contain the necessary information for the client to connect to the VNet.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ab690-109">開始する前に</span><span class="sxs-lookup"><span data-stu-id="ab690-109">Before you begin</span></span>

<span data-ttu-id="ab690-110">このラボを完了するには、以下が必要です。</span><span class="sxs-lookup"><span data-stu-id="ab690-110">To complete this lab, you must have:</span></span>

- <span data-ttu-id="ab690-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ab690-111">Azure PowerShell</span></span>

- <span data-ttu-id="ab690-112">C:\cert という名前のフォルダー</span><span class="sxs-lookup"><span data-stu-id="ab690-112">A folder called C:\cert</span></span>

<span data-ttu-id="ab690-113">Azure PowerShell をインストールするには:</span><span class="sxs-lookup"><span data-stu-id="ab690-113">To install Azure PowerShell:</span></span>

1. <span data-ttu-id="ab690-114">Windows ボタンを右クリックし、**[PowerShell (管理者)]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="ab690-114">Right-click the Windows button and click **PowerShell (Admin)**</span></span>

1. <span data-ttu-id="ab690-115">**[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="ab690-115">In the **User Account Control** message box, click **Yes**</span></span>

1. <span data-ttu-id="ab690-116">PowerShell ウィンドウで、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-116">In the PowerShell window, type the following command and press Enter:</span></span>

    ```PowerShell
    Import-Module AzureRM
    ```

1. <span data-ttu-id="ab690-117">セキュリティのプロンプトで、「A」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-117">At the security prompt, type A and press Enter.</span></span>

## <a name="sign-in-and-set-variables"></a><span data-ttu-id="ab690-118">サインインと変数の設定</span><span class="sxs-lookup"><span data-stu-id="ab690-118">Sign in and set variables</span></span>

<span data-ttu-id="ab690-119">サインインし、変数を設定するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="ab690-119">To sign in and set variables, carry out the following steps:</span></span>

1. <span data-ttu-id="ab690-120">次のコマンドを入力して Enter キーを押すことで、Azure に接続します。</span><span class="sxs-lookup"><span data-stu-id="ab690-120">Connect to Azure by entering the following command and pressing Enter:</span></span>

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. <span data-ttu-id="ab690-121">Azure サブスクリプションの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="ab690-121">Get a list of your Azure subscriptions.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
1. <span data-ttu-id="ab690-122">使用するサブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="ab690-122">Specify the subscription that you want to use.</span></span>

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    <span data-ttu-id="ab690-123">"Name of subscription" をサブスクリプション名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="ab690-123">Replace "Name of subscription" with your subscription name.</span></span>

1. <span data-ttu-id="ab690-124">次の変数を入力し、それぞれの入力後に Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-124">Enter the following variables and press Enter after each one.</span></span>

    ```PowerShell
    $VNetName  = "VNetData"
    $FESubName = "FrontEnd"
    $BESubName = "Backend"
    $GWSubName = "GatewaySubnet"
    $VNetPrefix1 = "192.168.0.0/16"
    $VNetPrefix2 = "10.254.0.0/16"
    $FESubPrefix = "192.168.1.0/24"
    $BESubPrefix = "10.254.1.0/24"
    $GWSubPrefix = "192.168.200.0/26"
    $VPNClientAddressPool = "172.16.201.0/24"
    $RG = "TestRG"
    $Location = "East US"
    $GWName = "VNetDataGW"
    $GWIPName = "VNetDataGWPIP"
    $GWIPconfName = "gwipconf"
    ```

## <a name="configure-a-vnet"></a><span data-ttu-id="ab690-125">VNet の構成</span><span class="sxs-lookup"><span data-stu-id="ab690-125">Configure a vNet</span></span>

1. <span data-ttu-id="ab690-126">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="ab690-126">Create a resource group.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. <span data-ttu-id="ab690-127">仮想ネットワークのサブネット構成を作成します。</span><span class="sxs-lookup"><span data-stu-id="ab690-127">Create subnet configurations for the virtual network.</span></span> <span data-ttu-id="ab690-128">これらの名前は **FrontEnd、BackEnd**、**GatewaySubnet** です。</span><span class="sxs-lookup"><span data-stu-id="ab690-128">These have the name **FrontEnd, BackEnd**, and **GatewaySubnet**.</span></span> <span data-ttu-id="ab690-129">これらすべてのサブネットが仮想ネットワークのプレフィックス内に存在します。</span><span class="sxs-lookup"><span data-stu-id="ab690-129">All of these subnets exist within the virtual network prefix.</span></span>

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. <span data-ttu-id="ab690-130">サブネットの値と静的 DNS サーバーを使用して仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="ab690-130">Create the virtual network using the subnet values and a static DNS server.</span></span> 
    > [!IMPORTANT]
    > <span data-ttu-id="ab690-131">警告メッセージを無視し、コマンドが完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="ab690-131">Ignore the warning message, then wait for the command to complete.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. <span data-ttu-id="ab690-132">ここで、先ほど作成したこのネットワークでの変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="ab690-132">Now specify the variables for this network that you have just created.</span></span>

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. <span data-ttu-id="ab690-133">動的に割り当てられたパブリック IP アドレスを要求します。</span><span class="sxs-lookup"><span data-stu-id="ab690-133">Request a dynamically-assigned public IP address.</span></span>

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a><span data-ttu-id="ab690-134">VPN ゲートウェイを作成する</span><span class="sxs-lookup"><span data-stu-id="ab690-134">Create the VPN gateway</span></span>

<span data-ttu-id="ab690-135">この VPN ゲートウェイを作成する場合:</span><span class="sxs-lookup"><span data-stu-id="ab690-135">When creating this VPN gateway:</span></span>

- <span data-ttu-id="ab690-136">GatewayType は Vpn である必要があります</span><span class="sxs-lookup"><span data-stu-id="ab690-136">GatewayType must be Vpn</span></span>
- <span data-ttu-id="ab690-137">VpnTpe は RouteBased である必要があります</span><span class="sxs-lookup"><span data-stu-id="ab690-137">VpnTpe must be RouteBased</span></span>

> [!NOTE]
> <span data-ttu-id="ab690-138">演習のこの部分は、ゲートウェイ sku の値によっては完了まで最大 45 分かかることにご注意ください</span><span class="sxs-lookup"><span data-stu-id="ab690-138">Note that this part of the exercise can take up to 45 minutes to complete, depending on the value of gateway sku</span></span>

1. <span data-ttu-id="ab690-139">VPN ゲートウェイを作成するには、次のコマンドを実行し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-139">To create the VPN gateway, run the following command and press Enter.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. <span data-ttu-id="ab690-140">コマンド出力が表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="ab690-140">Wait for the command output to appear.</span></span>

## <a name="add-the-vpn-client-address-pool"></a><span data-ttu-id="ab690-141">VPN クライアント アドレス プールの追加</span><span class="sxs-lookup"><span data-stu-id="ab690-141">Add the VPN client address pool</span></span>

1. <span data-ttu-id="ab690-142">次のコマンドを実行し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-142">Run the following command and press Enter.</span></span>

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. <span data-ttu-id="ab690-143">コマンド出力が表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="ab690-143">Wait for the command output to appear.</span></span>

## <a name="generate-a-client-certificate"></a><span data-ttu-id="ab690-144">クライアント証明書の生成</span><span class="sxs-lookup"><span data-stu-id="ab690-144">Generate a client certificate</span></span>

1. <span data-ttu-id="ab690-145">自己署名ルート証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="ab690-145">Create the self-signed root certificate.</span></span>

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. <span data-ttu-id="ab690-146">クライアント証明書を生成します。</span><span class="sxs-lookup"><span data-stu-id="ab690-146">Generate a client certificate.</span></span>

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. <span data-ttu-id="ab690-147">ルート証明書の公開キーをエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="ab690-147">Export the root certificate public key.</span></span> <span data-ttu-id="ab690-148">クライアント コンピューターで、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-148">On your client computer, type the following command and press Enter.</span></span>

    ```PowerShell
    certmgr
    ```

1. <span data-ttu-id="ab690-149">[個人]/[証明書] に移動します。</span><span class="sxs-lookup"><span data-stu-id="ab690-149">Navigate to Personal/Certificates.</span></span> <span data-ttu-id="ab690-150">P2SRootCert を右クリックし、**[すべてのタスク]** をクリックしてから **[エクスポート]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ab690-150">Right-click P2SRootCert, click **All tasks**, then select **Export**.</span></span>

1. <span data-ttu-id="ab690-151">証明書のエクスポート ウィザードで **[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-151">In the Certificate Export Wizard, click **Next**.</span></span>

1. <span data-ttu-id="ab690-152">**[いいえ、秘密キーをエクスポートしません]** が選択されていることを確認し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-152">Ensure that **No, do not export the private key** is selected, then click **Next**.</span></span>

1. <span data-ttu-id="ab690-153">**[エクスポート ファイルの形式]** ページで **[Base-64 encoded X.509 (.CER)]** が選択されていることを確認し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-153">On the **Export File Format** page, ensure that **Base-64 encoded X.509 (.CER)** is selected, then click **Next**.</span></span>

1. <span data-ttu-id="ab690-154">**[エクスポートするファイル]** ページの **[ファイル名]** で、「**C:\cert\P2SRootCert.cer**」と入力して [次へ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-154">In the **File to Export** page, under **File name**, enter **C:\cert\P2SRootCert.cer**, then click Next.</span></span>

1. <span data-ttu-id="ab690-155">**[証明書のエクスポート ウィザードを完了しています]** ページで、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-155">On the **Completing the Certificate Export Wizard** page, click **Finish**.</span></span>

1. <span data-ttu-id="ab690-156">**[証明書のエクスポート ウィザード]** メッセージ ボックスで **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-156">On the **Certificate Export Wizard** message box, click **OK**.</span></span>

## <a name="upload-the-root-certificate-public-key-information"></a><span data-ttu-id="ab690-157">ルート証明書の公開キー情報のアップロード</span><span class="sxs-lookup"><span data-stu-id="ab690-157">Upload the root certificate public key information</span></span>

1. <span data-ttu-id="ab690-158">PowerShell ウィンドウで、次のコマンドを実行して証明書名の変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="ab690-158">In the PowerShell window, execute the following command to declare a variable for the certificate name:</span></span>

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. <span data-ttu-id="ab690-159">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="ab690-159">Execute the following command:</span></span>

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. <span data-ttu-id="ab690-160">ここで Azure に証明書をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="ab690-160">Now upload the certificate to Azure.</span></span> <span data-ttu-id="ab690-161">Azure では、それが信頼されたルート証明書として認識されます。</span><span class="sxs-lookup"><span data-stu-id="ab690-161">Azure then recognizes it as a trusted root certificate.</span></span>

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## <a name="configure-the-native-vpn-client"></a><span data-ttu-id="ab690-162">ネイティブ VPN クライアントの構成</span><span class="sxs-lookup"><span data-stu-id="ab690-162">Configure the native VPN client</span></span>

1. <span data-ttu-id="ab690-163">次のコマンドを実行して、VPN クライアント構成ファイルを .ZIP 形式で作成します。</span><span class="sxs-lookup"><span data-stu-id="ab690-163">Execute the following command to create VPN Client configuration files in .ZIP format.</span></span>

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. <span data-ttu-id="ab690-164">このコマンドからの出力で返される URL をコピーして、お使いのブラウザーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="ab690-164">Copy the URL returned in the output from this command and paste it into your Browser.</span></span> <span data-ttu-id="ab690-165">お使いのブラウザーが .ZIP ファイルのダウンロードを開始します。</span><span class="sxs-lookup"><span data-stu-id="ab690-165">Your browser should start downloading a .ZIP file.</span></span> <span data-ttu-id="ab690-166">これを解凍し、適切な場所に配置します。</span><span class="sxs-lookup"><span data-stu-id="ab690-166">Unzip it and put it in a suitable location.</span></span>

1. <span data-ttu-id="ab690-167">抽出したフォルダー内で、WindowsAmd64 フォルダー (64 ビット Windows コンピューターの場合) または WindowsZX86 (32 ビット コンピューターの場合) に移動します。</span><span class="sxs-lookup"><span data-stu-id="ab690-167">In the extracted folder, navigate to either the WindowsAmd64 folder (for 64-bit Windows computers) or to the WindowsZX86 (for 32-bit computers).</span></span>

1. <span data-ttu-id="ab690-168">アーキテクチャによっては、VpnClientSetupxxxxx.exe ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-168">Double-click on the VpnClientSetupxxxxx.exe file, depending on your architecture.</span></span>

1. <span data-ttu-id="ab690-169">**[Windows によって PC が保護されました]** 画面で、**[詳細情報]** をクリックし、**[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-169">In the **Windows protected your PC** screen, click **More info**, then click **Run anyway**.</span></span>

1. <span data-ttu-id="ab690-170">**[ユーザー アカウント制御]** ダイアログ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-170">In the **User Account Control** dialog box, click **Yes**.</span></span>

1. <span data-ttu-id="ab690-171">**[VNetData]** ダイアログ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-171">In the **VNetData** dialog box, click **Yes**.</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="ab690-172">Azure への接続</span><span class="sxs-lookup"><span data-stu-id="ab690-172">Connect to Azure</span></span>

1. <span data-ttu-id="ab690-173">Windows キーを押し、「**設定**」と入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-173">Press the Windows key, type **Settings** and press Enter.</span></span>

1. <span data-ttu-id="ab690-174">**[設定]** ウィンドウで、**[ネットワークとインターネット]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-174">In the **Settings** window, click **Network and Internet**.</span></span>

1. <span data-ttu-id="ab690-175">左側のウィンドウで、**[VPN]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-175">In the left-hand pane, click **VPN**.</span></span>

1. <span data-ttu-id="ab690-176">右側のウィンドウで、**[VNetData]** をクリックし、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-176">In the right-hand pane, click **VNetData**, then click **Connect**.</span></span>

1. <span data-ttu-id="ab690-177">[VNetData] ウィンドウで、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-177">In the VNetData window, click **Connect**.</span></span>

1. <span data-ttu-id="ab690-178">[VNetData] ウィンドウで、**[続行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-178">In the next VNetData window, click **Continue**.</span></span>

1. <span data-ttu-id="ab690-179">**[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ab690-179">In the **User Account Control** message box, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="ab690-180">これらの手順が機能しない場合は、コンピューターの再起動が必要な可能性があります</span><span class="sxs-lookup"><span data-stu-id="ab690-180">If these steps do not work, you may need to restart your computer</span></span>

## <a name="verify-your-connection"></a><span data-ttu-id="ab690-181">接続を確認する</span><span class="sxs-lookup"><span data-stu-id="ab690-181">Verify your connection</span></span>

1. <span data-ttu-id="ab690-182">Windows キーを押し、「**cmd**」と入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-182">Press the Windows key, type **cmd** and press Enter.</span></span> <span data-ttu-id="ab690-183">**[コマンド プロンプト]** ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="ab690-183">The **Command Prompt** window appears.</span></span>

1. <span data-ttu-id="ab690-184">「`IPCONFIG /ALL`」と入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="ab690-184">Type `IPCONFIG /ALL` and press Enter.</span></span>

1. <span data-ttu-id="ab690-185">PPP アダプター VNetData の下の IP アドレスをコピーするか、メモしておきます。</span><span class="sxs-lookup"><span data-stu-id="ab690-185">Copy the IP address under PPP adapter VNetData, or write it down.</span></span>

1. <span data-ttu-id="ab690-186">IP アドレスが **172.16.201.0/24 の VPNClientAddressPool 範囲**内にあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="ab690-186">Confirm that IP address is in the **VPNClientAddressPool range of 172.16.201.0/24**.</span></span>

1. <span data-ttu-id="ab690-187">Azure VPN ゲートウェイへの接続が正常に行われました。</span><span class="sxs-lookup"><span data-stu-id="ab690-187">You have successfully made a connection to the Azure VPN gateway.</span></span>

## <a name="summary"></a><span data-ttu-id="ab690-188">まとめ</span><span class="sxs-lookup"><span data-stu-id="ab690-188">Summary</span></span>

<span data-ttu-id="ab690-189">VPN ゲートウェイを設定したので、Azure 内の vNet に暗号化されたクライアント接続を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="ab690-189">You just set up a VPN gateway, allowing you to make an encrypted client connection to a vNet in Azure.</span></span> <span data-ttu-id="ab690-190">この方法は、クライアント コンピューターと小規模なサイト間接続に優れています。</span><span class="sxs-lookup"><span data-stu-id="ab690-190">This approach is great with client computers and smaller site-to-site connections.</span></span>