<span data-ttu-id="b83c8-101">アプリケーションのフローのこの時点で、次のことが分かっています。</span><span class="sxs-lookup"><span data-stu-id="b83c8-101">At this point in the flow of the application we know:</span></span>

1.  <span data-ttu-id="b83c8-102">画像内の顔のリスト (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="b83c8-102">The list of faces in the image (if any).</span></span>
2.  <span data-ttu-id="b83c8-103">それぞれの顔に使用する絵文字。</span><span class="sxs-lookup"><span data-stu-id="b83c8-103">The emoji to use for each face.</span></span>
3.  <span data-ttu-id="b83c8-104">画像内のそれぞれの顔に外接する長方形。</span><span class="sxs-lookup"><span data-stu-id="b83c8-104">The bounding rectangle of each face in the image.</span></span>

<span data-ttu-id="b83c8-105">したがって、画像内で検出された顔ごとに、絵文字を顔に重ね、顔に合わせて絵文字のサイズを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b83c8-105">So for each face discovered in the image, we need to layer an emoji over the face, resizing the emoji to fit the face.</span></span>

<span data-ttu-id="b83c8-106">この機能を実装するために、オープン ソースの画像操作ライブラリ [Jimp](https://www.npmjs.com/package/jimp) を使用します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-106">To implement this functionality, we will use the open source image manipulation library [Jimp](https://www.npmjs.com/package/jimp).</span></span>

<span data-ttu-id="b83c8-107">この講義の目標は、`Jimp` ライブラリを使用して画像を操作する方法を学ぶこと、具体的には、絵文字を顔に重ねて、その画像をディスクに保存する方法を学ぶことです。</span><span class="sxs-lookup"><span data-stu-id="b83c8-107">The goal of this lecture is to learn how to use the `Jimp` library to manipulate images and specifically to learn how to layer an emoji over a face and save that image back out to disk.</span></span>

<span data-ttu-id="b83c8-108">前の講義で出発点にした `bin/mojify.ts` スクリプトを、`createMojifiedImage` 関数を具体化することによって拡張します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-108">We are going to expand on the `bin/mojify.ts` script we started in the previous lecture by fleshing out the `createMojifiedImage` function.</span></span>

> <span data-ttu-id="b83c8-109">注: 後の講義で Slack コマンドと Azure 関数を作成するときに、このスクリプトのすべてのコードを再利用します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-109">NOTE We will be re-using all the code from this script when we create the Slack command and Azure Function in later lectures.</span></span> <span data-ttu-id="b83c8-110">無駄な努力ではありません!</span><span class="sxs-lookup"><span data-stu-id="b83c8-110">It's not wasted effort!</span></span>

## <a name="steps"></a><span data-ttu-id="b83c8-111">手順</span><span class="sxs-lookup"><span data-stu-id="b83c8-111">Steps</span></span>

### <a name="required-imports"></a><span data-ttu-id="b83c8-112">必要なインポート</span><span class="sxs-lookup"><span data-stu-id="b83c8-112">Required Imports</span></span>

<span data-ttu-id="b83c8-113">Jimp 使用してファイル システム上のファイルを操作するために、まず、いくつかのパッケージをインポートする必要があります。</span><span class="sxs-lookup"><span data-stu-id="b83c8-113">To play around with Jimp and manipulate files on our filesystem we need to import a few packages at the top</span></span>

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> <span data-ttu-id="b83c8-114">注: `path` は、ディスクからファイルを読み込むために必要です。</span><span class="sxs-lookup"><span data-stu-id="b83c8-114">NOTE `path` is needed because we want to load files from disk</span></span>

### <a name="basic-use-case"></a><span data-ttu-id="b83c8-115">基本的なユース ケース</span><span class="sxs-lookup"><span data-stu-id="b83c8-115">Basic Use Case</span></span>

<span data-ttu-id="b83c8-116">複雑さをできるだけ省いて、具体例を見てみましょう。</span><span class="sxs-lookup"><span data-stu-id="b83c8-116">Let's strip away a lot of the complexity and ask what it would take to:</span></span>

1. <span data-ttu-id="b83c8-117">画像の読み込み</span><span class="sxs-lookup"><span data-stu-id="b83c8-117">Load up an image</span></span>
2. <span data-ttu-id="b83c8-118">😕 の絵文字を右上隅に配置します (50x50 ピクセルにサイズを変更します)</span><span class="sxs-lookup"><span data-stu-id="b83c8-118">Place the 😕 emoji in the top right corner (resized to 50x50px)</span></span>
3. <span data-ttu-id="b83c8-119">画像の保存</span><span class="sxs-lookup"><span data-stu-id="b83c8-119">Save the image</span></span>

<span data-ttu-id="b83c8-120">次のように、上記すべての機能を 6 行のコードで実装できます。</span><span class="sxs-lookup"><span data-stu-id="b83c8-120">We can implement all the functionality above in 6 lines of code, like so:</span></span>

```typescript
async function createMojifiedImage(imageUrl) {
  let sourceImage = await Jimp.read(imageUrl);

  let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
  let emojiImage = await Jimp.read(mojiPath);

  emojiImage.resize(50, 50);

  sourceImage = sourceImage.composite(emojiImage, 0, 0);
  sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
}
```

<span data-ttu-id="b83c8-121">順を追って説明します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-121">We'll break it down step by step.</span></span>

<span data-ttu-id="b83c8-122">`Jimp` を使用して画像を読み込むには、次のように `Jimp.read` 関数を使用します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-122">To load an image using `Jimp` we use the `Jimp.read` function, like so:</span></span>

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

<span data-ttu-id="b83c8-123">各絵文字の png ファイルのディレクトリが `shared/emojis` にあります。</span><span class="sxs-lookup"><span data-stu-id="b83c8-123">We have a directory of png files for each emoji in `shared/emojis`.</span></span> <span data-ttu-id="b83c8-124">各絵文字の png の名前は <emoji>.png なので、`😕.png` は、😕 絵文字の png が含まれるを含むファイルです。</span><span class="sxs-lookup"><span data-stu-id="b83c8-124">Each emoji png is named as <emoji>.png, so `😕.png` is a file that contains a png of the 😕 emoji.</span></span>

<span data-ttu-id="b83c8-125">次のように `😕.png` を読み込みます。</span><span class="sxs-lookup"><span data-stu-id="b83c8-125">We load up `😕.png` like so:</span></span>

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

<span data-ttu-id="b83c8-126">次に、emojiImage のサイズを幅 50 ピクセル、高さ 50 ピクセルに変更する必要があり、これは resize 関数を使用して実行できます。</span><span class="sxs-lookup"><span data-stu-id="b83c8-126">Next up we need to resize the emojiImage to 50 pixels width x 50 pixels height, we can do that by using the resize function like so:</span></span>

```typescript
emojiImage.resize(50, 50);
```

<span data-ttu-id="b83c8-127">50x50 ピクセルの空間に収まるよう、`emojiImage` のサイズが変更されました。</span><span class="sxs-lookup"><span data-stu-id="b83c8-127">The `emojiImage` has now been resized to fit in a 50x50 px space.</span></span>

<span data-ttu-id="b83c8-128">ここで、左上隅の sourceImage の上に emojiImage を_配置_する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b83c8-128">We now need to _place_ the emojiImage over the sourceImage in the top left corner, like so:</span></span>

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

<span data-ttu-id="b83c8-129">`composite` 関数を使用します。これは、`sourceImage` の上から 0 ピクセル、下から 0 ピクセルの位置に `emojiImage` を配置します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-129">We use the `composite` function, which places `emojiImage` ontop of `sourceImage` 0 pixels from the top and 0 pixels down.</span></span> <span data-ttu-id="b83c8-130">`composite` 関数は `sourceImage` の位置を変えない代わりに、`emojiImage` がその上に配置された `sourceImage` のコピーを返します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-130">The `composite` fucntion doesn't alter `sourceImage` in place, instead it returns a copy of `sourceImage` with the `emojiImage` placed on top.</span></span>

<span data-ttu-id="b83c8-131">最後に、次のようにして出力画像をディスクに保存します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-131">Finally we save the output image to disk like so:</span></span>

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

### <a name="full-use-case"></a><span data-ttu-id="b83c8-132">完全なユース ケース</span><span class="sxs-lookup"><span data-stu-id="b83c8-132">Full Use Case</span></span>

<span data-ttu-id="b83c8-133">`Jimp` がどのように動作するか、また、これを使用して画像を合成する方法はよく理解できましたでしょうか。</span><span class="sxs-lookup"><span data-stu-id="b83c8-133">Hopefully by now you have a good understanding of how `Jimp` works and how we can use it to composite images.</span></span> <span data-ttu-id="b83c8-134">その理解を前提に、`createMojifiedImage` 関数のコード全体を検証することには大きな意義があります。</span><span class="sxs-lookup"><span data-stu-id="b83c8-134">So now when we go through the full code for the `createMojifiedImage` function it should make a lot more sense.</span></span>

<span data-ttu-id="b83c8-135">次のコードをコピーして、`bin/mojify.ts` 内の `createMojifiedImage` 関数に貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="b83c8-135">Copy and paste the bellow code into your `createMojifiedImage` function in `bin/mojify.ts`.</span></span>

```typescript
async function createMojifiedImage(imageUrl) {
  // Create a composite image, we will "append" to this composite an emoji image for each face found
  let compositeImage = sourceImage;

  for (let face of faces) {
    const mojiIcon = face.mojiIcon;
    const faceHeight = face.faceRectangle.height;
    const faceWidth = face.faceRectangle.width;
    const faceTop = face.faceRectangle.top;
    const faceLeft = face.faceRectangle.left;

    // Load the emoji from disk
    let mojiPath = path.resolve(
      __dirname,
      "../shared/emojis/" + mojiIcon + ".png"
    );
    let emojiImage = await Jimp.read(mojiPath);

    // Resize the emoji to fit the face
    emojiImage.resize(faceWidth, faceHeight);

    // Compose the emoji on the image
    compositeImage = compositeImage.composite(emojiImage, faceLeft, faceTop);
  }
  compositeImage.write(path.join(__dirname, "..", "mojified.jpg"));
}
```

<span data-ttu-id="b83c8-136">このコードは先に説明した基本ケースとよく似ていますが、絵文字をハードコードして配置するのではなく、どの絵文字を合成してどこにそれを配置するかを、渡された顔の配列に基づいて決定します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-136">The above code is very similar to the base case we just went through, rather than hardcoding an emoji and poisition however we are deciding which emoji to composite and where to place it based on the array of faces passed in.</span></span>

<span data-ttu-id="b83c8-137">顔の配列は、最後の講義で具体化した `getFaces` 関数からのものであり、次のように、すべて main 関数にまとめられています。</span><span class="sxs-lookup"><span data-stu-id="b83c8-137">The array of faces comes from the `getFaces` function we fleshed out in the last lecture, it's all connected up together in the main function, like so:</span></span>

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

<span data-ttu-id="b83c8-138">`imageUrl` を渡して `getFaces` を呼び出すことで、`Face` インスタンスの配列を取得します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-138">We call `getFaces` with the passed in `imageUrl` to get the array of `Face` instances.</span></span>
<span data-ttu-id="b83c8-139">この配列を元の画像と一緒に `createMojifiedImage` 関数に渡すと、この関数は人の顔の上に絵文字を合成し、結果のファイルを `mojified.jpg` という名前でプロジェクトのルート フォルダーに保存します。</span><span class="sxs-lookup"><span data-stu-id="b83c8-139">We pass this array to the `createMojifiedImage` function along with the original image, this function composites emojis on peoples faces and saves the resulting file to the project root folder as `mojified.jpg`</span></span>

### <a name="try-it-out"></a><span data-ttu-id="b83c8-140">試してみる</span><span class="sxs-lookup"><span data-stu-id="b83c8-140">Try it out</span></span>

<span data-ttu-id="b83c8-141">次のように、このコードを自分自身で試してみてください。</span><span class="sxs-lookup"><span data-stu-id="b83c8-141">Try this code out yourself, like so:</span></span>

```bash
node bin/mojify.js <url>
```

<span data-ttu-id="b83c8-142">成功すれば、ソース ファイルの絵文字バージョンが `mojified.jpg` という名前でプロジェクトのルートに保存されているはずです。</span><span class="sxs-lookup"><span data-stu-id="b83c8-142">If this worked then a mojified version of the source fil should be stored in the project root called `mojified.jpg`.</span></span>

<span data-ttu-id="b83c8-143">別の画像で試してみてください!</span><span class="sxs-lookup"><span data-stu-id="b83c8-143">Try it out with different images!</span></span>
