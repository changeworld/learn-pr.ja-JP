<span data-ttu-id="a6a57-101">データは常に永続的であるとは限りません。特定の時点で削除したい場合もあります。</span><span class="sxs-lookup"><span data-stu-id="a6a57-101">Data is not always permanent, and there are times you would like to delete it at a specific time.</span></span> <span data-ttu-id="a6a57-102">たとえば、インスタント メッセージング アプリケーションでは、ユーザーはグループ チャットの表示名を設定できます。</span><span class="sxs-lookup"><span data-stu-id="a6a57-102">For example, in your instant messaging application, users can set the display name of the group chat.</span></span> <span data-ttu-id="a6a57-103">ただし、1 時間後にこの名前をリセットする必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6a57-103">However, you want the name to reset after one hour.</span></span> <span data-ttu-id="a6a57-104">これを実現するには、1 時間のタイマーを設定し、名前を削除する独自のサーバー側ロジックを記述します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-104">You could accomplish this by writing your own server-side logic that set an hour timer and deleted the name.</span></span> <span data-ttu-id="a6a57-105">ただし、Azure Redis Cache では、データの有効期限がサポートされています。これは、追加のロジックを記述しなくても、これを自動的に実行する機能です。</span><span class="sxs-lookup"><span data-stu-id="a6a57-105">However, Azure Redis Cache supports data expiration, which is a feature that does this automatically without writing additional logic.</span></span>

<span data-ttu-id="a6a57-106">ここでは、データの有効期限を実装するための一般的な Redis コマンドについて説明します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-106">Here, you'll learn about the common Redis commands to implement data expiration.</span></span>

## <a name="what-is-data-expiration"></a><span data-ttu-id="a6a57-107">データの有効期限とは</span><span class="sxs-lookup"><span data-stu-id="a6a57-107">What is data expiration?</span></span>

<span data-ttu-id="a6a57-108">データの有効期限は、一定時間の経過後に、キャッシュ内のキーと値を自動的に削除できる機能です。</span><span class="sxs-lookup"><span data-stu-id="a6a57-108">Data expiration is a feature that can automatically delete a key and value in the cache after a set amount of time.</span></span>

## <a name="why-use-data-expiration"></a><span data-ttu-id="a6a57-109">データの有効期限を使用する理由</span><span class="sxs-lookup"><span data-stu-id="a6a57-109">Why use data expiration?</span></span>

<span data-ttu-id="a6a57-110">データの有効期限は、保存するデータの有効期間が短い場合によく使用されます。</span><span class="sxs-lookup"><span data-stu-id="a6a57-110">Data expiration is commonly used in situations where the data you're storing is short-lived.</span></span>  <span data-ttu-id="a6a57-111">これが重要なのは、Azure Redis Cache はメモリ内データベースであり、ディスクに保存する場合のように多くのメモリを使用することができないためです。</span><span class="sxs-lookup"><span data-stu-id="a6a57-111">This is important because Azure Redis Cache is an in-memory database and you don't have as much memory available to use as you would if you were storing on disk.</span></span> <span data-ttu-id="a6a57-112">Azure Redis Cache ではストレージ容量が限られているため、重要なデータのみを保存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="a6a57-112">Since you have limited storage with Azure Redis Cache, you want to make sure you're only storing data that is important.</span></span> <span data-ttu-id="a6a57-113">データが長期間存在する必要がない場合は、有効期限を設定してください。</span><span class="sxs-lookup"><span data-stu-id="a6a57-113">If the data doesn't need to be around for a long time, make sure you set an expiration.</span></span>

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a><span data-ttu-id="a6a57-114">Azure Redis Cache でデータの有効期限を使用する方法</span><span class="sxs-lookup"><span data-stu-id="a6a57-114">How to use data expiration in Azure Redis Cache</span></span>

<span data-ttu-id="a6a57-115">Azure Redis Cache でデータの有効期限を実装して管理するためのさまざまなコマンドがあります。</span><span class="sxs-lookup"><span data-stu-id="a6a57-115">There are different commands to implement and manage data expiration in Azure Redis Cache:</span></span>

- <span data-ttu-id="a6a57-116">`EXPIRE`: キーのタイムアウトを秒単位で設定します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-116">`EXPIRE`: Sets the timeout of a key in seconds</span></span>
- <span data-ttu-id="a6a57-117">`PEXPIRE`: キーのタイムアウトをミリ秒単位で設定します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-117">`PEXPIRE`: Sets the timeout of a key in milliseconds</span></span>
- <span data-ttu-id="a6a57-118">`EXPIREAT`: 絶対 UNIX タイムスタンプ (秒単位) を使用してキーのタイムアウトを設定します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-118">`EXPIREAT`: Sets the timeout of a key using an absolute Unix timestamp in seconds</span></span>
- <span data-ttu-id="a6a57-119">`PEXPIREAT`: 絶対 UNIX タイムスタンプ (ミリ秒単位) を使用してキーのタイムアウトを設定します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-119">`PEXPIREAT`: Sets the timeout of a key using an absolute Unix timestamp in milliseconds</span></span>
- <span data-ttu-id="a6a57-120">`TTL`: キーの有効期間の残り時間を秒単位で返します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-120">`TTL`: Returns the remaining time a key has to live in seconds</span></span>
- <span data-ttu-id="a6a57-121">`PTTL`: キーの有効期間の残り時間をミリ秒単位で返します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-121">`PTTL`: Returns the remaining time a key has to live in milliseconds</span></span>
- <span data-ttu-id="a6a57-122">`PERSIST`: キーを無期限にします。</span><span class="sxs-lookup"><span data-stu-id="a6a57-122">`PERSIST`: Makes a key never expire</span></span>

<span data-ttu-id="a6a57-123">最も一般的なコマンドは `EXPIRE` であり、このモジュール全体を通じて使用します。</span><span class="sxs-lookup"><span data-stu-id="a6a57-123">The most common command is `EXPIRE`, and we'll use it throughout this module.</span></span>

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a><span data-ttu-id="a6a57-124">C# と ServiceStack.Redis を使用したデータの有効期限の例</span><span class="sxs-lookup"><span data-stu-id="a6a57-124">Example of data expiration using C# and ServiceStack.Redis</span></span>

<span data-ttu-id="a6a57-125">**ServiceStack.Redis** などのクライアント ライブラリを使用する場合は、下位の Azure Redis Cache コマンドを覚えておく必要はありません。</span><span class="sxs-lookup"><span data-stu-id="a6a57-125">Remember, when using a client library like **ServiceStack.Redis**, we don't have to worry about remembering the low-level Azure Redis Cache commands.</span></span> <span data-ttu-id="a6a57-126">代わりに、単純な C# メソッドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="a6a57-126">Instead, we can use simple C# methods.</span></span>

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.SetValue(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

<span data-ttu-id="a6a57-127">Azure Redis Cache では、データの有効期限を使用して、一定時間の経過後にデータを自動的に削除できます。</span><span class="sxs-lookup"><span data-stu-id="a6a57-127">Azure Redis Cache allows you to delete data automatically after a set amount of time using data expiration.</span></span> <span data-ttu-id="a6a57-128">これが重要なのは、Azure Redis Cache はメモリ内データベースであり、データをディスクに保存する場合のように多くのメモリを使用することができないためです。</span><span class="sxs-lookup"><span data-stu-id="a6a57-128">This is important because Azure Redis Cache is an in-memory database, and you don't have as much memory available as you would with storing data on disk.</span></span> <span data-ttu-id="a6a57-129">保存するデータが長期間存在する必要がない場合は、有効期限を設定してください。</span><span class="sxs-lookup"><span data-stu-id="a6a57-129">If the data you're storing doesn't need to be around for a long time, make sure you set an expiration.</span></span>