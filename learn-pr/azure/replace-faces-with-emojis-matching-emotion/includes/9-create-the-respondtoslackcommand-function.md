絵文字化された画像を返す `MojifyImage` Azure 関数を作成しました。`\mojify` コマンドが実行されるたびに Slack が呼び出す 2 番目のエンドポイントが必要です。

この 2 番目のエンドポイントでは、`MojifyImage` Azure 関数の URL を返す必要があります。

`\mojify [some-image-url]` のような Slack コマンドを実行すると、構成されているエンドポイントに対して POST 要求が行われ、メッセージの本文に埋め込まれた `text` クエリ パラメーターとして `[some-image-url]` が渡されます。

この講義の目標は、Slack コマンドを調整してそれに応答するこの関数を作成することです。この 2 番目の Azure 関数は `RespondToSlackCommand` という名前にします。

この講義が終了するまでに、次の内容を学習します。

- Slack コマンドに応答する HTTP エンドポイントを作成する方法。Slack が要求に期待する形式と、画像に応答する方法を理解します。

## <a name="create-the-function-trigger-and-convert-to-typescript"></a>関数トリガーを作成して TypeScript に変換する

HTTP でトリガーされるもう 1 つの Azure 関数を作成する必要があります。 手順は前の講義と同じです。唯一の違いは、この関数の名前が `MojifyImage` ではなく `RespondToSlackCommand` であることです。

1. <kbd>Ctrl</kbd> + <kbd>P</kbd> キーを押してコマンド パレットを開き、`Azure Functions: Create Function...` を検索して選択します

   ![新しい関数を作成する](/media-drafts/7.create-function.png)

2. 先ほど関数プロジェクトを作成したフォルダーを選択します (1 つ目のオプションは現在のフォルダー)

   ![フォルダーを選択する](/media-drafts/7.select-current-project.png)

3. ここで作成するのは "_HTTP トリガー_" なので、そのオプションを選択します。

   ![フォルダーを選択する](/media-drafts/7.select-trigger.png)

4. 作成する関数の名前として「`RespondToSlackCommand`」と入力します

   ![名前を選択する](/media-drafts/7.choose-function-name.png)

5. 認証レベルとして匿名レベルの認証を選択します

   ![名前を選択する](/media-drafts/7.choose-auth-level.png)

正常に完了すると、`RespondToSlackCommand` という名前のフォルダーがルート ディレクトリに作成されます。

前回の講義と同じように、これを `JavaScript` から `TypeScript` に変換します。

6. `RespondToSlackCommand` フォルダーに `index.ts` という名前のファイルを作成します。

   `TypeScript` ビルド プロセスがまだ動いていること、および `index.js` ファイルと `index.map` ファイルに自動的にコンパイルされたことを確認します。

7. このコードを `index.ts` に貼り付けます

```typescript
export async function index(context, req) {
  context.log("RespondToSlackCommand HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

## <a name="try-it-out"></a>試してみる

ブラウザーで http://localhost:7171/api/RespondToSlackCommand にアクセスしてすべてが動いていることを確認します。`Hello!` と表示されるはずです。

## <a name="flesh-out-the-index-function"></a>`index` 関数を具体化する

この `index` 関数は前の関数よりずっと簡単に作成できます。

最初に `context.res` オブジェクトを設定します

```typescript
context.res = {
  headers: {
    "Content-Type": "application/json"
  },
  body: null
};
```

前より簡単です。`isRaw` プロパティは既定値の `false` にするので設定しませんが、コンテンツ タイプは `application/json` に設定します。

Slack による画像 URL の送信で前に説明したように、`text` のキーを持つクエリ文字列として処理しますが、セキュリティ上の理由から要求の本文に埋め込まれているので、必要な情報の取得は少し難しくなりますが、難しすぎることはありません。

簡単にするため、ノード `querystring` パッケージをインポートします。これは NodeJS の一部なので何も余分にインストールする必要はありません。

```typescript
import * as querystring from "querystring";
```

次に、`index` 関数で `req.body` をオブジェクトに変換でき、それから `text` プロパティを抽出します。

```typescript
const { text } = querystring.parse(req.body);
```

ユーザーがコマンドを正しく入力した場合、テキストには画像の URL が含まれます。基本的な検証を追加できます。

```typescript
let message = "Your mojified image my liege...";
if (!text) {
  message = "You must provide an image to mojify";
}
```

Slack のコマンドが https://somedomain.com/api/RespondToSlackCommand を呼び出していることを思い出してください。MojifyImage URL で応答する必要があります。通常は、同じドメイン https://somedomain.com/api/MojifyImage にあります

ファイルにドメインをハードコーディングするのではなく、要求と同じドメインを `MojifyImage` URL のドメインとして使用します。

```typescript
const mojifyUrl = req.url.substr(0, req.url.lastIndexOf("/")) + "/MojifyImage";
```

最後に応答の本体を設定してみましょう。slack コマンドは､応答が次のような形式であると想定しています。

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

上で重要なのは、`attachments` プロパティの `image_url` です。ユーザーが `imageUrl` クエリ パラメーターとしてコマンドで指定した URL で渡す `mojifyUrl` を返すようにこれを設定します。

## <a name="try-it-out"></a>試してみる

ブラウザーで http://localhost:7171/api/RespondToSlackCommand にアクセスしてすべてが動いていることを確認します。次のような `json` が表示されるはずです。

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
