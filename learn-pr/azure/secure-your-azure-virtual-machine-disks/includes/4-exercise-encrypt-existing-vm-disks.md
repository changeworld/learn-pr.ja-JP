新興企業の財務管理アプリケーションを開発しているものとします。 顧客のすべてのデータが保護されるようにするため、このアプリケーションをホストするサーバー上のすべての OS ディスクとデータ ディスクに Azure Disk Encryption (ADE) を実装することにしました。 コンプライアンス要件の一部として、独自の暗号化キーの管理も自分で行う必要があります。

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

このユニットでは、既存の仮想マシン上のディスクを暗号化し、独自の Azure Key Vault を使用して暗号化キーを管理します。

## <a name="prepare-the-environment"></a>環境を準備する

まず、Azure 仮想マシンで新しい Windows VM をデプロイします。

### <a name="deploy-windows-vm"></a>Windows VM をデプロイする

Azure PowerShell を使用して、新しい Windows 仮想マシンを作成してデプロイします。

1. まず、新しいリソースを配置する場所を決定します。 次のリストから、自分に近い場所を選択します。

    <!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]
    

1. 選択した場所を保持する PowerShell 変数を定義します。 ここでは "米国東部" として定義されています。これを任意の場所に変更します。

    ```powershell
    $location = "eastus"
    ```
    
    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. 次に、_リソース グループ_と VM の_名前_を取り込むためのさらに便利な変数をいくつか定義します。 ここでは事前に作成されたリソース グループを使用することに注意してください。通常は、`New-AzureRmResourceGroup` を使用してサブスクリプションで_新しい_リソース グループを作成します。

    ```powershell
    $vmName = "fmdata-vm01"
    $rgName = "<rgn>[sandbox Resource Group]</rgn>"
    ```
    
1. `New-AzureRmVm` を使用して、新しい仮想マシンを作成します。
    
    ```powershell
    New-AzureRmVm `
        -ResourceGroupName $rgName `
        -Name $vmName `
        -Location $location `
        -OpenPorts 3389
    ```
    
    Cloud Shell でプロンプトが表示されたら、VM 用のユーザー名とパスワードを入力します。 これは、VM 用に作成される初期アカウントとして使用されます。
    
    > [!NOTE]
    > 一連のオプションを指定しなかったため、このコマンドではいくつかの既定値が使用されます。 つまり、サイズが _Standard_DS1_v2_ の _Windows 2016 Server_ が作成されます。 VM サイズを指定する場合、Basic レベルの VM では ADE がサポートされないことに注意してください。

1. VM のデプロイが完了したら、変数に VM の詳細を取り込みます。 この変数を使用して、作成内容を確認できます。

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```
    
1. VM に OS ディスクが接続されていることがわかります。

    ```powershell
    $vm.StorageProfile.OSDisk
    ```

    ```output
    OsType                  : Windows
    EncryptionSettings      :
    Name                    : fmdata-vm01_OsDisk_1_6bcf8dcd49794aa785bad45221ec4433
    Vhd                     :
    Image                   :
    Caching                 : ReadWrite
    WriteAcceleratorEnabled :
    CreateOption            : FromImage
    DiskSizeGB              : 127
    ManagedDisk             : Microsoft.Azure.Management.Compute.Models.ManagedDiskP
                              arameters
    ```
        
1. OS ディスク (およびすべてのデータ ディスク) の現在の暗号化の状態を確認します。

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  `
        -ResourceGroupName $rgName `
        -VMName $vmName
    ```

    ご覧のとおり、ディスクは現在_暗号化されていません_。 

    ```output
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : NotEncrypted
    OsVolumeEncryptionSettings :
    ProgressMessage            : No Encryption extension or metadata found on the VM
    ```
    
これを変更してみましょう。
    
## <a name="encrypt-the-vm-disks-with-azure-disk-encryption"></a>Azure Disk Encryption で VM ディスクを暗号化する

このデータを保護する必要があるため、ディスクを暗号化してみます。 次のようないくつかの手順を行う必要があることを思い出してください。

1. キー コンテナーを作成します。
1. ディスク暗号化をサポートするようにキー コンテナーを設定します。
1. キー コンテナーに格納されているキーを使用して、VM ディスクを暗号化するように Azure に指示します。

> [!TIP]
> ここでは手順を個別に見ていきますが、ご自分のサブスクリプションでこのタスクを実行するときは、このモジュールの「まとめ」でリンクされている便利な PowerShell スクリプトを使用することができます。

### <a name="create-a-key-vault"></a>キー コンテナーを作成する

Azure Key Vault を作成するには、サブスクリプションでサービスを有効にする必要があります。 これは 1 回限りの要件です。

> [!TIP]
> サブスクリプションによっては、`Register-AzureRmResourceProvider` コマンドレットを使用して **Microsoft.KeyVault** プロバイダーを有効にする必要がある場合があります。 Azure サンドボックス サブスクリプションではこれは必要ありません。

1. 新しいキー コンテナーの名前を決めます。 この名前は一意である必要があります。数字、文字、ダッシュで構成された 3 から 24 文字の範囲で設定できます。 末尾に乱数をいくつか追加して、以下の "1234" を置き換えてみてください。

    ```powershell
    $keyVaultName = "mvmdsk-kv-1234"
    ```
        
1. `New-AzureRmKeyVault` を使用して Azure Key Vault を作成します。
    - ご自身の VM と同じリソース グループ_および_場所に配置されていることを確認します。
    - ディスクの暗号化で使用するために Key Vault を有効にします。 
    - 一意の Key Vault 名を指定します。

    ```powershell
    New-AzureRmKeyVault -VaultName $keyVaultName `
        -Location $location `
        -ResourceGroupName $rgName `
        -EnabledForDiskEncryption
    ```

    このコマンドから、アクセス権を持つユーザーがいないことを示す警告が表示されます。

    ```output
    WARNING: Access policy is not set. No user or application have access permission to use this vault. This can happen if the vault was created by a service principal. Please use Set-AzureRmKeyVaultAccessPolicy to set access policies.
    ```

    ここでは VM 用の暗号化キーを格納するコンテナーを使用するだけで、ユーザーがこのデータにアクセスする必要はないため、問題はありません。

### <a name="encrypt-the-disk"></a>ディスクを暗号化する

ディスクを暗号化する準備はほとんどできています。 暗号化する前に、バックアップの作成に関する警告が表示されます。

> [!IMPORTANT]
> 運用システムを使用している場合は、Azure Backup を使用するか、スナップショットを作成して、マネージド ディスクのバックアップを作成する必要があります。 スナップショットは Azure portal で、またはコマンドラインを使用して作成できます。 PowerShell で、`New-AzureRmSnapshot` コマンドレットを使用します。 これはシンプルな演習であり、完了時にこのデータを破棄するため、この手順はスキップします。 

1. まず、Key Vault の情報を保持する変数を定義します。

    ```powershell
    $keyVault = Get-AzureRmKeyVault `
        -VaultName $keyVaultName `
        -ResourceGroupName $rgName
    ```

1. 次に、`Set-AzureRmVmDiskEncryptionExtension` コマンドレットを使用して、VM ディスクを暗号化します。
    - `VolumeType` パラメーターを使用して、暗号化するディスク (_[すべて]_ | _[OS]_ | _[データ]_) を指定できます。 既定では _[すべて]_ に設定されます。 Linux の場合、データ ディスクを暗号化できるのは一部のディストリビューションのみとなります。
    - マネージド ディスクに `SkipVmBackup` フラグを指定する必要があります。そうしないと、スナップショットがないため、コマンドは失敗します。

    ```powershell
    Set-AzureRmVmDiskEncryptionExtension `
        -ResourceGroupName $rgName `
        -VMName $vmName `
        -VolumeType All `
        -DiskEncryptionKeyVaultId $keyVault.ResourceId `
        -DiskEncryptionKeyVaultUrl $keyVault.VaultUri `
        -SkipVmBackup
    ```

1. このコマンドレットでは、VM をオフラインにする必要があることと、タスクが完了するまで数分かかる場合があることを示す警告が表示されます。 先に進んで、作業を続行します。

    ```output
    Enable AzureDiskEncryption on the VM
    This cmdlet prepares the VM and enables encryption which may reboot the machine and takes 10-15 minutes to
    finish. Please save your work on the VM before confirming. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    ```
    
1. 完了したら、暗号化の状態をもう一度確認します。

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
    ```

    この時点で OS ディスクが暗号化されているはずです。 Windows に表示される、接続されたデータ ディスクもすべて暗号化されます。

    ```output
    OsVolumeEncrypted          : Encrypted
    DataVolumesEncrypted       : NoDiskFound
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : Provisioning succeeded
    ```

> [!NOTE]        
> 暗号化後に追加された新しいディスクは、自動的に暗号化_されません_。 `Set-AzureRmVMDiskEncryptionExtension` コマンドレットを再実行して、新しいディスクを暗号化することができます。 既にディスクが暗号化されている VM にディスクを追加する場合は、必ず、新しいシーケンス番号を指定してください。 また、オペレーティング システムに表示されないディスクは暗号化されません。ディスクを正しくパーティション分割し、書式設定し、マウントして Bitlocker 拡張機能で確認できるようにする必要があります。