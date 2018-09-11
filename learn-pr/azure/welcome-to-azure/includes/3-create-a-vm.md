As a technology professional, you likely have expertise in a specific area. Perhaps you're a storage admin or virtualization expert, or maybe you focus on the latest security practices. If you're a student, you may still be exploring what interests you most.

::: zone pivot="windows-cloud"

No matter your role, most people get started with the cloud by creating a virtual machine. Here you'll bring up a virtual machine running Windows Server 2016.

::: zone-end

::: zone pivot="linux-cloud"

No matter your role, most people get started with the cloud by creating a virtual machine. Here you'll bring up a virtual machine running Ubuntu 16.04.

::: zone-end

There are many ways to create a virtual machine on Azure. Here, you'll bring up a Windows or Linux VM using an interactive terminal called Cloud Shell. If you work from the terminal on a daily basis, you know this is often the fastest way to get the job done.

::: zone pivot="windows-cloud"

> [!TIP]
> Prefer Linux or want to try something new? Select **Linux** from the top of this page to run a Linux VM.

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Prefer Windows or want to try something new? Select **Windows** from the top of this page to run a Windows Server VM.

::: zone-end

Let's review some basic terms and get your first virtual machine up and running.

## What is a virtual machine?

A virtual machine, or VM, is a software emulation of a physical computer. Because VMs exist as software, dozens, hundreds, or even thousands of Azure VMs can be generated in minutes, then deleted when you don't need them anymore. With low-cost, per-minute billing, you pay only for the compute resources you use, for as long as you are using them. Plus, there are many ways to configure the VMs to fit your needs.

::: zone pivot="windows-cloud"

A snapshot of a running VM is called an _image_. Azure provides images for Windows and several flavors of Linux. You can also create your own preconfigured images to make deployments go faster. Here you'll bring up a Windows Server 2016 VM, provided by Microsoft.
::: zone-end

::: zone pivot="linux-cloud"

A snapshot of a running VM is called an _image_. Azure provides images for Windows and several flavors of Linux. You can also create your own preconfigured images to make deployments go faster. Here you'll bring up an Ubuntu 16.04 VM, provided by Canonical.
::: zone-end

## What defines a virtual machine on Azure?

A virtual machine is defined by a number of factors, including its size and location. Before you bring up your VM, let's briefly cover what's involved.

:::row:::
    :::column:::
        **Size**
    :::column-end:::
    :::column span="3":::
A VM's _size_ defines its processor speed, amount of memory, initial amount of storage, and expected network bandwidth. Some sizes even include specialized hardware such as GPUs for heavy graphics rendering and video editing.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Region**
    :::column-end:::
    :::column span="3":::
Azure is made up of data centers distributed throughout the world. A _region_ is a set of Azure data centers in a named geographic location. Every Azure resource, including virtual machines, is assigned a region. East US and North Europe are examples of regions.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Network**
    :::column-end:::
    :::column span="3":::
A _virtual network_ is a logically isolated network on Azure. Each virtual machine on Azure is associated with a virtual network. Azure provides cloud-level firewalls for your virtual networks called _network security groups_.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Resource groups**
    :::column-end:::
    :::column span="3":::
Virtual machines and other cloud resources are grouped into logical containers called _resource groups_. Groups are typically used to organize sets of resources that are deployed together as part of an application or service. You refer to a resource group by its name.
    :::column-end:::
:::row-end:::

## What is Azure Cloud Shell?

Azure Cloud Shell is a browser-based command-line experience for managing and developing Azure resources. Think of Cloud Shell as an interactive console that you run in the cloud.

Cloud Shell provides two experiences to choose from: Bash and PowerShell. Both include access to the Azure CLI, the command-line interface for Azure.

You can use any Azure management interface, including the Azure portal, Azure CLI, and Azure PowerShell, to manage any kind of VM. For learning purposes, here you'll use PowerShell if you're creating a Windows VM, or the Azure CLI if you're creating a Linux VM.

::: zone pivot="windows-cloud"

## Create a Windows VM

Let's get your Windows VM up and running. First, we create security credentials so you can later log in to your VM.

1. From Cloud Shell on the right side of this page, run these commands to generate a credential object. Replace "Password" with a password you'll remember later.

    > [!NOTE]
    > Choose a password that contains at least 8 characters with a combination of upper and lowercase letters, numbers, and symbols. Don't use a password you use elsewhere.

    ```powershell
    $pass = ConvertTo-SecureString "Password" -AsPlainText -Force
    ```
    ```powershell
    $cred = New-Object System.Management.Automation.PSCredential ("azureuser", $pass)
    ```
    **azureuser** specifies the user name. You can change it if you'd like.
 
1. Run the `New-AzureRmVm` cmdlet to create your VM.

    ```powershell
    New-AzureRmVm `
      -Image "Win2016Datacenter" `
      -ResourceGroupName "myResourceGroup" `
      -Name "myVM" `
      -Size "Standard_DS2_v2" `
      -Location "East US" `
      -VirtualNetworkName "myVnet" `
      -SubnetName "mySubnet" `
      -SecurityGroupName "myNetworkSecurityGroup" `
      -PublicIpAddressName "myPublicIpAddress" `
      -OpenPorts 80 `
      -Credential $cred `
      -Verbose
    ```

    > [!TIP]
    > This is a long command. You can use the **Copy** button to copy it. To paste it, right click on the new line in the Cloud Shell window and select **Paste**.

    Your VM will take about five minutes to come up. Compare that to the time it takes to purchase, rack, and configure a system in your data center. Quite a difference!

While you're waiting, let's review the command you just ran.

* **Win2016Datacenter** specifies the Windows Server 2016 VM image.
* The resource group, or the VM's logical container, is named **myResourceGroup**.
* The VM is named **myVM**. This name identifies the VM in Azure. It also becomes the VM's internal hostname, or computer name.
* **Standard_DS2_v2** refers to the size of the VM. This size has two virtual CPUs and 7 GB of memory.
* The VM exists in the **East US** location, or region.
* The command also assigns a public IP address to the VM. You can configure a VM to be accessible from the Internet or only from the internal network.
* The network firewall allows inbound traffic on port 80 to allow HTTP traffic to your web server.
* The credential object specifies your username and password.
* `-Verbose` is an optional parameter you can provide to get detailed information about the operation, similar to a trace or transaction log. You can use this parameter to learn what's happening during the operation or to troubleshoot failures.

You can also check out this short video about some of the options you have to create and manage VMs.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

When the process completes, you see information in Cloud Shell about your new VM. Here's an example.

```console
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
VmId                     : 6684cc9a-ef9f-47dd-92ed-ce1dbcd98396
Name                     : myVM
Type                     : Microsoft.Compute/virtualMachines
Location                 : eastus
Tags                     : {}
HardwareProfile          : {VmSize}
NetworkProfile           : {NetworkInterfaces}
OSProfile                : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState        : Succeeded
StorageProfile           : {ImageReference, OsDisk, DataDisks}
FullyQualifiedDomainName : myvm-edce6d.East US.cloudapp.azure.com
```

::: zone-end

::: zone pivot="linux-cloud"

## Create a Linux VM

Here, you'll bring up an Ubuntu VM using Cloud Shell.

1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create --location eastus --name myResourceGroup
    ```

1. Run the `az vm create` command to create your VM.

    ```azurecli
    az vm create -n myVM -g myResourceGroup --image UbuntuLTS --size Standard_DS2_v2 --generate-ssh-keys
    ```

Your VM will take about two minutes to come up. Compare that to the time it takes to purchase, rack, and configure a system in your data center. Quite a difference!

While you're waiting, let's review the command you just ran.

* **UbuntuLTS** specifies the Ubuntu 16.04 LTS VM image.
* The resource group, or the VM's logical container, is named **myResourceGroup**.
* The VM is named **myVM**. This name identifies the VM in Azure. It also becomes the VM's internal hostname, or computer name.
* **Standard_DS2_v2** refers to the size of the VM. This size has two virtual CPUs and 7 GB of memory.
* The VM exists in the **East US** location, or region.
* The command also assigns a public IP address to the VM. You can configure a VM to be accessible from the Internet or only from the internal network.
* The `--generate-ssh-keys` option creates an SSH key pair to enable you to log in to the VM.

You can also check out this short video about some of the options you have to create and manage VMs.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

When the VM is ready, you see information about it. Here's an example.

```console
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

::: zone-end

## Summary

With just a few concepts under your belt, you're able to spin up a VM on Azure in just a few minutes. Many of these concepts, such as a VM's size and firewall rules, are likely familiar to you already.

Next, you'll connect to your VM, install a web server, and configure your web server to serve up a basic web site.