The MEAN stack of components requires a server. It could be a Linux machine or virtual machine running in your own server room, or it can be configured on a cloud-based virtual machine. In this module, we will set up the stack to run on an Ubuntu Linux virtual machine running on Azure.

In this unit, you will be creating a new Ubuntu Linux virtual machine hosted on Azure. You could also install your MEAN stack components on an existing virtual machine or physical host machine. By creating a new one with this exercise, you can tie together all the components into an Azure resource group for easier management and clean-up after you complete the exercises.

We will use the Cloud Shell command line that's integrated into the Azure portal to create the Linux VM.

## Provision an Ubuntu Linux VM

1. Go to the [Azure portal](https://portal.azure.com?azure-portal=true).

1. Open Cloud Shell from the angle bracket (>_) icon in the Azure portal toolbar.

<!---TODO: Update for sandbox--->
1. In Cloud Shell, execute the command to create an Azure resource group, which will include our VM. Substitute your own resource group name for `<resource-group-name>` and your desired Azure location for `<resource-group-location>` (`westus`, for example).


    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    Remember your resource group name, as we will use it in other commands.

1. In Cloud Shell, run the following command to create a new Ubuntu Linux VM. Substitute your own resource group name for `<resource-group-name>` and your preferred admin username and password for `<vm-admin-username>` and `<vm-admin-password>`.

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    Take note of your admin username and password to allow you to connect to this VM later.

    This command takes about two minutes to complete. When the command finishes, the resulting output will look similar to this.

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<location you chose for the resource group>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<name you chose for thr resource group>",
        "zones": ""
    }
    ```

    You will also want to save the public IP address of the newly created VM in order to connect to the VM.

1. Try connecting to your new VM.

    Open a command prompt/terminal window and run the following command. Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    The first time you connect to the machine, you'll be asked if you trust the remote machine. By answering `yes`, the machine's ECDSA key fingerprint will be saved locally, so subsequent connections will be trusted.

    If everything looks fine, type `exit` to close the SSH session.

1. Open port 80 to allow incoming HTTP traffic to the new web application that you will create.

    Go back to Cloud Shell on the Azure portal. Issue the following command using your original resource group name for `<resource-group-name>`.

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    This command will open up the HTTP port on your VM that was named "MeanDemo" when it was created.

## Summary

With your new Ubuntu Linux VM ready to go, we can now connect to it to start installing the various components of the MEAN stack.