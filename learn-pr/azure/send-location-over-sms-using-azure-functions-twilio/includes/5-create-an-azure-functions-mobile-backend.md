<span data-ttu-id="7c44d-101">この時点では、アプリはユーザーの位置を取得しようとしています。また、アプリは Azure 関数に送信される準備ができています。</span><span class="sxs-lookup"><span data-stu-id="7c44d-101">At this point, the app is working to get the user's location and is ready to be sent to an Azure function.</span></span> <span data-ttu-id="7c44d-102">この演習では、Azure 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-102">In this unit, you build the Azure function.</span></span>

## <a name="create-an-azure-functions-project"></a><span data-ttu-id="7c44d-103">Azure Functions プロジェクトを作成する</span><span class="sxs-lookup"><span data-stu-id="7c44d-103">Create an Azure Functions project</span></span>

1. <span data-ttu-id="7c44d-104">ソリューションを右クリックし、*[追加]、[新しいプロジェクト]* の順に選択して新しいプロジェクトを `ImHere` ソリューションに追加します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-104">Add a new project to the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="7c44d-105">左側にあるツリーから、*[Visual C#]、[クラウド]* の順に選択し、中央のパネルから *[Azure Functions]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-105">From the tree on the left-hand side, select *Visual C#->Cloud*, and then select *Azure Functions* from the panel in the center.</span></span>

1. <span data-ttu-id="7c44d-106">プロジェクトに "ImHere.Functions" という名前を付け、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-106">Name the project "ImHere.Functions", and then click **OK**.</span></span>

    ![[新しいプロジェクトの追加] ダイアログ](../media/5-add-new-functions-project.png)

1. <span data-ttu-id="7c44d-108">**[新しいプロジェクト]** 構成ダイアログが表示されます。これには更新されたテンプレートが読み込まれている間、左下にスピナーが表示される場合があります。</span><span class="sxs-lookup"><span data-stu-id="7c44d-108">The **New Project** configuration dialog will appear, and it may show a spinner in the bottom-left whilst loading updated templates.</span></span> <span data-ttu-id="7c44d-109">これが表示されたら、読み込みが完了するまでお待ちください。更新されたテンプレートが使用できるようになったら、**[更新]** オプションをクリックします。このオプションは最新の関数テンプレートを確実に取得できるようにするために表示されます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-109">If you see this, wait until this has finished loading, then if updated templates are available, click the **Refresh** option that will appear to ensure you get the latest Function templates.</span></span>

    ![最新のテンプレートを読み込んでいる新しいプロジェクト ダイアログ](../media/5-loading-templates.png)

1. <span data-ttu-id="7c44d-111">**[新しいプロジェクト]** 構成ダイアログで、Functions のバージョンが (_v1 (.NET Framework)_ **ではなく**) 確実に *Azure Functions v2 (.NET Standard)* に設定されているようにします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-111">In the **New Project** configuration dialog, ensure the Functions version is set to *Azure Functions v2 (.NET Standard)* (**NOT** _v1 (.NET Framework)_).</span></span> <span data-ttu-id="7c44d-112">*[Http トリガー]* を選択し、ストレージ アカウントを *[ストレージ エミュレーター]* に設定されているままにし、アクセス権を *[匿名]* に設定します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-112">Select *Http Trigger*, leave the storage account set to *Storage Emulator*, and set the access rights to *Anonymous*.</span></span> <span data-ttu-id="7c44d-113">次に、 **[OK]** をクリックします</span><span class="sxs-lookup"><span data-stu-id="7c44d-113">Then click **OK**.</span></span>

    ![Azure 関数のプロジェクト構成ダイアログ](../media/5-configure-trigger.png)

    <span data-ttu-id="7c44d-115">新しいプロジェクトが作成され、`Function1` という名前の既定の関数が与えられます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-115">The new project will be created and have a default function called `Function1`.</span></span>

> [!NOTE]
> <span data-ttu-id="7c44d-116">この関数は匿名アクセスで作成されました。</span><span class="sxs-lookup"><span data-stu-id="7c44d-116">This function was created with anonymous access.</span></span> <span data-ttu-id="7c44d-117">Azure に公開されると、URL を知っている人は誰でもこの関数を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-117">Once published to Azure, anybody who knows the URL will be able to call this function.</span></span> <span data-ttu-id="7c44d-118">実際のシナリオでは、[Azure App Service 認証](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true)または [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true) など、何らかの形式の認証でこれを保護します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-118">In a real-world scenario, you would protect this with some form of authentication, such as [Azure App Service authentication](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview?azure-portal=true) or [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c?azure-portal=true).</span></span>

## <a name="create-the-function"></a><span data-ttu-id="7c44d-119">関数を作成する</span><span class="sxs-lookup"><span data-stu-id="7c44d-119">Create the function</span></span>

<span data-ttu-id="7c44d-120">Azure Functions プロジェクトは `Function1` という名前のシングル HTTP トリガー関数を使用して作成されます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-120">The Azure Functions project is created with a single HTTP trigger function called `Function1`.</span></span> <span data-ttu-id="7c44d-121">HTTP トリガーでは HTTP 要求を使用して関数を呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-121">HTTP triggers allow you to invoke your functions using HTTP requests.</span></span> <span data-ttu-id="7c44d-122">関数自体は `Function1` クラスに静的 `Run` メソッドとして実装されます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-122">The function itself is implemented as a static `Run` method in the `Function1` class.</span></span>

1. <span data-ttu-id="7c44d-123">ソリューション エクスプローラーのファイルの名前を "Function1.cs" から "SendLocation.cs" に変更します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-123">Rename the file in Solution Explorer from "Function1.cs" to "SendLocation.cs".</span></span> <span data-ttu-id="7c44d-124">コード要素 `Function1` の参照の名前をすべて変更するように求められたら、**[はい]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-124">When prompted to rename all references to the code element `Function1`, click **Yes**.</span></span>

1. <span data-ttu-id="7c44d-125">属性の関数名を "SendLocation" に変更します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-125">Rename the function name in the attribute to "SendLocation".</span></span>

    ```cs
    [FunctionName("SendLocation")]
    ```

1. <span data-ttu-id="7c44d-126">ロガーに情報メッセージを書き込む最初の行を除き、関数の内容を削除します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-126">Delete the contents of the function, except the first line that writes an information message to the logger.</span></span>

    ```cs
    public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                             "get", "post",
                                                             Route = null)]HttpRequestMessage req,
                                                ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
    }
    ```

## <a name="create-a-class-to-share-data-between-the-mobile-app-and-function"></a><span data-ttu-id="7c44d-127">モバイル アプリと関数の間でデータを共有するクラスを作成する</span><span class="sxs-lookup"><span data-stu-id="7c44d-127">Create a class to share data between the mobile app and function</span></span>

<span data-ttu-id="7c44d-128">データが Azure 関数に送信されるとき、データは JSON として送信されます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-128">When data is sent to the Azure function, it will be sent as JSON.</span></span> <span data-ttu-id="7c44d-129">モバイル アプリによりデータが JSON にシリアル化され、関数により JSON から逆シリアル化されます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-129">The mobile app will serialize data into JSON and the function will deserialize from JSON.</span></span> <span data-ttu-id="7c44d-130">モバイル アプリと関数の間でこのデータの整合性を維持するためには、場所と電話番号データを保持するクラスを含む新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-130">To keep this data consistent between the mobile app and the function, create a new project that contains a class to hold the location and phone number data.</span></span> <span data-ttu-id="7c44d-131">このプロジェクトはアプリと関数によって参照されます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-131">This project will then be referenced by the app and function.</span></span>

1. <span data-ttu-id="7c44d-132">ソリューションを右クリックし、*[追加]、[新しいプロジェクト]* の順に選択して、`ImHere` ソリューションの下で新しいプロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-132">Create a new project under the `ImHere` solution by right-clicking on the solution and selecting *Add->New Project...*.</span></span>

1. <span data-ttu-id="7c44d-133">左側にあるツリーから、*[Visual C#]、[.NET Standard]* の順に選択し、中央のパネルから *[クラス ライブラリ (.NET Standard)]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-133">From the tree on the left-hand side, select *Visual C#->.NET Standard*, and then select *Class Library (.NET Standard)* from the panel in the center.</span></span>

1. <span data-ttu-id="7c44d-134">プロジェクトに "ImHere.Data" という名前を付け、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-134">Name the project "ImHere.Data", and then click **OK**.</span></span>

    ![[新しいプロジェクトの追加] ダイアログ](../media/5-add-new-net-standard-project.png)

1. <span data-ttu-id="7c44d-136">自動生成された "Class1.cs" ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-136">Delete the auto-generated "Class1.cs" file.</span></span>

1. <span data-ttu-id="7c44d-137">プロジェクトを右クリックし、*[追加]、[クラス]* の順に選択して、`PostData` という名前の `ImHere.Data` プロジェクトで新しいクラスを作成します。新しいクラスに "PostData" という名前を付け、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-137">Create a new class in the `ImHere.Data` project called `PostData` by right-clicking on the project and then selecting *Add->Class...*. Name the new class "PostData" and click **OK**.</span></span> <span data-ttu-id="7c44d-138">この新しいクラスを `public` としてマークします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-138">Mark this new class as `public`.</span></span>

1. <span data-ttu-id="7c44d-139">緯度と経度に `double` プロパティを、送信先の電話番号に `string[]` プロパティを追加します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-139">Add `double` properties for the latitude and longitude, as well as a `string[]` property for the phone numbers to send to.</span></span>

    ```cs
    public class PostData
    {
        public double Latitude { get; set; }
        public double Longitude { get; set; }
        public string[] ToNumbers { get; set; }
    }
    ```

1. <span data-ttu-id="7c44d-140">プロジェクトを右クリックし、*[追加]、[参照]* を選択して、このプロジェクトの参照を `ImHere.Functions` プロジェクトと `ImHere` プロジェクトの両方に追加します。左側にあるツリーから *[プロジェクト]* を選択し、*ImHere.Data* の隣にあるボックスをオンにします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-140">Add a reference to this project to both the `ImHere.Functions` and `ImHere` projects by right-clicking on the project and then selecting *Add->Reference...*. Select *Projects* from the tree on the left-hand side, and then check the box next to *ImHere.Data*.</span></span>

    ![プロジェクト参照を構成する](../media/5-configure-project-references.png)

## <a name="read-the-data-sent-to-the-function"></a><span data-ttu-id="7c44d-142">関数に送信されたデータを読み込む</span><span class="sxs-lookup"><span data-stu-id="7c44d-142">Read the data sent to the function</span></span>

<span data-ttu-id="7c44d-143">Azure 関数では、`req` パラメーターには行われた HTTP 要求が含まれます。この要求の中にあるデータは JSON でシリアル化された `PostData` オブジェクトになります。</span><span class="sxs-lookup"><span data-stu-id="7c44d-143">In the Azure function, the `req` parameter contains the HTTP request that was made and the data inside this request will be a JSON serialized `PostData` object.</span></span>

1. <span data-ttu-id="7c44d-144">`ImHere.Functions` プロジェクトで `SendLocation` クラスを開きます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-144">Open the `SendLocation` class in the `ImHere.Functions` project.</span></span>

1. <span data-ttu-id="7c44d-145">HTTP 要求の内容を文字列に読み込んでから、それを `PostData` オブジェクトに逆シリアル化します。これにより、`ImHere.Data` 名前空間に対して using ディレクティブが追加されます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-145">Read the contents of the HTTP request into a string, then deserialize it into a `PostData` object, adding a using directive for the `ImHere.Data` namespace.</span></span>

    ```cs
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    ```

1. <span data-ttu-id="7c44d-146">`PostData` からの緯度と経度を利用して Google Maps URL を作成します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-146">Construct a Google Maps URL using the latitude and longitude from the `PostData`.</span></span>

   ```cs
   string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
   ```

1. <span data-ttu-id="7c44d-147">URL をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-147">Log the URL.</span></span>

    ```cs
    log.LogInformation($"URL created - {url}");
    ```

1. <span data-ttu-id="7c44d-148">200 ステータス コードを返し、関数がエラーなく完了したことを示します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-148">Return a 200 status code to show the function completed without error.</span></span>

    ```cs
    return new OkResult();
    ```

<span data-ttu-id="7c44d-149">完全な関数を下に示します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-149">The complete function is shown below.</span></span>

```cs
[FunctionName("SendLocation")]
public static async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous,
                                                                "get", "post",
                                                         Route = null)]HttpRequest req,
                                                    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");
    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    PostData data = JsonConvert.DeserializeObject<PostData>(requestBody);
    string url = $"https://www.google.com/maps/search/?api=1&query={data.Latitude},{data.Longitude}";
    log.LogInformation($"URL created - {url}");
    return new OkResult();
}
```

## <a name="run-the-azure-function-locally"></a><span data-ttu-id="7c44d-150">Azure 関数をローカルで実行する</span><span class="sxs-lookup"><span data-stu-id="7c44d-150">Run the Azure function locally</span></span>

<span data-ttu-id="7c44d-151">関数は、ローカル ストレージ アカウントとローカル Azure Functions ランタイムを利用してローカルで実行できます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-151">Functions can be run locally using a local storage account and local Azure Functions runtime.</span></span> <span data-ttu-id="7c44d-152">このローカル ランタイムでは、Azure にデプロイする前に関数をテストできます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-152">This local runtime allows you to test out your function before deploying it to Azure.</span></span>

1. <span data-ttu-id="7c44d-153">ソリューション エクスプローラーで `ImHere.Functions` プロジェクトを右クリックし、*[スタートアップ プロジェクトに設定]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-153">Right-click on the `ImHere.Functions` project in the solution explorer, and then select *Set as StartUp project*.</span></span>

1. <span data-ttu-id="7c44d-154">*[デバッグ]* メニューから *[デバッグなしで開始]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-154">From the *Debug* menu, select *Start Without Debugging*.</span></span> <span data-ttu-id="7c44d-155">ローカル Azure Functions ランタイムはコンソール ウィンドウ内で起動され、これによってご利用の関数が開始され、`localhost` 上で利用できるポートがリッスンされます。</span><span class="sxs-lookup"><span data-stu-id="7c44d-155">The local Azure Functions runtime will launch inside a console window and start your function, listening on an available port on `localhost`.</span></span> <span data-ttu-id="7c44d-156">ファイアウォール アクセスを求めるダイアログが表示されたら、プライベート ネットワーク (既定のオプション) へのアクセスを許可します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-156">If you see a dialog asking for firewall access, allow access to private networks (the default option).</span></span>

    ![ローカルで実行される Azure 関数](../media/5-function-running-locally.png)

1. <span data-ttu-id="7c44d-158">関数がリッスンしているポートをメモします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-158">Take a note of the port that the function is listening on.</span></span> <span data-ttu-id="7c44d-159">これは次の演習でモバイル アプリをテストするために必要になります。</span><span class="sxs-lookup"><span data-stu-id="7c44d-159">You'll need this in the next unit to test out the mobile app.</span></span> <span data-ttu-id="7c44d-160">上記の図で、関数はポート **7071** でリッスンしています。</span><span class="sxs-lookup"><span data-stu-id="7c44d-160">In the image above, the function is listening on port **7071**.</span></span>

    ```sh
    Listening on http://localhost:7071/
    ```

1. <span data-ttu-id="7c44d-161">次の演習でモバイル アプリをテストできるように、関数は実行中のままにします。</span><span class="sxs-lookup"><span data-stu-id="7c44d-161">Leave the function running so that you can test the mobile app in the next unit.</span></span>

## <a name="summary"></a><span data-ttu-id="7c44d-162">まとめ</span><span class="sxs-lookup"><span data-stu-id="7c44d-162">Summary</span></span>

<span data-ttu-id="7c44d-163">この演習では、Visual Studio で Azure Functions プロジェクトを作成する方法を学習し、モバイル アプリと関数の間で共有するデータ オブジェクトを含む共有プロジェクトを追加し、渡されたデータを逆シリアル化する関数の基本実装を作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="7c44d-163">In this unit, you learned how to create an Azure Functions project in Visual Studio, added a shared project with a data object to be shared between the mobile app and the function, and learned how to create a basic implementation of the function to deserialize the data passed in.</span></span> <span data-ttu-id="7c44d-164">Azure 関数をローカルで実行する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="7c44d-164">You also learned how to run an Azure function locally.</span></span> <span data-ttu-id="7c44d-165">次の演習では、モバイル アプリから Azure 関数を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="7c44d-165">In the next unit, you'll call the Azure function from the mobile app.</span></span>