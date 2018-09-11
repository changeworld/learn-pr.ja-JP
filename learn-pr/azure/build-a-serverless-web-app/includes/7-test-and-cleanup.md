You've successfully created a full-featured, serverless application using Azure services.

## Clean up
<!---TODO: Update for sandbox--->

When you are done working with this application, you can use the following command to delete all resources created during the tutorial:

```azurecli
az group delete --name first-serverless-app
```

Type `y` when prompted.  

## Next steps

In this module, you learned how to:
  - Configure Azure Blob storage to host a static website and uploaded images.
  - Upload images to Azure Blob storage using Azure Functions.
  - Resize images using Azure Functions.
  - Store image metadata in Azure Cosmos DB. 
  - Use the Cognitive Services Computer Vision API to auto-generate image captions.
  - Use Azure Active Directory to secure the web app by authenticating users.

To learn how to connect even more services to Functions, continue to the [Functions with Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email) tutorial.