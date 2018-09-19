## <a name="get-an-access-key"></a>アクセス キーを取得する

まだ行っていない場合は、Azure Cloud Shell で次のコマンドを実行し、`key` という名前の変数に API アクセス キーを格納します。 後の呼び出しでこの変数を使用します。

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[Sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
