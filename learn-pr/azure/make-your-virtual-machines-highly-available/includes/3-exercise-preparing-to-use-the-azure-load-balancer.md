In this exercise, you will create a load balancer, a virtual network, and multiple virtual machines using the Azure portal.

Suppose you work for Woodgrove Bank, a startup that is about to launch online banking services. This sector is highly competitive, so you need to guarantee of a minimum of 99.99% service availability. You have determined that Azure Load Balancer with a pool of three virtual machines will meet this goal.

## Create a public load balancer

1. In a browser, navigate to the [Azure portal](https://portal.azure.com/?azure-portal=true) and sign in to your account.

1. In the sidebar, click **Create a resource**. Then, in the **New** blade, click **Networking**, and then click **Load Balancer**.

1. In the **Create load balancer** blade, enter or select the following information:
    - Name: **woodgrove-LB**
    - Type: **Public**
    - SKU: **Basic**
    - Public IP address: Select **Create new**. In the text box, type **woodgrove-LB-ip**. Leave the Assignment as **Dynamic**.
    - Resource group: Select **Create new**, and in the box, type **woodgrove-RG**.
    - Location: Select a region near you.

1. Click **Create**.

1. Wait until the load balancer has deployed before continuing with the exercise.

## Create a virtual network

1. In the left menu, click **Create a resource**. In the **New** blade, click **Networking**, and then click **Virtual network**.

1. In the **Create virtual network** blade, enter or select the following information:
    - Name: **woodgrove-VNET**
    - Address space: **172.20.0.0/16**
    - Resource group: Select **Use existing**, and then select **woodgrove-RG**.
    - Subnet: **backendSubnet**
    - Address space: **172.20.0.0/24**
    - DDoS protection: **Basic**
    - Service endpoints: **Disabled**

1. Click **Create**.

1. Wait until the virtual network has deployed before continuing with the exercise.

## Create a VM template

Start by defining the basic VM information:

1. In the Azure portal, in the left menu, click **Virtual machines**, and then click **Create virtual machine**.

1. On the **Compute** blade, in the **Recommended** section, click **Windows Server**.

1. In the **Windows Server** blade, click **Windows Server 2016 Datacenter**.

1. In the **Windows Server 2016 Datacenter** blade, click **Create**.

1. In the **Basics** blade, in the **Name** box, type **woodgrove-SVR01**.

1. In the **Username** and **Password boxes**, type a secure name and password for an administrator account on this server.

1. In the **Subscription** box, select your Azure subscription.

1. Under **Resource group**, select **Use existing**. In the list, select **woodgrove-RG**.

1. In the **Location** drop-down list, select a region near you.

1. Click **OK**.

Choose a size for the VM, and then configure the settings:

1. On the **Choose a size** blade, select a **Standard** SKU, such as **D2s_v3**. Then click **Select**.

1. On the **Settings** blade, click **Availability set**.

1. On the **Change availability set** blade, click **Create new**.

1. On the **Create new** blade, in the **Name** box, type **woodgrove-AS**, and then click **OK**.

1. On the **Settings** blade, under **Network Security Group**, click **Advanced**, and then click **(new) woodgrove-SVR01-nsg**.

1. On the **Create network Security group** blade, in the **Name** box, change the name to **woodgrove-NSG**, and then click **OK**.

1. On the **Settings** blade, click **OK**.

Save the settings to a template, so that you can easily deploy multiple VMs.

1. On the **Create** blade, click **Download template and parameters**.

1. On the **Template** blade, click **Add to library**.

1. On the **Save template** blade, in the **Name** and **Description** boxes, type **woodgrove-server-template**. Then click **Save**.

> [!NOTE]
> If you need to find this template, click **All services** in the left menu, type **template** in the filter box, and then click **Templates (PREVIEW)**.

## Use the template to provision the first VM

1. On the **Template** blade, click **Deploy**.

1. On the **Custom deployment** blade, under **Resource Group**, select **Use existing**. In the list, select **woodgrove-RG**.

1. On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.

1. On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).

1. Wait until the VM has deployed before continuing with the exercise. This is so you can be sure that the template is correctly configured before you use it to provision additional VMs, and that all the associated resources have been created.

## Use the template to provision two additional VMs

1. In the Azure portal, on the **Template** blade, click **Deploy**.

1. On the **Custom deployment** blade, under **Resource Group**, select **Use existing**. In the list, select **woodgrove-RG**.

1. On the **Custom deployment** blade, in the **Virtual Machine Name** box, change the name to **woodgrove-SVR02**.

1. On the **Custom deployment** blade, in the **Network Interface Name** box, change the name to **woodgrovesvr02222**.

1. On the **Custom deployment** blade, in the **Admin password** box, type the same password that you used previously.

1. On the **Custom deployment** blade, in the **Public Ip Address Name** box, change the name to **woodgrove-SVR02-ip**.

1. On the **Custom deployment** blade, select the **I agree to the terms and conditions** check box, and then click **Purchase** (the cost is the regular Azure compute charge, which depends on the VM pricing tier).

1. Repeat steps 1 - 7, using the following information:
    - Virtual machine name: **woodgrove-SVR03**
    - Network interface name: **woodgrovesvr03333**
    - Public IP address name: **woodgrove-SVRr03-ip**

1. Wait until the VMs have deployed before continuing with the exercise.

You now have a public load balancer ready to configure, and three VMs ready to use with this load balancer.