<span data-ttu-id="7b420-101">モバイル アプリが実行されます。Azure 関数の最初のバージョンが作成されました。</span><span class="sxs-lookup"><span data-stu-id="7b420-101">The mobile app runs and the initial version of the Azure function has been created.</span></span> <span data-ttu-id="7b420-102">この演習では、モバイル アプリから Azure 関数を呼び出し、ユーザーの位置と、ユーザーが SMS メッセージを送信する電話番号の一覧を渡します。</span><span class="sxs-lookup"><span data-stu-id="7b420-102">In this unit, you call the Azure function from the mobile app, passing in the user's location and the list of phone numbers the user wants to send SMS messages to.</span></span>

## <a name="calling-the-azure-function-from-the-mobile-app"></a><span data-ttu-id="7b420-103">モバイル アプリから Azure 関数を呼び出す</span><span class="sxs-lookup"><span data-stu-id="7b420-103">Calling the Azure function from the mobile app</span></span>

1. <span data-ttu-id="7b420-104">`MainViewModel` を開きます。</span><span class="sxs-lookup"><span data-stu-id="7b420-104">Open the `MainViewModel`.</span></span>

1. <span data-ttu-id="7b420-105">このクラスで、`client` という名前のプライベート `HttpClient` フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="7b420-105">In this class, add a private `HttpClient` field called `client`.</span></span> <span data-ttu-id="7b420-106">`System.Net.Http` 名前空間に参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7b420-106">You'll need to add a reference to the `System.Net.Http` namespace.</span></span>

    ```cs
    HttpClient client = new HttpClient();
    ```

1. <span data-ttu-id="7b420-107">関数の基本 URL に定数フィールドを追加します。</span><span class="sxs-lookup"><span data-stu-id="7b420-107">Add a constant field for the base URL for the function.</span></span> <span data-ttu-id="7b420-108">ローカルの Azure Functions ランタイムがリッスンしているアドレスにこれを設定します。</span><span class="sxs-lookup"><span data-stu-id="7b420-108">Set this to the address that the local Azure Functions runtime is listening on.</span></span> <span data-ttu-id="7b420-109">関数が Azure にデプロイされたら、この定数を Azure URL に変更できます。</span><span class="sxs-lookup"><span data-stu-id="7b420-109">Once the function is deployed to Azure, this constant can be changed to be the Azure URL.</span></span>

    ```cs
    const string baseUrl = "http://localhost:7071";
    ```

1. <span data-ttu-id="7b420-110">`SendLocation` メソッド内に、場所が見つかった後、場所とユーザーが入力した電話番号の一覧を利用して `PostData` の新しいインスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7b420-110">Inside the `SendLocation` method, after the location has been found, create a new instance of `PostData` using the location and the list of phone numbers entered by the user.</span></span> <span data-ttu-id="7b420-111">`ImHere.Data` 名前空間に対して using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7b420-111">You'll need to add a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    PostData postData = new PostData
    {
        Latitude = location.Latitude,
        Longitude = location.Longitude,
        ToNumbers = PhoneNumbers.Split('\n')
    };
    ```

    > [!NOTE]
    > <span data-ttu-id="7b420-112">これは、`Editor` コントロールの行ごとに 1 つ、電話番号が正しい形式で入力されていることを前提とします。</span><span class="sxs-lookup"><span data-stu-id="7b420-112">This assumes that the phone numbers have been entered in the correct format, one per line in the `Editor` control.</span></span> <span data-ttu-id="7b420-113">運用品質のアプリの場合は、これに対して検証が行われ、1 つまたは複数の電話番号が入力されたこと、形式が正しいことが確保されます。</span><span class="sxs-lookup"><span data-stu-id="7b420-113">In a production-quality app, there would be validation around this to ensure one or more phone numbers were entered and were in the correct format.</span></span>    
 

1. <span data-ttu-id="7b420-114">JSON として `PostData` をシリアル化する最も簡単な方法は、Newtonsoft.Json NuGet パッケージを使用することです。</span><span class="sxs-lookup"><span data-stu-id="7b420-114">To serialize the `PostData` as JSON, the easiest way is to use the Newtonsoft.Json NuGet package.</span></span> <span data-ttu-id="7b420-115">前のユニットで Xamarin.Essentials を追加したときと同じ方法で `ImHere` プロジェクトにこの NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="7b420-115">Add this NuGet package to the `ImHere` project in the same way that you added Xamarin.Essentials in an earlier unit.</span></span>

1. <span data-ttu-id="7b420-116">`JsonConvert` 静的クラスを使用して `PostData` を `string` にシリアル化します。</span><span class="sxs-lookup"><span data-stu-id="7b420-116">Serialize the `PostData` to a `string` using the `JsonConvert` static class.</span></span> <span data-ttu-id="7b420-117">`Newtonsoft.Json` 名前空間に対して using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="7b420-117">You'll need to add a using directive for the `Newtonsoft.Json` namespace.</span></span> <span data-ttu-id="7b420-118">JSON として Azure 関数に渡せるように、`StringContent` クラスにこの文字列をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="7b420-118">Encode this string into a `StringContent` class so that it can be passed to the Azure function as JSON.</span></span>

    ```cs
    string data = JsonConvert.SerializeObject(postData);
    StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
    ```

1. <span data-ttu-id="7b420-119">このデータを関数に送り、結果を取得します。</span><span class="sxs-lookup"><span data-stu-id="7b420-119">Post this data to the function and get the result back.</span></span>

   ```cs
    HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                        content);
   ```

   <span data-ttu-id="7b420-120">Azure 関数は `/api/<function name>` の使用でアクセスされます。ローカル Functions ランタイムで選択されるポートが 7071 であると想定すると、`SendLocation` 関数には `http://localhost:7071/api/SendLocation` でアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="7b420-120">Azure functions are accessed using `/api/<function name>`, so assuming the port chosen by the local Functions runtime is 7071, the `SendLocation` function will be accessible at `http://localhost:7071/api/SendLocation`.</span></span>

1. <span data-ttu-id="7b420-121">結果に応じて、UI にメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="7b420-121">Depending on the result, show a message on the UI.</span></span>

    ```cs
    if (result.IsSuccessStatusCode)
        Message = "Location sent successfully";
    else
        Message = $"Error - {result.ReasonPhrase}";
    ```

<span data-ttu-id="7b420-122">新しいフィールドと `SendLocation` メソッドの完全なコードは下のようになります。</span><span class="sxs-lookup"><span data-stu-id="7b420-122">The full code for the new fields and the `SendLocation` method is below.</span></span>

```cs
HttpClient client = new HttpClient();
const string baseUrl = "http://localhost:7071";

async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";

        PostData postData = new PostData
        {
            Latitude = location.Latitude,
            Longitude = location.Longitude,
            ToNumbers = PhoneNumbers.Split('\n')
        };

        string data = JsonConvert.SerializeObject(postData);
        StringContent content = new StringContent(data, Encoding.UTF8, "application/json");
        HttpResponseMessage result = await client.PostAsync($"{baseUrl}/api/SendLocation",
                                                            content);

        if (result.IsSuccessStatusCode)
            Message = "Location sent successfully";
        else
            Message = $"Error - {result.ReasonPhrase}";
    }
}
```

## <a name="testing-it-out"></a><span data-ttu-id="7b420-123">テスト</span><span class="sxs-lookup"><span data-stu-id="7b420-123">Testing it out</span></span>

1. <span data-ttu-id="7b420-124">Azure 関数が引き続きローカルで実行されていること、ポートが `SendLocation` メソッドに一致することを確認します。</span><span class="sxs-lookup"><span data-stu-id="7b420-124">Make sure the Azure function is still running locally and the port matches the `SendLocation` method.</span></span>

1. <span data-ttu-id="7b420-125">UWP アプリをスタートアップ アプリとして設定し、実行します。</span><span class="sxs-lookup"><span data-stu-id="7b420-125">Set the UWP app as the startup app and run it.</span></span> <span data-ttu-id="7b420-126">**[Send Location]\(場所の送信\)** ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="7b420-126">Click the **Send Location** button.</span></span> <span data-ttu-id="7b420-127">Functions ランタイム コンソールに出力が表示され、呼び出されている関数を確認できます。ログには生成された URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7b420-127">You'll see output in the Functions runtime console window showing the function being called, and the logging showing the generated URL.</span></span>

    ![呼び出されている関数の出力](../media/6-function-called.png)

1. <span data-ttu-id="7b420-129">URL 生成をテストするには、それをコンソールからブラウザーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="7b420-129">To test the URL generation, paste it from the console into a browser.</span></span> <span data-ttu-id="7b420-130">現在の場所が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="7b420-130">It should show your current location.</span></span>

    > [!TIP]
    > <span data-ttu-id="7b420-131">表示場所はアプリが実行されている場所です。したがって、VM の実行元であるデータ センターの近くということになります。</span><span class="sxs-lookup"><span data-stu-id="7b420-131">The location you'll see is the location where the app is running, so will be near to the data center that the VM is running from.</span></span> <span data-ttu-id="7b420-132">このアプリがご利用のローカル デバイス上で実行されていた場合、その場所が表示されます。</span><span class="sxs-lookup"><span data-stu-id="7b420-132">If this app was running on your local device then it would show your location.</span></span>

## <a name="summary"></a><span data-ttu-id="7b420-133">まとめ</span><span class="sxs-lookup"><span data-stu-id="7b420-133">Summary</span></span>

<span data-ttu-id="7b420-134">この演習では、モバイル アプリから Azure 関数を呼び出す方法について学習しました。</span><span class="sxs-lookup"><span data-stu-id="7b420-134">In this unit, you learned how to call an Azure function from the mobile app.</span></span> <span data-ttu-id="7b420-135">この呼び出しでは、ユーザーの場所と、JSON として入力した電話番号が渡されました。</span><span class="sxs-lookup"><span data-stu-id="7b420-135">This call passed the user's location and the phone numbers they entered as JSON.</span></span> <span data-ttu-id="7b420-136">次の演習では、Azure 関数を Twilio にバインドし、この場所を SMS メッセージとして送信します。</span><span class="sxs-lookup"><span data-stu-id="7b420-136">In the next unit, you'll bind the Azure function to Twilio to send this location as an SMS message.</span></span>