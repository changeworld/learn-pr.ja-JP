<span data-ttu-id="6d34d-101">仮想マシンを実行する際に行う主なタスクの 1 つが、仮想マシンの起動と停止です。</span><span class="sxs-lookup"><span data-stu-id="6d34d-101">One of the main tasks you'll want to do while running virtual machines is to start and stop them.</span></span>

## <a name="stopping-a-vm"></a><span data-ttu-id="6d34d-102">VM の停止</span><span class="sxs-lookup"><span data-stu-id="6d34d-102">Stopping a VM</span></span>

<span data-ttu-id="6d34d-103">実行中の VM を停止するには、`vm stop` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6d34d-103">We can stop a running VM with the `vm stop` command.</span></span> <span data-ttu-id="6d34d-104">VM の名前とリソース グループ、または一意の ID を渡す必要があります。</span><span class="sxs-lookup"><span data-stu-id="6d34d-104">You must pass the name and resource group, or the unique ID for the VM:</span></span>

```azurecli
az vm stop -n SampleVM -g <rgn>[sandbox resource group name]</rgn>
```

<span data-ttu-id="6d34d-105">停止したことを確認するには、`ssh` を使用して、または `vm get-instance-view` コマンドを使用して、パブリック IP アドレスに ping を試行します。</span><span class="sxs-lookup"><span data-stu-id="6d34d-105">We can verify it has stopped by attempting to ping the public IP address, using `ssh`, or through the `vm get-instance-view` command.</span></span> <span data-ttu-id="6d34d-106">この最後の方法では、`vm show` と同じ基本的なデータが返されますが、インスタンス自体の詳細が含まれます。</span><span class="sxs-lookup"><span data-stu-id="6d34d-106">This final approach returns the same basic data as `vm show` but includes details about the instance itself.</span></span> <span data-ttu-id="6d34d-107">Azure Cloud Shell に次のコマンドを入力して、VM の現在の実行状態を確認してみましょう。</span><span class="sxs-lookup"><span data-stu-id="6d34d-107">Try typing the following command into Azure Cloud Shell to see the current running state of your VM:</span></span>

```azurecli
az vm get-instance-view -n SampleVM -g <rgn>[sandbox resource group name]</rgn> --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

<span data-ttu-id="6d34d-108">このコマンドでは、結果として `VM stopped` が返されるはずです。</span><span class="sxs-lookup"><span data-stu-id="6d34d-108">This command should return `VM stopped` as the result.</span></span>

## <a name="starting-a-vm"></a><span data-ttu-id="6d34d-109">VM の起動</span><span class="sxs-lookup"><span data-stu-id="6d34d-109">Starting a VM</span></span>

<span data-ttu-id="6d34d-110">`vm start` コマンドを使用して逆のことができます。</span><span class="sxs-lookup"><span data-stu-id="6d34d-110">We can do the reverse through the `vm start` command.</span></span>

```azurecli
az vm start -n SampleVM -g <rgn>[sandbox resource group name]</rgn>
```

<span data-ttu-id="6d34d-111">このコマンドは、停止した VM を起動します。</span><span class="sxs-lookup"><span data-stu-id="6d34d-111">This command will start a stopped VM.</span></span> <span data-ttu-id="6d34d-112">これを確認するには、`vm get-instance-view` クエリを使用します。これにより `VM running` が返されるはずです。</span><span class="sxs-lookup"><span data-stu-id="6d34d-112">We can verify it through the `vm get-instance-view` query, which should now return `VM running`.</span></span>

## <a name="restarting-a-vm"></a><span data-ttu-id="6d34d-113">VM の再起動</span><span class="sxs-lookup"><span data-stu-id="6d34d-113">Restarting a VM</span></span>

<span data-ttu-id="6d34d-114">最後に、VM に再起動が必要な変更を行った場合は、`vm restart` コマンドを使用して再起動することができます。</span><span class="sxs-lookup"><span data-stu-id="6d34d-114">Finally, we can restart a VM if we have made changes that require a reboot using the `vm restart` command.</span></span> <span data-ttu-id="6d34d-115">VM の再起動を待たずに Azure CLI をすぐに復帰させる場合は、`--no-wait` フラグを追加します。</span><span class="sxs-lookup"><span data-stu-id="6d34d-115">You can add the `--no-wait` flag if you want the Azure CLI to return immediately without waiting for the VM to reboot.</span></span>

