パブリック インターネット経由で暗号化されたトンネルを使用して環境内のクライアントまたはサイトを Azure に接続できるようにする必要があります。 このユニットでは、ポイント対サイト VPN ゲートウェイを作成し、クライアント コンピューターからそのゲートウェイに接続し。 セキュリティを確保するためにネイティブの Azure 証明書認証接続を使用します。

次のプロセスを実行します。

1. RouteBased VPN ゲートウェイを作成します。

1. 認証用にルート証明書の公開キーをアップロードします。

1. ルート証明書からクライアント証明書を生成し、認証のために、仮想ネットワークに接続する各クライアント コンピューターにクライアント証明書をインストールします。

1. VPN クライアントにクライアントが仮想ネットワークに接続するために必要な情報を含む構成ファイルを作成します。

## <a name="before-you-begin"></a>開始する前に
<!---TODO: These should be prerequisites in the first unit and on the index.yml--->

このモジュールを完了するには、が必要です。

- Azure PowerShell のインストール

- という名前のフォルダー **C:\cert**

Azure PowerShell をインストールするには:

1. Windows ボタンを右クリックし、をクリックして**PowerShell (管理者)** します。

1. **[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします。

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

## <a name="configure-a-virtual-network"></a>仮想ネットワークを構成する

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
    > 警告メッセージを無視し、コマンドが完了するまで待ちます。

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
- Vpn の種類は RouteBased である必要があります。

> [!NOTE]
> この部分の演習を GatewaySku の値によっては、完了までに最大 45 分かかることに注意してください。

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

1. [個人]/[証明書] に移動します。 P2SRootCert を右クリックし、をクリックして**すべてのタスク**、し、**エクスポート**します。

1. 証明書のエクスポート ウィザードで **[次へ]** をクリックします。

1. いることを確認**秘密キーをエクスポートしません**が選択されているし、をクリックし、**次**。

1. **エクスポート ファイルの形式**いることを確認 ページで、 **Base 64 でエンコードされた X.509 (します。CER)** が選択されているし、をクリックし、**次**します。

1. **エクスポートするファイル**] ページ [**ファイル名**、入力**C:\cert\P2SRootCert.cer**、[次へ] を順にクリックします。

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

1. VPN クライアント構成ファイルを作成する次のコマンドを実行します。ZIP 形式です。

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. このコマンドの出力で返される URL をコピーして、お使いのブラウザーに貼り付けます。 お使いのブラウザーが .ZIP ファイルのダウンロードを開始します。 これを解凍し、適切な場所に配置します。

1. 抽出したフォルダー (64 ビット Windows コンピューター) の場合、WindowsAmd64 フォルダーまたは WindowsZX86 フォルダー (32 ビット コンピューター) の場合のいずれかに移動します。

1. アーキテクチャによっては、VpnClientSetupxxxxx.exe ファイルをダブルクリックします。

1. **Windows PC を保護する**画面で、**詳細**、順にクリックします**実行**します。

1. **[ユーザー アカウント制御]** ダイアログ ボックスで、**[はい]** をクリックします。

1. **[VNetData]** ダイアログ ボックスで、**[はい]** をクリックします。

## <a name="connect-to-azure"></a>Azure への接続

1. Windows キーを押し、「**設定**」と入力して Enter キーを押します。

1. **[設定]** ウィンドウで、**[ネットワークとインターネット]** をクリックします。

1. 左側のウィンドウで、**[VPN]** をクリックします。

1. 右側のウィンドウで次のようにクリックします。 **VNetData**、 をクリックし、 **Connect**します。

1. [VNetData] ウィンドウで、**[接続]** をクリックします。

1. [VNetData] ウィンドウで、**[続行]** をクリックします。

1. **[ユーザー アカウント制御]** メッセージ ボックスで、**[はい]** をクリックします。

> [!NOTE]
> 次の手順が機能しない場合は、コンピューターを再起動する必要があります。

## <a name="verify-your-connection"></a>接続を確認する

1. Windows キーを押し、「**cmd**」と入力して Enter キーを押します。 **[コマンド プロンプト]** ウィンドウが表示されます。

1. 「`IPCONFIG /ALL`」と入力して Enter キーを押します。

1. PPP アダプター VNetData の下の IP アドレスをコピーするか、メモしておきます。

1. IP アドレスが **172.16.201.0/24 の VPNClientAddressPool 範囲**内にあることを確認します。

1. Azure VPN ゲートウェイへの接続が正常に行われました。

## <a name="summary"></a>まとめ

設定した VPN ゲートウェイでは、Azure で仮想ネットワークに暗号化されたクライアント接続を作成することができます。 この方法は、クライアント コンピューターと小規模なサイト間接続に優れています。