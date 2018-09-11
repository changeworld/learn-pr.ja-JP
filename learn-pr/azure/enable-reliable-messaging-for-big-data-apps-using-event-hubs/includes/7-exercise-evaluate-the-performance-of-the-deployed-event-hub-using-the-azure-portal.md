In this unit, you'll use the Azure portal to verify your event hub is working and performing according to the desired expectations. You'll also test how event hub messaging works when it's temporarily unavailable and use Event Hubs metrics to check the performance of your event hub.

## View event hub activity

1. Sign in to the [Azure portal](https://portal.azure.com?azure-portal=true).

1. Find your event hub, using the Search bar, and open it.

1. On the Overview page, view the message counts.

    ![View Event Hub messages](../media-draft/6-view-messages.png)

1. The SimpleSend and EventProcessorSample applications are configured to send/receive 100 messages. You'll see that the event hub has processed 100 messages from the SimpleSend application and has transmitted 100 messages to the EventProcessorSample application.

## Test event hub resilience

Use the following steps to see what happens when an application sends messages to an event hub while it's temporarily unavailable.

1. Resend messages to the event hub using the SimpleSend application. Use the following command:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. When you see **Send Complete...**, press ENTER.

1. In the Azure portal, click **Event Hubs Instance** > **SETTINGS** > **Properties**.

1. Under Event Hub state, click **Disabled**.

    ![Disable Event Hub](../media-draft/7-disable-event-hub.png)

Wait for a minimum of five minutes.

1. Click **Active** under Event Hub state to re-enable your event hub and save your changes.

1. Rerun the EventProcessorSample application to receive messages. Use the following command.

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. When messages stop being displayed to the console, press ENTER.

1. In the Azure portal, find your event hub **_namespace_** and open it. 

1. Click **Event Hubs Namespace** > **MONITORING** > **Metrics (preview)**.

    ![Use Event Hub Metrics](../media-draft/7-event-hub-metrics.png)

1. From the **Metric** list, select **Incoming Messages** and click **Add Metric**.

1. From the **Metric** list, select **Outgoing Messages** and click **Add Metric**.

1. At the top of the chart, click **Last 24 hours (Automatic)** to change the time period to **Last 30 minutes**.

1. Click **Apply**.

You'll see that though the messages were sent before the event hub was taken offline for a period, all 100 messages were successfully transmitted.

## Summary

In this unit, you used the Event Hubs metrics to test that your event hub is successfully processing the sending and receiving messages.
