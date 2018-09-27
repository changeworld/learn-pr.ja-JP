元のシナリオである CRM ソフトウェアをテストする VM の作成を思い出してください。 新しいビルドを使用できるようになったので、クリーンなイメージからインストール エクスペリエンス全体をテストできるように、新しい VM を起動してみたいと思います。 それが完了したら、VM を削除します。

VM の作成に使用するコマンドを試してみましょう。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-linux-vm-with-azure-powershell"></a>Azure PowerShell を使用して Linux VM を作成する

Azure サンドボックスを使用しているので、リソース グループを作成する必要はありません。 代わりに、**<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループを使用します。 また、場所の制限についても注意してください。

PowerShell を使用して新しい Azure VM を作成してみましょう。

1. `New-AzureRmVm` コマンドレットを使用して VM を作成します。
    - **<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループを使用します。
    - VM に名前を付けます。通常、VM の目的、場所、(複数ある場合は) インスタンス番号を識別できる何か意味のある名前を付けます。 ここでは、"米国東部内のテスト VM、インスタンス 1" を表す "testvm-eus-01" を使用します。 VM の配置場所に基づいて独自の名前を付けてください。
    - Azure サンドボックスで使用できる次のリストから、自分に近い場所を選択します。 コピーと貼り付けを使用する場合は、次のコマンド例の値を必ず変更してください。

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - イメージには "UbuntuLTS" を使用します (これは Ubuntu Linux です)。
    - `Get-Credential` コマンドレットを使用し、結果を `Credential` パラメーターに指定します。
    - `-OpenPorts` パラメーターを追加し、ポートとして "22" を渡します。これで、SSH でマシンに接続できるようになります。
 
    ```powershell
    New-AzureRmVm -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -Name "testvm-eus-01" -Credential (Get-Credential) -Location "East US" -Image UbuntuLTS -OpenPorts 22
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]
    
1. これが完了するまでに数分かかります。 完了したら、クエリを実行し、変数 (`$vm`) に VM オブジェクトを割り当てることができます。

    ```powershell
    $vm = Get-AzureRmVM -Name "testvm-eus-01" -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```
    
1. 次に、値のクエリを実行し、VM に関する情報をダンプします。

    ```powershell
    $vm
    ```

    次のような結果が表示されます。

    ```output
    ResourceGroupName : <rgn>[sandbox resource group name]</rgn>
    Id                : /subscriptions/xxxxxxxx-xxxx-aaaa-bbbb-cccccccccccc/resourceGroups/<rgn>[sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/testvm-eus-01
    VmId              : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    Name              : testvm-eus-01
    Type              : Microsoft.Compute/virtualMachines
    Location          : eastus
    Tags              : {}
    HardwareProfile   : {VmSize}
    NetworkProfile    : {NetworkInterfaces}
    OSProfile         : {ComputerName, AdminUsername, LinuxConfiguration, Secrets}
    ProvisioningState : Succeeded
    StorageProfile    : {ImageReference, OsDisk, DataDisks}
    ```
    
1. 複合オブジェクトを指定するには、ドット (".") 構文を使用します。 たとえば、HardwareProfile セクションに関連付けられた `VMSize` オブジェクトのプロパティを表示するには、次のように入力します。

    ```powershell
    $vm.HardwareProfile
    ```

1. また、ディスクのいずれかに関する情報を取得するには、次のように入力します。

    ```powershell
    $vm.StorageProfile.OsDisk
    ```

1. VM オブジェクトを他のコマンドレットに渡すこともできます。 VM のパブリック IP アドレスを取得する例を次に示します。

    ```powershell
    $vm | Get-AzureRmPublicIpAddress
    ```

1. IP アドレスを指定し、SSH を使用して VM に接続することができます。 たとえば、ユーザー名 "bob" を使用し、IP アドレスが "205.22.16.5" の場合、このコマンドで Linux マシンに接続されます。

    ```powershell
    ssh bob@205.22.16.5
    ```

    「`exit`」と入力してログアウトします。


## <a name="delete-a-vm"></a>VM を削除する

その他のコマンドを試すために、VM を削除してみましょう。 まずシャットダウンします。

```powershell
Stop-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

次に `Remove-AzureRmVM` コマンドレットを使用して VM を削除します。

```powershell
Remove-AzureRmVM -Name $vm.Name -ResourceGroup $vm.ResourceGroupName
```

このコマンドで、リソース グループ内のすべてのリソースを一覧表示してみましょう。

```powershell
Get-AzureRmResource -ResourceGroupName $vm.ResourceGroupName | ft
```

まだ存在するすべてのリソース (ディスク、仮想ネットワークなど) が一覧表示されます。 

```output
Microsoft.Compute/disks
Microsoft.Network/networkInterfaces
Microsoft.Network/networkSecurityGroups
Microsoft.Network/publicIPAddresses
Microsoft.Network/virtualNetworks
```

これは、`Remove-AzureRmVM` コマンドで削除されるのは _VM のみ_のためです。 他のリソースはまったくクリーンアップされません。 この時点では、リソース グループ自体を削除するだけで完了です。 手動でクリーンアップするには、この演習全体を実行してください。 コマンドのパターンがわかるはずです。

1. ネットワーク インターフェイスを削除します。

    ```powershell
    $vm | Remove-AzureRmNetworkInterface –Force
    ```
    
1. マネージド OS ディスクとストレージ アカウントを削除します。

    ```powershell
    Get-AzureRmDisk -ResourceGroupName $vm.ResourceGroupName -DiskName $vm.StorageProfile.OSDisk.Name | Remove-AzureRmDisk -Force
    ```

1. 次に仮想ネットワークを削除します。

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmVirtualNetwork -Force
    ```

1. ネットワーク セキュリティ グループを削除します。

    ```powershell
    Get-AzureRmNetworkSecurityGroup -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmNetworkSecurityGroup -Force
    ```

1. 最後に、パブリック IP アドレスです。

    ```powershell
    Get-AzureRmPublicIpAddress -ResourceGroup $vm.ResourceGroupName | Remove-AzureRmPublicIpAddress -Force
    ```

リソース グループを見て、作成したすべてのリソースに対応したことを確認します。 ここでは多数の手動コマンドを実行しましたが、後でこのロジックを再利用して VM を作成または削除できるように、_スクリプト_を作成することをお勧めします。 PowerShell を使用したスクリプトを見てみましょう。