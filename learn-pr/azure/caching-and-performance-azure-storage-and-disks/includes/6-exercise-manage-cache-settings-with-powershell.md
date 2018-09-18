
前回の演習では、Azure portal を使用して以下のタスクを実行しました。

- OS ディスクのキャッシュ状態を表示する
- OS ディスクのキャッシュ設定を変更する
- VM にデータ ディスクを追加する
- 新しいデータ ディスク上のキャッシュの種類を変更する

Azure PowerShell を使用してこれらの操作を練習してみましょう。 ここでは、これまでの演習で作成した VM を使用します。 このラボの操作では、以下のことを前提としています。

- VM は存在し、**fotoshareVM** という名前です
- VM は、**fotoshare-rg** というリソース グループ内に存在します。

これらの値は異なる名前を使用した場合は、これらの値を実際の名前で置き換えてください。 

前回の演習の VM ディスクは、次のような状態です。 

![OS ディスクとデータ ディスクのスクリーンショットでは、どちらも読み取り専用キャッシュに設定されています。](../media-draft/disks-final-config-portal.PNG)

ポータルを使用して、OS ディスクとデータ ディスクの両方に ***[ホスト キャッシュ]** フィールドを設定しています。 以下の手順を行う際には、この初期状態を念頭に置いてください。 

### <a name="set-up-some-variables"></a>変数をいくつか設定する
まず、後で使用できるように、いくつかのリソース名を用意しておきましょう。

右側の Azure Cloud Shell のターミナルを使用して、次の PowerShell コマンドを実行します。 

```powershell
$myRgName = "fotoshare-rg"
$myVMName = "fotoshareVM"
```

> [!TIP]
> Cloud Shell セッションがタイムアウトした場合は、これらの変数を再度設定する必要があります。そのため、可能であれば、単一のセッションでこのラボ全体を実行します。 

### <a name="get-info-about-our-vm"></a>VM に関する情報を取得する

次のコマンドを実行して、VM のプロパティを取得します。
 
```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVMName
```
`$myVM` 変数に応答を保存します。 次のコマンドを実行すると、ここで指定したプロパティを表示できます。

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

次のスクリーンショットが示すように、この VM が目的の VM です。 それでは次に進みましょう。 

![最後に実行した 4 つのコマンドの結果を表示する PowerShell コンソール。](../media-draft/ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a>OS ディスクのキャッシュ状態を表示する

次のように、`StorageProfile` オブジェクトを介してキャッシュの設定を確認することができます。

```powershell
$myVM.StorageProfile.OsDisk.Caching
```
この例では、現在の値は **None** です。 これを OS ディスクの既定に戻してみましょう。

![キャッシュ値が "None" の OS ディスクが表示されている PowerShell コンソール。](../media-draft/ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a>OS ディスクのキャッシュ設定を変更する

次のように、同じ StorageProfile オブジェクトを使用してキャッシュの種類の値を設定できます。
 
```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

このコマンドは高速で実行されます。ローカルで何らかの処理が実行されていることがわかります。 このコマンドを実行すると、単に myVM オブジェクトのプロパティが変更されます。 次のスクリーンショットが示すように、`$myVM` 変数を更新しても、VM 上のキャッシュ値は変更されません。

![実際には VM を更新しなかったため、"myVM" オブジェクトを更新すると、キャッシュが "none" にリセットされることを示す PowerShell コンソール。](../media-draft/ps-commands-2.PNG)

VM 自体を変更するには、次のように `Update-AzureRmVM` を呼び出します。

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

この呼び出しが完了するまでには時間がかかります。 これは、実際の VM を更新しており、変更を反映するために、Azure は VM を再起動するためです。

![キャッシュ値が "None" の OS ディスクが表示されている PowerShell コンソール。](../media-draft/ps-oscaching-rw.PNG)

`$myVM` 変数を再度更新すると、オブジェクトの変更が表示されます。 ポータルでディスクを表示して変更を確認することもできます。 それでは、新しいデータ ディスクの作成に進みましょう。  

### <a name="list-data-disk-info"></a>データ ディスク情報を一覧表示する

VM 上にあるデータ ディスクを確認するには、次のコマンドを実行します。 

```powershell
$myVM.StorageProfile.DataDisks
```

現在、データ ディスクは 1 つしかありません。 `Lun` フィールドは重要です。 これは一意の論理ユニット番号 (**L****U****N**) です。 別のデータ ディスクを追加する場合は、一意の `Lun` 値を付けます。 

### <a name="add-a-new-data-disk-to-our-vm"></a>VM に新しいデータ ディスクを追加する 

便宜的に、新しいディスク名を用意します。

```powershell
$newDiskName = "fotoshareVM-data2"
```

次の `Add-AzureRmVMDataDisk` コマンドを実行して、新しいディスクを定義します。 

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

1 の Lun 値は使用されていないため、このディスクに使用しました。 作成するディスクを定義したので、次は `Update-AzureRmVM` を実行して実際に変更してみましょう。 

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

データ ディスク情報をもう一度見てみましょう。

```powershell
$myVM.StorageProfile.DataDisks
```

![2 つのデータ ディスクが表示されている PowerShell コンソール。](../media-draft/2-data-disks-part1.png)

これで 2 つのディスクができました。 新しいディスクの **Lun** 値は 1 で、**Caching** の既定値は **None** です。 この値を変更してみましょう。

### <a name="change-cache-settings-of-new-data-disk"></a>新しいデータ ディスクのキャッシュ設定を変更する

次のように、`Set-AzureRmVMDataDisk` コマンドレットを使用して仮想マシン データ ディスクのプロパティを変更します。

```powershell
Set-AzureRmVMDataDisk -Lun "1" -Caching ReadWrite
```

いつものように、`Update-AzureRmVM` を使用して変更をコミットします。

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

この演習で実行した結果のポータルのビューを次に示します。 VM には 2 つのデータ ディスクがあり、すべての **[ホスト キャッシュ]** の設定を調整しました。 そのすべてをいくつかのコマンドだけで実行しました。 これが Azure PowerShell の機能です。

![2 つのデータ ディスクが表示されている PowerShell コンソール。](../media-draft/disks-final-config-portal2.png)
