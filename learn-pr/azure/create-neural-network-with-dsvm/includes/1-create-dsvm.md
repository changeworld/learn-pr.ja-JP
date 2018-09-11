### Create an Ubuntu Data Science VM

The Data Science Virtual Machine for Linux is a virtual-machine image that simplifies getting started with data science. Multiple tools are already built, installed, and configured in order to get you up and running quickly. The NVIDIA GPU driver, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) and the [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn) (cuDNN) library are also included, as are [Jupyter](http://jupyter.org/), several sample Jupyter notebooks, and [TensorFlow](https://www.tensorflow.org/). All pre-installed frameworks are GPU-enabled but work on CPUs as well. In this unit, you will create an instance of the Data Science Virtual Machine (DSVM) for Linux on Azure.

1. Open the [Azure Portal](https://portal.azure.com/?azure-portal=true) in your browser.

1. Click **Create a resource** in the menu on the left side of the portal, and then type "data science" (without quotation marks) into the search box. Select **Data Science Virtual Machine for Linux (Ubuntu)** from the results list.

    ![Finding the Ubuntu Data Science VM](../media-draft/1-new-data-science-vm.png)

1. Take a moment to review the list of tools included in the VM. Then, click **Create** at the bottom of the blade.

1. Fill in the "Basics" blade as shown below. Provide a password that's at least 12 characters long containing a mix of uppercase letters, lowercase letters, numbers, and special characters. *Be sure to remember the user name and password that you enter, because you will need them later in the module.*

    ![Entering basic information about the VM](../media-draft/1-create-data-science-vm-1.png)

1. In the "Choose a size" blade, select **DS1_V2 Standard**, which provides a low-cost way to experiment with Data Science VMs. Then, click the **Select** button at the bottom of the blade.

    ![Choosing a VM size](../media-draft/1-create-data-science-vm-2.png)

1. In the **Settings** blade, check **SSH (22)** in the list of inbound ports so clients can connect to the VM using the [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) protocol on port 22. Then, click **OK**.

    ![Creating the VM](../media-draft/1-create-data-science-vm-3.png)

1. In the **Create** blade, take a moment to review the options you selected for the VM, and click **Create** to start the VM creation process.

    ![Creating the VM](../media-draft/1-create-data-science-vm-4.png)

1. Click **Resource groups** in the menu on the left side of the portal. Then, click the **data-science-rg** resource group.

    ![Opening the resource group](../media-draft/1-open-resource-group.png)

  
1. Wait until "Deploying" changes to "Succeeded", indicating that DSVM and supporting Azure resources have been created. Deployment typically takes five minutes or less. Periodically click **Refresh** at the top of the blade to refresh the deployment status.

    ![Monitoring the deployment status](../media-draft/1-deployment-succeeded.png)
