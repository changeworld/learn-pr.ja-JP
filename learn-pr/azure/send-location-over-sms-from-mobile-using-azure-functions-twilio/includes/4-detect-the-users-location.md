<span data-ttu-id="71f53-101">アプリには UI と ViewModel があります。</span><span class="sxs-lookup"><span data-stu-id="71f53-101">The app has a UI and a ViewModel.</span></span> <span data-ttu-id="71f53-102">この演習では、Xamarin.Essentials を使用して、ViewModel に場所の参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="71f53-102">In this unit, you add location lookup to the ViewModel using Xamarin.Essentials.</span></span>

## <a name="enable-location-permissions"></a><span data-ttu-id="71f53-103">場所のアクセス許可を有効にする</span><span class="sxs-lookup"><span data-stu-id="71f53-103">Enable location permissions</span></span>

<span data-ttu-id="71f53-104">すべてのモバイル プラットフォームでは、カメラ、フォト ライブラリ、ユーザーの場所など、ユーザーの情報と特定のハードウェアに関するセキュリティが適用されます。</span><span class="sxs-lookup"><span data-stu-id="71f53-104">All mobile platforms have security around user information and certain hardware, such as the camera, photo library, and the user's location.</span></span> <span data-ttu-id="71f53-105">アプリからユーザーの場所にアクセスできるようにするには、ユーザーはアクセス許可を付与する必要があります。その場合、インストール時に暗黙的にアクセス許可を付与するか、実行時にアクセス許可を付与するように選択します。</span><span class="sxs-lookup"><span data-stu-id="71f53-105">Before an app can access the user's location, the user has to grant permission - either by implicitly granting these permissions at install time or by choosing to grant a permission at runtime.</span></span> <span data-ttu-id="71f53-106">ストアで UWP アプリを表示すると、一覧にアプリで必要なアクセス許可が表示されます。</span><span class="sxs-lookup"><span data-stu-id="71f53-106">When you view a UWP app on the store, the listing will show the permissions that the app needs.</span></span> <span data-ttu-id="71f53-107">アプリをインストールすることで、アクセス許可を暗黙的に付与します。</span><span class="sxs-lookup"><span data-stu-id="71f53-107">By installing the app, you implicitly grant permission.</span></span> <span data-ttu-id="71f53-108">これらのアクセス許可はアプリのマニフェスト ファイルで構成されます。</span><span class="sxs-lookup"><span data-stu-id="71f53-108">These permissions are configured in an app manifest file.</span></span>

1. <span data-ttu-id="71f53-109">`ImHere.UWP` アプリ プロジェクトで、`Package.appxmanifest` ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="71f53-109">In the `ImHere.UWP` app project, open the `Package.appxmanifest` file.</span></span>

1. <span data-ttu-id="71f53-110">**[機能]** タブに移動して、*位置情報*機能をオンにします。</span><span class="sxs-lookup"><span data-stu-id="71f53-110">Head to the **Capabilities** tab and check the *Location* capability.</span></span>

    ![UWP の [機能] タブ](../media-drafts/4-uwp-location-capability.png)

> <span data-ttu-id="71f53-112">Android や iOS をサポートする場合は、アクセス許可を個別に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f53-112">If you want to support Android or iOS, the permissions need to be configured differently.</span></span> <span data-ttu-id="71f53-113">詳細については、[Xamarin.Essentials の地理的位置情報に関するドキュメント](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=android#getting-started)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="71f53-113">This is detailed in the [Xamarin.Essentials Geolocation docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=android#getting-started).</span></span>

## <a name="query-for-the-users-location"></a><span data-ttu-id="71f53-114">ユーザーの場所をクエリする</span><span class="sxs-lookup"><span data-stu-id="71f53-114">Query for the user's location</span></span>

<span data-ttu-id="71f53-115">ユーザーの場所を取得するには、2 つの方法があります。つまり、最新の場所または現在の場所を取得します。</span><span class="sxs-lookup"><span data-stu-id="71f53-115">There are two ways to get the user's location - the last known or the current.</span></span> <span data-ttu-id="71f53-116">現在の場所を取得するには時間がかかる場合があります。これは、デバイスで GPS リンクを確立し、正確な場所が取得されるまで待機する必要がある場合があるためです。</span><span class="sxs-lookup"><span data-stu-id="71f53-116">The current location can take some time to get because the device may need to establish a GPS link and wait for the accurate location to be retrieved.</span></span> <span data-ttu-id="71f53-117">最も簡単な方法は、デバイスによって検出された最新の場所を取得することです。</span><span class="sxs-lookup"><span data-stu-id="71f53-117">The fastest way is to get the last known location detected by the device.</span></span> <span data-ttu-id="71f53-118">最新の場所は正確性に欠ける可能性がありますが、呼び出しははるかに速くなります。</span><span class="sxs-lookup"><span data-stu-id="71f53-118">The last known location is potentially less accurate but is a much faster call.</span></span> <span data-ttu-id="71f53-119">場所は緯度と経度 ([10 進経緯度](https://en.wikipedia.org/wiki/Decimal_degrees)に関する記述を参照) で表され、デバイスの海抜高度 (メートル単位) が示されます。</span><span class="sxs-lookup"><span data-stu-id="71f53-119">Locations come as the latitude and longitude in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees) and the altitude of the device in meters above sea level.</span></span>

1. <span data-ttu-id="71f53-120">`ImHere` .NET 標準プロジェクトで `MainViewModel` クラスを開きます。</span><span class="sxs-lookup"><span data-stu-id="71f53-120">Open the `MainViewModel` class in the `ImHere` .NET standard project.</span></span>

1. <span data-ttu-id="71f53-121">`SendLocation` メソッドで、`Xamarin.Essentials` 名前空間の `Geolocation` クラスに対して `GetLastKnownLocationAsync` 静的メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="71f53-121">In the `SendLocation` method, make a call to the `GetLastKnownLocationAsync` static method on the `Geolocation` class in the `Xamarin.Essentials` namespace.</span></span>

    ```cs
    Location location = await Geolocation.GetLastKnownLocationAsync();
    ```

1. <span data-ttu-id="71f53-122">ユーザーの場所が見つかった場合は、その場所で `Message` プロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="71f53-122">Update the `Message` property with the user's location if one is found.</span></span>

    ```cs
    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
    ```

<span data-ttu-id="71f53-123">このメソッドの完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="71f53-123">The full code for this method is below.</span></span>

```cs
async Task SendLocation()
{
    Location location = await Geolocation.GetLastKnownLocationAsync();

    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
}
```

<span data-ttu-id="71f53-124">アプリを実行し、**[場所の送信]** ボタンをクリックして UI で場所を表示します。</span><span class="sxs-lookup"><span data-stu-id="71f53-124">Run the app and click the **Send Location** button to see the location on the UI.</span></span>

![ユーザーの場所を示す実行中のアプリ](../media-drafts/4-running-app-showing-location.png)

> <span data-ttu-id="71f53-126">このアプリでは最新の場所を使用します。</span><span class="sxs-lookup"><span data-stu-id="71f53-126">This app uses the last known location.</span></span> <span data-ttu-id="71f53-127">運用品質アプリで、タイムアウトを設定して現在の正確な場所を取得する必要があります。時間内に見つからない場合は、最新の場所にフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="71f53-127">In a production-quality app, you would want to get the current accurate location with a time-out, and if one is not found in time, fall back to the last known.</span></span> <span data-ttu-id="71f53-128">これを行う方法の詳細については、[Xamarin.Essentials の地理的位置情報に関するドキュメント](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation)を参照してください。このアプリではエラー処理は行われません。</span><span class="sxs-lookup"><span data-stu-id="71f53-128">You can read more on how to do this in the [Xamarin.Essentials Geolocation docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation). This app does not have error handling.</span></span> <span data-ttu-id="71f53-129">運用品質のアプリでは、場所を使用できなかった場合など、発生した例外を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="71f53-129">In a production-quality app, you should handle any exceptions that occur, such as if the location was not available.</span></span>

## <a name="summary"></a><span data-ttu-id="71f53-130">まとめ</span><span class="sxs-lookup"><span data-stu-id="71f53-130">Summary</span></span>

<span data-ttu-id="71f53-131">この演習では、Xamarin.Essentials を使用して、ユーザーの場所を取得する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="71f53-131">In this unit, you learned how to use Xamarin.Essentials to get the user's location.</span></span> <span data-ttu-id="71f53-132">次の演習では、モバイル アプリ用のバック エンドとして動作する Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="71f53-132">In the next unit, you'll create an Azure function to act as a back end for the mobile app.</span></span>