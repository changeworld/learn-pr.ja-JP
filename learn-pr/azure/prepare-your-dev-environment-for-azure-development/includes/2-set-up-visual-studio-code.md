Visual Studio Code is a popular choice for developing applications for Azure. The integrated development environment (IDE) is lightweight, taking up only megabytes of storage space, versus gigabytes for some IDEs. VS Code is cross platform; it works on Windows, Linux, and macOS. And it's flexible. You can use Visual Studio Code to deploy your apps through the Azure CLI or Azure App Service, or by using a Docker container image. You can even deploy your apps by using Azure Functions with the serverless approach. 

## Visual Studio Code

Visual Studio Code, or just VS Code, is a powerful, yet lightweight editor. It supports most programming languages, hundreds in fact, and it is designed to connect to cloud services.

With a focus on cross-platform support (runs on Windows, Linux, and macOS) and providing a portable agile experience, the base installation of VS Code contains an editor that recognizes an ever-growing coverage of programming language syntax highlighting. However, there is no compiler. Compilation is meant to take place in the cloud, or via an extension.

You get great built-in support for source control using a Git source control manager (SCM), which does mean you need to install the Git framework first.

## Extension model

One of the most powerful features of VS Code is the extension model. It allows third-party functionality to run as an integrated part of the VS Code IDE, and extend the capabilities of the IDE in almost any way imaginable.

An extension is written in either TypeScript or JavaScript, and can even be developed in VS Code itself. You can use Yeoman to scaffold an extension. All of these support IntelliSense, code navigation, and a full debugging experience.

There are three general categories of extensions for VS Code: Extensions, Language Servers, and Debugger. The latter two have additional protocols that allow them to provide specialized functionality, either across languages in the editor, or to hook into the debug experience.

The extensions available in the Extension Marketplace include language support for Python, Go, C++, and many others. The extensions also include code formatting tools, such as linters, tools for cloud connectivity such as Azure, new themes, code formatters, and snippet libraries. All of these extensions are available on the [VS Code Marketplace](https://marketplace.visualstudio.com/).

## Azure extensions

Many of the extensions target Azure features and products, with more being added all the time. They target areas such as Docker support, subscription management, tooling for the Azure CLI, database access, Azure Storage API integration, and general Azure extension.

Each Azure extension adds a set of VS Code features that makes your development with Azure integration points easier and more efficient.

## Getting VS Code and preparing for Azure development

There are three different platforms on which VS Code is supported: Windows, macOS, and Linux. While they all are installed from a downloadable file, they do differ in the setup.

To get VS Code running on Windows, download the file for your version of Windows (32-bit or 64-bit), and install like any other Windows application.

For macOS, you also download the file and expand the contents. It is recommended to add VS Code to the LaunchPad and the Dock.

Linux is a bit more complex, depending on your distribution of choice. Either you need to download and install VS Code on Debian and Ubuntu, or you need to use the Yum repository on RHEL, Fedora, CentOS, openSUSE, or SLE. For other distributions, there may be less supported community editions available as well.

To get VS Code ready for Azure development on any platform, use the Extension Marketplace to install the necessary Azure extensions. If you are working with App Service, use the App Service extension. If you work with Node.js, then you need the Node Pack for Azure.

## Summary

VS Code is a perfect companion for developing and creating applications for Azure. The lightweight, cross-platform IDE, paired with an extensive range of extensions to improve both efficiency and robustness of apps, makes VS Code ideal for Azure development.
