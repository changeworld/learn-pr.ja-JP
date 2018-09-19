<span data-ttu-id="d208a-101">絵文字化された画像を返す `MojifyImage` Azure 関数を作成しました。`\mojify` コマンドが実行されるたびに Slack が呼び出す 2 番目のエンドポイントが必要です。</span><span class="sxs-lookup"><span data-stu-id="d208a-101">We've created the `MojifyImage` Azure Function which returns a mojified image; we need a second endpoint which slack calls every time someone executes the `\mojify` command.</span></span>

<span data-ttu-id="d208a-102">この 2 番目のエンドポイントでは、`MojifyImage` Azure 関数の URL を返す必要があります。</span><span class="sxs-lookup"><span data-stu-id="d208a-102">This second endpoint needs to return the URL to the `MojifyImage` Azure Function.</span></span>

<span data-ttu-id="d208a-103">`\mojify [some-image-url]` のような Slack コマンドを実行すると、構成されているエンドポイントに対して POST 要求が行われ、メッセージの本文に埋め込まれた `text` クエリ パラメーターとして `[some-image-url]` が渡されます。</span><span class="sxs-lookup"><span data-stu-id="d208a-103">When you run a slack command like `\mojify [some-image-url]` it makes a POST request to the endpoint you have configured and pass in `[some-image-url]` as a `text` query param embedded in the body of the message.</span></span>

<span data-ttu-id="d208a-104">この講義の目標は、Slack コマンドを調整してそれに応答するこの関数を作成することです。この 2 番目の Azure 関数は `RespondToSlackCommand` という名前にします。</span><span class="sxs-lookup"><span data-stu-id="d208a-104">The goal of this lecture is to create this function that coordinates and responds to the slack commands; we call this second Azure Function `RespondToSlackCommand`.</span></span>

<span data-ttu-id="d208a-105">この講義が終了するまでに、次の内容を学習します。</span><span class="sxs-lookup"><span data-stu-id="d208a-105">By the end of this lecture you will learn:</span></span>

- <span data-ttu-id="d208a-106">Slack コマンドに応答する HTTP エンドポイントを作成する方法。Slack が要求に期待する形式と、画像に応答する方法を理解します。</span><span class="sxs-lookup"><span data-stu-id="d208a-106">How to create a HTTP endpoint which responds to Slack commands, you will understand the format Slack expects for the request and how to respond with an image.</span></span>

## <a name="create-the-function-trigger-and-convert-to-typescript"></a><span data-ttu-id="d208a-107">関数トリガーを作成して TypeScript に変換する</span><span class="sxs-lookup"><span data-stu-id="d208a-107">Create the Function Trigger and convert to TypeScript</span></span>

<span data-ttu-id="d208a-108">HTTP でトリガーされるもう 1 つの Azure 関数を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d208a-108">We need to create another HTTP Triggered Azure Function.</span></span> <span data-ttu-id="d208a-109">手順は前の講義と同じです。唯一の違いは、この関数の名前が `MojifyImage` ではなく `RespondToSlackCommand` であることです。</span><span class="sxs-lookup"><span data-stu-id="d208a-109">These instructions are the same as the previous lecture; the only difference is that we are calling this function `RespondToSlackCommand` instead of `MojifyImage`.</span></span>

1. <span data-ttu-id="d208a-110"><kbd>Ctrl</kbd> + <kbd>P</kbd> キーを押してコマンド パレットを開き、`Azure Functions: Create Function...` を検索して選択します</span><span class="sxs-lookup"><span data-stu-id="d208a-110">Type <kbd>Ctrl</kbd>+<kbd>P</kbd> to open up the command palette, then search for and select `Azure Functions: Create Function...`</span></span>

   ![新しい関数を作成する](/media-drafts/7.create-function.png)

2. <span data-ttu-id="d208a-112">先ほど関数プロジェクトを作成したフォルダーを選択します (1 つ目のオプションは現在のフォルダー)</span><span class="sxs-lookup"><span data-stu-id="d208a-112">Select the folder where you created the function project previously (the first option is the current folder)</span></span>

   ![フォルダーを選択する](/media-drafts/7.select-current-project.png)

3. <span data-ttu-id="d208a-114">ここで作成するのは "_HTTP トリガー_" なので、そのオプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="d208a-114">We are creating a _HTTP Trigger_, so select that option.</span></span>

   ![フォルダーを選択する](/media-drafts/7.select-trigger.png)

4. <span data-ttu-id="d208a-116">作成する関数の名前として「`RespondToSlackCommand`」と入力します</span><span class="sxs-lookup"><span data-stu-id="d208a-116">Type in `RespondToSlackCommand` as the name of the Function you want to create</span></span>

   ![名前を選択する](/media-drafts/7.choose-function-name.png)

5. <span data-ttu-id="d208a-118">認証レベルとして匿名レベルの認証を選択します</span><span class="sxs-lookup"><span data-stu-id="d208a-118">Choose Anonymous level authentication as the authentication level</span></span>

   ![名前を選択する](/media-drafts/7.choose-auth-level.png)

<span data-ttu-id="d208a-120">正常に完了すると、`RespondToSlackCommand` という名前のフォルダーがルート ディレクトリに作成されます。</span><span class="sxs-lookup"><span data-stu-id="d208a-120">If it completed successfully then a folder called `RespondToSlackCommand` should be present in the root directory.</span></span>

<span data-ttu-id="d208a-121">前回の講義と同じように、これを `JavaScript` から `TypeScript` に変換します。</span><span class="sxs-lookup"><span data-stu-id="d208a-121">Same as the last lecture let's now convert this from `JavaScript` to `TypeScript`.</span></span>

6. <span data-ttu-id="d208a-122">`RespondToSlackCommand` フォルダーに `index.ts` という名前のファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="d208a-122">Create a file called `index.ts` in the `RespondToSlackCommand` folder.</span></span>

   <span data-ttu-id="d208a-123">`TypeScript` ビルド プロセスがまだ動いていること、および `index.js` ファイルと `index.map` ファイルに自動的にコンパイルされたことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d208a-123">Ensure that the `TypeScript` build process is still running and that it automatically compiled it into `index.js` and `index.map` files.</span></span>

7. <span data-ttu-id="d208a-124">このコードを `index.ts` に貼り付けます</span><span class="sxs-lookup"><span data-stu-id="d208a-124">Paste this code in `index.ts`</span></span>

```typescript
export async function index(context, req) {
  context.log("RespondToSlackCommand HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

## <a name="try-it-out"></a><span data-ttu-id="d208a-125">試してみる</span><span class="sxs-lookup"><span data-stu-id="d208a-125">Try it out</span></span>

<span data-ttu-id="d208a-126">ブラウザーで http://localhost:7171/api/RespondToSlackCommand にアクセスしてすべてが動いていることを確認します。`Hello!` と表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="d208a-126">Ensure that everything is working by visiting http://localhost:7171/api/RespondToSlackCommand in a browser, it should print our `Hello!`.</span></span>

## <a name="flesh-out-the-index-function"></a><span data-ttu-id="d208a-127">`index` 関数を具体化する</span><span class="sxs-lookup"><span data-stu-id="d208a-127">Flesh out the `index` function</span></span>

<span data-ttu-id="d208a-128">この `index` 関数は前の関数よりずっと簡単に作成できます。</span><span class="sxs-lookup"><span data-stu-id="d208a-128">This `index` function is a lot quicker to write than the previous function.</span></span>

<span data-ttu-id="d208a-129">最初に `context.res` オブジェクトを設定します</span><span class="sxs-lookup"><span data-stu-id="d208a-129">Let's first setup our `context.res` object</span></span>

```typescript
context.res = {
  headers: {
    "Content-Type": "application/json"
  },
  body: null
};
```

<span data-ttu-id="d208a-130">前より簡単です。`isRaw` プロパティは既定値の `false` にするので設定しませんが、コンテンツ タイプは `application/json` に設定します。</span><span class="sxs-lookup"><span data-stu-id="d208a-130">Simpler than before, we don't set the `isRaw` property since this defaults to `false`, but we do set the content type to be `application/json`.</span></span>

<span data-ttu-id="d208a-131">Slack による画像 URL の送信で前に説明したように、`text` のキーを持つクエリ文字列として処理しますが、セキュリティ上の理由から要求の本文に埋め込まれているので、必要な情報の取得は少し難しくなりますが、難しすぎることはありません。</span><span class="sxs-lookup"><span data-stu-id="d208a-131">As mentioned earlier on Slack sends the image URL we want to process as a query string with the key of `text`, but for security reasons it embeds this in the body of the request, so we need to work a little harder to get the information we need, not too hard though!</span></span>

<span data-ttu-id="d208a-132">簡単にするため、ノード `querystring` パッケージをインポートします。これは NodeJS の一部なので何も余分にインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d208a-132">To make our lives easier let's import the node `querystring` package, this comes as part of NodeJS so no need to install anything extra.</span></span>

```typescript
import * as querystring from "querystring";
```

<span data-ttu-id="d208a-133">次に、`index` 関数で `req.body` をオブジェクトに変換でき、それから `text` プロパティを抽出します。</span><span class="sxs-lookup"><span data-stu-id="d208a-133">Then in our `index` function we can convert the `req.body` into an object from which we will extract the `text` property, like so:</span></span>

```typescript
const { text } = querystring.parse(req.body);
```

<span data-ttu-id="d208a-134">ユーザーがコマンドを正しく入力した場合、テキストには画像の URL が含まれます。基本的な検証を追加できます。</span><span class="sxs-lookup"><span data-stu-id="d208a-134">If the user typed the command properly then the text should contain the URL of an image, we can add in some basic validation like so:</span></span>

```typescript
let message = "Your mojified image my liege...";
if (!text) {
  message = "You must provide an image to mojify";
}
```

<span data-ttu-id="d208a-135">Slack のコマンドが https://somedomain.com/api/RespondToSlackCommand を呼び出していることを思い出してください。MojifyImage URL で応答する必要があります。通常は、同じドメイン https://somedomain.com/api/MojifyImage にあります</span><span class="sxs-lookup"><span data-stu-id="d208a-135">Remember the slack command is calling https://somedomain.com/api/RespondToSlackCommand, and we need to respond with the MojifyImage URL which will most likely be on the same domain, https://somedomain.com/api/MojifyImage</span></span>

<span data-ttu-id="d208a-136">ファイルにドメインをハードコーディングするのではなく、要求と同じドメインを `MojifyImage` URL のドメインとして使用します。</span><span class="sxs-lookup"><span data-stu-id="d208a-136">Rather than hardcoding the domain in the file, let's instead use the same domain as the request as the domain of te `MojifyImage` url, like so:</span></span>

```typescript
const mojifyUrl = req.url.substr(0, req.url.lastIndexOf("/")) + "/MojifyImage";
```

<span data-ttu-id="d208a-137">最後に応答の本体を設定してみましょう。slack コマンドは､応答が次のような形式であると想定しています。</span><span class="sxs-lookup"><span data-stu-id="d208a-137">Finally let's set the body of the response, the slack command expects a specific format in the response, like so:</span></span>

```typescript
context.res.body = {
  response_type: "in_channel",
  text: message,
  attachments: [
    {
      image_url: `${mojifyUrl}?imageUrl=${text}`
    }
  ]
};

context.done();
```

<span data-ttu-id="d208a-138">上で重要なのは、`attachments` プロパティの `image_url` です。ユーザーが `imageUrl` クエリ パラメーターとしてコマンドで指定した URL で渡す `mojifyUrl` を返すようにこれを設定します。</span><span class="sxs-lookup"><span data-stu-id="d208a-138">The critical thing to note above is the `image_url` in the `attachments` property; we set this to the return the `mojifyUrl` passing in the URL the user supplied in the command as the `imageUrl` query param.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="d208a-139">試してみる</span><span class="sxs-lookup"><span data-stu-id="d208a-139">Try it out</span></span>

<span data-ttu-id="d208a-140">ブラウザーで http://localhost:7171/api/RespondToSlackCommand にアクセスしてすべてが動いていることを確認します。次のような `json` が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="d208a-140">Ensure that everything is working by visiting http://localhost:7171/api/RespondToSlackCommand in a browser, it should now print out some `json` like the below:</span></span>

```json
{
  "response_type": "in_channel",
  "text": "You must provide an image to mojify",
  "attachments": [
    {
      "image_url": "http://localhost:7071/api/MojifyImage?imageUrl=undefined"
    }
  ]
}
```
