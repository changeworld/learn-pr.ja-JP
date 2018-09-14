<span data-ttu-id="8e2e3-101">関数アプリを作成したので、Azure 関数をビルド、構成、実行する方法について学習しましょう。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-101">Now that we have a function app created, we'll learn how to build, configure, and execute an Azure function.</span></span>

### <a name="triggers"></a><span data-ttu-id="8e2e3-102">トリガー</span><span class="sxs-lookup"><span data-stu-id="8e2e3-102">Triggers</span></span>

<span data-ttu-id="8e2e3-103">関数はイベント駆動型です。つまり、イベントに応答して実行されるということです。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-103">Functions are event driven, which means they run in response to an event.</span></span>

<span data-ttu-id="8e2e3-104">関数を開始するイベントの種類は、*トリガー*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-104">The type of event that starts the function is called a *trigger*.</span></span> <span data-ttu-id="8e2e3-105">関数は必ず 1 つのトリガーで構成します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-105">You configure a function with exactly one trigger.</span></span>

 <span data-ttu-id="8e2e3-106">Azure では、次のサービスに対してトリガーをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-106">Azure supports triggers for the following services.</span></span>


|<span data-ttu-id="8e2e3-107">サービス</span><span class="sxs-lookup"><span data-stu-id="8e2e3-107">Service</span></span>  |<span data-ttu-id="8e2e3-108">トリガーの説明</span><span class="sxs-lookup"><span data-stu-id="8e2e3-108">Trigger description</span></span>  |
|---------|---------|
|<span data-ttu-id="8e2e3-109">BLOB ストレージ</span><span class="sxs-lookup"><span data-stu-id="8e2e3-109">Blob storage</span></span>     |  <span data-ttu-id="8e2e3-110">新しい BLOB または更新された BLOB が検出されたときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-110">Start a function when a new or updated blob is detected.</span></span>       |
|<span data-ttu-id="8e2e3-111">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8e2e3-111">Cosmos DB</span></span>     |  <span data-ttu-id="8e2e3-112">挿入および更新が検出されたときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-112">Start a function when inserts and updates are detected.</span></span>      |
|<span data-ttu-id="8e2e3-113">Event Grid</span><span class="sxs-lookup"><span data-stu-id="8e2e3-113">Event Grid</span></span>     |   <span data-ttu-id="8e2e3-114">Event Grid からイベントを受信したときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-114">Start a function when an event is received from Event Grid.</span></span>       |
|<span data-ttu-id="8e2e3-115">HTTP</span><span class="sxs-lookup"><span data-stu-id="8e2e3-115">HTTP</span></span>     |   <span data-ttu-id="8e2e3-116">HTTP 要求で関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-116">Start a function with an HTTP request.</span></span>      |
|<span data-ttu-id="8e2e3-117">Microsoft Graph イベント</span><span class="sxs-lookup"><span data-stu-id="8e2e3-117">Microsoft Graph Events</span></span>     |  <span data-ttu-id="8e2e3-118">Microsoft Graph から受信した Webhook に応答して関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-118">Start a function in response to an incoming webhook from the Microsoft Graph.</span></span> <span data-ttu-id="8e2e3-119">このトリガーの各インスタンスは、それぞれ Microsoft Graph の 1 つのリソースの種類に応答できます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-119">Each instance of this trigger can react to one Microsoft Graph resource type.</span></span>       |
|<span data-ttu-id="8e2e3-120">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="8e2e3-120">Queue storage</span></span>     |    <span data-ttu-id="8e2e3-121">キューで新しい項目を受け取ったときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-121">Start a function when a new item is received on a queue.</span></span> <span data-ttu-id="8e2e3-122">キュー メッセージは、関数への入力として提供されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-122">The queue message is provided as input to the function.</span></span>      |
|<span data-ttu-id="8e2e3-123">Service Bus</span><span class="sxs-lookup"><span data-stu-id="8e2e3-123">Service Bus</span></span>     |  <span data-ttu-id="8e2e3-124">Service Bus キューからのメッセージに応答して関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-124">Start a function in response to messages from a Service Bus queue.</span></span>       |
|<span data-ttu-id="8e2e3-125">Timer</span><span class="sxs-lookup"><span data-stu-id="8e2e3-125">Timer</span></span>     |  <span data-ttu-id="8e2e3-126">スケジュールに従って関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-126">Start a function on a schedule.</span></span>       |
|<span data-ttu-id="8e2e3-127">Webhook</span><span class="sxs-lookup"><span data-stu-id="8e2e3-127">Webhooks</span></span>     |  <span data-ttu-id="8e2e3-128">Webhook 要求で関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-128">Start a function with a webhook request.</span></span>       |

### <a name="bindings"></a><span data-ttu-id="8e2e3-129">バインド</span><span class="sxs-lookup"><span data-stu-id="8e2e3-129">Bindings</span></span>

<span data-ttu-id="8e2e3-130">Azure Functions バインドは、データとサービスを作成した関数に接続するための宣言型の方法です。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-130">Azure Functions bindings are a declarative way to connect data and services to your function.</span></span> <span data-ttu-id="8e2e3-131">データ ソースに接続して、接続を管理するために、自分で関数にコードを記述する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-131">You don't have to write code in your function to connect to data sources and manage connections.</span></span> <span data-ttu-id="8e2e3-132">ユーザーの代わりに、プラットフォームによってその複雑さが処理されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-132">The platform takes care that complexity for you.</span></span> <span data-ttu-id="8e2e3-133">バインドには方向があります。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-133">A binding has a direction.</span></span> <span data-ttu-id="8e2e3-134">コードは、*入力*バインドからデータを読み取り、*出力*バインドにデータを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-134">Your code reads data from *input* bindings and writes data to *output* bindings.</span></span> <span data-ttu-id="8e2e3-135">1 つ例を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-135">Let's look at an example.</span></span>

<span data-ttu-id="8e2e3-136">Blob Storage からデータを読み取るには、*BLOB* 型の入力バインドを構成します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-136">To read data from Blob storage, you configure an input binding of type *blob*.</span></span> <span data-ttu-id="8e2e3-137">メッセージを Queue Storage に書き込むには、*キュー*型の出力バインドを構成します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-137">To write a message to Queue storage, you configure an output binding of type *queue*.</span></span>

<span data-ttu-id="8e2e3-138">サポートされるバインドとトリガーの詳細については、[Azure ドキュメント](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-138">For more information about the supported bindings and triggers, check out the [Azure documentation](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings).</span></span>

### <a name="a-sample-trigger-and-binding-functionjson"></a><span data-ttu-id="8e2e3-139">サンプルのトリガーとバインド (function.json)</span><span class="sxs-lookup"><span data-stu-id="8e2e3-139">A sample trigger and binding (function.json)</span></span>

<span data-ttu-id="8e2e3-140">次の JSON は、関数に対するトリガーとバインドの定義のサンプルです。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-140">The following JSON is sample definition of a trigger and binding for a function.</span></span>

```javascript
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

<span data-ttu-id="8e2e3-141">前述のとおり、各バインドには、それが入力バインドなのか出力バインドなのかを定義する方向があります。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-141">As we mentioned previously, each binding has a direction that defines whether it is an input or output binding.</span></span> <span data-ttu-id="8e2e3-142">トリガーは常に入力バインドです。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-142">Triggers are always input bindings.</span></span> <span data-ttu-id="8e2e3-143">上記のサンプルは、**myqueue-items** という名前のキューに追加されるメッセージによってトリガーされる関数を示しています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-143">The preceding sample shows a function that is triggered by a message being added to a queue named **myqueue-items**.</span></span> <span data-ttu-id="8e2e3-144">次に、関数の戻り値を Azure テーブル ストレージ内の **outTable** テーブルに送信します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-144">It then sends the return value of the function to the **outTable** table in Azure Table storage.</span></span>

## <a name="creating-a-function-in-the-azure-portal"></a><span data-ttu-id="8e2e3-145">Azure portal で関数を作成する</span><span class="sxs-lookup"><span data-stu-id="8e2e3-145">Creating a function in the Azure portal</span></span>

<span data-ttu-id="8e2e3-146">Azure では、Azure 関数の一般的なシナリオ向けに、事前構成済みのテンプレートが用意されています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-146">Azure provides pre-configured templates for common Azure function scenarios.</span></span>

### <a name="quickstart-templates"></a><span data-ttu-id="8e2e3-147">クイック スタート テンプレート</span><span class="sxs-lookup"><span data-stu-id="8e2e3-147">Quickstart templates</span></span>

<span data-ttu-id="8e2e3-148">Azure 関数を追加するには、関数アプリを選択する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-148">To add an Azure function, you must select a function app.</span></span> <span data-ttu-id="8e2e3-149">関数アプリは、山かっこ内の稲妻アイコンによって識別できます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-149">The function app can be identified by the lightning bolt within pointy brackets icon.</span></span>  
![リソース グループで選択した関数アプリを示すスクリーンショット。](../images/5-function-icon.png)

<span data-ttu-id="8e2e3-151">初めて Azure 関数を追加するときに、クイック スタート画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-151">When adding your first Azure function, you are presented with the Quickstart screen.</span></span> <span data-ttu-id="8e2e3-152">この画面を使用して、トリガーの種類とプログラミング言語を選択することができます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-152">This screen allows you to choose a trigger type and programming language.</span></span> <span data-ttu-id="8e2e3-153">その後、選択内容に基づいて、関数のコードと構成が Azure によって生成されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-153">Then, based on your selections, Azure will generate the function code and configuration for you.</span></span> 
 
![関数のクイックスタート](../images/5-quickstart-form.png)

### <a name="function-templates"></a><span data-ttu-id="8e2e3-155">関数テンプレート</span><span class="sxs-lookup"><span data-stu-id="8e2e3-155">Function templates</span></span>

<span data-ttu-id="8e2e3-156">テンプレートの選択範囲は、クイック スタートに表示されているテンプレートに限りません。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-156">The selection of templates is not limited to the templates displayed in the Quickstart.</span></span> <span data-ttu-id="8e2e3-157">30 個を超えるさまざまなテンプレートから選択するオプションもあります。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-157">You also have the option of choosing from over 30 different templates.</span></span> <span data-ttu-id="8e2e3-158">テンプレート リストの画面には、後続の関数の作成時、またはクイック スタート画面で **[カスタム関数]** オプションを選択してアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-158">You can access the template list screen while creating subsequent functions, or by selecting the **Custom function** option on the Quickstart screen.</span></span>  
<span data-ttu-id="8e2e3-159">![用意されている関数を使用してすばやく開始するのに役立つ Azure Functions クイック スタートのユーザー インターフェイスのスクリーンショット。](../images/5-template-list.png)</span><span class="sxs-lookup"><span data-stu-id="8e2e3-159">![Screenshot of Azure Functions Quickstart user interface to help you get started quickly with a premade function.](../images/5-template-list.png)</span></span>

## <a name="navigating-to-your-function-and-files"></a><span data-ttu-id="8e2e3-160">関数とファイルに移動する</span><span class="sxs-lookup"><span data-stu-id="8e2e3-160">Navigating to your function and files</span></span>

<span data-ttu-id="8e2e3-161">テンプレートから関数を作成すると、複数のファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-161">When you create a function from a template, several files are created.</span></span> <span data-ttu-id="8e2e3-162">たとえば、JavaScript を使用して Webhook + API クイック スタートを使用することを選択した場合は、構成ファイル (**function.json**) とソース コード ファイル (**index.js**) が生成されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-162">For example, if you opted to use the Webhook + API Quickstart using JavaScript, the files generated would be a configuration file, **function.json**, and a source code file, **index.js**.</span></span> <span data-ttu-id="8e2e3-163">関数アプリで作成した関数は、関数アプリ ポータル内の [関数] メニュー項目の下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-163">The functions you create in a function app appear under the Functions menu item in the function app portal.</span></span>

<span data-ttu-id="8e2e3-164">関数アプリで関数を選択すると、次のスクリーンショットで示されているように、コード エディターが開き、ご自分の関数用のコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-164">When you select a function in your function app, a code editor opens and displays the code for your function, as illustrated in the following screenshot.</span></span>

![関数の開発を高速化するために使用できるテンプレートのセットが一覧表示された関数テンプレートの選択インターフェイスを示すスクリーンショット。](../images/5-file-navigation.png)

<span data-ttu-id="8e2e3-166">前のスクリーンショットで確認できるように、右側にポップアップ メニューがあり、**[ファイルの表示]** へのタブが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-166">As you can see in the preceding screenshot,  there's a flyout menu on the right that includes a tab to **View files**.</span></span> <span data-ttu-id="8e2e3-167">このタブを選択すると、ご自分の関数を構成するファイル構造が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-167">Selecting this tab shows the file structure that makes up your function.</span></span>  

## <a name="execution-options-for-testing-your-azure-function"></a><span data-ttu-id="8e2e3-168">Azure 関数をテストするための実行オプション</span><span class="sxs-lookup"><span data-stu-id="8e2e3-168">Execution options for testing your Azure function</span></span>

<span data-ttu-id="8e2e3-169">関数を作成したら、テストを行います。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-169">Once you've created a function, you'll want to test it.</span></span> <span data-ttu-id="8e2e3-170">手動で実行する方法と、Azure portal 内からテストする方法の 2 種類があります。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-170">There are a couple of approaches: manual execution and testing from within the Azure portal itself.</span></span>

### <a name="manual-execution-of-a-function"></a><span data-ttu-id="8e2e3-171">関数の手動での実行</span><span class="sxs-lookup"><span data-stu-id="8e2e3-171">Manual execution of a function</span></span>

<span data-ttu-id="8e2e3-172">構成されたトリガーを手動でトリガーすることによって、関数を開始できます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-172">You can start a function by manually triggering the configured trigger.</span></span> <span data-ttu-id="8e2e3-173">たとえば、HttpTrigger を使用している場合、Postman または cURL などのツールを使用して、自分の関数エンドポイント URL への HTTP 要求を開始することができます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-173">For instance, if you are using an HttpTrigger - you can use a tool such as Postman or cURL to initiate an HTTP request to your function endpoint URL.</span></span>  

![<span data-ttu-id="8e2e3-174">[URL] フィールドに関数 URL が入力され、要求本文にサンプルの本文が入力された、Postman アプリケーション インターフェイスのスクリーンショット (部分)。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-174">Partial screenshot of the Postman application interface with a function URL typed into the url field and the request body filled out with a sample body.</span></span> ](../images/5-postman-execution.png)

<span data-ttu-id="8e2e3-175">エンドポイント URL を取得するには、左側のナビゲーションから [HTTP トリガー] を選択し、**[関数の URL の取得]** ボタンを選択します。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-175">To get endpoint URL, select the HTTP Trigger from the left navigation, and then select the **Get function URL** button.</span></span>  

![関数が選択され、ページの上部で *関数の URL の取得* アクションが選択された Function App ポータルのスクリーンショット。](../images/5-get-function-url.png)

### <a name="testing-a-function-in-the-azure-portal"></a><span data-ttu-id="8e2e3-177">Azure portal で関数をテストする</span><span class="sxs-lookup"><span data-stu-id="8e2e3-177">Testing a function in the Azure portal</span></span>

<span data-ttu-id="8e2e3-178">ポータルにも、関数をテストするための便利な手段が用意されています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-178">The portal also provides a convenient way to test your functions.</span></span> <span data-ttu-id="8e2e3-179">コード ウィンドウの右側に、タブ付きのポップアップ ナビゲーション メニューがあります。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-179">On the right side of the code window, there is a flyout tabbed navigation menu.</span></span> <span data-ttu-id="8e2e3-180">このメニューには、**[テスト]** 項目が含まれています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-180">This menu contains a **Test** item.</span></span> <span data-ttu-id="8e2e3-181">メニューを展開してこのタブを選択して、別の方法で関数を実行し、結果を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-181">Expanding the menu and selecting this tab gives you another way to execute your function and view the result.</span></span> <span data-ttu-id="8e2e3-182">HttpTrigger シナリオに合わせて、HTTP メソッドを設定し、クエリ文字列パラメーターと HTTP ヘッダーを要求に追加することができます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-182">In keeping with the HttpTrigger scenario, you can set the HTTP method, and add query string parameters and HTTP headers to the request.</span></span> <span data-ttu-id="8e2e3-183">要求本文を変更して、追加のシナリオをテストすることもできます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-183">You may also change the request body to test additional scenarios.</span></span> <span data-ttu-id="8e2e3-184">次の例では、*name* と呼ばれる 1 つのパラメーターを持つ要求本文で HTTP POST 要求としてテストが構成されています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-184">In the following example, the test is configured as an HTTP POST request with a request body that has one parameter called *name*.</span></span>
 
![HTTP POST 要求とサンプルの要求本文を示す Function App ポータルのテスト ウィンドウのスクリーンショット。](../images/5-portal-execution.png)

<span data-ttu-id="8e2e3-187">このテスト ウィンドウで **[実行]** をクリックすると、その結果が出力ウィンドウに状態コードと共に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-187">When you click **Run** in this test window, the results are displayed in the output window, along with a status code.</span></span> 

## <a name="monitoring-an-azure-function"></a><span data-ttu-id="8e2e3-188">Azure 関数を監視する</span><span class="sxs-lookup"><span data-stu-id="8e2e3-188">Monitoring an Azure function</span></span>

<span data-ttu-id="8e2e3-189">メッセージを記録し関数を監視する機能は、開発中と運用環境で重要です。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-189">The ability to log messages and monitor your functions is critical during development and in production.</span></span> <span data-ttu-id="8e2e3-190">Azure portal には、監視ダッシュボードと、ご使用の Azure 関数から取得した実行ログと例外を確認するための方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-190">The Azure portal provides a monitoring dashboard as well as a way to review execution logs and exceptions obtained from your Azure functions.</span></span>

### <a name="log-window"></a><span data-ttu-id="8e2e3-191">ログ ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="8e2e3-191">Log window</span></span>

<span data-ttu-id="8e2e3-192">関数にログ ステートメントを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-192">You're also able to add log statements to your function.</span></span> <span data-ttu-id="8e2e3-193">これらのステートメントは、関数のログ ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-193">These statements will appear in the log window of the function.</span></span> <span data-ttu-id="8e2e3-194">ログ ウィンドウは、コード ウィンドウの下部にあるタブ付きのポップアップ メニューにあります。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-194">The log window is located in a tabbed flyout menu located at the bottom of the code window.</span></span> <span data-ttu-id="8e2e3-195">次の JavaScript コード スニペットは、`context.log` メソッドを使用してメッセージを記録する方法について示しています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-195">The following JavaScript code snippet shows how to log a message using the `context.log` method.</span></span>

```javascript
  context.log('Enter your logging statement here');
```  

![Azure Functions のユーザー インターフェイスのコード エディター セクションのスクリーンショット。](../images/5-log-window.png)

### <a name="errors-and-warnings-window"></a><span data-ttu-id="8e2e3-198">エラーと警告ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="8e2e3-198">Errors and warnings window</span></span>

<span data-ttu-id="8e2e3-199">エラーと警告ウィンドウ タブは、ログ ウィンドウと同じポップアップ メニューで検索できます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-199">You can locate the errors and warnings window tab in the same flyout menu as the log window.</span></span> <span data-ttu-id="8e2e3-200">このウィンドウには、コード内のコンパイル エラーと警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-200">This window will show compilation errors and warnings within your code.</span></span> <span data-ttu-id="8e2e3-201">JavaScript は動的なインタープリタ型言語であるため、次の図は、C# 関数でのコンパイル エラーを示しています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-201">As JavaScript is a dynamic and interpreted language, the following image shows a compilation error in a C# function.</span></span>  

![画面の下部で **ログ** ウィンドウが強調表示された、Azure Functions のユーザー インターフェイスのコード エディター セクションのスクリーン ショット。](../images/5-errors-window.png)

### <a name="monitoring-dashboard"></a><span data-ttu-id="8e2e3-203">監視ダッシュボード</span><span class="sxs-lookup"><span data-stu-id="8e2e3-203">Monitoring dashboard</span></span>

<span data-ttu-id="8e2e3-204">関数アプリのナビゲーション メニューでは、関数ノードを展開すると **[監視]** メニュー項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-204">In the function app navigation menu, once you expand the function node - you will see a **Monitor** menu item.</span></span> <span data-ttu-id="8e2e3-205">監視ダッシュボードには、関数の実行履歴を表示する簡単な方法が用意されています。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-205">The monitor dashboard provides a quick way to view the history of function executions.</span></span> <span data-ttu-id="8e2e3-206">このビューには、タイムスタンプ、結果コード、期間、および操作 ID も表示されます (正常に完了した場合)。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-206">This view displays the timestamp, result code, duration, and operation ID, as well as if it completed successfully.</span></span> <span data-ttu-id="8e2e3-207">データは、Azure Application Insights を通じて入力されます。</span><span class="sxs-lookup"><span data-stu-id="8e2e3-207">The data is populated via Azure Application Insights.</span></span>  

![関数の呼び出しの成功と失敗の一覧を示している、**[監視]** 関数のメニュー項目から起動した監視ダッシュボードのスクリーンショット。](../images/5-monitor-function.png)
