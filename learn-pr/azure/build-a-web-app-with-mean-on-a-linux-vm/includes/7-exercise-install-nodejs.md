In this unit, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine. Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.

## Connect to the VM

In order to install Node.js, you have to connect to the VM using **ssh**. If you aren't still connected to your VM, run the following command. Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## Install Node.js

> [!Important]
> Ubuntu provides an unofficial package called **Node.js-legacy**. This package is not maintained by Node.js and is outdated.

1. Register the Node.js package repository, so **apt-get** can find the right package to install on your virtual machine.

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. Install the Node.js package on your Linux system.

    ```bash
    sudo apt-get install -y Node.js
    ```

1. Verify the Node.js installation succeeded by running the following simple Node.js command.

    ```bash
    node -v
    ```

    The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version that's available when you install the package.

## Summary

With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.