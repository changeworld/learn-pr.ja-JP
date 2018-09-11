The following is a high-level illustration of what we're going to build in this exercise.

![Visual representation of default HTTP trigger, showing HTTP request and response as well as respective req and res binding parameters.](../media-draft/default-http-trigger-visual-small.PNG)

We'll create a function that will start when it receives an HTTP request and will respond to each request by sending back a message. The parameters `req` and `res` are the trigger binding and output binding respectively. Let's get going!

Sign in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with your Azure account.

### Create a function app

Let's create a function app that we'll use throughout this entire module. A function app lets you group functions as a logical unit for easier management, deployment, and sharing of resources.

[!INCLUDE [resource-group-note](./rg-notice.md)]

1. Select the **Create a resource** button found on the upper left-hand corner of the Azure portal, then select **Compute** > **Function App**.
1. Set the function app properties as follows:


    | Property      | Suggested value  | Description                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **App name** | Globally unique name | Name that identifies your new function app. Valid characters are `a-z`, `0-9`, and `-`.  | 
    | **Subscription** | Your subscription | The subscription under which this new function app is created. | 
    | **Resource Group**|  [!INCLUDE [resource-group-name](./rg-name.md)] | Name for the new resource group in which to create your function app. | 
    | **OS** | Windows | The operating system that hosts the function app.  |
    | **Hosting** |   Consumption plan | Hosting plan that defines how resources are allocated to your function app. In the default **Consumption Plan**, resources are added dynamically as required by your functions. In this [serverless](https://azure.microsoft.com/overview/serverless-computing/) hosting, you only pay for the time your functions run.   |
    | **Location** | West Europe | Choose a [region](https://azure.microsoft.com/regions/) near you or near other services your functions access. |
    | **Storage account** |  Globally unique name |  Name of the new storage account used by your function app. Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only. This dialog populates the field with a unique name that is derived from the name you gave the app. However, feel free to use a different name or even an existing account. |


3. Select **Create** to provision and deploy the function app.

4. Select the Notification icon in the upper-right corner of the portal and watch for a **Deployment in progress** message similar to the following message.

![Notification that function app deployment is in progress](../media-draft/func-app-deploy-progress-small.PNG)

5. Deployment can take some time. So, stay in the notification hub and  watch for a **Deployment succeeded** message similar to the following message.

![Notification that function app deployment has completed](../media-draft/func-app-deploy-success-small.PNG)

6. Congratulations! You've created and deployed your function app. Select **Go to resource** to view your new function app.

>[!TIP]
>If you are having trouble finding your function apps in the portal, find out how to [add Function Apps to your favorites in the portal](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

### Create a function

Now that we have a function app, it's time to create a function. A function is activated through a trigger. In this module, we'll use an HTTP trigger.

1. Expand your new function app, then hover over the functions collection and select the Add (**+**) button next to **Functions**. This action starts the function creation process. The following animation illustrates this action.

![Animation of the plus sign appearing when the user hovers over the functions menu item.](../media-draft/func-app-plus-hover-small.gif)

2. In the **Get started quickly** page, select **WebHook + API**, select a language for your function, and click **Create this function**.

3. A function is created in your chosen language using the template for an HTTP triggered function. In this exercise, we'll create a JavaScript function.

### Try it out

Let's test what we have so far by doing the following:

1. In your new function, click **</> Get function URL** at the top right, select **default (Function key)**, and then click **Copy**.

2. Paste the function URL you copied into your browser's address bar. Add the query string value `&name=<yourname>` to the end of this URL and press the `Enter` key on your keyboard to execute the request. You should see a response similar to the following response returned by the function displayed in your browser.  

Nice work! You have now added a HTTP-triggered function to your function app and tested to make sure it is working as expected!

![Screenshot of response message of a successful call to our function.](../media-draft/default-http-trigger-response-small.PNG)

As you can see from this exercise so far, you have to select a trigger type when creating a function. Every function has one, and only one trigger. In this example, we're using an HTTP trigger, which means our function starts when it receives an HTTP request. The default implementation, shown in the following screenshot in JavaScript, responds with the value of a parameter *name* it received in the query string  or body of the request. If no string was provided, the function responds with a message asking whoever is calling to supply a name value.

![Screenshot of default JavaScript implementation of a HTTP-triggered Azure function.](../media-draft/default-http-trigger-implementation-small.PNG)

All of this code is in the *index.js* file in this function's folder. Let's look briefly at the function's other file, the *function.json* config file. This configuration data is shown in the following JSON listing.

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

As you can see, this function has a trigger binding named **req** of type `httpTrigger` and an output binding named **res**  of type `HTTP`. In the preceding code for our function, we saw how we accessed the payload of the incoming HTTP request through our **req** parameter. Similarly, we sent an HTTP response simply by setting our **res** parameter. Bindings really do take care of some of the heavy lifting for us!

>[!TIP]
>You can see index.js and function.json by expanding the **View Files** menu on the right of the function panel in the Azure portal.  

### Explore binding types

1. Notice under the function entry there is a set of menu items as shown in the following screenshot.

![Screenshot showing menu items under a function in the Function Apps blade.](../media-draft/func-menu-small.PNG)

2. Select the Integrate menu item to open the integration tab for our function. If you have been following along with this unit, the integrate tab should look very similar to the following screenshot.

![Screenshot showing integrate UI or tab.](../media-draft/func-integrate-tab-small.PNG)

Notice that we have already defined a trigger and an output binding as shown in this screenshot. You can also see that we can't add more than one trigger. In fact, to change the trigger for our function we would have to first delete the trigger and create a new one.

On the other hand, the **Inputs** and **Outputs** sections of this form display a plus `+` sign to add more bindings.

3. Select **+ New Input** under the **Inputs** column. A list of all possible input binding types is displayed as shown in the following screenshot.

![Screenshot showing the list of possible input bindings.](../media-draft/func-input-bindings-selector-small.PNG)

Take a moment to consider each of these input bindings and how you might use them in a solution. There are a lot to choose from. This list may even have changed by the time you read this module as we continue to support more data sources.

4. Select **Cancel** to dismiss this list.

5. Select **+ New Output** under the **Outputs** column. A list of all possible output binding types is displayed as shown in the following screenshot.

![Screenshot showing the list of possible output bindings.](../media-draft/func-output-bindings-selector-small.PNG)

Again, you have lots of options here, as shown by the need for a scroll bar to the right in this screenshot.

>[!TIP]
>To learn more details about the bindings that are supported, check out the [list of supported bindings](https://docs.microsoft.com/azure/azure-functions/functions-versions) in the Azure Functions documentation.

So far we've learned how to create a function app and add a function to it. We've seen a simple function in action that runs when an HTTP request is made to it. We've also explored the portal UI and types of input and output binding that are available to our functions. In the next unit, we'll use an input binding to read text from a a database.