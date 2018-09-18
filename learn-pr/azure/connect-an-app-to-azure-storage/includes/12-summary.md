Azure Storage アカウントを作成し、そのアカウントに接続する方法の基本を学習しました。 単純なアプリケーションを作成して、ストレージ アカウントに接続し、BLOB コンテナーを作成しました。 このラーニング パスの他のモジュールでは、その知識に基づいて、Blob Storage とキューを使用してデータを格納し、アプリどうしを接続する方法を示します。

ここでは JavaScript と C# の例のみを使用していますが、Azure ではさまざまな言語がサポートされます。 現在サポートされている全言語のクライアント ライブラリへの公式のリンク一覧については、公式の [SDK Tools ドキュメント ページ](https://docs.microsoft.com/azure/#pivot=sdkstools)を参照してください。

## <a name="additional-resources"></a>その他のリソース

- [Azure Storage サービスの REST API リファレンス](https://docs.microsoft.com/rest/api/storageservices/)
- [共有アクセス署名を使用してストレージ アカウントへの制限付きアクセスを提供する](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Azure Key Vault を使用してシークレットを管理する](https://docs.microsoft.com/learn/modules/manage-secrets-with-azure-key-vault/)
- [Azure Key Vault ストレージ アカウント キーをサーバー アプリで使用する](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys)
- [.NET Azure SDK のソース コード](https://github.com/Azure/azure-sdk-for-net)
- [JavaScript 用 Azure Storage クライアント ライブラリ](https://github.com/Azure/azure-storage-node#azure-storage-javascript-client-library-for-browsers)

## <a name="clean-up"></a>クリーンアップ
<!---TODO: Update for sandbox?--->

Azure のリソースが不要になったらクリーンアップすることをお勧めします。 無料のサンドボックスを使用している場合は、この手順を実行する必要はありません。ただし、保有しているサブスクリプションでは、使用していないリソースを削除して不必要な課金を回避することをお勧めします。 これを行うための最も簡単な方法は、すべてのリソースを配置したリソース グループを削除することです。 これは、Azure portal またはコマンド ラインで行うことができます。