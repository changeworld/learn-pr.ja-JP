前の演習では、Azure portal を使用して以下のタスクを実行しました。

- OS ディスクのキャッシュ状態を表示する
- OS ディスクのキャッシュ設定を変更する
- VM にデータ ディスクを追加する
- 新しいデータ ディスク上のキャッシュの種類を変更する

Azure PowerShell を使用してこれらの操作を練習してみましょう。 

> [!NOTE]
> ここでは Azure PowerShell を使用しますが、コンソール ベースのツールとして同様の機能を提供する Azure CLI を使用することもできます。 macOS、Linux、Windows で動作します。 Azure CLI について詳しく学習したい場合は、「**Azure CLI を使用して仮想マシンを管理する**」モジュールをご覧ください。

ここでは、これまでの演習で作成した VM を使用します。 このラボの操作では、以下のことを前提としています。

- VM は存在し、**fotoshareVM** という名前です
- この VM は **<rgn>[サンドボックス リソース グループ名]</rgn>** という名前のリソース グループに存在します。

これらの値は異なる名前を使用した場合は、これらの値を実際の名前で置き換えてください。

前回の演習の VM ディスクは、次のような状態です。

![OS ディスクとデータ ディスクのスクリーンショットでは、どちらも読み取り専用キャッシュに設定されています。](../media/disks-final-config-portal.PNG)

ポータルを使用して、OS ディスクとデータ ディスクの両方に **[ホスト キャッシュ]** フィールドを設定しています。 以下の手順を行う際には、この初期状態を念頭に置いてください。

### <a name="set-up-some-variables"></a>変数をいくつか設定する

まず、後で使用できるように、いくつかのリソース名を用意しておきましょう。

1. 右側の Azure Cloud Shell のターミナルを使用して、次の PowerShell コマンドを実行します。

    > [!NOTE]
    > まだ行っていない場合は、コマンドを試す前に Cloud Shell セッションを **PowerShell** に切り替えます。
    
    ```powershell
    $myRgName = "<rgn>[Sandbox resource group name]</rgn>"
    $myVMName = "fotoshareVM"
    ```
    
    > [!TIP]
    > Cloud Shell セッションがタイムアウトした場合は、これらの変数を再度設定する必要があります。そのため、可能であれば、単一のセッションでこのラボ全体を実行します。
    
### <a name="get-info-about-our-vm"></a>VM に関する情報を取得する

1. 次のコマンドを実行して、VM のプロパティを取得します。

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    ```
    
1. `$myVM` 変数に応答を保存します。 出力を `select-object` コマンドレットにパイプして、特定のプロパティが表示されるようにフィルター処理できます。

    ```powershell
    $myVM | select-object -property ResourceGroupName, Name, Type, Location
    ```
    
1. 次のように表示されます。

    ```output
    ResourceGroupName Name        Type                              Location
    ----------------- ----        ----                              --------
    <rgn>[Sandbox resource group name]</rgn> fotoshareVM Microsoft.Compute/virtualMachines eastus
    ```
    
### <a name="view-os-disk-cache-status"></a>OS ディスクのキャッシュ状態を表示する

1. 次のように、`StorageProfile` オブジェクトを介してキャッシュの設定を確認することができます。

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching
    ```

    ```output
    ReadOnly
    ```
   
1. これを OS ディスクの既定値である _ReadWrite_ に戻してみましょう。

### <a name="change-the-cache-settings-of-the-os-disk"></a>OS ディスクのキャッシュ設定を変更する

1. 次のように、同じ `StorageProfile` オブジェクトを使用してキャッシュの種類の値を設定できます。

    ```powershell
    $myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
    ```
    
    このコマンドは高速で実行されます。ローカルで何らかの処理が実行されていることがわかります。 このコマンドを実行すると、単に `myVM` オブジェクトのプロパティが変更されます。 次のスクリーンショットが示すように、`Get-AzureRmVM` コマンドレットを使用して再割り当てすることによって `$myVM` 変数を更新しても、VM 上のキャッシュ値は変更されません。

1. VM 自体を変更するには、次のように `Update-AzureRmVM` を呼び出します。

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
    この呼び出しが完了するまでには時間がかかります。 これは、実際の VM を更新しており、変更を反映するために、Azure は VM を再起動するためです。

    ```output
    RequestId IsSuccessStatusCode StatusCode ReasonPhrase
    --------- ------------------- ---------- ------------
                             True         OK OK
    ```
    
1. `$myVM` 変数を再度更新すると、オブジェクトの変更が表示されます。 ポータルでディスクを表示して変更を確認することもできます。 

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
    $myVM.StorageProfile.OsDisk.Caching
    ```
    
    ```output
    ReadWrite
    ```
    
### <a name="list-data-disk-info"></a>データ ディスク情報を一覧表示する

1. VM 上にあるデータ ディスクを確認するには、次のコマンドを実行します。

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    ```
    
現在、データ ディスクは 1 つしかありません。 `Lun` フィールドは重要です。 これは一意の論理ユニット番号 (**L****U****N**) です。 別のデータ ディスクを追加する場合は、一意の `Lun` 値を付けます。

### <a name="add-a-new-data-disk-to-our-vm"></a>VM に新しいデータ ディスクを追加する

1. 便宜的に、新しいディスク名を用意します。

    ```powershell
    $newDiskName = "fotoshareVM-data2"
    ```
    
1. 次の `Add-AzureRmVMDataDisk` コマンドを実行して、新しく 1 GB の空のデータ ディスクを定義します。

    ```powershell
    Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
    ```
    次のような応答が表示されます。

    ```output
    ResourceGroupName  : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx
    Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxx-xxxxxxx/resourceGroups/<rgn>[Sandbox resource group name]</rgn>/providers/Microsoft.Compute/virtualMachines/fotoshareVM
    VmId               : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx
    Name               : fotoshareVM
    Type               : Microsoft.Compute/virtualMachines
    Location           : eastus
    Tags               : {}
    DiagnosticsProfile : {BootDiagnostics}
    HardwareProfile    : {VmSize}
    NetworkProfile     : {NetworkInterfaces}
    OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
    ProvisioningState  : Succeeded
    StorageProfile     : {ImageReference, OsDisk, DataDisks}
        ```
    
1. We've given this disk a `Lun` value of `1` because it's not taken. We defined the disk we want to create, so it's time to run `Update-AzureRmVM` to make the actual change:

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
1. データ ディスク情報をもう一度見てみましょう。

    ```powershell
    $myVM.StorageProfile.DataDisks
    ```
    
    ```output
    Name            : fotosharesVM-data
    DiskSizeGB      : 1023
    Lun             : 0
    Caching         : ReadOnly
    CreateOption    : Attach
    SourceImage     :
    VirtualHardDisk :
    
    Name            : fotoshareVM-data2
    DiskSizeGB      : 1
    Lun             : 1
    Caching         : None
    CreateOption    : Empty
    SourceImage     :
    VirtualHardDisk :
    ```

これで 2 つのディスクができました。 新しいディスクは `Lun` が `1` で、`Caching` の既定値は `None` です。 この値を変更してみましょう。

### <a name="change-cache-settings-of-new-data-disk"></a>新しいデータ ディスクのキャッシュ設定を変更する

1. 次のように、`Set-AzureRmVMDataDisk` コマンドレットを使用して仮想マシン データ ディスクのプロパティを変更します。

    ```powershell
    Set-AzureRmVMDataDisk -VM $myVM -Lun "1" -Caching ReadWrite
    ```
    
1. いつものように、`Update-AzureRmVM` を使用して変更をコミットします。

    ```powershell
    Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
    ```
    
この演習で実行した結果のポータルのビューを次に示します。 VM には 2 つのデータ ディスクがあり、すべての **[ホスト キャッシュ]** の設定を調整しました。 そのすべてをいくつかのコマンドだけで実行しました。 これが Azure PowerShell の機能です。

![2 つのデータ ディスクが表示されている VM ブレードの [ディスク] セクションを示す Azure portal のスクリーンショット。](../media/disks-final-config-portal2.png)
