<span data-ttu-id="6b780-101">Azure Cosmos DB は、Microsoft のサーバーレスなグローバル分散型マルチモデル データベースです。</span><span class="sxs-lookup"><span data-stu-id="6b780-101">Azure Cosmos DB is Microsoft's serverless, globally distributed, multi-model database.</span></span> <span data-ttu-id="6b780-102">このモジュールでは、Azure Functions を使用して、画像のメタデータを JSON ドキュメントとして Azure Cosmos DB に格納および取得する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6b780-102">In this module, you learn how to use Azure Functions to store and retrieve image metadata as JSON documents in Azure Cosmos DB.</span></span>

## <a name="create-an-azure-cosmos-db-account-database-and-collection"></a><span data-ttu-id="6b780-103">Azure Cosmos DB アカウント、データベース、およびコレクションを作成する</span><span class="sxs-lookup"><span data-stu-id="6b780-103">Create an Azure Cosmos DB account, database, and collection</span></span>

<span data-ttu-id="6b780-104">Azure Cosmos DB アカウントは、Azure Cosmos DB データベースを含む Azure リソースです。</span><span class="sxs-lookup"><span data-stu-id="6b780-104">An Azure Cosmos DB account is an Azure resource that contains Azure Cosmos DB databases.</span></span>

1. <span data-ttu-id="6b780-105">Cloud Shell に引き続きサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6b780-105">Ensure you're still signed into Cloud Shell.</span></span> <span data-ttu-id="6b780-106">そうでない場合は、**[Enter focus mode]\(フォーカス モードにする\)** を選択して、Cloud Shell ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b780-106">If you aren't, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="6b780-107">このチュートリアルの他のリソースと同じリソース グループ内で、一意の名前の Azure Cosmos DB アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-107">Create an Azure Cosmos DB account with a unique name in the same resource group as the other resources in this tutorial.</span></span>

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. <span data-ttu-id="6b780-108">Azure Cosmos DB アカウントが作成されたら、そのアカウントに **imagesdb** という名前の新しいデータベースを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-108">After the Azure Cosmos DB account is created, create a new database named **imagesdb** in the account.</span></span>

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. <span data-ttu-id="6b780-109">データベースが作成されたら、400 要求ユニット (RU) のスループットで、そのデータベースに **images** という名前の新しいコレクションを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-109">After the database is created, create a new collection named **images** in the database with a throughput of 400 request units (RUs).</span></span>

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-azure-cosmos-db-when-a-thumbnail-is-created"></a><span data-ttu-id="6b780-110">サムネイルの作成時にドキュメントを Azure Cosmos DB に保存する</span><span class="sxs-lookup"><span data-stu-id="6b780-110">Save a document to Azure Cosmos DB when a thumbnail is created</span></span>

<span data-ttu-id="6b780-111">Azure Cosmos DB の出力バインディングを使用すると、Azure Functions から Azure Cosmos DB コレクション内にドキュメントを作成できます。</span><span class="sxs-lookup"><span data-stu-id="6b780-111">The Azure Cosmos DB output binding lets you create documents in an Azure Cosmos DB collection from Azure Functions.</span></span> <span data-ttu-id="6b780-112">次の手順で、**ResizeImage** 関数の Azure Cosmos DB 出力バインディングを構成し、保存するドキュメント (オブジェクト) を返すようにこの関数を変更します。</span><span class="sxs-lookup"><span data-stu-id="6b780-112">In the following steps, you configure an Azure Cosmos DB output binding in the **ResizeImage** function and modify the function to return a document (object) to be saved.</span></span>

1. <span data-ttu-id="6b780-113">Azure portal で関数アプリを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b780-113">Open the function app in the Azure portal.</span></span>

1. <span data-ttu-id="6b780-114">左側のナビゲーションで、**ResizeImage** 関数を展開し、**[統合]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-114">In the left navigation, expand the **ResizeImage** function, and then select **Integrate**.</span></span>

1. <span data-ttu-id="6b780-115">**[出力]** で **[新しい出力]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-115">Under **Outputs**, click **New Output**.</span></span>

1. <span data-ttu-id="6b780-116">**[Azure Cosmos DB]** 項目を見つけて選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-116">Find the **Azure Cosmos DB** item and select it.</span></span> <span data-ttu-id="6b780-117">次に **[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-117">Then click **Select**.</span></span>

    ![[新しい出力] を選択する](../media/4-new-output.jpg)

1. <span data-ttu-id="6b780-119">**[Azure Cosmos DB output]\(Azure Cosmos DB 出力\)** の各フィールドに次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="6b780-119">Fill out the fields under **Azure Cosmos DB output** with the following values.</span></span>

    | <span data-ttu-id="6b780-120">設定</span><span class="sxs-lookup"><span data-stu-id="6b780-120">Setting</span></span>      |  <span data-ttu-id="6b780-121">推奨値</span><span class="sxs-lookup"><span data-stu-id="6b780-121">Suggested value</span></span>   | <span data-ttu-id="6b780-122">説明</span><span class="sxs-lookup"><span data-stu-id="6b780-122">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6b780-123">**ドキュメント パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="6b780-123">**Document parameter name**</span></span> | <span data-ttu-id="6b780-124">**[関数の戻り値を使用する]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-124">Select **Use function return value**.</span></span> | <span data-ttu-id="6b780-125">ボックス内の値が自動的に **$return** に設定されます。</span><span class="sxs-lookup"><span data-stu-id="6b780-125">The value in the box is automatically set to **$return**.</span></span> |
    | <span data-ttu-id="6b780-126">**データベース名**</span><span class="sxs-lookup"><span data-stu-id="6b780-126">**Database name**</span></span> | <span data-ttu-id="6b780-127">imagesdb</span><span class="sxs-lookup"><span data-stu-id="6b780-127">imagesdb</span></span> | <span data-ttu-id="6b780-128">作成したデータベースの名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="6b780-128">Use the name of the database that you created.</span></span> |
    | <span data-ttu-id="6b780-129">**コレクション名**</span><span class="sxs-lookup"><span data-stu-id="6b780-129">**Collection name**</span></span> | <span data-ttu-id="6b780-130">images</span><span class="sxs-lookup"><span data-stu-id="6b780-130">images</span></span> | <span data-ttu-id="6b780-131">作成したコレクションの名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="6b780-131">Use the name of the collection that you created.</span></span> |

1. <span data-ttu-id="6b780-132">**[Azure Cosmos DB アカウント接続]** の横にある **[新規]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-132">Next to **Azure Cosmos DB account connection**, click **new**.</span></span> <span data-ttu-id="6b780-133">以前に作成した Azure Cosmos DB アカウントを選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-133">Select the Azure Cosmos DB account that you previously created.</span></span>

    ![Azure Cosmos DB 出力バインディング用の設定を入力する](../media/4-cosmos-db-output.png)

1. <span data-ttu-id="6b780-135">**[保存]** をクリックして、Azure Cosmos DB 出力バインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-135">Click **Save** to create the Azure Cosmos DB output binding.</span></span>

1. <span data-ttu-id="6b780-136">左側の関数名 **ResizeImage** をクリックして、関数を開きます。</span><span class="sxs-lookup"><span data-stu-id="6b780-136">Click on the **ResizeImage** function name on the left to open the function.</span></span>

1. <span data-ttu-id="6b780-137">**C#**</span><span class="sxs-lookup"><span data-stu-id="6b780-137">**C#**</span></span>

    1. <span data-ttu-id="6b780-138">(C#) 関数の戻り値の型を **void** から **object** に変更します。</span><span class="sxs-lookup"><span data-stu-id="6b780-138">(C#) Change the return type of the function from **void** to **object**.</span></span>

    1. <span data-ttu-id="6b780-139">(C#) 関数の末尾に次のコード ブロックを追加して、保存するドキュメントを返すようにします。</span><span class="sxs-lookup"><span data-stu-id="6b780-139">(C#) At the end of the function, add the following code block to return the document to be saved:</span></span>
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![変更後の ResizeImages 関数用の run.csx](../media/4-update-function.png)

1. <span data-ttu-id="6b780-141">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6b780-141">**JavaScript**</span></span>

    <span data-ttu-id="6b780-142">(JavaScript) `else` 句の `context.done()` ステートメントを変更して、Azure Cosmos DB に保存するドキュメントを返すようにします。</span><span class="sxs-lookup"><span data-stu-id="6b780-142">(JavaScript) Change the `context.done()` statement in the `else` clause to return the document to be saved to Azure Cosmos DB.</span></span>

    ```javascript
    if (error) {
        context.done(error);
    } else {
        context.bindings.thumbnail = stream;
        context.done(null, {
            id: context.bindingData.name,
            imgPath: "/images/" + context.bindingData.name,
            thumbnailPath: "/thumbnails/" + context.bindingData.name
        });
    }
    ```

1. <span data-ttu-id="6b780-143">コード ウィンドウの下の **[ログ]** をクリックして、ログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="6b780-143">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="6b780-144">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-144">Click **Save**.</span></span> <span data-ttu-id="6b780-145">関数が正常に保存され、エラーがないことをログ パネルで確認します。</span><span class="sxs-lookup"><span data-stu-id="6b780-145">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="create-a-function-to-list-images-from-azure-cosmos-db"></a><span data-ttu-id="6b780-146">Azure Cosmos DB の画像を一覧表示する関数を作成する</span><span class="sxs-lookup"><span data-stu-id="6b780-146">Create a function to list images from Azure Cosmos DB</span></span>

<span data-ttu-id="6b780-147">Web アプリケーションでは、Azure Cosmos DB から画像のメタデータを取得するための API が必要です。</span><span class="sxs-lookup"><span data-stu-id="6b780-147">The web application requires an API to retrieve image metadata from Azure Cosmos DB.</span></span> <span data-ttu-id="6b780-148">次の手順では、Azure Cosmos DB 入力バインディングを使用してデータベース コレクションを照会する、HTTP によってトリガーされる関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-148">In the following steps, you create an HTTP-triggered function that uses an Azure Cosmos DB input binding to query the database collection.</span></span>

1. <span data-ttu-id="6b780-149">関数アプリで、左側の **[関数]** をポイントしてプラス記号 (+) をクリックし、新しい関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-149">In your function app, point to **Functions** on the left and click the plus sign (+) to create a new function.</span></span>

1. <span data-ttu-id="6b780-150">**HttpTrigger** テンプレートを検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-150">Find the **HttpTrigger** template and select it.</span></span>

1. <span data-ttu-id="6b780-151">以下の値を使用して、取得する画像の URL を生成する関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-151">Use these values to create a function that generates a get images URL:</span></span>

    | <span data-ttu-id="6b780-152">設定</span><span class="sxs-lookup"><span data-stu-id="6b780-152">Setting</span></span>      |  <span data-ttu-id="6b780-153">推奨値</span><span class="sxs-lookup"><span data-stu-id="6b780-153">Suggested value</span></span>   | <span data-ttu-id="6b780-154">説明</span><span class="sxs-lookup"><span data-stu-id="6b780-154">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6b780-155">**関数名の指定**</span><span class="sxs-lookup"><span data-stu-id="6b780-155">**Name your function**</span></span> | <span data-ttu-id="6b780-156">GetImages</span><span class="sxs-lookup"><span data-stu-id="6b780-156">GetImages</span></span> | <span data-ttu-id="6b780-157">アプリケーションから関数を検出できるように、この名前を、表示されているとおりに入力します。</span><span class="sxs-lookup"><span data-stu-id="6b780-157">Enter this name exactly as shown, so the application can discover the function.</span></span> |
    | <span data-ttu-id="6b780-158">**承認レベル**</span><span class="sxs-lookup"><span data-stu-id="6b780-158">**Authorization level**</span></span> | <span data-ttu-id="6b780-159">Anonymous</span><span class="sxs-lookup"><span data-stu-id="6b780-159">Anonymous</span></span> | <span data-ttu-id="6b780-160">関数にパブリックにアクセスできるようにします。</span><span class="sxs-lookup"><span data-stu-id="6b780-160">Allow the function to be accessed publicly.</span></span> |

1. <span data-ttu-id="6b780-161">**[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-161">Click **Create**.</span></span>

1. <span data-ttu-id="6b780-162">新しい関数が作成されたら、左側のナビゲーションの関数名の下にある **[統合]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-162">When the new function is created, click **Integrate** under the function name on the left navigation.</span></span>

1. <span data-ttu-id="6b780-163">**[新しい入力]** をクリックして、**[Azure Cosmos DB]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-163">Click **New Input** and select **Azure Cosmos DB**.</span></span> 

    ![[新しい入力] を選択する](../media/4-new-input.jpg)

1. <span data-ttu-id="6b780-165">**[選択]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-165">Click **Select**.</span></span>

1. <span data-ttu-id="6b780-166">次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="6b780-166">Fill out the following values:</span></span>

    | <span data-ttu-id="6b780-167">設定</span><span class="sxs-lookup"><span data-stu-id="6b780-167">Setting</span></span>      |  <span data-ttu-id="6b780-168">推奨値</span><span class="sxs-lookup"><span data-stu-id="6b780-168">Suggested value</span></span>   | <span data-ttu-id="6b780-169">説明</span><span class="sxs-lookup"><span data-stu-id="6b780-169">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6b780-170">**ドキュメント パラメーター名**</span><span class="sxs-lookup"><span data-stu-id="6b780-170">**Document parameter name**</span></span> | <span data-ttu-id="6b780-171">documents</span><span class="sxs-lookup"><span data-stu-id="6b780-171">documents</span></span> | <span data-ttu-id="6b780-172">関数内のパラメーター名と一致します。</span><span class="sxs-lookup"><span data-stu-id="6b780-172">Matches the parameter name in the function.</span></span> |
    | <span data-ttu-id="6b780-173">**データベース名**</span><span class="sxs-lookup"><span data-stu-id="6b780-173">**Database name**</span></span> | <span data-ttu-id="6b780-174">imagesdb</span><span class="sxs-lookup"><span data-stu-id="6b780-174">imagesdb</span></span> |  |
    | <span data-ttu-id="6b780-175">**コレクション名**</span><span class="sxs-lookup"><span data-stu-id="6b780-175">**Collection name**</span></span> | <span data-ttu-id="6b780-176">images</span><span class="sxs-lookup"><span data-stu-id="6b780-176">images</span></span> |  |
    | <span data-ttu-id="6b780-177">**SQL クエリ**</span><span class="sxs-lookup"><span data-stu-id="6b780-177">**SQL query**</span></span> | <span data-ttu-id="6b780-178">select \* from c order by c._ts desc</span><span class="sxs-lookup"><span data-stu-id="6b780-178">select \* from c order by c._ts desc</span></span> | <span data-ttu-id="6b780-179">ドキュメントを取得します。最新のドキュメントが最初に取得されます。</span><span class="sxs-lookup"><span data-stu-id="6b780-179">Get documents, latest documents first.</span></span> |
    | <span data-ttu-id="6b780-180">**Azure Cosmos DB アカウント接続**</span><span class="sxs-lookup"><span data-stu-id="6b780-180">**Azure Cosmos DB account connection**</span></span> | <span data-ttu-id="6b780-181">既存の接続文字列を選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-181">Select existing the connection string.</span></span> |  |

1. <span data-ttu-id="6b780-182">**[保存]** をクリックして入力バインディングを作成します。</span><span class="sxs-lookup"><span data-stu-id="6b780-182">Click **Save** to create the input binding.</span></span>

1. <span data-ttu-id="6b780-183">**C#**</span><span class="sxs-lookup"><span data-stu-id="6b780-183">**C#**</span></span>

    <span data-ttu-id="6b780-184">関数名をクリックしてコード ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b780-184">Click the function name to open the code window.</span></span> <span data-ttu-id="6b780-185">**run.csx** ファイルのすべてを、[**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx) ファイル内の内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6b780-185">Replace all of the **run.csx** file with the content in the [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx) file.</span></span>

1. <span data-ttu-id="6b780-186">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6b780-186">**JavaScript**</span></span>

    <span data-ttu-id="6b780-187">関数名をクリックしてコード ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b780-187">Click the function name to open the code window.</span></span> <span data-ttu-id="6b780-188">**index.js** ファイルのすべてを、[**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js) ファイル内の内容で置き換えます。</span><span class="sxs-lookup"><span data-stu-id="6b780-188">Replace all of the **index.js** file with the content in the [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js) file.</span></span>

1. <span data-ttu-id="6b780-189">コード ウィンドウの下の **[ログ]** をクリックして、ログ パネルを展開します。</span><span class="sxs-lookup"><span data-stu-id="6b780-189">Click **Logs** below the code window to expand the Logs panel.</span></span>

1. <span data-ttu-id="6b780-190">**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6b780-190">Click **Save**.</span></span> <span data-ttu-id="6b780-191">関数が正常に保存され、エラーがないことをログ パネルで確認します。</span><span class="sxs-lookup"><span data-stu-id="6b780-191">Check the Logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="6b780-192">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="6b780-192">Test the application</span></span>

1. <span data-ttu-id="6b780-193">ブラウザーでアプリケーションを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b780-193">Open the application in a browser.</span></span> <span data-ttu-id="6b780-194">画像ファイルを選択してアップロードします。</span><span class="sxs-lookup"><span data-stu-id="6b780-194">Select an image file and upload it.</span></span>

1. <span data-ttu-id="6b780-195">数秒後に、新しい画像のサムネイルがページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="6b780-195">After a few seconds, the thumbnail of the new image appears on the page.</span></span>

1. <span data-ttu-id="6b780-196">Azure Portal の **[検索]** ボックスを使用して、Azure Cosmos DB アカウントを名前で検索します。</span><span class="sxs-lookup"><span data-stu-id="6b780-196">In the Azure portal, use the **Search** box to search for your Azure Cosmos DB account by name.</span></span> <span data-ttu-id="6b780-197">名前をクリックしてアカウントを開きます。</span><span class="sxs-lookup"><span data-stu-id="6b780-197">Click on the name to open the account.</span></span>

1. <span data-ttu-id="6b780-198">左側の **[データ エクスプローラー]** をクリックし、コレクションとドキュメントを参照します。</span><span class="sxs-lookup"><span data-stu-id="6b780-198">Click **Data Explorer** on the left to browse collections and documents.</span></span>

1. <span data-ttu-id="6b780-199">**imagesdb** データベースの下で、**images** コレクションを選択します。</span><span class="sxs-lookup"><span data-stu-id="6b780-199">Under the **imagesdb** database, select the **images** collection.</span></span>

1. <span data-ttu-id="6b780-200">アップロードした画像に対してドキュメントが作成されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6b780-200">Confirm that a document was created for the uploaded image.</span></span>

    ![アップロードした画像に対するドキュメントが表示されているデータ エクスプローラー](../media/4-data-explorer.png)



## <a name="summary"></a><span data-ttu-id="6b780-202">まとめ</span><span class="sxs-lookup"><span data-stu-id="6b780-202">Summary</span></span>

<span data-ttu-id="6b780-203">このユニットでは、Azure Cosmos DB アカウント、データベース、およびコレクションを作成する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="6b780-203">In this unit, you learned how to create an Azure Cosmos DB account, database, and collection.</span></span> <span data-ttu-id="6b780-204">Azure Cosmos DB バインディングを使用して、Azure Cosmos DB コレクション内に画像のメタデータを保存し、取得する方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="6b780-204">You also learned how to use the Azure Cosmos DB bindings to save and retrieve image metadata in the Azure Cosmos DB collection.</span></span> <span data-ttu-id="6b780-205">次は、Microsoft Cognitive Services を使用して、アップロードした各画像のキャプションを自動的に生成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="6b780-205">Next, you learn how to automatically generate a caption for each uploaded image using Microsoft Cognitive Services.</span></span>