Now that your VM is up and running, let's do something with it. Here you'll install a web server and serve up a basic web page that displays the VM's hostname.

To configure a VM, you have several choices. You can connect directly and interactively configure your system. For example, on Windows systems you can create a Remote Desktop session to connect to the UI of your remote Windows computer as if you were seated at it. On Linux systems, you can create an SSH connection to securely work with your remote Linux system from the terminal.

Manual configuration is a good start, but as you add systems, you can automate your deployments. Automation involves running repeatable processes such as programs and scripts that take care of the heavy lifting for you.

::: zone pivot="windows-cloud"

Here, you'll configure IIS remotely from your Cloud Shell session using a feature of Windows-based Azure virtual machines called the Custom Script Extension.

::: zone-end

::: zone pivot="linux-cloud"

Here, you'll interactively log into your VM and configure Nginx.

::: zone-end

::: zone pivot="windows-cloud"

## What is IIS?

Internet Information Services, or IIS, is a web server that runs on Windows. You can use IIS to serve standard web content (HTML, CSS, and JavaScript) or run ASP.NET and other kinds of web applications. IIS comes with Windows Server, but you need to activate it to start serving web pages.

## What's the Custom Script Extension?

The Custom Script Extension is an easy way to download and run scripts on your Azure VMs. It's just one of the many ways you can configure your VM once your VM is up and running.

You can store your scripts in Azure storage or in a public location such as GitHub. You can run scripts manually or as part of a more automated deployment. Here, you'll run a PowerShell command to download an IIS configuration script from GitHub and execute it on your VM.

## Configure IIS

From Cloud Shell, run this code to download and execute a PowerShell script that installs IIS and configures a basic home page.

```powershell
$settings = @'
  {"fileUris":["https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/663af8c47dbc5f27fc98412634246883af6f6159/iis.ps1"],
   "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File iis.ps1"}
'@

Set-AzureRmVMExtension `
    -ExtensionName "IIS" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.4 `
    -SettingString $settings `
    -Location "East US" `
    -Verbose
```

The process takes a few minutes to complete.

In the meantime, you can [examine the PowerShell script](https://gist.githubusercontent.com/tpetchel/26f9dab2628a80bf87a33caeed1b6ded/raw/663af8c47dbc5f27fc98412634246883af6f6159/iis.ps1) from a separate browser tab if you'd like. The script installs IIS and configures the home page to display a welcome message along with the VM's computer name, "myVM".

## Verify the configuration

Now that IIS is set up, let's verify that it's running.

1. From Cloud Shell, run this cmdlet to print your VM's public IP address to the console.

    ```powershell
    Get-AzureRmPublicIPAddress `
      -ResourceGroupName "myResourceGroup" `
      -Name "myPublicIPAddress" `
      | select IpAddress
    ```

You see output similar to this.

    ```console
    IpAddress
    ---------
    40.117.84.233
    ```

In a new browser tab, navigate to your VM's IP address. You see your welcome message and your VM's name.

![](../media/web-server-browser.png)

::: zone-end

::: zone pivot="linux-cloud"

## What is Nginx?

Nginx (pronounced "engine-x") is a popular, free, open-source web server that runs on Unix, Linux, and Windows. Here you'll use Nginx to serve a basic web page.

## What is SSH?

SSH stands for Secure Shell. It's a communication protocol that helps you connect to a remote system and work with it as if you were seated at it.

SSH runs over a secure connection. There are two common ways to authenticate access: _password authentication_ and _key-based authentication_. Here you'll use key-based authentication because most consider it to be more secure.

We won't go into the details of how key-based authentication works here. All you need to know for now is that key-based authentication involves two files &mdash; a _private key_ that exists on your workstation and a _public key_ that exists on the computer you want to connect to. When you created your VM, you specified the `--generate-ssh-keys` option. This option sets up these files for you.

## Connect to your VM from Cloud Shell

Let's connect to your Linux VM over SSH and run a few commands to get a feel for how things work.

1. First, run `whoami` to get your username.

    ```bash
    whoami
    ```
    You see your username. Here's an example.
    ```console
    thomas
    ```
    You'll use this name to connect to your VM. By default, when you create a Linux VM from Cloud Shell, the username on the VM is the same as your Cloud Shell username.

1. Run this `az vm list-ip-addresses` command to list your VM's public IP addresses.

    ```azurecli
    az vm list-ip-addresses -g myResourceGroup -n myVM --query "[].virtualMachine.network.publicIpAddresses[0].ipAddress" -o tsv
    ```

    > [!NOTE]
    > This command is pretty complex, especially `--query` argument. But you'll be a pro at this as you dig in and explore Azure.

    You see your VM's public IP address. Here's an example.
    ```console
    137.135.110.210
    ```

    Make a note of the IP address, as you'll need it near the end of this exercise.

1. Create an SSH connection to your VM. Replace the username and IP address shown in this example with yours.

    ```bash
    ssh thomas@137.135.110.210
    ```
    When prompted, enter "yes" to connect.

1. Once logged in, your commands run on the VM. Let's explore it: run `ls` to print the contents of the root directory.

    ```bash
    ls -w 40 /
    ```
    You see the contents of the root directory.
    ```console
    bin   initrd.img  mnt   sbin  usr
    boot  lib         opt   snap  var
    dev   lib64       proc  srv   vmlinuz
    etc   lost+found  root  sys
    home  media       run   tmp
    ```
1. As a second example, run `hostname` to print out your VM's name.
 
    ```bash
    hostname
    ```
    You see the name of your VM, **myVM**.
    ```console
    myVM
    ```

## Configure Nginx

Let's install Nginx on the VM and configure a basic home page.

1. From your SSH connection, start by updating the `apt` cache.

    ```bash
    sudo apt-get update
    ```

    This command updates the system's package index. Keeping the package index up to date helps ensure you're getting the most up-to-date software.

    > [!NOTE]
    > The `sudo` part enables you to run commands as the `root` user. Running as `root` is required to make system changes such as installing software.

1. Next, install Nginx.

    ```bash
    sudo apt-get install nginx -y
    ```
1. Run this command to add some initial content to the default home page, `/var/www/html/index.html`.

    ```bash
    echo "Hello, Azure! My name is $(hostname)." | sudo tee -a /var/www/html/index.html
    ```
    The home page displays a welcome message, along with the VM's computer name.

## Verify the configuration

Now that Nginx is set up, let's verify that it's running. You'll start by verifying things locally, then remotely.

1. Run `curl localhost`. "Localhost" is the VM, since you're still connected to it. `curl` will fetch the homepage from the webserver locally and print its contents to the console.

    ```bash
    curl localhost
    ```
    You see this.
    ```console
    Hello, Azure! My name is myVM.
    ```
1. Run `exit` to end your SSH session. From this point on, your commands will no longer run in the VM, they will run in the Cloud Shell system.

    ```bash
    exit
    ```
1. Run this `az vm open-port` command to open port 80 (HTTP) through the firewall.

    ```azurecli
    az vm open-port --name myVM --port 80 --resource-group myResourceGroup
    ```

Now let's confirm that we can access the web server from the Internet. From a new browser tab, navigate to your VM's IP address. You see your welcome message and your VM's name.

![](../media/web-server-browser.png)

::: zone-end

## Summary

Your VM is running and can now serve up web pages, but what does that mean for you?

Remember, every journey starts with the basics, and almost any great innovation born in the cloud, from companies big and small, started with a similar setup to yours. As your idea evolves, it begins making a positive impact on your business and your users.