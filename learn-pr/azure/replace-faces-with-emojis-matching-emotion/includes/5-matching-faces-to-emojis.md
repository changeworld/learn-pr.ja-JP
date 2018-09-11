In the last chapter we learnt that `shared/mojis.ts` file has a list of emojis and their emotional points.

In this chapter we will learn about the rest of the code that we will use to map a face to an emoji.

You will learn how to:

1. Create a script where you pass in a URL of an image of a face.
2. Calculate the emotional point of any faces in the image.
3. Map the faces in the image to the closest emojis

Eventually we will be implementing this functionally as a Slack command, but for now we are going to start with a simple node script that you can run on the comand line, like so:

```bash
node bin/mojify.js <url-of-image-with-face>
```

## Debugging TypeScript

We are writing in TypeScript but executing JavaScript. This makes debugging hard as we would have to set breakpoints and debug in the transpiled JavaScript files which can be hard to read.

What we ideally want is to write _and_ debug in TypeScript.

The good news is that it's possible with vs code with just a little bit of configuration.

Open up the `launch.json` file by using the command paletee `Ctrl+P > Debug: Open launch.json`

> TODO: Image

Add in a configuration option like so:

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

Now in the debug menu you will see an option called `Mojify` this will run the mojify script passing in a URL as the argument. You will be able to set breakpoints in the TypeScript file and inspect variables directly from there.

## Open up `bin/mojis.ts`

The file is already scaffolded with all the required imports, like so:

```typescript
require("dotenv").config();
import fetch from "node-fetch";
import { EmotivePoint, Face, Rect } from "../shared/models";
```

`dotenv` is a helper package which loads up the contents of a .env file in the root of your project as environment variables, useful for development.

`node-fetch` we use to make http requests to the Azure Face API.

`EmotivePoint` is a helper class that describes a point in _emotional space_ - we will be discussing this in more details below.

`Rect` is a helper class to describe a rectangle shape, we use this to describe the position of a face in an image.

`Face` is a helper utility class which combines the rectangle and emotive point informatoin about a face in an image.

In the middle of the file you should see some stub functions, like so:

```typescript
async function getFaces(imageUrl) {
  //TODO
}

async function createMojifiedImage(imageUrl, faces) {
  //TODO
}
```

These are the functions you will be fleshing out in this lecture and the next

At the end of the file you should see this code

```typescript
async function main() {
  const [imageUrl] = process.argv.slice(2);
  console.log(imageUrl);
  const faces = await getFaces(imageUrl);
  await createMojifiedImage(imageUrl, faces);
}
main();
```

## Add the environment variables

We are going to call the Face API so we need to use those secret keys and urls we generated before, add this to the top of the file under the imports:

```typescript
const API_URL = process.env["FACE_API_URL"];
const API_KEY = process.env["FACE_API_KEY"];
```

## Call the Face API with the provided image and get a response

To make a reques to the Face API we add this code to the top of the `getFaces` function

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

The code above uses the `fetch` command to send a `POST` request to the Face API.

We pass the `API_KEY` in the header so the Face API knows the request comes from us, otherwise the request is rejected.

We pass the `imageUrl` we want the Face API to analyse in the body.

We then get the responce from the API request and print it out.

If you now run the script with

```bash
node bin/mojify.js <path-to-image>
```

It should print out the json responce returned from passing that image to the face API.

## Parse the responce

To calculate the emojis ee need to convert each face returned in the responce from the API to an instance of a `Face` class.

We add this code just after the code to call the API in the `getFaces` fucntion:

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

- We loop through each face returned in the responce.
- We generate an `EmotivePoint`, a `Rect` and a `Face` from the returned json.
- Creating the `Face` instance matches the face to an emoji
- To see which emoji was matched we print out the `mojiicon`.

## Try it out

Now if you run the script it should:

- Pass the provided image through the Face API and calculate the emotion.
- Match emotions to emojis.
- Print the emojis to the terminal.

Like so:

```bash
node bin/mojify.js <path-to-image>
<path-to-image>
😕
```
