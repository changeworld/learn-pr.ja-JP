You now have a local running ASP.NET Core web application. In this unit, you'll publish your application to Azure App Service.

### Visual Studio for Windows

1. In Solution Explorer, right-click your project and select **Publish**.

1. In the dialog box that appears, on the left, choose **App Service** as your publish target.  On the right, select **Create New** to create a new app service.

1. Click the **Publish** button to create your new app service.

> [!NOTE]
> When you deploy your apps, you can create a new App Service plan or you can continue to add apps to an existing plan. For this exercise, you will create a new **Azure app service**.

### Visual Studio Mac

1. In Solution Explorer, right-click your project.

1. Select **Publish**.

1. Click **Publish to Azure**. This will connect to your Azure account. It may take a few minutes to connect and list your current services.

1. Click **New** to create a new app service.

## Configure your new Azure App Service

1. In the **Create App Service dialog** box, click **Add an account**, and sign in to your Azure subscription. If you're already signed in, select the account containing the desired subscription from the drop-down menu.

1. Enter the required information about your App Service plan.

    ![Create App Service](../media-draft/5-CreateAppService.png)

    In the **Create App Service** dialog box, you will provide the following information:

    - **App Name**: This is the name of your application.  This will determine the URL of the published application, which will be https://_AppName_.azurewebsites.net.  It has to be a unique value. Therefore, you will have to try out some different names to find one that is not reserved.

    - **Subscription**: The Azure subscription you wish to deploy the app service to.

    - **Resource Group:** Click the **New...** button next to the resource group, and enter a unique name for the resource group.

        ![New Resource Group](../media-draft/5-NewResourceGroup.png)

    - **Hosting Plan:** The hosting plan specifies the location, size, and features of the web server farm that hosts your app. You can select an existing hosting plan or create a new one. For this exercise, you will create a new one.

        Click the **New...** button next to the hosting plan.

        ![New Hosting Plan](../media-draft/5-NewHostingPlan.png)

        Give your hosting plan a name, and then specify the location and the size.  
        
        > [!NOTE]
        > Specifying different locations and sizes will impact the cost of the service. For the exercise, it is recommended that you specify the **Free** size, which will ensure you won't incur any costs.

    - **Application Insights:** Specifies if you want to use Application Insights for your application. For this exercise, we recommend that you choose **None.**

1. Click the **Create** button to start provisioning your app service. You will see progress as it deploys:

    ![Deploy Progress](../media-draft/5-DeployProgress.png)

1. Once the app service is provisioned, Visual Studio will build and deploy your web app.  In your Visual Studio build output, you can see the output of the deployment.

    ![Publish Result](../media-draft/5-PublishResult.png)

1. Congratulations, your ASP.NET Core web application is now published and live. You will be able to see the final URL for your site in the build output and also on the publishing page in Visual Studio.

    ![Publish Result](../media-draft/5-PublishPage.png)

1. To test your website, go to the URL indicated.

    ![Live Site](../media-draft/5-WebPageLive.png)

## Summary

This exercise demonstrated the process of provisioning an Azure app service and deploying an ASP.NET Core web application. You now have a live web app.
