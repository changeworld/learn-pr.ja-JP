最後の章では､`shared/mojis.ts` ファイルに絵文字とその感情点数の一覧であることが分かりました｡

この章では、顔を絵文字に対応づけるために使用するコードの残りの部分について学びます｡

学習内容:

1. 顔画像の URL で渡すスクリプトを作成する｡
2. 画像上の顔の感情点数を計算する｡
3. 画像のすべての顔を最も近い絵文字に対応づける｡

最終的には､この機能を Slack コマンドとして実装しますが､さしあたりここでは､このようにコマンドラインから実行できる簡単なノード スクリプトから始めましょう｡

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript"></a>TypeScript のデバッグ

作成は TypeScript ですが､実行は JavaScript になります｡ これにより、ブレークポイントを設定したり、読みにくいトランスパイル JavaScript ファイルでデバッグする必要があるため､デバッグは難しい作業になります｡

理想的には､作成_も_デバッグも TypeScript で行いたいところです｡

グッドニュースは､ほんのわずかな構成で vs code でそのことが可能であることです｡

コマンド paletee `Ctrl+P > Debug: Open launch.json` を使用して `launch.json` ファイルを開きます｡

> TODO: 画像

次のような構成オプションで追加します｡

```json
{
  "type": "node",
  "request": "launch",
  "name": "Mojify",
  "env": {
    "DEBUG": "*"
  },
  "args": [
    "https://pbs.twmedia-drafts.com/profile_images/833970306339446784/83MO53R9_400x400.jpg"
  ],
  "sourceMaps": true,
  "stopOnEntry": false,
  "console": "integratedTerminal",
  "cwd": "${workspaceRoot}",
  "program": "${workspaceRoot}/bin/mojify.js",
  "outFiles": ["${workspaceRoot}/bin/mojify.js"]
}
```

[デバッグ] メニューには､`Mojify` という名前のオプションがあります｡このオプションは､URL の引数として渡される mojify スクリプトを実行します｡ TypeScript ファイルにブレークポイントを設定し、そこから直接変数を調べることができます。

## <a name="open-up-binmojists"></a>`bin/mojis.ts` を開きます｡

このファイルには､次のように必要とされるすべてのインポートがすでに取り込まれています｡

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

`dotenv` は開発に有用なヘルパー パッケージです｡このパッケージは､プロジェクトのルートにある .env ファイルの内容を環境変数としてロードします｡

Azure の Face API に http 要求を行うために使用する `node-fetch`｡

`EmotivePoint` は､ _感情空間上の点数を表すヘルパークラス_ です｡このクラスについては､後ほど詳しく説明します｡

`Rect` は､矩形を表すためのヘルパー クラスです｡このクラスを使用して､画像上の顔の位置を表します｡

`Face` は､画像上の顔の矩形と感情点数情報を組み合わせるヘルパー ユーティリティ クラスです。

ファイルの中ほどに､このような stub 関数がいくつかあるはずです｡

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

これらは、本講義と次の講義で肉付けする関数です｡

ファイルの最後には、次のコードがあるはずです｡

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a>環境変数を追加する

Face API を呼び出します｡以前に生成した秘密キーと URL を利用する必要があります｡ファイルの先頭にある imports の下にこれを追加します｡

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a>指定画像で Face API を呼び出し、応答を取得します。

Face API にリクエストを行うには､`getFaces` 関数の先頭にこのコードを追加します｡

```typescript
let response = await fetch(API_URL, {
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

上記のコードは `fetch` コマンドを使用して Face API に`POST`要求を送信します｡

ヘッダーで `API_KEY` を渡します｡それにより、Face API は私たちの要求を認識し､そうでない場合は､拒否します｡

本体で Face API に分析させたい `imageUrl` を渡します｡

API 要求から応答があり､印刷します｡

スクリプトを実行すると､

```bash
node bin/mojify.js <path-to-image>
```

Face API に画像を渡すことで返された JSON 応答が印刷されます｡

## <a name="parse-the-responce"></a>応答を解析する

絵文字を求めるには､API からの応答で返されたそれぞれの顔を `Face` クラスの 1 つのインスタンスに変換する必要があります。

`getFaces` 関数で API を呼び出すコードの直後に次のコードを追加します｡

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

- 応答で返される顔を順にループします。
- 返された JSON から `EmotivePoint` と `Rect`､ `Face` を生成します｡
- `Face` インスタンスを作成すると､その顔が絵文字に対応づけられます｡
- 対応づけられた絵文字を確認するには､`mojiicon` を印刷します。

## <a name="try-it-out"></a>試してみる

スクリプトを実行すると､以下のことが起こります｡

- 指定した画像が Face API を使って渡され、感情が計算されます｡
- 感情が絵文字に対応づけられます｡
- 端末に絵文字が印刷されます。

次に例を示します。

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
