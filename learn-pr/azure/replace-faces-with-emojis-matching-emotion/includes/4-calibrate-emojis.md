We can’t pass an image of the emoji to the Face API to get it’s emotion because, well because it’s not human. So for each emoji, I needed a human proxy, me.

I took pictures of myself _accurately_ mimicking each emoji, and used the _emotional point_ for that image as the proxy for the emoji. To keep things interesting I also chose people from my team and associated them with emojis as well, like so:

![Team Moji](/media-drafts/team.jpg)

For the emoji of love eyes (😍) I chose a picture of my wife ❤️. In memory of [Stephen Hawking](https://en.wikipedia.org/wiki/Stephen_Hawking) I picked a picture of him to represent 🤔.

You can see the list of proxy images for each emoji in the `bin/proxy-images` folder in the sample code associated with this tutorial.

In this chapter you will generate a key so you can use the Azure Face API and then use the Face API to calibrate all the emojies using proxied images of me.

## Generate an Azure Face API Key

<!-- To make calls to the Azure Face API we will need a special authorization key.

We are going to create one using the `az` CLI. -->

To use the Azure Face API we need a special authentication key, head over to https://azure.microsoft.com/try/cognitive-services/ and signup to trial the Face API.

![Team Moji](/media-drafts/4.calibrating-emojis.get-face-api.png)

> TODO: Find az commands to create faceAPI and grab keys

<!-- > NOTE the Azure Face API doesn't return the emotion information by default, we need to switch on this behavior by setting some query parameters, like so:
> https://westeurope.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion -->

## Setup the environment variables

The calibration script needs to know your Face API URL and Key in order to make the correct calls, rather than hardcoding these in the script we are going to use environment varialbes, run these commands in the terminal you expect to run the application in:

```bash
FACE_API_URL=<the-face-api-url>
FACE_API_KEY=<your-face-api-key>
```

<!-- > NOTE
> Don't forget to add the query param returnFaceAttributes=emotion to ensure the Face API returns emotion as well -->

## Create some proxy images for emojis

I've provided all the proxy images myself, but feel free to generate your own!

For each emoji in the `bin/proxy-images` folder, take a picture of yourself mimicking that emoji and replace the image with your image.

## Try it out

Now comes the fun part! We are going to run each of the images in the `bin/proxy-images` through the Face API to calculate an emotional point for that emoji in _emotional space_, run:

```bash
node bin/calibrate.js
```

The output of this command should look something like so:

```json
...
Processing 🤓
Processing 🤔
Processing 🦄
Processing 😃
Processing 😆
...
{
    emotiveValues: new EmotivePoint({
        anger: 0,
        contempt: 0.005,
        disgust: 0.001,
        fear: 0,
        happiness: 0,
        neutral: 0.922,
        sadness: 0.071,
        surprise: 0
    }),
    emojiIcon: "😴"
}
```

It will first print out the emoji's it is processing and then finally print out to the console an array which defines the `EmotivePoint` of all the emoji's. This is the same format as the array in `shared/mojis.ts`.

If you changed some of the proxied images then copy the output of this script to the relevant section of `mojis.ts`
