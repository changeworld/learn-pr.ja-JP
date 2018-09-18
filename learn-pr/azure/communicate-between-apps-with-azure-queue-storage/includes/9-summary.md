<span data-ttu-id="23fbf-101">このモジュールでは、Azure ストレージ アカウント内のキューを使用して、分散アプリケーション内のコンポーネント間でメッセージの受け渡しをする方法について説明しました。</span><span class="sxs-lookup"><span data-stu-id="23fbf-101">In this module, you've seen how queues in Azure storage accounts are used to pass messages between components in a distributed application.</span></span> <span data-ttu-id="23fbf-102">このような方法でキューを使用することは、分散アプリケーションの信頼性を向上させ、障害や需要が高い期間に対する回復力を高めることに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="23fbf-102">Using queues in this way can help to make a distributed application more reliable and resilient to failures and periods of high demand.</span></span> <span data-ttu-id="23fbf-103">.NET 用 Microsoft Azure Storage クライアント ライブラリを使用すれば、キューの作成、メッセージの追加、またはキューからのメッセージの取得および削除を行うための C# または VB.NET コードを容易に記述することができます。</span><span class="sxs-lookup"><span data-stu-id="23fbf-103">If you use the Microsoft Azure Storage Client Library for .NET, you can easily write C# or VB.NET code that creates queues, adds messages, or retrieves and removes messages from queues.</span></span>

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

<span data-ttu-id="23fbf-104">独自のサブスクリプションで作業している場合は、次の Azure CLI コマンドを実行して、リソース グループと、すべての関連付けられたリソースを削除することができます。</span><span class="sxs-lookup"><span data-stu-id="23fbf-104">When you are working in your own subscription, you can execute the following Azure CLI command to delete the resource group and all associated resources.</span></span>

```azurecli
az group delete --name [resource-group-name] --yes --no-wait
```

<span data-ttu-id="23fbf-105">省略可能な `--no-wait` オプションは、Azure が操作を完了するまで待機しないようシェルに指示します。</span><span class="sxs-lookup"><span data-stu-id="23fbf-105">The optional `--no-wait` option tells the shell to not wait for Azure to complete the operation.</span></span>