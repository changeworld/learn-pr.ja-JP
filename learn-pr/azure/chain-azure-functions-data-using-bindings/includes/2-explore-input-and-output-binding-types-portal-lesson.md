Accessing and processing data are key tasks in many software solutions. Consider some of these scenarios:

* You've been asked to implement a way to move incoming data from blob storage to Azure Cosmos DB.
* You want to post incoming messages to a queue for processing by another component in your enterprise.
* Your service needs to grab gamer scores from a queue and update an online scoreboard.

All of these examples are about moving data. The data source and destinations differ from scenario to scenario, but the pattern is similar. You connect to a data source, you read and write data. Azure Functions helps you integrate with data and services using bindings. 

## What is a binding?

In functions, bindings provide a declarative way to connect to data from within your code. They make it easier to integrate with data streams consistently in a function. A trigger defines how a function is invoked. You can only have one trigger, but you can have multiple bindings in a function. This is powerful because you can connect to your data sources without having to hard code the values, like for instance, the *connection string*.

### Two kinds of bindings

There are two kinds of bindings you can use for your functions:

1. **Input binding**
    An input binding is a connection to a data **source**. Our function reads data from this source.

1. **Output binding**
    An output binding is a connection to a data **destination**. Our function writes data to this destination.

### Types of supported bindings

The *type* of binding defines where we are reading or sending data. There is a binding to respond to web requests and a large selection of bindings to interact with storage.

Here's a list of commonly used bindings:
- Blob
- Queue
- CosmosDB
- Event Hubs
- External Files
- External Tables
- HTTP

### Binding properties

There are four properties that are required in all bindings. You may have to supply additional properties based on the type of binding and/or storage you are using.

- **Name**
    The `name` property defines the function parameter through which you access the data. For example, in a queue input binding, this is the name of the function parameter that receives the queue message content. 

- **Type**
    The `type` property identifies the type of binding, i.e., the type of data or service we want to interact with.

- **Direction**
    The `direction` property identifies the direction data is flowing ie. is it an input or output binding?

- **Connection**
    The `connection` property is the name of an app setting that contains the connection string, not the connection string itself. Bindings use connection strings stored in app settings to enforce the best practice that function.json does not contain service secrets.

## Create a binding

Bindings are defined in JSON. A binding is configured in your function's configuration file, which is named `function.json` and lives in the same folder as your function code.

 Let's examine a sample of an *input binding*:

```json
    ...
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
    ...
```

To create this binding we:

1. Create a binding in our `function.json` file.

1. Provide the value for the `name` variable. In this example, the variable will hold the blob data.

1. Provide the storage `type`, in the preceding example, we are using Blob storage.

1. Provide the `path`, which specifies the container and the item name which goes in it. The `path` property is required for Blobs.

1. Provide the `connection` string setting name defined in the application's settings file. It's used as a lookup to find the connection string to connect to your storage.

1. Define the `direction` as `in`, it will read data from the Blob.

Bindings are used to connect to data into your function. In the example we looked at, we used an input binding to connect user images to be processed by our function as thumbnails.