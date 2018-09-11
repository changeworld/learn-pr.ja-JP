In this unit, you will create a Computer Vision API service using the Azure CLI.

# Exercise: Create a Computer Vision service

[Computer Vision](/azure/cognitive-services/computer-vision/home) is part of [Azure Cognitive Services](/azure/cognitive-services/welcome), which is a complete set of machine learning algorithms available as a service for anyone to use. Instead of generating enough data to train a model from scratch, we make available pre-trained models for anyone to use.

Among the many services available, we provide services that can handle voice, image, video, search, language, and more.

In this exercise, we're going to focus on creating the necessary service that will allow us to handle images. After this exercise, you will be able to create an API endpoint that will be able to identify images.

# Create a Computer Vision API service

Before you create your Computer Vision API service, you need a *resource group* to deploy it to. A resource group is a logical collection into which all Azure resources are deployed and managed.

```azurecli
az group create --name ComputerVisionRG --location westus2
```

Once you've created the resource group, create the service with the `az cognitiveservices account create` command and the name of your service. 

```azurecli
az cognitiveservices account create --kind ComputerVision -l westus2 -n ComputerVisionService --sku F0 -g ComputerVisionRG
```
