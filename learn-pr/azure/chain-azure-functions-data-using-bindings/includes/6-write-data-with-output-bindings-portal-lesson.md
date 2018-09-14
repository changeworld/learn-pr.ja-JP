入力バインドと同様は複数の出力バインドの種類です。

複数の種類の出力バインドがある、ただしすべての種類をサポートして、入力し出力します。 送信またはデータを格納するときにいつでも使用します。 ここでは、出力バインドと使用すると、その使用をサポートする型を紹介します。

## <a name="output-binding-types"></a>出力バインドの種類

- **Blob Storage** blob 出力を書き込む blob へのバインドを使用することができます。

- **Cosmos DB** Azure Cosmos DB 出力バインドを使用して SQL API を使用して Azure Cosmos DB データベースに新しいドキュメントを記述できます。

- **Event Hubs** Event Hubs 出力をイベント ストリームにイベントを記述するバインドを使用します。 イベントを書き込むには、イベント ハブへの送信アクセス許可が必要です。

- **HTTP** HTTP 出力の HTTP 要求送信者に応答するバインドを使用します。 このバインドには、HTTP トリガーが必要です。このバインドを使用すると、トリガーの要求に関連付けられている応答をカスタマイズすることができます。

- **Microsoft Graph** Microsoft Graph の出力バインドでは、OneDrive でファイルに書き込むし、Excel データを変更して、Outlook で電子メールを送信することができます。

- **Mobile Apps** Mobile Apps 出力バインドの書き込みを Mobile Apps テーブルに新しいレコード。

- **Notification Hubs** Notification Hubs の出力バインドでプッシュ通知を送信することができます。

- **Queue Storage** Azure Queue storage の出力をキューにメッセージを書き込むバインドを使用します。

- **グリッドの送信**における SendGrid のバインディングを使用してメールを送信します。

- **Service Bus**使用して Azure Service Bus キューまたはトピック メッセージを送信するバインドを出力します。

- **テーブル ストレージ**使用して、Azure Table storage の出力を Azure Storage アカウント内のテーブルへの書き込みにバインドします。

- **Twilio**を Twilio でテキスト メッセージを送信します。

- **Webhook** HTTP 出力の HTTP 要求送信者に応答するバインドを使用します。 このバインドには、HTTP トリガーが必要です。このバインドを使用すると、トリガーの要求に関連付けられている応答をカスタマイズすることができます。

## <a name="how-to-create-an-output-binding"></a>出力バインドを作成する方法は?
バインドを定義するには、入力を定義する必要あります、`direction`として`out`します。
バインドの種類ごとにパラメーターが異なる場合がありますにも記載されているもの[Microsoft のドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)

## <a name="combining-input-and-output-bindings"></a>結合の入力呼び出し力バインド 
1 つの関数に複数のバインドを適用することになります。 これにより、入力と出力の両方のバインドを定義することができます。

同じバインドの種類は、入力と出力ですることもできます.