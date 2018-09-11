このモジュールでは、Azure Key Vault でアプリのシークレット構成をセキュリティ保護しました。 このアプリ コードでは、マネージド サービス ID を使用してコンテナーへの認証が行われ、起動時にコンテナーから ASP.NET Core の構成システムにシークレットが自動的に読み込まれます。

## <a name="cleanup"></a>クリーンアップ

Azure サブスクリプションをクリーンアップするには、Azure Cloud Shell で次を実行して、このモジュールで作成したすべてのリソースを含むリソース グループを削除します。

```console
az group delete --name keyvault-exercise-group
```

Cloud Shell ストレージをクリーンアップするには、`KeyVaultDemoApp` ディレクトリを削除します。

## <a name="next-steps"></a>次の手順

実際のアプリの場合、次の手順は何でしょうか。

- すべてのアプリ シークレットをコンテナーに配置することです! 構成ファイル内に入れておく理由は何もありません。
- 引き続きアプリケーションを開発します。 運用環境はすべてセットアップ済みなので、今後のデプロイですべてのセットアップを繰り返す必要はありません。
- 開発をサポートするには、名前は同じでも値の異なるシークレットを含む開発環境コンテナーを作成します。 アプリの開発環境構成ファイルで、開発チームにアクセス許可を付与し、コンテナー名を構成します。 開発者がアプリをローカルで実行すると、`AddAzureKeyVault` は Visual Studio および Azure CLI のローカル インストールを自動的に検出し、それらのアプリケーションで構成されている Azure 資格情報を使用してサインインし、コンテナーにアクセスします。
- ユーザー受け入れテストなどの目的で追加の環境を作成します。
- 異なるサブスクリプションやリソース グループ間でコンテナーを別々にして、分離します。
- 該当するユーザーに他の環境コンテナーへのアクセス権を付与します。

## <a name="further-reading"></a>参考資料

- [Key Vault のドキュメント](https://docs.microsoft.com/azure/key-vault/)
- [AddAzureKeyVault とその高度なオプションの詳細](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x)
- [このチュートリアル](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application)では、MSI の代わりにクライアント シークレットを使用して Azure Active Directory への認証を手動で行う手順を含め、`KeyVaultClient` の使用手順について説明します。
- 認証ワークフローを自分で実装するための [MSI トークン サービスのドキュメント](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol)