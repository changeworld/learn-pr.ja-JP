このモジュールでは、Azure Blob Storage を使用して Web アプリケーションのデータを格納する方法について説明しました。 Web アプリで BLOB ストレージを使用するための戦略を作成するためのヒント、および Azure Storage SDK for .NET Core を使用して BLOB に書き込みまたは BLOB から読み取る方法について説明しました。 作成したアプリが、ユーザーからのファイルのアップロードを受け取り、それを BLOB ストレージに格納し、ダウンロードできるようにします。

## <a name="cleanup"></a>クリーンアップ

Azure サブスクリプションをクリーンアップするには、Azure Cloud Shell で次を実行して、このモジュールで作成したすべてのリソースを含むリソース グループを削除します。

```console
az group delete --name blob-exercise-group
```

Cloud Shell ストレージをクリーンアップするには、`rm -rf TODO` を使用して `TODO` ディレクトリを削除します。

## <a name="additional-resources"></a>その他のリソース

**TODO cross-docset リンク?**

* **ストレージ アカウント キーを安全に格納する**: シークレット構成値を格納するための最も堅牢なエンドツーエンド ソリューションは、Azure Key Vault です。 ASP.NET Core アプリケーションでの Key Vault の使用に関する詳細は、[こちら](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x)をご覧ください。 または、App Service アプリケーション設定で接続文字列を安全に格納して、[ASP.NET Core Secret Manager ツール](https://docs.microsoft.com/aspnet/core/security/app-secrets?view=aspnetcore-2.1&tabs=windows)を使用して、開発者環境をサポートすることもできます。
* [ASP.NET Core でのストリーミングによる大きいファイルのアップロード](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads?view=aspnetcore-2.1#uploading-large-files-with-streaming)
* [BLOB の同時実行: AccessConditions と BLOB のリース](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/)
* [Shared Access Signature を使用して、Azure Storage オブジェクトへの制限付きアクセスを許可する](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
* [Azure Search を使用した BLOB ストレージのインデックス作成](https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage)
* [コンテナーと BLOB 名の制限](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata#resource-names)