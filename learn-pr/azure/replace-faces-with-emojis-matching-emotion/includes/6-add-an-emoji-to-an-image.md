At this point in the flow of the application we know:

1.  The list of faces in the image (if any).
2.  The emoji to use for each face.
3.  The bounding rectangle of each face in the image.

So for each face discovered in the image, we need to layer an emoji over the face, resizing the emoji to fit the face.

To implement this functionality, we will use the open source image manipulation library [Jimp](https://www.npmjs.com/package/jimp).

The goal of this lecture is to learn how to use the `Jimp` library to manipulate images and specifically to learn how to layer an emoji over a face and save that image back out to disk.

We are going to expand on the `bin/mojify.ts` script we started in the previous lecture by fleshing out the `createMojifiedImage` function.

> NOTE
> We will be re-using all the code from this script when we create the Slack command and Azure Function in later lectures. It's not wasted effort!

## Steps

### Required Imports

To play around with Jimp and manipulate files on our filesystem we need to import a few packages at the top

```typescript
import * as Jimp from "jimp";
import * as path from "path";
```

> NOTE
> `path` is needed because we want to load files from disk

### Basic Use Case

Let's strip away a lot of the complexity and ask what it would take to:

1. Load up an image
2. Place the 😕 emoji in the top right corner (resized to 50x50px)
3. Save the image

We can implement all the functionality above in 6 lines of code, like so:

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

We'll break it down step by step.

To load an image using `Jimp` we use the `Jimp.read` function, like so:

```typescript
let sourceImage = await Jimp.read(imageUrl);
```

We have a directory of png files for each emoji in `shared/emojis`. Each emoji png is named as <emoji>.png, so `😕.png` is a file that contains a png of the 😕 emoji.

We load up `😕.png` like so:

```typescript
let mojiPath = path.resolve(__dirname, "../shared/emojis/😕.png");
let emojiImage = await Jimp.read(mojiPath);
```

Next up we need to resize the emojiImage to 50 pixels width x 50 pixels height, we can do that by using the resize function like so:

```typescript
emojiImage.resize(50, 50);
```

The `emojiImage` has now been resized to fit in a 50x50 px space.

We now need to _place_ the emojiImage over the sourceImage in the top left corner, like so:

```typescript
sourceImage = sourceImage.composite(emojiImage, 0, 0);
```

We use the `composite` function, which places `emojiImage` ontop of `sourceImage` 0 pixels from the top and 0 pixels down. The `composite` fucntion doesn't alter `sourceImage` in place, instead it returns a copy of `sourceImage` with the `emojiImage` placed on top.

Finally we save the output image to disk like so:

```typescript
sourceImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

### Full Use Case

Hopefully by now you have a good understanding of how `Jimp` works and how we can use it to composite images. So now when we go through the full code for the `createMojifiedImage` function it should make a lot more sense.

Copy and paste the bellow code into your `createMojifiedImage` function in `bin/mojify.ts`.

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

The above code is very similar to the base case we just went through, rather than hardcoding an emoji and poisition however we are deciding which emoji to composite and where to place it based on the array of faces passed in.

The array of faces comes from the `getFaces` function we fleshed out in the last lecture, it's all connected up together in the main function, like so:

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

We call `getFaces` with the passed in `imageUrl` to get the array of `Face` instances.
We pass this array to the `createMojifiedImage` function along with the original image, this function composites emojis on peoples faces and saves the resulting file to the project root folder as `mojified.jpg`

### Try it out

Try this code out yourself, like so:

```bash
node bin/mojify.js <url>
```

If this worked then a mojified version of the source fil should be stored in the project root called `mojified.jpg`.

Try it out with different images!
