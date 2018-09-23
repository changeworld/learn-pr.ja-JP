<span data-ttu-id="29518-101">ここで作成しているアプリケーションは、フォト ギャラリーです。</span><span class="sxs-lookup"><span data-stu-id="29518-101">The application you're building is a photo gallery.</span></span> <span data-ttu-id="29518-102">このアプリケーションでは、クライアント側 JavaScript を使用して API を呼び出し、画像をアップロードして表示します。</span><span class="sxs-lookup"><span data-stu-id="29518-102">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="29518-103">このユニットでは、時間制限付き URL を生成して画像をアップロードするサーバーレス関数を使用して、API を作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-103">In this unit, you will create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="29518-104">この Web アプリケーションでは、[Blob Storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api) を使って Blob Storage に画像をアップロードするために、この URL が使用されます。</span><span class="sxs-lookup"><span data-stu-id="29518-104">The web application uses this URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="29518-105">画像用の Blob Storage コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="29518-105">Create a Blob storage container for images</span></span>

<span data-ttu-id="29518-106">アプリケーションには、画像をアップロードしてホストするために、別個のストレージ コンテナーが必要です。</span><span class="sxs-lookup"><span data-stu-id="29518-106">The application requires a separate storage container to upload and host images.</span></span>

<span data-ttu-id="29518-107">すべての BLOB へのパブリック アクセスを持つストレージ アカウント内に **images** という名前の新しいコンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-107">Create a new container in your Storage account named **images** in your Storage account with public access to all blobs.</span></span>

```azurecli
az storage container create \
    -n images \
    --account-name <storage account name> \
    --public-access blob
```

## <a name="create-a-function-app"></a><span data-ttu-id="29518-108">関数アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="29518-108">Create a function app</span></span>

<span data-ttu-id="29518-109">Azure Functions はサーバーレス関数を実行するためのサービスです。</span><span class="sxs-lookup"><span data-stu-id="29518-109">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="29518-110">サーバーレス関数は、HTTP 要求などのイベントによって、または BLOB がストレージ コンテナーに作成されたときに、トリガーする (呼び出す) ことができます。</span><span class="sxs-lookup"><span data-stu-id="29518-110">A serverless function can be triggered (called) by events, such as an HTTP request, or when a blob is created in a storage container.</span></span>

<span data-ttu-id="29518-111">関数アプリは、1 つ以上のサーバーレス関数のためのコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="29518-111">A function app is a container for one or more serverless functions.</span></span>

<span data-ttu-id="29518-112">一意の名前を指定して新しい関数アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-112">Create a new function app with a unique name.</span></span> <span data-ttu-id="29518-113">関数アプリには、ストレージ アカウントが必要です。</span><span class="sxs-lookup"><span data-stu-id="29518-113">Function apps require a Storage account.</span></span> <span data-ttu-id="29518-114">このユニットでは、前回のユニットで作成した既存のストレージ アカウントを使用します。</span><span class="sxs-lookup"><span data-stu-id="29518-114">In this unit, you will use the existing storage account you created in the last unit.</span></span>

```azurecli
az functionapp create \
    -n <function app name> \
    -g <rgn>[Sandbox resource group name]</rgn> \
    -s <storage account name> \
    --consumption-plan-location $(az group show -n <rgn>[Sandbox resource group name]</rgn> --query location --output tsv)
```

## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="29518-115">HTTP によってトリガーされるサーバーレス関数を作成する</span><span class="sxs-lookup"><span data-stu-id="29518-115">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="29518-116">画像を Blob Storage に安全にアップロードするため、フォト ギャラリー Web アプリでは、サーバーレス関数に HTTP 要求を行い、時間制限付き URL を生成します。</span><span class="sxs-lookup"><span data-stu-id="29518-116">To securely upload an image to Blob storage, the photo gallery web app makes an HTTP request to the serverless function to generate a time-limited URL.</span></span> <span data-ttu-id="29518-117">関数は HTTP 要求によってトリガーされます。この関数では Azure Storage SDK を使用し、セキュリティで保護された URL が生成され、返されます。</span><span class="sxs-lookup"><span data-stu-id="29518-117">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="29518-118">サンドボックスをアクティブ化したときと同じアカウントを使用して、[Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="29518-118">Sign into the [Azure portal](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) using the same account you activated the sandbox with.</span></span>

1. <span data-ttu-id="29518-119">**[検索]** ボックスを使用し、作成したばかりの関数アプリを検索します。</span><span class="sxs-lookup"><span data-stu-id="29518-119">Search for the function app you just created using the **Search** box.</span></span> <span data-ttu-id="29518-120">見つかったアプリをクリックして開きます。</span><span class="sxs-lookup"><span data-stu-id="29518-120">Click on the result to open it.</span></span>

    ![関数アプリを開く](../media/2-search-function-app.png)

1. <span data-ttu-id="29518-122">[Function App] ウィンドウの左側のナビゲーションで、**[関数]** をポイントし、プラス記号 (+) をクリックして新しいサーバーレス関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-122">In the left navigation of the Function Apps window, point to **Functions** and click the plus sign (+) to create a new serverless function.</span></span>

    ![新しい関数を作成する](../media/2-new-function.png)

1. <span data-ttu-id="29518-124">**[カスタム関数]** をクリックして、関数テンプレートの一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="29518-124">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="29518-125">**HttpTrigger** テンプレートを見つけて、C# または JavaScript をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-125">Find the **HttpTrigger** template and click C# or JavaScript.</span></span>

1. <span data-ttu-id="29518-126">次の値を使用して、BLOB のアップロード URL を生成する関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-126">Use the following values to create a function that generates a blob upload URL:</span></span>

    | <span data-ttu-id="29518-127">設定</span><span class="sxs-lookup"><span data-stu-id="29518-127">Setting</span></span>      |  <span data-ttu-id="29518-128">推奨値</span><span class="sxs-lookup"><span data-stu-id="29518-128">Suggested value</span></span>   | <span data-ttu-id="29518-129">説明</span><span class="sxs-lookup"><span data-stu-id="29518-129">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="29518-130">**言語**</span><span class="sxs-lookup"><span data-stu-id="29518-130">**Language**</span></span> | <span data-ttu-id="29518-131">C# または JavaScript</span><span class="sxs-lookup"><span data-stu-id="29518-131">C# or JavaScript</span></span> | <span data-ttu-id="29518-132">使用する言語を選びます。</span><span class="sxs-lookup"><span data-stu-id="29518-132">Select the language that you want to use.</span></span> |
    | <span data-ttu-id="29518-133">**関数名の指定**</span><span class="sxs-lookup"><span data-stu-id="29518-133">**Name your function**</span></span> | <span data-ttu-id="29518-134">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="29518-134">GetUploadUrl</span></span> | <span data-ttu-id="29518-135">アプリケーションから関数を検出できるように、この名前を、表示されているとおりに入力します。</span><span class="sxs-lookup"><span data-stu-id="29518-135">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="29518-136">**承認レベル**</span><span class="sxs-lookup"><span data-stu-id="29518-136">**Authorization level**</span></span> | <span data-ttu-id="29518-137">Anonymous</span><span class="sxs-lookup"><span data-stu-id="29518-137">Anonymous</span></span> | <span data-ttu-id="29518-138">関数にパブリックにアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="29518-138">Allows the function to be accessed publicly.</span></span> |

    ![HTTP によってトリガーされる新しい関数の設定を入力する](../media/2-new-function-httptrigger.png)

1. <span data-ttu-id="29518-140">**[作成]** をクリックして、関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-140">Click **Create** to create the function.</span></span>

<span data-ttu-id="29518-141">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="29518-141">::: zone pivot="csharp"</span></span>

8. <span data-ttu-id="29518-142">関数のソース コードが表示されたら、**run.csx** ファイル内のすべての内容を [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="29518-142">When the function's source code appears, replace all of the content in the **run.csx** file with the content in the [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx) file.</span></span>

1. <span data-ttu-id="29518-143">コード ウィンドウの下の **[ログ]** をクリックしてログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="29518-143">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="29518-144">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-144">Click **Save**.</span></span> <span data-ttu-id="29518-145">関数が正常にコンパイルされることを確実にするため、ログパネルをチェックします。</span><span class="sxs-lookup"><span data-stu-id="29518-145">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="29518-146">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29518-146">::: zone-end</span></span>

<span data-ttu-id="29518-147">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="29518-147">::: zone pivot="javascript"</span></span>

8. <span data-ttu-id="29518-148">この関数には、npm の `azure-storage` パッケージが必要です。</span><span class="sxs-lookup"><span data-stu-id="29518-148">This function requires the `azure-storage` package from npm.</span></span> <span data-ttu-id="29518-149">このパッケージによって、セキュリティで保護された URL を構築するために必要な Shared Access Signature (SAS) トークンが生成されます。</span><span class="sxs-lookup"><span data-stu-id="29518-149">The package generates the shared access signature (SAS) token that's required to build the secure URL.</span></span> <span data-ttu-id="29518-150">npm パッケージをインストールするには、左側のナビゲーションで Functions アプリをクリックし、**[プラットフォーム機能]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-150">To install the npm package, click on the Functions app on the left navigation and click **Platform features**.</span></span>

1. <span data-ttu-id="29518-151">**[コンソール]** をクリックしてコンソール ウィンドウを表示します。</span><span class="sxs-lookup"><span data-stu-id="29518-151">Click **Console** to reveal a console window.</span></span>

    ![コンソール ウィンドウを開く](../media/2-open-console.jpg)

1. <span data-ttu-id="29518-153">`cd d:\home\site\wwwroot` コマンドを実行し、現在のディレクトリを **d:\home\site\wwwroot** にします。</span><span class="sxs-lookup"><span data-stu-id="29518-153">Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

1. <span data-ttu-id="29518-154">`npm init -y` コマンドを実行し、空の **package.json** ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-154">Run the command `npm init -y` to create an empty **package.json** file.</span></span>

1. <span data-ttu-id="29518-155">パッケージをインストールするには、コンソールで `npm install --save azure-storage` コマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="29518-155">To install the package, run the command `npm install --save azure-storage` in the console.</span></span> <span data-ttu-id="29518-156">パッケージを **package.json** として保存します。</span><span class="sxs-lookup"><span data-stu-id="29518-156">Save the package as **package.json**.</span></span> <span data-ttu-id="29518-157">操作が完了するまで数分かかることがあります。</span><span class="sxs-lookup"><span data-stu-id="29518-157">It may take a few minutes to complete the operation.</span></span>

1. <span data-ttu-id="29518-158">左側のナビゲーションで関数 (**GetUploadUrl**) をクリックし、関数を表示します。</span><span class="sxs-lookup"><span data-stu-id="29518-158">Click on the function (**GetUploadUrl**) in the left navigation to reveal the function.</span></span> <span data-ttu-id="29518-159">**index.js** ファイルのすべての内容を、[**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) ファイルの内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="29518-159">Replace all of the content in the **index.js** file with the content in the [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js) file.</span></span>

    ![更新後の index.js の内容](../media/2-paste-js.jpg)

1. <span data-ttu-id="29518-161">コード ウィンドウの下の **[ログ]** をクリックしてログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="29518-161">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="29518-162">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-162">Click **Save**.</span></span> <span data-ttu-id="29518-163">関数が正常にコンパイルされることを確実にするため、ログパネルをチェックします。</span><span class="sxs-lookup"><span data-stu-id="29518-163">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="29518-164">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29518-164">::: zone-end</span></span>

<span data-ttu-id="29518-165">この関数により、Blob Storage にファイルをアップロードするために使用する共有アクセス署名 (SAS) URL が生成されます。</span><span class="sxs-lookup"><span data-stu-id="29518-165">The function generates a shared access signature (SAS) URL that's used to upload a file to Blob storage.</span></span> <span data-ttu-id="29518-166">SAS URL の有効期間は短く、この URL でアップロードが許可されるのは 1 つのファイルだけです。</span><span class="sxs-lookup"><span data-stu-id="29518-166">The SAS URL is valid for a short time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="29518-167">[共有アクセス署名の使用方法についてはこちら](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)の Blob Storage のドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="29518-167">Consult the Blob storage documentation for more information on [how to use shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="29518-168">ストレージ接続文字列に環境変数を追加する</span><span class="sxs-lookup"><span data-stu-id="29518-168">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="29518-169">作成した関数で SAS URL を生成できるようにするには、ストレージ アカウントの接続文字列が必要です。</span><span class="sxs-lookup"><span data-stu-id="29518-169">The function that you created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="29518-170">接続文字列は、関数本体でハードコーディングするのではなく、アプリケーション設定として格納できます。</span><span class="sxs-lookup"><span data-stu-id="29518-170">Instead of hardcoding the connection string in the function body, it can be stored as an application setting.</span></span> <span data-ttu-id="29518-171">アプリケーション設定には、関数アプリ内のすべての関数が環境変数としてアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="29518-171">Application settings are accessible as environment variables by all functions in the function app.</span></span>

1. <span data-ttu-id="29518-172">Cloud Shell で、ストレージ アカウント接続文字列のクエリを実行し、**STORAGE_CONNECTION_STRING** という名前の Bash 変数に保存します。</span><span class="sxs-lookup"><span data-stu-id="29518-172">In Cloud Shell, query the Storage account connection string and save it to a Bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="29518-173">変数が正常に設定されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29518-173">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="29518-174">前の手順で保存した値を使用して、**AZURE_STORAGE_CONNECTION_STRING** という名前の新しいアプリケーション設定を作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-174">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set \
        -n <function app name> \
        -g <rgn>[Sandbox resource group name]</rgn> \
        --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING \
        -o table
    ```

    <span data-ttu-id="29518-175">コマンドの出力に、正しい値が指定された新しいアプリケーション設定が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29518-175">Confirm that the command's output contains the new application setting with the correct value.</span></span>

## <a name="test-the-serverless-function"></a><span data-ttu-id="29518-176">サーバーレス関数をテストする</span><span class="sxs-lookup"><span data-stu-id="29518-176">Test the serverless function</span></span>

<span data-ttu-id="29518-177">Azure portal には、関数の作成と編集だけでなく、関数をテストするツールも組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="29518-177">In addition to creating and editing functions, the Azure portal also provides a built-in tool for testing functions.</span></span>

1. <span data-ttu-id="29518-178">HTTP サーバーレス関数をテストするには、コード ウィンドウの右側にある **[テスト]** タブをクリックして、テスト パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="29518-178">To test the HTTP serverless function, on the right of the code window, click on the **Test** tab to expand the test panel.</span></span>

1. <span data-ttu-id="29518-179">**[HTTP メソッド]** を **[GET]** に変更します。</span><span class="sxs-lookup"><span data-stu-id="29518-179">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="29518-180">**[クエリ]** の下の **[パラメーターの追加]** をクリックし、次のパラメーターを追加します。</span><span class="sxs-lookup"><span data-stu-id="29518-180">Under **Query**, click **Add parameter** and add the following parameter:</span></span>

    | <span data-ttu-id="29518-181">名前</span><span class="sxs-lookup"><span data-stu-id="29518-181">Name</span></span>      |  <span data-ttu-id="29518-182">値</span><span class="sxs-lookup"><span data-stu-id="29518-182">Value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="29518-183">**filename**</span><span class="sxs-lookup"><span data-stu-id="29518-183">**filename**</span></span> | <span data-ttu-id="29518-184">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="29518-184">image1.jpg</span></span> |

1. <span data-ttu-id="29518-185">テスト パネルで **[実行]** をクリックして、HTTP 要求を関数に送信します。</span><span class="sxs-lookup"><span data-stu-id="29518-185">In the test panel, click **Run** to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="29518-186">関数の出力にアップロード URL が返されます。</span><span class="sxs-lookup"><span data-stu-id="29518-186">The function returns an upload URL in the output.</span></span> <span data-ttu-id="29518-187">関数の実行は、ログ パネルに表示されます。</span><span class="sxs-lookup"><span data-stu-id="29518-187">The function execution appears in the Logs panel.</span></span>

    ![関数が正常に実行されたことを示すログ](../media/2-test-function.png)

## <a name="configure-cors-in-the-function-app"></a><span data-ttu-id="29518-189">関数アプリでの CORS を構成する</span><span class="sxs-lookup"><span data-stu-id="29518-189">Configure CORS in the function app</span></span>

<span data-ttu-id="29518-190">関数のフロントエンドは Blob Storage でホストされているため、そのドメイン名は関数アプリとは異なります。</span><span class="sxs-lookup"><span data-stu-id="29518-190">Because the function front end is hosted in Blob storage, it has a different domain name than the function app.</span></span> <span data-ttu-id="29518-191">作成した関数がクライアント側の JavaScript によって正常に呼び出されるように、関数アプリをクロス オリジン リソース共有 (CORS) 用に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29518-191">For the client-side JavaScript to successfully call the function that you created, the function app has to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="29518-192">関数アプリ ウィンドウの左側のナビゲーションで、関数アプリの名前をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-192">In the left navigation of the Function Apps window, click on the name of your function app.</span></span>

1. <span data-ttu-id="29518-193">**[プラットフォーム機能]** をクリックして、高度な機能の一覧を表示します。</span><span class="sxs-lookup"><span data-stu-id="29518-193">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="29518-194">**[API]** で **[CORS]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-194">Under **API**, click **CORS**.</span></span>

    ![CORS を選択する](../media/2-open-cors.jpg)

1. <span data-ttu-id="29518-196">前のユニットで作成した Web サイトの URL の許可されたオリジンを追加し、末尾のスラッシュ (/) を省略します。</span><span class="sxs-lookup"><span data-stu-id="29518-196">Add an allow origin for the URL of your web site you created in the previous unit and omit the trailing slash (/).</span></span> <span data-ttu-id="29518-197">たとえば、「`https://firstserverlessweb.z4.web.core.windows.net`」のように入力します。</span><span class="sxs-lookup"><span data-stu-id="29518-197">For example: `https://firstserverlessweb.z4.web.core.windows.net`.</span></span>

    ![追加されたサーバーレス Web アプリの URL を示す CORS 設定](../media/2-add-cors.png)

1. <span data-ttu-id="29518-199">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-199">Click **Save**.</span></span>

<span data-ttu-id="29518-200">::: zone pivot="javascript"</span><span class="sxs-lookup"><span data-stu-id="29518-200">::: zone pivot="javascript"</span></span>

6. <span data-ttu-id="29518-201">引き続き Azure portal で [Function App] ウィンドウに移動し、関数アプリの名前をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-201">Still in the Azure portal, navigate to the Function Apps window and click on the name of your function app.</span></span> <span data-ttu-id="29518-202">**[概要]** タブを選択します。**[再起動]** をクリックし、CORS への変更を有効にします。</span><span class="sxs-lookup"><span data-stu-id="29518-202">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

<span data-ttu-id="29518-203">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29518-203">::: zone-end</span></span>

<span data-ttu-id="29518-204">::: zone pivot="csharp"</span><span class="sxs-lookup"><span data-stu-id="29518-204">::: zone pivot="csharp"</span></span>

6. <span data-ttu-id="29518-205">`GetUploadUrl` 関数に戻り、**[統合]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="29518-205">Navigate back to the `GetUploadUrl` function and select the **Integrate** tab.</span></span>

1. <span data-ttu-id="29518-206">**[選択した HTTP メソッド]** で、**[OPTIONS]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="29518-206">Under **Selected HTTP methods**, select **OPTIONS**.</span></span>

    <span data-ttu-id="29518-207">**[GET]**、**[POST]**、**[OPTIONS]** がすべて選択されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="29518-207">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="29518-208">CORS では **OPTIONS** メソッドが使用されます。C# 関数の既定では、これは選択されていません。</span><span class="sxs-lookup"><span data-stu-id="29518-208">CORS uses the **OPTIONS** method, which isn't selected by default for C# functions.</span></span>  

1. <span data-ttu-id="29518-209">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-209">Click **Save**.</span></span>

1. <span data-ttu-id="29518-210">引き続き Azure portal で [Function App] ウィンドウに移動し、関数アプリの名前をクリックします。</span><span class="sxs-lookup"><span data-stu-id="29518-210">Still in the Azure portal, navigate to the Function Apps window and click on the name of your function app.</span></span> <span data-ttu-id="29518-211">**[概要]** タブを選択します。**[再起動]** をクリックし、CORS への変更を有効にします。</span><span class="sxs-lookup"><span data-stu-id="29518-211">Select the **Overview** tab. Click **Restart** to make sure that the changes for CORS take effect.</span></span>

<span data-ttu-id="29518-212">::: zone-end</span><span class="sxs-lookup"><span data-stu-id="29518-212">::: zone-end</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="29518-213">ストレージ アカウントで CORS を構成する</span><span class="sxs-lookup"><span data-stu-id="29518-213">Configure CORS in the Storage account</span></span>

<span data-ttu-id="29518-214">関数アプリでは Blob Storage に対してクライアント側 JavaScript 呼び出しを実行し、ファイルをアップロードするため、ストレージ アカウントを CORS 用に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="29518-214">Because the function app also makes client-side JavaScript calls to Blob storage to upload files, you have to configure the Storage account for CORS.</span></span>

- <span data-ttu-id="29518-215">次のコマンドを実行し、すべてのオリジンでファイルをストレージ アカウントにアップロードできるようにします。</span><span class="sxs-lookup"><span data-stu-id="29518-215">Run the following command to allow all origins to upload files to the Storage account:</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```

## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="29518-216">Web アプリを変更して画像をアップロードする</span><span class="sxs-lookup"><span data-stu-id="29518-216">Modify the web app to upload images</span></span>

<span data-ttu-id="29518-217">Web アプリでは、**settings.js** という名前のファイルから設定を取得します。</span><span class="sxs-lookup"><span data-stu-id="29518-217">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="29518-218">次の手順では、Cloud Shell を使用してファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-218">In the following steps, you create the file using Cloud Shell.</span></span> <span data-ttu-id="29518-219">`window.apiBaseUrl` を関数アプリの URL に設定し、`window.blobBaseUrl` を Azure Blob Storage エンドポイントの URL に設定します。</span><span class="sxs-lookup"><span data-stu-id="29518-219">You set `window.apiBaseUrl` to the URL of the function app, and `window.blobBaseUrl` to the URL of the Azure Blob storage endpoint.</span></span>

1. <span data-ttu-id="29518-220">Cloud Shell で、現在のディレクトリが確実に **www/dist** フォルダーであるようにします。</span><span class="sxs-lookup"><span data-stu-id="29518-220">In Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```bash
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="29518-221">コマンド `code .` を入力し (ドットも含めます)、現在のディレクトリで Cloud Shell Editor を開きます。</span><span class="sxs-lookup"><span data-stu-id="29518-221">Open the Cloud Shell Editor in the current directory by typing the command `code .`, including the dot.</span></span>

    ```bash
    code .
    ```

1. <span data-ttu-id="29518-222">エディターの下の Cloud Shell ウィンドウで、関数アプリの URL を問い合わせます。</span><span class="sxs-lookup"><span data-stu-id="29518-222">In the Cloud Shell window below the editor, query the function app's URL.</span></span>

    ```azurecli
    echo "https://"$(az functionapp show -n <function app name> -g <rgn>[Sandbox resource group name]</rgn> --query "defaultHostName" --output tsv)
    ```

1. <span data-ttu-id="29518-223">前の手順で取得した関数アプリ URL を利用し、次の行をエディター ウィンドウに追加します。</span><span class="sxs-lookup"><span data-stu-id="29518-223">Add the following line into the editor window, using the function app URL you retrieved in the previous step.</span></span>

    ```bash
    window.apiBaseUrl = '<function app url>'
    ```

1. <span data-ttu-id="29518-224">エディターの下の Cloud Shell ウィンドウで、Azure Blob Storage エンドポイントの URL を問い合わせます。</span><span class="sxs-lookup"><span data-stu-id="29518-224">In the Cloud Shell window below the editor, query the Azure Blob Storage endpoint URL.</span></span>

    ```azurecli
    echo $(az storage account show -n <storage account name> -g <rgn>[Sandbox resource group name]</rgn> --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

1. <span data-ttu-id="29518-225">前の手順で取得したストレージ エンドポイント URL を利用し、2 つ目の行をエディター ウィンドウに追加します。</span><span class="sxs-lookup"><span data-stu-id="29518-225">Append a second line into the editor window, using the Storage endpoint URL you retrieved in the previous step.</span></span>

    ```bash
    window.blobBaseUrl = '<blob storage endpoint url>'
    ```

1. <span data-ttu-id="29518-226">**settings.js** としてファイルを保存し、エディターを閉じます。</span><span class="sxs-lookup"><span data-stu-id="29518-226">Save the file as **settings.js** and close the editor.</span></span>

1. <span data-ttu-id="29518-227">ファイルが正常に書き込まれたこと、2 行が含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29518-227">Confirm the file was successfully written and it now contains 2 lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="29518-228">ファイルを Blob Storage にアップロードします。</span><span class="sxs-lookup"><span data-stu-id="29518-228">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload \
        -c \$web \
        --account-name <storage account name> \
        -f settings.js \
        -n settings.js
    ```

## <a name="test-the-web-application"></a><span data-ttu-id="29518-229">Web アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="29518-229">Test the web application</span></span>

<span data-ttu-id="29518-230">この時点で、ギャラリー アプリケーションでは Blob Storage に画像をアップロードできますが、まだ画像は表示できません。</span><span class="sxs-lookup"><span data-stu-id="29518-230">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="29518-231">`GetImages` 関数の呼び出しが試行されますが、この関数はまだ存在しません。これは後のモジュールで作成します。</span><span class="sxs-lookup"><span data-stu-id="29518-231">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="29518-232">この呼び出しは失敗し、Web ページには "分析中…" と表示されたままになりますが、選択する画像は正常にアップロードされます。</span><span class="sxs-lookup"><span data-stu-id="29518-232">The call will fail and the web page will appear to be stuck on "Analyzing...," but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="29518-233">画像が正常にアップロードされていることを確認するには、Azure portal で **images** コンテナーの内容を確認します。</span><span class="sxs-lookup"><span data-stu-id="29518-233">You can verify that an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="29518-234">ブラウザー ウィンドウで、アプリケーションを参照します。</span><span class="sxs-lookup"><span data-stu-id="29518-234">In a browser window, browse to the application.</span></span> <span data-ttu-id="29518-235">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="29518-235">Select an image file and upload it.</span></span> <span data-ttu-id="29518-236">アップロードが完了しますが、画像を表示する機能はまだ追加していないため、アップロードした写真はアプリに表示されません </span><span class="sxs-lookup"><span data-stu-id="29518-236">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="29518-237">(Web ページには "画像分析中..." と表示されたままになります。これは後で修正します)。</span><span class="sxs-lookup"><span data-stu-id="29518-237">(The web page appears to be stuck on "Analyzing image..." You'll fix that later.)</span></span>

1. <span data-ttu-id="29518-238">Cloud Shell で、**images** コンテナーに画像がアップロードされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="29518-238">In Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list \
        --account-name <storage account name> \
        -c images \
        -o table
    ```

1. <span data-ttu-id="29518-239">次のユニットに進む前に、**images** コンテナー内のすべてのファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="29518-239">Before moving on to the next unit, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch \
        -s images \
        --account-name <storage account name>
    ```

## <a name="summary"></a><span data-ttu-id="29518-240">まとめ</span><span class="sxs-lookup"><span data-stu-id="29518-240">Summary</span></span>

<span data-ttu-id="29518-241">このユニットでは、Azure Functions アプリを作成しました。また、サーバーレス関数を使用して、Web アプリケーションで画像を Blob Storage にアップロードできるようにする方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="29518-241">In this unit, you created an Azure Functions app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="29518-242">次は、BLOB によってトリガーされるサーバーレス関数を使用して、アップロードされた画像のサムネイルを作成する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="29518-242">Next, you will learn how to create thumbnails for the uploaded images using a blob-triggered serverless function.</span></span>