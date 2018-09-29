<span data-ttu-id="13721-101">Text Analytics API サービスを呼び出してセンチメント スコアを返すように関数の実装を更新しましょう。</span><span class="sxs-lookup"><span data-stu-id="13721-101">Let's update our function implementation to call the Text Analytics API service and get back a sentiment score.</span></span>

1. <span data-ttu-id="13721-102">ポータルの関数アプリで関数 [!INCLUDE [func-name-discover](./func-name-discover.md)] を選択します。</span><span class="sxs-lookup"><span data-stu-id="13721-102">Select our function, [!INCLUDE [func-name-discover](./func-name-discover.md)], in our function app in the portal.</span></span>

1. <span data-ttu-id="13721-103">画面の右側にある **[ファイルの表示]** メニューを展開します。</span><span class="sxs-lookup"><span data-stu-id="13721-103">Expand the **View files** menu on the right of the screen.</span></span>

1. <span data-ttu-id="13721-104">**[ファイルの表示]** タブで **[index.js]** を選択し、エディターでコード ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="13721-104">Under the **View files** tab, select **index.js** to open the code file in the editor.</span></span>

1. <span data-ttu-id="13721-105">**index.js** の内容全体を次の JavaScript に変更し、**[保存]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="13721-105">Replace the entire content of **index.js** with the following JavaScript and **Save**.</span></span>

    [!code-javascript[](../code/discover-sentiment-sort.js?highlight=7)]

1. <span data-ttu-id="13721-106">貼り付けたコードの `accessKey` 値を、このモジュールで先に保存した Text Analytics API のアクセス キーに変更します。</span><span class="sxs-lookup"><span data-stu-id="13721-106">Update the value of `accessKey` in the code you pasted with the access key for the Text Analytics API that you saved earlier in this module.</span></span> 

1. <span data-ttu-id="13721-107">`uri` 値をアクセス キーの取得元であるリージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="13721-107">Update the `uri` value with the region from which you obtained your access key.</span></span>

<span data-ttu-id="13721-108">このコードで何が起こっているかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="13721-108">Let's look at what's happening in this code:</span></span>

- <span data-ttu-id="13721-109">Text Analytics サービスを呼び出すたびにアクセス キーが必要になりますが、これは `Ocp-Apim-Subscription-Key` ヘッダーとして追加します。</span><span class="sxs-lookup"><span data-stu-id="13721-109">Each call to the Text Analytics service, needs the access key, which we add as the `Ocp-Apim-Subscription-Key` header.</span></span> 
- <span data-ttu-id="13721-110">各呼び出しは、コード内で `uri` により定義されているリージョン固有のエンドポイントに行われます。</span><span class="sxs-lookup"><span data-stu-id="13721-110">Each call is made to a region-specific endpoint, defined by `uri` in our code.</span></span>
- <span data-ttu-id="13721-111">コード ファイルの末尾に、`documents` 配列を定義してあります。</span><span class="sxs-lookup"><span data-stu-id="13721-111">At the bottom of the code file, we've defined a `documents` array.</span></span> <span data-ttu-id="13721-112">この配列は、Text Analytics サービスに送信するペイロードです。</span><span class="sxs-lookup"><span data-stu-id="13721-112">This array is the payload we send to the Text Analytics service.</span></span>
- <span data-ttu-id="13721-113">ここでは、`documents` 配列には単一のエントリがあります。これは、関数をトリガーしたキュー メッセージです。</span><span class="sxs-lookup"><span data-stu-id="13721-113">The `documents` array has a single entry in this case, which is the queue message that triggered our function.</span></span> <span data-ttu-id="13721-114">この配列内には 1 つのドキュメントしかありませんが、ソリューションが一度に 1 つのメッセージしか処理できないわけではありません。</span><span class="sxs-lookup"><span data-stu-id="13721-114">Although we only have one document in our array, it doesn't mean that our solution can only handle one message at a time.</span></span> <span data-ttu-id="13721-115">Azure Functions ランタイムは、関数のいくつかのインスタンスを "*並列*" で呼び出して、メッセージを一括して取得および処理します。</span><span class="sxs-lookup"><span data-stu-id="13721-115">The Azure Functions runtime retrieves and processes messages in batches, calling several instances of our function *in parallel*.</span></span> <span data-ttu-id="13721-116">現在、既定のバッチ サイズは 16 で、最大バッチ サイズは 32 です。</span><span class="sxs-lookup"><span data-stu-id="13721-116">Currently, the default batch size is 16 and the maximum batch size is 32.</span></span>
- <span data-ttu-id="13721-117">`id` は、配列内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="13721-117">The `id` must be unique within the array.</span></span> <span data-ttu-id="13721-118">`language` プロパティは、ドキュメント テキストの言語を指定するものです。</span><span class="sxs-lookup"><span data-stu-id="13721-118">The `language` property specifies the language of the document text.</span></span>
- <span data-ttu-id="13721-119">次に、`get_sentiments` メソッドを呼び出します。このメソッドは、HTTPS モジュールを使用して、Text Analytics API を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="13721-119">We then call our method `get_sentiments`, which uses the HTTPS module to make the call to Text Analytics API.</span></span> <span data-ttu-id="13721-120">各要求のヘッダーで、サブスクリプションまたはアクセス キーを渡している点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="13721-120">Notice that we pass our subscription, or access, key in the header of every request.</span></span>
- <span data-ttu-id="13721-121">サービスが終了するとき、`response_handler` が呼び出され、コンソールへの応答が `context.log` を使用してログに記録されます</span><span class="sxs-lookup"><span data-stu-id="13721-121">When the service returns, our `response_handler` is called, and we log the response to the console using `context.log`</span></span>


## <a name="try-it-out"></a><span data-ttu-id="13721-122">試してみる</span><span class="sxs-lookup"><span data-stu-id="13721-122">Try it out</span></span>

<span data-ttu-id="13721-123">キューへの分類を見る前に、テスト実行用にどのようなものがあるかを確認しておきましょう。</span><span class="sxs-lookup"><span data-stu-id="13721-123">Before we look at sorting into queues, let's take what we have for a test run.</span></span>

1. <span data-ttu-id="13721-124">ポータルの Function App 領域で関数 [!INCLUDE [func-name-discover](./func-name-discover.md)] を選択した状態で、右端にある **[テスト]** メニュー項目をクリックします。</span><span class="sxs-lookup"><span data-stu-id="13721-124">With our function, [!INCLUDE [func-name-discover](./func-name-discover.md)], selected in the Function Apps area of the portal, click on the **Test** menu item on the far right.</span></span>

1. <span data-ttu-id="13721-125">**[テスト]** メニュー項目を選択し、テスト パネルを開いていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="13721-125">Select the **Test** menu item, and verify that you have the test panel open.</span></span>

1. <span data-ttu-id="13721-126">スクリーンショットに示すように、要求本文にテキストの文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="13721-126">Add a string of text into the request body as shown in the screenshot.</span></span>

    ![展開された関数のテスト パネルを示すスクリーンショット。](../media/test-panel-open-small.png)

1.  <span data-ttu-id="13721-128">テスト パネルの下部にある **[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="13721-128">Click **Run** at the bottom of the test panel.</span></span>

1. <span data-ttu-id="13721-129">メイン画面の左下のコード エディターの下で、**[ログ]** タブが展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="13721-129">Make sure the **Logs** tab is expanded at the bottom left of the main screen, under the code editor.</span></span>

1. <span data-ttu-id="13721-130">**[ログ]** タブで、関数が完了したというログ情報が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="13721-130">Verify that the **Logs** tab displays log information that the function completed.</span></span> <span data-ttu-id="13721-131">ウィンドウには、Text Analytics API 呼び出しからの応答も表示されます。</span><span class="sxs-lookup"><span data-stu-id="13721-131">The window will also display the response from the Text Analytics API call.</span></span>

    ![テスト パネルと成功したテストの結果を示すスクリーンショット。](../media/sentiment-response-log1.png)

<span data-ttu-id="13721-133">うまくいきました。</span><span class="sxs-lookup"><span data-stu-id="13721-133">Congratulations!</span></span> <span data-ttu-id="13721-134">[!INCLUDE [func-name-discover](./func-name-discover.md)] は設計どおりに動作します。</span><span class="sxs-lookup"><span data-stu-id="13721-134">The [!INCLUDE [func-name-discover](./func-name-discover.md)] works as designed.</span></span> <span data-ttu-id="13721-135">この例では、非常に明るいメッセージを渡し、0.98 を超えるスコアを受け取りました。</span><span class="sxs-lookup"><span data-stu-id="13721-135">In this example, we passed in a very upbeat message and received a score of over 0.98.</span></span> <span data-ttu-id="13721-136">それよりも明るくないメッセージに変更してテストを再実行し、応答に注意してみてください。</span><span class="sxs-lookup"><span data-stu-id="13721-136">Try changing the message to something less optimistic, rerun the test, and note the response.</span></span>

## <a name="add-a-message-to-the-queue"></a><span data-ttu-id="13721-137">メッセージをキューに追加する</span><span class="sxs-lookup"><span data-stu-id="13721-137">Add a message to the queue</span></span>

<span data-ttu-id="13721-138">テストを繰り返してみましょう。</span><span class="sxs-lookup"><span data-stu-id="13721-138">Let's repeat the test.</span></span> <span data-ttu-id="13721-139">今回は、ポータルのテスト ウィンドウを使用する代わりに、実際に入力キューにメッセージを入れて、何が起きるかを監視します。</span><span class="sxs-lookup"><span data-stu-id="13721-139">This time, instead of using the Test window of the portal, we'll actually place a message into the input queue and watch what happens.</span></span>

1. <span data-ttu-id="13721-140">ポータルの **[リソース グループ]** セクションで、該当するリソース グループに移動します。</span><span class="sxs-lookup"><span data-stu-id="13721-140">Navigate to your resource group in the **Resource Groups** section of the portal.</span></span>

1. <span data-ttu-id="13721-141">このレッスンで使用されたリソース グループである、<rgn>[サンドボックス リソース グループ名]</rgn> を選択します。</span><span class="sxs-lookup"><span data-stu-id="13721-141">Select <rgn>[sandbox resource group name]</rgn>, the resource group used in this lesson.</span></span>

1. <span data-ttu-id="13721-142">表示された **[リソース グループ]** パネルで、[ストレージ アカウント] エントリを検索して選択します。</span><span class="sxs-lookup"><span data-stu-id="13721-142">In the **Resource group** panel that appears, locate the Storage Account entry, and select it.</span></span>

    ![スクリーンショット ([リソース グループ] ウィンドウでストレージ アカウントを選択したところ)。](../media/select-storage-account.png)

1. <span data-ttu-id="13721-144">[ストレージ アカウント] メイン ウィンドウの左側のメニューから **[ストレージ エクスプローラー (プレビュー)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="13721-144">Select **Storage Explorer (preview)** from the left menu of the Storage Account main window.</span></span> <span data-ttu-id="13721-145">この操作により、ポータル内で Azure Storage Explorer が開きます。</span><span class="sxs-lookup"><span data-stu-id="13721-145">This action opens the Azure Storage Explorer inside the portal.</span></span> 

    ![ストレージ アカウントが示されているストレージ エクスプローラーのスクリーンショット (現時点でキューはなし)。](../media/sa-no-queue.png)

    <span data-ttu-id="13721-147">ご覧のとおり、このストレージ アカウントにはまだキューがないので追加しましょう。</span><span class="sxs-lookup"><span data-stu-id="13721-147">As you can see, we don't have any queues in this storage account yet, so let's add one.</span></span>

5. <span data-ttu-id="13721-148">このレッスンの前の方で、トリガーに関連付けられているキューに **new-feedback-q** という名前を付けました。</span><span class="sxs-lookup"><span data-stu-id="13721-148">If you remember from earlier in this lesson, we named the queue associated with our trigger **new-feedback-q**.</span></span> <span data-ttu-id="13721-149">ストレージ エクスプローラーで **[キュー]** 項目を右クリックし、*[キューの作成]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="13721-149">Right-click on the **Queues** item in the Storage Explorer, and select *Create Queue*.</span></span>

1. <span data-ttu-id="13721-150">ダイアログ ボックスが開いたら、「**new-feedback-q**」と入力して **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="13721-150">In the dialog that opens, enter **new-feedback-q** and click **OK**.</span></span> <span data-ttu-id="13721-151">これで、入力キューができました。</span><span class="sxs-lookup"><span data-stu-id="13721-151">We now have our input queue.</span></span>

1. <span data-ttu-id="13721-152">左側のメニューで新しいキューを選択して、このキューのデータ エクスプローラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="13721-152">Select the new queue in the left-hand menu to see the data explorer for this queue.</span></span> <span data-ttu-id="13721-153">当然、キューは空です。</span><span class="sxs-lookup"><span data-stu-id="13721-153">As expected, the queue is empty.</span></span> <span data-ttu-id="13721-154">ウィンドウ上部にある **[メッセージの追加]** コマンドを使って、キューにメッセージを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="13721-154">Let's add a message to the queue using the **Add Message** command at the top of the window.</span></span>

1. <span data-ttu-id="13721-155">**[メッセージの追加]** ダイアログ ボックスの **[メッセージ テキスト]** フィールドに「This message came from our input queue, new-feedback-q」 (このメッセージは入力キューの new-feedback-q からのものです) と入力し、ダイアログの下部にある **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="13721-155">In the **Add Message** dialog, enter "This message came from our input queue, new-feedback-q" into the **Message text** field, and click **OK** at the bottom of the dialog.</span></span>

1. <span data-ttu-id="13721-156">データ エクスプローラーで、次のスクリーンショットのようなメッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="13721-156">Observe the message, similar to the message in the following screenshot, in the data explorer.</span></span>
    <span data-ttu-id="13721-157">![ストレージ アカウントが示されているストレージ エクスプローラーのスクリーンショット (作成したメッセージがキューにある状態)。](../media/message-in-input-queue.png)</span><span class="sxs-lookup"><span data-stu-id="13721-157">![Screenshot of Storage Explorer showing our storage account, with the message we created in the queue.](../media/message-in-input-queue.png)</span></span>

1. <span data-ttu-id="13721-158">数秒後、**[最新の情報に更新]** をクリックして、キューのビューを最新の情報に更新します。</span><span class="sxs-lookup"><span data-stu-id="13721-158">After a few seconds, click **Refresh** to refresh the view of the queue.</span></span> <span data-ttu-id="13721-159">キューが**空**であるかどうかをもう一度確認します。</span><span class="sxs-lookup"><span data-stu-id="13721-159">Observe that the queue is **empty** once again.</span></span> <span data-ttu-id="13721-160">何かがキューからメッセージを読み取ったはずです。</span><span class="sxs-lookup"><span data-stu-id="13721-160">Something must have read the message from the queue.</span></span>

1. <span data-ttu-id="13721-161">ポータルで関数に戻り、**[モニター]** タブを開きます。一覧で最新のメッセージを選択します。</span><span class="sxs-lookup"><span data-stu-id="13721-161">Navigate back to our function in the portal, and open the **Monitor** tab. Select the newest message in the list.</span></span> <span data-ttu-id="13721-162">new-feedback-q に投稿したキュー メッセージが関数によって処理されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="13721-162">Observe that our function processed the queue message we had posted to the new-feedback-q.</span></span> <span data-ttu-id="13721-163">このログでは結果が遅れることがあります。そのため、場合によっては、数分待ち、*[更新]* を押す必要があります。</span><span class="sxs-lookup"><span data-stu-id="13721-163">Results may be delayed by in this log, so you might have to wait a few minutes and hit *Refresh*.</span></span>

    ![new-feedback-q に投稿したキュー メッセージが関数によって処理されたことを示すエントリが表示されている [モニター] ダッシュボードのスクリーンショット。](../media/message-in-monitor.png)

    <span data-ttu-id="13721-165">このテストでは、キューに何かを追加した後で関数によってそれが処理されるのを確認する、完全なラウンドトリップを行いました。</span><span class="sxs-lookup"><span data-stu-id="13721-165">In this test, we did a complete round trip of posting something into our queue and then seeing the function process it.</span></span>

<span data-ttu-id="13721-166">ソリューションは着実に進歩しています。</span><span class="sxs-lookup"><span data-stu-id="13721-166">We're making progress with our solution!</span></span> <span data-ttu-id="13721-167">関数が有益なことを実行できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="13721-167">Our function is now doing something useful.</span></span> <span data-ttu-id="13721-168">入力キューからテキストを受け取り、Text Analytics API サービスを呼び出してセンチメント スコアを取得しています。</span><span class="sxs-lookup"><span data-stu-id="13721-168">It's receiving text from our input queue and then calling out to the Text Analytics API service to get a sentiment score.</span></span> <span data-ttu-id="13721-169">また、Azure portal と Storage Explorer を通じて関数をテストする方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="13721-169">We've also learned how to test our function through the Azure portal and the Storage Explorer.</span></span> <span data-ttu-id="13721-170">次の演習では、出力バインディングを使用したキューへの書き込みがいかに簡単であるかを見ていきます。</span><span class="sxs-lookup"><span data-stu-id="13721-170">In the next exercise, we'll see how easy it is to write to queues using output bindings.</span></span>