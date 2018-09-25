## <a name="get-an-access-key"></a><span data-ttu-id="6276a-101">アクセス キーを取得する</span><span class="sxs-lookup"><span data-stu-id="6276a-101">Get an access key</span></span>

<span data-ttu-id="6276a-102">まだ行っていない場合は、Azure Cloud Shell で次のコマンドを実行し、`key` という名前の変数に API アクセス キーを格納します。</span><span class="sxs-lookup"><span data-stu-id="6276a-102">If you haven't done so already, run the following command in Azure Cloud Shell to store the API access key in a variable called `key`.</span></span> <span data-ttu-id="6276a-103">後の呼び出しでこの変数を使用します。</span><span class="sxs-lookup"><span data-stu-id="6276a-103">We'll use this variable in subsequent calls.</span></span>

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
