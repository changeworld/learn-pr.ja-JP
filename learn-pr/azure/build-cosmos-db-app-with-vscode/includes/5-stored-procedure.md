Multiple documents in your database frequently need to be updated at the same time. This unit discusses how to create, register, and run stored procedures from your .NET console application.

## Create a stored procedure in your app

In this stored procedure, the OrderId, which contains a list of all the items in the order, is used to calculate an order total. The order total is calculated from the sum of the items in the order, less any dividends (credits) the customer has, and takes any coupon codes into account.

1. In Visual Studio Code, in the Azure tab, expand the **learning-module (SQL)** > **Users** > **WebCustomers** and then right-click **Stored Procedures** and then click **Create Stored Procedure**.

1. In the text box at the top of the screen, type *UpdateOrderTotal* and click Enter to give the stored procedure a name.

1. Expand **Stored Procedures** and click **UpdateOrderTotal**.

1. By default, a stored procedure that retrieves the first item is provided.

1. To run this stored procedure from your application, add the following code to the Program.cs file.

    ```csharp
    public async Task RunStoredProcedure(string databaseName, string collectionName, User user)
    {
        try
        {
            await client.ExecuteStoredProcedureAsync<string>(UriFactory.CreateStoredProcedureUri(databaseName, collectionName, "sample"), new RequestOptions { PartitionKey = new PartitionKey(user.UserId) });
            Console.WriteLine("Stored procedure complete");
        }
        catch (DocumentClientException de)
        {
            throw;
        }
    }
    ```
    <!--TODO: Update sproc to take order total and check for available dividend, and use of summer coupon code, and provide updated total-->

1. Now copy the following code and paste it into the end of the **BasicOperations** method.

    ```
    await this.RunStoredProcedure("Users", "WebCustomers", yanhe);
    ```

1. In the integrated terminal, run the following command to run the sample with the stored procedure.

    ```
    dotnet run
    ```
    The console displays output indicating that the stored procedure was completed.

## Clean up
<!---TODO: Update for sandbox?--->

If you plan to continue working on the modules in this learning path, skip the clean-up process, or else use the following steps to delete your resources to avoid incurring charges for use of the service.

1. In the Azure portal, select **Resource groups** on the far left, and then select the resource group you created.  

    If the left menu is collapsed, click ![Expand button](../media/5-javascript-programming/expand.png) to expand it.

   ![Metrics in the Azure portal](../media/5-javascript-programming/delete-resources-select.png)

1. In the new window select the resource group, and then click **Delete resource group**.

   ![Metrics in the Azure portal](../media/5-javascript-programming/delete-resources.png)

1. In the new window, type the name of the resource group to delete, and then click **Delete**.

## Summary

In this module you've created a .NET Core console application that creates, updates, and deletes user records, queries the users by using SQL and LINQ, and runs a stored procedure to query items in the database.