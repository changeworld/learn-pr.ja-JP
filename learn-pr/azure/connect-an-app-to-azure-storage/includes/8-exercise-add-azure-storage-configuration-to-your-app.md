::: zone pivot="csharp"
Let's add support to our .NET core application to retrieve a connection string from a configuration file. We'll start by adding the necessary plumbing to manage configuration in a JSON file.

## Create a JSON configuration file

1. Make sure you are in the correct working directory for your project.

1. Use the `touch` tool on the command line to create a file named **appsettings.json**.

    ```bash
    touch appsettings.json
    ```

1. Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE. The following commands are for the Cloud Shell editor, but are very similar to VS Code.
    
    ```bash
    code .
    ```

1. Select the **appsettings.json** file in the editor and add the following text. Save the file - in the online editor, there is a menu in the top right corner which has common file operations.

    ```json
    {
      "StorageAccountConnectionString": ""
    }
    ```

1. Next, select the project file (**PhotoSharingApp.csproj**) to open it in the editor.

1. Add the following configuration block to include the new file in the project and copy it to the output folder. This ensures that the app configuration file is placed in the output directory when the app is compiled/built.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk">
       ...
        <ItemGroup>
            <None Update="appsettings.json">
              <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
            </None>
        </ItemGroup>
    </Project>
    ```

1. Save the file. (Make sure you do this or you will lose the change when you add the package below!)

## Add support to read a JSON configuration file

A .NET Core application requires additional NuGet packages to read a JSON configuration file.

1. In the command prompt section of the window, add a reference to the  **Microsoft.Extensions.Configuration.Json** NuGet package.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration.Json
    ```

## Add code to read the configuration file

Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our console application.

1. Select **Program.cs** in the editor.

1. At the top of the file, a **using System;** line is present. Underneath that line, add the following lines of code:

    ```csharp
    using Microsoft.Extensions.Configuration;
    using System.IO;
    ```

1. Replace the contents of the **Main** method with the following code. This code initializes the configuration system to read from the **appsettings.json** file.

    ```csharp
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json");

    var configuration = builder.Build();
    ```

Your **Program.cs** file should now look like the following:

```csharp
using System;
using Microsoft.Extensions.Configuration;
using System.IO;

namespace PhotoSharingApp
{
    class Program
    {
        static void Main(string[] args)
        {
            var builder = new ConfigurationBuilder()
                .SetBasePath(Directory.GetCurrentDirectory())
                .AddJsonFile("appsettings.json");

            var configuration = builder.Build();            
        }
    }
}
```

::: zone-end

::: zone-pivot="javascript"

Let's add support to our Node.js application to retrieve a connection string from a configuration file. We'll start by adding the necessary plumbing to manage configuration from our JavaScript file.

## Create a .env configuration file

1. Make sure you are in the correct working directory for your project.

1. Use the `touch` tool on the command line to create a file named **.env**.

    ```bash
    touch .env
    ```

1. Open the project with the interactive editor, if you are working locally, use your editor of choice - we recommend **Visual Studio Code** which is an extensible cross-platform IDE. The following commands are for the Cloud Shell editor, but are very similar to VS Code.
    
    ```bash
    code .
    ```

1. Select the **.env** file in the editor and add the following text. Save the file - in the online editor, there is a menu in the top right corner which has common file operations.

    ```
    AZURE_STORAGE_CONNECTION_STRING=<value>
    ```

    > [!TIP]
    > The **AZURE_STORAGE_CONNECTION_STRING** value is a hard-coded environment variable used for Storage APIs to look up access keys. You can use your own name if you prefer - but you must supply the name to the when you create the `BlobService` object.

1. Save the file.

## Add support to read an environment configuration file

Node.js apps can include support to read from the **.env** file by adding the **dotenv** package.

1. In the command prompt section of the window, add a dependency to the  *dotenv** package.

    ```bash
    node install dotenv --save
    ```

## Add code to read the configuration file

Now that we have added the required libraries to enable reading configuration, we need to enable that functionality within our application.

1. Select *index.js** in the editor.

1. At the top of the file, a **#!/usr/bin/env node** line is present. Underneath that line, add a `require` statement to load the **dotenv** package. This will make environment variables defined in our **.env** file available to the program.

    ```javascript
    #!/usr/bin/env node
    require('dotenv').load();

    ```
::: zone-end

## Add the connection string to the configuration file

Now we need to get the storage account connection string and place it into the configuration for our app.

1. Sign in to the [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. Navigate to your storage account. You can use the **All Resources** section to find the storage account, or search by name from the _search box_ at the top of the portal window. 

1. Select the **Access Keys** blade of the storage account in the portal.

1. Copy the **key1** Connection string.

1. Paste in the contents of the access key you copied from the portal as the value for the connection string configuration variable.

Your configuration should now look similar to the following:

::: zone pivot="csharp"
    ```json
    {
        "StorageAccountConnectionString": "DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net"
    }
    ```
::: zone-end

::: zone-pivot="javascript"
    ```
    AZURE_STORAGE_CONNECTION_STRING=DefaultEndpointsProtocol=https;AccountName=[account-name];AccountKey=[account-key];EndpointSuffix=core.windows.net
    ```
::: zone-end

Now that we have that all wired up, we can start adding code to use our storage account.