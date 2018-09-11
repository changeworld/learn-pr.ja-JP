Let's combine all the knowledge we've gained over the past few lectures to create a web app which will return a mojified image.

We want to create a web endpoint like so:

```
http://example.com/MojifyImage?imageUrl=[url-of-image]
```

If we visit this page in a browser it will display a mojified version of the passed in `imageUrl`.

The technology we are going to use to create a web app like the above is Azure Functions.

The goal of this lecture is to create an Azure Function which behaves like the above.

## Create Function App and Trigger

There are multiple ways to create an Azure Function, we are going to use the Visual Studio Code Azure Extension.

> TODO: Add images

1. Open up the starter project using vs code.
2. Open up the Azure Extension.
3. Open up the Functions viewlet.
4. If you need to sign in to Azure, once it's working you should see a list of your subscriptions.
5. Click the button to create a function app.
6. Once that's created click the button to create a function trigger.

- Choose JavaScript as the languge.
- Choose V2 as the engine.
- Choose HttpTrigger
- Give it a name of `MojifyImage`

Once these commands complete successfully you will have converted the starter project to a function app with a _http trigger_ function called `MojifyImage`.

To ensure everythig is working try debugging the application by either pressing `F5` or by selecting the debug menu and running the `Attach to JavaScript Function` debug command.

You should see the terminal appear and the function app started locally, it will print out a URL you can visit which looks something like http://localhost:7171/api/MojifyImage

If everything worked out fine you shoul dbe able to visit that URL and see a simple responce being returned in the browser, like so:

> TODO IMAGE

## Convert to TypeScript

We are coding in TypeScript but the function app created a JavaScript file.

1. Rename the `index.js` file to `index.ts`

- If you are running `npm run build` and the related `tsc -w` command then the `index.ts` file will be instantly compiled to `index.js` and you should also have an `index.map` file created.

2. Paste this code in `index.ts`

```typescript
export async function index(context, req) {
  context.log("ReplyWithMojifiedImage HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

To make sure everything is working, ensure the function app is running by using the debug menu, and then visit http://localhost:7171/api/MojifyImage in a browser. If everything is fine then this should print out `Hello!` to the browser.

You now have converted your function trigger to be TypeScript instead of JavaScript 👏.

## Copy existing code from the `mojify.ts` script

The work we spent in previous lectures creating our `bin\mojify.ts` code wasn't wasted, most of that code can now be copied into this function.

Copy everything except the `main` function over to the `index.ts` file, it doesn't matter where but I like to have all the imports dependant functions at the top of the file and the `index` function at the bottom.

The `getFaces` function remains completely the same.

The function does need one change, in the original version of this function were were saving the image to disk with a line like this at the end of the code

```typescript
compositeImage.write(path.join(__dirname, "..", "mojified.jpg"));
```

In the new web app version we don't want to save the file to disk, we want to return the image to the user and display it in the browser window instead.

To do that we need something calld a `buffer`, we can ask `Jimp` to give us the buffer for our image but we need to wrap the code in a `Promise` because the `Jimp.getBuffer` function is asynchronous.

So replace the above `compositeImage.write` code with this:

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

This returns the buffer of the image, the raw binary data.

## Conect it all together in the `index` function

The index function is the one that gets called when we visit this endpoint. When someone visits this endpoint we want:

1. Get the imageUrl they are requesting
2. Mojify the imageUrl and get the raw buffer.
3. Respond to the HTTP request in such a way that an image is displayed in the browser.

So let's start fleshing out the index function.

The `context.res` property defines the responce of this function, what we set here is what gets returned to the browser.

Let's configure `context.res` like so:

```typescript
context.res = {
  status: 200,
  headers: {
    "Content-Type": "image/jpeg"
  },
  isRaw: true
};
```

Add the above to the top of your function, this sets the responce to return a status code of 200, sets the return content type to be a jpeg and sets the `isRaw` flag to true to so we can return binary _image_ data.

Next we want to extract the `imageUrl` from the query params so we know which image we should be mojifying. We also want to return some nice error message if the user doesn't supply it.

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

Finally we want to actually call the `getFaces` and `createMojifiedImage` functions we added to the top of our file.

```typescript
// Get the responce from the faceAPI
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

The important two lines above are:

```typescript
context.res.body = buffer;
context.done();
```

This sets the raw binary data as the body to our responce object and then we call context.done() so the function app knows we are done and it can perform the task of returning the result.

The full listing for the index.ts function is like so:

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

  // Get the responce from the faceAPI
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

## Try it out

Ensure the function app is running by starting it from the debug menu or running it from the terminal like so:

```bash
func host start
```

If the function app started correctly then the output window should show something like this:

```
Http Functions:
        MojifyImage: http://localhost:7071/api/MojifyImage
```

If the function app started correctly then visit the URL rememberung to pass in the URL to an image via the `imageUrl` query param, like so:

http://localhost:7171/api/MojifyImage?imageUrl=[url-to-an-image]

If all is working then it should return a mojified version of the image like so:

![Mojified Image](/media-drafts/7.mojified-image.png)
