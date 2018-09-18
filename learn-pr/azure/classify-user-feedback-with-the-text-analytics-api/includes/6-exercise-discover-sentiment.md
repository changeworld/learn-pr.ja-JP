<span data-ttu-id="af64b-101">Text Analytics API サービスを呼び出してセンチメント スコアを返すように関数の実装を更新しましょう。</span><span class="sxs-lookup"><span data-stu-id="af64b-101">Let's update our function implementation to call the Text Analytics API service and get back a sentiment score.</span></span>

1. <span data-ttu-id="af64b-102">Function App ポータルで目的の関数 ([!INCLUDE [func-name-discover](./func-name-discover.md)]) を選択します。</span><span class="sxs-lookup"><span data-stu-id="af64b-102">Select our function, [!INCLUDE [func-name-discover](./func-name-discover.md)], in the Function Apps portal.</span></span>

1. <span data-ttu-id="af64b-103">画面の右側にある **[ファイルの表示]** メニューを展開します。</span><span class="sxs-lookup"><span data-stu-id="af64b-103">Expand the **View files** menu on the right of the screen.</span></span>

1. <span data-ttu-id="af64b-104">**[ファイルの表示]** タブで **[index.js]** を選択し、エディターでコード ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="af64b-104">Under the **View files** tab, select **index.js** to open the code file in the editor.</span></span>

1. <span data-ttu-id="af64b-105">コード ファイルの内容全体を次の JavaScript に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="af64b-105">Replace the entire content of the code file with the following JavaScript.</span></span>

[!code-javascript[](../code/discover-sentiment-sort.js?highlight=7)]

<span data-ttu-id="af64b-106">このコードで何が起こっているかを見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="af64b-106">Let's look at what's happening in this code:</span></span>

- <span data-ttu-id="af64b-107">テキスト分析サービスを呼び出すには、コード スニペットで強調表示されている `accessKey` に、以前に保存しておいたキーを設定します。</span><span class="sxs-lookup"><span data-stu-id="af64b-107">To call the text analytics service, set `accessKey`, highlighted in the code snippet, to the key you saved earlier.</span></span>
- <span data-ttu-id="af64b-108">`uri` を、アクセス キーを取得したリージョンに更新します (そのリージョンが、この例で示されている *westus* ではない場合)。</span><span class="sxs-lookup"><span data-stu-id="af64b-108">Update `uri` to the region from which you obtained your access key, if that region is different than *westus* shown in this example.</span></span>
- <span data-ttu-id="af64b-109">コード ファイルの末尾に、`documents` 配列を定義してあります。</span><span class="sxs-lookup"><span data-stu-id="af64b-109">At the bottom of the code file, we've defined a `documents` array.</span></span> <span data-ttu-id="af64b-110">この配列は、Text Analytics サービスに送信するペイロードです。</span><span class="sxs-lookup"><span data-stu-id="af64b-110">This array is the payload we send to the Text Analytics service.</span></span> 
- <span data-ttu-id="af64b-111">ここでは、`documents` 配列には単一のエントリがあります。これは、関数をトリガーしたキュー メッセージです。</span><span class="sxs-lookup"><span data-stu-id="af64b-111">The `documents` array has a single entry in this case, which is the queue message that triggered our function.</span></span> <span data-ttu-id="af64b-112">この配列内には 1 つのドキュメントしかありませんが、ソリューションが一度に 1 つのメッセージしか処理できないわけではありません。</span><span class="sxs-lookup"><span data-stu-id="af64b-112">Although we only have one document in our array, it doesn't mean that our solution can only handle one message at a time.</span></span> <span data-ttu-id="af64b-113">Azure Functions ランタイムは、関数のいくつかのインスタンスを "*並列*" で呼び出して、メッセージを一括して取得および処理します。</span><span class="sxs-lookup"><span data-stu-id="af64b-113">The Azure Functions runtime retrieves and processes messages in batches, calling several instances of our function *in parallel*.</span></span> <span data-ttu-id="af64b-114">現在、既定のバッチ サイズは 16 で、最大バッチ サイズは 32 です。</span><span class="sxs-lookup"><span data-stu-id="af64b-114">Currently, the default batch size is 16 and the maximum batch size is 32.</span></span>
- <span data-ttu-id="af64b-115">`id` は、配列内で一意である必要があります。</span><span class="sxs-lookup"><span data-stu-id="af64b-115">The `id` must be unique within the array.</span></span> <span data-ttu-id="af64b-116">`language` プロパティは、ドキュメント テキストの言語を指定するものです。</span><span class="sxs-lookup"><span data-stu-id="af64b-116">The `language` property specifies the language of the document text.</span></span>  
- <span data-ttu-id="af64b-117">次に、`get_sentiments` メソッドを呼び出します。このメソッドは、HTTPS モジュールを使用して、Text Analytics API を呼び出します。</span><span class="sxs-lookup"><span data-stu-id="af64b-117">We then call our method `get_sentiments`, which uses the HTTPS module to make the call to Text Analytics API.</span></span> <span data-ttu-id="af64b-118">各要求のヘッダーで、サブスクリプションまたはアクセス キーを渡している点に注意してください。</span><span class="sxs-lookup"><span data-stu-id="af64b-118">Notice that we pass our subscription, or access, key in the header of every request.</span></span>
- <span data-ttu-id="af64b-119">サービスが終了するときに、`response_handler` が呼び出され、コンソールへの応答が `context.log` を使用してログに記録されます</span><span class="sxs-lookup"><span data-stu-id="af64b-119">When the service returns, our `response_handler` is called and we log the response to the console using `context.log`</span></span> 

## <a name="try-it-out"></a><span data-ttu-id="af64b-120">試してみる</span><span class="sxs-lookup"><span data-stu-id="af64b-120">Try it out</span></span>

<span data-ttu-id="af64b-121">キューへの分類を見る前に、テスト実行用にどのようなものがあるかを確認しておきましょう。</span><span class="sxs-lookup"><span data-stu-id="af64b-121">Before we look at sorting into queues, let's take what we have for a test run.</span></span> 

1.  <span data-ttu-id="af64b-122">Function App ポータルで関数 [!INCLUDE [func-name-discover](./func-name-discover.md)] を選択した状態で、左端にある [テスト] メニュー項目をクリックして展開します。</span><span class="sxs-lookup"><span data-stu-id="af64b-122">With our function, [!INCLUDE [func-name-discover](./func-name-discover.md)], selected in the Function Apps portal, click on the Test menu item on the far left to expand it.</span></span>

2. <span data-ttu-id="af64b-123">**[テスト]** メニュー項目を選択し、テスト パネルが開いていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="af64b-123">Select the **Test** menu item and verify that you have the test panel open.</span></span> <span data-ttu-id="af64b-124">次のスクリーンショットは、どのような表示になるかを示しています。</span><span class="sxs-lookup"><span data-stu-id="af64b-124">The following screenshot shows what it should look like.</span></span> 

![展開された関数のテスト パネルを示すスクリーン ショット。](../media-draft/test-panel-open-small.png)

3. <span data-ttu-id="af64b-126">スクリーンショットに示すように、要求本文にテキストの文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="af64b-126">Add a string of text into the request body as shown in the screenshot.</span></span> 

1.  <span data-ttu-id="af64b-127">テスト パネルの下部にある **[実行]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="af64b-127">Click **Run** at the bottom of the test panel.</span></span>

1. <span data-ttu-id="af64b-128">メイン画面の左下のコード エディターの下で、**[ログ]** タブが展開されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="af64b-128">Make sure the **Logs** tab is expanded at the bottom left of the main screen, under the code editor.</span></span> 

1. <span data-ttu-id="af64b-129">**[ログ]** タブで、関数が完了したというログ情報が表示されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="af64b-129">Verify that the **Logs** tab displays log information that the function completed.</span></span> <span data-ttu-id="af64b-130">ウィンドウには、Text Analytics API 呼び出しからの応答も表示されます。</span><span class="sxs-lookup"><span data-stu-id="af64b-130">The window will also display the response from the Text Analytics API call.</span></span> 

![テスト パネルと成功したテストの結果を示すスクリーンショット。](../media-draft/sentiment-response-log1.png)

<span data-ttu-id="af64b-132">うまくいきました。</span><span class="sxs-lookup"><span data-stu-id="af64b-132">Congratulations!</span></span> <span data-ttu-id="af64b-133">[!INCLUDE [func-name-discover](./func-name-discover.md)] は、設計どおりに動作します。</span><span class="sxs-lookup"><span data-stu-id="af64b-133">The [!INCLUDE [func-name-discover](./func-name-discover.md)] works as designed.</span></span> <span data-ttu-id="af64b-134">この例では、非常に明るいメッセージを渡し、0.98 を超えるスコアを受け取りました。</span><span class="sxs-lookup"><span data-stu-id="af64b-134">In  this example, we passed in a very upbeat message and received a score of over 0.98.</span></span> <span data-ttu-id="af64b-135">それよりも明るくないメッセージに変更し、テストを再実行して、応答に注意してみてください。</span><span class="sxs-lookup"><span data-stu-id="af64b-135">Try changing the message to something less optimistic, rerun the test and note the response.</span></span>

## <a name="try-it-out-again-optional"></a><span data-ttu-id="af64b-136">もう一度試してみる (省略可能)</span><span class="sxs-lookup"><span data-stu-id="af64b-136">Try it out again (optional)</span></span>

<span data-ttu-id="af64b-137">テストを繰り返してみましょう。</span><span class="sxs-lookup"><span data-stu-id="af64b-137">Let's repeat the test.</span></span> <span data-ttu-id="af64b-138">今回は、ポータルのテスト ウィンドウを使用する代わりに、実際に入力キューにメッセージを入れて、何が起きるかを監視します。</span><span class="sxs-lookup"><span data-stu-id="af64b-138">This time, instead of using the Test window of the portal, we'll actually place a message into the input queue and watch what happens.</span></span> 

1. <span data-ttu-id="af64b-139">ポータルの **[リソース グループ]** セクションで、該当するリソース グループに移動します。</span><span class="sxs-lookup"><span data-stu-id="af64b-139">Navigate to your resource group in the **Resource Groups** section of the portal.</span></span>

1. <span data-ttu-id="af64b-140">このレッスンで使用したリソース グループを選択します。</span><span class="sxs-lookup"><span data-stu-id="af64b-140">Select the resource group used in this lesson.</span></span>

1. <span data-ttu-id="af64b-141">表示された **[リソース グループ]** パネルで、[ストレージ アカウント] エントリを探して選択します。</span><span class="sxs-lookup"><span data-stu-id="af64b-141">In the **Resource group** panel that appears, locate the Storage Account entry and select it.</span></span>

![スクリーンショット ([リソース グループ] ウィンドウでストレージ アカウントを選択したところ)。](../media-draft/select-storage-account.png)

1. <span data-ttu-id="af64b-143">[ストレージ アカウント] メイン ウィンドウの左側のメニューから **[ストレージ エクスプローラー (プレビュー)]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="af64b-143">Select **Storage Explorer (preview)** from the left menu of the Storage Account main window.</span></span>  <span data-ttu-id="af64b-144">この操作により、ポータル内で Azure Storage Explorer が開きます。</span><span class="sxs-lookup"><span data-stu-id="af64b-144">This action opens the Azure Storage Explorer inside the portal.</span></span> <span data-ttu-id="af64b-145">この段階では、画面は次のスクリーンショットのようになります。</span><span class="sxs-lookup"><span data-stu-id="af64b-145">Your screen should look like the following screenshot at this stage.</span></span> 

![ストレージ アカウントが示されているストレージ エクスプローラーのスクリーンショット (現時点でキューはなし)。](../media-draft/sa-no-queue.png)

<span data-ttu-id="af64b-147">ご覧のとおり、このストレージ アカウントにはまだキューがないので、修正しましょう。</span><span class="sxs-lookup"><span data-stu-id="af64b-147">As you can see, we don't have any queues in this storage account yet, so let's fix that.</span></span>

1. <span data-ttu-id="af64b-148">このレッスンの前の方で、トリガーに関連付けられているキューに **new-feedback-q** という名前を付けました。</span><span class="sxs-lookup"><span data-stu-id="af64b-148">If you remember from earlier in this lesson, we named the queue associated with our trigger **new-feedback-q**.</span></span> <span data-ttu-id="af64b-149">ストレージ エクスプローラーで **[キュー]** 項目を右クリックし、*[キューの作成]* を選択します。</span><span class="sxs-lookup"><span data-stu-id="af64b-149">Right-click on the **Queues** item in the storage explorer and select *Create Queue*.</span></span>

1. <span data-ttu-id="af64b-150">開かれたダイアログ ボックスで「**new-feedback-q**」と入力し、**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="af64b-150">In the dialog that opens, enter **new-feedback-q** and click **OK**.</span></span> <span data-ttu-id="af64b-151">これで、入力キューができました。</span><span class="sxs-lookup"><span data-stu-id="af64b-151">We now have our input queue.</span></span> 

1. <span data-ttu-id="af64b-152">左側のメニューで新しいキューを選択して、このキューのデータ エクスプローラーを表示します。</span><span class="sxs-lookup"><span data-stu-id="af64b-152">Select the new queue in the left-hand menu to see the data explorer for this queue.</span></span> <span data-ttu-id="af64b-153">当然、キューは空です。</span><span class="sxs-lookup"><span data-stu-id="af64b-153">As expected, the queue is empty.</span></span> <span data-ttu-id="af64b-154">ウィンドウ上部にある **[メッセージの追加]** コマンドを使って、キューにメッセージを追加してみましょう。</span><span class="sxs-lookup"><span data-stu-id="af64b-154">Let's add a message to the queue using the **Add Message** command at the top of the window.</span></span>

1. <span data-ttu-id="af64b-155">**[メッセージの追加]** ダイアログ ボックスの **[メッセージ テキスト]** フィールドに「This message came from our input queue, new-feedback-q」 (このメッセージは入力キューの new-feedback-q からのものです) と入力し、ダイアログの下部の **[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="af64b-155">In the **Add Message** dialog, enter "This message came from our input queue, new-feedback-q" into the **Message text** field and click **OK** at the bottom of the dialog.</span></span> 

1. <span data-ttu-id="af64b-156">データ エクスプローラーで、次のスクリーンショットのようなメッセージを確認します。</span><span class="sxs-lookup"><span data-stu-id="af64b-156">Observe the message, similar to the message in the following screenshot, in the data explorer.</span></span>
<span data-ttu-id="af64b-157">![ストレージ アカウントが示されているストレージ エクスプローラーのスクリーンショット (作成したメッセージがキューにある状態)。](../media-draft/message-in-input-queue.png)</span><span class="sxs-lookup"><span data-stu-id="af64b-157">![Screenshot of storage explorer showing our storage account, with the message we created in the queue.](../media-draft/message-in-input-queue.png)</span></span>

1. <span data-ttu-id="af64b-158">数秒後、**[最新の情報に更新]** をクリックして、キューのビューを最新の情報に更新します。</span><span class="sxs-lookup"><span data-stu-id="af64b-158">After a few seconds, click **Refresh** to refresh the view of the queue.</span></span> <span data-ttu-id="af64b-159">キューが空であるかどうかをもう一度確認します。</span><span class="sxs-lookup"><span data-stu-id="af64b-159">Observe that the queue is empty once again.</span></span> <span data-ttu-id="af64b-160">何かがキューからメッセージを読み取ったはずです。</span><span class="sxs-lookup"><span data-stu-id="af64b-160">Something must have read the message from the queue.</span></span> 

1. <span data-ttu-id="af64b-161">ポータルで関数に戻り、**[モニター]** タブを開きます。一覧で最新のメッセージを選択します。</span><span class="sxs-lookup"><span data-stu-id="af64b-161">Navigate back to our function in the portal and open the **Monitor** tab. Select the newest message and in the list.</span></span> <span data-ttu-id="af64b-162">new-feedback-q に投稿したキュー メッセージが関数によって処理されたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="af64b-162">Observe that our function processed the queue message we had posted to the new-feedback-q.</span></span>

![new-feedback-q に投稿したキュー メッセージが関数によって処理されたことを示すエントリが表示されている [モニター] ダッシュボードのスクリーンショット。](../media-draft/message-in-monitor.png)

<span data-ttu-id="af64b-164">このテストでは、キューに何かを追加した後で関数によってそれが処理されるのを確認する、完全なラウンドトリップを行いました。</span><span class="sxs-lookup"><span data-stu-id="af64b-164">In this test, we did a complete round trip of posting something into our queue and then seeing the function process it.</span></span>

<span data-ttu-id="af64b-165">ソリューションは着実に進歩しています。</span><span class="sxs-lookup"><span data-stu-id="af64b-165">We're making progress with our solution!</span></span> <span data-ttu-id="af64b-166">関数が有益なことを実行できるようになりました。</span><span class="sxs-lookup"><span data-stu-id="af64b-166">Our function is now doing something useful.</span></span> <span data-ttu-id="af64b-167">入力キューからテキストを受け取り、Text Analytics API サービスを呼び出してセンチメント スコアを取得しています。</span><span class="sxs-lookup"><span data-stu-id="af64b-167">It's receiving text from our input queue and then calling out to the Text Analytics API service to get a sentiment score.</span></span>  <span data-ttu-id="af64b-168">また、Azure portal と Storage Explorer を通じて関数をテストする方法も学習しました。</span><span class="sxs-lookup"><span data-stu-id="af64b-168">We've also learned how to test our function through the Azure portal and the Storage Explorer.</span></span> <span data-ttu-id="af64b-169">次の演習では、出力バインディングを使用したキューへの書き込みがいかに簡単であるかを見ていきます。</span><span class="sxs-lookup"><span data-stu-id="af64b-169">In the next exercise, we'll see how easy it is to write to queues using output bindings.</span></span>