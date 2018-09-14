データは常に永続的でないと、特定の時点で削除するには時間があります。 たとえば、インスタント メッセージング アプリケーションでユーザーがグループ チャットの表示名を設定できます。 ただし、1 時間後にリセットする名前が必要です。 時間のタイマーを設定し、名前を削除した独自のサーバー側ロジックを記述することで、これを行う可能性があります。 ただし、Azure Redis Cache では、追加のロジックを記述することがなく自動的にはこの機能は、データの有効期限をサポートしています。

ここでは、データの有効期限を実装する一般的な Redis コマンドについて説明します。

## <a name="what-is-data-expiration"></a>データの有効期限とは何ですか。

データの有効期限は、自動的に一定の時間後に、キーとキャッシュ内の値を削除する機能です。

## <a name="why-use-data-expiration"></a>データの有効期限を使用する理由

データの有効期限は、状況を格納するデータが短時間で通常使用されます。  これは、機能は、Azure Redis Cache がメモリ内のデータベースであり、ディスクに格納していた場合と同様に使用する使用可能な量のメモリがないため重要です。 重要なデータのみを格納するかどうかを確認するため、Azure Redis Cache を記憶域が制限されることです。 データが長時間される必要がある場合、有効期限を設定することを確認します。

## <a name="how-to-use-data-expiration-in-azure-redis-cache"></a>Azure Redis Cache でデータの有効期限を使用する方法

実装し、Azure Redis Cache でデータの有効期限を管理する別のコマンドがあります。

- `EXPIRE`: キーのタイムアウトを秒単位で設定します。
- `PEXIRE`: キーのタイムアウトをミリ秒単位で設定します。
- `EXPIREAT`: 秒で絶対の Unix のタイムスタンプを使用して、キーのタイムアウトを設定します。
- `PEXPIREAT`: ミリ秒単位で絶対の Unix のタイムスタンプを使用してキーのタイムアウトを設定します。
- `TTL`: キーは、秒の有効期限が残りの時間を返します
- `PTTL`: キーは、ミリ秒単位で有効期限が残りの時間を返します
- `PERSIST`: 有効期限なしのキーは、します。

最も一般的なコマンドは`EXPIRE`、し、このモジュールで使用します。

### <a name="example-of-data-expiration-using-c-and-servicestackredis"></a>C# と ServiceStack.Redis を使用してデータの有効期限の例

などのクライアント ライブラリを使用する場合に、注意してください**ServiceStack.Redis**に低レベルの Azure Redis Cache コマンドを覚える必要はありません。 代わりに、単純な c# メソッドを使用できます。

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

Azure Redis Cache のデータの有効期限を使用して設定された後、データを自動的に削除することができます。 これは、機能は、Azure Redis Cache は、メモリ内データベースおよび量のメモリ使用可能なディスク上のデータを格納すると同様にしていないため、重要です。 格納するデータが長時間される必要がある場合、有効期限を設定することを確認します。