この講義の目的では、mojify を渡されたイメージには、画面に表示されるように mojified イメージを返す Azure 関数の HTTP エンドポイントを作成します。

終了するまでは、次の説明は。

- 変換する方法、`JavaScript`に Azure 関数プロジェクト`TypeScript`します。
- 返す方法`raw`image 関数エンドポイントからのデータ。

## <a name="convert-to-typescript"></a>TypeScript への変換します。

コーディングに`TypeScript`が function app の作成、`JavaScript`ファイルに変換する必要があります`TypeScript`します。

1. 名前の変更、`index.js`ファイル `index.ts`

> **注**
>
> 実行している場合`npm run build`と、関連`tsc -w`コマンド、`index.ts`をすぐにファイルをコンパイル`index.js`必要もあり、`index.map`作成されたファイル。
>
> 場合、`index.js`と`index.map`ファイルは自動的に作成されませんし、再確認、`npm run build`コマンドが実行されていると、出力は、すべてのエラーを表示しません。

2. このコードを貼り付けます `index.ts`

```typescript
export async function index(context, req) {
  context.log("ReplyWithMojifiedImage HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

3. 関数アプリを実行します。

いずれかを使用してアプリが実行されて、関数について説明しました以前は、使用するかを確認、`func host start`コマンドまたは [デバッグ] メニューを使用して、アプリケーションを実行します。

次を参照してください http://localhost:7171/api/MojifyImageブラウザーでします。

すべてが正しく機能しているかどうかは、これを印刷する必要があります`Hello!`ブラウザーにします。

TypeScript は JavaScript の代わりに、関数のトリガーを変換した👏します。

## <a name="environment-variables-in-azure-functions"></a>Azure Functions で環境変数

は、コードでハードコーディング プライベート データでないことに、セキュリティで保護されました。代わりに環境変数に使用してきた。

具体的には、使用している環境変数、Face API URL とキーを格納するようになります。

```bash
export FACE_API_URL=[face-api-url]
export FACE_API_KEY=[face-api-key]
```

という名前のファイルも作成されますが、Azure Functions プロジェクトを作成するときに`local.settings.json`します。 このファイルもに、`.gitignore`ためファイル_isnot がソース管理にチェックイン_します。

公開したくない機密情報やローカルだけで構成を格納する場所になります。

既定では、ようになります。

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "node"
  }
}
```

追加したもの、`Values`オブジェクトは、環境変数として、NodeJS のコードに使用可能になります。

Face API 変数で追加した場合のようにそのようにします。

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "FACE_API_URL": "[face-api-url]",
    "FACE_API_KEY": "[face-api-key]"
  }
}
```

次に、`FACE_API_URL`と`FACE_API_KEY`よう環境変数として変数がノードで使用可能になります。

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="copy-existing-code-from-the-mojifyts-script"></a>既存のコードをコピー、`mojify.ts`スクリプト

前の講義の作成に費やされた作業、`bin\mojify.ts`コードが浪費されていない、そのコードのほとんどは、この関数にコピーされるようになりましたことができます。

すべてコピー**を除く**、`main`経由で関数を`index.ts`ファイル。

場所がかは関係ありません、ファイルの上部にあるすべてのインポートと従属する関数を作成して、`index`下部にある関数。

> **大事な**
>
> 上書きしないでください、`index`関数を操作する Azure 関数の存在する必要があります。

私たちは変更されません、 `getFaces` ; 関数全体は、変わりません。

ただし、`createMojifiedImage`関数は必要がある 1 つ変更をこの関数の元のバージョンでされたイメージを保存してコードの末尾には、このような行を使用したディスク。

```typescript
compositeImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

ディスクにファイルを保存したくないバージョンでは、新しい Azure 関数コードの_返す_イメージをユーザーに代わりに、ブラウザー ウィンドウに表示します。

呼ばれるもの必要があるため、 `buffer`、質問`Jimp`のイメージですがでのコードをラップする必要を提供していただくためにバッファーを`Promise`ため、`Jimp.getBuffer`関数は非同期で。

したがって、上記のように置き換えます`compositeImage.write`この行。

```typescript
return new Promise((resolve, reject) => {
  compositeImage.getBuffer(Jimp.MIME_JPEG, (error, buffer) => {
    // get a buffer of the composed image
    if (error) {
      let message = "There was an error adding the emoji to the image";
      context.log.error(error);
      reject(message);
    } else {
      // put the image into the context body
      resolve(buffer);
    }
  });
});
```

これは、イメージ、生のバイナリ データのバッファーを返します。

お気付きこの時点でについて認識していないことは、TypeScript で不平、`context`変数。

追加するこの問題を解決するのには`context`最初の引数として、`createMojifiedImage`関数の場合、次のように。

```typescript
async function createMojifiedImage(context, imageUrl, faces) {
  ...
}
```

## <a name="connect-it-all-in-the-index-function"></a>接続がすべて、`index`関数

このエンドポイントにアクセスするだれかにします。

1. 取得、`imageUrl`要求しています。
2. Mojify、`imageUrl`生バッファーを取得します。
3. イメージが、ブラウザーで表示されるように HTTP 要求に応答します。

インデックス関数肉づけを起動しましょう。

`context.res`プロパティは、応答を説明します。 書き込みのこの関数は、ここで設定は、ブラウザーに返される内容。

構成してみましょう`context.res`ようになります。

```typescript
context.res = {
  status: 200,
  headers: {
    "Content-Type": "image/jpeg"
  },
  isRaw: true
};
```

上記の先頭に追加、 `index` 200 の状態コードを返す応答設定関数の場合、jpeg を返すコンテンツの種類を設定し、設定、`isRaw`フラグ バイナリ返すことができるようにする場合は true を_イメージ_データ。

次に、抽出する、 `imageUrl` params を以下のクエリからどのイメージがわかるようにべき mojifying します。 ユーザーが指定しない場合は、いくつかの優れたエラー メッセージを取得します。

```typescript
const { imageUrl } = req.query;
if (!imageUrl) {
  let message = `imageUrl is required`;
  context.log.error(message);
  return context.done(message, { status: 400, body: message });
} else {
  context.log(`Called with imageUrl: "${imageUrl}"`);
}
```

最後に、呼び出したい、`getFaces`と`createMojifiedImage`関数は、ファイルの先頭に追加しました。

```typescript
// Get the response from the faceAPI
try {
  let faces = await getFaces(imageUrl);
  let buffer = await createMojifiedImage(context, imageUrl, faces);
  context.res.body = buffer;
  context.log(`Posted reply with mojified image`);
  context.done();
} catch (err) {
  let message =
    "There was an error processing this image: " + JSON.stringify(err);
  context.log.error(message);
  context.done(null, {
    status: 400,
    body: message
  });
}
```

上記の重要な 2 つの行は次のとおりです。

```typescript
context.res.body = buffer;
context.done();
```

生のバイナリ データ、応答オブジェクトを本文として設定し呼び出して`context.done()`が完了し、応答を返すのタスクを実行できます関数アプリが認識できるようにします。

Index.ts 関数のコード全体を次のようにします。

```typescript
export async function index(context, req) {
  context.log("ReplyWithMojifiedImage HTTP trigger");

  context.res = {
    status: 200,
    headers: {
      "Content-Type": "image/jpeg"
    },
    isRaw: true
  };

  const { imageUrl } = req.query;
  if (!imageUrl) {
    let message = `imageUrl is required`;
    context.log.error(message);
    return context.done(message, { status: 400, body: message });
  } else {
    context.log(`Called with imageUrl: "${imageUrl}"`);
  }

  // Get the response from the faceAPI
  try {
    let faces = await getFaces(imageUrl);
    let buffer = await createMojifiedImage(context, imageUrl, faces);
    context.res.body = buffer;
    context.log(`Posted reply with mojified image`);
    context.done();
  } catch (err) {
    let message =
      "There was an error processing this image: " + JSON.stringify(err);
    context.log.error(message);
    context.done(null, {
      status: 400,
      body: message
    });
  }
}
```

## <a name="try-it-out"></a>試してみる

デバッグ メニューから開始することや、ターミナルから実行することによって、関数アプリが実行されているようになります。

```bash
func host start
```

出力ウィンドウは、このようなものを表示する必要があり、関数アプリが正常に開始する: 場合

```
Http Functions:
        MojifyImage: http://localhost:7071/api/MojifyImage
```

Function app が正常に開始された場合に渡す URL を使用してイメージを記憶する URL を参照してください。、 `imageUrl` param、のクエリを実行するようになります。

http://localhost:7171/api/MojifyImage?imageUrl=[url-、-イメージ]

イメージの mojified バージョンを返す必要がありますすべてが動作して場合ようになります。

![Mojified イメージ](/media-drafts/7.mojified-image.png)
