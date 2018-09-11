In this exercise, you will create a virtual network in Microsoft Azure. You will then create two virtual machines and use the virtual network to connect the virtual machines to each other and to the Internet.

Before you begin this unit, you will need to log into [Azure Cloud Shell](https://shell.azure.com) with your trial subscription credentials. We will use Azure CLI via Azure Cloud Shell to create the resource groups, virtual networks, and virtual machines.

## Create a resource group

1. In the **Welcome to Azure Cloud Shell** window, click **Bash (Linux)**.

1. In the **you have no storage mounted** window, click **Create Storage**.

1. In the Azure PowerShell command-line prompt, type the following code and press Enter. Replace the `<myResourceGroup>` value with a descriptive name to make it easy to remember when you clean up any resources created later. You'll use this name through the reset of this lab.

    ```bash
    az group create --name <myResourceGroup> --location eastus
    ```

## Create a virtual network

1. To create a virtual network, enter the following command and press Enter. Replace the `<myVirtualNetwork>` value with a descriptive name to make it easy to remember

    ```bash
    az network vnet create --name <myVirtualNetwork> --resource-group <myResourceGroup> --subnet-name default
    ```

## Create two virtual machines

1. To create the first virtual machine, execute the following command to create a Windows VM with a public IP address that is accessible over port 3389 (Remote Desktop). Name the VM `dataProcStage1`:

    ```bash
    az vm create --name dataProcStage1 --resource-group <myResourceGroup> --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. Supply values for your password at the prompts. Remeber to write this password down as you'll need it later to access the server.

1. You'll now create the second VM. This VM will not have a public IP adderss. Execute the following command to create a Windows VM **without** a public IP address using an empty string. Name the VM `dataProcStage2`:

    ```bash
    az vm create -n dataProcStage2 -g <myResourceGroup> --public-ip-address "" --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. When completed, the output from the second command will return a value for publicIpAddress.  

## Connect to dataProcStage1 using Remote Desktop

1. On your client computer, press the Windows key and type RDP.

1. Ensure that **Remote Desktop Connection** app is selected, and then press Enter.

1. In the **Remote Desktop Connection** dialog box, in the **Computer** field, enter the value of `dataProcStage1`'s PublicIPAddress, and then click **Connect**.
    
    Instructions to get remote ip if not written down

1. In the **Do you trust this remote connection?** dialog box, click **Connect**.

1. In the **Windows Security** dialog box, enter the user name and password you used when you created `dataProcStage1`. 

    Have to change profiles here

1. In the **Remote Desktop Connection** dialog box, click **OK**.

1. Sign in to the remote computer in Azure.

1. When the **Networks** message appears, click **No**.

1. Close Server Manager.

1. In the remote session, right-click the Windows key and click **Command Prompt**.

1. In the command prompt window, type the following command and press Enter.

    ```cmd
    ping dataProcStage2 -4
    ```

1. There should be no response from `dataProcStage2`. This is because by default, Windows Firewall prevents ICMP responses on `dataProcStage2`.

## Connect to dataProcStage2 using Remote Desktop

You'll configure the Windows Firewall on `dataProcStage2` using a new remote desktop seesion. However, you'll not able to access `dataProcStage2` from your desktop. Recall, `dataProcStage2` does not have a public IP address. You will using remote desktop from `dataProcStage1` to connect to `dataProcStage2`.

1. On `dataProcStage1`, press the Windows key and type **RDP**. Select the **Remote Desktop Connection** app and press Enter.

1. In the **Computer** field, enter `dataProcStage2`, and then click **Connect**. Based on the default network configuration`dataProcStage1` is able to resolve the address for `dataProcStage2` using the computer name.

1. In the **Do you trust this remote connection?** dialog box, click **Connect**.

1. In the **Windows Security** dialog box, enter the user name and password you used when you created `dataProcStage2`.

1. In the **Remote Desktop Connection** dialog box, click **OK**. You now sign in to the remote computer in Azure.

1. When the **Networks** message appears, click **No**.

1. Close Server Manager.

1. On `dataProcStage2`, press the Windows key, type **Firewall**, and press Enter. The **Windows Firewall with Advanced Security** console appears.

1. In the left-hand pane, click **Inbound Rules**.

1. In the right-hand pane, scroll down and right-click **File and Printer Sharing (Echo Request - ICMPv4-In)**, and then click **Enable Rule**.

1. Switch back to the `dataProcStage1` console and in the command prompt window, type the following command and press Enter.

    ```cmd
    ping dataProcStage2 -4
    ```

1. `dataProcStage2` responds with four replies, demonstrating connectivity between the two VMs.

## Summary

You have successfully created a virtual network, created two VMs that are attached to that virtual network, connected to one of the VMs and shown network connectivity to the other VM within the same virtual network. You can use Azure Virtual Network to connect resources within the Azure network. However, those resources need to be within the same resource group and subscription. Next, we will look at VPN gateways, which enable you to connect virtual network in different resource groups, subscriptions, and even geographical regions.
