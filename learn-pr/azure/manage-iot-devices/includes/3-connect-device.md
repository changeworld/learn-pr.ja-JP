Capturing of weather data is an important task as weather can effect everything
from traffic patterns to how HVAC systems in retail stores are operated. In this
exercise, you will be harnessing the online Raspberry Pi simulator to capture
simulated weather data and capture said data via the Azure IoT Hub.

While this exercise is being conducted in a simulated environment, the
application running on the simulated device can be transferred to a real device
in future.

## Create an IoT hub

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Select **Create a resource** \> **Internet of Things** \> **IoT Hub**.

![Screenshot of Azure portal navigation to IoT Hub](../media-draft/fa40d1bc51bc4490f657e3c1a8371b5b.png)

1. In the **IoT hub** pane, enter the following information for your IoT hub:

 - **Subscription**: Choose the subscription that you want to use to create this IoT hub.
 - **Resource group**: Create a resource group to host the IoT hub or use an existing one. For more information, see [Use resource groups to manage your Azure resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).
 - **Region**: Select the closest location to you.
 - **Name**: Create a name for your IoT hub. If the name you enter is available, a green check mark appears.

> [!IMPORTANT]
> The IoT hub will be publicly discoverable as a DNS endpoint, so make sure to avoid any sensitive information while naming it.

   ![IoT Hub basics window](./../media-draft/dbb7319388673b8ee0e0b407536156c0.png)

1.  Select **Next: Size and scale** to continue creating your IoT hub.

1.  Choose your **Pricing and scale tier**. For this article, select the **F1 - Free** tier if it's still available on your subscription. For more information, see the [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).

   ![IoT Hub size and scale window](../media-draft/b506eb3293fa4aa9d4785ad498fc476c.png)

1.  Select **Review + create**.

1.  Review your IoT hub information, then click **Create**. Your IoT hub might take a few minutes to create. You can monitor the progress in the **Notifications** pane.

Now that you have created an IoT hub, locate the important information that you use to connect devices and applications to your IoT hub.

In your IoT hub navigation menu, open **Shared access policies**. Select the **iothubowner** policy, and then copy the **Connection string---primary key** of your IoT hub. For more information, see [Control access to IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> You do not need this iothubowner connection string for this set-up tutorial. However, you may need it for some of the tutorials or different IoT scenarios after you complete this set-up.

![Get your IoT hub connection string](../media-draft/a4b41e6ea46ccbef653c411a9829610c.png)

## Register a device in the IoT hub for your device
------------------------------------------------

1.  In your IoT hub navigation menu, open **IoT devices**, then click **Add** to register a device in your IoT hub.

   ![Add a device in the IoT Devices of your IoT hub](../media-draft/ee5f177abcf06b86dd007fce3b8448ad.png)

1.  Enter a **Device ID** for the new device. Device IDs are case sensitive.

> [!IMPORTANT]
> The device ID may be visible in the logs collected for customer support and troubleshooting, so make sure to avoid any sensitive information while naming it.

1.  Click **Save**.

1.  After the device is created, open the device from the list in the **IoT
    devices** pane.

1.  Copy the **Connection string---primary key** to use later.

   ![Get the device connection string](../media-draft/fba4413dcb652be92a6ab0f6bb638561.png)

## Run a sample application on Pi web simulator

1. In coding area, make sure you are working on the default sample application. Replace the placeholder in Line 15 with the Azure IoT hub device connection string.

    ![Replace the device connection string](../media-draft/92ea2c31d42f5b939fb5512e7220e957.png)

2.  Click **Run** or type npm start to run the application.

You should see the following output that shows the sensor data and the messages
that are sent to your IoT hub

   ![Output - sensor data sent from Raspberry Pi to your IoT hub](../media-draft/96b28d30e317b04347abb0d613738117.png)

<!--Reference links
https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started-->
