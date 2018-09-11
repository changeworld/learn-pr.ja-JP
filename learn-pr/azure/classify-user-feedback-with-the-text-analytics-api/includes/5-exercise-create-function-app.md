To build our solution, we'll need to host some code.  An Azure Functions function app is a good place to host our logic. 

## Create a Function App to host our function

[!INCLUDE [resource-group-note](./rg-notice.md)]

1. Make sure you are signed in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with your Azure account.

1. Select the **Create a resource** button found on the upper left-hand corner of the Azure portal, then select **Compute** > **Function App**.

1. Enter the function app settings as specified in the following table.


    | Setting      | Suggested value  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **App name** | Globally unique name | Name that identifies your new function app. Valid characters are `a-z`, `0-9`, and `-`.  | 
    | **Subscription** | Your subscription | The subscription under which this new function app is created. | 
    | **Resource Group**|  [!INCLUDE [resource-group-name](./rg-name.md)] | Name for the  resource group in which to create your function app.<br/><br/>Make sure to select **Use existing** and use the resource group that we created in the last exercise. That way, all resource we made in this module are kept together. | 
    | **OS** | Windows | The operating system that hosts the function app.  |
    | **Hosting** |   Consumption plan | Hosting plan that defines how resources are allocated to your function app. In the default **Consumption Plan**, resources are added dynamically as required by your functions. In this [serverless](https://azure.microsoft.com/overview/serverless-computing/) hosting, you only pay for the time your functions run.   |
    | **Location** | West US | Choose a [region](https://azure.microsoft.com/regions/) near you or near other services your functions access.<br/><br/>Select the same region that you used when creating the Text Analytics API account in the last exercise. |
    | **Storage account** |  Globally unique name |  Name of the new storage account used by your function app. Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only. This dialog populates the field with a unique name that is derived from the name you gave the app. However, feel free to use a different name or even an existing account. |

3. Select **Create** to provision and deploy the function app.

4. Select the Notification icon in the upper-right corner of the portal and watch for a **Deployment in progress** message similar to the following message.

![Notification that function app deployment is in progress](../media-draft/func-app-deploy-progress-small.PNG)

5. Deployment can take some time. So, stay in the notification hub and  watch for a **Deployment succeeded** message similar to the following message.

![Notification that function app deployment has completed](../media-draft/func-app-text-analytics-deploy-success.png)

6. Congratulations! You've created and deployed your function app. Select **Go to resource** to view your new function app.

>[!TIP]
>Having trouble finding your function apps in the portal, try [adding Function Apps to your favorites in the Azure portal](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## Create a function to hold our logic

Now that we have a function app, it's time to create a function. A function is activated through a trigger. In this module, we'll use a Queue trigger. The runtime will poll a queue and start this function to process a new message.

1. Expand your new function app, then hover over the **Functions** collection. Select the Add (**+**) button when it appears to start the function creation process.

![Animation of the plus sign appearing when the user hovers over the functions menu item.](../media-draft/func-app-plus-hover-small.gif)

2. In the **Get started quickly** page that now appears, select **Custom function**, which loads the list of available function templates. 

1. Select **JavaScript** on the **Queue trigger** template list entry.

![Screenshot of Azure Functions templates with JavaScript selected on the Queue trigger entry.](../media-draft/quickstart-select-queue-trigger.png)

1. In the **New Function** dialog that appears, enter the following values.


|Property  |Value  |
|---------|---------|
|Language     |   **JavaScript**      |
|Name     |   **discover-sentiment-function**      |
|Queue name     |   **new-feedback-q**      |
|Storage account connection        |  **AzureWebJobsDashboard**       |

![Screenshot of Azure Functions templates with JavaScript selected on the Queue trigger entry.](../media-draft/new-function-dialog.png)

5. Select **Create** to begin the function creation process.

1. A function is created in your chosen language using the Queue Trigger function template. While we'll implement the function in JavaScript in this module, you can create a function in any [supported language](https://docs.microsoft.com/azure/azure-functions/supported-languages).

When the create process is complete, the code editor opens in the portal and loads the *index.js* page. This file is the code file where we write our function logic.

## Try it out

Let's test what we have so far. We haven't written any code yet, so this test is to make sure what we've configured so far, runs.

1. Click **Run** at the top of the code editor.

2. Observe the **Logs** tab that opens at the bottom of the screen. If everything works as planned, you'll see a message similar to the following message.
![Screenshot of response message of a successful call to our function.](../media-draft/func-default-run.PNG)

The **Run** button started our function and passed *sample queue data*, the default text from the **Test** request window to our function.

Nice work! You've successfully added a Queue-triggered function to your function app and tested to make sure it's working as expected! We'll add more functionality to the function in the next exercise.

 Let's look briefly at the function's other file, the *function.json* config file. The configuration data from this file is shown in the following JSON listing.

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "new-feedback-q",
      "connection": "AzureWebJobsDashboard"
    }
  ],
  "disabled": false
}
```

As you can see, this function has a trigger binding named **myQueueItem** of type `queueTrigger`. When a new message arrives in the queue we've named **new-feedback-q**, our function is called. We reference the new message through the myQueueItem binding parameter. Bindings really do take care of some of the heavy lifting for us!

In the next step, we'll add code to call the Text Analytics API service.

>[!TIP]
>You can see index.js and function.json by expanding the **View Files** menu on the right of the function panel in the Azure portal. 

This exercise was all about getting our Azure Functions infrastructure in place. We have a working function hosted in a function app that runs when a new message arrives in our queue that we've named [!INCLUDE [input-q](./q-name-input.md)]. The real fun begins in the next exercise, when we add code to call a Microsoft Cognitive Service to do sentiment analysis.