Visual Studio is a fully featured and rich integrated development environment (IDE), aimed at almost any kind of software professional. Visual Studio has a full set of tools and features specifically aimed at developing applications with Microsoft Azure. The tight integration in Visual Studio means that Azure deployment, debugging, and development tools are first class. Visual Studio for Mac is no different. This unit will introduce you to both, and their strengths when it comes to Azure development.

## Visual Studio

Visual Studio is a fully featured IDE used to develop applications for Windows, Android, iOS, the web, and Azure.

When installing Visual Studio, you'll see that several workloads are available. Workloads are collections of libraries and components that define an area of functionality that can be installed. Instead of installing a single individual component, where you must know and remember the dependencies between each, you can use workloads to do "themed" installations. This ensures that all necessary components are included.

The base installation of Visual Studio comes with no tools or libraries for Azure development. For that, you need to include the Azure development workload. This includes the Azure SDKs, tooling, and template projects for getting started creating applications and experiences on Azure.

To install Visual Studio, download the installer. This installer will ask which workloads to install, which is where you specify the Azure development workload. If there is additional functionality needed, this is most likely available through NuGet or a Visual Studio extension.

## Visual Studio for Mac

Visual Studio for Mac is a natively designed and developed IDE for macOS. It provides a first class developer experience for creating applications for mobile apps on Android and iOS, the web, and .NET Core solutions. It is also perfectly suited for creating applications on Azure.

The base installation of Visual Studio for Mac comes with contextual integration of Azure tooling. For example, if you are building a Xamarin app for Android, then the Connected Services workload will provide a link to create a mobile back end with Azure App Service. If you want to create an Azure function, then that is a project template under the Cloud category.

If you require tools for Azure features and functions that aren't in the base installation, NuGet is the way to go. The NuGet package manager has a ton of Azure packages that extend the functionality and tooling of Visual Studio for Mac.

To install Visual Studio for Mac, download the installer. The installer will inspect your system to determine what components are needed or need to be updated. You can customize the components to install from those that are found to be missing from your system. The base installation will include Azure tooling. If additional functionality is needed, it will likely be available through NuGet or a Visual Studio for Mac extension.

> [!NOTE]
> You may be prompted for administrator credentials on your machine to install certain components.

## Summary

In this unit, you have installed Visual Studio either on Windows or on macOS.

For Windows you chose the Azure development workload in the installer experience, which installed all the necessary tooling for building Azure applications and generating Azure resources. You can then access all the Azure resources for your subscription through Explorer tooling, or via resource referencing.

Visual Studio for Mac comes with some Azure tooling built into the base installation, and many more features available through NuGet. This will give access to resources and services on your Azure subscription as well.
