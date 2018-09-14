最後の章でについて説明してきましたが`shared/mojis.ts`ファイル絵文字と、感情的なポイントの一覧があります。

この章では、顔を絵文字にマップを使用してコードの残りの部分について説明します。

学習内容は次のとおりです。

1. 顔のイメージの URL を渡すことのスクリプトを作成します。
2. イメージ内のすべての顔の感情のポイントを計算します。
3. イメージに含まれる面を最も近い絵文字にマップします。

機能的には、Slack をコマンドとして実装最終的には、ここでは、ここでは最初に、単純なノード スクリプト、コマンドラインで実行できるようになります。

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript-in-vs-code"></a>VS Code で TypeScript のデバッグ

記述している`TypeScript`が、実行時`JavaScript`。 これにより、デバッグ ブレークポイントを設定し、デバッグに終わるだろうとハードで、トランス パイルされた JavaScript ファイル読みにくくなることができます。

書き込みには、理想的には必要な_と_TypeScript でデバッグします。

良い知らせは、ほんの少しの構成で vs code で可能であります。

開き、`launch.json`コマンド palete を使用してファイル<kbd>Ctrl</kbd>+<kbd>P</kbd> 」と入力します。 `Debug: Open launch.json`

![開く起動 Json](/media-drafts/5.open-debug-launch.json.png)

これにより、`launch.json`構成ファイル。 追加し、デバッグ構成を削除するのには、このファイルを編集します。

追加の設定の配列にデバッグ構成オプションの下。

```json
{
  "type": "node",
  "request": "launch",
  "name": "Mojify",
  "env": {
    "DEBUG": "*"
  },
  "args": [
    "https://pbs.twimg.com/profile_images/833970306339446784/83MO53R9_400x400.jpg"
  ],
  "sourceMaps": true,
  "stopOnEntry": false,
  "console": "integratedTerminal",
  "cwd": "${workspaceRoot}",
  "program": "${workspaceRoot}/bin/mojify.js",
  "outFiles": ["${workspaceRoot}/bin/mojify.js"]
}
```

デバッグ メニューのオプションが参照してください。 これで`Mojify`、引数として URL を渡して mojify スクリプトを実行します。 ブレークポイントを設定することができます、`TypeScript`ファイルし、そこから直接変数を確認します。

## <a name="open-up-binmojists"></a>広げて下さい `bin/mojis.ts`

ファイルが必要なすべてのインポートでスキャフォールディングされた既に次のようにします。

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

`dotenv` ヘルパー パッケージ開発のための便利な環境変数として、プロジェクトのルートに .env ファイルの内容を読み込みます。

`node-fetch` Azure の Face API に HTTP 要求を行うを使用します。

`EmotivePoint` 特定の時点を表すヘルパー クラスは、_感情的な領域_-この詳細については、以下で説明します。

`Rect` 四角形を記述するヘルパー クラスは、イメージ内の顔の位置を記述するこれを使用します。

`Face` 四角形と emotive ポイントについては、イメージ内の顔を取りまとめたヘルパー ユーティリティ クラスです。

いくつかのスタブ関数の表示、ファイルの途中で次のようにします。

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

これらは、この講演で、[次へ] 肉づけするは関数です。

ファイルの末尾には、このコードが表示されます。

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a>環境変数を追加します。

ここ、Face API を呼び出すため、これらの秘密キーとする前に生成した Url を使用して、インポート ファイルの先頭に追加する必要があります。

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a>指定されたイメージで Face API を呼び出すし、応答を取得します。

Face API を要求するには、先頭に次のコードを追加しました、`getFaces`関数

```typescript
const fullUrl =
  API_URL +
  "/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion";

let response = await fetch(fullUrl, {
  headers: {
    "Ocp-Apim-Subscription-Key": API_KEY,
    "Content-Type": "application/json"
  },
  method: "POST",
  body: JSON.stringify({ url: imageUrl })
});
let resp = await response.json();
console.log(resp);
```

使用上のコード、`fetch`コマンドを送信する、 `POST` Face API に要求します。

> **注**
>
> 追加する必要があります`/detect`と顔を検出し、感情を返すことも、取得するために API_URL をいくつかのクエリ パラメーター。

渡して、`API_KEY`ヘッダーで Face API が認識できるように、要求元が私たちは、それ以外の場合、要求は拒否されます。

渡して、`imageUrl`を本文に分析して、Face API をいたします。

API の要求から応答が得られる次を印刷すること。

使用してスクリプトを実行する場合

```bash
node bin/mojify.js <path-to-image>
```

Face API にそのイメージを渡すから返される JSON 応答が出力されます。

## <a name="parse-the-response"></a>応答を解析します。

Ee は、絵文字を計算する、API からのインスタンスへの応答で返されるそれぞれの顔を変換する必要があります、`Face`クラス。

このコードを追加で API を呼び出すコードの直後後、`getFaces`関数。

```typescript
let faces = [];
for (let f of resp) {
  let scores = new EmotivePoint(f.faceAttributes.emotion);
  let faceRectangle = new Rect(f.faceRectangle);
  let face = new Face(scores, faceRectangle);
  console.log(face.mojiIcon);
  faces.push(face);
}
return faces;
```

- 応答で返されるそれぞれの顔をループします。
- 生成、 `EmotivePoint`、`Rect`と`Face`返された json から。
- 作成、`Face`インスタンスが、絵文字を顔と一致します。
- 印刷する絵文字の一致を表示する、`mojiicon`します。

## <a name="try-it-out"></a>試してみる

これで、スクリプトを実行する場合は、次のことを行ってください。

1. Face API を使って指定されたイメージを渡すし、感情を計算します。
2. 絵文字を感情と一致します。
3. 端末に絵文字を印刷します。

次に例を示します。

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
