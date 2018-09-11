In this unit, you will extract handwriting from an image with the Computer Vision API service that we created previously.

# Extracting the handwriting from an image

Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API. Store the output of that command within the `key` variable.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.

The image we're going to be using for handwriting recognition is a picture of this note board.

![Picture of a handwriting sample on a note](../media/6-handwriting.jpg)

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/recognizeText?handwriting=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/handwriting.jpg\"}" -D -
```

The command above will output the headers. Copy the `Operation-Location` header value into the command below.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" "<Operation-Location>"
```

You will now receive a JSON file containing the result of the handwriting recognition request.

Experiment with other images to get different results by changing the URL used in the sample above.