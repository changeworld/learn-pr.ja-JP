You have connected your coffee machine to the Azure IoT Central application, enabling the exchange of data that allows you to monitor and manage your coffee machine. In this unit, you create rules that trigger actions when the water temperature of the coffee machine is outside the normal range. The actions are either emails or mobile notifications depending on whether or not the machine is under warranty. To add Microsoft Flow as an action, you need an Azure subscription. If you do not have an Azure subscription, adding Microsoft Flow as an action is optional.

## Create rules in IoT Central with Email as the action
Azure IoT Central has its native email capabilities to send notifications. In this scenario, if the coffee machine is outside the optimal temperature range and is not protected by the warranty, an email is sent by IoT Central to the client’s maintenance department.

Navigate to the **Rules** page for the exercises in this unit. Select **+ New Rule**, then **Telemetry**. Add the following two rules when the coffee machine warranty has expired and the water temperature is outside the optimal range. When you're finished, choose **Save**. 

> [!NOTE]
> When conditions are applied, all statements have to be true for the rules to be executed. If your condition is an "or" statement as in this scenario (e.g. the optimal temperature is less or greater than the predefined values while the warranty has expired), split the statement into two rules as shown here.

1. Name the rule: Coffee Maker Water Too Cold (Expired)

    Add the conditions:      
    * Device Warranty Expired equals 1
    * Water Temperature is less than Coffee Makers Min Temperature

    ![Using Rule](../images/5-flow-a.png)

1. Name the rule: Coffee Maker Water Too Hot (Expired)

    Add the conditions:      
    * Device Warranty Expired equals 1
    * Water Temperature is greater than Coffee Makers Max Temperature

1. To add an **Action**, scroll down on the Configure Telemetry Rule panel and choose **+** next to Actions, then choose **Email**.

1. To define the action, add the email address that you used to sign in to the IoT Central application. Add the notification message when the water temperature is too hot: "Coffee maker's water is too hot. Maintenance is required.  Warranty has expired." Repeat the same steps for when the water temperature is too cold. Add the message: "Coffee maker's water is too cold. Maintenance is required.  Warranty has expired."

1. Choose **Save**. Your rule is listed on the Rules page.

1. To trigger the rule, set the optimal temperature in Settings outside the range that you specified under Properties. Once you are done with the validation, turn off the rules to avoid flooding your Inbox with emails. 

## Create rules in IoT Central with Microsoft Flow as the action

Microsoft Flow automates workflows across many applications. It’s one of the actions that can be triggered when a rule is fired in IoT Central. In this scenario, Microsoft Flow sends a mobile notification to a local technician when the coffee machine reaches certain temperature threshold and is under warranty. Navigate to **Rules** to configure conditions and add a Flow as an action when the rule is fired. 
 
> [!NOTE]
> This exercise is optional if you do not have an Azure subscription to turn on Microsoft Flow.


### Extend your IoT Central trial to 30 days

1. To turn on Microsoft Flow, you need to extend your trial to 30 days. To do so, select **Extend Trial to 30 days** on the Billing page, choose an Azure Active Directory and Azure subscription. An Azure subscription enables you to create instances of Azure services. Azure IoT Central automatically finds all the Azure Subscriptions you have access to, and displays them in the drop-down.
    
1. If you don’t have an Azure subscription, you can create one on the [Azure sign-up page](https://aka.ms/createazuresubscription). After you create the Azure subscription, navigate back to the **Application Manager** page. Your new subscription appears in the **Azure Subscription** drop-down.
        

### Add the following rules when the coffee machine is under warranty. 

1. Name the rule: Coffee Maker Water Too Cold (Warranty)

    Add the conditions:      
    * Device Warranty Expired equals 0
    * Water Temperature is less than Coffee Makers Min Temperature

1. Name the rule: Coffee Maker Water Too Hot (Warranty)

    Add the conditions:      
    * Device Warranty Expired equals 0
    * Water Temperature is greater than Coffee Makers Max Temperature

1. After you save the rule conditions, choose Microsoft Flow as a new action when the coffee maker is under warranty. A new tab or window should open in your browser, taking you to Microsoft Flow. You land on an overview page showing an IoT Central connector connecting to a custom action. Choose **Continue**. 

    You are taken to the Microsoft Flow designer to build your workflow. The workflow has an IoT Central trigger that has your Application and Rule already filled in.

    At this point, you can add any action you want to your workflow. As an example, let's send a mobile notification. Search for notification, and choose Notifications - Send me a mobile notification.

    In the action, fill in the Text field with what you want your notification to say. You can include Dynamic content from your IoT Central rule, passing along important information such as device ID and name.
    
    ![Using Microsoft Flow as an action](../images/5-flow-b.png)

1. Once you've set up the workflow in Microsoft Flow, download the [Flow app](https://www.microsoft.com/en-us/p/microsoft-flow/9nkn0p5l9n84?activetab=pivot%3aoverviewtab) from Microsoft Store to your mobile device. Sign in using the same account that you used to set up the flow in the Flow web app. For testing purposes, set the optimal temperature out of range to trigger the rule. 

    > [!NOTE]
    > Device Property: Device Warranty Expired (1 for expired or 0 for under warranty in the device code) is randomly generated by the device and then sent by the device to the Azure IoT Central application. For testing purposes, if you'd like to control Device Warranty Expired (1 or 0), reboot your coffee machine until you receive the intended warranty state to trigger the action that you're testing. To receive a mobile notification, reboot your coffee machine until you see that Device Warranty Expired is 0 in the console log. 

    After several minutes, notifications appear in the Flow mobile app.

    ![Using Microsoft Flow as an action](../images/5-flow-c.png)

## Summary
You’ve learned to create rules in IoT Central and triggered actions such as an email or a mobile notification through Microsoft Flow when the rule is fired. In this case, when the water temperature of the coffee machine is out of the optimal range, notifications are sent to either a repair technician or the client depending on the status of the warranty. 


