First things first we need to make sure you are setup with a few accounts and have the sample code working locally.

## Create an Azure Account

You will need an accout on Azure. If you don’t already have one then you can signup and get a free years worth of services by following this link:

[Crate Azure Account](https://azure.microsoft.com/free)

## Create a Slack Workspace

To create a slack command you need admin privillages on a slack workspace.

So you eiher need to make sure you have admin privillages on an existing workspace or create a brand new slack workspace, like so:

[Crate Slack Account](https://slack.com/create)

## Clone the starter code

This module is designed so you follow a set of instructions step by step.

We'll provide all the code you need to write in order to create a completed application.

We do provide some initial bootstrap code to get you started, so to begin you must clone this _stater_ code to your local computer.

> TODO - Where is the code going?

```bash
git clone [github-url]
```

Then install the required pacakges using:

```bash
npm install
```

> We recommend you follow this module step by step. However if you become stuck and need help then you can find the completed code in the `completed` branch, like so:

```bash
git checkout completed
```

We will be coding up our application in `TypeScript`. NodeJS does not know how to run TypeScript, so as we develop we need to convert our `TypeScript` code to `JavaScript`. All the required tooling was installed int he step above so to convert the TypeScript run the command:

```bash
npm run build
```

If the avove command ran success fully next to the `*.ts` files you should now also see `*.js` and `*.map` files.

> NOTE: Keep this command running in a terminal shell, this will continuously watch for any changed to the TypeScript file and convert them to JavaScript files.
>
> If for some reason the files are not getting converted then check the console output in the terminal window, there may be errors in your TypeScript code.

## Install the Azure Command Line Tools

We will be interacting with Azure usin the Azure CLI, follow the instructions here to install the tool locally on your computer: https://docs.microsoft.com/cli/azure/install-azure-cli

Once installed, try loging in with your Azure credentials.

```bash
az login -o table
```

The above command will open a browser window, you may need to authenticate, once authetication is complete you the browser will close and the terminal will print some output, like so:

```bash
You have logged in. Now let us find all the subscriptions to which you have access...
```

## Install the Azure Functions Runtime

We will be hosting our code on the Azure Functions service.

In order to run and debug Azure Functions locally we also need to install the Function Tools, instructions can be found here: https://docs.microsoft.com/azure/azure-functions/functions-run-local

<!-- ## Install Visual Studio Code & Azure Functions Extension

There are many ways you can interact with Azure, in this module we will be interacting with Azure using Visual Studio Code and the associated Azure Functions Extensions plugin.

1. Download and install visual studio code from https://code.visualstudio.com/

2. Install the Azure Functions Code Extensions by following the instructions here: https://code.visualstudio.com/tutorials/functions-extension/getting-started -->

<!-- ## Install the Azure CLI

We will also be using the Azure Command Line Interface, install it using the most appropriate method for your OS using the information here:

[Install Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)

## Install the Function Tools CLI

In order to run and debug Azure Functions locally we also need to install the Function Tools, instructions can be found here:

[Install Function CLI](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

> Please install the V2 version of the Functon Tools -->
