<span data-ttu-id="b0e96-101">アプリには UI と ViewModel があります。</span><span class="sxs-lookup"><span data-stu-id="b0e96-101">The app has a UI and a ViewModel.</span></span> <span data-ttu-id="b0e96-102">この演習では、Xamarin.Essentials を使用して、ViewModel に場所の参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-102">In this unit, you add location lookup to the ViewModel using Xamarin.Essentials.</span></span>

## <a name="enable-location-permissions"></a><span data-ttu-id="b0e96-103">場所のアクセス許可を有効にする</span><span class="sxs-lookup"><span data-stu-id="b0e96-103">Enable location permissions</span></span>

<span data-ttu-id="b0e96-104">すべてのモバイル プラットフォームでは、カメラ、フォト ライブラリ、ユーザーの場所など、ユーザーの情報と特定のハードウェアに関するセキュリティが適用されます。</span><span class="sxs-lookup"><span data-stu-id="b0e96-104">All mobile platforms have security around user information and certain hardware, such as the camera, photo library, and the user's location.</span></span> <span data-ttu-id="b0e96-105">アプリからユーザーの場所にアクセスできるようにするには、ユーザーはアクセス許可を付与する必要があります。その場合、インストール時に暗黙的にアクセス許可を付与するか、実行時にアクセス許可を付与するように選択します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-105">Before an app can access the user's location, the user has to grant permission - either by implicitly granting these permissions at install time or by choosing to grant a permission at runtime.</span></span> <span data-ttu-id="b0e96-106">ストアで UWP アプリを表示すると、一覧にアプリで必要なアクセス許可が表示されます。</span><span class="sxs-lookup"><span data-stu-id="b0e96-106">When you view a UWP app on the store, the listing will show the permissions that the app needs.</span></span> <span data-ttu-id="b0e96-107">アプリをインストールすることで、アクセス許可を暗黙的に付与します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-107">By installing the app, you implicitly grant permission.</span></span> <span data-ttu-id="b0e96-108">これらのアクセス許可はアプリのマニフェスト ファイルで構成されます。</span><span class="sxs-lookup"><span data-stu-id="b0e96-108">These permissions are configured in an app manifest file.</span></span>

1. <span data-ttu-id="b0e96-109">`ImHere.UWP` アプリ プロジェクトで、`Package.appxmanifest` ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="b0e96-109">In the `ImHere.UWP` app project, open the `Package.appxmanifest` file.</span></span>

1. <span data-ttu-id="b0e96-110">**[機能]** タブに移動して、*位置情報*機能をオンにします。</span><span class="sxs-lookup"><span data-stu-id="b0e96-110">Head to the **Capabilities** tab and check the *Location* capability.</span></span>

    ![UWP の [機能] タブ](../media/4-uwp-location-capability.png)

## <a name="query-for-the-users-location"></a><span data-ttu-id="b0e96-112">ユーザーの場所をクエリする</span><span class="sxs-lookup"><span data-stu-id="b0e96-112">Query for the user's location</span></span>

<span data-ttu-id="b0e96-113">ユーザーの場所を取得するには、2 つの方法があります。つまり、最新の場所または現在の場所を取得します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-113">There are two ways to get the user's location - the last known or the current.</span></span> <span data-ttu-id="b0e96-114">現在の場所を取得するには時間がかかる場合があります。これは、デバイスで GPS リンクを確立し、正確な場所が取得されるまで待機する必要がある場合があるためです。</span><span class="sxs-lookup"><span data-stu-id="b0e96-114">The current location can take some time to get because the device may need to establish a GPS link and wait for the accurate location to be retrieved.</span></span> <span data-ttu-id="b0e96-115">最も簡単な方法は、デバイスによって検出された最新の場所を取得することです。</span><span class="sxs-lookup"><span data-stu-id="b0e96-115">The fastest way is to get the last known location detected by the device.</span></span> <span data-ttu-id="b0e96-116">最新の場所は正確性に欠ける可能性がありますが、呼び出しははるかに速くなります。</span><span class="sxs-lookup"><span data-stu-id="b0e96-116">The last known location is potentially less accurate but is a much faster call.</span></span> <span data-ttu-id="b0e96-117">場所は緯度と経度 ([10 進経緯度](https://en.wikipedia.org/wiki/Decimal_degrees?azure-portal=true)に関する記述を参照) で表され、デバイスの海抜高度 (メートル単位) が示されます。</span><span class="sxs-lookup"><span data-stu-id="b0e96-117">Locations come as the latitude and longitude in [decimal degrees](https://en.wikipedia.org/wiki/Decimal_degrees?azure-portal=true) and the altitude of the device in meters above sea level.</span></span>

1. <span data-ttu-id="b0e96-118">`ImHere` NET Standard プロジェクト内で `MainViewModel` クラスを開きます。</span><span class="sxs-lookup"><span data-stu-id="b0e96-118">Open the `MainViewModel` class in the `ImHere` .NET Standard project.</span></span>

1. <span data-ttu-id="b0e96-119">`SendLocation` メソッド内で、`Xamarin.Essentials` 名前空間の `Geolocation` クラスに対して `GetLastKnownLocationAsync` 静的メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-119">In the `SendLocation` method, make a call to the `GetLastKnownLocationAsync` static method on the `Geolocation` class in the `Xamarin.Essentials` namespace.</span></span> <span data-ttu-id="b0e96-120">`Xamarin.Essentials` 名前空間に対して using ディレクティブを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0e96-120">You will need to add a using directive for the `Xamarin.Essentials` namespace.</span></span>

    ```csharp
    Location location = await Geolocation.GetLastKnownLocationAsync();
    ```

1. <span data-ttu-id="b0e96-121">ユーザーの場所が見つかった場合は、その場所で `Message` プロパティを更新します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-121">Update the `Message` property with the user's location if one is found.</span></span>

    ```csharp
    if (location != null)
    {
        Message = $"Location found: {location.Latitude}, {location.Longitude}.";
    }
    ```

    <span data-ttu-id="b0e96-122">このメソッドの完全なコードは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="b0e96-122">The full code for this method is below.</span></span>
    
    ```csharp
    async Task SendLocation()
    {
        Location location = await Geolocation.GetLastKnownLocationAsync();
    
        if (location != null)
        {
            Message = $"Location found: {location.Latitude}, {location.Longitude}.";
        }
    }
    ```

1. <span data-ttu-id="b0e96-123">アプリを実行し、**[場所の送信]** ボタンをクリックして UI で場所を表示します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-123">Run the app and click the **Send Location** button to see the location on the UI.</span></span>

    ![ユーザーの場所を示す実行中のアプリ](../media/4-running-app-showing-location.png)    

> [!NOTE]
> <span data-ttu-id="b0e96-125">このアプリでは最新の場所を使用します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-125">This app uses the last known location.</span></span> <span data-ttu-id="b0e96-126">運用品質アプリで、タイムアウトを設定して現在の正確な場所を取得する必要があります。時間内に見つからない場合は、最新の場所にフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="b0e96-126">In a production-quality app, you would want to get the current accurate location with a time-out, and if one is not found in time, fall back to the last known.</span></span> <span data-ttu-id="b0e96-127">これを行う方法の詳細については、[Xamarin.Essentials の地理的位置情報に関するドキュメント](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation?azure-portal=true)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b0e96-127">You can read more on how to do this in the [Xamarin.Essentials Geolocation docs](https://docs.microsoft.com/xamarin/essentials/geolocation?tabs=uwp#using-geolocation?azure-portal=true).</span></span>
> 
> <span data-ttu-id="b0e96-128">このアプリではエラー処理は行われません。</span><span class="sxs-lookup"><span data-stu-id="b0e96-128">This app does not have error handling.</span></span> <span data-ttu-id="b0e96-129">運用品質のアプリでは、場所を使用できなかった場合など、発生した例外を処理する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b0e96-129">In a production-quality app, you should handle any exceptions that occur, such as if the location was not available.</span></span>

## <a name="summary"></a><span data-ttu-id="b0e96-130">まとめ</span><span class="sxs-lookup"><span data-stu-id="b0e96-130">Summary</span></span>

<span data-ttu-id="b0e96-131">この演習では、Xamarin.Essentials を使用して、ユーザーの場所を取得する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="b0e96-131">In this unit, you learned how to use Xamarin.Essentials to get the user's location.</span></span> <span data-ttu-id="b0e96-132">次の演習では、モバイル アプリ用のバック エンドとして動作する Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="b0e96-132">In the next unit, you'll create an Azure function to act as a back end for the mobile app.</span></span>