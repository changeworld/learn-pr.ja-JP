Azure CLI を使用して新しい Linux 仮想マシンを作成し、そのサイズを変更し、停止して起動し、構成を更新しました。

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

自分のサブスクリプションを操作する場合は、リソース グループと関連付けられているすべてのリソースを削除するのには、次の Azure CLI コマンドを実行できます。

```azurecli
az group delete --name [resource-group-name] --no-wait
```

プロンプトが表示されたら、"yes" と答えるか、`--yes` パラメーターを使用してプロンプトに自動応答します。

このコマンドは、リソース グループに関連付けられているすべてのリソースを削除し、必ず正しい順序でその割り当てを解除します。 `--no-wait` パラメーターは、削除の実行中に Azure CLI がブロックされないようにします。 リソースが削除されるまで待機するため、オフにしておきます。 または、スクリプトの後半で `az group wait -n [resource-group-name] --deleted` コマンドを使用して、削除が完了するまで待機します。


## <a name="further-reading"></a>参考資料

- [Azure CLI の概要](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Azure CLI コマンド リファレンス](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
