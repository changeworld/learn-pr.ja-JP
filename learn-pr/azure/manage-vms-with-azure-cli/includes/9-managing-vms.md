仮想マシンを実行するために行う主なタスクの 1 つが、仮想マシンの起動と停止です。

## <a name="stopping-a-vm"></a>VM の停止

実行中の VM を停止するには、`vm stop` コマンドを使用します。 VM の名前とリソース グループ、または一意の ID を渡す必要があります。

```azurecli
az vm stop -n SampleVM -g ExerciseResources
```

停止したことを確認するには、`ssh` を使用して、または `vm get-instance-view` コマンドを使用して、パブリック IP アドレスに ping を試行します。 この最後の方法では、`vm show` と同じ基本的なデータが返されますが、インスタンス自体の詳細が含まれます。 Azure Cloud Shell に次のコマンドを入力して、VM の現在の実行状態を確認してみましょう。

```azurecli
az vm get-instance-view -n SampleVM -g ExerciseResources --query "instanceView.statuses[?starts_with(code, 'PowerState/') == `true`].displayStatus" -o tsv
```

このコマンドでは、結果として `VM stopped` が返されるはずです。

## <a name="starting-a-vm"></a>VM の起動

`vm start` コマンドを使用して逆のことができます。

```azurecli
az vm start -n SampleVM -g ExerciseResources
```

このコマンドは、停止した VM を起動します。 これを確認するには、`vm get-instance-view` クエリを使用します。これにより `VM running` が返されるはずです。

## <a name="restarting-a-vm"></a>VM の再起動

最後に、VM に再起動が必要な変更を行った場合は、`vm restart` コマンドを使用して再起動することができます。 VM の再起動を待たずに Azure CLI をすぐに復帰させる場合は、`--no-wait` フラグを追加します。

