In this unit, you'll upload your ASP.NET Core application to an Azure Web App.

## Create a staging deployment slot

1. Open the Web App resource you created previously. If you have closed its resource blade, you can find it again by searching for the app in **All resources** or the containing resource group in **Resource groups**.

1. Click the **Deployment slots** menu item on the left-side navigation.

1. Inside the **Deployment slots** blade, click the **Add Slot** button on the top navigation bar of the deployment slots blade.

1. The Azure portal opens the **Add a slot** blade as shown below.

    1. Give your deployment slot a name. In this case, use `staging`.

    2. To choose a **Configuration Source**, you have two options.

        * You can choose to clone the configuration elements from any existing deployment slot or Web App created on Azure.
        * Or you can choose not to clone any configuration elements. Select the option **Don't clone configuration from an existing slot**.

        For this deployment slot, choose the second option: **Don't clone configuration from an existing slot**. You will configure it directly.

    ![Screenshot of the Azure portal showing the configuration for a new staging deployment slot.](../media/7-new-deployment-slot-blade.png)

1. Click the **OK** button at the bottom of the blade to create your new deployment slot.

1. Once the deployment slot is successfully created, the Azure portal navigates you back to the **Deployment slots** blade of your web app.

    Now, you can see the new deployment slot that you have just created.

    ![Screenshot of the Azure portal showing the Deployment slots blade with the new slot created.](../media/7-deployment-slot-created.png)

1. Select the new deployment slot.

1. The Azure portal navigates to the **Overview** page of the newly created deployment slot.

    ![Staging deployment slot](../media/7-deployment-slot-staging.png)

    Notice the **URL** of the staging deployment slot. It is a different URL from what you saw previously, with the slot name appended.

    A deployment slot is treated as a full Web App inside Azure. However, it is a special type that is a child of the original web app and can be swapped with the original web app.

    If you click the **URL**, you will see the same default page that Azure created for the web app the first time we created it in the Azure portal.

Now that the staging deployment slot is created successfully, you need to configure **deployment credentials**.

## Create deployment credentials

Azure requires deployment credentials to be set up before you can start the actual deployment process. For that reason, you will learn how to create your own deployment credentials.

1. Click the **Deployment credentials** menu item on the left-side navigation.

1. The Azure portal navigates to the **Deployment credentials** blade as shown below.

    Enter a **username** and **password** of your choice, and then confirm your password once again.

    > [!NOTE]
    > Make sure you write down your username and password so that you don't forget them! You will need them later when we start uploading and deploying our code to Azure.

    ![Screenshot of the Azure portal showing the Deployment Credentials blade of the staging slot with example credentials in the required fields.](../media/7-deployment-credentials.png)

1. Click the **Save** button at the top of the **Deployment credentials** blade.

Now that the deployment credentials are created successfully, you need to configure other deployment options.

## Use a local Git repository as your deployment option

Next, we'll create a local Git repository in Azure, so you can start uploading your code.

1. Within the **staging** deployment slot Web App, click the **Deployment options** menu item on the left-hand navigation.

1. The Azure portal navigates to the **Deployment options** blade.

1. Click on the **Choose Source** to configure the required settings.

1. The Azure portal displays the available options that you can configure and use. In our case, choose the **Local Git Repository** option.

1. You will be returned to the **Deployment option** blade. Click the **OK** button at the bottom of the blade to set up the deployment source.

1. Now, navigate to the **Deployment Center (Preview)** section on the left-side navigation to view the new deployment details.

    ![Screenshot of the Azure portal showing the deployment slot's Deployment Center blade with the Git Clone Uri for the slot highlighted.](../media/7-staging-after-setting-git.png)

    The important information to note here is the **Git Clone Uri**, which is the local Git repository URL that you will use as a **remote** for your local web application repository.

It is time to start uploading your code to the staging deployment slot.

## Install Git on your machine

If you don't already have it, install Git on your Linux machine.

> [!NOTE]
> These instructions are for Ubuntu 18.04, so the steps to install Git may differ for your distribution and version. Check out the [Linux Git installation instructions](https://git-scm.com/download/linux) for the appropriate steps.

1. Open a new **Terminal** window

1. Type the following command. You will be prompted to enter your Ubuntu user password.

    ```console
    sudo apt-get update
    ```

1. Once the update is successful, type the following command to install Git locally. You will be prompted to accept the installation of Git on your machine.

    ```console
    sudo apt-get install git-core
    ```

1. To verify that Git is now installed, type the following command:

    ```console
    git --version
    ```

   If the installation is successful, you will see the following output:

    ```console
    git version 2.17.1
    ```

1. It is always a good practice to configure Git settings by providing your name and email. For that, you need to issue the following commands, replacing the `{your name}` and `{your email}` placeholders with your own name and email (without the `{}` curly braces):

    ```console
    git config --global user.name "{your name}"
    git config --global user.email "{your email}"
    ```

1. To verify that your information has been recorded by Git, type the following command:

    ```console
    cat ~/.gitconfig
    ```

   You should be seeing the following, with your name and email shown:

    ```console
    [user]
        name = {your name}
        email = {your email}
    ```

## Initialize a local Git repository for your web app

To start using Git, you need to initialize a local Git repository for the web app.

1. Open a new **Terminal** window.

1. Navigate to the content root folder of the web app you created previously. You can type the following:

    ```console
    cd ~/Documents/BestBikeApp/
    ```

1. Initialize a new Git repository by issuing the following command:

    ```console
    git init
    ```

    If the command is successful, you receive a message like the following:

    ```console
    Initialized empty Git repository in /home/{your-user}/Documents/BestBikeApp/.git/
    ```

1. Stage all the web app files to Git.

   The next step is to let Git know about your web app files. Do that by adding all the files of the working directory so that they get **staged** by Git. Type the following command:

    ```console
    git add .
    ```

    The command above adds all files, represented by the ".", to the staging state of Git.

1. Now, you need to commit your changes to Git.

   Once you stage the files with Git, you need to commit your files to the **Git commit history** on your local machine. You do that by typing the following command:

    ```console
   git commit -m "Initial create"
    ```

   The `commit` command accepts  `-m` argument to include a message with the commit you are creating. Later on, when you push your code to Azure, you will be able to see the same message stored with this particular commit.

## Add a remote for the local Git repository

At this point, you have successfully initialized a new local Git repository. In addition, you've committed all of your web app files to Git. What remains is to add a **remote** to connect your local Git repository to that hosted on Azure.

To do so, you need to:

1. Copy the **Git clone url** that you saw above.

1. Once copied, you go back to the **Terminal** window and issue the following Git command:

    ```console
    git remote add origin https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git
    ```

    The above Git command hooks your local Git repository to the one hosted on Azure. Now, you can start pushing and pulling between the local and remote Git repositories!

1. To verify the above command, type the following Git command:

    ```console
    git remote -v
    ```

    The command above generates the following output:

    ```console
    origin	https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (fetch)
    origin	https://BESTBIKE-git@BESTBIKE-staging.scm.azurewebsites.net:443/BESTBIKE.git (push)
    ```

## Push your code to Azure

Now that you have your local Git repository hooked to the remote Git repository on Azure, you will develop and build the app, and then push your web app to Azure.

1. Type the following Git command to push your **master** branch to the remote Git repository on Azure:

    ```console
    git push origin master
    ```

1. You will be prompted to enter the password that you have configured in the **Deployment credentials** section above. Enter your password and hit Enter. Git starts uploading your committed files to the Azure remote Git repository configured under the staging deployment slot.

## Verify the web app is uploaded to Azure

1. Sign in to the Azure portal.

1. Click on the **All Resources** menu item on the left-side navigation.

1. The Azure portal navigates you to the list of all resources created on Azure so far.

1. Click on the staging slot created above. Remember, a deployment slot is considered a web app, and hence, it will appear as a web app resource under **All Resources**.

1. Once you arrive to the staging deployment slot blade, go to **Deployment options**.

    You will see that your first commit that you have locally on your machine is now uploaded to the Azure portal.

    By pushing your code locally to the remote Git repository hosted on Azure, Azure has recorded this operation.

    Every time you push your code to Azure, you will see a new record, together with the message that you type when committing your changes locally on your machine.

    ![Screenshot of the Azure portal showing a recent Git repo deployment in the Development options blade.](../media/7-staging-deployment-slot-after-uploading-files.png)

1. Let's visit the **staging slot** URL. The URL was mentioned above, however, if you forget that URL, you can always go to the **Overview** page of the staging deployment slot and pick up the URL.

1. Type the following URL in your browser address bar: [https://BESTBIKE-staging.azurewebsites.net/](https://BESTBIKE-staging.azurewebsites.net/).

    ![Screenshot showing a web browser view of the staging deployment slot web site.](../media/7-staging-slot-hosted-online.png)

You have successfully uploaded your local web app files to the staging deployment slot on Azure.

## Swapping the staging and production deployment slots

Now that the application is up and running on the staging deployment slot hosted on Azure, it is time to swap this slot with the production one. To do so, follow these steps:

1. Navigate to the original Web App blade created early. You can find the original Web App from the **All resources** blade.

1. Click the **Deployment slots** menu item on the left-side navigation.

1. Click on the **Swap** button at the top of the blade.

1. The Azure portal navigates you to the **Swap** blade.

1. For the **Swap** field, select **Swap**.

1. For the **Source** field, select **Staging**.

1. For the **Destination** field, select **Production**.

    ![Screenshot of the Azure portal showing the deployment slot swap blade.](../media/7-swap-blade.png)

1. Click on the **OK** button at the bottom of the blade.

1. Azure starts the swapping process. Usually, this operation takes a few seconds, depending on the size of the web app being swapped.

1. Once the operation ends, visit the web app URL: [https://bestbike.azurewebsites.net/](https://bestbike.azurewebsites.net/).

    ![Screenshot showing a web browser view of the previously staging deployment slot now hosted as the primary web app.](../media/7-web-app-page.png)

    The swapping operation has been successful! You can now see the code that you uploaded to the staging deployment slot also being hosted on the production slot.

1. Now, visit the URL of the staging slot: [https://bestbike-staging.azurewebsites.net/](https://bestbike-staging.azurewebsites.net/).

    ![Screenshot showing a web browser view of the previously primary deployment slot now hosted as the staging deployment slot web app.](../media/7-staging-after-swapping.png)

    The staging deployment slot now contains the original, default web application files that were previously hosted under the production slot.

Congratulations! You have successfully uploaded your web app to Azure and swapped deployment slots.
