<span data-ttu-id="96595-101">この時点で、モバイル アプリは完成しており、データを逆シリアル化できる Azure 関数にユーザーの場所と電話番号の一覧を送信できます。</span><span class="sxs-lookup"><span data-stu-id="96595-101">At this point, the mobile app is complete and it can send the user's location and list of phone numbers to an Azure function that can deserialize the data.</span></span> <span data-ttu-id="96595-102">この演習では、Azure 関数を Twilio にバインドして SMS メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="96595-102">In this unit, you bind the Azure function to Twilio to send SMS messages.</span></span>

<span data-ttu-id="96595-103">Azure Functions は、他のサービス: Azure のサービスまたはサード パーティのサービスのどちらかに接続できます。</span><span class="sxs-lookup"><span data-stu-id="96595-103">Azure Functions can be connected to other services, either services in Azure or third-party services.</span></span> <span data-ttu-id="96595-104">バインディングと呼ばれるこれらの接続には、入力バインディングと出力バインディングの 2 つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="96595-104">These connections, called bindings, exist in two forms - input and output bindings.</span></span> <span data-ttu-id="96595-105">入力バインディングでは、関数にデータを提供し、出力バインディングでは、関数からデータを取得して別のサービスに送信します。</span><span class="sxs-lookup"><span data-stu-id="96595-105">Input bindings provide data to your function and output bindings take data from your function and send it to another service.</span></span> <span data-ttu-id="96595-106">バインディングについては、[Azure Functions のバインディングに関するドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="96595-106">You can read about bindings in the [Azure Functions Binding docs](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings).</span></span>

<span data-ttu-id="96595-107">Visual Studio で作成された Azure Functions のバインディングの定義には、属性で修飾された関数のパラメーターが使用されます。</span><span class="sxs-lookup"><span data-stu-id="96595-107">Bindings for Azure Functions created in Visual Studio are defined using parameters to the function, decorated with attributes.</span></span>

## <a name="bind-the-azure-function-to-twilio"></a><span data-ttu-id="96595-108">Azure 関数を Twilio にバインドする</span><span class="sxs-lookup"><span data-stu-id="96595-108">Bind the Azure function to Twilio</span></span>

<span data-ttu-id="96595-109">Twilio を介して SMS メッセージを送信するには、アカウント サブスクリプション ID (SID) と認証トークンで構成されている出力バインディングが必要です。</span><span class="sxs-lookup"><span data-stu-id="96595-109">Sending SMS messages via Twilio requires an output binding that is configured with your account subscription ID (SID) and Auth Token.</span></span>

1. <span data-ttu-id="96595-110">ローカルの Azure Functions ランタイムが前の演習から引き続き実行されている場合は、これを停止します。</span><span class="sxs-lookup"><span data-stu-id="96595-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

1. <span data-ttu-id="96595-111">`ImHere.Functions` プロジェクトに "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="96595-111">Add the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package to the `ImHere.Functions` project.</span></span> <span data-ttu-id="96595-112">この NuGet パッケージには、バインディングに関連するクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="96595-112">This NuGet package contains the relevant classes for the binding.</span></span>

1. <span data-ttu-id="96595-113">`messages` という名前の静的な `SendLocation` クラスで、静的な `Run` メソッドに新しいパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="96595-113">Add a new parameter to the static `Run` method on the `SendLocation` static class called `messages`.</span></span> <span data-ttu-id="96595-114">このパラメーターの型は `ICollector<SMSMessage>` となります。</span><span class="sxs-lookup"><span data-stu-id="96595-114">This parameter will have a type of `ICollector<SMSMessage>`.</span></span> <span data-ttu-id="96595-115">`Twilio` 名前空間に対して using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="96595-115">You'll need to add a using directive for the `Twilio` namespace.</span></span>

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                      ICollector<SMSMessage> messages,
                                                      TraceWriter log)
    ```

1. <span data-ttu-id="96595-116">新しい `messages` パラメーターを `TwilioSms` 属性で装飾します。</span><span class="sxs-lookup"><span data-stu-id="96595-116">Decorate the new `messages` parameter with the `TwilioSms` attribute.</span></span> <span data-ttu-id="96595-117">この属性には、設定する必要がある 3 つのパラメーターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="96595-117">This attribute has three parameters you need to set.</span></span>

    | <span data-ttu-id="96595-118">Setting</span><span class="sxs-lookup"><span data-stu-id="96595-118">Setting</span></span>      |  <span data-ttu-id="96595-119">値</span><span class="sxs-lookup"><span data-stu-id="96595-119">Value</span></span>   | <span data-ttu-id="96595-120">説明</span><span class="sxs-lookup"><span data-stu-id="96595-120">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="96595-121">**AccountSidSetting**</span><span class="sxs-lookup"><span data-stu-id="96595-121">**AccountSidSetting**</span></span> | <span data-ttu-id="96595-122">"TwilioAccountSid"</span><span class="sxs-lookup"><span data-stu-id="96595-122">"TwilioAccountSid"</span></span> | <span data-ttu-id="96595-123">Twilio アカウントの SID。</span><span class="sxs-lookup"><span data-stu-id="96595-123">The SID for your Twilio account.</span></span> <span data-ttu-id="96595-124">SID を直接設定するのではなく、このパラメーターは、SID の取得に使用される関数アプリ設定の値の名前となります。</span><span class="sxs-lookup"><span data-stu-id="96595-124">Rather than set the SID directly, this parameter is the name of a value in the function app settings that will be used to retrieve the SID.</span></span> |
    | <span data-ttu-id="96595-125">**AuthTokenSetting**</span><span class="sxs-lookup"><span data-stu-id="96595-125">**AuthTokenSetting**</span></span> | <span data-ttu-id="96595-126">"TwilioAuthToken"</span><span class="sxs-lookup"><span data-stu-id="96595-126">"TwilioAuthToken"</span></span> | <span data-ttu-id="96595-127">Twilio アカウントの認証トークン。</span><span class="sxs-lookup"><span data-stu-id="96595-127">The Auth Token for your Twilio account.</span></span> <span data-ttu-id="96595-128">認証トークンを直接設定するのではなく、このパラメーターは、認証トークンの取得に使用される関数アプリ設定の値の名前となります。</span><span class="sxs-lookup"><span data-stu-id="96595-128">Rather than set the Auth Token directly, this parameter is the name of a value in the function app settings that will be used to retrieve the Auth Token.</span></span> |
    | <span data-ttu-id="96595-129">**From**</span><span class="sxs-lookup"><span data-stu-id="96595-129">**From**</span></span> | <span data-ttu-id="96595-130">Twilio 電話番号</span><span class="sxs-lookup"><span data-stu-id="96595-130">Your Twilio phone number</span></span> | <span data-ttu-id="96595-131">SMS メッセージの送信元となる国際フォーマットの Twilio 電話番号 (+\<国コード\>\<電話番号\>、たとえば "+1555123456")。</span><span class="sxs-lookup"><span data-stu-id="96595-131">The Twilio phone number that the SMS messages will come from in international format (+\<country code\>\<phone number\>, for example "+1555123456").</span></span> |

    <span data-ttu-id="96595-132">Twilio アカウントを作成すると、メッセージの送信元として使用する電話番号が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="96595-132">When you create a Twilio account, you are assigned a phone number that you can send messages from.</span></span> <span data-ttu-id="96595-133">この電話番号は、Twilio の **[Phone Numbers]\(電話番号\)** ダッシュ ボードで確認できます。</span><span class="sxs-lookup"><span data-stu-id="96595-133">You can find this phone number on the Twilio **Phone Numbers** dashboard.</span></span> <span data-ttu-id="96595-134">Twilio サイトで、左側のメニューの下部にある省略記号を選択します。</span><span class="sxs-lookup"><span data-stu-id="96595-134">From the Twilio site, select the ellipses at the bottom of the left-hand menu.</span></span> <span data-ttu-id="96595-135">次に、*[SUPER NETWORK]\(スーパー ネットワーク\) -> [Phone Numbers]\(電話番号\)* の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="96595-135">Then, select *SUPER NETWORK->Phone Numbers*.</span></span> <span data-ttu-id="96595-136">ピン アイコンを使用して、左側のメニューにこのダッシュ ボードをピン留めすることができます。</span><span class="sxs-lookup"><span data-stu-id="96595-136">You can pin this dashboard to the left-hand menu using the pin icon.</span></span> <span data-ttu-id="96595-137">*[Manage Numbers]\(番号の管理\) -> [Active Numbers]\(有効な番号\)* の下に、Twilio 番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="96595-137">Your Twilio number will be under *Manage Numbers->Active Numbers*.</span></span> <span data-ttu-id="96595-138">番号からすべてのスペースを削除する必要があります。</span><span class="sxs-lookup"><span data-stu-id="96595-138">You'll need to remove any spaces from the number.</span></span>

    ![Twilio 番号を検索する](../media/7-twilio-find-number.png)

    ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",
               AuthTokenSetting = "TwilioAuthToken",
               From = "+1xxxxxxxxx")]ICollector<SMSMessage> messages,
    ```

1. <span data-ttu-id="96595-140">関数アプリ設定は、`local.settings.json` ファイル内にローカルで構成できます。</span><span class="sxs-lookup"><span data-stu-id="96595-140">Function app settings can be configured locally inside the `local.settings.json` file.</span></span> <span data-ttu-id="96595-141">`TwilioSMS` 属性に渡された設定名を使用して、この JSON ファイルに Twilio アカウント SID と認証トークンを追加します。</span><span class="sxs-lookup"><span data-stu-id="96595-141">Add your Twilio account SID and Auth Token to this JSON file using the setting names that were passed to the `TwilioSMS` attribute.</span></span>

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

    <span data-ttu-id="96595-142">\<[Your SID]\(ユーザーの SID\)\>と\<[Your Auth Token]\(ユーザーの認証トークン\)\> を、Twilio ダッシュ ボードの値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="96595-142">Replace \<Your SID\> and \<Your Auth Token\> with the values from your Twilio dashboard.</span></span>

    > <span data-ttu-id="96595-143">これらのローカル設定は、ローカルでの実行だけを目的とします。</span><span class="sxs-lookup"><span data-stu-id="96595-143">These local settings will be only for running locally.</span></span> <span data-ttu-id="96595-144">運用アプリでは、これらの値はローカル開発またはテスト アカウントの資格情報となります。</span><span class="sxs-lookup"><span data-stu-id="96595-144">In a production app, these values would be your local development or test account credentials.</span></span> <span data-ttu-id="96595-145">Azure 関数が Azure にデプロイされたら、運用時の値を構成できるようになります。</span><span class="sxs-lookup"><span data-stu-id="96595-145">Once the Azure Function has been deployed to Azure, you'll be able to configure the production values.</span></span>
    > <span data-ttu-id="96595-146">コードをソース管理にチェックインすると、これらのローカル アプリケーション設定値もチェックインされます。そのため、コードがオープン ソース、またはどのような形式でもパブリックである場合は、これらのファイル内の実際の値をチェックインしないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="96595-146">If you check your code into source control, these local application setting values will be checked in, too, so be careful not to check in any actual values in these files if your code is open source or public in any form.</span></span>

## <a name="create-the-sms-messages"></a><span data-ttu-id="96595-147">SMS メッセージを作成する</span><span class="sxs-lookup"><span data-stu-id="96595-147">Create the SMS messages</span></span>

<span data-ttu-id="96595-148">`ICollector<SMSMessage>` パラメーターは `SMSMessage` インスタンスのコレクションであり、送信する SMS メッセージを収集するために使用します。</span><span class="sxs-lookup"><span data-stu-id="96595-148">The `ICollector<SMSMessage>` parameter is a collection of `SMSMessage` instances and is used to collect the SMS messages you want to send.</span></span> <span data-ttu-id="96595-149">関数の実行が終了したら、このコレクションに追加された `SMSMessage` のすべてのインスタンスが Twilio に渡され、関連する受信者に送信されます。</span><span class="sxs-lookup"><span data-stu-id="96595-149">After the function has finished running, any instances of `SMSMessage` added to this collection are passed to Twilio and sent to the relevant recipients.</span></span>

1. <span data-ttu-id="96595-150">`SendLocation` 関数で、`PostData` 内の電話番号をループするコードを追加し、各電話番号に対して SMS メッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="96595-150">In the `SendLocation` function, add code to loop through the phone numbers in the `PostData` and create an SMS message for each one.</span></span>

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

    <span data-ttu-id="96595-151">メッセージには、送信先の電話番号と、ユーザーの場所から作成された Google Maps URL を含む本文が必要です。</span><span class="sxs-lookup"><span data-stu-id="96595-151">The message needs the phone number to send to and a body that contains the Google Maps URL created from the user's location.</span></span>

1. <span data-ttu-id="96595-152">各メッセージを記録してから、`messages` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="96595-152">Log each message, and then add it to the `messages` collection.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.Info($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

<span data-ttu-id="96595-153">完全な `SendLocation` メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="96595-153">The complete `SendLocation` method is shown below.</span></span>

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

## <a name="test-it-out"></a><span data-ttu-id="96595-154">テストする</span><span class="sxs-lookup"><span data-stu-id="96595-154">Test it out</span></span>

1. <span data-ttu-id="96595-155">`ImHere.Functions` アプリをスタートアップ プロジェクトとして設定し、デバッグなしで起動します。</span><span class="sxs-lookup"><span data-stu-id="96595-155">Set the `ImHere.Functions` app as the startup project and start it without debugging.</span></span>

1. <span data-ttu-id="96595-156">`ImHere.UWP` アプリをスタートアップ プロジェクトとして設定し、実行します。</span><span class="sxs-lookup"><span data-stu-id="96595-156">Set the `ImHere.UWP` app as the startup project and run it.</span></span>

1. <span data-ttu-id="96595-157">Xamarin.Forms アプリに自分自身の電話番号を国際フォーマット (+\<国コード\>\<電話番号\>) で入力します。</span><span class="sxs-lookup"><span data-stu-id="96595-157">Enter your own phone number in international format (+\<country code\>\<phone number\>) into the Xamarin.Forms app.</span></span> <span data-ttu-id="96595-158">Twilio 試用版アカウントでは、検証済みの電話番号にのみメッセージを送信できます。そのためここでは、有料アカウントにアップグレードするか、他の番号を認証しない限り、自分自身にしかメッセージを送信できません。</span><span class="sxs-lookup"><span data-stu-id="96595-158">Twilio trial accounts can send  messages only to verified phone numbers, so for now, you'll only be able to message yourself unless you upgrade to a paid account or verify other numbers.</span></span>

1. <span data-ttu-id="96595-159">**[Send Location]\(場所の送信\)** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="96595-159">Click the **Send Location** button.</span></span> <span data-ttu-id="96595-160">SMS メッセージが正常に送信された場合は、Xamarin.Forms アプリに "Location sent successfully" (場所が正常に送信されました) というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="96595-160">If the SMS message was sent successfully, you'll see a message in the Xamarin.Forms app saying, "Location sent successfully".</span></span>

    ![場所が送信されたことを示す Xamarin.Forms アプリ](../media/7-ui-location-sent.png)

1. <span data-ttu-id="96595-162">Azure 関数のコンソール ログに、作成されて送信されているメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="96595-162">In the console logs for the Azure function, you'll see the message being created and sent.</span></span> <span data-ttu-id="96595-163">エラー (番号のフォーマットが正しくないなど) が発生した場合は、ここに記録されます。</span><span class="sxs-lookup"><span data-stu-id="96595-163">If any errors occur (such as, the number is in the wrong format), they will be logged out here.</span></span>

    ![メッセージが送信されたことを示す Azure 関数コンソール](../media/7-function-message-sent.png)

1. <span data-ttu-id="96595-165">自分の電話でメッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="96595-165">Check your phone for a message.</span></span> <span data-ttu-id="96595-166">メッセージに記載されたリンクに従って、自分の場所を確認します。</span><span class="sxs-lookup"><span data-stu-id="96595-166">Follow the link in the message to see your location.</span></span>

    ![携帯電話で受信した SMS メッセージ](../media/7-message-received.png)

## <a name="summary"></a><span data-ttu-id="96595-168">まとめ</span><span class="sxs-lookup"><span data-stu-id="96595-168">Summary</span></span>

<span data-ttu-id="96595-169">この演習では、Azure 関数に対して Twilio バインディングを作成し、ローカルで実行されている関数に、ユーザーの場所を示す SMS メッセージを送信する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="96595-169">In this unit, you learned how to create a Twilio binding for the Azure function and send an SMS message with the user's location to a function that was running locally.</span></span> <span data-ttu-id="96595-170">次の演習では、Azure に関数を発行します。</span><span class="sxs-lookup"><span data-stu-id="96595-170">In the next unit, you publish the function to Azure.</span></span>