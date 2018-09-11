Next, let's use the Azure CLI to create a resource group, and then to deploy a web app into this resource group. 

### Create a resource group

1. Open a bash shell on Linux or macOS, or open the Command Prompt window or PowerShell if working from Windows.

1. Start the Azure CLI and run the login command.

    ```bash
    az login
    ```
    If you do not get an Azure sign-in page in your web browser, follow the command-line instructions and enter an authorization code at [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

1. Create a resource group.

    ```bash
    az group create --location westeurope --name popupResGroup
    ```

1. Verify that the resource group was created successfully by listing all your resource groups in a table.

    ```bash
    az group list --output table
    ```

> [!TIP]
> You can also confirm the resource was created in the Azure portal. Open a web browser, sign in to the portal and navigate to the **Resource Groups** section. The new resource group should be displayed in the list.

1. If you have a lot of items in the group, you can filter the return values by adding a `--query` option, try this command:

    ```bash
    az group list --query '[?name == popupResGroup]'
    ```

    The query is formmated using **JMESPath** which is a standard query language for JSON requests. You can learn more about this powerful filter language at <http://jmespath.org/>. We also cover queries in more depth in the **Manage VMs with the Azure CLI** module.

### Steps to create a service plan

When you run Web Apps, using the Azure App Service, you pay for the Azure compute resources used by the app, and this depends on the App Service plan associated with your Web Apps. Service plans determine the region used for the app datacenter, number of VMs used, and pricing tier.

1. Create an App Service plan to run your app. The following command does not specify a pricing tier or VM instance details, so by default, you'll get a **Basic** plan with 1 **Small** VM instance.

    > [!WARNING]
    > The name of the app and plan must be _unique_, so add a suffix to the name and replace the `<unique>` text in the command below with a set of numbers, your initials, or some other piece of text to make sure it's unique in all of Azure. 

    ```bash
    az appservice plan create --name popupapp-<unique> --resource-group popupResGroup --location westeurope
    ```

1. Verify that the service plan was created successfully by listing all your plans in a table.

    ```bash
    az appservice plan list --output table
    ```

### Steps to create a web app

Next, you'll create the web app in your service plan. You can deploy the code at the same time, but for our example, we'll do this as separate steps.

1. Create the web app, supply the name of the plan you created above. **Just like the plan, the app name must be unique, replace the `<unique>` marker with some text to make the name globally unique.**
    ```bash
    az webapp create --name popupapp-<unique> --resource-group popupResGroup --plan popupapp-<unique>
    ```

1. Verify that the app was created successfully by listing all your apps in a table.

    ```bash
    az webapp list --output table
    ```

1. Make a note of the **DefaultHostName**; you will need this later.

### Steps to deploy code from GitHub

1. The final step is to deploy code from a GitHub repository to the web app. Let's use a simple PHP page available in the Azure Samples Github repository that displays "HelloWorld!" when it executes. Make sure to use the web app name you created.

    ```bash
    az webapp deployment source config --name popupapp-<unique> --resource-group popupResGroup --repo-url "https://github.com/Azure-Samples/php-docs-hello-world" --branch master --manual-integration
    ```

1. Once it's deployed, Azure will make your website available through the unique app name in the `azurewebsites.net` domain. For example, if my app name was "popupapp-mslearn123", then my website address would be: <http://popupapp-mslearn123.azurewebsites.net>. You will need to type in the correct URL to hit your specific instance.

1. The page displays "HelloWorld!"

1. Close the browser window.

## Summary

This exercise demonstrated a typical pattern for an interactive Azure CLI session. You first used a standard command to create a new resource group. You then used a set of commands to deploy a resource (in this example, a web app) into this resource group. This set of commands could easily be combined into a shell script, and executed every time you need to create the same resource.
