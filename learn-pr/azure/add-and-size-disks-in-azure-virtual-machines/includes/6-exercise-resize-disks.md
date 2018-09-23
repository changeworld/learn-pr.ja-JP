一部のアップロードの規模を低く見積もったため、アップロード ディスクの領域が不足しています。 領域を 2 倍にし、64 GB から 128 GB にアップグレードすることにします。

## <a name="resize-the-data-disk"></a>データ ディスクのサイズを変更する

ディスクのサイズを変更するには、ディスクの ID または名前が必要です。 ここでは、名前 (uploadDataDisk1) は既にわかっています。 しかし、それを記憶しなかった場合や、それが他のユーザーによって作成された場合、`az disk list` コマンドを使用してディスクのリストを取得することができます。

1. まず、リソース グループ内のマネージド ディスクのリストを取得します。単一のリソース グループに VM が複数ある場合、これには他のディスクが含まれる可能性があります。 この例では、Web サーバーのみです。

    ```azurecli
    az disk list \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

1. 次に、`az vm deallocate` を使用して VM を停止し、割り当てを解除します。 

    ```azurecli
    az vm deallocate --name support-web-vm01
    ```
1. これで、`az disk update` コマンドを使用してディスクのサイズを変更できます。

    ```azurecli
    az disk update --name uploadDataDisk1 --size-gb 128
    ```
    
1. サイズ変更の操作が完了したら、VM を再起動します。

    ```azurecli
    az vm start --name support-web-vm01
    ```

## <a name="expand-the-disk-partition"></a>ディスク パーティションを拡張する

最後の手順では、利用可能な領域について OS に伝えます。 パーティション分割とフォーマットについて前に行った手順と同様、オンプレミスのディスク拡張に関するこのプロセスは同じです。 

1. まず、実際の VM のパブリック IP アドレスを取得します。 VM は再起動されたため、変更されている可能性があります。 今度は別のアプローチを試してみましょう。その場合、パブリック IP アドレスを返すようにフィルターで `az vm show` を使用します。

    > [!TIP]
    > IP アドレスは既定で動的になります。 Azure DNS では IP 変更が自動的に補正されます。静的 IP アドレスを使用して、自分で動作を変更することもできます。

    ```azurecli
    az vm show --name support-web-vm01 -d --query [publicIps] --o tsv
    ```
    
1. Linux コンピューターに ssh 接続します。 正しい IP アドレスを指定する必要があります。

    ```bash
    ssh azureuser@40.76.193.249
    ```

1. ディスクのマウントを解除します。 `/dev/sdc1` であることを思い出してください。

    ```bash
    sudo umount /dev/sdc1
    ```

1. 管理者特権のシェルで `parted` を起動する

    ```bash
    sudo parted /dev/sdc
    ```
    
1. `resizepart` コマンドを使用して、パーティションを拡張します。 パーティション 1 と新しいサイズ (128 GB) を入力します。

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [64GB]? 128GB
    ```

    > [!WARNING]
    > サイズには注意してください。 パーティションのサイズを変更するとパーティションの縮小も可能になり、データ損失が発生する可能性が高くなります。
    
1. 「`quit`」と入力して、ツールを終了します。

1. パーティション ツールによってドライブが自動的に_再マウント_されます。 そのため、もう一度マウントを解除して、フォーマットできるようにします。

    ```bash
    sudo umount /dev/sdc1
    ```
    
1. `e2fsck` を使用してパーティションの整合性を確認します。 この手順はどうしても必要になりますが、ディスク ボリュームのサイズを変更する際には随時行うことをお勧めします。

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

1. `resize2fs` を使用して、ファイルシステムのサイズを変更します。

    ```bash
    sudo resize2fs /dev/sdc1
    ```

1. 最後に、ドライブをマウント ポイントにマウントし直します。

    ```bash
    sudo mount /dev/sdc1 /uploads
    ```

OS ディスクのサイズが変更されたことを確認するには、`df -h` を使用します。 ここでドライブが 128 GB であることが表示されます。
