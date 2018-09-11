You’ve now worked with both the Azure IoT Central application and connected the coffee machine to Azure IoT Central. You are well on your way to begin to monitor and manage your remote coffee machine. In this unit, you take a moment to validate your setup and connection by using the Connected Coffee Maker template that you defined earlier. You update the optimal temperature in settings, run commands to check for the state of your machine, and view your connected coffee machine in the dashboard. 

## Update settings to sync your application with the coffee machine

On the Settings page, you send configuration data to the coffee machine from your application. 

In this scenario, change the optimal temperature and choose **Update**. 
 When the setting is changed, the setting is marked as pending in the UI until the coffee machine acknowledges that it has responded to the setting change. 

> [!NOTE]
> Successful updates in the setting indicate data flow and validate your  connection. The telemetry measurements will respond to the update in Optimal  Temperature. You can observe the change on the Measurements page. 

## Run commands on the coffee machine 
Navigate to the **Commands** page for the following exercise. To validate the commands setup, you remotely run commands on the coffee machine from IoT Central. If successful, confirmation messages are sent from the coffee machine.

1. Start Brewing remotely by choosing **Run**. 
    
    The coffee machine will start if these three conditions are satisfied:
    - Cup detected
    - Not in maintenance
    - Not brewing already  

    > [!NOTE]
    > When you've successfully started brewing, the state of the machine changes to yellow as indicated in Measurements > State. 
    
    Look for confirmation messages in the console log on the coffee machine. 

    ![Run commands](../images/4-commands-brewing.png)

1. Set Maintenance Mode by choosing **Run**. The coffee machine will set to maintenance if it's *not* already in maintenance.
    
    Look for confirmation messages in the console log on the coffee machine. 

    > [!NOTE]
    > As in real life when the technician takes the machine offline to perform necessary repairs before switching it back online, the coffee machine continues to stay in the maintenance mode until you reboot the client code.

    ![Run commands](../images/4-commands-maintenance.png)

1. It's recommended that you run the Node.js application no more than 60 minutes or so to prevent the application from sending you unwanted notifications/emails. Stopping the application when you're not working on the tutorial also prevents you from exhausting the daily message quota.

## View the coffee machine in the dashboard
Navigate to the **Dashboard** page where you can collectively see the relevant information about your coffee machine. For the following exercise, turn on **Design Mode** to configure your dashboard. Whenever you are finished, choose **Save**.

1. Choose **Line Chart** and enter the title as Telemetry to see the telemetry measurements. Choose **Past 30 minutes** for **Time Range**.

    ![Viewing the dashboard](../images/4-dashboard-a.png)

1. Choose **Settings and Properties** and enter the title as Device Properties. In **Add/Remove**, choose Coffee Makers Max Temperature, Coffee Makers Min Temperature, Device Warranty Expired. 

1. Choose **Settings and Properties** and enter the title as Optimal Temperature. In **Add/Remove**, choose Optimal  Temperature. 

## Summary

In this unit, you spent some time to validate the connection between the coffee machine and Azure IoT Central. You achieved validation by updating the optimal temperature, running the commands. Finally you set up the dashboard to monitor your machine in one place by defining the information you'd like to see about your coffee machine. These validation steps are necessary before moving on to other tasks in the next unit. 