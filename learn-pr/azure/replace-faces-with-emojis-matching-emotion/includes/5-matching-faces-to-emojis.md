<span data-ttu-id="5dcdd-101">最後の章では､`shared/mojis.ts` ファイルに絵文字とその感情点数の一覧であることが分かりました｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-101">In the last chapter we learnt that `shared/mojis.ts` file has a list of emojis and their emotional points.</span></span>

<span data-ttu-id="5dcdd-102">この章では、顔を絵文字に対応づけるために使用するコードの残りの部分について学びます｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-102">In this chapter we will learn about the rest of the code that we will use to map a face to an emoji.</span></span>

<span data-ttu-id="5dcdd-103">学習内容:</span><span class="sxs-lookup"><span data-stu-id="5dcdd-103">You will learn how to:</span></span>

1. <span data-ttu-id="5dcdd-104">顔画像の URL で渡すスクリプトを作成する｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-104">Create a script where you pass in a URL of an image of a face.</span></span>
2. <span data-ttu-id="5dcdd-105">画像上の顔の感情点数を計算する｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-105">Calculate the emotional point of any faces in the image.</span></span>
3. <span data-ttu-id="5dcdd-106">画像のすべての顔を最も近い絵文字に対応づける｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-106">Map the faces in the image to the closest emojis</span></span>

<span data-ttu-id="5dcdd-107">最終的には､この機能を Slack コマンドとして実装しますが､さしあたりここでは､このようにコマンドラインから実行できる簡単なノード スクリプトから始めましょう｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-107">Eventually we will be implementing this functionally as a Slack command, but for now we are going to start with a simple node script that you can run on the comand line, like so:</span></span>

```bash
node bin/mojify.js <url-of-image-with-face>
```

## <a name="debugging-typescript"></a><span data-ttu-id="5dcdd-108">TypeScript のデバッグ</span><span class="sxs-lookup"><span data-stu-id="5dcdd-108">Debugging TypeScript</span></span>

<span data-ttu-id="5dcdd-109">作成は TypeScript ですが､実行は JavaScript になります｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-109">We are writing in TypeScript but executing JavaScript.</span></span> <span data-ttu-id="5dcdd-110">これにより、ブレークポイントを設定したり、読みにくいトランスパイル JavaScript ファイルでデバッグする必要があるため､デバッグは難しい作業になります｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-110">This makes debugging hard as we would have to set breakpoints and debug in the transpiled JavaScript files which can be hard to read.</span></span>

<span data-ttu-id="5dcdd-111">理想的には､作成_も_デバッグも TypeScript で行いたいところです｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-111">What we ideally want is to write _and_ debug in TypeScript.</span></span>

<span data-ttu-id="5dcdd-112">グッドニュースは､ほんのわずかな構成で vs code でそのことが可能であることです｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-112">The good news is that it's possible with vs code with just a little bit of configuration.</span></span>

<span data-ttu-id="5dcdd-113">コマンド paletee `Ctrl+P > Debug: Open launch.json` を使用して `launch.json` ファイルを開きます｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-113">Open up the `launch.json` file by using the command paletee `Ctrl+P > Debug: Open launch.json`</span></span>

> <span data-ttu-id="5dcdd-114">TODO: 画像</span><span class="sxs-lookup"><span data-stu-id="5dcdd-114">TODO: Image</span></span>

<span data-ttu-id="5dcdd-115">次のような構成オプションで追加します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-115">Add in a configuration option like so:</span></span>

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

<span data-ttu-id="5dcdd-116">[デバッグ] メニューには､`Mojify` という名前のオプションがあります｡このオプションは､URL の引数として渡される mojify スクリプトを実行します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-116">Now in the debug menu you will see an option called `Mojify` this will run the mojify script passing in a URL as the argument.</span></span> <span data-ttu-id="5dcdd-117">TypeScript ファイルにブレークポイントを設定し、そこから直接変数を調べることができます。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-117">You will be able to set breakpoints in the TypeScript file and inspect variables directly from there.</span></span>

## <a name="open-up-binmojists"></a><span data-ttu-id="5dcdd-118">`bin/mojis.ts` を開きます｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-118">Open up `bin/mojis.ts`</span></span>

<span data-ttu-id="5dcdd-119">このファイルには､次のように必要とされるすべてのインポートがすでに取り込まれています｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-119">The file is already scaffolded with all the required imports, like so:</span></span>

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

<span data-ttu-id="5dcdd-120">`dotenv` は開発に有用なヘルパー パッケージです｡このパッケージは､プロジェクトのルートにある .env ファイルの内容を環境変数としてロードします｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-120">`dotenv` is a helper package which loads up the contents of a .env file in the root of your project as environment variables, useful for development.</span></span>

<span data-ttu-id="5dcdd-121">Azure の Face API に http 要求を行うために使用する `node-fetch`｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-121">`node-fetch` we use to make http requests to the Azure Face API.</span></span>

<span data-ttu-id="5dcdd-122">`EmotivePoint` は､ _感情空間上の点数を表すヘルパークラス_ です｡このクラスについては､後ほど詳しく説明します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-122">`EmotivePoint` is a helper class that describes a point in _emotional space_ - we will be discussing this in more details below.</span></span>

<span data-ttu-id="5dcdd-123">`Rect` は､矩形を表すためのヘルパー クラスです｡このクラスを使用して､画像上の顔の位置を表します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-123">`Rect` is a helper class to describe a rectangle shape, we use this to describe the position of a face in an image.</span></span>

<span data-ttu-id="5dcdd-124">`Face` は､画像上の顔の矩形と感情点数情報を組み合わせるヘルパー ユーティリティ クラスです。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-124">`Face` is a helper utility class which combines the rectangle and emotive point informatoin about a face in an image.</span></span>

<span data-ttu-id="5dcdd-125">ファイルの中ほどに､このような stub 関数がいくつかあるはずです｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-125">In the middle of the file you should see some stub functions, like so:</span></span>

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

<span data-ttu-id="5dcdd-126">これらは、本講義と次の講義で肉付けする関数です｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-126">These are the functions you will be fleshing out in this lecture and the next</span></span>

<span data-ttu-id="5dcdd-127">ファイルの最後には、次のコードがあるはずです｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-127">At the end of the file you should see this code</span></span>

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## <a name="add-the-environment-variables"></a><span data-ttu-id="5dcdd-128">環境変数を追加する</span><span class="sxs-lookup"><span data-stu-id="5dcdd-128">Add the environment variables</span></span>

<span data-ttu-id="5dcdd-129">Face API を呼び出します｡以前に生成した秘密キーと URL を利用する必要があります｡ファイルの先頭にある imports の下にこれを追加します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-129">We are going to call the Face API so we need to use those secret keys and urls we generated before, add this to the top of the file under the imports:</span></span>

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## <a name="call-the-face-api-with-the-provided-image-and-get-a-response"></a><span data-ttu-id="5dcdd-130">指定画像で Face API を呼び出し、応答を取得します。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-130">Call the Face API with the provided image and get a response</span></span>

<span data-ttu-id="5dcdd-131">Face API にリクエストを行うには､`getFaces` 関数の先頭にこのコードを追加します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-131">To make a reques to the Face API we add this code to the top of the `getFaces` function</span></span>

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

<span data-ttu-id="5dcdd-132">上記のコードは `fetch` コマンドを使用して Face API に`POST`要求を送信します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-132">The code above uses the `fetch` command to send a `POST` request to the Face API.</span></span>

<span data-ttu-id="5dcdd-133">ヘッダーで `API_KEY` を渡します｡それにより、Face API は私たちの要求を認識し､そうでない場合は､拒否します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-133">We pass the `API_KEY` in the header so the Face API knows the request comes from us, otherwise the request is rejected.</span></span>

<span data-ttu-id="5dcdd-134">本体で Face API に分析させたい `imageUrl` を渡します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-134">We pass the `imageUrl` we want the Face API to analyse in the body.</span></span>

<span data-ttu-id="5dcdd-135">API 要求から応答があり､印刷します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-135">We then get the responce from the API request and print it out.</span></span>

<span data-ttu-id="5dcdd-136">スクリプトを実行すると､</span><span class="sxs-lookup"><span data-stu-id="5dcdd-136">If you now run the script with</span></span>

```bash
node bin/mojify.js <path-to-image>
```

<span data-ttu-id="5dcdd-137">Face API に画像を渡すことで返された JSON 応答が印刷されます｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-137">It should print out the json responce returned from passing that image to the face API.</span></span>

## <a name="parse-the-responce"></a><span data-ttu-id="5dcdd-138">応答を解析する</span><span class="sxs-lookup"><span data-stu-id="5dcdd-138">Parse the responce</span></span>

<span data-ttu-id="5dcdd-139">絵文字を求めるには､API からの応答で返されたそれぞれの顔を `Face` クラスの 1 つのインスタンスに変換する必要があります。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-139">To calculate the emojis ee need to convert each face returned in the responce from the API to an instance of a `Face` class.</span></span>

<span data-ttu-id="5dcdd-140">`getFaces` 関数で API を呼び出すコードの直後に次のコードを追加します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-140">We add this code just after the code to call the API in the `getFaces` fucntion:</span></span>

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

- <span data-ttu-id="5dcdd-141">応答で返される顔を順にループします。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-141">We loop through each face returned in the responce.</span></span>
- <span data-ttu-id="5dcdd-142">返された JSON から `EmotivePoint` と `Rect`､ `Face` を生成します｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-142">We generate an `EmotivePoint`, a `Rect` and a `Face` from the returned json.</span></span>
- <span data-ttu-id="5dcdd-143">`Face` インスタンスを作成すると､その顔が絵文字に対応づけられます｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-143">Creating the `Face` instance matches the face to an emoji</span></span>
- <span data-ttu-id="5dcdd-144">対応づけられた絵文字を確認するには､`mojiicon` を印刷します。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-144">To see which emoji was matched we print out the `mojiicon`.</span></span>

## <a name="try-it-out"></a><span data-ttu-id="5dcdd-145">試してみる</span><span class="sxs-lookup"><span data-stu-id="5dcdd-145">Try it out</span></span>

<span data-ttu-id="5dcdd-146">スクリプトを実行すると､以下のことが起こります｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-146">Now if you run the script it should:</span></span>

- <span data-ttu-id="5dcdd-147">指定した画像が Face API を使って渡され、感情が計算されます｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-147">Pass the provided image through the Face API and calculate the emotion.</span></span>
- <span data-ttu-id="5dcdd-148">感情が絵文字に対応づけられます｡</span><span class="sxs-lookup"><span data-stu-id="5dcdd-148">Match emotions to emojis.</span></span>
- <span data-ttu-id="5dcdd-149">端末に絵文字が印刷されます。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-149">Print the emojis to the terminal.</span></span>

<span data-ttu-id="5dcdd-150">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="5dcdd-150">Like so:</span></span>

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
