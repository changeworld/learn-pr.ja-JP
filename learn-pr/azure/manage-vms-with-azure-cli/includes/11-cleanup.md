<span data-ttu-id="6df19-101">Azure CLI を使用して新しい Linux 仮想マシンを作成し、そのサイズを変更し、停止して起動し、構成を更新しました。</span><span class="sxs-lookup"><span data-stu-id="6df19-101">You've created a new Linux virtual machine, changed its size, stopped and started it, and updated the configuration with the Azure CLI.</span></span>

## <a name="cleanup"></a><span data-ttu-id="6df19-102">クリーンアップ</span><span class="sxs-lookup"><span data-stu-id="6df19-102">Cleanup</span></span>

<span data-ttu-id="6df19-103">これで VM での作業が完了し、不要になったので、リソースを削除しましょう。</span><span class="sxs-lookup"><span data-stu-id="6df19-103">Now that we're finished with the VM and no longer need it, let's delete the resources.</span></span> <span data-ttu-id="6df19-104">クリーンアップは、アカウントに引き続き課金される可能性があるコンピューティング リソースおよびストレージ リソースにとって重要です。</span><span class="sxs-lookup"><span data-stu-id="6df19-104">Cleanup is important for compute and storage resources that can continue to be billed against your account.</span></span> 

<span data-ttu-id="6df19-105">`delete` コマンドを使用してリソースを個別に削除することもできますが、最も簡単な方法は、`az group delete` ですべてのリソースを削除することです。</span><span class="sxs-lookup"><span data-stu-id="6df19-105">You can delete individual resources with the `delete` command, but the easiest way to remove everything is with `az group delete`.</span></span> <span data-ttu-id="6df19-106">Azure CLI で次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="6df19-106">Execute the following command in the Azure CLI:</span></span>

```azurecli
az group delete --name ExerciseResources --no-wait
```

<span data-ttu-id="6df19-107">プロンプトが表示されたら、"yes" と答えるか、`--yes` パラメーターを使用してプロンプトに自動応答します。</span><span class="sxs-lookup"><span data-stu-id="6df19-107">Answer "yes" to the prompt when shown, or use the `--yes` parameter to auto-answer the prompt.</span></span>

<span data-ttu-id="6df19-108">このコマンドは、リソース グループに関連付けられているすべてのリソースを削除し、必ず正しい順序でその割り当てを解除します。</span><span class="sxs-lookup"><span data-stu-id="6df19-108">This command deletes all of the resources associated with the resource group, and is guaranteed to deallocate them in the correct order.</span></span> <span data-ttu-id="6df19-109">`--no-wait` パラメーターは、削除の実行中に Azure CLI がブロックされないようにします。</span><span class="sxs-lookup"><span data-stu-id="6df19-109">The `--no-wait` parameter keeps the Azure CLI from blocking while the deletion takes place.</span></span> <span data-ttu-id="6df19-110">リソースが削除されるまで待機するため、オフにしておきます。</span><span class="sxs-lookup"><span data-stu-id="6df19-110">Leave it off to wait for the resources to be deleted.</span></span> <span data-ttu-id="6df19-111">または、スクリプトの後半で `az group wait -n ExerciseResources --deleted` コマンドを使用して、削除が完了するまで待機します。</span><span class="sxs-lookup"><span data-stu-id="6df19-111">Or use the `az group wait -n ExerciseResources --deleted` command later in your script to wait for the deletion to finish.</span></span>


## <a name="further-reading"></a><span data-ttu-id="6df19-112">参考資料</span><span class="sxs-lookup"><span data-stu-id="6df19-112">Further reading</span></span>

- [<span data-ttu-id="6df19-113">Azure CLI の概要</span><span class="sxs-lookup"><span data-stu-id="6df19-113">Azure CLI overview</span></span>](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [<span data-ttu-id="6df19-114">Azure CLI コマンド リファレンス</span><span class="sxs-lookup"><span data-stu-id="6df19-114">Azure CLI command reference</span></span>](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
