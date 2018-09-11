Before we delve too deep into the implementations let’s first answer some higher level questions.

## How to calculate the emotion of a face?

Calculating emotion is one of the easiest parts of the application. We use the [FaceAPI](https://azure.microsoft.com/services/cognitive-services/face/?WT.mc_id=mojifier-sandbox-ashussai), part of the Azure Cognitive Services offering.

The FaceAPI takes as input an image and returns information about the image, including if it detected any faces, the locations of the faces in the image and if requested it will also calculate and return the emotions of the faces as well, like so:

```json
{
  "anger": 0.572,
  "contempt": 0.025,
  "disgust": 0.242,
  "fear": 0.001,
  "happiness": 0.014,
  "neutral": 0.111,
  "sadness": 0.033,
  "surprise": 0.003
}
```

Take for instance this image:

![Example Face](/media-drafts/example-face.jpg)

To process this image, you would make a POST request to an API endpoint like this:

    https://<region>.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=false&returnFaceLandmarks=false&returnFaceAttributes=emotion

We provide the image in the body like so:

```json
{
  "url": "<path-to-image>"
}
```

> **Note**
>
> The API by default doesn’t return the emotion, you need to explicitly specify the query param `returnFaceAttributes=emotion`

The API is authenticated by the use of a secret key; we need to send this key with the header

    Ocp-Apim-Subscription-Key: <your-subscription-key>

The API with the query params above would return a JSON like so:

```json
[
  {
    "faceRectangle": {
      "top": 207,
      "left": 198,
      "width": 229,
      "height": 229
    },
    "faceAttributes": {
      "emotion": {
        "anger": 0.001,
        "contempt": 0.014,
        "disgust": 0,
        "fear": 0,
        "happiness": 0.306,
        "neutral": 0.675,
        "sadness": 0.003,
        "surprise": 0.001
      }
    }
  }
]
```

It returns an array of results, one per face detected in the image. For each face, it returns the size/location of the face as `faceRectangle` and the emotions represented as a number from 0 to 1 as `faceAttributes`.

> **Tip**
>
> You can start playing around with Cognitive Services even _without_ having an Azure account, simply go to [this page](https://azure.microsoft.com/try/cognitive-services/?api=face-api&WT.mc_id=mojifier-sandbox-ashussai) and enter your email address to get trial access.

## How to map an emotion to an emoji?

Imagine there were only two emotions, fear and happiness, with values ranging from 0 to 1. Then every face could be plotted in a 2D _emotional space_ based on the emotion of the user, like so:

![Euclidean Distance 1](/media-drafts/graph-1.jpg)

Imagine then that we also figured out the emotional point for each emoji, and plotted those on the 2D emotional space as well. Then if we calculate the distance between your face and all the other emojis in this 2D emotional space we can figure out the closest emoji to your emotion, like so:

![Euclidean Distance 2](/media-drafts/graph-2.png)

This calculation is called the `euclidian distance`, and this is precisely what we used but not in 2D emotional space, in an 8D emotional space with (anger, contempt, disgust, fear, happiness, neutral, sadness, surprise).

> **Tip**
>
> To make like easier we used the npm package called euclidean-distance, <https://www.npmjs.com/package/euclidean-distance>.

## Shared Code

The sample starter project comes with a shared folder with code that already handles a lot of the use cases above.

### EmotivePoint

If you look closely at the `EmotivePoint` class in `shared/emmotive-point.ts` you will notice a few things

The contructor takes as input an object containing emotive information and stores as local member variables, like so:

```typescript
 constructor({
    anger,
    contempt,
    disgust,
    fear,
    happiness,
    neutral,
    sadness,
    surprise
  }) {
    this.anger = anger;
    this.contempt = contempt;
    this.disgust = disgust;
    this.fear = fear;
    this.happiness = happiness;
    this.neutral = neutral;
    this.sadness = sadness;
    this.surprise = surprise;
  }
```

It also has a function called distance which we can use to calcualte the euclidian distance between two emotive points, like so:

```typescript
  distance(other) {
    let myPoint = this.toArray();
    let otherPoint = other.toArray();
    return distance(myPoint, otherPoint);
  }
```

We therefore can create two emotive points and calculate how close they are like so:

```typescript
let a = new EmotivePoint({
  /* ... */
});
let b = new EmotivePoint({
  /* ... */
});
let distance = a.distance(b);
```

### Face

Another helper class is the `Face` class, this combines a few different properties including the `EmotivePoint` of a face and also the rectangle defining the face in the image, we use the `Rect` class for that.

If you look closely at the `Face` class constructor in `shared/face.ts` you will notice this line of code:

```typescript
this.moji = this.chooseMoji(this.emotivePoint);
```

`emotivePoint` is the emotive point of the face itself.
`chooseMoji` returns an appropriate emoji based on the emotivePoint of the face.

```typescript
  chooseMoji(point) {
    let closestMoji = null;
    let closestDistance = Number.MAX_VALUE;
    for (let moji of MOJIS) {
      let emoPoint = moji.emotiveValues;
      let distance = emoPoint.distance(point);
      if (distance < closestDistance) {
        closestMoji = moji;
        closestDistance = distance;
      }
    }
    return closestMoji;
  }
```

`MOJIS` is the list of emotive points for all the emojis, we'll discuss how to generate these in the next lecture.

The `chooseMoji` fucntion simply caclualtes the distance between this face and all the emojies, returning the closest one.

# Summary

For each emoji we calcualte a point in _emotional_ space, this is called _calibration_ and we will cover this in the next chapter.

Then using the Azure Face API we get a list of faces in an images with the emotional point of each face. Then using the euclidian distance algorithm to find the closest emoji to each face.
