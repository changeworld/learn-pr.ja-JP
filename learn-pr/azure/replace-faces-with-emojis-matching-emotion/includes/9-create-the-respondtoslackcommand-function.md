作成した、 `MojifyImage` Azure 関数 mojified イメージが返されます。 2 番目のエンドポイントがどの slack がだれかが実行されるたびに呼び出す必要があります、`\mojify`コマンド。

この 2 番目のエンドポイントの URL を返す必要があります、 `MojifyImage` Azure 関数。

Slack のようなコマンドを実行すると`\mojify [some-image-url]`に POST 要求が構成されているエンドポイントに渡す`[some-image-url]`として、`text`メッセージの本文に埋め込まれているクエリ パラメーター。

この講義の目標は、このを調整し、slack のコマンドに応答する関数を作成するにはこの 2 つ目の Azure 関数を呼び出して`RespondToSlackCommand`します。

この講義の終わりまでには、次の説明は。

- Slack に応答する HTTP エンドポイントを作成する方法がコマンド、Slack イメージで応答する方法と、要求が期待する形式を理解します。

## <a name="create-the-function-trigger-and-convert-to-typescript"></a>関数のトリガーを作成し、TypeScript への変換

HTTP トリガーされる Azure 関数のもう 1 つを作成する必要があります。 これらの手順は前の講義; と同じです。唯一の違いは、この関数を呼び出している`RespondToSlackCommand`の代わりに`MojifyImage`します。

1. 型<kbd>Ctrl</kbd>+<kbd>P</kbd>コマンド パレットを起動し、検索して選択するには `Azure Functions: Create Function...`

   ![新しい関数を作成します。](/media-drafts/7.create-function.png)

2. 以前で関数プロジェクトを作成したフォルダーを選択します (最初のオプションは、現在のフォルダー)

   ![フォルダーの選択](/media-drafts/7.select-current-project.png)

3. ここでは作成、 _HTTP トリガー_、ようにするオプションを選択します。

   ![フォルダーの選択](/media-drafts/7.select-trigger.png)

4. 入力`RespondToSlackCommand`として作成する関数の名前

   ![名前を選択します。](/media-drafts/7.choose-function-name.png)

5. 認証レベルとして匿名レベルの認証を選択します。

   ![名前を選択します。](/media-drafts/7.choose-auth-level.png)

正常に完了した、という名前のフォルダー場合`RespondToSlackCommand`ルート ディレクトリに存在する必要があります。

前回と同じ lecture みましょう今すぐ変換から`JavaScript`に`TypeScript`します。

6. という名前のファイルを作成する`index.ts`で、`RespondToSlackCommand`フォルダー。

   いることを確認、`TypeScript`ビルド プロセスがまだ実行されていると、自動的にコンパイルされた`index.js`と`index.map`ファイル。

7. このコードを貼り付けます `index.ts`

```typescript
export async function index(context, req) {
  context.log("RespondToSlackCommand HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

## <a name="try-it-out"></a>試してみる

アクセスしてすべてが動作していることを確認 http://localhost:7171/api/RespondToSlackCommandブラウザーで印刷する必要がありますが、`Hello!`します。

## <a name="flesh-out-the-index-function"></a>肉付け、`index`関数

これは、`index`関数は、前の関数よりもきわめて迅速です。

最初にセットアップ、`context.res`オブジェクト

```typescript
context.res = {
  headers: {
    "Content-Type": "application/json"
  },
  body: null
};
```

設定していない前よりも簡単な`isRaw`プロパティの既定値はため`false`、するコンテンツ タイプ設定の操作を行いますが、`application/json`します。

キーを持つクエリ文字列として処理する Slack イメージの URL を送信する前に説明したように`text`セキュリティ上の理由埋め込むこの要求の本文に必要な情報を取得するもっと一生懸命させる必要がありますが、、難しすぎる場合です。

私たちの生活をインポート、ノードが簡単に`querystring`パッケージ、これは NodeJS の一部としてされないように余分なソフトウェアをインストールする必要があります。

```typescript
import * as querystring from "querystring";
```

次に、`index`関数に変換するには、`req.body`抽出元となるオブジェクトに、`text`プロパティでは、次のように。

```typescript
const { text } = querystring.parse(req.body);
```

ユーザーがコマンドを正しく入力したかどうかは、テキストは、イメージの URL を含める必要がありますでいくつかの基本的な検証を追加できるようになります。

```typescript
let message = "Your mojified image my liege...";
if (!text) {
  message = "You must provide an image to mojify";
}
```

Slack のコマンドを呼び出すことに注意してください https://somedomain.com/api/RespondToSlackCommand、ほとんどは MojifyImage URL で応答すると、同じドメインの場合があります。 https://somedomain.com/api/MojifyImage

ファイル内のドメイン、ハードコーディングするのではなく代わりに、同じドメインとして使用 te のドメインとして要求`MojifyImage`url、次のようにします。

```typescript
const mojifyUrl = req.url.substr(0, req.url.lastIndexOf("/")) + "/MojifyImage";
```

最後に設定してみましょう slack コマンド応答の本文では、特定の形式の応答には、次のようにします。

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

上、重要なことです、`image_url`で、`attachments`プロパティは、戻り値に設定します、 `mojifyUrl` URL としてコマンドに指定されたユーザーを渡して、`imageUrl`パラメーターをクエリします。

## <a name="try-it-out"></a>試してみる

アクセスしてすべてが動作していることを確認 http://localhost:7171/api/RespondToSlackCommandブラウザーで、今すぐが出力されます一部`json`の下。

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
