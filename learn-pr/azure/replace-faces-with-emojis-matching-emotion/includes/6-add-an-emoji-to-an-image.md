この時点で、アプリケーションのフローでわかっています。

1.  (該当する場合) は、イメージ内の顔のリスト。
2.  顔ごとに使用する絵文字。
3.  イメージ内の顔ごとの外接する四角形。

イメージ内で検出された顔ごとに、必要があります、表面上、絵文字をレイヤーに顔を合わせて絵文字のサイズを変更します。

オープン ソースの画像操作ライブラリを使用してこの機能を実装する[Jimp](https://www.npmjs.com/package/jimp)します。

この講義の目標を使用する方法を理解する、`Jimp`面の上に絵文字をレイヤーし、そのイメージを保存する方法を具体的には、イメージを操作するライブラリ ディスクにバックアップします。

さらにここでは、`bin/mojify.ts`肉づけ前の講義でまずスクリプト、`createMojifiedImage`関数。

> **注**
>
> このスクリプトからのすべてのコードは、以降の講義で Slack コマンドと Azure 関数を作成したとき再利用します。 労力を浪費するがしません。

## <a name="add-the-required-imports"></a>必要なインポートを追加します。

ファイルの上部にある、いくつかのパッケージをインポートする Jimp、いろいろをシステム上のファイルの操作は、次のようにします。

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> **注**
>
> `path` ディスクからファイルをロードするために必要

## <a name="simplified-use-case"></a>簡略化されたユース ケース

複雑さの多くを取り除き、だけにかかるよう依頼としましょう。

1. イメージを読み込む
2. 場所、😕右上隅 (50x50px にサイズ変更) での絵文字
3. イメージを保存します。

6 行のコードで上記のすべての機能を実装します次のようにします。

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

中断が発生することステップ バイ ステップ。

使用してをイメージを読み込む`Jimp`使用してを`Jimp.read`関数の場合、次のように。

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

各絵文字の png ファイルのディレクトリがある`shared/emojis`します。 各絵文字 png という<emoji>.png のため、`😕.png`の png を含むファイル、😕絵文字。

読み込んでいます`😕.png`ようになります。

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

サイズを変更する必要がありますを次に、 `emojiImage` 50 ピクセル幅 x 50 ピクセルの高さにそのためにサイズ変更関数を使用して次のようにします。

```typescript
emojiImage.resize(50, 50);
```

`emojiImage` 50 x 50 ピクセルの領域に収まるようにサイズ変更されたようになりました。

今すぐ必要があります_配置_、`emojiImage`経由で、`sourceImage`左上隅で次のようにします。

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

使用して、`composite`関数で、配置`emojiImage`の上に`sourceImage`、最後の 2 つの引数は、場所を定義、`emojiImage`は、配置配置される上および 0 ピクセルから 0 ピクセル ダウンします。

`composite`関数は変更されません`sourceImage`インプレース; 代わりにコピーを返しますの`sourceImage`で、`emojiImage`上に配置される、このため、結果を代入 sourceImage に戻る `sourceImage = ...`

最後に、出力の画像をディスクに保存しました次のようにします。

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

## <a name="try-it-out"></a>試してみる

自分では、このコードを実行してください。 次のようにします。

```bash
node bin/mojify.js [url-to-image]
```

このプロジェクトのルートにファイルを保存し、呼び出された場合`mojified.jpg`ような形式。

![単純なイメージ](/media-drafts/6.simple-mojified-image.jpg)

## <a name="full-use-case"></a>完全なユース ケース

うまくいけば、これがある方法をよく理解`Jimp`works と複合イメージを使用しましたか。 完全なコードを進めていくときに今すぐ、`createMojifiedImage`関数が、はるかに多くの意味をする必要があります。

コピーして貼り付け、下記のコードを`createMojifiedImage`関数`bin/mojify.ts`します。

```typescript
async function createMojifiedImage(imageUrl) {
  let sourceImage = await Jimp.read(imageUrl);
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

上記のコードを調べていく簡略化された場合によく似ています、ハードコーディング、絵文字、位置ではなくただし、どの絵文字を合成するかを決定しましたおよびに渡される顔の配列に基づいてそれを配置する場所。

顔の配列に由来、`getFaces`最後の講義で肉付け関数はすべてをメインの関数で相互に接続されたようになります。

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

呼び出して`getFaces`で渡されたで`imageUrl`の配列を取得する`Face`インスタンス。
この配列を渡して、`createMojifiedImage`元の画像では、この関数の合成絵文字 peoples でと共に関数 faces し、としてプロジェクトのルート フォルダーに生成されたファイルを保存します。 `mojified.jpg`

## <a name="try-it-out"></a>試してみる

自分では、このコードを実行してください。 次のようにします。

```bash
node bin/mojify.js <url>
```

この代替 mojified バージョンのソース ファイルがプロジェクト ルートに保存し、呼び出された場合`mojified.jpg`、次のようにします。

![複雑なイメージ](/media-drafts/6.complex-mojified-image.jpg)

試して、さまざまなイメージを!
