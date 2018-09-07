<span data-ttu-id="e80e1-101">関数アプリを作成したので、関数をビルド、構成、実行する方法について確認しましょう。</span><span class="sxs-lookup"><span data-stu-id="e80e1-101">Now that we have a function app created, let's look at how to build, configure, and execute a function.</span></span>

### <a name="triggers"></a><span data-ttu-id="e80e1-102">トリガー</span><span class="sxs-lookup"><span data-stu-id="e80e1-102">Triggers</span></span>

<span data-ttu-id="e80e1-103">関数はイベント駆動型です。つまり、イベントに応答して実行されるということです。</span><span class="sxs-lookup"><span data-stu-id="e80e1-103">Functions are event driven, which means they run in response to an event.</span></span>

<span data-ttu-id="e80e1-104">関数を開始するイベントの種類は、*トリガー*と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-104">The type of event that starts the function is called a *trigger*.</span></span> <span data-ttu-id="e80e1-105">関数は必ず 1 つのトリガーで構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e80e1-105">You must configure a function with exactly one trigger.</span></span>

<span data-ttu-id="e80e1-106">Azure では、次のサービスに対してトリガーをサポートしています。</span><span class="sxs-lookup"><span data-stu-id="e80e1-106">Azure supports triggers for the following services.</span></span>

| <span data-ttu-id="e80e1-107">サービス</span><span class="sxs-lookup"><span data-stu-id="e80e1-107">Service</span></span>                 | <span data-ttu-id="e80e1-108">トリガーの説明</span><span class="sxs-lookup"><span data-stu-id="e80e1-108">Trigger description</span></span>  |
|-------------------------|---------|
| <span data-ttu-id="e80e1-109">BLOB ストレージ</span><span class="sxs-lookup"><span data-stu-id="e80e1-109">Blob storage</span></span>            | <span data-ttu-id="e80e1-110">新しい BLOB または更新された BLOB が検出されたときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-110">Start a function when a new or updated blob is detected.</span></span>       |
| <span data-ttu-id="e80e1-111">Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e80e1-111">Cosmos DB</span></span>               | <span data-ttu-id="e80e1-112">挿入および更新が検出されたときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-112">Start a function when inserts and updates are detected.</span></span>      |
| <span data-ttu-id="e80e1-113">Event Grid</span><span class="sxs-lookup"><span data-stu-id="e80e1-113">Event Grid</span></span>              | <span data-ttu-id="e80e1-114">Event Grid からイベントを受信したときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-114">Start a function when an event is received from Event Grid.</span></span>       |
| <span data-ttu-id="e80e1-115">HTTP</span><span class="sxs-lookup"><span data-stu-id="e80e1-115">HTTP</span></span>                    | <span data-ttu-id="e80e1-116">HTTP 要求で関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-116">Start a function with an HTTP request.</span></span>      |
| <span data-ttu-id="e80e1-117">Microsoft Graph イベント</span><span class="sxs-lookup"><span data-stu-id="e80e1-117">Microsoft Graph Events</span></span>  | <span data-ttu-id="e80e1-118">Microsoft Graph から受信した Webhook に応答して関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-118">Start a function in response to an incoming webhook from the Microsoft Graph.</span></span> <span data-ttu-id="e80e1-119">このトリガーの各インスタンスは、それぞれ Microsoft Graph の 1 つのリソースの種類に応答できます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-119">Each instance of this trigger can react to one Microsoft Graph resource type.</span></span>       |
| <span data-ttu-id="e80e1-120">Queue Storage</span><span class="sxs-lookup"><span data-stu-id="e80e1-120">Queue storage</span></span>           | <span data-ttu-id="e80e1-121">キューで新しい項目を受け取ったときに関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-121">Start a function when a new item is received on a queue.</span></span> <span data-ttu-id="e80e1-122">キュー メッセージは、関数への入力として提供されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-122">The queue message is provided as input to the function.</span></span>      |
| <span data-ttu-id="e80e1-123">Service Bus</span><span class="sxs-lookup"><span data-stu-id="e80e1-123">Service Bus</span></span>             | <span data-ttu-id="e80e1-124">Service Bus キューからのメッセージに応答して関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-124">Start a function in response to messages from a Service Bus queue.</span></span>       |
| <span data-ttu-id="e80e1-125">Timer</span><span class="sxs-lookup"><span data-stu-id="e80e1-125">Timer</span></span>                   | <span data-ttu-id="e80e1-126">スケジュールに従って関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-126">Start a function on a schedule.</span></span>       |
| <span data-ttu-id="e80e1-127">Webhook</span><span class="sxs-lookup"><span data-stu-id="e80e1-127">Webhooks</span></span>                | <span data-ttu-id="e80e1-128">Webhook 要求で関数を開始します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-128">Start a function with a webhook request.</span></span>       |

### <a name="bindings"></a><span data-ttu-id="e80e1-129">バインド</span><span class="sxs-lookup"><span data-stu-id="e80e1-129">Bindings</span></span>

<span data-ttu-id="e80e1-130">バインドは、データとサービスを作成した関数に接続するための宣言型の方法です。</span><span class="sxs-lookup"><span data-stu-id="e80e1-130">Bindings are a declarative way to connect data and services to your function.</span></span> <span data-ttu-id="e80e1-131">バインドは、さまざまなサービスに応答する方法を理解しています。つまり、データ ソースに接続して、接続を管理するために、自分で関数にコードを記述する必要はないということです。</span><span class="sxs-lookup"><span data-stu-id="e80e1-131">Bindings know how to talk to different services which means you don't have to write code in your function to connect to data sources and manage connections.</span></span> <span data-ttu-id="e80e1-132">ユーザーの代わりに、プラットフォームによってその複雑さがバインド コードの一部として処理されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-132">The platform takes care that complexity for you as part of the binding code.</span></span> <span data-ttu-id="e80e1-133">バインドごとに方向が 1 つあります。お客様のコードでは、*入力*バインディングからデータを読み取り、*出力*バインディングにデータを書き込みます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-133">Each binding has a direction - your code reads data from *input* bindings and writes data to *output* bindings.</span></span> <span data-ttu-id="e80e1-134">各関数には、関数によって処理された入出力データを管理するために、0 個またはそれ以上のバインドを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-134">Each function can have zero or more bindings to manage the input and output data processed by the function.</span></span>

> [!NOTE]
> <span data-ttu-id="e80e1-135">技術的には、トリガーは、実行を開始する追加機能を持つ特殊な入力バインディングです。</span><span class="sxs-lookup"><span data-stu-id="e80e1-135">Technically, a trigger is a special type of input binding that has the additional capability of initiating execution.</span></span>

<span data-ttu-id="e80e1-136">Azure では、さまざまなストレージとメッセージング サービスに接続するために、[多数のバインド](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings)が提供されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-136">Azure provides a [large number of bindings](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings) to connect to different storage and messaging services.</span></span>

### <a name="a-sample-binding-definition"></a><span data-ttu-id="e80e1-137">バインド定義のサンプル</span><span class="sxs-lookup"><span data-stu-id="e80e1-137">A sample binding definition</span></span>

<span data-ttu-id="e80e1-138">入力バインディング (トリガー) と出力バインディングで関数を構成する例を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="e80e1-138">Let's look at an example of configuring a function with an input binding (trigger) and an output binding.</span></span> <span data-ttu-id="e80e1-139">BLOB ストレージからデータを読み取り、関数で処理して、キューにメッセージを書き込む必要があるとします。</span><span class="sxs-lookup"><span data-stu-id="e80e1-139">Let's say we want to read data from Blob storage, process it in our function, and then write a message to a queue.</span></span> <span data-ttu-id="e80e1-140">*BLOB* の種類の_入力バインディング_と*キュー*の種類の_出力バインディング_を構成します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-140">You would configure an _input binding_ of type *blob* and an _output binding_ of type *queue*.</span></span>

<span data-ttu-id="e80e1-141">バインドは Azure portal で定義することができ、直接編集することもできる JSON ファイルとして保存されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-141">Bindings can be defined in the Azure portal, and are stored as JSON files which you can also edit directly.</span></span> <span data-ttu-id="e80e1-142">次の JSON は、関数に対するトリガーとバインドの定義のサンプルです。</span><span class="sxs-lookup"><span data-stu-id="e80e1-142">The following JSON is sample definition of a trigger and binding for a function.</span></span>

```json
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

<span data-ttu-id="e80e1-143">この例は、メッセージが **myqueue-items** という名前のキューに追加されることによってトリガーされる関数を示しています。</span><span class="sxs-lookup"><span data-stu-id="e80e1-143">This example shows a function that is triggered by a message being added to a queue named **myqueue-items**.</span></span> <span data-ttu-id="e80e1-144">次に、関数の戻り値を Azure テーブル ストレージ内の **outTable** テーブルに送信します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-144">It then sends the return value of the function to the **outTable** table in Azure Table storage.</span></span> <span data-ttu-id="e80e1-145">これは非常にシンプルな例で、出力を SendGrid バインディングを使用する電子メールに変更したり、アーキテクチャ内のその他のコンポーネントに通知するために Service Bus にイベントを配置したりすることができます。また、さまざまなサービスにデータをプッシュするように、複数の出力バインディングを含めることもできます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-145">This is a very simple example, we could change the output to be an email using a SendGrid binding, or put an event onto a Service Bus to notify some other component in our architecture, or even have multiple output bindings to push data to various services.</span></span>

## <a name="creating-a-function-in-the-azure-portal"></a><span data-ttu-id="e80e1-146">Azure portal で関数をテストする</span><span class="sxs-lookup"><span data-stu-id="e80e1-146">Creating a function in the Azure portal</span></span>

<span data-ttu-id="e80e1-147">Azure では、一般的なシナリオに使用できる、いくつかの事前に作成された関数テンプレートを提供します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-147">Azure provides several pre-made function templates for common scenarios.</span></span>

### <a name="quickstart-templates"></a><span data-ttu-id="e80e1-148">クイック スタート テンプレート</span><span class="sxs-lookup"><span data-stu-id="e80e1-148">Quickstart templates</span></span>

<span data-ttu-id="e80e1-149">初めて関数を追加する場合、クイック スタート画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-149">When adding your first function, you are presented with the Quickstart screen.</span></span> <span data-ttu-id="e80e1-150">この画面では、トリガーの種類 (HTTP、タイマーまたはデータ) およびプログラミング言語 (C#、JavaScript、F# または Java) を選ぶことができます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-150">This screen allows you to choose a trigger type (HTTP, Timer, or Data) and programming language (C#, JavaScript, F# or Java).</span></span> <span data-ttu-id="e80e1-151">次に、選択内容に基づいて、Azure では、ログで受け取った入力データを表示するために提供されたサンプル コードを使って、ユーザーのために関数コードと構成が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-151">Then, based on your selections, Azure will generate the function code and configuration for you with some sample code provided to display out the input data received in the log.</span></span> 
 
### <a name="custom-function-templates"></a><span data-ttu-id="e80e1-152">カスタムの関数テンプレート</span><span class="sxs-lookup"><span data-stu-id="e80e1-152">Custom function templates</span></span>

<span data-ttu-id="e80e1-153">クイック スタート テンプレートを選択すると、最も一般的なシナリオへの簡単なアクセスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-153">The selection of Quickstart templates provides easy access to the most common scenarios.</span></span> <span data-ttu-id="e80e1-154">しかし、Azure では使用を開始できるテンプレートが 30 個以上提供されています。</span><span class="sxs-lookup"><span data-stu-id="e80e1-154">However, Azure provides over 30 additional templates you can start with.</span></span> <span data-ttu-id="e80e1-155">これらのテンプレートは、後続の関数の作成時にテンプレート リストの画面から選択できます。または、クイック スタート画面で **[カスタム関数]** オプションを使用することで選択できます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-155">These can be selected from the template list screen when creating subsequent functions or be selected by using the **Custom function** option on the Quickstart screen.</span></span>

- <span data-ttu-id="e80e1-156">HTTP トリガーと C#、F#、または JavaScript</span><span class="sxs-lookup"><span data-stu-id="e80e1-156">HTTP trigger w/ C#, F#, or JavaScript</span></span>
- <span data-ttu-id="e80e1-157">タイマー トリガーと C#、F#、または JavaScript</span><span class="sxs-lookup"><span data-stu-id="e80e1-157">Timer trigger w/ C#, F#, or JavaScript</span></span>
- <span data-ttu-id="e80e1-158">キュー トリガーと C#、F#、または JavaScript</span><span class="sxs-lookup"><span data-stu-id="e80e1-158">Queue trigger w/ C#, F#, or JavaScript</span></span>
- <span data-ttu-id="e80e1-159">Service Bus キュー トリガーと C#、F#、または JavaScript</span><span class="sxs-lookup"><span data-stu-id="e80e1-159">Service Bus Queue trigger w/ C#, F#, or JavaScript</span></span>
- <span data-ttu-id="e80e1-160">Cosmos DB トリガーと C#、F#、または JavaScript</span><span class="sxs-lookup"><span data-stu-id="e80e1-160">Cosmos DB trigger w/ C# or JavaScript</span></span>
- <span data-ttu-id="e80e1-161">IoT Hub (イベント ハブ) と C#、F#、または JavaScript</span><span class="sxs-lookup"><span data-stu-id="e80e1-161">IoT Hub (Event Hub) w/ C#, F#, or JavaScript</span></span>
- <span data-ttu-id="e80e1-162">... その他</span><span class="sxs-lookup"><span data-stu-id="e80e1-162">... and many more</span></span>

## <a name="navigating-to-your-function-and-files"></a><span data-ttu-id="e80e1-163">関数とファイルに移動する</span><span class="sxs-lookup"><span data-stu-id="e80e1-163">Navigating to your function and files</span></span>

<span data-ttu-id="e80e1-164">テンプレートから関数を作成すると、複数のファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-164">When you create a function from a template, several files are created.</span></span> <span data-ttu-id="e80e1-165">たとえば、JavaScript を使用して Webhook + API クイック スタートを使用することを選択した場合は、構成ファイル (**function.json**) とソース コード ファイル (**index.js**) が生成されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-165">For example, if you opted to use the Webhook + API Quickstart using JavaScript, the files generated would be a configuration file, **function.json**, and a source code file, **index.js**.</span></span> <span data-ttu-id="e80e1-166">関数アプリで作成した関数は、関数アプリ ポータル内の **[関数]** メニュー項目の下に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-166">The functions you create in a function app appear under the **Functions** menu item in the function app portal.</span></span>

<span data-ttu-id="e80e1-167">関数アプリで関数を選択すると、次のスクリーンショットで示されているように、コード エディターが開き、ご自分の関数用のコードが表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-167">When you select a function in your function app, a code editor opens and displays the code for your function, as illustrated in the following screenshot.</span></span>

![関数テンプレートの選択インターフェイスには、関数の開発を高速化するために使用できるテンプレートのセットが一覧されます。](../media-draft/4-file-navigation.png)

<span data-ttu-id="e80e1-169">前のスクリーンショットで確認できるように、右側にポップアップ メニューがあり、**[ファイルの表示]** へのタブが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e80e1-169">As you can see in the preceding screenshot,  there's a flyout menu on the right that includes a tab to **View files**.</span></span> <span data-ttu-id="e80e1-170">このタブを選択すると、ご自分の関数を構成するファイル構造が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-170">Selecting this tab shows the file structure that makes up your function.</span></span>  

## <a name="testing-your-azure-function"></a><span data-ttu-id="e80e1-171">Azure 関数をテストする</span><span class="sxs-lookup"><span data-stu-id="e80e1-171">Testing your Azure function</span></span>

<span data-ttu-id="e80e1-172">関数を作成したら、テストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e80e1-172">Once you've created a function, you'll want to test it.</span></span> <span data-ttu-id="e80e1-173">手動で実行する方法と、Azure portal 内からテストする方法の 2 種類があります。</span><span class="sxs-lookup"><span data-stu-id="e80e1-173">There are a couple of approaches: manual execution and testing from within the Azure portal itself.</span></span>

### <a name="manual-execution"></a><span data-ttu-id="e80e1-174">手動での実行</span><span class="sxs-lookup"><span data-stu-id="e80e1-174">Manual execution</span></span>

<span data-ttu-id="e80e1-175">構成されたトリガーを手動でトリガーすることによって、関数を開始できます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-175">You can start a function by manually triggering the configured trigger.</span></span> <span data-ttu-id="e80e1-176">たとえば、HTTP トリガーを使用している場合、Postman や cURL などのツールを使用して、ご自分の関数エンドポイント URL への HTTP 要求を開始することができます。これは HTTP のトリガー定義から利用できます (**[Get function URL]\(関数の URL の取得\)**)。</span><span class="sxs-lookup"><span data-stu-id="e80e1-176">For instance, if you are using an HTTP trigger - you can use a tool such as Postman or cURL to initiate an HTTP request to your function endpoint URL which is available from the HTTP trigger definition (**Get function URL**).</span></span>  

### <a name="testing-in-the-azure-portal"></a><span data-ttu-id="e80e1-177">Azure portal でテストする</span><span class="sxs-lookup"><span data-stu-id="e80e1-177">Testing in the Azure portal</span></span>

<span data-ttu-id="e80e1-178">ポータルにも、関数をテストするための便利な手段が用意されています。</span><span class="sxs-lookup"><span data-stu-id="e80e1-178">The portal also provides a convenient way to test your functions.</span></span> <span data-ttu-id="e80e1-179">コード ウィンドウの右側に、タブ付きのポップアップ ナビゲーション メニューがあります。</span><span class="sxs-lookup"><span data-stu-id="e80e1-179">On the right side of the code window, there is a flyout tabbed navigation menu.</span></span> <span data-ttu-id="e80e1-180">このメニューには、**[テスト]** 項目が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e80e1-180">This menu contains a **Test** item.</span></span> <span data-ttu-id="e80e1-181">メニューを展開してこのタブを選択すると、別の方法で関数を実行し、結果を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-181">Expanding the menu and selecting this tab gives you another way to execute your function and view the result.</span></span> <span data-ttu-id="e80e1-182">このテスト ウィンドウで **[実行]** をクリックすると、その結果が出力ウィンドウに状態コードと共に表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-182">When you click **Run** in this test window, the results are displayed in the output window, along with a status code.</span></span> 

## <a name="monitoring-dashboard"></a><span data-ttu-id="e80e1-183">監視ダッシュボード</span><span class="sxs-lookup"><span data-stu-id="e80e1-183">Monitoring dashboard</span></span>

<span data-ttu-id="e80e1-184">関数を監視する機能は、開発時および運用環境で重要です。</span><span class="sxs-lookup"><span data-stu-id="e80e1-184">The ability to monitor your functions is critical during development and in production.</span></span> <span data-ttu-id="e80e1-185">Azure portal では、Application Insights の統合を有効にした場合に利用できる監視ダッシュボードを提供します。</span><span class="sxs-lookup"><span data-stu-id="e80e1-185">The Azure portal provides a monitoring dashboard available if you turn on the Application Insights integration.</span></span> <span data-ttu-id="e80e1-186">関数アプリのナビゲーション メニューでは、関数ノードを展開すると、**[監視]** メニュー項目が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-186">In the function app navigation menu, once you expand the function node you'll see a **Monitor** menu item.</span></span> <span data-ttu-id="e80e1-187">この監視ダッシュボードでは、関数の実行履歴を表示する簡単な方法が提供され、Application Insights によって設定されたタイムスタンプ、結果コード、期間、操作 ID が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-187">This monitor dashboard provides a quick way to view the history of function executions and displays the timestamp, result code, duration, and operation ID populated by Application Insights.</span></span>

![関数の呼び出しの成功と失敗の一覧を示している、**[監視]** 関数のメニュー項目から起動した監視ダッシュボードのスクリーンショット。](../media-draft/4-monitor-function.png)

## <a name="streaming-log-window"></a><span data-ttu-id="e80e1-189">ストリーミング ログ ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="e80e1-189">Streaming log window</span></span>

<span data-ttu-id="e80e1-190">Azure portal でデバッグのために、ご自分の関数にログ ステートメントを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-190">You're also able to add logging statements to your function for debugging in the Azure portal.</span></span> <span data-ttu-id="e80e1-191">言語ごとに呼び出されたメソッドによって、情報をログ記録するために使用される可能性がある "ログ記録" オブジェクトが、コード ウィンドウの下部にあるタブ付きのポップアップ メニューに配置されたログ ウィンドウに渡されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-191">The called methods for each language are passed a "logging" object which may be used to log information to the log window located in a tabbed flyout menu located at the bottom of the code window.</span></span> 

<span data-ttu-id="e80e1-192">次の JavaScript コード スニペットは、`context.log` メソッドを使用してメッセージを記録する方法について示します (`context` オブジェクトはハンドラーに渡されます)。</span><span class="sxs-lookup"><span data-stu-id="e80e1-192">The following JavaScript code snippet shows how to log a message using the `context.log` method (the `context` object is passed to the handler).</span></span>

```javascript
  context.log('Enter your logging statement here');
```  

<span data-ttu-id="e80e1-193">C# では `log.Info` メソッドを使用して同じことを行えます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-193">We could the same thing in C# using the `log.Info` method.</span></span> <span data-ttu-id="e80e1-194">この場合、`log` オブジェクトは、その関数を処理する C# メソッドに渡されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-194">In this case, the `log` object is passed ot the C# method processing the function.</span></span>

```csharp
  log.Info("Enter your logging statement here");
```

### <a name="errors-and-warnings-window"></a><span data-ttu-id="e80e1-195">エラーと警告ウィンドウ</span><span class="sxs-lookup"><span data-stu-id="e80e1-195">Errors and warnings window</span></span>

<span data-ttu-id="e80e1-196">エラーと警告ウィンドウ タブは、ログ ウィンドウと同じポップアップ メニューで検索できます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-196">You can locate the errors and warnings window tab in the same flyout menu as the log window.</span></span> <span data-ttu-id="e80e1-197">このウィンドウには、コード内のコンパイル エラーと警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="e80e1-197">This window will show compilation errors and warnings within your code.</span></span>
