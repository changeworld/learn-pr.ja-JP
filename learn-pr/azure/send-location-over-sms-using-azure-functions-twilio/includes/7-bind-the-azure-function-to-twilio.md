<span data-ttu-id="096a9-101">この時点で、モバイル アプリは完成しており、データを逆シリアル化できる Azure 関数にユーザーの場所と電話番号の一覧を送信できます。</span><span class="sxs-lookup"><span data-stu-id="096a9-101">At this point, the mobile app is complete and it can send the user's location and list of phone numbers to an Azure function that can deserialize the data.</span></span> <span data-ttu-id="096a9-102">この演習では、Azure 関数を Twilio にバインドして SMS メッセージを送信します。</span><span class="sxs-lookup"><span data-stu-id="096a9-102">In this unit, you bind the Azure function to Twilio to send SMS messages.</span></span>

<span data-ttu-id="096a9-103">Azure Functions は、他のサービス: Azure のサービスまたはサード パーティのサービスのどちらかに接続できます。</span><span class="sxs-lookup"><span data-stu-id="096a9-103">Azure Functions can be connected to other services, either services in Azure or third-party services.</span></span> <span data-ttu-id="096a9-104">バインディングと呼ばれるこれらの接続には、入力バインディングと出力バインディングの 2 つの形式があります。</span><span class="sxs-lookup"><span data-stu-id="096a9-104">These connections, called bindings, exist in two forms - input and output bindings.</span></span> <span data-ttu-id="096a9-105">入力バインディングでは、関数にデータを提供し、出力バインディングでは、関数からデータを取得して別のサービスに送信します。</span><span class="sxs-lookup"><span data-stu-id="096a9-105">Input bindings provide data to your function and output bindings take data from your function and send it to another service.</span></span> <span data-ttu-id="096a9-106">バインディングについては、[Azure Functions のバインディングに関するドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings?azure-portal=true)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="096a9-106">You can read about bindings in the [Azure Functions Binding docs](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings?azure-portal=true).</span></span>

<span data-ttu-id="096a9-107">Visual Studio で作成された Azure Functions のバインディングの定義には、属性で修飾された関数のパラメーターが使用されます。</span><span class="sxs-lookup"><span data-stu-id="096a9-107">Bindings for Azure Functions created in Visual Studio are defined using parameters to the function, decorated with attributes.</span></span>

## <a name="bind-the-azure-function-to-twilio"></a><span data-ttu-id="096a9-108">Azure 関数を Twilio にバインドする</span><span class="sxs-lookup"><span data-stu-id="096a9-108">Bind the Azure function to Twilio</span></span>

<span data-ttu-id="096a9-109">Twilio を介して SMS メッセージを送信するには、アカウント サブスクリプション ID (SID) と認証トークンで構成されている出力バインディングが必要です。</span><span class="sxs-lookup"><span data-stu-id="096a9-109">Sending SMS messages via Twilio requires an output binding that is configured with your account subscription ID (SID) and Auth Token.</span></span>

1. <span data-ttu-id="096a9-110">ローカルの Azure Functions ランタイムが前の演習から引き続き実行されている場合は、これを停止します。</span><span class="sxs-lookup"><span data-stu-id="096a9-110">Stop the local Azure Functions runtime if it's still running from the previous unit.</span></span>

1. <span data-ttu-id="096a9-111">`ImHere.Functions` プロジェクトに "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet パッケージ v3.0.0-rc1 を追加します。</span><span class="sxs-lookup"><span data-stu-id="096a9-111">Add the "Microsoft.Azure.WebJobs.Extensions.Twilio" NuGet package v3.0.0-rc1 to the `ImHere.Functions` project.</span></span> <span data-ttu-id="096a9-112">**Twilio バインディングを使用した安定バージョンでのバグが原因で、バージョン 3.0.0 ではなく 3.0.0-rc1 を使用します**。</span><span class="sxs-lookup"><span data-stu-id="096a9-112">**Use version 3.0.0-rc1, NOT 3.0.0 due to a bug in the stable version with Twilio bindings**.</span></span> <span data-ttu-id="096a9-113">この NuGet パッケージには、バインディングに関連するクラスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="096a9-113">This NuGet package contains the relevant classes for the binding.</span></span>

1. <span data-ttu-id="096a9-114">`messages` という名前の静的な `SendLocation` クラスで、静的な `Run` メソッドに新しいパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="096a9-114">Add a new parameter to the static `Run` method on the `SendLocation` static class called `messages`.</span></span> <span data-ttu-id="096a9-115">このパラメーターの型は `ICollector<CreateMessageOptions>` となります。</span><span class="sxs-lookup"><span data-stu-id="096a9-115">This parameter will have a type of `ICollector<CreateMessageOptions>`.</span></span> <span data-ttu-id="096a9-116">`Twilio.Rest.Api.V2010.Account` 名前空間に対して `using` ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="096a9-116">You'll need to add a `using` directive for the `Twilio.Rest.Api.V2010.Account` namespace.</span></span>

    ```cs
    [FunctionName("SendLocation")]
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,"get", "post", Route = null)]HttpRequestMessage req,
                                                ICollector<CreateMessageOptions> messages,
                                                ILogger log)
    ```

1. <span data-ttu-id="096a9-117">次のように新しい `messages` パラメーターを `TwilioSms` 属性で修飾します。</span><span class="sxs-lookup"><span data-stu-id="096a9-117">Decorate the new `messages` parameter with the `TwilioSms` attribute as follows:</span></span> 

      ```cs
    [TwilioSms(AccountSidSetting = "TwilioAccountSid",AuthTokenSetting = "TwilioAuthToken", From = "+1xxxxxxxxx")]ICollector<CreateMessageOptions> messages,
    ```
    <span data-ttu-id="096a9-118">この属性には、設定する必要がある 3 つのパラメーターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="096a9-118">This attribute has three parameters you need to set:</span></span>

    * <span data-ttu-id="096a9-119">**AccountSidSetting** - これは `"TwilioAccountSid"` に設定します</span><span class="sxs-lookup"><span data-stu-id="096a9-119">**AccountSidSetting** - set this to `"TwilioAccountSid"`</span></span>
  
        <span data-ttu-id="096a9-120">これは、モジュール内で以前に記録したご自分の Twilio アカウントの SID です。</span><span class="sxs-lookup"><span data-stu-id="096a9-120">This is the SID for your Twilio account that you recorded earlier in the module.</span></span> <span data-ttu-id="096a9-121">SID を直接設定するのではなく、このパラメーターは、SID の取得に使用される関数アプリ設定内の値の名前となります。</span><span class="sxs-lookup"><span data-stu-id="096a9-121">Rather than set the SID directly, this parameter is the name of a value in the function app settings that will be used to retrieve the SID.</span></span>

    * <span data-ttu-id="096a9-122">**AuthTokenSetting** - これは `"TwilioAuthToken"` に設定します</span><span class="sxs-lookup"><span data-stu-id="096a9-122">**AuthTokenSetting** - set this to `"TwilioAuthToken"`</span></span>

       <span data-ttu-id="096a9-123">これは、モジュール内で以前に記録したご自分の Twilio アカウントの認証トークンです。</span><span class="sxs-lookup"><span data-stu-id="096a9-123">This is the Auth Token for your Twilio account that you recorded earlier in the module.</span></span> <span data-ttu-id="096a9-124">このパラメーターによって認証トークンが直接設定されるのではなく、このパラメーターは認証トークンの取得に使用される関数アプリ設定の値の名前となります。</span><span class="sxs-lookup"><span data-stu-id="096a9-124">Rather than set the Auth Token directly, this parameter is the name of a value in the function app settings that will be used to retrieve the Auth Token.</span></span>

    * <span data-ttu-id="096a9-125">**From** - これは、モジュール内で以前に記録したご自分のアクティブな Twilio 電話番号に設定します。</span><span class="sxs-lookup"><span data-stu-id="096a9-125">**From** - set this to your Twilio active phone number that you recorded earlier in the module.</span></span>

        <span data-ttu-id="096a9-126">SMS メッセージの送信元となる国際フォーマットの Twilio 電話番号 (+\<国コード\>\<電話番号\>、たとえば "+1555123456")。</span><span class="sxs-lookup"><span data-stu-id="096a9-126">The Twilio phone number that the SMS messages will come from in international format (+\<country code\>\<phone number\>, for example "+1555123456").</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="096a9-127">電話番号から必ずすべての空白を削除してください。</span><span class="sxs-lookup"><span data-stu-id="096a9-127">Make sure to remove all spaces from the phone number.</span></span>


1. <span data-ttu-id="096a9-128">関数アプリ設定は、`local.settings.json` ファイル内にローカルで構成できます。</span><span class="sxs-lookup"><span data-stu-id="096a9-128">Function app settings can be configured locally inside the `local.settings.json` file.</span></span> <span data-ttu-id="096a9-129">`TwilioSMS` 属性に渡された設定名を使用して、この JSON ファイルに Twilio アカウント SID と認証トークンを追加します。</span><span class="sxs-lookup"><span data-stu-id="096a9-129">Add your Twilio account SID and Auth Token to this JSON file using the setting names that were passed to the `TwilioSMS` attribute.</span></span>

    ```json
    {
        "IsEncrypted": false,
        "Values": {
            "AzureWebJobsStorage": "UseDevelopmentStorage=true",
            "FUNCTIONS_WORKER_RUNTIME": "dotnet",
            "TwilioAccountSid": "<Your SID>",
            "TwilioAuthToken": "<Your Auth Token>"
        }
    }
    ```

    <span data-ttu-id="096a9-130">\<[Your SID]\(ユーザーの SID\)\>と\<[Your Auth Token]\(ユーザーの認証トークン\)\> を、Twilio ダッシュ ボードの値に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="096a9-130">Replace \<Your SID\> and \<Your Auth Token\> with the values from your Twilio dashboard.</span></span>

    > [!NOTE]
    > <span data-ttu-id="096a9-131">これらのローカル設定は、ローカルでの実行だけを目的とします。</span><span class="sxs-lookup"><span data-stu-id="096a9-131">These local settings will be only for running locally.</span></span> <span data-ttu-id="096a9-132">運用アプリでは、これらの値はローカル開発またはテスト アカウントの資格情報となります。</span><span class="sxs-lookup"><span data-stu-id="096a9-132">In a production app, these values would be your local development or test account credentials.</span></span> <span data-ttu-id="096a9-133">Azure 関数が Azure にデプロイされたら、運用時の値を構成できるようになります。</span><span class="sxs-lookup"><span data-stu-id="096a9-133">Once the Azure Function has been deployed to Azure, you'll be able to configure the production values.</span></span>

     > [!NOTE]
    > <span data-ttu-id="096a9-134">コードをソース管理にチェックインすると、これらのローカル アプリケーション設定値もチェックインされます。そのため、コードがオープン ソース、またはどのような形式でもパブリックである場合は、これらのファイル内の実際の値をチェックインしないようにしてください。</span><span class="sxs-lookup"><span data-stu-id="096a9-134">If you check your code into source control, these local application setting values will be checked in, too, so be careful not to check in any actual values in these files if your code is open source or public in any form.</span></span>
    

## <a name="create-the-sms-messages"></a><span data-ttu-id="096a9-135">SMS メッセージを作成する</span><span class="sxs-lookup"><span data-stu-id="096a9-135">Create the SMS messages</span></span>

<span data-ttu-id="096a9-136">`ICollector<CreateMessageOptions>` パラメーターは `CreateMessageOptions` インスタンスのコレクションであり、送信する SMS メッセージを収集するために使用します。</span><span class="sxs-lookup"><span data-stu-id="096a9-136">The `ICollector<CreateMessageOptions>` parameter is a collection of `CreateMessageOptions` instances and is used to collect the SMS messages you want to send.</span></span> <span data-ttu-id="096a9-137">関数の実行が終了したら、このコレクションに追加された `CreateMessageOptions` のインスタンスはいずれも Twilio に渡され、関連する受信者に送信するメッセージを作成するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="096a9-137">After the function has finished running, any instances of `CreateMessageOptions` added to this collection are passed to Twilio and used to create messages to be sent to the relevant recipients.</span></span>

1. <span data-ttu-id="096a9-138">`SendLocation` 関数内で、`PostData` 内の電話番号をループするコードを追加し、各電話番号に対して SMS メッセージを作成します。</span><span class="sxs-lookup"><span data-stu-id="096a9-138">In the `SendLocation` function, add code to loop through the phone numbers in the `PostData` and create an SMS message for each one.</span></span> <span data-ttu-id="096a9-139">`Twilio.Types` 用の using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="096a9-139">You will need to add a using directive for `Twilio.Types`.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        PhoneNumber number = new PhoneNumber(toNo);
        CreateMessageOptions message = new CreateMessageOptions(number)
        {
            Body = $"I'm here! {url}"
        };
    }
    ```

    <span data-ttu-id="096a9-140">メッセージには、送信先の電話番号と、ユーザーの場所から作成された Google Maps URL を含む本文が必要です。</span><span class="sxs-lookup"><span data-stu-id="096a9-140">The message needs the phone number to send to and a body that contains the Google Maps URL created from the user's location.</span></span>

1. <span data-ttu-id="096a9-141">各メッセージを記録してから、`messages` コレクションに追加します。</span><span class="sxs-lookup"><span data-stu-id="096a9-141">Log each message, and then add it to the `messages` collection.</span></span>

    ```cs
    foreach (string toNo in data.ToNumbers)
    {
        ...
        log.LogInformation($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }
    ```

<span data-ttu-id="096a9-142">完全な `SendLocation` メソッドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="096a9-142">The complete `SendLocation` method is shown below.</span></span> <span data-ttu-id="096a9-143">`From` パラメーター内のプレースホルダーは、ご利用のアクティブな電話番号に置き換えられます。</span><span class="sxs-lookup"><span data-stu-id="096a9-143">Your active phone number would replace the placeholder in the `From` parameter.</span></span>

```cs
[FunctionName("SendLocation")]
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                         "get", "post",
                                                         Route = null)]HttpRequest req,
                                            [TwilioSms(AccountSidSetting = "TwilioAccountSid",
                                                       AuthTokenSetting = "TwilioAuthToken",
                                                       From = "+1xxxxxxxxx")] ICollector<CreateMessageOptions> messages,
                                            ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.LogInformation($"URL created - {url}");

    foreach (string toNo in data.ToNumbers)
    {
        PhoneNumber number = new PhoneNumber(toNo);
        CreateMessageOptions message = new CreateMessageOptions(number)
        {
            Body = $"I'm here! {url}"
        };
        log.LogInformation($"Creating SMS message to {message.To}, message is '{message.Body}'.");
        messages.Add(message);
    }

    return new OkResult();
}
```

## <a name="test-it-out"></a><span data-ttu-id="096a9-144">テストする</span><span class="sxs-lookup"><span data-stu-id="096a9-144">Test it out</span></span>

1. <span data-ttu-id="096a9-145">`ImHere.Functions` アプリをスタートアップ プロジェクトとして設定し、デバッグなしで起動します。</span><span class="sxs-lookup"><span data-stu-id="096a9-145">Set the `ImHere.Functions` app as the startup project and start it without debugging.</span></span>

1. <span data-ttu-id="096a9-146">`ImHere.UWP` アプリをスタートアップ プロジェクトとして設定し、実行します。</span><span class="sxs-lookup"><span data-stu-id="096a9-146">Set the `ImHere.UWP` app as the startup project and run it.</span></span>

1. <span data-ttu-id="096a9-147">Xamarin.Forms アプリに自分自身の電話番号を国際フォーマット (+\<国コード\>\<電話番号\>) で入力します。</span><span class="sxs-lookup"><span data-stu-id="096a9-147">Enter your own phone number in international format (+\<country code\>\<phone number\>) into the Xamarin.Forms app.</span></span> <span data-ttu-id="096a9-148">Twilio 試用版アカウントでは、検証済みの電話番号にのみメッセージを送信できます。そのためここでは、有料アカウントにアップグレードするか、他の番号を認証しない限り、自分自身にしかメッセージを送信できません。</span><span class="sxs-lookup"><span data-stu-id="096a9-148">Twilio trial accounts can send  messages only to verified phone numbers, so for now, you'll only be able to message yourself unless you upgrade to a paid account or verify other numbers.</span></span>

1. <span data-ttu-id="096a9-149">**[Send Location]\(場所の送信\)** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="096a9-149">Click the **Send Location** button.</span></span> <span data-ttu-id="096a9-150">SMS メッセージが正常に送信された場合は、Xamarin.Forms アプリに "Location sent successfully" (場所が正常に送信されました) というメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="096a9-150">If the SMS message was sent successfully, you'll see a message in the Xamarin.Forms app saying, "Location sent successfully".</span></span>

    ![場所が送信されたことを示す Xamarin.Forms アプリ](../media/7-ui-location-sent.png)

1. <span data-ttu-id="096a9-152">Azure 関数のコンソール ログに、作成されて送信されているメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="096a9-152">In the console logs for the Azure function, you'll see the message being created and sent.</span></span> <span data-ttu-id="096a9-153">エラー (番号のフォーマットが正しくないなど) が発生した場合は、ここに記録されます。</span><span class="sxs-lookup"><span data-stu-id="096a9-153">If any errors occur (such as, the number is in the wrong format), they will be logged out here.</span></span>

    ![メッセージが送信されたことを示す Azure 関数コンソール](../media/7-function-message-sent.png)

1. <span data-ttu-id="096a9-155">自分の電話でメッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="096a9-155">Check your phone for a message.</span></span> <span data-ttu-id="096a9-156">メッセージに記載されたリンクに従って、自分の場所を確認します。</span><span class="sxs-lookup"><span data-stu-id="096a9-156">Follow the link in the message to see your location.</span></span>

    ![携帯電話で受信した SMS メッセージ](../media/7-message-received.png)

    > [!TIP]
    > <span data-ttu-id="096a9-158">表示場所はアプリが実行されている場所です。したがって、VM の実行元であるデータ センターの近くということになります。</span><span class="sxs-lookup"><span data-stu-id="096a9-158">The location you'll see is the location where the app is running, so will be near to the data center that the VM is running from.</span></span> <span data-ttu-id="096a9-159">このアプリがご利用のローカル デバイス上で実行されていた場合、その場所が表示されます。</span><span class="sxs-lookup"><span data-stu-id="096a9-159">If this app was running on your local device then it would show your location.</span></span>

## <a name="summary"></a><span data-ttu-id="096a9-160">まとめ</span><span class="sxs-lookup"><span data-stu-id="096a9-160">Summary</span></span>

<span data-ttu-id="096a9-161">この演習では、Azure 関数に対して Twilio バインディングを作成し、ローカルで実行されている関数に、ユーザーの場所を示す SMS メッセージを送信する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="096a9-161">In this unit, you learned how to create a Twilio binding for the Azure function and send an SMS message with the user's location to a function that was running locally.</span></span> <span data-ttu-id="096a9-162">次の演習では、Azure に関数を発行します。</span><span class="sxs-lookup"><span data-stu-id="096a9-162">In the next unit, you publish the function to Azure.</span></span>
