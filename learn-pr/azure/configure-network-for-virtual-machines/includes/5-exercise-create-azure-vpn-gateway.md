パブリック インターネット経由で暗号化されたトンネルを使用して環境内のクライアントまたはサイトを Azure に接続できるようにする必要があります。 この演習では、ポイント対サイト VPN ゲートウェイを作成し、クライアント コンピューターからそのゲートウェイに接続します。 セキュリティを確保するためにネイティブの Azure 証明書認証接続を使用します。

次のプロセスを実行します。

1. RouteBased VPN ゲートウェイを作成する

1. 認証の目的でルート証明書の公開キーをアップロードする

1. ルート証明書からクライアント証明書を生成し、認証の目的で、VNet に接続する各クライアント コンピューターにクライアント証明書をインストールする

1. クライアントが VNet に接続するために必要な情報を含む VPN クライアント構成ファイルを作成する。

## <a name="before-you-begin"></a>開始する前に

このラボを完了するには、以下が必要です。

- Azure PowerShell

- C:\cert という名前のフォルダー

Azure PowerShell をインストールするには:

1. Windows ボタンを右クリックし、**[PowerShell (管理者)]** をクリックします

1. **[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします

1. PowerShell ウィンドウで、次のコマンドを入力し、Enter キーを押します。

    ```PowerShell
    Import-Module AzureRM
    ```

1. セキュリティのプロンプトで、「A」と入力し、Enter キーを押します。

## <a name="sign-in-and-set-variables"></a>サインインと変数の設定

サインインし、変数を設定するには、次の手順を実行します。

1. 次のコマンドを入力して Enter キーを押すことで、Azure に接続します。

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. Azure サブスクリプションの一覧を取得します。

    ```PowerShell
    Get-AzureRmSubscription
    ```
1. 使用するサブスクリプションを指定します。

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    "Name of subscription" をサブスクリプション名に置き換えます。

1. 次の変数を入力し、それぞれの入力後に Enter キーを押します。

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

## <a name="configure-a-vnet"></a>VNet の構成

1. リソース グループを作成します。

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. 仮想ネットワークのサブネット構成を作成します。 これらの名前は **FrontEnd、BackEnd**、**GatewaySubnet** です。 これらすべてのサブネットが仮想ネットワークのプレフィックス内に存在します。

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. サブネットの値と静的 DNS サーバーを使用して仮想ネットワークを作成します。 
    > [!IMPORTANT]
    > 警告メッセージを無視し、コマンドが完了するまで待機します。

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. ここで、先ほど作成したこのネットワークでの変数を指定します。

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. 動的に割り当てられたパブリック IP アドレスを要求します。

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## <a name="create-the-vpn-gateway"></a>VPN ゲートウェイを作成する

この VPN ゲートウェイを作成する場合:

- GatewayType は Vpn である必要があります
- VpnTpe は RouteBased である必要があります

> [!NOTE]
> 演習のこの部分は、ゲートウェイ sku の値によっては完了まで最大 45 分かかることにご注意ください

1. VPN ゲートウェイを作成するには、次のコマンドを実行し、Enter キーを押します。

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. コマンド出力が表示されるまで待ちます。

## <a name="add-the-vpn-client-address-pool"></a>VPN クライアント アドレス プールの追加

1. 次のコマンドを実行し、Enter キーを押します。

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. コマンド出力が表示されるまで待ちます。

## <a name="generate-a-client-certificate"></a>クライアント証明書の生成

1. 自己署名ルート証明書を作成します。

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. クライアント証明書を生成します。

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. ルート証明書の公開キーをエクスポートします。 クライアント コンピューターで、次のコマンドを入力し、Enter キーを押します。

    ```PowerShell
    certmgr
    ```

1. [個人]/[証明書] に移動します。 P2SRootCert を右クリックし、**[すべてのタスク]** をクリックしてから **[エクスポート]** を選択します。

1. 証明書のエクスポート ウィザードで **[次へ]** をクリックします。

1. **[いいえ、秘密キーをエクスポートしません]** が選択されていることを確認し、**[次へ]** をクリックします。

1. **[エクスポート ファイルの形式]** ページで **[Base-64 encoded X.509 (.CER)]** が選択されていることを確認し、**[次へ]** をクリックします。

1. **[エクスポートするファイル]** ページの **[ファイル名]** で、「**C:\cert\P2SRootCert.cer**」と入力して [次へ] をクリックします。

1. **[証明書のエクスポート ウィザードを完了しています]** ページで、**[完了]** をクリックします。

1. **[証明書のエクスポート ウィザード]** メッセージ ボックスで **[OK]** をクリックします。

## <a name="upload-the-root-certificate-public-key-information"></a>ルート証明書の公開キー情報のアップロード

1. PowerShell ウィンドウで、次のコマンドを実行して証明書名の変数を宣言します。

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. 次のコマンドを実行します。

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. ここで Azure に証明書をアップロードします。 Azure では、それが信頼されたルート証明書として認識されます。

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## <a name="configure-the-native-vpn-client"></a>ネイティブ VPN クライアントの構成

1. 次のコマンドを実行して、VPN クライアント構成ファイルを .ZIP 形式で作成します。

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. このコマンドからの出力で返される URL をコピーして、お使いのブラウザーに貼り付けます。 お使いのブラウザーが .ZIP ファイルのダウンロードを開始します。 これを解凍し、適切な場所に配置します。

1. 抽出したフォルダー内で、WindowsAmd64 フォルダー (64 ビット Windows コンピューターの場合) または WindowsZX86 (32 ビット コンピューターの場合) に移動します。

1. アーキテクチャによっては、VpnClientSetupxxxxx.exe ファイルをダブルクリックします。

1. **[Windows によって PC が保護されました]** 画面で、**[詳細情報]** をクリックし、**[実行]** をクリックします。

1. **[ユーザー アカウント制御]** ダイアログ ボックスで、**[はい]** をクリックします。

1. **[VNetData]** ダイアログ ボックスで、**[はい]** をクリックします。

## <a name="connect-to-azure"></a>Azure への接続

1. Windows キーを押し、「**設定**」と入力して Enter キーを押します。

1. **[設定]** ウィンドウで、**[ネットワークとインターネット]** をクリックします。

1. 左側のウィンドウで、**[VPN]** をクリックします。

1. 右側のウィンドウで、**[VNetData]** をクリックし、**[接続]** をクリックします。

1. [VNetData] ウィンドウで、**[接続]** をクリックします。

1. [VNetData] ウィンドウで、**[続行]** をクリックします。

1. **[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします。

> [!NOTE]
> これらの手順が機能しない場合は、コンピューターの再起動が必要な可能性があります

## <a name="verify-your-connection"></a>接続を確認する

1. Windows キーを押し、「**cmd**」と入力して Enter キーを押します。 **[コマンド プロンプト]** ウィンドウが表示されます。

1. 「`IPCONFIG /ALL`」と入力して Enter キーを押します。

1. PPP アダプター VNetData の下の IP アドレスをコピーするか、メモしておきます。

1. IP アドレスが **172.16.201.0/24 の VPNClientAddressPool 範囲**内にあることを確認します。

1. Azure VPN ゲートウェイへの接続が正常に行われました。

## <a name="summary"></a>まとめ

VPN ゲートウェイを設定したので、Azure 内の vNet に暗号化されたクライアント接続を行うことができます。 この方法は、クライアント コンピューターと小規模なサイト間接続に優れています。