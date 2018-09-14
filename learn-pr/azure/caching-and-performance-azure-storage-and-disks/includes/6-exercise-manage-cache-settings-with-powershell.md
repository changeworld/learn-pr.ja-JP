前の演習では、Azure portal を使用して、次のタスクを実行しました。

- ビューの OS ディスク キャッシュの状態
- OS ディスクのキャッシュ設定を変更します。
- VM にデータ ディスクを追加する
- 新しいデータ ディスクでキャッシングの種類を変更します。

Azure PowerShell を使用してこれらの操作を練習してみましょう。 前の演習で作成した VM を使用していきます。 このラボでの操作があるとします。

- VM が存在すると呼びます**fotoshareVM**
- VM と呼ばれるリソース グループに在住 **<rgn>[サンド ボックス リソース グループ名]</rgn>**

名前の別のセットを持つ終えた場合、自分のこれらの値を置き換えます。

前回の演習からの VM ディスクの現在の状態を次に示します。

![読み取り専用キャッシュに、OS とデータ ディスクのスクリーン ショットは、どちらも設定します。](../media/disks-final-config-portal.PNG)

設定する、ポータルを使用しています、**ホスト キャッシュ**OS とデータ ディスクに対応するフィールド。 努力を次の手順では、この初期状態に注意してください。

### <a name="set-up-some-variables"></a>いくつかの変数を設定します。

最初に、後で使用して、いくつかのリソース名を格納しましょう。

右側の次の PowerShell コマンドを実行するのに Azure Cloud Shell のターミナルを使用します。

> [!NOTE]
> Cloud Shell セッションを切り替える**PowerShell**でない場合、これらのコマンドを試す前にします。

```powershell
$myRgName = "<rgn>[Sandbox resource group name]</rgn>"
$myVMName = "fotoshareVM"
```

> [!TIP]
> Cloud Shell セッションがタイムアウトした場合、これらの変数をもう一度設定する必要があります。そのため、可能であれば、作業この演習全体を通じて、単一セッションでします。

### <a name="get-info-about-our-vm"></a>VM に関する情報を取得します。

VM のプロパティに戻るには、次のコマンドを実行します。

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVmName
```

応答を格納します、`$myVM`変数。 次のコマンドは、ここで指定します、プロパティを表示だけを実行できます。

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

次のスクリーン ショットに示す、この VM は実際に私たちが求めている VM にします。 そのため、先に進みましょう。

![最後の 4 つのコマンドの結果を示す Azure PowerShell コンソールのスクリーン ショット。](../media/6-ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a>ビューの OS ディスク キャッシュの状態

使用して、キャッシュ設定を確認することができます、`StorageProfile`オブジェクトに、次のようにします。

```powershell
$myVM.StorageProfile.OsDisk.Caching
```

この例では、現在の値は`None`します。 OS ディスクの既定値に変更してみましょう。

!["None"キャッシュ値を持つ、OS ディスクを示す Azure PowerShell コンソールのスクリーン ショット。](../media/6-ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a>OS ディスクのキャッシュ設定を変更します。

使用して、同じキャッシュの種類の値を設定します`StorageProfile`オブジェクトに、次のようにします。

```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

このコマンドは、これがどのように指示する何かローカルでこれには高速で実行されます。 コマンドは、のプロパティを変更するだけです、`myVM`オブジェクト。 更新する場合は次のスクリーン ショットとして、`$myVM`変数、キャッシュ値はありませんが変更された VM で。

!["MyVM"オブジェクトを更新することを示す Azure PowerShell コンソールのスクリーン ショットでは、VM 実際に更新していないために、"none"にキャッシュをリセットします。](../media/6-ps-commands-2.PNG)

VM 自体に変更するには、呼び出す`Update-AzureRmVM`、次のようにします。

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

この呼び出しが完了するまでに受け取ることに注意してください。 実際の VM を更新していて、Azure は、変更した VM を再起動するためです。

!["None"キャッシュ値を持つ、OS ディスクを示す Azure PowerShell コンソールのスクリーン ショット。](../media/6-ps-oscaching-rw.PNG)

更新する場合、`$myVM`変数もう一度に表示されます、変更、オブジェクト。 ポータルでディスクを見ても表示されます、変更があります。 新しいデータ ディスクの作成に進みましょう。

### <a name="list-data-disk-info"></a>データ ディスク情報の一覧

VM があるため、どのようなデータ ディスクを表示するには、次のコマンドを実行します。

```powershell
$myVM.StorageProfile.DataDisks
```

現時点では 1 つだけのデータ ディスクがあります。 `Lun`フィールドが重要です。 これは一意**L**ogical **U**nit **N**umber します。 別のデータ ディスクを追加する際にお任せ、一意`Lun`値。

### <a name="add-a-new-data-disk-to-our-vm"></a>新しいデータ ディスクを VM に追加します。

利便性のため、新しいディスクの名前を格納します。

```powershell
$newDiskName = "fotoshareVM-data2"
```

次の実行`Add-AzureRmVMDataDisk`新しいディスクを定義するコマンド。

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

このディスクが提供された、`Lun`の値`1`として処理しないためです。 実行するところを作成しディスクを定義した`Update-AzureRmVM`実際の変更を加えます。

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

もう一度、データ ディスクの情報を見てみましょう。

```powershell
$myVM.StorageProfile.DataDisks
```

![2 つのデータ ディスクを示す Azure PowerShell コンソールのスクリーン ショット。](../media/2-data-disks-part1.png)

2 つのディスクがあるようになりました。 当社の新しいディスクが、`Lun`の`1`の既定値と`Caching`は`None`。 その値を変更してみましょう。

### <a name="change-cache-settings-of-new-data-disk"></a>新しいデータ ディスクのキャッシュ設定を変更します。

データ ディスクを仮想マシンのプロパティを変更しました、`Set-AzureRmVMDataDisk`コマンドレットを次のとおりです。

```powershell
Set-AzureRmVMDataDisk -VM $myVM -Lun "1" -Caching ReadWrite
```

いつものようでの変更をコミット`Update-AzureRmVM`:

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

この演習では達成してきた内容のポータルからビューを次に示します。 2 つのデータ ディスクには、VM と、すべてを調整しました**ホスト キャッシュ**設定します。 すべてをいくつかのコマンドを実行しました。 Azure PowerShell の機能です。

![2 つのデータ ディスクを備えた、VM のブレードの [ディスク] セクションを示す Azure portal のスクリーン ショット。](../media/disks-final-config-portal2.png)
