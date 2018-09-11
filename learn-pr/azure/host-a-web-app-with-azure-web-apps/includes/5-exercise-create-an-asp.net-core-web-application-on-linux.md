In this unit, you will create an ASP.NET Core web app using the .NET CLI on an Ubuntu machine.

## ASP.NET Core installation on Linux environment

Visit the Microsoft [.NET Downloads page](https://www.microsoft.com/net/download) and follow the same steps mentioned on the .NET Core SDK page. They are below. In order to execute the commands, you need to open a new **Terminal** command-line instance.

### Register Microsoft key and feed

Before installing .NET, you'll need to register the Microsoft key, register the product repository, and install the required dependencies. This only needs to be done once per machine.

```console
~$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
~$ sudo dpkg -i packages-microsoft-prod.deb
```

## Install the .NET SDK

Update the products available for installation, then install the .NET SDK.

At your command prompt, run the following commands:

```console
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
```

During the installation process, you might be asked to enter your account password. Provide your password and hit Enter to continue.

To verify the installation, type the following:

```console
dotnet --version
```

And the output displayed is:

```console
2.1.302
```

Now that you have the .NET Core SDK installed, you also have the ASP.NET Core installed. Let's see together how you can make use of the .NET CLI to create a new ASP.NET Core project using the commands we just learned.

## Open a Terminal window

First, you need to open the Terminal window. The Terminal window allows you to execute commands, and has a similar role to the Windows Command Prompt window.

## Create a new web project

The heart of the .NET CLI tools is the *dotnet* driver tool. Using this command, you will create a new ASP.NET Core web project.

To create a new ASP.NET Core MVC application, you only need to type the following commands:

```console
cd ~/Documents
mkdir BestBikeApp   # Hit Enter
cd BestBikeApp      # Hit Enter
dotnet new mvc      # Hit Enter
```

The above commands navigate to the *Documents* root folder, and then create a new folder. Then, you go inside that folder. Finally, you issue the .NET CLI command to generate a new ASP.NET MVC application with all the packages restored:

```console
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore-template-3pn-210 for details.

Processing post-creation actions...
Running 'dotnet restore' on /home/your-user/Documents/BestBikeApp/BestBikeApp.csproj...
...
```

All you have to do now is run the application by issuing this command:

```console
dotnet run
```

In the *terminal*, the *dotnet* command displays some useful information for the running app:

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Development
Content root path: /home/your-user/Documents/BestBikeApp
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

The output describes the situation after starting your app: the application is running and listening at port 5001 and 5002 (via HTTPS).

In order to run HTTPS locally on the machine in development mode, you need to create and trust a **self-signed certificate**.

## Create a self-signed certificate

Run the command below to generate a development self-signed certificate:

```console
dotnet dev-certs https -v
```

Running the command returns the following:

```console
The HTTPS developer certificate was generated successfully.
```

## Run the application

For this demonstration, we are using the Firefox browser.

Open the browser and type in the address `http://localhost:5001` to verify the application is running successfully.

![Screenshot showing a web browser view of the ASP.NET Core MVC template default web page.](../media/5-asp-core-mvc-default-template.PNG)

> [!NOTE]
> You need to **add an exception** for the application's URL because the development self-signed certificate couldn't be verified by Firefox.
