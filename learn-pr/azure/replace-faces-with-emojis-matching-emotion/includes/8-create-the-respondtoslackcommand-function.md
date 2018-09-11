We've created the `MojifyImage` Azure Function which returns a mojified image, we need a second endpoint which slack will call everytime someone executes the `\mojify` command.

This second endpoint needs to return the URL to the `MojifyImage` Azure Function.

When you run a slack command like `\mojify [some-image-url]` it will make a POST request to the endpoint you have configured and pass in `[some-image-url]` as a `text` query param embedded in the body of the message.

The goal of this lecture is to create this function that coordinates and responds to the slack commands, we will call this second Azure function `RespondToSlackCommand`.

## Create the Function Trigger and convert to TypeScript

Samilar to the previous lecture we need to create a function trigger, we will use vs code to do this again.

> TODO: Add images

1. Using the command palette create a function trigger

- Choose JavaScript as the languge.
- Choose V2 as the engine.
- Choose HttpTrigger
- Give it a name of `RespondToSlackCommand`

If it completed successfully then a folder called `RespondToSlackCommand` should be present in the root directory. Same as the last lecture let's now convert this from JavaScript into TypeScript.

2. Create a file called `index.ts` in the `RespondToSlackCommand` folder.

- Ensure that the typescript build process is still working and it automatically created an `index.js` and `index.map` files.

3. Paste this code in `index.ts`

```typescript
export async function index(context, req) {
  context.log("RespondToSlackCommand HTTP trigger");
  context.res.body = "Hello!";
  context.done();
}
```

## Try it out

Ensure that everything is working by visiting http://localhost:7171/api/RespondToSlackCommand in a browser, it should print our `Hello!`.

## Flesh out the `index` function

This `index` function will be a lot quicker to write than the previous function.

Let's first setup our `context.res` object

```typescript
context.res = {
  headers: {
    "Content-Type": "application/json"
  },
  body: null
};
```

Simpler than before, we don't set the `isRaw` property, defualts to `false`, but we do set the content type to be `application/json`.

As mentioned earlier on slack will send the url we want to process as a query string with key of `text` but for security reasons will embed this in the body of the request so we need to work a little harder to get the information we need, not too hard though!

To make our lives easier let's import the node `querystring` package, this comes as part of nodejs so no need to install anything extra.

```typescript
import * as querystring from "querystring";
```

Then in our `index` function we can convert the `req.body` into an object from which we will extract the `text` property, like so:

```typescript
const { text } = querystring.parse(req.body);
```

If the user typed the command properly then text should contain the url of an image, we can add in some rudimentally validation like so:

```typescript
let message = "Your mojified image my liege...";
if (!text) {
  message = "You must provide an image to mojify";
}
```

Remember the slack command is calling https://somedomain.com/api/RespondToSlackCommand and we will need to respond with the MojifyImage url which will most likly be on the same domain, https://somedomain.com/api/MojifyImage

Rather than hardcoding the domain in the file, let's instead use the same domain as the request as the domain of te MojifyImage url, like so:

```typescript
const mojifyUrl = req.url.substr(0, req.url.lastIndexOf("/")) + "/MojifyImage";
```

Finally let's set the body of the responce, the slack command expects a specific format in the responce, like so:

```typescript
context.res.body = {
  response_type: "in_channel",
  text: message,
  attachments: [
    {
      image_url: `${mojifyUrl}?imageUrl=${text}`
    }
  ]
};

context.done();
```

The key thing to note above is the `image_url` in the `attachments` property, we set this to the return the `mojifyUrl` passing in the url the user supplied in the command as the `imageUrl` query param.
