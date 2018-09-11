You want to ensure that you can connect clients or sites within your environment into Azure using encrypted tunnels across the public Internet. In this unit, you'll create a point-to-site VPN gateway, and then connect to that gateway from a client computer. You'll use native Azure certificate authentication connections for security.

You will carry out the following process:

1. Create a RouteBased VPN gateway.

1. Upload the public key for a root certificate for authentication purposes.

1. Generate a client certificate from the root certificate, and then install the client certificate on each client computer that will connect to the virtual network for authentication purposes.

1. Create VPN client configuration files, which contain the necessary information for the client to connect to the virtual network.

## Before you begin
<!---TODO: These should be prerequisites in the first unit and on the index.yml--->

To complete this module, you must have:

- Azure PowerShell installed

- A folder named **C:\cert**

To install Azure PowerShell:

1. Right-click the Windows button and click **PowerShell (Admin)**.

1. In the **User Account Control** message box, click **Yes**.

1. In the PowerShell window, type the following command and press Enter:

    ```PowerShell
    Import-Module AzureRM
    ```

1. At the security prompt, type A and press Enter.

## Sign in and set variables

To sign in and set variables, carry out the following steps:

1. Connect to Azure by entering the following command and pressing Enter:

    ```PowerShell
    Connect-AzureRmAccount
    ```

1. Get a list of your Azure subscriptions.

    ```PowerShell
    Get-AzureRmSubscription
    ```

1. Specify the subscription that you want to use.

    ```PowerShell
    Select-AzureRmSubscription -SubscriptionName "{Name of your subscription}"
    ```

    Replace "Name of subscription" with your subscription name.

1. Enter the following variables and press Enter after each one.

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

## Configure a virtual network

1. Create a resource group.

    ```PowerShell
    New-AzureRmResourceGroup -Name $RG -Location $Location
    ```

1. Create subnet configurations for the virtual network. These have the name **FrontEnd, BackEnd**, and **GatewaySubnet**. All of these subnets exist within the virtual network prefix.

    ```PowerShell
    $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
    $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
    $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix
    ```

1. Create the virtual network using the subnet values and a static DNS server.

    > [!IMPORTANT]
    > Ignore the warning message, and then wait for the command to finish.

    ```PowerShell
    New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer 10.2.1.3
    ```

1. Now specify the variables for this network that you have just created.

    ```PowerShell
    $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
    ```

1. Request a dynamically assigned public IP address.

    ```PowerShell
    $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
    $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
    ```

## Create the VPN gateway

When creating this VPN gateway:

- GatewayType must be Vpn
- VpnType must be RouteBased

> [!NOTE]
> Note that this part of the exercise can take up to 45 minutes to complete, depending on the value of GatewaySku.

1. To create the VPN gateway, run the following command and press Enter.

    ```PowerShell
    New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
    -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
    -VpnType RouteBased -EnableBgp $false -GatewaySku VpnGw1 -VpnClientProtocol "IKEv2"
    ```

1. Wait for the command output to appear.

## Add the VPN client address pool

1. Run the following command and press Enter.

    ```PowerShell
    $Gateway = Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG -Name $GWName
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $Gateway -VpnClientAddressPool $VPNClientAddressPool
    ```

1. Wait for the command output to appear.

## Generate a client certificate

1. Create the self-signed root certificate.

    ```PowerShell
    $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
    -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
    ```

1. Generate a client certificate.

    ```PowerShell
    New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
    -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 -KeyLength 2048 `
    -CertStoreLocation "Cert:\CurrentUser\My" `
    -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
    ```

1. Export the root certificate public key. On your client computer, type the following command and press Enter.

    ```PowerShell
    certmgr
    ```

1. Navigate to Personal/Certificates. Right-click P2SRootCert, click **All tasks**, and then select **Export**.

1. In the Certificate Export Wizard, click **Next**.

1. Ensure that **No, do not export the private key** is selected, and then click **Next**.

1. On the **Export File Format** page, ensure that **Base-64 encoded X.509 (.CER)** is selected, and then click **Next**.

1. In the **File to Export** page, under **File name**, enter **C:\cert\P2SRootCert.cer**, and then click Next.

1. On the **Completing the Certificate Export Wizard** page, click **Finish**.

1. On the **Certificate Export Wizard** message box, click **OK**.

## Upload the root certificate public key information

1. In the PowerShell window, execute the following command to declare a variable for the certificate name:

    ```PowerShell
    $P2SRootCertName = "P2SRootCert.cer"
    ```

1. Execute the following command:

    ```PowerShell
    $filePathForCert = "C:\cert\P2SRootCert.cer"
    $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
    $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
    $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64
    ```

1. Now upload the certificate to Azure. Azure then recognizes it as a trusted root certificate.

    ```PowerShell
    Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName -VirtualNetworkGatewayname "VNetDataGW" -ResourceGroupName "TestRG" -PublicCertData $CertBase64
    ```

## Configure the native VPN client

1. Execute the following command to create VPN client configuration files in .ZIP format.

    ```PowerShell
    $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNetDataGW" -AuthenticationMethod "EapTls"
    $profile.VPNProfileSASUrl
    ```

1. Copy the URL returned in the output from this command and paste it into your browser. Your browser should start downloading a .ZIP file. Unzip it and put it in a suitable location.

1. In the extracted folder, navigate to either the WindowsAmd64 folder (for 64-bit Windows computers) or the WindowsZX86 folder (for 32-bit computers).

1. Double-click on the VpnClientSetupxxxxx.exe file, depending on your architecture.

1. In the **Windows protected your PC** screen, click **More info**, and then click **Run anyway**.

1. In the **User Account Control** dialog box, click **Yes**.

1. In the **VNetData** dialog box, click **Yes**.

## Connect to Azure

1. Press the Windows key, type **Settings** and press Enter.

1. In the **Settings** window, click **Network and Internet**.

1. In the left-hand pane, click **VPN**.

1. In the right-hand pane, click **VNetData**, and then click **Connect**.

1. In the VNetData window, click **Connect**.

1. In the next VNetData window, click **Continue**.

1. In the **User Account Control** message box, click **Yes**.

> [!NOTE]
> If these steps do not work, you may need to restart your computer.

## Verify your connection

1. Press the Windows key, type **cmd** and press Enter. The **Command Prompt** window appears.

1. Type `IPCONFIG /ALL` and press Enter.

1. Copy the IP address under PPP adapter VNetData, or write it down.

1. Confirm that IP address is in the **VPNClientAddressPool range of 172.16.201.0/24**.

1. You have successfully made a connection to the Azure VPN gateway.

## Summary

You just set up a VPN gateway, allowing you to make an encrypted client connection to a virtual network in Azure. This approach is great with client computers and smaller site-to-site connections.