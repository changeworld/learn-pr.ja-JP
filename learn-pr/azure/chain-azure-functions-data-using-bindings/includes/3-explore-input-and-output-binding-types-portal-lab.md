<span data-ttu-id="0d803-101">この演習で作成するデモの概要図を次に示します。</span><span class="sxs-lookup"><span data-stu-id="0d803-101">The following is a high-level illustration of what we're going to build in this exercise.</span></span>

![HTTP 要求と HTTP 応答、およびそれぞれの req バインディング パラメーターと res バインディング パラメーターを示す、既定の HTTP トリガーの視覚的な表現。](../media-draft/default-http-trigger-visual-small.PNG)

<span data-ttu-id="0d803-103">HTTP 要求を受信すると起動し、メッセージを送信することで各要求に応答する関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="0d803-103">We'll create a function that will start when it receives an HTTP request and will respond to each request by sending back a message.</span></span> <span data-ttu-id="0d803-104">パラメーター `req` と `res` は、それぞれトリガー バインディングと出力バインディングです。</span><span class="sxs-lookup"><span data-stu-id="0d803-104">The parameters `req` and `res` are the trigger binding and output binding respectively.</span></span> <span data-ttu-id="0d803-105">さあ、始めましょう。</span><span class="sxs-lookup"><span data-stu-id="0d803-105">Let's get going!</span></span>

<span data-ttu-id="0d803-106">Azure アカウントで Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) にサインインします。</span><span class="sxs-lookup"><span data-stu-id="0d803-106">Sign in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with your Azure account.</span></span>

### <a name="create-a-function-app"></a><span data-ttu-id="0d803-107">関数アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="0d803-107">Create a function app</span></span>

<span data-ttu-id="0d803-108">このモジュール全体で使用する関数アプリを作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="0d803-108">Let's create a function app that we'll use throughout this entire module.</span></span> <span data-ttu-id="0d803-109">関数アプリを使用すると、リソースの管理、デプロイ、および共有を容易にするための論理ユニットとして関数をグループ化できます。</span><span class="sxs-lookup"><span data-stu-id="0d803-109">A function app lets you group functions as a logical unit for easier management, deployment, and sharing of resources.</span></span>

[!INCLUDE [resource-group-note](./rg-notice.md)]

1. <span data-ttu-id="0d803-110">Azure portal の左上隅にある **[リソースの作成]** ボタンを選択し、**[コンピューティング]** > **[Function App]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="0d803-110">Select the **Create a resource** button found on the upper left-hand corner of the Azure portal, then select **Compute** > **Function App**.</span></span>
1. <span data-ttu-id="0d803-111">関数アプリのプロパティを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="0d803-111">Set the function app properties as follows:</span></span>


    | <span data-ttu-id="0d803-112">プロパティ</span><span class="sxs-lookup"><span data-stu-id="0d803-112">Property</span></span>      | <span data-ttu-id="0d803-113">推奨値</span><span class="sxs-lookup"><span data-stu-id="0d803-113">Suggested value</span></span>  | <span data-ttu-id="0d803-114">説明</span><span class="sxs-lookup"><span data-stu-id="0d803-114">Description</span></span>                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="0d803-115">**アプリ名**</span><span class="sxs-lookup"><span data-stu-id="0d803-115">**App name**</span></span> | <span data-ttu-id="0d803-116">グローバルに一意の名前</span><span class="sxs-lookup"><span data-stu-id="0d803-116">Globally unique name</span></span> | <span data-ttu-id="0d803-117">新しい関数アプリを識別する名前。</span><span class="sxs-lookup"><span data-stu-id="0d803-117">Name that identifies your new function app.</span></span> <span data-ttu-id="0d803-118">有効な文字は、`a-z`、`0-9`、および `-` です。</span><span class="sxs-lookup"><span data-stu-id="0d803-118">Valid characters are `a-z`, `0-9`, and `-`.</span></span>  | 
    | <span data-ttu-id="0d803-119">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="0d803-119">**Subscription**</span></span> | <span data-ttu-id="0d803-120">ご利用のサブスクリプション</span><span class="sxs-lookup"><span data-stu-id="0d803-120">Your subscription</span></span> | <span data-ttu-id="0d803-121">この新しい関数アプリが作成されるサブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="0d803-121">The subscription under which this new function app is created.</span></span> | 
    | <span data-ttu-id="0d803-122">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="0d803-122">**Resource Group**</span></span>|  [!INCLUDE [resource-group-name](./rg-name.md)] | <span data-ttu-id="0d803-123">関数アプリを作成するための新しいリソース グループの名前。</span><span class="sxs-lookup"><span data-stu-id="0d803-123">Name for the new resource group in which to create your function app.</span></span> | 
    | <span data-ttu-id="0d803-124">**OS**</span><span class="sxs-lookup"><span data-stu-id="0d803-124">**OS**</span></span> | <span data-ttu-id="0d803-125">Windows</span><span class="sxs-lookup"><span data-stu-id="0d803-125">Windows</span></span> | <span data-ttu-id="0d803-126">関数アプリをホストするオペレーティング システム。</span><span class="sxs-lookup"><span data-stu-id="0d803-126">The operating system that hosts the function app.</span></span>  |
    | <span data-ttu-id="0d803-127">**ホスティング**</span><span class="sxs-lookup"><span data-stu-id="0d803-127">**Hosting**</span></span> |   <span data-ttu-id="0d803-128">従量課金プラン</span><span class="sxs-lookup"><span data-stu-id="0d803-128">Consumption plan</span></span> | <span data-ttu-id="0d803-129">Function App にどのようにリソースが割り当てられるかを定義するホスティング プラン。</span><span class="sxs-lookup"><span data-stu-id="0d803-129">Hosting plan that defines how resources are allocated to your function app.</span></span> <span data-ttu-id="0d803-130">既定の **[従量課金プラン]** では、リソースは関数の必要に応じて動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-130">In the default **Consumption Plan**, resources are added dynamically as required by your functions.</span></span> <span data-ttu-id="0d803-131">この[サーバーなしの](https://azure.microsoft.com/overview/serverless-computing/)ホスティングでは、関数が実行された時間にのみ課金されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-131">In this [serverless](https://azure.microsoft.com/overview/serverless-computing/) hosting, you only pay for the time your functions run.</span></span>   |
    | <span data-ttu-id="0d803-132">**場所**</span><span class="sxs-lookup"><span data-stu-id="0d803-132">**Location**</span></span> | <span data-ttu-id="0d803-133">西ヨーロッパ</span><span class="sxs-lookup"><span data-stu-id="0d803-133">West Europe</span></span> | <span data-ttu-id="0d803-134">ユーザーに近い[リージョン](https://azure.microsoft.com/regions/)、または関数がアクセスする他のサービスの近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="0d803-134">Choose a [region](https://azure.microsoft.com/regions/) near you or near other services your functions access.</span></span> |
    | <span data-ttu-id="0d803-135">**ストレージ アカウント**</span><span class="sxs-lookup"><span data-stu-id="0d803-135">**Storage account**</span></span> |  <span data-ttu-id="0d803-136">グローバルに一意の名前</span><span class="sxs-lookup"><span data-stu-id="0d803-136">Globally unique name</span></span> |  <span data-ttu-id="0d803-137">関数アプリによって使用される新しいストレージ アカウントの名前。</span><span class="sxs-lookup"><span data-stu-id="0d803-137">Name of the new storage account used by your function app.</span></span> <span data-ttu-id="0d803-138">ストレージ アカウント名の長さは 3 - 24 文字で、数字と小文字のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="0d803-138">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="0d803-139">このダイアログ ボックスでは、アプリに指定した名前から派生した一意の名前がフィールドに入力されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-139">This dialog populates the field with a unique name that is derived from the name you gave the app.</span></span> <span data-ttu-id="0d803-140">ただし、別の名前や既存のアカウントも自由に使用できます。</span><span class="sxs-lookup"><span data-stu-id="0d803-140">However, feel free to use a different name or even an existing account.</span></span> |


3. <span data-ttu-id="0d803-141">**[作成]** を選択して、関数アプリをプロビジョニングし、デプロイします。</span><span class="sxs-lookup"><span data-stu-id="0d803-141">Select **Create** to provision and deploy the function app.</span></span>

4. <span data-ttu-id="0d803-142">ポータルの右上隅の通知アイコンを選択し、"**デプロイが進行中**" であることを示す次のようなメッセージが表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="0d803-142">Select the Notification icon in the upper-right corner of the portal and watch for a **Deployment in progress** message similar to the following message.</span></span>

![関数アプリのデプロイが進行中であることを示す通知](../media-draft/func-app-deploy-progress-small.PNG)

5. <span data-ttu-id="0d803-144">デプロイには時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="0d803-144">Deployment can take some time.</span></span> <span data-ttu-id="0d803-145">そのため、通知ハブを表示した状態で、"**デプロイに成功**" したことを示す次のようなメッセージが表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="0d803-145">So, stay in the notification hub and  watch for a **Deployment succeeded** message similar to the following message.</span></span>

![関数アプリのデプロイが完了したことを示す通知](../media-draft/func-app-deploy-success-small.PNG)

6. <span data-ttu-id="0d803-147">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="0d803-147">Congratulations!</span></span> <span data-ttu-id="0d803-148">関数アプリを作成し、デプロイしました。</span><span class="sxs-lookup"><span data-stu-id="0d803-148">You've created and deployed your function app.</span></span> <span data-ttu-id="0d803-149">**[リソースに移動]** を選択して、新しい関数アプリを確認します。</span><span class="sxs-lookup"><span data-stu-id="0d803-149">Select **Go to resource** to view your new function app.</span></span>

>[!TIP]
><span data-ttu-id="0d803-150">目的の関数アプリがポータルに見つからない場合は、[ポータル内のお気に入りに Function App を追加する](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite)方法を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0d803-150">If you are having trouble finding your function apps in the portal, find out how to [add Function Apps to your favorites in the portal](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).</span></span>

### <a name="create-a-function"></a><span data-ttu-id="0d803-151">関数を作成する</span><span class="sxs-lookup"><span data-stu-id="0d803-151">Create a function</span></span>

<span data-ttu-id="0d803-152">関数アプリを用意できたので、ここでは関数を作成しましょう。</span><span class="sxs-lookup"><span data-stu-id="0d803-152">Now that we have a function app, it's time to create a function.</span></span> <span data-ttu-id="0d803-153">関数はトリガーによってアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-153">A function is activated through a trigger.</span></span> <span data-ttu-id="0d803-154">このモジュールでは、HTTP トリガーを使用します。</span><span class="sxs-lookup"><span data-stu-id="0d803-154">In this module, we'll use an HTTP trigger.</span></span>

1. <span data-ttu-id="0d803-155">新しい関数アプリを展開し、関数のコレクションをポイントして、**[関数]** の横にある [追加] (**+**) ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="0d803-155">Expand your new function app, then hover over the functions collection and select the Add (**+**) button next to **Functions**.</span></span> <span data-ttu-id="0d803-156">この操作により関数作成プロセスが開始されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-156">This action starts the function creation process.</span></span> <span data-ttu-id="0d803-157">次のアニメーションで、この操作を示します。</span><span class="sxs-lookup"><span data-stu-id="0d803-157">The following animation illustrates this action.</span></span>

![ユーザーが [関数] メニュー項目をポイントしたときにプラス記号が表示されるというアニメーション。](../media-draft/func-app-plus-hover-small.gif)

2. <span data-ttu-id="0d803-159">**[関数への早道]** ページで、**[WebHook + API]** を選択し、関数の言語を選択して、**[この関数を作成する]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0d803-159">In the **Get started quickly** page, select **WebHook + API**, select a language for your function, and click **Create this function**.</span></span>

3. <span data-ttu-id="0d803-160">HTTP によってトリガーされる関数のテンプレートを使用して、選択した言語で関数が作成されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-160">A function is created in your chosen language using the template for an HTTP triggered function.</span></span> <span data-ttu-id="0d803-161">この演習では、JavaScript 関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="0d803-161">In this exercise, we'll create a JavaScript function.</span></span>

### <a name="try-it-out"></a><span data-ttu-id="0d803-162">試してみる</span><span class="sxs-lookup"><span data-stu-id="0d803-162">Try it out</span></span>

<span data-ttu-id="0d803-163">次の手順に従って、これまでに行った作業をテストしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="0d803-163">Let's test what we have so far by doing the following:</span></span>

1. <span data-ttu-id="0d803-164">新しい関数で、右上の **[</> 関数の URL の取得]** をクリックし、**[既定値 (関数キー)]** を選択して、**[コピー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="0d803-164">In your new function, click **</> Get function URL** at the top right, select **default (Function key)**, and then click **Copy**.</span></span>

2. <span data-ttu-id="0d803-165">コピーした関数 URL をご利用のブラウザーのアドレス バーに貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="0d803-165">Paste the function URL you copied into your browser's address bar.</span></span> <span data-ttu-id="0d803-166">この URL の末尾にクエリ文字列 `&name=<yourname>` を追加し、キーボードで `Enter` キーを押して要求を実行します。</span><span class="sxs-lookup"><span data-stu-id="0d803-166">Add the query string value `&name=<yourname>` to the end of this URL and press the `Enter` key on your keyboard to execute the request.</span></span> <span data-ttu-id="0d803-167">次のような応答が関数によって返され、ブラウザーに表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-167">You should see a response similar to the following response returned by the function displayed in your browser.</span></span>  

<span data-ttu-id="0d803-168">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="0d803-168">Nice work!</span></span> <span data-ttu-id="0d803-169">HTTP によってトリガーされる関数を関数アプリに追加し、想定どおりに動作することを確認するためにテストしました。</span><span class="sxs-lookup"><span data-stu-id="0d803-169">You have now added a HTTP-triggered function to your function app and tested to make sure it is working as expected!</span></span>

![関数の呼び出しが成功したことを示す応答メッセージのスクリーンショット。](../media-draft/default-http-trigger-response-small.PNG)

<span data-ttu-id="0d803-171">これまでの演習からわかるように、関数を作成するときはトリガーの種類を選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d803-171">As you can see from this exercise so far, you have to select a trigger type when creating a function.</span></span> <span data-ttu-id="0d803-172">各関数には、トリガーが 1 つだけあります。</span><span class="sxs-lookup"><span data-stu-id="0d803-172">Every function has one, and only one trigger.</span></span> <span data-ttu-id="0d803-173">この例では HTTP トリガーを使用しています。つまり、この関数は HTTP 要求を受信すると起動します。</span><span class="sxs-lookup"><span data-stu-id="0d803-173">In this example, we're using an HTTP trigger, which means our function starts when it receives an HTTP request.</span></span> <span data-ttu-id="0d803-174">次の JavaScript のスクリーンショットに示すように、既定の実装は、クエリ文字列または要求の本文で受信したパラメーター*名*の値を返します。</span><span class="sxs-lookup"><span data-stu-id="0d803-174">The default implementation, shown in the following screenshot in JavaScript, responds with the value of a parameter *name* it received in the query string  or body of the request.</span></span> <span data-ttu-id="0d803-175">文字列が指定されていない場合、関数は呼び出し元のユーザーに対して、名前値を指定するよう求めるメッセージを返します。</span><span class="sxs-lookup"><span data-stu-id="0d803-175">If no string was provided, the function responds with a message asking whoever is calling to supply a name value.</span></span>

![HTTP によってトリガーされる Azure 関数の既定の JavaScript 実装のスクリーンショット。](../media-draft/default-http-trigger-implementation-small.PNG)

<span data-ttu-id="0d803-177">このすべてのコードは、この関数のフォルダー内の *index.js* ファイルに格納されています。</span><span class="sxs-lookup"><span data-stu-id="0d803-177">All of this code is in the *index.js* file in this function's folder.</span></span> <span data-ttu-id="0d803-178">では、関数のもう 1 つのファイルである *function.json* 構成ファイルを簡単に見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="0d803-178">Let's look briefly at the function's other file, the *function.json* config file.</span></span> <span data-ttu-id="0d803-179">次の JSON の一覧に、この構成データが示されています。</span><span class="sxs-lookup"><span data-stu-id="0d803-179">This configuration data is shown in the following JSON listing.</span></span>

```json
{
  "disabled": false,
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

<span data-ttu-id="0d803-180">ご覧のように、この関数には、`httpTrigger` 型の **req** という名前のトリガー バインディングと、`HTTP` 型の **res** という名前の出力バインディングが含まれています。</span><span class="sxs-lookup"><span data-stu-id="0d803-180">As you can see, this function has a trigger binding named **req** of type `httpTrigger` and an output binding named **res**  of type `HTTP`.</span></span> <span data-ttu-id="0d803-181">この関数の前述のコードでは、**req** パラメーターを使用して着信 HTTP 要求のペイロードにアクセスする方法を確認しました。</span><span class="sxs-lookup"><span data-stu-id="0d803-181">In the preceding code for our function, we saw how we accessed the payload of the incoming HTTP request through our **req** parameter.</span></span> <span data-ttu-id="0d803-182">同様に、**res** パラメーターを設定するだけで HTTP 応答を送信しました。</span><span class="sxs-lookup"><span data-stu-id="0d803-182">Similarly, we sent an HTTP response simply by setting our **res** parameter.</span></span> <span data-ttu-id="0d803-183">このようにバインディングを使用すると、面倒な作業の一部が自動的に行われます。</span><span class="sxs-lookup"><span data-stu-id="0d803-183">Bindings really do take care of some of the heavy lifting for us!</span></span>

>[!TIP]
><span data-ttu-id="0d803-184">Azure portal で関数パネルの右側にある **[ファイルの表示]** メニューを展開すると、index.js と function.json を確認できます。</span><span class="sxs-lookup"><span data-stu-id="0d803-184">You can see index.js and function.json by expanding the **View Files** menu on the right of the function panel in the Azure portal.</span></span>  

### <a name="explore-binding-types"></a><span data-ttu-id="0d803-185">バインディングの種類を確認する</span><span class="sxs-lookup"><span data-stu-id="0d803-185">Explore binding types</span></span>

1. <span data-ttu-id="0d803-186">関数エントリの下には、次のスクリーンショットに示す一連のメニュー項目があります。</span><span class="sxs-lookup"><span data-stu-id="0d803-186">Notice under the function entry there is a set of menu items as shown in the following screenshot.</span></span>

![[Function App] ブレードの関数の下にあるメニュー項目を示すスクリーンショット。](../media-draft/func-menu-small.PNG)

2. <span data-ttu-id="0d803-188">[統合] メニュー項目を選択して、関数の統合タブを開きます。</span><span class="sxs-lookup"><span data-stu-id="0d803-188">Select the Integrate menu item to open the integration tab for our function.</span></span> <span data-ttu-id="0d803-189">このユニットの手順に従うと、統合タブは次のスクリーンショットのようになるはずです。</span><span class="sxs-lookup"><span data-stu-id="0d803-189">If you have been following along with this unit, the integrate tab should look very similar to the following screenshot.</span></span>

![統合の UI またはタブを示すスクリーンショット。](../media-draft/func-integrate-tab-small.PNG)

<span data-ttu-id="0d803-191">このスクリーンショットに示すように、トリガー バインディングと出力バインディングを既に定義しています。</span><span class="sxs-lookup"><span data-stu-id="0d803-191">Notice that we have already defined a trigger and an output binding as shown in this screenshot.</span></span> <span data-ttu-id="0d803-192">また、複数のトリガーを追加できないこともわかります。</span><span class="sxs-lookup"><span data-stu-id="0d803-192">You can also see that we can't add more than one trigger.</span></span> <span data-ttu-id="0d803-193">実際、この関数のトリガーを変更するには、まずトリガーを削除してから、新しいトリガーを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d803-193">In fact, to change the trigger for our function we would have to first delete the trigger and create a new one.</span></span>

<span data-ttu-id="0d803-194">一方、このフォームの **[入力]** セクションと **[出力]** セクションには、さらにバインディングを追加するためのプラス `+` 記号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-194">On the other hand, the **Inputs** and **Outputs** sections of this form display a plus `+` sign to add more bindings.</span></span>

3. <span data-ttu-id="0d803-195">**[入力]** 列の下にある **[+ 新しい入力]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d803-195">Select **+ New Input** under the **Inputs** column.</span></span> <span data-ttu-id="0d803-196">次のスクリーンショットに示すように、入力バインディングとして考えられる全種類の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-196">A list of all possible input binding types is displayed as shown in the following screenshot.</span></span>

![考えられる入力バインディングの一覧を示すスクリーンショット。](../media-draft/func-input-bindings-selector-small.PNG)

<span data-ttu-id="0d803-198">これらの各入力バインディングについて、およびそれらをソリューションでどのように使用できるかについて、よく検討してみましょう。</span><span class="sxs-lookup"><span data-stu-id="0d803-198">Take a moment to consider each of these input bindings and how you might use them in a solution.</span></span> <span data-ttu-id="0d803-199">選択肢は多数あります。</span><span class="sxs-lookup"><span data-stu-id="0d803-199">There are a lot to choose from.</span></span> <span data-ttu-id="0d803-200">弊社がサポートするデータ ソースは常に増えているので、このモジュールを読むときには、この一覧の情報が既に古くなっている可能性もあります。</span><span class="sxs-lookup"><span data-stu-id="0d803-200">This list may even have changed by the time you read this module as we continue to support more data sources.</span></span>

4. <span data-ttu-id="0d803-201">**[キャンセル]** を選択してこの一覧を閉じます。</span><span class="sxs-lookup"><span data-stu-id="0d803-201">Select **Cancel** to dismiss this list.</span></span>

5. <span data-ttu-id="0d803-202">**[出力]** 列で **[+ 新しい出力]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="0d803-202">Select **+ New Output** under the **Outputs** column.</span></span> <span data-ttu-id="0d803-203">次のスクリーンショットに示すように、出力バインディングとして考えられる全種類の一覧が表示されます。</span><span class="sxs-lookup"><span data-stu-id="0d803-203">A list of all possible output binding types is displayed as shown in the following screenshot.</span></span>

![考えられる出力バインディングの一覧を示すスクリーンショット。](../media-draft/func-output-bindings-selector-small.PNG)

<span data-ttu-id="0d803-205">この場合も多数のオプションがあるため、スクリーンショットに示すように、右側のスクロール バーを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d803-205">Again, you have lots of options here, as shown by the need for a scroll bar to the right in this screenshot.</span></span>

>[!TIP]
><span data-ttu-id="0d803-206">サポートされているバインディングの詳細については、Azure Functions ドキュメント内の[サポートされるバインディングの一覧](https://docs.microsoft.com/azure/azure-functions/functions-versions)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="0d803-206">To learn more details about the bindings that are supported, check out the [list of supported bindings](https://docs.microsoft.com/azure/azure-functions/functions-versions) in the Azure Functions documentation.</span></span>

<span data-ttu-id="0d803-207">ここまで、関数アプリを作成し、関数を追加する方法を学習しました。</span><span class="sxs-lookup"><span data-stu-id="0d803-207">So far we've learned how to create a function app and add a function to it.</span></span> <span data-ttu-id="0d803-208">HTTP 要求が行われると実行される、単純な関数の動作を見てきました。</span><span class="sxs-lookup"><span data-stu-id="0d803-208">We've seen a simple function in action that runs when an HTTP request is made to it.</span></span> <span data-ttu-id="0d803-209">また、ポータル UI、および関数に使用できる入力バインディングと出力バインディングの種類も確認しました。</span><span class="sxs-lookup"><span data-stu-id="0d803-209">We've also explored the portal UI and types of input and output binding that are available to our functions.</span></span> <span data-ttu-id="0d803-210">次のユニットでは、入力バインディングを使用してデータベースからテキストを読み取ります。</span><span class="sxs-lookup"><span data-stu-id="0d803-210">In the next unit, we'll use an input binding to read text from a a database.</span></span>