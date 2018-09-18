データは常に永続的であるとは限りません。特定の時点で削除したい場合もあります。 たとえば、インスタント メッセージング アプリケーションでは、ユーザーはグループ チャットの表示名を設定できますが、1 時間後にこの名前をリセットするとします。 これを実現するには、1 時間のタイマーを設定し、名前を削除する独自のサーバー側ロジックを記述します。 ただし、Redis では、データの有効期限がサポートされています。これは、追加のロジックを記述しなくても、これを自動的に実行する機能です。

ここでは、データの有効期限を実装するための一般的な Redis コマンドについて説明します。

## <a name="what-is-data-expiration"></a>データの有効期限とは

データの有効期限は、一定時間の経過後に、キャッシュ内のキーと値を自動的に削除できる機能です。

## <a name="why-use-data-expiration"></a>データの有効期限を使用する理由

データの有効期限は、保存するデータの有効期間が短い場合によく使用されます。  これが重要なのは、Redis はメモリ内データベースであり、ディスクに保存する場合のように多くのメモリを使用することはできないためです。 Redis ではストレージ容量が限られているため、重要なデータのみを保存する必要があります。 データが長期間存在する必要がない場合は、有効期限を設定してください。

## <a name="how-to-use-data-expiration-in-redis"></a>Redis でデータの有効期限を使用する方法

Redis でデータの有効期限を実装して管理するためのさまざまなコマンドがあります。

- `EXPIRE`: キーのタイムアウトを秒単位で設定します。
- `PEXIRE`: キーのタイムアウトをミリ秒単位で設定します。
- `EXPIREAT`: 絶対 UNIX タイムスタンプ (秒単位) を使用してキーのタイムアウトを設定します。
- `PEXPIREAT`: 絶対 UNIX タイムスタンプ (ミリ秒単位) を使用してキーのタイムアウトを設定します。
- `TTL`: キーの有効期間の残り時間を秒単位で返します。
- `PTTL`: キーの有効期間の残り時間をミリ秒単位で返します。
- `PERSIST`: キーを無期限にします。

最も一般的なコマンドは `EXPIRE` であり、このモジュール全体を通じて使用します。

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a>C# と ServiceStack.Redis を使用したデータの有効期限の例

**ServiceStack.Redis** などのクライアント ライブラリを使用する場合は、下位の Redis コマンドを覚えておく必要はありません。 代わりに、単純な C# メソッドを使用できます。

```csharp
public static void SetGroupChatName(string groupChatID, string chatName)
{
    using (RedisClient redisClient = new RedisClient(redisConnectionString))
    {
        //Create a key for group chat display names
        string key = groupChatID + "displayName";

        //Set the group chat display name
        redisClient.Set(key, chatName);

        //Set the expiration for one hour
        redisClient.Expire(key, 3600);
    }
}
```

Redis では、データの有効期限を使用して、一定時間の経過後にデータを自動的に削除できます。 これが重要なのは、Redis はメモリ内データベースであり、データをディスクに保存する場合のように多くのメモリを使用することはできないためです。 保存するデータが長期間存在する必要がない場合は、有効期限を設定してください。