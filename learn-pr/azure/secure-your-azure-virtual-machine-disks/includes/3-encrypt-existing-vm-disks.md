社内のすべての VM に Azure Disk Encryption (ADE) を実装することに決まったとします。 既存の VM ボリュームに暗号化をロールアウトする方法を評価する必要があります。 ここでは、ADE の要件と、既存の Linux および Windows VM のディスクを暗号化するための手順を紹介します。

## <a name="azure-disk-encryption-prerequisites"></a>Azure Disk Encryption の前提条件

VM ディスクを暗号化する前に、次を行う必要があります。

1. キー コンテナーを作成します。
1. ディスク暗号化をサポートするようにキー コンテナーのアクセス ポリシーを設定します。
1. キー コンテナーを使用して、ADE の暗号化キーを格納します。

### <a name="azure-key-vault"></a>Azure Key Vault

ADE によって使用される暗号化キーは、Azure Key Vault に格納できます。 Azure Key Vault は、シークレットを安全に保管し、それにアクセスするためのツールです。 シークレットは、API キー、パスワード、証明書など、アクセスを厳密に制御する必要がある任意のものです。 これにより、Federal Information Processing Standards (FIPS) 140-2 レベル 2 で検証済みのハードウェア セキュリティ モジュール (HSM) で定義されているように、可用性が高くスケーラブルで安全なストレージが提供されます。 Key Vault を使用して、データの暗号化に使用されるキーを全面的に制御し、キーの使用方法を管理して監査できます。 

> [!NOTE]
> Azure Disk Encryption では、暗号化シークレットがリージョン境界を越えることがないため、キー コンテナーと VM が同じ Azure リージョンにある必要があります。

キー コンテナーは次の方法で構成および管理できます。

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmKeyVault -Location <location> `
    -ResourceGroupName <resource-group> `
    -VaultName "myKeyVault" `
    -EnabledForDiskEncryption
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az keyvault create \
    --name "myKeyVault" \
    --resource-group <resource-group> \
    --location <location> \
    --enabled-for-disk-encryption True
```

### <a name="azure-portal"></a>Azure portal

Azure Key Vault は、通常のリソース作成プロセスを使用して Azure portal で作成できるリソースです。

1. 左側のサイドバーで **[リソースの作成]** をクリックします。

1. "キー コンテナー" を検索します。 詳細ウィンドウで **[作成]** をクリックします。

    ![Azure Marketplace でのキー コンテナーを示すスクリーンショット](../media/3-create-keyvault.png)

1. 新しい Key Vault の詳細を入力します。
    - Key Vault の **[名前]** を入力します。
    - 配置先のサブスクリプションを選択します (既定値は現在のサブスクリプション)。
    - **[リソース グループ]** を選択するか、または Key Vault を所有する新しいリソース グループを作成します。
    - Key Vault の **[場所]** を選択します。 VM の存在する場所を必ず選択してください。
    - Standard または Premium のいずれかの価格レベルを選択できます。 主な違いは、Premium レベルではハードウェア暗号化のバックアップ キーを使用できる点です。

1. ディスク暗号化をサポートするには、アクセス ポリシーを変更する必要があります。 既定では、ポリシーに_自分の_アカウントが追加されます。
    - **[アクセス ポリシー]** を選択します。
    - **[高度なアクセス ポリシー]** をクリックします。
    - **[ボリューム暗号化に対して Azure Disk Encryption へのアクセスを有効にする]** をオンにします。
    - 必要に応じてアカウントを削除できます。ディスクの暗号化のみを目的として Key Vault を使用する場合、削除する必要はありません。
    - **[OK]** をクリックして変更を保存します。

    ![Azure Disk Encryption オプションがオンになって強調表示された、Key Vault の詳細プロパティを示すスクリーンショット](../media/3-configure-access-policy.png)

1. **[作成]** をクリックして、新しい Key Vault を作成します。

## <a name="enabling-access-policies-in-the-key-vault"></a>キー コンテナーでアクセス ポリシーを有効にする
Azure には、キー コンテナー内の暗号化キーまたはシークレットへのアクセス権を付与する必要があります。これにより、ボリュームをブートして復号化する際に、それらの情報を VM に提供できるようになります。 上記で **[高度なアクセス ポリシー]** を変更した際に、ポータルについてこのことを示しました。

有効にできるポリシーは 3 つあります。
1. **ディスク暗号化** - Azure Disk Encryption で必須です。
1. **デプロイ:** - (オプション) 仮想マシンの作成時など、このキー コンテナーがリソース作成時に参照される場合、Microsoft.Compute リソース プロバイダーがこのキー コンテナーからシークレットを取得できるようにします。
1. **テンプレートのデプロイ** - (オプション) テンプレートのデプロイでこのキー コンテナーが参照される場合、Azure Resource Manager がこのキー コンテナーからシークレットを取得できるようにします。

ディスクの暗号化ポリシーを有効にする方法を次に示します。 他の 2 つは似ていますが、異なるフラグが使用されています。

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <keyvault-name> -ResourceGroupName <resource-group> -EnabledForDiskEncryption
```

```azurecli
az keyvault update --name <keyvault-name> --resource-group <resource-group> --enabled-for-disk-encryption "true"
```

## <a name="encrypting-an-existing-vm-disk"></a>既存の VM ディスクの暗号化

Key Vault をセットアップしたら、Azure CLI または Azure PowerShell を使用して VM を暗号化できます。 Windows VM を初めて暗号化する場合は、すべてのディスクを暗号化するか、OS ディスクのみを暗号化するかを選択できます。 Linux ディストリビューションによっては、データ ディスクのみが暗号化される場合があります。 暗号化の対象となるには、Windows のディスクが NTFS ボリュームとしてフォーマットされている必要があります。

> [!WARNING]
> 暗号化をオンにするには、まずマネージド ディスクのスナップショットまたはバックアップを作成する必要があります。 下記で指定する `SkipVmBackup` フラグにより、マネージド ディスクでバックアップが完了したことがツールに通知されます。 バックアップがなければ、何らかの理由で暗号化が失敗した場合に VM を復旧できません。

PowerShell で、`Set-AzureRmVmDiskEncryptionExtension` コマンドレットを使用して暗号化を有効にします。

```powershell

Set-AzureRmVmDiskEncryptionExtension `
    -ResourceGroupName <resource-group> `
    -VMName <vm-name> `
    -VolumeType [All | OS | Data]
    -DiskEncryptionKeyVaultId <keyVault.ResourceId> `
    -DiskEncryptionKeyVaultUrl <keyVault.VaultUri> `
     -SkipVmBackup
```

Azure CLI を使用する場合は、`az vm encryption enable` コマンドレットを使用して暗号化を有効にします。

```azurecli
az vm encryption enable \
    --resource-group <resource-group> \
    --name <vm-name> \
    --disk-encryption-keyvault <keyvault-name> \
    --volume-type [all | os | data] \
    --skipvmbackup
```

## <a name="viewing-the-status-of-the-disk"></a>ディスクの状態の表示

特定のディスクが暗号化されるかどうかを確認できます。

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName <resource-group> -VMName <vm-name>
```

```azurecli
az vm encryption status --resource-group <resource-group> --name <vm-name>
```

これらのコマンドの両方により、指定された VM にアタッチされている各ディスクの状態が返されます。

## <a name="decrypting-drives"></a>ドライブの復号化

`Disable-AzureRmVMDiskEncryption` を使用して、PowerShell で復号化できます。

```powershell
Disable-AzureRmVMDiskEncryption -ResourceGroupName <resource-group> -VMName <vm-name>
```

Azure CLI を使用する場合は、`vm encryption disable` コマンドを使用します。

```azurecli
az vm encryption disable --resource-group <resource-group> --name <vm-name>
```

これらのコマンドにより、指定された仮想マシンの all タイプのボリュームに対する暗号化が無効になります。 暗号化の場合と同様に、復号化するディスクを決定する`-VolumeType`パラメーター`[All | OS | Data]`を指定できます。 指定されていない場合の既定値は `All` です。

> [!WARNING]
> OS ディスクとデータ ディスクの両方が暗号化されている場合に Windows VM でデータ ディスク暗号化を無効にしようとしても、うまくいきません。 代わりに、すべてのディスクの暗号化を無効にする必要があります。

これらのいくつかのコマンドを新しい VM で試してみましょう。