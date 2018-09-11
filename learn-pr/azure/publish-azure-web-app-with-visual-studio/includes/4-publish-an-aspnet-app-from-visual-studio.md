You created a site and you want to deploy it to Azure. Now we need to consider which Azure services to leverage to best support our needs. Web Apps provides a highly scalable, self-patching web hosting service for your applications.

Here, we look at how to use Visual Studio to publish your ASP.NET Core web application to an Azure App Service plan.

## Azure subscription

To publish to Azure, you need to have an Azure subscription. You can use an [Azure Free Subscription](https://azure.microsoft.com/free/) for testing the Azure App Service features.

## What is Web Apps?

The Web Apps feature of Azure App Service is a service for hosting web applications, REST APIs, and mobile backends. Web Apps host code is written in a variety of languages, such as .NET, .NET Core, Java, Ruby, Node.js, PHP, and Python. Web Apps is ideal for most websites, particularly if you don't need a lot of control over the hosting infrastructure.

## What is the App Service plan?

The App Service plan defines the compute resources your app will consume, where those resources are located, how many additional resources the plan can consume, and the type of service plan in use. These compute resources are analogous to the server farm in conventional web hosting. One or more apps can be configured to run on the same App Service plan.

When you deploy your apps, you can create a new App Service plan or you can continue to add apps to an existing plan.  However, apps in the same App Service plan all share the same compute resources. To determine whether the new app has the necessary resources, you need to understand the capacity of the existing App Service plan, and the expected load for the new app. Overloading an App Service plan can potentially cause downtime for your new and existing apps.

You can define an App Service plan in advance in the Azure portal with PowerShell or the Azure CLI, or set one up as you publish your application in Visual Studio.

Each App Service plan defines:

- Region (West US, East US, and so on)
- Number of VM instances
- Size of VM instances (small, medium, large)
- Pricing tier (Free, Shared, Basic, Standard, Premium, Premium V2, Isolated, Consumption)

## Specify the region

When creating an App Service plan, you have to define a region or location where that plan will be hosted. Typically, you would choose a region geographically close to your expected customers.

## What are the pricing and reliability levels?

A key factor when deploying apps to Azure App Service is choosing the right service plan. Your App Service plan can be scaled up and down at any time. You can choose a lower pricing tier at first and scale up later when you need more App Service features.

There are a few categories of pricing tiers:

**Shared compute**: **Free** and **Shared**, the two base tiers, run an app on the same Azure VM as other App Service apps, including apps of other customers. These tiers allocate CPU quotas to each app that runs on the shared resources, and the resources cannot scale out.

Free and Shared plans are best for small-scale personal projects with very limited traffic demands, with a set limit of 165 MB of outbound data every 24 hours.

**Dedicated compute**: The **Basic, Standard, Premium, and Premium V2** tiers run apps on dedicated Azure VMs. Only apps in the same App Service plan share the same compute resources. The higher the tier, the more VM instances are available to you for scale out.

The Standard service plan is best suited for live production workloads, where you are publishing commercial applications to customers.
The Premium service plans support high-capacity web apps where you do not want the additional costs of a dedicated (isolated) plan.

**Isolated**: This tier runs dedicated Azure VMs on dedicated Azure virtual networks, which provide network isolation on top of compute isolation to your apps. It provides the maximum scale-out capabilities. You would only select an Isolated service plan when you have a specific requirement for the highest levels of security and performance.

Isolate your app into a new App Service plan when:

- The app is resource-intensive
- You want to scale the app independently from the other apps in the existing plan
- The app needs resources in a different geographical region

**Consumption**: This tier is only available to function apps. It scales the functions dynamically depending on workload. For more information, see the Azure Functions hosting plans comparison.

## Specify the resource group

As with most Azure resources, you will need to specify the resource group you want to use. You can use an existing resource group or create a new one directly from Visual Studio. A resource group is a logical container into which Azure resources like web apps, databases, and storage accounts are deployed and managed. It is a useful mechanism for keeping associated resources together.

## Deploy your web app from Visual Studio

It's a short process to publish your app to Azure from Visual Studio.

### Select the project

1. Right-click the project in Solution Explorer.

1. Select **Publish > Publish to Azure**.

### Configure the App Service plan in Windows

1. Configure the App Service plan

    - Hosting: You will configure your App Service plan in this tab. It specifies the location, size, and features of the web server that hosts your app. You can select an existing hosting plan or create a new one. Windows will automatically generate globally unique names which you can change during setup.
    - Services: You can configure a SQL database for your site here.

        > [!NOTE]
        > Although you can connect and manage a SQL database in Azure from Visual Studio, that is beyond the scope of this module.

1. Click **Create** to deploy the app. Once this is done, Visual Studio will launch the web page where your site is hosted.

### Configure the App Service plan for Mac

1. You can select an existing App Service plan if you have one set up in Azure, or you can create a new one.

1. Configure your  App Service plan in this tab. It specifies the location, size, and features of the web server that hosts your app. You can select an existing hosting plan or create a new one. Remember that Azure requires the name of your website and all resources to be globally unique.

1. Click **Create** to deploy the app. Once this is done, Visual Studio will launch the web page where your site is hosted.

## Summary

ASP.NET Core web applications and Azure App Services provide a flexible, scalable solution for hosting your ASP.NET web app. As with all Azure services, you will need to provide a resource group and select the pricing tier which best fulfills your needs.
