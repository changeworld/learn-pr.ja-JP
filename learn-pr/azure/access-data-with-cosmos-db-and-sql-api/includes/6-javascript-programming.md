Multiple documents in your database frequently need to be updated at the same time. 

For your online retail application, when a user places an order and wants to use a coupon code, a credit, or a dividend (or all three at once), you need to query their account for those options, make updates to their account indicating they used them, update the order total, and process the order.

All of these actions need to happen at the same time, within a single transaction. If the user chooses to cancel the order, you want to roll back the changes and not modify their account information, so that their coupon codes, credits, and dividends are available for their next purchase.

The way to perform these transactions in Azure Cosmos DB is by using stored procedures and user-defined functions (UDFs). Stored procedures are the only way to ensure ACID (Atomicity, Consistency, Isolation, Durability) transactions because they are run on the server, and are thus referred to as server-side programming. UDFs are also stored on the server and are used during queries to perform computational logic on values or documents within the query. 

In this module, you'll learn about stored procedures and UDFs, and then run some in the portal.

## Stored procedure basics

Stored procedures perform complex transactions on documents and properties. Stored procedures are written in JavaScript and are stored in a collection on Azure Cosmos DB. By performing the stored procedures on the database engine and close to the data, you can improve performance over client-side programming.

Stored procedures are the only way to achieve atomic transactions within Azure Cosmos DB; the client-side SDKs do not support transactions.

Performing batch operations in stored procedures is also recommended because of the reduced need to create separate transactions.

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## Stored procedure example

The following sample is a simple HelloWorld stored procedure that gets the current context and sends a response that displays "Hello, World". Note that the stored procedure has an ID value, just like Azure Cosmos DB documents.

```java
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

## User-defined function basics

UDFs are used to extend the Azure Cosmos DB SQL query language grammar and implement custom business logic, such as calculations on properties and documents. UDFs can be called only from inside queries and, unlike stored procedures, they do not have access to the context object, so they cannot read or write documents.

In an online commerce scenario, a UDF could be used to determine the sales tax to apply to an order total or a percentage discount to apply to products or orders.

## User-defined function example

The following sample creates a UDF to calculate discounts based on an order total, and then it returns the modified order total based on the discount:

```java
var discountUdf = {
    id: "discount",
    serverScript: function discount(orderTotal) {

        if(orderTotal == undefined) 
            throw 'no input';

        if (orderTotal < 50) 
            return orderTotal * 0.9;
        else if (orderTotal < 100) 
            return orderTotal * 0.8;
        else
            return orderTotal * 0.7;
    }
}
```

## Create a stored procedure in the portal

Let's create a new stored procedure in the portal. The portal automatically populates a simple stored procedure that retrieves the first item in the collection, so we'll run this stored procedure first.

1. In the Data Explorer, click **New Stored Procedure**.

    Data Explorer displays a new tab with a sample stored procedure.

  <!--TODO: Insert animated .gif of creating the stored procedure.-->

2. In the **Stored Procedure Id** box, enter the name *sample*, click **Save**, and then click **Execute**.


3. In the **Input parameters** box, type the name of a partition key, *33218896*, and then click **Execute**. Note that stored procedures work within a single partition.

    ![Run a stored procedure in the portal](../media/6-stored-procedure.gif)

    The **Result** pane displays the feed from the first document in the collection.

## Create a stored procedure that creates documents

Now, let's create a stored procedure that creates documents.

1. In the Data Explorer, click **New Stored Procedure**. Name this stored procedure *createDocuments*, click **Save**, and then click **Execute**.

    ```java
    var createDocumentStoredProc = {
        id: "createMyDocument",
        productid: "5"
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();
    
            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }
    ```

<!--TODO: Need to fix code above.-->

2. Enter a partition key value of *3*, and then click **Execute**.

    Data Explorer displays the newly created document. 

## Create a user-defined function

Now, let's create a UDF in Data Explorer.

In the Data Explorer, click **New UDF**. Copy the following code into the window, name the UDF *tax*, and then click **Save**. There's no way to run the UDF from the portal, but we'll use it in a later module.

```java
function userDefinedFunction(){
    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }
}
```

