Now that you've got your app up and running on your local machine, it's time to get it published to Azure. 

In this unit, you will create, build, and run a new ASP.NET web application on your local machine.

## Create a new project

### Visual Studio for Windows

The first step is to start Visual Studio and create a local ASP.NET Core web application.

1. On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.

1. In the **New Project** dialog box, on the left-hand pane, select **Web**.

1. In the center pane, click **ASP.NET Core Web Application**.

1. At the bottom of the dialog box, in the **Name** field, enter **Alpine Ski House**.

1. Select a **Location** for your new solution.

1. Click the **OK** button to create your project.

1. In the **New ASP.NET Core Web Application** dialog box, you will see a selection of starting templates. For this exercise, select **Web Application**, and then click **OK** to create your project.

    ![New Project Dialog](../media-draft/3-aspnet-templates.png)

    > [!NOTE]
    > You can also select different starting templates in this dialog box depending on your web development requirements. At the top of the dialog box, you are also able to select the version of ASP.NET Core. You should select ASP.NET Core 2.0 or later.

1. You should now have your new ASP.NET Core web application solution.

    ![New Project Dialog](../media-draft/3-new-solution.png)

### Visual Studio Mac

1. On the Visual Studio start page, select **File**, then click **New**, and then click **Project..**.

1. Under .**NET Core**, select an **ASP.NET Core Web App**, and then click **Next**.

1. For the **Project Name**, type **AlpineSkiHouse**. This should also auto-populate the solution name.

1. Select a **location** on your local machine for the project.

1. Click **Create**.

## Build and test on your local machine

Now, let's build and test your new project on your local machine to ensure it builds and deploys locally before deploying to Azure.

1. Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger.

    ![New Project Dialog](../media-draft/3-webapp-launch.png)

Visual Studio starts IIS Express and runs the app. When Visual Studio creates a web project, a random port is used for the web server. In the preceding image, the port number is 44381. When you run the app, you'll likely see a different port number.

> [!TIP]
> Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes. Many developers prefer to use non-debug mode to quickly launch the app and view changes.

> [!IMPORTANT]
> You might notice the section at the top of the web page that provides a place for your privacy and cookie use policy. Select **Accept** to consent to tracking. This app doesn't track personal information. The template-generated code includes assets to help meet General Data Protection Regulation (GDPR).

## Summary

The first step to getting your ASP.NET site up and running is to create it and run it locally. Now that your site is created, you are ready to deploy it to Azure.
