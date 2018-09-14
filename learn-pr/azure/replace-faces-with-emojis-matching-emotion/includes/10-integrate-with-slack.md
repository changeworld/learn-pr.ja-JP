<span data-ttu-id="01c37-101">すべての必要な Azure functions を作成しましたし、それらをローカルで実行できます。</span><span class="sxs-lookup"><span data-stu-id="01c37-101">We have created all the required Azure functions, and we can run them locally.</span></span>

<span data-ttu-id="01c37-102">この講義で Slack で、Azure Functions を接続し、Slack 作成_スラッシュ_コマンドを Azure 関数の呼び出しを slack のウィンドウで、結果として得られる mojified イメージを表示します。</span><span class="sxs-lookup"><span data-stu-id="01c37-102">In this lecture, we connect our Azure Functions with Slack and create a Slack _slash_ command which calls our Azure Function and displays the resulting mojified image in the slack window.</span></span>

<span data-ttu-id="01c37-103">この講義の終わりまでには、次の説明は。</span><span class="sxs-lookup"><span data-stu-id="01c37-103">By the end of this lecture you will learn:</span></span>

- <span data-ttu-id="01c37-104">Slack アプリを作成し、スラッシュのコマンドを Azure 関数のエンドポイントに接続する方法。</span><span class="sxs-lookup"><span data-stu-id="01c37-104">How to create a Slack app and connect a slash command with an Azure Function endpoint.</span></span>
- <span data-ttu-id="01c37-105">をクラウドに展開することがなく、スラッシュのコマンドをローカルをテストする方法</span><span class="sxs-lookup"><span data-stu-id="01c37-105">How to test your slash command locally, without deploying to the cloud.</span></span>

## <a name="using-ngrok-to-develop-with-slash-commands"></a><span data-ttu-id="01c37-106">ngrok を使用してスラッシュ コマンドで開発する</span><span class="sxs-lookup"><span data-stu-id="01c37-106">Using ngrok to develop with slash commands</span></span>

<span data-ttu-id="01c37-107">Azure 関数が現在実行中のローカル; だけただし、slack は、インターネットでは、クラウドで実行されます。</span><span class="sxs-lookup"><span data-stu-id="01c37-107">Our Azure functions are currently only running locally; however, slack runs in the cloud, on the internet.</span></span>

<span data-ttu-id="01c37-108">設定すると、slack を_スラッシュ_スラッシュ コマンドをユーザーが実行するたびに呼び出しをパブリック URL を要求するこれは、次のセクションでコマンドします。</span><span class="sxs-lookup"><span data-stu-id="01c37-108">When we set up a slack _slash_ command in the next section, it's going to ask for a public URL which it calls whenever someone executes a slash command.</span></span>

<span data-ttu-id="01c37-109">付与する、`RespondToSlackCommand`ローカル コンピューターですが、URL がのみ存在します。</span><span class="sxs-lookup"><span data-stu-id="01c37-109">We want to give them our `RespondToSlackCommand` URL, but that only exists on our local computer.</span></span>

<span data-ttu-id="01c37-110">クラウド内のサービスが開発用ローカル コンピューター上のリソースにアクセスできるようにするには、どうすればよいでしょうか。</span><span class="sxs-lookup"><span data-stu-id="01c37-110">How can you give services in the cloud access to resources on your local computer for development?</span></span>

<span data-ttu-id="01c37-111">この難問に対する優れたソリューションが`ngrok`これは、インターネットからローカル コンピューターにセキュリティで保護されたトンネルを作成するツールです。</span><span class="sxs-lookup"><span data-stu-id="01c37-111">A great solution to this conundrum is `ngrok` this is a tool which creates secure tunnels to your local machine from the internet.</span></span>

1. <span data-ttu-id="01c37-112">https://ngrok.com/download を参照してください</span><span class="sxs-lookup"><span data-stu-id="01c37-112">Visit https://ngrok.com/download</span></span>
2. <span data-ttu-id="01c37-113">指示に従って `ngrok` パッケージをローカル コンピューターにダウンロードしてインストールします。</span><span class="sxs-lookup"><span data-stu-id="01c37-113">Follow the instructions to download and install the `ngrok` package to your local machine.</span></span>
3. <span data-ttu-id="01c37-114">ターミナルで `ngrok http 7071` を実行します。</span><span class="sxs-lookup"><span data-stu-id="01c37-114">Run `ngrok http 7071` in a terminal.</span></span>
   <span data-ttu-id="01c37-115">![ngrok](/media-drafts/9.ngrok.png)</span><span class="sxs-lookup"><span data-stu-id="01c37-115">![ngrok](/media-drafts/9.ngrok.png)</span></span>
4. <span data-ttu-id="01c37-116">これで終わるパブリック URL を出力`ngrok.io`など`http://bbade778.ngrok.io`、メモ、これを次のセクションで必要になります。</span><span class="sxs-lookup"><span data-stu-id="01c37-116">This prints out a public URL ending in `ngrok.io` e.g. `http://bbade778.ngrok.io`, make a note of this you need it in the next section.</span></span>

> <span data-ttu-id="01c37-117">**注**</span><span class="sxs-lookup"><span data-stu-id="01c37-117">**Note**</span></span>
>
> <span data-ttu-id="01c37-118">これは、 `http://*.ngrock.io` URL を使用したのと同様に使用することができます`http://localhost:7071`イメージを試すようになります `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`</span><span class="sxs-lookup"><span data-stu-id="01c37-118">This `http://*.ngrock.io` URL you can use just as you used `http://localhost:7071`, try it out with an image like so `http://[ngrok-url]/api/MojifyImage?imageUrl=[url-to-an-image]`</span></span>

## <a name="create-a-slack-app"></a><span data-ttu-id="01c37-119">Slack アプリを作成する</span><span class="sxs-lookup"><span data-stu-id="01c37-119">Create a slack app</span></span>

<span data-ttu-id="01c37-120">スラッシュ コマンドは、Slack アプリである Slack "_ボット_" の一部として存在します。</span><span class="sxs-lookup"><span data-stu-id="01c37-120">A slash command exists as part of a slack app, a slack _bot_.</span></span>

<span data-ttu-id="01c37-121">「[Create Slack App](https://api.slack.com/apps/new)」(Slack アプリの作成) にアクセスします</span><span class="sxs-lookup"><span data-stu-id="01c37-121">Visit [Create Slack App](https://api.slack.com/apps/new)</span></span>

<span data-ttu-id="01c37-122">選択、`App Name`と関連付けること、`Workspace`この演習を押してからの開始時に作成した`Create App`、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="01c37-122">Choose an `App Name` and associate it with the `Workspace` you created at the start of this exercise and then press `Create App`, like so:</span></span>

![Slack アプリを作成する](/media-drafts/9.create-slack-app.png)

<span data-ttu-id="01c37-124">作成されたら、Slack アプリに移動し、スラッシュ コマンドを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="01c37-124">Once that's created we then need to go into our slack app and create a slash command.</span></span>

## <a name="create-a-slash-command"></a><span data-ttu-id="01c37-125">スラッシュ コマンドを作成する</span><span class="sxs-lookup"><span data-stu-id="01c37-125">Create a slash command</span></span>

<span data-ttu-id="01c37-126">をクリックして、`Slash Commands`メニュー項目。</span><span class="sxs-lookup"><span data-stu-id="01c37-126">Click on the `Slash Commands` menu item.</span></span>

![スラッシュ コマンド](/media-drafts/9.slash-commands.png)

<span data-ttu-id="01c37-128">[`Create New Command`] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="01c37-128">Click `Create New Command`</span></span>

- <span data-ttu-id="01c37-129">[`command`]\(コマンド\) フィールドに任意のスラッシュ コマンド名を入力します。</span><span class="sxs-lookup"><span data-stu-id="01c37-129">Enter whatever name you want for your slash command in the `command` field.</span></span>
- <span data-ttu-id="01c37-130">Ngrok パブリック URL を入力、`Request URL`フィールド</span><span class="sxs-lookup"><span data-stu-id="01c37-130">Enter the ngrok public URL into the `Request URL` field</span></span>

  > <span data-ttu-id="01c37-131">**大事な**</span><span class="sxs-lookup"><span data-stu-id="01c37-131">**IMPORTANT**</span></span>
  >
  > <span data-ttu-id="01c37-132">追加する`/api/RespondToSlackCommand`url</span><span class="sxs-lookup"><span data-stu-id="01c37-132">Remember to append `/api/RespondToSlackCommand` to the URL</span></span>

- <span data-ttu-id="01c37-133">簡単な説明と使用のヒントを追加します。</span><span class="sxs-lookup"><span data-stu-id="01c37-133">Add a short description and usage hint.</span></span>
- <span data-ttu-id="01c37-134">[`Save`]\(保存\) をクリックします</span><span class="sxs-lookup"><span data-stu-id="01c37-134">Press `Save`</span></span>

![スラッシュ コマンド](/media-drafts/9.create-slash-command.png)

<span data-ttu-id="01c37-136">保存すると次のような画面が表示されます。</span><span class="sxs-lookup"><span data-stu-id="01c37-136">Once saved you should see a screen like so:</span></span>

![スラッシュ コマンド成功](/media-drafts/9.create-slash-commands-success.png)

## <a name="install-the-slack-app-to-the-workspace"></a><span data-ttu-id="01c37-138">ワークスペースへの slack アプリをインストールします。</span><span class="sxs-lookup"><span data-stu-id="01c37-138">Install the slack app to the workspace</span></span>

<span data-ttu-id="01c37-139">Slack アプリ ワークスペースで使用できる、前にインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="01c37-139">Before we can use our slack app in our workspace, we need to install it.</span></span>

1. <span data-ttu-id="01c37-140">[`Basic Information`]\(基本情報\) メニュー項目を選択します</span><span class="sxs-lookup"><span data-stu-id="01c37-140">Select the `Basic Information` menu item</span></span>

2. <span data-ttu-id="01c37-141">[`Install your app to your workspace`]\(ワークスペースにアプリをインストールする\) オプションを展開し、[`Install app to workspace`]\(アプリをワークスペースにインストールする\) ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="01c37-141">Expand out the `Install your app to your workspace` option and press the `Install app to workspace` button.</span></span>

   ![アプリをワークスペースにインストールする](/media-drafts/9.install-app-to-workspace.png)

3. <span data-ttu-id="01c37-143">次のようにアプリケーションの認証を求められる場合があります。</span><span class="sxs-lookup"><span data-stu-id="01c37-143">It may ask you to authenticate your application like so:</span></span>

   ![ワークスペースに対してアプリを認証する](/media-drafts/9.authenticate-slack-app.png)

## <a name="try-it-out"></a><span data-ttu-id="01c37-145">試してみる</span><span class="sxs-lookup"><span data-stu-id="01c37-145">Try it out</span></span>

<span data-ttu-id="01c37-146">これで、スラッシュ コマンドを作成してみましょう ngrok リンク経由で Azure Functions のローカルで実行中インスタンスに接続をテストします。</span><span class="sxs-lookup"><span data-stu-id="01c37-146">Now that your slash command has been created and connected to our locally running instance of Azure Functions via the ngrok link let's test it.</span></span>

1. <span data-ttu-id="01c37-147">Slack アプリに関連付けた Slack ワークスペースに移動します。</span><span class="sxs-lookup"><span data-stu-id="01c37-147">Go to the slack workspace you associated with your slack app.</span></span>
2. <span data-ttu-id="01c37-148">任意のチャット ウィンドウに移動して「`/mojify`」 (または、アプリに指定した名前) と入力します</span><span class="sxs-lookup"><span data-stu-id="01c37-148">Go to any chat window and type `/mojify` (or whatever name you gave the app)</span></span>
3. <span data-ttu-id="01c37-149">すべてが正しく機能、表示されるはず`mojify`オプションとして</span><span class="sxs-lookup"><span data-stu-id="01c37-149">If everything has worked correctly, you should see `mojify` as an option</span></span>

   ![Mojify を確認する](/media-drafts/9.slack-check-mojify.png)

4. <span data-ttu-id="01c37-151">ここで「`/mojify`し画像 URL を追加し、、enter キーを押して次ように。</span><span class="sxs-lookup"><span data-stu-id="01c37-151">Now type `/mojify` and add an image URL then press enter, like so:</span></span>

   ![Mojify と入力する](/media-drafts/9.slack-type-mojify.png)
