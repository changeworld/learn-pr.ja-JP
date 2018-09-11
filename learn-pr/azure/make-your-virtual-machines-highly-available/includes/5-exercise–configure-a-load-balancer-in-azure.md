You have already created the key resources for your online bank architecture. In this exercise, you will complete the setup by configuring security settings, a back-end address pool, health probe rules, and load-balancing rules. Once setup is complete, you will test the load balancer by removing a VM from the pool and verifying that client requests are no longer sent to this VM.

## Inbound rules

Create an inbound security rule to allow for inbound HTTP connections that use port 80.

1. In the Azure portal, in the left menu, click **All resources**. Then, in the resource list, click **woodgrove-NSG**.

1. On the **woodgrove-NSG** blade, under **Settings**, click **Inbound security rules**, and then click **Add**.

1. On the **Add inbound security rule** blade, enter the following information:
    - Source: **Service Tag**
    - Source service tag: **Internet**
    - Source port ranges: **\***
    - Destination: **Any**
    - Destination port ranges: **80**
    - Protocol: **TCP**
    - Action: **Allow**
    - Priority: **100**
    - Name: **woodgrove-HTTP-rule**

1. Click **Add**.

Create an inbound security rule to allow for inbound RDP connections that use port 3389.

1. On the **woodgrove-NSG - Inbound security rules** blade, click **Add**.

1. On the **Add inbound security rule** blade, enter the following information:
    - Source: **Service Tag**
    - Source service tag: **Internet**
    - Source port ranges: **\***
    - Destination: **Any**
    - Destination port ranges: **3389**
    - Protocol: **TCP**
    - Action: **Allow**
    - Priority: **200**
    - Name: **woodgrove-RDP-rule**

1. Click **Add**.

## Prepare a script to install and configure IIS on the VMs

1. Copy the following PowerShell commands into a text editor:

    ```powershell
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools

    # remove default htm file
    remove-item  C:\inetpub\wwwroot\iisstart.htm

    # Add a new htm file that displays server name
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Web page from <b>" + $env:computername + "</b>")
    ```

1. Save the file locally as **ConfigureIIS.ps1**.

### Install and configure IIS on VMs

1. In the Azure portal, in the left menu, click **All resources**. Then, in the resource list, click **woodgrove-SVR01**.

1. On the Overview page, click **Connect**.

1. In the **Connect to virtual machine** blade, click **Download RDP File**. Then, in the pop-up, click **Open**.

1. In the **Remote Desktop Connection** window, click **Connect**.

1. In the **Windows Security** dialog box, click **More choices**, and then click **Use a different account**.

1. In the **Windows Security** dialog box, enter the credentials that you used when you provisioned the VMs, and then click **OK**.

1. If you get a **Remote Desktop Connection** certificate warning, click **Yes**.

1. On your local computer, open the folder containing your script.

1. Copy **ConfigureIIS.ps1** to the clipboard.

1. Switch to the RDP session. Then, the server, open Windows Explorer, and paste **ConfigureIIS.ps1** to **C:\\**.

1. In the RDP session, click **Start**, and then open **Windows PowerShell**.

1. At the PowerShell prompt, type the following command, and then press **Enter:**

    ```powershell
    cd \
    ```

1. At the PowerShell prompt, type the following command, and then press **Enter:**

    ```powershell
    .\ConfigureIIS.ps1
    ```

1. Close the RDP connection.

1. Repeat steps 1-13 for **woodgrove-SVR02** and **woodgrove-SVR03**.

## Create a back-end address pool

Next, use the portal to create a back-end address pool for the public load balancer.

1. In the Azure portal, in the left menu, click **All resources**. Then, in the resource list, click **woodgrove-LB**.

1. On the **woodgrove-LB** blade, under **Settings**, click **Backend pools**, and then click **Add**.

1. On the **Add backend pool** blade, add the following information:
    - Name: **woodgrove-BEP**
    - Associated to: select **Availability set**.
    - Availability set: **woodgrove-AS**.

1. Click **Add a target network IP configuration**. Then, in the **Target virtual machine** list, select **woodgrove-SVR01**. In the **Network IP configuration** list, select the VM's IP address pool.

1. Click **Add a target network IP configuration**. Then, in the **Target virtual machine** list, select **woodgrove-SVR02**, and in the **Network IP configuration** list, select the VM's IP address pool.

1. Click **Add a target network IP configuration**. Then, in the **Target virtual machine** list, select **woodgrove-SVR03**, and in the **Network IP configuration** list, select the VM's IP address pool.

1. On the **Add backend pool** blade, click **OK**.

1. Wait until the load balancer configuration has updated before proceeding with the exercise.

## Create a health probe for the load balancer

Next, add a health probe for HTTP over port 80.

1. On the **woodgrovelb** blade, under **Settings**, click **Health probes**, and then click **Add**.

1. On the **Add health probe** blade, add the following information:
    - Name: **woodgrove-HP**
    - Protocol: **HTTP**
    - Port: **80**
    - Path: **/**
    - Interval: **15**.
    - Unhealthy threshold: **2**

1. On the **Add health probe** blade, click **OK**.

1. Wait until the load balancer configuration has updated before proceeding with the exercise.

## Create a load balancer rule

Finally, create a load balancing rule for the HTTP over port 80, that associates the back-end pool with the health probe.

1. On the **woodgrove-LB** blade, under **Settings**, click **Load balancing rules**, and then click **Add**.

1. On the **Add load balancing rule** blade, in the **Name** box, type **woodgrove-HTTP-LBRule**. Then verify that the following information has been automatically entered:
    - IP Version: **IPv4**
    - Frontend IP address: **LoadBalancerFrontEnd**
    - Protocol: **TCP**
    - Port: **80**
    - Backend port: **80**
    - Backend pool: **woodgrove-BEP**
    - Health probe: **woodgrove-HP**
    - Session persistence: **None**
    - Idle timeout: **4 minutes**.
    - Floating IP: **Disabled**

1. On the **Add load balancing rule** blade, click **OK**.

1. Wait until the load balancer configuration has updated before proceeding with the exercise.

## Test the load balancer

1. In the Azure portal, in the left menu, click **All resources**, and then in the resource list, click **woodgrove-LB-ip**.

1. On the **Overview** blade, select the **IP address**, and then click the **Copy** button.

1. In a new browser tab, paste the IP address into the address bar of your browser. Note the name of the server and keep this tab open.

1. In the Azure portal, in the left menu, click **All resources**. Then, in the resource list, click the server you noted above.

1. On the **Overview** blade, click **Stop**, and then click **Yes**.

1. Wait until the VM has stopped, and then switch to the tab you viewed in step 3. Refresh the page.

1. The load balancer will now send your HTTP request to one of your other VMs.

In this exercise, you completed the deployment of your backend VMs and the load balancer.