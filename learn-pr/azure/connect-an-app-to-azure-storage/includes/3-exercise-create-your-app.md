Recall that we are working on a photo-sharing application that will use Azure Storage to manage pictures and other bits of data we store on behalf of our users.

::: zone pivot="csharp"

To simplify our scenario so that we can focus on the Storage APIs, we will create a new .NET Core Console application. We will also assume it always has network connectivity. However, you should always harden your app to ensure network failures will not impact the user experience, or result in a failure of the application itself.

## Create a .NET Core application

.NET Core is a cross-platform version of .NET that runs on macOS, Windows, and Linux. You can install the tools locally, or use the Cloud Shell on the right side of the window to execute the below steps. 

1. Sign into the Cloud Shell or open a command line session and create a new .NET Core Console application with the name "PhotoSharingApp". You can add the `-o` or `--output` flag to create the app in a specific folder.

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. Run the app to make sure it builds and executes correctly. It should display "Hello, World!" to the console.

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

To simplify our scenario so that we can focus on the Storage APIs, we will create a new Node.js application that can run from the console. We will also assume it always has network connectivity. However, you should always harden your app to ensure network failures will not impact the user experience, or result in a failure of the application itself.

## Create a Node.js application

Node.js is a popular framework for running JavaScript apps. It is most commonly used for web apps, but you can use it to run logic from the command line as well. If you have the tools installed locally, you can run the following steps from a command line. Alternatively, you can use the Cloud Shell on the right side of the window to execute the below steps.

1. Sign into the Cloud Shell or open a command line session and create a new folder named "PhotoSharingApp".

    ```bash
    mkdir PhotoSharingApp
    ```

1. Change into the new folder and create a **package.json** file with the Node Package Manager (NPM) that will describe our new app.
    - Name it "PhotoSharingApp".
    - You can take defaults for all the other prompts.

    ```bash
    cd PhotoSharingApp
    npm init
    ```

1. Create a new source file **index.js** which will be where our code will go.

    ```bash
    touch index.js
    ```

1. Open the **index.js** file with an editor. If you are using the Cloud Shell, you can type `code .` to open an editor.

1. Put the following program into the **index.js** file.

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. Save the file - you can use the "..." menu on the top right corner of the Cloud Shell editor.

1. Run the app to make sure it executes correctly. It should display "Hello, World!" to the console.

    ```bash
    node index.js
    ```

::: zone-end