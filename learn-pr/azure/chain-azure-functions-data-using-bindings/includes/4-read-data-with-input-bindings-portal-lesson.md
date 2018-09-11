In order to connect to a data source we have to configure an *input binding*. This binding will make it possible to write minimal code to create a message. You don't have to write code for tasks such as opening a storage connection. The Azure Functions runtime and binding take care of those tasks for you.

## Input binding types

There are multiple types of input, however not all types support both input and output. You'll use them anytime you want to ingest data of that type. Here, we'll look at the types that support input bindings and when to use them.

- **Blob Storage**
    The blob storage bindings allow you to read from a blob.

- **Cosmos DB**
    The Azure Cosmos DB input binding uses the SQL API to retrieve one or more Azure Cosmos DB documents and passes them to the input parameter of the function. The document ID or query parameters can be determined based on the trigger that invokes the function.

- **Microsoft Graph**
    Microsoft Graph input bindings allow you to read files from OneDrive, read data from Excel, and get auth tokens so you can interact with any Microsoft Graph API.
- **Mobile Apps**
    The Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.

- **Table storage**
    You can read data and work with Azure Table storage.

## How to create an input binding?

In order to define a binding an input, you must define the `direction` as `in`.
The parameters for each type of binding may differ, those are well documented in [Microsoft's Documentation](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings?azure-portal=true)

## What is a binding expression?

A binding expression is specialized text in function.json, function parameters, or code that is evaluated when the function is invoked to yield a value. For example, you can use a binding expression to get the current time or retrieve a value from app settings.

### Types of binding expressions

- App settings
- Trigger file name
- Trigger metadata
- JSON payloads
- New GUID
- Current date and time
- Binding expressions

Most expressions are identified by wrapping them in curly braces. However, app setting binding expressions are identified differently from other binding expressions: they are wrapped in percent signs rather than curly braces. For example if the blob output binding path is `%Environment%/newblob.txt` and the Environment app setting value is Development, a blob will be created in the Development container.

Input bindings allow us to connect our function to a data source. There are several types of data sources we can connect to and the parameters for each vary. We can use binding expressions in the function.json, function parameters or code, to resolve values from various sources.