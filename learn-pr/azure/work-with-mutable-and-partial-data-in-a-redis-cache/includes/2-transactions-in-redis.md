There are times where you must guarantee that multiple operations execute together. For example, in your instant messaging application, users can send: an individual picture, an individual text message, or a picture and text message together. When the user chooses to send a picture and text message together, we must ensure that other members of the group receive them at the same time. This is important because if a picture and text message are not received together, it’s possible that a separate message could be sent in between the picture and text message, which could make the overall conversation confusing.

Here, we'll look at how to create a transaction in Redis to guarantee multiple operations are executed together.

## How to create a transaction

To create a transaction, you need to have a transaction block. This is a queue that contains the commands that will be executed together. The `MULTI` command is used to create a transaction block and all subsequent commands are queued for atomic execution.

## How to execute a transaction

To execute the commands in a transaction block, you use the `EXEC` command. This will execute all queued commands and restore the connection state to normal. If you decide you don't want to execute a transaction, you can use the `DISCARD` command, which will clear out the transaction block and set the connection state to normal.

One important thing to understand about Redis transactions is that they don't support rollbacks. This means that if a command inside a transaction fails, the remaining commands will still be executed.

## What is ServiceStack.Redis?

In the previous module, **Optimize your web application by caching read-only data with Redis** we used the **StackExchange.Redis** client. Here, we're going to try out another popular client, **ServiceStack.Redis**.

**ServiceStack.Redis** is a C# client library for interacting with a Redis cache. This means that instead of using low-level Redis commands, we can use C# classes and methods.

For example, to create a transaction, we normally must use the Redis `MULTI` command. However, using **ServiceStack.Redis** we can create a transaction using the `CreateTransaction()` method.

```csharp
var transaction = redisClient.CreateTransaction();
```

## Create a transaction using C# and the ServiceStack.Redis client

Here's an example of using **ServiceStack.Redis** to create a transaction that can send a message that includes a picture and text.

```csharp
public bool SendPictureAndText(string groupChatID, string text, string pictureURL)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create the transaction
        var transaction = redisClient.CreateTransaction();

        //Add multiple operations to the transaction
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, pictureURL));
        transaction.QueueCommand(c => c.PublishMessage(groupChatID, text));

        //Commit and get result of transaction
        var transactionResult = transaction.Commit();

        return transactionResult;
    }
}
```
Transactions ensure that multiple operations are executed together. You create a transaction using the `MULTI` command, however, using a client library like **ServiceStack.Redis**, you can use the `CreateTransaction()` method.