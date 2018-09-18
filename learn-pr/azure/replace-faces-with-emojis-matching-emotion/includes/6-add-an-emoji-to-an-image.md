アプリケーションのフローのこの時点で、次のことが分かっています。

1.  画像内の顔のリスト (存在する場合)。
2.  それぞれの顔に使用する絵文字。
3.  画像内のそれぞれの顔に外接する長方形。

したがって、画像内で検出された顔ごとに、絵文字を顔に重ね、顔に合わせて絵文字のサイズを変更する必要があります。

この機能を実装するために、オープン ソースの画像操作ライブラリ [Jimp](https://www.npmjs.com/package/jimp) を使用します。

この講義の目標は、`Jimp` ライブラリを使用して画像を操作する方法を学ぶこと、具体的には、絵文字を顔に重ねて、その画像をディスクに保存する方法を学ぶことです。

前の講義で出発点にした `bin/mojify.ts` スクリプトを、`createMojifiedImage` 関数を具体化することによって拡張します。

> 注: 後の講義で Slack コマンドと Azure 関数を作成するときに、このスクリプトのすべてのコードを再利用します。 無駄な努力ではありません!

## <a name="steps"></a>手順

### <a name="required-imports"></a>必要なインポート

Jimp 使用してファイル システム上のファイルを操作するために、まず、いくつかのパッケージをインポートする必要があります。

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> 注: `path` は、ディスクからファイルを読み込むために必要です。

### <a name="basic-use-case"></a>基本的なユース ケース

複雑さをできるだけ省いて、具体例を見てみましょう。

1. 画像の読み込み
2. 😕 の絵文字を右上隅に配置します (50x50 ピクセルにサイズを変更します)
3. 画像の保存

次のように、上記すべての機能を 6 行のコードで実装できます。

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

順を追って説明します。

`Jimp` を使用して画像を読み込むには、次のように `Jimp.read` 関数を使用します。

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

各絵文字の png ファイルのディレクトリが `shared/emojis` にあります。 各絵文字の png の名前は <emoji>.png なので、`😕.png` は、😕 絵文字の png が含まれるを含むファイルです。

次のように `😕.png` を読み込みます。

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

次に、emojiImage のサイズを幅 50 ピクセル、高さ 50 ピクセルに変更する必要があり、これは resize 関数を使用して実行できます。

```typescript
emojiImage.resize(50, 50);
```

50x50 ピクセルの空間に収まるよう、`emojiImage` のサイズが変更されました。

ここで、左上隅の sourceImage の上に emojiImage を_配置_する必要があります。

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

`composite` 関数を使用します。これは、`sourceImage` の上から 0 ピクセル、下から 0 ピクセルの位置に `emojiImage` を配置します。 `composite` 関数は `sourceImage` の位置を変えない代わりに、`emojiImage` がその上に配置された `sourceImage` のコピーを返します。

最後に、次のようにして出力画像をディスクに保存します。

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

### <a name="full-use-case"></a>完全なユース ケース

`Jimp` がどのように動作するか、また、これを使用して画像を合成する方法はよく理解できましたでしょうか。 その理解を前提に、`createMojifiedImage` 関数のコード全体を検証することには大きな意義があります。

次のコードをコピーして、`bin/mojify.ts` 内の `createMojifiedImage` 関数に貼り付けます。

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

このコードは先に説明した基本ケースとよく似ていますが、絵文字をハードコードして配置するのではなく、どの絵文字を合成してどこにそれを配置するかを、渡された顔の配列に基づいて決定します。

顔の配列は、最後の講義で具体化した `getFaces` 関数からのものであり、次のように、すべて main 関数にまとめられています。

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

`imageUrl` を渡して `getFaces` を呼び出すことで、`Face` インスタンスの配列を取得します。
この配列を元の画像と一緒に `createMojifiedImage` 関数に渡すと、この関数は人の顔の上に絵文字を合成し、結果のファイルを `mojified.jpg` という名前でプロジェクトのルート フォルダーに保存します。

### <a name="try-it-out"></a>試してみる

次のように、このコードを自分自身で試してみてください。

```bash
node bin/mojify.js <url>
```

成功すれば、ソース ファイルの絵文字バージョンが `mojified.jpg` という名前でプロジェクトのルートに保存されているはずです。

別の画像で試してみてください!
