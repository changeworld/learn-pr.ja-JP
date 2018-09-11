You have learned the basics of creating and connecting to an Azure storage account. You wrote a simple application to connect to a storage account and create a blob container. Other modules in this Learning Path will build on that knowledge to show you how to use Blob Storage and Queues to store data and connect apps together.

We only used examples of JavaScript and C#, but Azure supports a variety of other languages. Check the official [SDK Tools documentation page](https://docs.microsoft.com/azure/#pivot=sdkstools) to find the official list of links to the client libraries for all the currently supported languages.

## Additional resources

- [Azure Storage Services REST API Reference](https://docs.microsoft.com/rest/api/storageservices/)
- [Using shared access signatures to provide limited access to a storage account](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Manage secrets with Azure Key Vault](https://docs.microsoft.com/learn/modules/manage-secrets-with-azure-key-vault/)
- [Using Azure Key Vault Storage Account Keys with server apps](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys)
- [Source code for the .NET Azure SDKs](https://github.com/Azure/azure-sdk-for-net)
- [Azure Storage Client Library for JavaScript](https://github.com/Azure/azure-storage-node#azure-storage-javascript-client-library-for-browsers)

## Clean up
<!---TODO: Update for sandbox?--->

It's a good idea to cleanup your resources in Azure when you are finished with them. If you are using the free sandbox, you don't have to do this step - but in your own subscriptions you'll want to delete unused resources to avoid unnecessary charges. The easiest way to do this is to delete the Resource Group you placed all the resources into. This can be done through the Azure portal, or on the command line.