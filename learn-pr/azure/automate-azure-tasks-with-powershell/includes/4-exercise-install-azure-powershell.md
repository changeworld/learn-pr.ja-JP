In this unit, you will install **Azure PowerShell** on your local machine. Choose the appropriate section for your operating system.

## Linux and Mac
On Linux and macOS, the first step is to install **PowerShell Core**.

### Linux
As mentioned in the last unit, installing PowerShell for Linux will involve using a package manager. We will use **Ubuntu 18.04** for our example here, but we have [detailed instructions for other versions and distributions in our documentation](https://docs.microsoft.com/powershell/scripting/setup/installing-powershell-core-on-linux).

You will install PowerShell Core on Ubuntu Linux using the Advanced Packaging Tool (**apt**) and the Bash command line. 

1. Import the encryption key for the Microsoft Ubuntu repository. This will allow the package manager to verify that the PowerShell Core package you install comes from Microsoft.

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    ```

1. Register the **Microsoft Ubuntu repository** so the package manager can locate the PowerShell Core package.

    ```bash
    sudo curl -o /etc/apt/sources.list.d/microsoft.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list
    ```

1. Update the list of packages.

    ```bash
    sudo apt-get update
    ```

1. Install PowerShell Core.

    ```bash
    sudo apt-get install -y powershell
    ```

1. Start PowerShell to verify that it installed successfully.

    ```bash
    pwsh
    ```

### macOS
Next, install **PowerShell Core** on macOS using the Homebrew package manager.

> [!IMPORTANT]
> If the **brew** command is unavailable, you may need to install the Homebrew package manager. For details see the [Homebrew website](https://brew.sh/).

1. Install Homebrew-Cask to obtain more packages, including the PowerShell Core package:

    ```bash
    brew tap caskroom/cask
    ```

1. Install PowerShell Core:

    ```bash
    brew cask installs powershell
    ```

1. Start PowerShell Core to verify that it installed successfully:

    ```bash
    pwsh
    ```

## Install Azure PowerShell
After installing the base **PowerShell** product, install **Azure PowerShell** to add the Azure-specific commands.

### Windows
Install Azure PowerShell on Windows using the `Install-Module` PowerShell command.

> [!IMPORTANT]
> You must have PowerShell version 5.0 or higher to install Azure PowerShell. To check your version of PowerShell, use the following command: 
>
> `$PSVersionTable.PSVersion` 
>
>If the version number is lower than 5.0, follow the instructions for [upgrading existing Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell).

1. Open the **Start** menu and type **Windows PowerShell**.

1. Right-click the **Windows PowerShell** icon and select **Run as administrator**.

1. In the **User Account Control** dialog, select **Yes**.

1. Type the following command, and then press Enter:

    ```powershell
    Install-Module -Name AzureRM
    ```

1. If you are asked whether you trust modules from PSGallery, answer **Yes** or **Yes to All**.

> [!TIP]
> If you get an error message indicating that a version of the Azure PowerShell module is already installed, you can update to the _latest_ version by issuing the command:
> 
> `Update-Module -Name AzureRM`
> 
> As with the `Install-Module` command, answer **Yes** or **Yes to All** when prompted to trust the module.

### Linux or macOS
We use the same basic process to install the Azure PowerShell on either Linux or macOS. The procedure is the same for both operating systems. The difference is in getting an elevated PowerShell Core session.

1. In a terminal, type the following command to launch PowerShell Core with elevated privileges.

    ```bash
    sudo pwsh
    ```

1. Run the following command at the PowerShell Core prompt to install Azure PowerShell.

    ```powershell
    Install-Module AzureRM.NetCore
    ```

1. If you are asked whether you trust modules from **PSGallery**, answer **Yes** or **Yes to All**.

## Summary
You have setup your local machine(s) to administer Azure resources with Azure PowerShell. You can now use Azure PowerShell locally to enter commands or execute scripts. Azure PowerShell will forward your commands to the Azure datacenters where they will run inside your Azure subscription.