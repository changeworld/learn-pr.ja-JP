<span data-ttu-id="a6acd-101">ここでは、Azure Redis Cache でデータに有効期限を追加します。</span><span class="sxs-lookup"><span data-stu-id="a6acd-101">Here, you'll add an expiration time to our data in the Azure Redis Cache.</span></span>

## <a name="add-an-expiration-time"></a><span data-ttu-id="a6acd-102">有効期限を追加する</span><span class="sxs-lookup"><span data-stu-id="a6acd-102">Add an expiration time</span></span>

<span data-ttu-id="a6acd-103">前回の演習では、**Program.cs** に次のコードを追加したところで終わりました。</span><span class="sxs-lookup"><span data-stu-id="a6acd-103">In the last exercise, we left off with the following code in **Program.cs**.</span></span>

```csharp
using (RedisClient redisClient = new RedisClient(redisConnectionString))
{
    //Create the transaction
    var transaction = redisClient.CreateTransaction();

    //Add multiple operations to the transaction
    transaction.QueueCommand(c => c.Set("MyKey1", "MyValue1"));
    transaction.QueueCommand(c => c.Set("MyKey2", "MyValue2"));

    //Commit and get result of transaction
    var transactionResult = transaction.Commit();

    if(transactionResult)
        Console.WriteLine("Transaction committed");
    else
        Console.WriteLine("Transaction failed to commit");
}
```

<span data-ttu-id="a6acd-104">**MyKey1** と **MyKey2** の両方に 15 秒の有効期限を追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="a6acd-104">Let’s add an expiration of 15 seconds to both **MyKey1** and **MyKey2**.</span></span>

1. <span data-ttu-id="a6acd-105">トランザクションをコミットする前に次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="a6acd-105">Add the following code before you commit the transaction</span></span>

    ```csharp
    //Add an expiration time
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey1", 15));
    transaction.QueueCommand(c => ((RedisNativeClient)c).Expire("MyKey2", 15));
    ```

    <span data-ttu-id="a6acd-106">このコードでは、**Expire** メソッドは **RedisNativeClient** の一部です。</span><span class="sxs-lookup"><span data-stu-id="a6acd-106">In this code, the **Expire** method is a part of the **RedisNativeClient**.</span></span> <span data-ttu-id="a6acd-107">このメソッドにアクセスするには、まずオブジェクトをキャストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6acd-107">To access the method, we must first cast our object.</span></span>

## <a name="verify-the-expiration"></a><span data-ttu-id="a6acd-108">有効期限を確認する</span><span class="sxs-lookup"><span data-stu-id="a6acd-108">Verify the expiration</span></span>

<span data-ttu-id="a6acd-109">データを期限切れにするコードを追加したので、プログラムを実行し、Redis からデータが 削除されていることを確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="a6acd-109">Now that we added the code to expire our data, lets run the program and check that the data is removed from Redis.</span></span>

1. <span data-ttu-id="a6acd-110">プログラムを実行します。</span><span class="sxs-lookup"><span data-stu-id="a6acd-110">Run the program.</span></span>

    ```bash
    dotnet run
    ```
    
1. <span data-ttu-id="a6acd-111">Azure portal の Azure Redis コンソールに戻ります。</span><span class="sxs-lookup"><span data-stu-id="a6acd-111">Switch back to the Azure Redis Console in the Azure portal.</span></span>

1. <span data-ttu-id="a6acd-112">データがまだ存在することを確認するために、次のコマンドを発行します。</span><span class="sxs-lookup"><span data-stu-id="a6acd-112">To verify that the data is still there, issue the following command:</span></span>

    ```
    get MyKey1
    ```

1. <span data-ttu-id="a6acd-113">15 秒後に、コマンドをもう一度発行します。</span><span class="sxs-lookup"><span data-stu-id="a6acd-113">After 15 seconds, issue the command again.</span></span> <span data-ttu-id="a6acd-114">データがなくなっていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="a6acd-114">You should see that the data is no longer there.</span></span>

    ![MyKey1 の値が nil であることを示す Azure Redis コンソールのスクリーンショット。](../media/6-redis-console-data-expiration.png)