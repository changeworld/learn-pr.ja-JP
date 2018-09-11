In this unit, you will install Visual Studio on either your Windows or your macOS computer. On Windows, the Azure development workload will need to be installed. And on Visual Studio for Mac, the built-in Connected Services workflow will enable you to build apps for Azure App Service. At the end, you will be ready to start creating applications and publishing them to Azure.

## Exercise steps

There are slight differences in installing Visual Studio between Windows and macOS. The following sections outline these differences.

### Windows

1. Download the Visual Studio installer from https://visualstudio.microsoft.com/downloads/.

1. Run the installer and it will open the Workloads window.

1. Choose the **Azure development** workload.

    The following screenshot shows the Visual Studio Installer workload selected to allow Azure development within Visual Studio.

    ![Screenshot of the Visual Studio Installer with the Azure development workload highlighted.](../media/5-select-azure-workload.png)

1. (Optional) Install the ASP.NET and web development workload to be ready to create web applications for Azure.

1. Click **Install**, and wait for Visual Studio to install.

1. When the installation is complete, open Visual Studio.

1. Go to the View menu in Visual Studio and make sure you have the **Cloud Explorer** option.

    The following screenshot shows the Cloud Explorer menu option that will be present if you have the Azure development workload installed.

    ![Screenshot of the Visual Studio View menu with the Cloud Explorer menu option highlighted.](../media/5-verify-cloud-explorer.png)

### macOS

1. Go to https://visualstudio.microsoft.com/ and download the Visual Studio for Mac installer.

1. Click the VisualStudioInstaller.dmg file to mount the installer and then run it by double-clicking the logo.

1. Acknowledge the Privacy and License terms when presented.

1. The installer will ask which components you wish to install. Azure components are already part of Visual Studio for Mac, but it is recommended to install the **.NET Core** platform to develop web experiences for Azure.

    The following screenshot shows the .NET Core platform required to add Azure development capabilities to Visual Studio for Mac.

    ![Screenshot of the Visual Studio for Mac installer with the selected .NET Core platform option highlighted.](../media/5-vsmac-install-net-core.png)

1. Click **Install and Update** once you are happy with the selections, and wait for the installer to complete.

1. If you are prompted to elevate the permissions needed, use your administrator credentials to do so.

1. Once the installer is complete, start Visual Studio for Mac.
