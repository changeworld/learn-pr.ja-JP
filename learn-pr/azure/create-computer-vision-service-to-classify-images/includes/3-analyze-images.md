In this unit, you will analyze images with the Computer Vision API service that we created in the previous step.

# Analyzing an image with Computer Vision API

Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API. Store the output of that command within the `key` variable.

```azurecli
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.

The parameters that are sent to the service are `visualFeatures`, `details`, and `languages`. While without them, the service will still work, it provides hints to the service on the content of the images. As an example, `details` can be set to `Landmarks` or `Celebrities` to help you identify landmarks or celebrities. `visualFeatures` identify what kind of information you want the service to return you. The `Categories` option will categorize the content of the images like trees, buildings, and more. `Faces` will identify people's faces and give you their gender and age.

More details can be found in [the documentation](https://westus.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fa).

For now, let's identify landmarks and have the service return us `Categories` and `Description`.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/media/mountains.jpg\"}"
```

The result of the request is the raw JSON describing the picture in the `url`. We can also add ` | jq '.'` at the end to prettify the JSON output.

```azurecli
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/analyze?visualFeatures=Categories,Description&details=Landmarks&language=en" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/media/mountains.jpg\"}" | jq '.'
```

Copy the link to any other images found online, and try it with the service by replacing the URL in the above commands.