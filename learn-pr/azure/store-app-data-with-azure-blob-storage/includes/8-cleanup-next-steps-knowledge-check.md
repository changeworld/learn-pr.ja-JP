<span data-ttu-id="24e4d-101">このモジュールでは、Azure Blob Storage を使用して Web アプリケーションのデータを格納する方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="24e4d-101">In this module, you learned how to use Azure Blob storage to store web application data.</span></span> <span data-ttu-id="24e4d-102">Web アプリで BLOB ストレージを使用するための戦略を作成するためのヒント、および Azure Storage SDK for .NET Core を使用して BLOB に書き込みまたは BLOB から読み取る方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="24e4d-102">We discussed tips for creating a strategy to use Blob storage in a web app and how to use the Azure Storage SDK for .NET Core to write to and read from blobs.</span></span> <span data-ttu-id="24e4d-103">作成したアプリが、ユーザーからのファイルのアップロードを受け取り、それを BLOB ストレージに格納し、ダウンロードできるようにします。</span><span class="sxs-lookup"><span data-stu-id="24e4d-103">The app we made accepts uploaded files from users, stores them in Blob storage, and makes them available for download.</span></span>

[!include[](../../../includes/azure-sandbox-cleanup.md)]

<span data-ttu-id="24e4d-104">Cloud Shell ストレージをクリーンアップするには、`mslearn-store-data-in-azure` ディレクトリを削除します。</span><span class="sxs-lookup"><span data-stu-id="24e4d-104">To clean up your Cloud Shell storage, delete the `mslearn-store-data-in-azure` directory.</span></span>

<!---TODO: Remove further reading
## Further reading

- **Securely storing secrets like connection strings**: The most robust end-to-end solution for storing secret configuration values is Azure Key Vault. See [here](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x) for information about using Key Vault in an ASP.NET Core application. Alternatively, you can safely store connection strings in App Service application settings and use the [ASP.NET Core Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows) to support developer environments.
- [Uploading large files with streaming in ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?view=aspnetcore-2.1#uploading-large-files-with-streaming)
- [Blob concurrency: AccessConditions and blob leases](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)
- [Granting limited access to Azure Storage object with shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Indexing Blob storage with Azure Search](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage)
- [Container and blob name restrictions](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#resource-names)
--->