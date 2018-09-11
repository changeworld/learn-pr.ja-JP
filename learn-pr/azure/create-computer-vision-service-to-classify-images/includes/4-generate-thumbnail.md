In this unit, you will generate thumbnails from a source image with the Computer Vision API service that we created previously.

# Generate a thumbnail from an image with Computer Vision API

Execute the `az cognitiveservices account keys list` command to retrieve a key used to authenticate against the API. Store the output of that command within the `key` variable.

```output
key=$(az cognitiveservices account keys list -g ComputerVisionRG --name ComputerVisionService --query key1 -o tsv)
```

Execute a `curl` command to do an HTTP request against the Computer Vision API and reuse the previously declared variable `key`.

Different parameters can be provided to the API to generate the proper thumbnail for your needs. `width` and `height` are required and will tell the API which size you need for a specific image. Finally, the `smartCropping` parameter generates smarter cropping by analyzing the region of interest in your image to keep it within the thumbnail. As an example, with smart cropping enabled, a cropped profile picture would keep someone's face within the picture frame even when the picture isn't in the same ratio as the one that we asked.

```bash
curl -H "Ocp-Apim-Subscription-Key: $key" -H "Content-Type: application/json" "https://westus2.api.cognitive.microsoft.com/vision/v1.0/generateThumbnail?width=100&height=100&smartCropping=true" -d "{\"url\":\"https://docs.microsoft.com/en-us/learn/modules/create-computer-vision-service/media/mountains.jpg\"}" -o clouddrive/thumbnail.jpg
```

# Downloading the thumbnail

The generated thumbnail will be found in your Azure Cloud Shell storage account within a resource group named `cloud-shell-storage-<region>`.

1. Get into the automatically generated storage account.

    ![Screenshot of the generated storage account](../media/4-storage-account.png)

2. Click on the files section.

    ![Screenshot of the storage account with the files section circled](../media/4-storage-account-click-on-files.png)

3. You will find the thumbnail at the root of the container.

    ![Screenshot of the storage account with the thumbnail circled](../media/4-storage-account-thumbnail.png)

4. Click on the file, and then download it.

From within your download folder, you can open the `100x100`-pixels image with any image viewer.