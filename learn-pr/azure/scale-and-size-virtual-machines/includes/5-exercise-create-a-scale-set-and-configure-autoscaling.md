In this exercise, you will use the Azure portal to create a virtual machine scale set with rules for autoscaling.

## Create a virtual machine scale set

1. In the Azure portal, click **Create a resource**.

1. In the search box, type **scale set** and press ENTER. In the **Results** blade, click **Virtual machine scale set**, and in the **Virtual machine scale set** blade, click **Create**.

1. In the **Create virtual machine scale set** blade, enter the following values (replacing `<your initials>`, `<date>`, and `<time>` with relevant data), and then click **Create**.

    |Setting|Value|
    |---|---|
    |Virtual machine scale set name|WebServerSS|
    |Resource group|Use existing - ExerciseRG|
    |Username|LocalAdmin|
    |Password and Confirm password|Adm1nPa$$word|
    |Instance count|2|
    |Instance size|D2s_v3|
    |Choose load balancing options|Load balancer|
    |Public IP address name|WebServerPubIP|
    |Domain name label|`<your initials><date><time>` (for example ja0904181202)|

1. Wait for the scale set to be created.

1. Navigate to the **ExerciseRG** resource group. In the **ExerciseRG** blade, click the **WebServerSS** object, and in the **WebServerSS** blade, click **Instances**. Note the two virtual machine instances running within the scale set.

## Create and apply autoscale rules

1. In the **WebServerSS** blade, click **Scaling**. On the **WebServerSS - Scaling** blade, click **Enable autoscale**.

1. On the **WebServerSS - Scaling** blade, click **Add a rule**.

1. In the Scale rule blade, enter the following information to create a rule to scale out an extra two virtual machines when average CPU usage is more than 75% over a five-minute period, and then click **Add**.

    |Setting|Value|
    |---|---|
    |Time aggregation|Average|
    |Metric name|Percentage CPU|
    |Time grain statistic|Average|
    |Operator|Greater than|
    |Threshold|75|
    |Duration (in minutes)|5|
    |Operation|Increase count by|
    |Instance count|2|
    |Cool down (minutes)|5|

1. On the **WebServerSS - Scaling** blade, click **Add a rule**.

1. In the Scale rule blade, enter the following information to create a rule to scale in one server at a time when average CPU usage is below 30% over a five-minute period, and then click **Add**.

    |Setting|Value|
    |---|---|
    |Time aggregation|Average|
    |Metric name|Percentage CPU|
    |Time grain statistic|Average|
    |Operator|Less than|
    |Threshold|30|
    |Duration (in minutes)|5|
    |Operation|Decrease count by|
    |Instance count|1|
    |Cool down (minutes)|5|

1. On the **WebServerSS - Scaling** blade, in the **Autoscale setting name** box, type **WebAutoscaleSetting**. Next to **Instance limits**, set the following values and then click **Save**.

    |Setting|Value|
    |---|---|
    |Minimum|2|
    |Maximum|8|
    |Default|2|

## Generate load to demonstrate autoscaling

Now you will use the CPUStress.exe tool to generate load on the virtual machines in the scale set and demonstrate automatic scaling out.

1. To determine the public IP address to connect to, browse to the **ExerciseRG** resource group. In the **ExerciseRG** blade, click the **WebServerSSlb** object.

1. On the **WebServerSSlb** blade, click Frontend IP configuration and make a note of the public IP address shown. Close the **LoadBalancerFrontEnd** blade.

1. To determine the port number to connect to, on the **WebServerSSlb** blade, click **Inbound NAT rules**. Note down the custom port number in the Service column for each virtual machine.

1. On your local computer, start the **Remote Desktop Connection**. In the **Computer** box, type the public IP address of the load balancer, followed by a colon and then the port number value for Instance 0, (for example, 40.67.191.221:50000) and then click Connect.

1. In the **Windows Security** dialog box, click **More choices**, click **Use a different account**, in the **User name** box type **LocalAdmin**, in the **Password** box type **Adm1nPa$$word**, and then click **OK**.

1. In the **Remote Desktop Connection** dialog box, click **Yes**.

1. In the virtual machine remote desktop, open Internet Explorer, and in the **Set up Internet Explorer** dialog box, click **OK**.

1. In **Internet Explorer**, in the address bar, type **http://download.sysinternals.com/files/CPUSTRES.zip** and press ENTER. In the **Internet Explorer** dialog box, click **Add**. In the **Trusted Sites** dialog box, click **Add** and then click **Close**.

1. If the download prompt does not appear automatically, you may need to enter the URL again and press ENTER. In the **Internet Explorer** dialog box, click **Save**. Once the download is complete, click **Open folder**.

1. In the **Downloads** folder, double-click **CPUSTRES**, and then double-click the **CPUSTRES** application. In the **Compressed (zipped) folders** dialog box, click **Run**.

1. In the **CPU Stress** window, under **Thread 1**, in the **Activity** drop-down, select **Maximum**. Under **Thread 2**, click the **Active** checkbox, and in the **Activity** drop-down, select **Maximum**.

1. Click the Start button, then click **Task Manager**. In Task Manager, click **More details**, and confirm that the CPU is over 75%.

1. Repeat the installation and launch of **CPUSTRES** on the other virtual machine in the scale set and then wait for five minutes.

1. In the Azure portal, browse to the **ExerciseRG** resource group, and in the **ExerciseRG** blade, click the **WebServerSS** object, and in the **WebServerSS** blade, click **Instances**. Note how many virtual machine instances are displayed within the scale set and the status.

Here, you created a virtual machine scale set with two virtual machines. You then created rules to automatically scale out and scale in the scale set and added the rules to an autoscale profile.