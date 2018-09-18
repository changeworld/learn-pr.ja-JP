<span data-ttu-id="833ea-101">パブリック インターネット経由で暗号化されたトンネルを使用して、環境内のクライアントまたはサイトを Azure に接続できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="833ea-101">You want to ensure that you can connect clients or sites within your environment into Azure using encrypted tunnels across the public internet.</span></span> <span data-ttu-id="833ea-102">このユニットでは、ポイント対サイト VPN ゲートウェイを作成し、クライアント コンピューターからそのゲートウェイに接続します。</span><span class="sxs-lookup"><span data-stu-id="833ea-102">In this unit, you'll create a point-to-site VPN gateway, and then connect to that gateway from a client computer.</span></span> <span data-ttu-id="833ea-103">セキュリティを確保するためにネイティブの Azure 証明書認証接続を使用します。</span><span class="sxs-lookup"><span data-stu-id="833ea-103">You'll use native Azure certificate authentication connections for security.</span></span>

<span data-ttu-id="833ea-104">次のプロセスを実行します。</span><span class="sxs-lookup"><span data-stu-id="833ea-104">You will carry out the following process:</span></span>

1. <span data-ttu-id="833ea-105">RouteBased VPN ゲートウェイを作成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-105">Create a RouteBased VPN gateway.</span></span>

1. <span data-ttu-id="833ea-106">認証の目的でルート証明書の公開キーをアップロードします。</span><span class="sxs-lookup"><span data-stu-id="833ea-106">Upload the public key for a root certificate for authentication purposes.</span></span>

1. <span data-ttu-id="833ea-107">ルート証明書からクライアント証明書を生成し、その後、認証の目的で仮想ネットワークに接続する各クライアント コンピューターにクライアント証明書をインストールします。</span><span class="sxs-lookup"><span data-stu-id="833ea-107">Generate a client certificate from the root certificate, and then install the client certificate on each client computer that will connect to the virtual network for authentication purposes.</span></span>

1. <span data-ttu-id="833ea-108">クライアントが仮想ネットワークに接続するために必要な情報を含む、VPN クライアント構成ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-108">Create VPN client configuration files, which contain the necessary information for the client to connect to the virtual network.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="833ea-109">開始する前に</span><span class="sxs-lookup"><span data-stu-id="833ea-109">Before you begin</span></span>
<!---TODO: These should be prerequisites in the first unit and on the index.yml--->

<span data-ttu-id="833ea-110">このモジュールを完了するには、以下が必要です。</span><span class="sxs-lookup"><span data-stu-id="833ea-110">To complete this module, you must have:</span></span>

- <span data-ttu-id="833ea-111">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="833ea-111">Azure PowerShell</span></span>

- <span data-ttu-id="833ea-112">C:\cert という名前のフォルダー</span><span class="sxs-lookup"><span data-stu-id="833ea-112">A folder called C:\cert</span></span>

<span data-ttu-id="833ea-113">Azure PowerShell をインストールするには:</span><span class="sxs-lookup"><span data-stu-id="833ea-113">To install Azure PowerShell:</span></span>

1. <span data-ttu-id="833ea-114">Windows ボタンを右クリックし、**[PowerShell (管理者)]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-114">Right-click the Windows button and click **PowerShell (Admin)**.</span></span>

1. <span data-ttu-id="833ea-115">**[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-115">In the **User Account Control** message box, click **Yes**.</span></span>

1. <span data-ttu-id="833ea-116">PowerShell ウィンドウで、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-116">In the PowerShell window, type the following command and press Enter:</span></span>

    ```PowerShell
    Import-Module AzureRM
    ```

1. <span data-ttu-id="833ea-117">セキュリティのプロンプトで、「A」と入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-117">At the security prompt, type A and press Enter.</span></span>

## <a name="sign-in-and-set-variables"></a><span data-ttu-id="833ea-118">サインインと変数の設定</span><span class="sxs-lookup"><span data-stu-id="833ea-118">Sign in and set variables</span></span>

<span data-ttu-id="833ea-119">サインインし、変数を設定するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="833ea-119">To sign in and set variables, carry out the following steps:</span></span>

1. <span data-ttu-id="833ea-120">次のコマンドを入力して Enter キーを押すことで、Azure に接続します。</span><span class="sxs-lookup"><span data-stu-id="833ea-120">Connect to Azure by entering the following command and pressing Enter:</span></span>

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. <span data-ttu-id="833ea-121">Azure サブスクリプションの一覧を取得します。</span><span class="sxs-lookup"><span data-stu-id="833ea-121">Get a list of your Azure subscriptions.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
1. <span data-ttu-id="833ea-122">使用するサブスクリプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="833ea-122">Specify the subscription that you want to use.</span></span>

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    <span data-ttu-id="833ea-123">"Name of subscription" をサブスクリプション名に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="833ea-123">Replace "Name of subscription" with your subscription name.</span></span>

1. <span data-ttu-id="833ea-124">次の変数を入力し、それぞれの入力後に Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-124">Enter the following variables and press Enter after each one.</span></span>

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

## <a name="configure-a-virtual-network"></a><span data-ttu-id="833ea-125">仮想ネットワークを構成する</span><span class="sxs-lookup"><span data-stu-id="833ea-125">Configure a virtual network</span></span>

1. <span data-ttu-id="833ea-126">リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-126">Create a resource group.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. <span data-ttu-id="833ea-127">仮想ネットワークのサブネット構成を作成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-127">Create subnet configurations for the virtual network.</span></span> <span data-ttu-id="833ea-128">これらの名前は **FrontEnd、BackEnd**、**GatewaySubnet** です。</span><span class="sxs-lookup"><span data-stu-id="833ea-128">These have the name **FrontEnd, BackEnd**, and **GatewaySubnet**.</span></span> <span data-ttu-id="833ea-129">これらすべてのサブネットが仮想ネットワークのプレフィックス内に存在します。</span><span class="sxs-lookup"><span data-stu-id="833ea-129">All of these subnets exist within the virtual network prefix.</span></span>

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. <span data-ttu-id="833ea-130">サブネットの値と静的 DNS サーバーを使用して、仮想ネットワークを作成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-130">Create the virtual network using the subnet values and a static DNS server.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="833ea-131">警告メッセージを無視し、コマンドが完了するまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="833ea-131">Ignore the warning message, and then wait for the command to finish.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. <span data-ttu-id="833ea-132">ここで、先ほど作成したこのネットワークに対する変数を指定します。</span><span class="sxs-lookup"><span data-stu-id="833ea-132">Now specify the variables for this network that you have just created.</span></span>

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. <span data-ttu-id="833ea-133">動的に割り当てられたパブリック IP アドレスを要求します。</span><span class="sxs-lookup"><span data-stu-id="833ea-133">Request a dynamically assigned public IP address.</span></span>

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a><span data-ttu-id="833ea-134">VPN ゲートウェイを作成する</span><span class="sxs-lookup"><span data-stu-id="833ea-134">Create the VPN gateway</span></span>

<span data-ttu-id="833ea-135">この VPN ゲートウェイを作成する場合:</span><span class="sxs-lookup"><span data-stu-id="833ea-135">When creating this VPN gateway:</span></span>

- <span data-ttu-id="833ea-136">GatewayType は Vpn である必要があります</span><span class="sxs-lookup"><span data-stu-id="833ea-136">GatewayType must be Vpn</span></span>
- <span data-ttu-id="833ea-137">VpnType は RouteBased である必要があります</span><span class="sxs-lookup"><span data-stu-id="833ea-137">VpnType must be RouteBased</span></span>

> [!NOTE]
> <span data-ttu-id="833ea-138">演習のこの部分は、GatewaySku の値によっては、完了までに最大 45 分かかることに注意ください。</span><span class="sxs-lookup"><span data-stu-id="833ea-138">Note that this part of the exercise can take up to 45 minutes to complete, depending on the value of GatewaySku.</span></span>

1. <span data-ttu-id="833ea-139">VPN ゲートウェイを作成するには、次のコマンドを実行し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-139">To create the VPN gateway, run the following command and press Enter.</span></span>

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. <span data-ttu-id="833ea-140">コマンド出力が表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="833ea-140">Wait for the command output to appear.</span></span>

## <a name="add-the-vpn-client-address-pool"></a><span data-ttu-id="833ea-141">VPN クライアント アドレス プールの追加</span><span class="sxs-lookup"><span data-stu-id="833ea-141">Add the VPN client address pool</span></span>

1. <span data-ttu-id="833ea-142">次のコマンドを実行し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-142">Run the following command and press Enter.</span></span>

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. <span data-ttu-id="833ea-143">コマンド出力が表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="833ea-143">Wait for the command output to appear.</span></span>

## <a name="generate-a-client-certificate"></a><span data-ttu-id="833ea-144">クライアント証明書の生成</span><span class="sxs-lookup"><span data-stu-id="833ea-144">Generate a client certificate</span></span>

1. <span data-ttu-id="833ea-145">自己署名ルート証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-145">Create the self-signed root certificate.</span></span>

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. <span data-ttu-id="833ea-146">クライアント証明書を生成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-146">Generate a client certificate.</span></span>

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. <span data-ttu-id="833ea-147">ルート証明書の公開キーをエクスポートします。</span><span class="sxs-lookup"><span data-stu-id="833ea-147">Export the root certificate public key.</span></span> <span data-ttu-id="833ea-148">クライアント コンピューターで、次のコマンドを入力し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-148">On your client computer, type the following command and press Enter.</span></span>

    ```PowerShell
    certmgr
    ```

1. <span data-ttu-id="833ea-149">[個人]、[証明書] の順に移動します。</span><span class="sxs-lookup"><span data-stu-id="833ea-149">Navigate to Personal/Certificates.</span></span> <span data-ttu-id="833ea-150">P2SRootCert を右クリックし、**[すべてのタスク]** をクリックしてから **[エクスポート]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="833ea-150">Right-click P2SRootCert, click **All tasks**, and then select **Export**.</span></span>

1. <span data-ttu-id="833ea-151">証明書のエクスポート ウィザードで、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-151">In the Certificate Export Wizard, click **Next**.</span></span>

1. <span data-ttu-id="833ea-152">**[いいえ、秘密キーをエクスポートしません]** が選択されていることを確認し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-152">Ensure that **No, do not export the private key** is selected, and then click **Next**.</span></span>

1. <span data-ttu-id="833ea-153">**[エクスポート ファイルの形式]** ページで **[Base-64 encoded X.509 (.CER)]** が選択されていることを確認し、**[次へ]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-153">On the **Export File Format** page, ensure that **Base-64 encoded X.509 (.CER)** is selected, and then click **Next**.</span></span>

1. <span data-ttu-id="833ea-154">**[エクスポートするファイル]** ページの **[ファイル名]** で、「**C:\cert\P2SRootCert.cer**」と入力して [次へ] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-154">In the **File to Export** page, under **File name**, enter **C:\cert\P2SRootCert.cer**, and then click Next.</span></span>

1. <span data-ttu-id="833ea-155">**[証明書のエクスポート ウィザードの完了]** ページで、**[完了]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-155">On the **Completing the Certificate Export Wizard** page, click **Finish**.</span></span>

1. <span data-ttu-id="833ea-156">**[証明書のエクスポート ウィザード]** メッセージ ボックスで **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-156">On the **Certificate Export Wizard** message box, click **OK**.</span></span>

## <a name="upload-the-root-certificate-public-key-information"></a><span data-ttu-id="833ea-157">ルート証明書の公開キー情報のアップロード</span><span class="sxs-lookup"><span data-stu-id="833ea-157">Upload the root certificate public key information</span></span>

1. <span data-ttu-id="833ea-158">PowerShell ウィンドウで、次のコマンドを実行して証明書名の変数を宣言します。</span><span class="sxs-lookup"><span data-stu-id="833ea-158">In the PowerShell window, execute the following command to declare a variable for the certificate name:</span></span>

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. <span data-ttu-id="833ea-159">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="833ea-159">Execute the following command:</span></span>

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. <span data-ttu-id="833ea-160">ここで Azure に証明書をアップロードします。</span><span class="sxs-lookup"><span data-stu-id="833ea-160">Now upload the certificate to Azure.</span></span> <span data-ttu-id="833ea-161">Azure では、それが信頼されたルート証明書として認識されます。</span><span class="sxs-lookup"><span data-stu-id="833ea-161">Azure then recognizes it as a trusted root certificate.</span></span>

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## <a name="configure-the-native-vpn-client"></a><span data-ttu-id="833ea-162">ネイティブ VPN クライアントを構成する</span><span class="sxs-lookup"><span data-stu-id="833ea-162">Configure the native VPN client</span></span>

1. <span data-ttu-id="833ea-163">次のコマンドを実行して、VPN クライアント構成ファイルを .ZIP 形式で作成します。</span><span class="sxs-lookup"><span data-stu-id="833ea-163">Execute the following command to create VPN client configuration files in .ZIP format.</span></span>

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. <span data-ttu-id="833ea-164">このコマンドからの出力で返される URL をコピーして、お使いのブラウザーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="833ea-164">Copy the URL returned in the output from this command and paste it into your browser.</span></span> <span data-ttu-id="833ea-165">ブラウザーで .ZIP ファイルのダウンロードが開始されます。</span><span class="sxs-lookup"><span data-stu-id="833ea-165">Your browser should start downloading a .ZIP file.</span></span> <span data-ttu-id="833ea-166">これを解凍し、適切な場所に配置します。</span><span class="sxs-lookup"><span data-stu-id="833ea-166">Unzip it and put it in a suitable location.</span></span>

1. <span data-ttu-id="833ea-167">抽出したフォルダー内で、WindowsAmd64 フォルダー (64 ビット Windows コンピューターの場合) または WindowsZX86 フォルダー (32 ビット コンピューターの場合) に移動します。</span><span class="sxs-lookup"><span data-stu-id="833ea-167">In the extracted folder, navigate to either the WindowsAmd64 folder (for 64-bit Windows computers) or the WindowsZX86 folder (for 32-bit computers).</span></span>

1. <span data-ttu-id="833ea-168">アーキテクチャに応じて、VpnClientSetupxxxxx.exe ファイルをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-168">Double-click on the VpnClientSetupxxxxx.exe file, depending on your architecture.</span></span>

1. <span data-ttu-id="833ea-169">**[Windows によって PC が保護されました]** 画面で、**[詳細情報]**、**[実行]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-169">In the **Windows protected your PC** screen, click **More info**, and then click **Run anyway**.</span></span>

1. <span data-ttu-id="833ea-170">**[ユーザー アカウント制御]** ダイアログ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-170">In the **User Account Control** dialog box, click **Yes**.</span></span>

1. <span data-ttu-id="833ea-171">**[VNetData]** ダイアログ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-171">In the **VNetData** dialog box, click **Yes**.</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="833ea-172">Azure への接続</span><span class="sxs-lookup"><span data-stu-id="833ea-172">Connect to Azure</span></span>

1. <span data-ttu-id="833ea-173">Windows キーを押し、「**設定**」と入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-173">Press the Windows key, type **Settings** and press Enter.</span></span>

1. <span data-ttu-id="833ea-174">**[設定]** ウィンドウで、**[ネットワークとインターネット]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-174">In the **Settings** window, click **Network and Internet**.</span></span>

1. <span data-ttu-id="833ea-175">左側のウィンドウで、**[VPN]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-175">In the left-hand pane, click **VPN**.</span></span>

1. <span data-ttu-id="833ea-176">右側のウィンドウで、**[VNetData]**、**[接続]** の順にクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-176">In the right-hand pane, click **VNetData**, and then click **Connect**.</span></span>

1. <span data-ttu-id="833ea-177">[VNetData] ウィンドウで、**[接続]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-177">In the VNetData window, click **Connect**.</span></span>

1. <span data-ttu-id="833ea-178">[VNetData] ウィンドウで、**[続行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-178">In the next VNetData window, click **Continue**.</span></span>

1. <span data-ttu-id="833ea-179">**[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="833ea-179">In the **User Account Control** message box, click **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="833ea-180">これらの手順が機能しない場合は、コンピューターの再起動が必要になる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="833ea-180">If these steps do not work, you may need to restart your computer.</span></span>

## <a name="verify-your-connection"></a><span data-ttu-id="833ea-181">接続を確認する</span><span class="sxs-lookup"><span data-stu-id="833ea-181">Verify your connection</span></span>

1. <span data-ttu-id="833ea-182">Windows キーを押し、「**cmd**」と入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-182">Press the Windows key, type **cmd** and press Enter.</span></span> <span data-ttu-id="833ea-183">**[コマンド プロンプト]** ウィンドウが表示されます。</span><span class="sxs-lookup"><span data-stu-id="833ea-183">The **Command Prompt** window appears.</span></span>

1. <span data-ttu-id="833ea-184">「`IPCONFIG /ALL`」と入力して Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="833ea-184">Type `IPCONFIG /ALL` and press Enter.</span></span>

1. <span data-ttu-id="833ea-185">PPP アダプター VNetData の下の IP アドレスをコピーするか、メモしておきます。</span><span class="sxs-lookup"><span data-stu-id="833ea-185">Copy the IP address under PPP adapter VNetData, or write it down.</span></span>

1. <span data-ttu-id="833ea-186">IP アドレスが **172.16.201.0/24 の VPNClientAddressPool 範囲**内にあることを確認します。</span><span class="sxs-lookup"><span data-stu-id="833ea-186">Confirm that IP address is in the **VPNClientAddressPool range of 172.16.201.0/24**.</span></span>

1. <span data-ttu-id="833ea-187">Azure VPN ゲートウェイへの接続が正常に行われました。</span><span class="sxs-lookup"><span data-stu-id="833ea-187">You have successfully made a connection to the Azure VPN gateway.</span></span>

## <a name="summary"></a><span data-ttu-id="833ea-188">まとめ</span><span class="sxs-lookup"><span data-stu-id="833ea-188">Summary</span></span>

<span data-ttu-id="833ea-189">VPN ゲートウェイを設定したので、Azure 内の仮想ネットワークへの暗号化されたクライアント接続を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="833ea-189">You just set up a VPN gateway, allowing you to make an encrypted client connection to a virtual network in Azure.</span></span> <span data-ttu-id="833ea-190">この方法は、クライアント コンピューターと小規模なサイト間の接続に適しています。</span><span class="sxs-lookup"><span data-stu-id="833ea-190">This approach is great with client computers and smaller site-to-site connections.</span></span>