<span data-ttu-id="40fad-101">この時点では、アプリはユーザーの位置を取得しようとしています。また、アプリは Azure 関数に送信される準備ができています。</span><span class="sxs-lookup"><span data-stu-id="40fad-101">At this point, the app is working to get the user's location and is ready to be sent to an Azure function.</span></span> <span data-ttu-id="40fad-102">この演習では、Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="40fad-102">In this unit, you build the Azure function.</span></span>

## <a name="create-an-azure-functions-project"></a><span data-ttu-id="40fad-103">Azure Functions プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="40fad-103">Create an Azure Functions project</span></span>

1. <span data-ttu-id="40fad-104">ソリューションを右クリックし、*[追加]、[新しいプロジェクト]* の順に選択して新しいプロジェクトを `ImHere` ソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="40fad-104">Add a new project to the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

2. <span data-ttu-id="40fad-105">左側にあるツリーから、*[Visual C#]、[クラウド]* の順に選択し、中央のパネルから *[Azure Functions]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="40fad-105">From the tree on the left-hand side, select *Visual C#->Cloud*, and then select *Azure Functions* from the panel in the center.</span></span>

3. <span data-ttu-id="40fad-106">プロジェクトに "ImHere.Functions" という名前を付け、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40fad-106">Name the project "ImHere.Functions", and then click **OK**.</span></span>

    ![[新しいプロジェクトの追加] ダイアログ](../media/5-add-new-functions-project.png)

4. <span data-ttu-id="40fad-108">**[新しいプロジェクト]** 構成ダイアログで、Functions のバージョンは *[Azure Functions v1 (.NET Framework)]* に設定されているままにします。</span><span class="sxs-lookup"><span data-stu-id="40fad-108">In the **New Project** configuration dialog, leave the Functions version set to *Azure Functions v1 (.NET Framework)*.</span></span> <span data-ttu-id="40fad-109">*[Http トリガー]* を選択し、ストレージ アカウントを *[ストレージ エミュレーター]* に設定されているままにし、アクセス権を *[匿名]* に設定します。</span><span class="sxs-lookup"><span data-stu-id="40fad-109">Select *Http Trigger*, leave the storage account set to *Storage Emulator*, and set the access rights to *Anonymous*.</span></span> <span data-ttu-id="40fad-110">次に、 **[OK]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="40fad-110">Then click **OK**.</span></span>

    ![Azure 関数のプロジェクト構成ダイアログ](../media/5-configure-trigger.png)

<span data-ttu-id="40fad-112">新しいプロジェクトが作成され、`Function1` という名前の既定の関数が与えられます。</span><span class="sxs-lookup"><span data-stu-id="40fad-112">The new project will be created and have a default function called `Function1`.</span></span>

> <span data-ttu-id="40fad-113">この関数は匿名アクセスで作成されました。</span><span class="sxs-lookup"><span data-stu-id="40fad-113">This function was created with anonymous access.</span></span> <span data-ttu-id="40fad-114">Azure に公開されると、URL を知っている人は誰でもこの関数を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="40fad-114">Once published to Azure, anybody who knows the URL will be able to call this function.</span></span> <span data-ttu-id="40fad-115">現実世界のシナリオでは、[Azure App Service 認証](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview)または [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c) など、何らかの形式の認証でこれを保護します。</span><span class="sxs-lookup"><span data-stu-id="40fad-115">In a real-world scenario, you would protect this with some form of authentication, such as [Azure App Service authentication](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) or [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c).</span></span>

## <a name="create-the-function"></a><span data-ttu-id="40fad-116">関数を作成する</span><span class="sxs-lookup"><span data-stu-id="40fad-116">Create the function</span></span>

<span data-ttu-id="40fad-117">Azure Functions プロジェクトは `Function1` という名前のシングル HTTP トリガーで作成されます。</span><span class="sxs-lookup"><span data-stu-id="40fad-117">The Azure Functions project is created with a single HTTP trigger function called `Function1`.</span></span> <span data-ttu-id="40fad-118">この関数自体は `Function1` クラスの静的 `Run` メソッドとして実装されます。</span><span class="sxs-lookup"><span data-stu-id="40fad-118">The function itself is implemented as a static `Run` method in the `Function1` class.</span></span>

1. <span data-ttu-id="40fad-119">ソリューション エクスプローラーのファイルの名前を "Function1.cs" から "SendLocation.cs" に変更します。</span><span class="sxs-lookup"><span data-stu-id="40fad-119">Rename the file in Solution Explorer from "Function1.cs" to "SendLocation.cs".</span></span> <span data-ttu-id="40fad-120">コード要素 `Function1` の参照の名前をすべて変更するように求められたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40fad-120">When prompted to rename all references to the code element `Function1`, click **Yes**.</span></span>

2. <span data-ttu-id="40fad-121">属性の関数名を "SendLocation" に変更します。</span><span class="sxs-lookup"><span data-stu-id="40fad-121">Rename the function name in the attribute to "SendLocation".</span></span>

    ```cs
    [FunctionName("SendLocation")]
    ```

3. <span data-ttu-id="40fad-122">ロガーに情報メッセージを書き込む最初の行を除き、関数の内容を削除します。</span><span class="sxs-lookup"><span data-stu-id="40fad-122">Delete the contents of the function, except the first line that writes an information message to the logger.</span></span>

    ```cs
    public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                   "get", "post",
                                                                   Route = null)]HttpRequestMessage req,
                                                       TraceWriter log)
    {
        log.Info("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a><span data-ttu-id="40fad-123">モバイル アプリと関数の間でデータを共有するクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="40fad-123">Create a class to share data between the mobile app and function</span></span>

<span data-ttu-id="40fad-124">データが Azure 関数に送信されるとき、データは JSON として送信されます。</span><span class="sxs-lookup"><span data-stu-id="40fad-124">When data is sent to the Azure function, it will be sent as JSON.</span></span> <span data-ttu-id="40fad-125">モバイル アプリによりデータが JSON にシリアル化され、関数により JSON から逆シリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="40fad-125">The mobile app will serialize data into JSON and the function will deserialize from JSON.</span></span> <span data-ttu-id="40fad-126">モバイル アプリと関数の間でこのデータの整合性を維持するためには、場所と電話番号データを保持するクラスを含む新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="40fad-126">To keep this data consistent between the mobile app and the function, create a new project that contains a class to hold the location and phone number data.</span></span> <span data-ttu-id="40fad-127">このプロジェクトはアプリと関数によって参照されます。</span><span class="sxs-lookup"><span data-stu-id="40fad-127">This project will then be referenced by the app and function.</span></span>

1. <span data-ttu-id="40fad-128">ソリューションを右クリックし、*[追加]、[新しいプロジェクト]* の順に選択して、`ImHere` ソリューションの下で新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="40fad-128">Create a new project under the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

2. <span data-ttu-id="40fad-129">左側にあるツリーから、*[Visual C#]、[.NET Standard]* の順に選択し、中央のパネルから *[クラス ライブラリ (.NET Standard)]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="40fad-129">From the tree on the left-hand side, select *Visual C#->.NET Standard*, and then select *Class Library (.NET Standard)* from the panel in the center.</span></span>

3. <span data-ttu-id="40fad-130">プロジェクトに "ImHere.Data" という名前を付け、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40fad-130">Name the project "ImHere.Data", and then click **OK**.</span></span>

    ![[新しいプロジェクトの追加] ダイアログ](../media/5-add-new-net-standard-project.png)

4. <span data-ttu-id="40fad-132">自動生成された "Class1.cs" ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="40fad-132">Delete the auto-generated "Class1.cs" file.</span></span>

5. <span data-ttu-id="40fad-133">プロジェクトを右クリックし、*[追加]、[クラス]* の順に選択して、`PostData` という名前の `ImHere.Data` プロジェクトで新しいクラスを作成します。新しいクラスに "PostData" という名前を付け、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="40fad-133">Create a new class in the `ImHere.Data` project called `PostData` by right-clicking on the project and then selecting *Add->Class...*. Name the new class "PostData" and click **OK**.</span></span>

6. <span data-ttu-id="40fad-134">緯度と経度に `double` プロパティと送信先の電話番号の `string[]` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="40fad-134">Add `double` properties for the latitude and longitude, as well as a `string[]` property for the phone numbers to send to.</span></span>

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

7. <span data-ttu-id="40fad-135">プロジェクトを右クリックし、*[追加]、[参照]* を選択して、このプロジェクトの参照を `ImHere.Functions` プロジェクトと `ImHere` プロジェクトの両方に追加します。左側にあるツリーから *[プロジェクト]* を選択し、*ImHere.Data* の隣にあるボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="40fad-135">Add a reference to this project to both the `ImHere.Functions` and `ImHere` projects by right-clicking on the project and then selecting *Add->Reference...*. Select *Projects* from the tree on the left-hand side, and then check the box next to *ImHere.Data*.</span></span>

    ![プロジェクト参照を構成する](../media/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a><span data-ttu-id="40fad-137">関数に送信されたデータを読み込む</span><span class="sxs-lookup"><span data-stu-id="40fad-137">Read the data sent to the function</span></span>

<span data-ttu-id="40fad-138">Azure 関数では、`req` パラメーターには行われた HTTP 要求が含まれます。この要求の中にあるデータは JSON でシリアル化された `PostData` オブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="40fad-138">In the Azure function, the `req` parameter contains the HTTP request that was made and the data inside this request will be a JSON serialized `PostData` object.</span></span>

1. <span data-ttu-id="40fad-139">`ImHere.Functions` プロジェクトで `SendLocation` クラスを開きます。</span><span class="sxs-lookup"><span data-stu-id="40fad-139">Open the `SendLocation` class in the `ImHere.Functions` project.</span></span>

2. <span data-ttu-id="40fad-140">HTTP 要求の内容を `PostData` オブジェクトに読み込み、`ImHere.Data` 名前空間に using ディレクティブを追加します。</span><span class="sxs-lookup"><span data-stu-id="40fad-140">Read the contents of the HTTP request into a `PostData` object, adding a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    PostData data = await req.Content.ReadAsAsync<PostData>();
    ```

3. <span data-ttu-id="40fad-141">`PostData` からの緯度と経度を利用して Google Maps URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="40fad-141">Construct a Google Maps URL using the latitude and longitude from the `PostData`.</span></span>

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

4. <span data-ttu-id="40fad-142">URL をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="40fad-142">Log the URL.</span></span>

    ```cs
    log.Info($"URL created - {url}");
    ```

5. <span data-ttu-id="40fad-143">200 ステータス コードを返し、関数がエラーなく完了したことを示します。</span><span class="sxs-lookup"><span data-stu-id="40fad-143">Return a 200 status code to show the function completed without error.</span></span>

    ```cs
    return req.CreateResponse(HttpStatusCode.OK);
    ```

<span data-ttu-id="40fad-144">完全な関数を下に示します。</span><span class="sxs-lookup"><span data-stu-id="40fad-144">The complete function is shown below.</span></span>

```cs
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                                Route = null)]HttpRequestMessage req,
                                                    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    PostData data = await req.Content.ReadAsAsync<PostData>();
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.Info($"URL created - {url}");
    return req.CreateResponse(HttpStatusCode.OK);
}
```

## <a name="run-the-azure-function-locally"></a><span data-ttu-id="40fad-145">Azure 関数をローカルで実行する</span><span class="sxs-lookup"><span data-stu-id="40fad-145">Run the Azure function locally</span></span>

<span data-ttu-id="40fad-146">関数は、ローカル ストレージ アカウントとローカル Azure Functions ランタイムを利用してローカルで実行できます。</span><span class="sxs-lookup"><span data-stu-id="40fad-146">Functions can be run locally using a local storage account and local Azure Functions runtime.</span></span> <span data-ttu-id="40fad-147">このローカル ランタイムでは、Azure にデプロイする前に関数をテストできます。</span><span class="sxs-lookup"><span data-stu-id="40fad-147">This local runtime allows you to test out your function before deploying it to Azure.</span></span>

1. <span data-ttu-id="40fad-148">ソリューション エクスプローラーで `ImHere.Functions` プロジェクトを右クリックし、*[スタートアップ プロジェクトに設定]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="40fad-148">Right-click on the `ImHere.Functions` project in the solution explorer, and then select *Set as StartUp project*.</span></span>

2. <span data-ttu-id="40fad-149">*[デバッグ]* メニューから *[デバッグなしで開始]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="40fad-149">From the *Debug* menu, select *Start Without Debugging*.</span></span> <span data-ttu-id="40fad-150">ローカル Azure Functions ランタイムはコンソール ウィンドウ内で起動し、関数を開始し、`localhost` で利用できるポートをリッスンします。</span><span class="sxs-lookup"><span data-stu-id="40fad-150">The local Azure Functions runtime will launch inside a console window and start your function, listening on an available port on `localhost`.</span></span>

    ![ローカルで実行される Azure 関数](../media/5-function-running-locally.png)

3. <span data-ttu-id="40fad-152">関数がリッスンしているポートをメモします。</span><span class="sxs-lookup"><span data-stu-id="40fad-152">Take a note of the port that the function is listening on.</span></span> <span data-ttu-id="40fad-153">これは次の演習でモバイル アプリをテストするために必要になります。</span><span class="sxs-lookup"><span data-stu-id="40fad-153">You'll need this in the next unit to test out the mobile app.</span></span> <span data-ttu-id="40fad-154">上記の図で、関数はポート **7071** でリッスンしています。</span><span class="sxs-lookup"><span data-stu-id="40fad-154">In the image above, the function is listening on port **7071**.</span></span>

    ```sh
    Listening on http://localhost:7071/
    ```

4. <span data-ttu-id="40fad-155">次の演習でモバイル アプリをテストできるように、関数は実行中のままにします。</span><span class="sxs-lookup"><span data-stu-id="40fad-155">Leave the function running so that you can test the mobile app in the next unit.</span></span>

## <a name="summary"></a><span data-ttu-id="40fad-156">まとめ</span><span class="sxs-lookup"><span data-stu-id="40fad-156">Summary</span></span>

<span data-ttu-id="40fad-157">この演習では、Visual Studio で Azure Functions プロジェクトを作成する方法を学習し、モバイル アプリと関数の間で共有するデータ オブジェクトを含む共有プロジェクトを追加し、渡されたデータを逆シリアル化する関数の基本実装を作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="40fad-157">In this unit, you learned how to create an Azure Functions project in Visual Studio, added a shared project with a data object to be shared between the mobile app and the function, and learned how to create a basic implementation of the function to deserialize the data passed in.</span></span> <span data-ttu-id="40fad-158">Azure 関数をローカルで実行する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="40fad-158">You also learned how to run an Azure function locally.</span></span> <span data-ttu-id="40fad-159">次の演習では、モバイル アプリから Azure 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="40fad-159">In the next unit, you'll call the Azure function from the mobile app.</span></span>