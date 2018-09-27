最もわかりやすいタスクから始めましょう: Azure 仮想マシンの作成。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="logins-subscriptions-and-resource-groups"></a>ログイン、サブスクリプション、リソース グループ

この後の作業には右側の Azure Cloud Shell を使用します。 サンドボックスをアクティブにした後、Microsoft Learn によって管理されている無料のサブスクリプションで Azure にログインします。 自分で Azure にログインしたり、サブスクリプションを選択したりする必要はありません。これらは自動的に行われます。 さらに、通常であれば、新しいリソースを保持するための "_リソース グループ_" を作成します。 このモジュールでは、すべてのコマンドの実行に使用されるリソース グループが Azure サンドボックスによって自動的に作成されます。

## <a name="create-a-linux-vm-with-the-azure-cli"></a>Azure CLI を使用して Linux VM を作成する

Azure CLI には、Azure 内の仮想マシンを操作するための `vm` コマンドが含まれます。 特定のタスクを実行するためのサブコマンドがいくつかあります。 最も一般的なものを次に示します。

| サブコマンド | 説明 |
|-------------|-------------|
| `create`    | 新しい仮想マシンを作成します |
| `deallocate` | 仮想マシンの割り当てを解除します |
| `delete` | 仮想マシンの削除 |
| `list` | サブスクリプションに作成されている仮想マシンの一覧を表示します |
| `open-port` | 受信トラフィック用に特定のネットワーク ポートを開きます |
| `restart` | 仮想マシンの再起動 |
| `show` | 仮想マシンについての詳細を取得します |
| `start` | 停止している仮想マシンを起動します |
| `stop` | 実行している仮想マシンを停止します |
| `update` | 仮想マシンのプロパティを更新します |

> [!NOTE]
> コマンドの完全な一覧については、[Azure CLI のリファレンス ドキュメント](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)をご覧ください。

最初の `az vm create` から始めましょう。 このコマンドは、リソース グループ内に仮想マシンを作成するために使用します。 複数のパラメーターを渡して、新しい VM のすべての面を構成することができます。 次の 3 つのパラメーターを必ず指定する必要があります。

> [!div class="mx-tableFixed"]
> | パラメーター | 説明 |
> |-----------|-------------|
> | `resource-group` | 仮想マシンを所有するリソース グループです。**<rgn>[サンドボックス リソース グループ]</rgn>** を使用します。 |
> | `name` | 仮想マシンの名前であり、リソース グループ内で一意である必要があります。 |
> | `image` | VM の作成に使用するオペレーティング システム イメージです。 |
> | `location` | VM を配置するリージョンです。 通常は、VM のコンシューマーの近くになります。 この演習では、次の一覧から近くの場所を選択します。 |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

さらに、`--verbose` フラグを追加して VM の作成の進行状況を表示すると便利です。 

## <a name="create-a-linux-virtual-machine"></a>Linux 仮想マシンの作成

新しい Linux 仮想マシンを作成してみます。 "米国西部" の場所に Debian Linux マシンを作成するには、Azure Cloud Shell で次のコマンドを実行します。 米国西部が近くではない場合は、場所を変更してください。

```azurecli
az vm create --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --location westus --verbose 
```

[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


このコマンドでは、新しい **Debian** Linux 仮想マシンが `SampleVM` という名前で作成されます。 VM が作成されている間、Azure CLI ツールは待っていることに注意してください。 `--no-wait` オプションを追加することで、Azure CLI ツールに対して、すぐに戻り、VM の作成を Azure にバックグラウンドで続けさせるように指示できます。 これは、スクリプトでコマンドを実行している場合に便利です。 後でスクリプトの中で `azure vm wait --name [vm-name]` コマンドを使用して、VM の作成が完了するのを待機します。

詳細な応答を見ると、VM のさまざまな依存関係の名前に `SampleVM` という名前が使用されていることもわかります。

```output
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

これらの自動生成されるリソースの名前は、省略可能なパラメーター `vm create` を使用してオーバーライドできます (`--vnet-name` や `--public-ip-address-dns-name` など)。

`admin-username` フラグを使用して管理者アカウント名を **"aldis"** と指定しています。 これを省略した場合、`vm create` コマンドは "_現在のユーザー名_" を使用します。 アカウントの命名規則は OS によって異なるため、特定の名前を指定する方が安全です。 

> [!NOTE]
> "root" や "admin" などの一般的な名前は、ほとんどのイメージで使用できません。

ここでは、`generate-ssh-keys` フラグも使用しています。 このパラメーターは Linux ディストリビューションに対して使用され、`ssh` ツールを使用して仮想マシンにリモート アクセスできるように、セキュリティ キーのペアが作成されます。 コンピューター上と VM 内の `.ssh` フォルダーに、2 つのファイルが配置されます。 ターゲット フォルダーに `id_rsa` という名前の SSH キーが既にある場合はそれが使用され、新しいキーは生成されません。

VM の作成が終了すると、JSON 応答を受け取ります。それには、仮想マシンの現在の状態と、Azure によって割り当てられたパブリックおよびプライベート IP アドレスが含まれます。

```json
{
  "fqdns": "",
  "id": "/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "2568d0d0-efe3-4d04-a08f-df7f009f822a",
  "zones": ""
}
```
