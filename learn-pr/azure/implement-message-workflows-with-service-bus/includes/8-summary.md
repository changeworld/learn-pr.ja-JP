In this module, you created a Service Bus namespace, a queue, and a topic in your subscription by using the Azure portal, and you sent and received messages through the queue and the topic.

Service Bus queues and topics are excellent tools that you can use to increase the resilience of communications within a distributed application. By acting as a temporary storage location, they remove the requirement for direct communication between components and handle peaks in demand smoothly. Consider using them whenever you have a component that can communicate with another component in a loosely coupled configuration.

## Clean up
<!---TODO: Update for sandbox?--->

Service Bus queues and topics in your Azure subscription incur a cost, although it is likely to be small when there are few, small messages. The easiest way to clean up your Azure subscription is to remove the resource group that you created during our first exercise. Doing so will also delete all the topics, queues, namespaces, and other resources in the group. When you are finished with this module, take the following steps:

1. In the **Azure portal**, in the navigation on the left, click **Resource groups**.
1. In the list of resource groups, click **SalesTeamRG**.
1. In the **Resource group** blade, click **Delete resource group**.
1. In the **TYPE THE RESOURCE GROUP NAME** text box, type **SalesTeamAppRG**, and then click **Delete**. Azure removes the resource group and all its resources.