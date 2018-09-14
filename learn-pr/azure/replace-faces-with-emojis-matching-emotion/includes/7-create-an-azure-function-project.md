<span data-ttu-id="c3913-101">最終的には、クラウドのどこかにコードをホストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c3913-101">Eventually, we need to host our code somewhere in the cloud.</span></span> <span data-ttu-id="c3913-102">Azure のテクノロジを使用して、これを行うには、Azure Functions です。</span><span class="sxs-lookup"><span data-stu-id="c3913-102">The Azure technology we are going to use to do this is Azure Functions.</span></span>

<span data-ttu-id="c3913-103">この講義では、次の説明は。</span><span class="sxs-lookup"><span data-stu-id="c3913-103">In this lecture you will learn:</span></span>

- <span data-ttu-id="c3913-104">Azure Function App を作成する方法と`JavaScript`Http トリガーします。</span><span class="sxs-lookup"><span data-stu-id="c3913-104">How to create an Azure Function App and `JavaScript` Http Trigger.</span></span>
- <span data-ttu-id="c3913-105">実行し、Azure 関数をローカルでデバッグする方法。</span><span class="sxs-lookup"><span data-stu-id="c3913-105">How to run and debug an Azure Function locally.</span></span>

## <a name="create-an-azure-function-project"></a><span data-ttu-id="c3913-106">Azure 関数プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="c3913-106">Create an Azure Function Project</span></span>

<span data-ttu-id="c3913-107">Azure 関数プロジェクトは、複数の関数のコンテナーです。</span><span class="sxs-lookup"><span data-stu-id="c3913-107">An Azure Function Project is a container for multiple Functions.</span></span> <span data-ttu-id="c3913-108">さまざまな方法でトリガーされる関数私たちがするごとにトリガー関数の HTTP 要求を行います。</span><span class="sxs-lookup"><span data-stu-id="c3913-108">Functions are triggered in different ways; we'll be triggering our functions by making a HTTP request.</span></span>

<span data-ttu-id="c3913-109">Azure Functions; を作成する方法はたくさんあります。このモジュール内で事前にインストールされている Azure の関数の拡張機能で Visual Studio Code を使用するでしょう。</span><span class="sxs-lookup"><span data-stu-id="c3913-109">There are many ways to create Azure Functions; we are going to use Visual Studio Code with the Azure Functions Extension which we installed earlier on in this module.</span></span>

<span data-ttu-id="c3913-110">まず、関数プロジェクトを作成してみましょう。</span><span class="sxs-lookup"><span data-stu-id="c3913-110">Let's first create our Function Project.</span></span>

1. <span data-ttu-id="c3913-111">型<kbd>Ctrl</kbd>+<kbd>P</kbd>コマンド パレットを起動し、検索して選択するには `Azure Functions: Create New Project...`</span><span class="sxs-lookup"><span data-stu-id="c3913-111">Type <kbd>Ctrl</kbd>+<kbd>P</kbd> to open up the command palette, then search for and select `Azure Functions: Create New Project...`</span></span>

   > <span data-ttu-id="c3913-112">**注**</span><span class="sxs-lookup"><span data-stu-id="c3913-112">**NOTE**</span></span>
   >
   > <span data-ttu-id="c3913-113">など一部のファイルを上書きするかどうかがありますが問わ`.gitignore`、回答**ありません**すべてに!</span><span class="sxs-lookup"><span data-stu-id="c3913-113">It might ask if you want to overwrite some files such as `.gitignore`, answer **no** to all!</span></span>

   ![新しいプロジェクトの作成](/media-drafts/7.create-new-project.png)

2. <span data-ttu-id="c3913-115">(最初のオプションは、現在のフォルダー)、関数アプリを作成するフォルダーを選択します。</span><span class="sxs-lookup"><span data-stu-id="c3913-115">Select the folder where you want to create the function app (the first option is the current folder)</span></span>

   ![フォルダーの選択](/media-drafts/7.select-folder.png)

3. <span data-ttu-id="c3913-117">選択`JavaScript`目的の言語として。</span><span class="sxs-lookup"><span data-stu-id="c3913-117">Choose `JavaScript` as the desired language.</span></span>

   ![言語の選択](/media-drafts/7.select-language.png)

4. <span data-ttu-id="c3913-119">正常に完了、上記の場合は表示のフォルダーに作成されたファイルの下</span><span class="sxs-lookup"><span data-stu-id="c3913-119">If the above completed successfully you should see the below files created in the folder</span></span>

   ![アプリの作成](/media-drafts/7.app-created.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="c3913-121">Azure 関数の作成</span><span class="sxs-lookup"><span data-stu-id="c3913-121">Create an Azure Function</span></span>

<span data-ttu-id="c3913-122">それでは次に HTTP 要求に応答するコードの一部、Azure 関数自体を作成します。</span><span class="sxs-lookup"><span data-stu-id="c3913-122">Next up let's create the Azure Function itself, the piece of code that responds to a HTTP request.</span></span>

<span data-ttu-id="c3913-123">もう一度 Visual Studio コード拡張機能を使用して行います。</span><span class="sxs-lookup"><span data-stu-id="c3913-123">Again we are going to use the Visual Studio Code Extention.</span></span>

1.  <span data-ttu-id="c3913-124">型<kbd>Ctrl</kbd>+<kbd>P</kbd>コマンド パレットを起動し、検索して選択するには `Azure Functions: Create Function...`</span><span class="sxs-lookup"><span data-stu-id="c3913-124">Type <kbd>Ctrl</kbd>+<kbd>P</kbd> to open up the command palette, then search for and select `Azure Functions: Create Function...`</span></span>

    ![新しい関数を作成します。](/media-drafts/7.create-function.png)

2.  <span data-ttu-id="c3913-126">以前で関数プロジェクトを作成したフォルダーを選択します (最初のオプションは、現在のフォルダー)</span><span class="sxs-lookup"><span data-stu-id="c3913-126">Select the folder where you created the function project previously (the first option is the current folder)</span></span>

    ![フォルダーの選択](/media-drafts/7.select-current-project.png)

3.  <span data-ttu-id="c3913-128">ここでは作成、 _HTTP トリガー_、ようにするオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="c3913-128">We are creating a _HTTP Trigger_, so select that option.</span></span>

    ![フォルダーの選択](/media-drafts/7.select-trigger.png)

4.  <span data-ttu-id="c3913-130">入力`MojifyImage`として作成する関数の名前</span><span class="sxs-lookup"><span data-stu-id="c3913-130">Type in `MojifyImage` as the name of the Function you want to create</span></span>

    ![名前を選択します。](/media-drafts/7.choose-function-name.png)

5.  <span data-ttu-id="c3913-132">選択`Anonymous`認証レベルとして</span><span class="sxs-lookup"><span data-stu-id="c3913-132">Choose `Anonymous` as the authentication level</span></span>

    > <span data-ttu-id="c3913-133">注: 選択して`Anonymous`関数が、世界中に開かれ、安全でないです。</span><span class="sxs-lookup"><span data-stu-id="c3913-133">NOTE: By choosing `Anonymous` the function is open to the world and insecure.</span></span>

    ![名前を選択します。](/media-drafts/7.choose-auth-level.png)

## <a name="run-the-function-locally"></a><span data-ttu-id="c3913-135">関数をローカルで実行する</span><span class="sxs-lookup"><span data-stu-id="c3913-135">Run the function locally</span></span>

<span data-ttu-id="c3913-136">関数のプロジェクトにスタート プロジェクトを変換するこれらのコマンドが正常に完了すると、 _HTTP トリガー_関数と呼ばれる`MojifyImage`します。</span><span class="sxs-lookup"><span data-stu-id="c3913-136">Once these commands complete successfully you have converted the starter project to a function project with a _HTTP Trigger_ function called `MojifyImage`.</span></span>

<span data-ttu-id="c3913-137">すべてが動作することを確認する関数アプリを実行できるよう、ローカルで。</span><span class="sxs-lookup"><span data-stu-id="c3913-137">To ensure everything is working you can run the function app locally, like so:</span></span>

```bash
func host start
```

<span data-ttu-id="c3913-138">関数のランタイムをローカルで起動します。場合、そのしくみを最終的に正しく表示されるはずのように、次のコンソールに出力します。</span><span class="sxs-lookup"><span data-stu-id="c3913-138">This starts the function's runtime locally; if it all works correctly eventually you should see something like the below printed to the console:</span></span>

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

> <span data-ttu-id="c3913-139">**注**</span><span class="sxs-lookup"><span data-stu-id="c3913-139">**Note**</span></span>
>
> <span data-ttu-id="c3913-140">これは、Azure Functions ランタイムをインストールする利点の 1 つ、実稼働環境での関数の実行に使用される同じ基になるテクノロジを使用してローカルで関数を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="c3913-140">This is one of the advantages of installing the Azure Functions runtime, it allows us to run the function locally using the same underlying technology that would be used to run the function in production.</span></span>

<span data-ttu-id="c3913-141">関数が正常に動作させるには、出力はブラウザーのウィンドウで、コンソールに出力 URL にアクセスします。</span><span class="sxs-lookup"><span data-stu-id="c3913-141">To ensure the function is working correctly, visit the URL printed to the console, this prints in the browser window:</span></span>

![Function App が機能しています。](/media-drafts/7.default-function-app-working.png)

## <a name="debug-the-function-locally"></a><span data-ttu-id="c3913-143">ローカル関数をデバッグします。</span><span class="sxs-lookup"><span data-stu-id="c3913-143">Debug the function locally</span></span>

> <span data-ttu-id="c3913-144">**重要**</span><span class="sxs-lookup"><span data-stu-id="c3913-144">**Important**</span></span>
>
> <span data-ttu-id="c3913-145">終了するかどうかを確認、 `func host start` Visual Studio Code を使用してデバッグする前に実行したコマンドします。</span><span class="sxs-lookup"><span data-stu-id="c3913-145">Make sure you exit the `func host start` command you just ran before trying to debug it using Visual Studio Code.</span></span>

<span data-ttu-id="c3913-146">さらに、実行できる_およびデバッグ_visual studio のコード内でアプリケーション。</span><span class="sxs-lookup"><span data-stu-id="c3913-146">Additionally, we can run _and debug_ the application inside visual studio code.</span></span>

<span data-ttu-id="c3913-147">ブレークポイントを追加、`index.js`ファイル、JavaScript 関数の上部にあるようになります。</span><span class="sxs-lookup"><span data-stu-id="c3913-147">Add a breakpoint to the `index.js` file, at the top of the JavaScript function, like so:</span></span>

![ブレークポイントを追加します。](/media-drafts/7.add-breakpoint.png)

<span data-ttu-id="c3913-149">デバッグ モードの初回アクセス時に、[デバッグ] メニューから関数を実行する<kbd>Cmd<kbd> + <kbd>Shift<kbd> + <kbd>D<kbd>します。</span><span class="sxs-lookup"><span data-stu-id="c3913-149">To run the function in debug mode first visit the debug menu, <kbd>Cmd<kbd> + <kbd>Shift<kbd> + <kbd>D<kbd>.</span></span>

<span data-ttu-id="c3913-150">並べ替えた`Attach to JavaScript Functions`最後に、デバッグ セッションを開始する緑の三角形を押す前に、デバッグ構成から。</span><span class="sxs-lookup"><span data-stu-id="c3913-150">Then pick `Attach to JavaScript Functions` from the debug configuration before finally pressing the green triangle to start the debug session.</span></span>

> <span data-ttu-id="c3913-151">**注**</span><span class="sxs-lookup"><span data-stu-id="c3913-151">**Note**</span></span>
>
> <span data-ttu-id="c3913-152">`Attach to JavaScript Functions`関数プロジェクトを作成したときに、デバッグ構成が自動的に追加されました。</span><span class="sxs-lookup"><span data-stu-id="c3913-152">The `Attach to JavaScript Functions` debug configuration was added automatically when you created the Function Project.</span></span>

<span data-ttu-id="c3913-153">または、キー <kbd>F5<kbd>から任意の場所は、アプリケーションでこれを実行、前回のデバッグ構成します。</span><span class="sxs-lookup"><span data-stu-id="c3913-153">Alternatively, you can press <kbd>F5<kbd> from anywhere in the application; this runs the last debug configuration.</span></span>

<span data-ttu-id="c3913-154">`func host start`コマンドを実行するは、ターミナルする必要がありますを開くことで最終的に同じ上記と同じ出力します。</span><span class="sxs-lookup"><span data-stu-id="c3913-154">The `func host start` command is run for you; a terminal should open with eventually the same output as above:</span></span>

```output
Http Functions:

        MojifyImage: http://localhost:7071/api/MojifyImage
```

<span data-ttu-id="c3913-155">デバッグのメニュー バーに表示されますを参照もデバッグしているために次のようにします。</span><span class="sxs-lookup"><span data-stu-id="c3913-155">Since we are debugging you also see the debug menu bar appear, like so:</span></span>

![メニュー バーのデバッグ](/media-drafts/7.debug-menu-bar.png)

<span data-ttu-id="c3913-157">これで、ブレークポイントで中断が上記の URL にアクセスすると、指定したし、関数は、手順を実行できます。</span><span class="sxs-lookup"><span data-stu-id="c3913-157">Now when you visit the URL above it breaks at the breakpoint you specified, and you can step through the function.</span></span>

<!-- TODO Find Link -->

<span data-ttu-id="c3913-158">できます[詳細](https://code.visualstudio.com/docs/editor/debugging)Visual Studio Code でのデバッグについて</span><span class="sxs-lookup"><span data-stu-id="c3913-158">You can [learn more](https://code.visualstudio.com/docs/editor/debugging) about debugging in Visual Studio Code</span></span>
