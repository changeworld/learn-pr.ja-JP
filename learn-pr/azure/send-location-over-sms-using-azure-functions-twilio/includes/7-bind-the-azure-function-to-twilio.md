この時点で、モバイル アプリは完成しており、データを逆シリアル化できる Azure 関数にユーザーの場所と電話番号の一覧を送信できます。 この演習では、Azure 関数を Twilio にバインドして SMS メッセージを送信します。

Azure Functions は、他のサービス: Azure のサービスまたはサード パーティのサービスのどちらかに接続できます。 バインディングと呼ばれるこれらの接続には、入力バインディングと出力バインディングの 2 つの形式があります。 入力バインディングでは、関数にデータを提供し、出力バインディングでは、関数からデータを取得して別のサービスに送信します。 バインディングについては、[Azure Functions のバインディングに関するドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)をご覧ください。

Visual Studio で作成された Azure Functions のバインディングの定義には、属性で修飾された関数のパラメーターが使用されます。

## <a name="bind-the-azure-function-to-twilio"></a>Azure 関数を Twilio にバインドする

Twilio を介して SMS メッセージを送信するには、アカウント サブスクリプション ID (SID) と認証トークンで構成されている出力バインディングが必要です。

1. ローカルの Azure Functions ランタイムが前の演習から引き続き実行されている場合は、これを停止します。

1. `ImHere.Functions` プロジェクトに "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet パッケージを追加します。 この NuGet パッケージには、バインディングに関連するクラスが含まれています。

1. `messages` という名前の静的な `SendLocation` クラスで、静的な `Run` メソッドに新しいパラメーターを追加します。 このパラメーターの型は `ICollector<SMSMessage>` となります。 `Twilio` 名前空間に対して using ディレクティブを追加する必要があります。

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                      ICollector<SMSMessage> messages,
                                                      TraceWriter log)
    ```

1. 新しい `messages` パラメーターを `TwilioSms` 属性で装飾します。 この属性には、設定する必要がある 3 つのパラメーターが含まれます。

    | Setting      |  値   | 説明                                        |
    | --- | --- | ---|
    | **AccountSidSetting** | "TwilioAccountSid" | Twilio アカウントの SID。 SID を直接設定するのではなく、このパラメーターは、SID の取得に使用される関数アプリ設定の値の名前となります。 |
    | **AuthTokenSetting** | "TwilioAuthToken" | Twilio アカウントの認証トークン。 認証トークンを直接設定するのではなく、このパラメーターは、認証トークンの取得に使用される関数アプリ設定の値の名前となります。 |
    | **From** | Twilio 電話番号 | SMS メッセージの送信元となる国際フォーマットの Twilio 電話番号 (+\<国コード\>\<電話番号\>、たとえば "+1555123456")。 |

    Twilio アカウントを作成すると、メッセージの送信元として使用する電話番号が割り当てられます。 この電話番号は、Twilio の **[Phone Numbers]\(電話番号\)** ダッシュ ボードで確認できます。 Twilio サイトで、左側のメニューの下部にある省略記号を選択します。 次に、*[SUPER NETWORK]\(スーパー ネットワーク\) -> [Phone Numbers]\(電話番号\)* の順に選択します。 ピン アイコンを使用して、左側のメニューにこのダッシュ ボードをピン留めすることができます。 *[Manage Numbers]\(番号の管理\) -> [Active Numbers]\(有効な番号\)* の下に、Twilio 番号が表示されます。 番号からすべてのスペースを削除する必要があります。

    ![Twilio 番号を検索する](../media/7-twilio-find-number.png)

    ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
               AuthTokenSetting = "TwilioAuthToken",
               From = "+1xxxxxxxxx")]ICollector<SMSMessage> messages,
    ```

1. 関数アプリ設定は、`local.settings.json` ファイル内にローカルで構成できます。 `TwilioSMS` 属性に渡された設定名を使用して、この JSON ファイルに Twilio アカウント SID と認証トークンを追加します。

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "UseDevelopmentStorage=true",
            "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
            "TwilioAccountSid": "<Your SID>",
            "TwilioAuthToken": "<Your Auth Token>"
        }
    }
    ```

    \<[Your SID]\(ユーザーの SID\)\>と\<[Your Auth Token]\(ユーザーの認証トークン\)\> を、Twilio ダッシュ ボードの値に置き換えます。

    > これらのローカル設定は、ローカルでの実行だけを目的とします。 運用アプリでは、これらの値はローカル開発またはテスト アカウントの資格情報となります。 Azure 関数が Azure にデプロイされたら、運用時の値を構成できるようになります。
    > コードをソース管理にチェックインすると、これらのローカル アプリケーション設定値もチェックインされます。そのため、コードがオープン ソース、またはどのような形式でもパブリックである場合は、これらのファイル内の実際の値をチェックインしないようにしてください。

## <a name="create-the-sms-messages"></a>SMS メッセージを作成する

`ICollector<SMSMessage>` パラメーターは `SMSMessage` インスタンスのコレクションであり、送信する SMS メッセージを収集するために使用します。 関数の実行が終了したら、このコレクションに追加された `SMSMessage` のすべてのインスタンスが Twilio に渡され、関連する受信者に送信されます。

1. `SendLocation` 関数で、`PostData` 内の電話番号をループするコードを追加し、各電話番号に対して SMS メッセージを作成します。

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        SMSMessage message = new SMSMessage
        {
            Body = $"I'm here! {url}",
            To = toNo
        };
    }
    ```

    メッセージには、送信先の電話番号と、ユーザーの場所から作成された Google Maps URL を含む本文が必要です。

1. 各メッセージを記録してから、`messages` コレクションに追加します。

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

完全な `SendLocation` メソッドを次に示します。

```cs
[FunctionName("SendLocation")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                                Route = null)]HttpRequestMessage req,
                                                    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
                                                                AuthTokenSetting = "TwilioAuthToken",
                                                                From = "<your Twilio phone number>")]ICollector<SMSMessage> messages,
                                                    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    PostData data = await req.Content.ReadAsAsync<PostData>();
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.Info($"URL created - {url}");
    foreach (string toNo in data.ToNumbers)
    {
        SMSMessage message = new SMSMessage
        {
            Body = $"I'm here! {url}",
            To = toNo
        };
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

## <a name="test-it-out"></a>テストする

1. `ImHere.Functions` アプリをスタートアップ プロジェクトとして設定し、デバッグなしで起動します。

1. `ImHere.UWP` アプリをスタートアップ プロジェクトとして設定し、実行します。

1. Xamarin.Forms アプリに自分自身の電話番号を国際フォーマット (+\<国コード\>\<電話番号\>) で入力します。 Twilio 試用版アカウントでは、検証済みの電話番号にのみメッセージを送信できます。そのためここでは、有料アカウントにアップグレードするか、他の番号を認証しない限り、自分自身にしかメッセージを送信できません。

1. **[Send Location]\(場所の送信\)** ボタンをクリックします。 SMS メッセージが正常に送信された場合は、Xamarin.Forms アプリに "Location sent successfully" (場所が正常に送信されました) というメッセージが表示されます。

    ![場所が送信されたことを示す Xamarin.Forms アプリ](../media/7-ui-location-sent.png)

1. Azure 関数のコンソール ログに、作成されて送信されているメッセージが表示されます。 エラー (番号のフォーマットが正しくないなど) が発生した場合は、ここに記録されます。

    ![メッセージが送信されたことを示す Azure 関数コンソール](../media/7-function-message-sent.png)

1. 自分の電話でメッセージを確認します。 メッセージに記載されたリンクに従って、自分の場所を確認します。

    ![携帯電話で受信した SMS メッセージ](../media/7-message-received.png)

## <a name="summary"></a>まとめ

この演習では、Azure 関数に対して Twilio バインディングを作成し、ローカルで実行されている関数に、ユーザーの場所を示す SMS メッセージを送信する方法について説明しました。 次の演習では、Azure に関数を発行します。