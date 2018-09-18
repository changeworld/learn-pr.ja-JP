<span data-ttu-id="6e1db-101">ソリューションをビルドするには、コードをホストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6e1db-101">To build our solution, we'll need to host some code.</span></span>  <span data-ttu-id="6e1db-102">Azure Functions の関数アプリは、ロジックをホストするのに適しています。</span><span class="sxs-lookup"><span data-stu-id="6e1db-102">An Azure Functions function app is a good place to host our logic.</span></span> 

## <a name="create-a-function-app-to-host-our-function"></a><span data-ttu-id="6e1db-103">関数をホストする関数アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="6e1db-103">Create a Function App to host our function</span></span>

[!INCLUDE [resource-group-note](./rg-notice.md)]

1. <span data-ttu-id="6e1db-104">Azure アカウントを使用して Azure portal ([https://portal.azure.com](https://portal.azure.com?azure-portal=true)) にサインインしていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-104">Make sure you are signed in to the Azure portal at [https://portal.azure.com](https://portal.azure.com?azure-portal=true) with your Azure account.</span></span>

1. <span data-ttu-id="6e1db-105">Azure portal の左上隅にある **[リソースの作成]** ボタンをクリックし、**[コンピューティング]** > **[Function App]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-105">Select the **Create a resource** button found on the upper left-hand corner of the Azure portal, then select **Compute** > **Function App**.</span></span>

1. <span data-ttu-id="6e1db-106">次の表に示す関数アプリの設定を入力します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-106">Enter the function app settings as specified in the following table.</span></span>


    | <span data-ttu-id="6e1db-107">設定</span><span class="sxs-lookup"><span data-stu-id="6e1db-107">Setting</span></span>      | <span data-ttu-id="6e1db-108">推奨値</span><span class="sxs-lookup"><span data-stu-id="6e1db-108">Suggested value</span></span>  | <span data-ttu-id="6e1db-109">説明</span><span class="sxs-lookup"><span data-stu-id="6e1db-109">Description</span></span>                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | <span data-ttu-id="6e1db-110">**アプリ名**</span><span class="sxs-lookup"><span data-stu-id="6e1db-110">**App name**</span></span> | <span data-ttu-id="6e1db-111">グローバルに一意の名前</span><span class="sxs-lookup"><span data-stu-id="6e1db-111">Globally unique name</span></span> | <span data-ttu-id="6e1db-112">新しい関数アプリを識別する名前。</span><span class="sxs-lookup"><span data-stu-id="6e1db-112">Name that identifies your new function app.</span></span> <span data-ttu-id="6e1db-113">有効な文字は、`a-z`、`0-9`、および `-` です。</span><span class="sxs-lookup"><span data-stu-id="6e1db-113">Valid characters are `a-z`, `0-9`, and `-`.</span></span>  | 
    | <span data-ttu-id="6e1db-114">**サブスクリプション**</span><span class="sxs-lookup"><span data-stu-id="6e1db-114">**Subscription**</span></span> | <span data-ttu-id="6e1db-115">お使いのサブスクリプション</span><span class="sxs-lookup"><span data-stu-id="6e1db-115">Your subscription</span></span> | <span data-ttu-id="6e1db-116">この新しい関数アプリが作成されるサブスクリプション。</span><span class="sxs-lookup"><span data-stu-id="6e1db-116">The subscription under which this new function app is created.</span></span> | 
    | <span data-ttu-id="6e1db-117">**リソース グループ**</span><span class="sxs-lookup"><span data-stu-id="6e1db-117">**Resource Group**</span></span>|  [!INCLUDE [resource-group-name](./rg-name.md)] | <span data-ttu-id="6e1db-118">関数アプリを作成するリソース グループの名前。</span><span class="sxs-lookup"><span data-stu-id="6e1db-118">Name for the  resource group in which to create your function app.</span></span><br/><br/><span data-ttu-id="6e1db-119">必ず **[既存のものを使用]** を選択し、前回の演習で作成したリソース グループを使用します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-119">Make sure to select **Use existing** and use the resource group that we created in the last exercise.</span></span> <span data-ttu-id="6e1db-120">これにより、このモジュールで作成したすべてのリソースがまとめられます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-120">That way, all resource we made in this module are kept together.</span></span> | 
    | <span data-ttu-id="6e1db-121">**OS**</span><span class="sxs-lookup"><span data-stu-id="6e1db-121">**OS**</span></span> | <span data-ttu-id="6e1db-122">Windows</span><span class="sxs-lookup"><span data-stu-id="6e1db-122">Windows</span></span> | <span data-ttu-id="6e1db-123">関数アプリをホストするオペレーティング システム。</span><span class="sxs-lookup"><span data-stu-id="6e1db-123">The operating system that hosts the function app.</span></span>  |
    | <span data-ttu-id="6e1db-124">**ホスティング**</span><span class="sxs-lookup"><span data-stu-id="6e1db-124">**Hosting**</span></span> |   <span data-ttu-id="6e1db-125">従量課金プラン</span><span class="sxs-lookup"><span data-stu-id="6e1db-125">Consumption plan</span></span> | <span data-ttu-id="6e1db-126">Function App にどのようにリソースが割り当てられるかを定義するホスティング プラン。</span><span class="sxs-lookup"><span data-stu-id="6e1db-126">Hosting plan that defines how resources are allocated to your function app.</span></span> <span data-ttu-id="6e1db-127">既定の**従量課金プラン**では、関数での必要に応じてリソースが動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-127">In the default **Consumption Plan**, resources are added dynamically as required by your functions.</span></span> <span data-ttu-id="6e1db-128">この[サーバーレス](https://azure.microsoft.com/overview/serverless-computing/) ホスティングでは、関数が実行された時間にのみ課金されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-128">In this [serverless](https://azure.microsoft.com/overview/serverless-computing/) hosting, you only pay for the time your functions run.</span></span>   |
    | <span data-ttu-id="6e1db-129">**場所**</span><span class="sxs-lookup"><span data-stu-id="6e1db-129">**Location**</span></span> | <span data-ttu-id="6e1db-130">米国西部</span><span class="sxs-lookup"><span data-stu-id="6e1db-130">West US</span></span> | <span data-ttu-id="6e1db-131">ユーザーの最寄りの[リージョン](https://azure.microsoft.com/regions/)、または関数がアクセスする他のサービスの近くのリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-131">Choose a [region](https://azure.microsoft.com/regions/) near you or near other services your functions access.</span></span><br/><br/><span data-ttu-id="6e1db-132">前回の演習で Text Analytics API アカウントを作成したときに使用したリージョンを選択します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-132">Select the same region that you used when creating the Text Analytics API account in the last exercise.</span></span> |
    | <span data-ttu-id="6e1db-133">**ストレージ アカウント**</span><span class="sxs-lookup"><span data-stu-id="6e1db-133">**Storage account**</span></span> |  <span data-ttu-id="6e1db-134">グローバルに一意の名前</span><span class="sxs-lookup"><span data-stu-id="6e1db-134">Globally unique name</span></span> |  <span data-ttu-id="6e1db-135">関数アプリによって使用される新しいストレージ アカウントの名前。</span><span class="sxs-lookup"><span data-stu-id="6e1db-135">Name of the new storage account used by your function app.</span></span> <span data-ttu-id="6e1db-136">ストレージ アカウント名の長さは 3 から 24 文字である必要があり、数字と小文字のみを使用できます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-136">Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.</span></span> <span data-ttu-id="6e1db-137">このダイアログでは、アプリに指定した名前から派生した一意の名前がこのフィールドに入力されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-137">This dialog populates the field with a unique name that is derived from the name you gave the app.</span></span> <span data-ttu-id="6e1db-138">ただし、別の名前や既存のアカウントを自由に使用できます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-138">However, feel free to use a different name or even an existing account.</span></span> |

3. <span data-ttu-id="6e1db-139">**[作成]** を選択して、関数アプリをプロビジョニングし、デプロイします。</span><span class="sxs-lookup"><span data-stu-id="6e1db-139">Select **Create** to provision and deploy the function app.</span></span>

4. <span data-ttu-id="6e1db-140">ポータルの右上隅の通知アイコンを選択し、**デプロイが進行中**であることを示す次のようなメッセージが表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-140">Select the Notification icon in the upper-right corner of the portal and watch for a **Deployment in progress** message similar to the following message.</span></span>

![関数アプリのデプロイが進行中であることを示す通知](../media-draft/func-app-deploy-progress-small.PNG)

5. <span data-ttu-id="6e1db-142">デプロイには時間がかかることがあります。</span><span class="sxs-lookup"><span data-stu-id="6e1db-142">Deployment can take some time.</span></span> <span data-ttu-id="6e1db-143">そのため、通知ハブを表示した状態で、**デプロイに成功**したことを示す次のようなメッセージが表示されるまで待ちます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-143">So, stay in the notification hub and  watch for a **Deployment succeeded** message similar to the following message.</span></span>

![関数アプリのデプロイが完了したことを示す通知](../media-draft/func-app-text-analytics-deploy-success.png)

6. <span data-ttu-id="6e1db-145">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="6e1db-145">Congratulations!</span></span> <span data-ttu-id="6e1db-146">関数アプリを作成し、デプロイしました。</span><span class="sxs-lookup"><span data-stu-id="6e1db-146">You've created and deployed your function app.</span></span> <span data-ttu-id="6e1db-147">**[リソースに移動]** を選択して、新しい関数アプリを確認します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-147">Select **Go to resource** to view your new function app.</span></span>

>[!TIP]
><span data-ttu-id="6e1db-148">ポータルで目的の関数アプリが見つからない場合は、[Azure portal でお気に入りに Function App を追加](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite)してみてください。</span><span class="sxs-lookup"><span data-stu-id="6e1db-148">Having trouble finding your function apps in the portal, try [adding Function Apps to your favorites in the Azure portal](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).</span></span>

## <a name="create-a-function-to-hold-our-logic"></a><span data-ttu-id="6e1db-149">ロジックを保持する関数を作成する</span><span class="sxs-lookup"><span data-stu-id="6e1db-149">Create a function to hold our logic</span></span>

<span data-ttu-id="6e1db-150">関数アプリを用意できたので、次に関数を作成します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-150">Now that we have a function app, it's time to create a function.</span></span> <span data-ttu-id="6e1db-151">関数はトリガーによってアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-151">A function is activated through a trigger.</span></span> <span data-ttu-id="6e1db-152">このモジュールでは、キュー トリガーを使用します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-152">In this module, we'll use a Queue trigger.</span></span> <span data-ttu-id="6e1db-153">ランタイムによってキューがポーリングされ、新しいメッセージを処理するためにこの関数が起動されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-153">The runtime will poll a queue and start this function to process a new message.</span></span>

1. <span data-ttu-id="6e1db-154">新しい関数アプリを展開し、**[関数]** コレクションをポイントします。</span><span class="sxs-lookup"><span data-stu-id="6e1db-154">Expand your new function app, then hover over the **Functions** collection.</span></span> <span data-ttu-id="6e1db-155">表示された追加 (**+**) ボタンをクリックして、関数作成プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-155">Select the Add (**+**) button when it appears to start the function creation process.</span></span>

![ユーザーが [関数] メニュー項目をポイントするとプラス記号が表示されることを示すアニメーション。](../media-draft/func-app-plus-hover-small.gif)

2. <span data-ttu-id="6e1db-157">表示された **[Get started quickly]\(すぐに使用を開始する\)** ページで、**[カスタム関数]** を選択します。これにより、使用可能な関数テンプレートのリストが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-157">In the **Get started quickly** page that now appears, select **Custom function**, which loads the list of available function templates.</span></span> 

1. <span data-ttu-id="6e1db-158">**[キュー トリガー]** テンプレート リスト エントリで **[JavaScript]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-158">Select **JavaScript** on the **Queue trigger** template list entry.</span></span>

![[キュー トリガー] エントリで選択した JavaScript を使用した Azure Functions テンプレートのスクリーンショット。](../media-draft/quickstart-select-queue-trigger.png)

1. <span data-ttu-id="6e1db-160">表示された **[新しい関数]** ダイアログに次の値を入力します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-160">In the **New Function** dialog that appears, enter the following values.</span></span>


|<span data-ttu-id="6e1db-161">プロパティ</span><span class="sxs-lookup"><span data-stu-id="6e1db-161">Property</span></span>  |<span data-ttu-id="6e1db-162">値</span><span class="sxs-lookup"><span data-stu-id="6e1db-162">Value</span></span>  |
|---------|---------|
|<span data-ttu-id="6e1db-163">言語</span><span class="sxs-lookup"><span data-stu-id="6e1db-163">Language</span></span>     |   <span data-ttu-id="6e1db-164">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6e1db-164">**JavaScript**</span></span>      |
|<span data-ttu-id="6e1db-165">名前</span><span class="sxs-lookup"><span data-stu-id="6e1db-165">Name</span></span>     |   <span data-ttu-id="6e1db-166">**discover-sentiment-function**</span><span class="sxs-lookup"><span data-stu-id="6e1db-166">**discover-sentiment-function**</span></span>      |
|<span data-ttu-id="6e1db-167">キュー名</span><span class="sxs-lookup"><span data-stu-id="6e1db-167">Queue name</span></span>     |   <span data-ttu-id="6e1db-168">**new-feedback-q**</span><span class="sxs-lookup"><span data-stu-id="6e1db-168">**new-feedback-q**</span></span>      |
|<span data-ttu-id="6e1db-169">ストレージ アカウント接続</span><span class="sxs-lookup"><span data-stu-id="6e1db-169">Storage account connection</span></span>        |  <span data-ttu-id="6e1db-170">**AzureWebJobsDashboard**</span><span class="sxs-lookup"><span data-stu-id="6e1db-170">**AzureWebJobsDashboard**</span></span>       |

![[キュー トリガー] エントリで選択した JavaScript を使用した Azure Functions テンプレートのスクリーンショット。](../media-draft/new-function-dialog.png)

5. <span data-ttu-id="6e1db-172">**[作成]** を選択して、関数作成プロセスを開始します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-172">Select **Create** to begin the function creation process.</span></span>

1. <span data-ttu-id="6e1db-173">キュー トリガー関数テンプレートを使用して、選択した言語で関数が作成されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-173">A function is created in your chosen language using the Queue Trigger function template.</span></span> <span data-ttu-id="6e1db-174">このモジュールでは JavaScript で関数を実装しますが、[サポートされている任意の言語](https://docs.microsoft.com/azure/azure-functions/supported-languages)で関数を作成できます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-174">While we'll implement the function in JavaScript in this module, you can create a function in any [supported language](https://docs.microsoft.com/azure/azure-functions/supported-languages).</span></span>

<span data-ttu-id="6e1db-175">作成プロセスが完了すると、ポータルでコード エディターが開き、*index.js* ページが読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-175">When the create process is complete, the code editor opens in the portal and loads the *index.js* page.</span></span> <span data-ttu-id="6e1db-176">このファイルは、関数ロジックを記述するコード ファイルです。</span><span class="sxs-lookup"><span data-stu-id="6e1db-176">This file is the code file where we write our function logic.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="6e1db-177">試してみる</span><span class="sxs-lookup"><span data-stu-id="6e1db-177">Try it out</span></span>

<span data-ttu-id="6e1db-178">これまでの内容をテストしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="6e1db-178">Let's test what we have so far.</span></span> <span data-ttu-id="6e1db-179">コードはまだ記述していないので、このテストは、これまでに構成した内容を確認するために実行します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-179">We haven't written any code yet, so this test is to make sure what we've configured so far, runs.</span></span>

1. <span data-ttu-id="6e1db-180">コード エディターの上部にある **[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="6e1db-180">Click **Run** at the top of the code editor.</span></span>

2. <span data-ttu-id="6e1db-181">画面の下部に表示される **[ログ]** タブを確認します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-181">Observe the **Logs** tab that opens at the bottom of the screen.</span></span> <span data-ttu-id="6e1db-182">すべてが計画どおりに動作していれば、次のようなメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-182">If everything works as planned, you'll see a message similar to the following message.</span></span>
<span data-ttu-id="6e1db-183">![関数の呼び出しが成功したことを示す応答メッセージのスクリーンショット。](../media-draft/func-default-run.PNG)</span><span class="sxs-lookup"><span data-stu-id="6e1db-183">![Screenshot of response message of a successful call to our function.](../media-draft/func-default-run.PNG)</span></span>

<span data-ttu-id="6e1db-184">**[実行]** ボタンによって関数が起動され、*[テスト]* 要求ウィンドウから関数に、既定のテキストである**サンプル キュー データ**が渡されました。</span><span class="sxs-lookup"><span data-stu-id="6e1db-184">The **Run** button started our function and passed *sample queue data*, the default text from the **Test** request window to our function.</span></span>

<span data-ttu-id="6e1db-185">お疲れさまでした。</span><span class="sxs-lookup"><span data-stu-id="6e1db-185">Nice work!</span></span> <span data-ttu-id="6e1db-186">キューによってトリガーされる関数を関数アプリに正常に追加し、テストを実行して想定どおりに動作することを確認しました。</span><span class="sxs-lookup"><span data-stu-id="6e1db-186">You've successfully added a Queue-triggered function to your function app and tested to make sure it's working as expected!</span></span> <span data-ttu-id="6e1db-187">次の演習では、関数にさらに多くの機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-187">We'll add more functionality to the function in the next exercise.</span></span>

 <span data-ttu-id="6e1db-188">では、関数のもう 1 つのファイルである *function.json* 構成ファイルを簡単に見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="6e1db-188">Let's look briefly at the function's other file, the *function.json* config file.</span></span> <span data-ttu-id="6e1db-189">次の JSON リストに、このファイルの構成データが示されています。</span><span class="sxs-lookup"><span data-stu-id="6e1db-189">The configuration data from this file is shown in the following JSON listing.</span></span>

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "new-feedback-q",
      "connection": "AzureWebJobsDashboard"
    }
  ],
  "disabled": false
}
```

<span data-ttu-id="6e1db-190">ご覧のように、この関数には `queueTrigger` 型の **myQueueItem** という名前のトリガー バインディングがあります。</span><span class="sxs-lookup"><span data-stu-id="6e1db-190">As you can see, this function has a trigger binding named **myQueueItem** of type `queueTrigger`.</span></span> <span data-ttu-id="6e1db-191">**new-feedback-q** という名前のキューに新しいメッセージが到着すると、関数が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-191">When a new message arrives in the queue we've named **new-feedback-q**, our function is called.</span></span> <span data-ttu-id="6e1db-192">myQueueItem バインディング パラメーターを使用して、新しいメッセージを参照します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-192">We reference the new message through the myQueueItem binding parameter.</span></span> <span data-ttu-id="6e1db-193">このようにバインディングを使用すると、面倒な作業の一部が自動的に行われます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-193">Bindings really do take care of some of the heavy lifting for us!</span></span>

<span data-ttu-id="6e1db-194">次の手順では、Text Analytics API サービスを呼び出すコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="6e1db-194">In the next step, we'll add code to call the Text Analytics API service.</span></span>

>[!TIP]
><span data-ttu-id="6e1db-195">Azure portal で関数パネルの右側にある **[ファイルの表示]** メニューを展開すると、index.js と function.json を確認できます。</span><span class="sxs-lookup"><span data-stu-id="6e1db-195">You can see index.js and function.json by expanding the **View Files** menu on the right of the function panel in the Azure portal.</span></span> 

<span data-ttu-id="6e1db-196">この演習では、Azure Functions のインフラストラクチャを整えました。</span><span class="sxs-lookup"><span data-stu-id="6e1db-196">This exercise was all about getting our Azure Functions infrastructure in place.</span></span> <span data-ttu-id="6e1db-197">[!INCLUDE [input-q](./q-name-input.md)] という名前を付けたキューに新しいメッセージが到着したときに実行される、関数アプリでホストされる作業関数を作成しました。</span><span class="sxs-lookup"><span data-stu-id="6e1db-197">We have a working function hosted in a function app that runs when a new message arrives in our queue that we've named [!INCLUDE [input-q](./q-name-input.md)].</span></span> <span data-ttu-id="6e1db-198">本当に楽しくなるのは、次の演習で Microsoft Cognitive Service を呼び出して感情分析を実行するコードを追加してからです。</span><span class="sxs-lookup"><span data-stu-id="6e1db-198">The real fun begins in the next exercise, when we add code to call a Microsoft Cognitive Service to do sentiment analysis.</span></span>