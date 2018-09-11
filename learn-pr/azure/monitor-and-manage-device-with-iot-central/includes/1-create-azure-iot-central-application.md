 In this tutorial, you follow the scenario in which a remote coffee machine is connected to Azure IoT Central for monitoring and management of issues. You can monitor telemetry such as water temperature and humidity, observe the state of your machine, set optimal temperature, receive warranty status, and send commands. When the water temperature of the coffee machine exceeds certain threshold values while your machine is under warranty, Microsoft Flow sends a mobile notification to a remote technician's mobile device. Likewise, if the warranty is expired when the water temperature is outside the expected range, an email from IoT Central is sent to the client’s maintenance department for further action.

To implement the scenario, you begin by creating a device template in Azure IoT Central to define measurements (telemetry and state), settings, properties, and commands. You then connect your coffee machine to Azure IoT Central, followed by configuring rules for maintenance notifications when water temperature is outside the optimal range.

In this module, you will to:
- Create an Azure IoT Central custom application 
- Create and define your device template
- Connect your coffee machine to the application
- Validate your connection and data flow
- Configure rules for maintenance notifications
 
## Sign in to Azure IoT Central
In this unit, you sign in to IoT Central to create a new custom application. A 7-days trial is sufficient to complete units 1–4. If you wish to complete the optional exercise on using Microsoft Flow to send a mobile notification in unit 5, you need to extend the IoT Central trial to 30 days. The extension is enabled if you have an Azure subscription.  

1. Navigate to the Azure IoT Central [Application Manager](https://aka.ms/iotcentral) page. 

1. On the sign in page, enter the email address and password that you use to access your Microsoft account.

## Create a new custom application

1. To create a new Azure IoT Central application, choose **New Application**. 

1. On the Create Application page: 
    * Choose **Free** for the payment plan
    * Select **Custom Application** as the application template
    * Choose a friendly application name, such as **Coffee Maker 01**
    * Azure IoT Central generates a unique URL prefix for you
    Choose **Create**
    
   > [!NOTE]
   > Extending your trial to 30 days is optional, but it is a prerequisite if you wish to complete the exercise on using Microsoft Flow to send a mobile notification in unit 5. The 30-day extension is enabled if you have an Azure subscription. For instruction on enabling the extension, see unit 5 on configuring rules and actions to monitor your coffee machine.

In this unit, you created an Azure IoT custom application. You may also have chosen to sign up for an Azure subscription. In the next unit, you will continue to build on the application framework that you created. 