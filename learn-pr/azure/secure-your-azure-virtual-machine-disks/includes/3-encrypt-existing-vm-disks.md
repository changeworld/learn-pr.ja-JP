<span data-ttu-id="ce0c5-101">社内のすべての VM に Azure Disk Encryption (ADE) を実装することに決まったとします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-101">Suppose your company has decided to implement Azure Disk Encryption (ADE) across all VMs.</span></span> <span data-ttu-id="ce0c5-102">既存の VM ボリュームに暗号化をロールアウトする方法を評価する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-102">You need to evaluate how to roll out encryption to existing VM volumes.</span></span> <span data-ttu-id="ce0c5-103">ここでは、ADE の要件と、既存の Linux および Windows VM のディスクを暗号化するための手順を紹介します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-103">Here, we'll look at the requirements for ADE, and the steps involved in encrypting disks on existing Linux and Windows VMs.</span></span>

## <a name="azure-disk-encryption-prerequisites"></a><span data-ttu-id="ce0c5-104">Azure Disk Encryption の前提条件</span><span class="sxs-lookup"><span data-stu-id="ce0c5-104">Azure Disk Encryption prerequisites</span></span>

<span data-ttu-id="ce0c5-105">VM ディスクを暗号化する前に、次を行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-105">Before you can encrypt your VM disks, you need to:</span></span>

1. <span data-ttu-id="ce0c5-106">キー コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-106">Create a key vault.</span></span>
1. <span data-ttu-id="ce0c5-107">ディスク暗号化をサポートするようにキー コンテナーのアクセス ポリシーを設定します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-107">Set the key vault access policy to support disk encryption.</span></span>
1. <span data-ttu-id="ce0c5-108">キー コンテナーを使用して、ADE の暗号化キーを格納します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-108">Use the key vault to store the encryption keys for ADE.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="ce0c5-109">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ce0c5-109">Azure Key Vault</span></span>

<span data-ttu-id="ce0c5-110">ADE によって使用される暗号化キーは、Azure Key Vault に格納できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-110">The encryption keys used by ADE can be stored in Azure Key Vault.</span></span> <span data-ttu-id="ce0c5-111">Azure Key Vault は、シークレットを安全に保管し、それにアクセスするためのツールです。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-111">Azure Key Vault is a tool for securely storing and accessing secrets.</span></span> <span data-ttu-id="ce0c5-112">シークレットは、API キー、パスワード、証明書など、アクセスを厳密に制御する必要がある任意のものです。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-112">A secret is anything that you want to tightly control access to, such as API keys, passwords, or certificates.</span></span> <span data-ttu-id="ce0c5-113">これにより、Federal Information Processing Standards (FIPS) 140-2 レベル 2 で検証済みのハードウェア セキュリティ モジュール (HSM) で定義されているように、可用性が高くスケーラブルで安全なストレージが提供されます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-113">This provides highly available and scalable secure storage, as defined in Federal Information Processing Standards (FIPS) 140-2 Level 2 validated Hardware Security Modules (HSMs).</span></span> <span data-ttu-id="ce0c5-114">Key Vault を使用して、データの暗号化に使用されるキーを全面的に制御し、キーの使用方法を管理して監査できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-114">Using Key Vault, you keep full control of the keys used to encrypt your data, and you can manage and audit your key usage.</span></span> 

> [!NOTE]
> <span data-ttu-id="ce0c5-115">Azure Disk Encryption では、暗号化シークレットがリージョン境界を越えることがないため、キー コンテナーと VM が同じ Azure リージョンにある必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-115">Azure Disk Encryption requires that your key vault and your VMs are in the same Azure region; this ensures that encryption secrets do not cross regional boundaries.</span></span>

<span data-ttu-id="ce0c5-116">キー コンテナーは次の方法で構成および管理できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-116">You can configure and manage your key vault with:</span></span>

#### <a name="powershell"></a><span data-ttu-id="ce0c5-117">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ce0c5-117">Powershell</span></span>

```powershell
New-AzureRmKeyVault -Location <location> `
    -ResourceGroupName <resource-group> `
    -VaultName "myKeyVault" `
    -EnabledForDiskEncryption
```

### <a name="azure-cli"></a><span data-ttu-id="ce0c5-118">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ce0c5-118">Azure CLI</span></span>

```azurecli
az keyvault create \
    --name "myKeyVault" \
    --resource-group <resource-group> \
    --location <location> \
    --enabled-for-disk-encryption True
```

### <a name="azure-portal"></a><span data-ttu-id="ce0c5-119">Azure portal</span><span class="sxs-lookup"><span data-stu-id="ce0c5-119">Azure portal</span></span>

<span data-ttu-id="ce0c5-120">Azure Key Vault は、通常のリソース作成プロセスを使用して Azure portal で作成できるリソースです。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-120">An Azure Key Vault is a resource that can be created in the Azure portal using the normal resource creation process.</span></span>

1. <span data-ttu-id="ce0c5-121">左側のサイドバーで **[リソースの作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-121">Click **Create a resource** in the sidebar on the left.</span></span>

1. <span data-ttu-id="ce0c5-122">"キー コンテナー" を検索します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-122">Search for "Key vault".</span></span> <span data-ttu-id="ce0c5-123">詳細ウィンドウで **[作成]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-123">Click **Create** in the details window.</span></span>

    ![Azure Marketplace でのキー コンテナーを示すスクリーンショット](../media/3-create-keyvault.png)

1. <span data-ttu-id="ce0c5-125">新しい Key Vault の詳細を入力します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-125">Enter the details for the new Key Vault:</span></span>
    - <span data-ttu-id="ce0c5-126">Key Vault の **[名前]** を入力します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-126">Enter a **Name** for the Key Vault</span></span>
    - <span data-ttu-id="ce0c5-127">配置先のサブスクリプションを選択します (既定値は現在のサブスクリプション)。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-127">Select the subscription to place it in (defaults to your current subscription).</span></span>
    - <span data-ttu-id="ce0c5-128">**[リソース グループ]** を選択するか、または Key Vault を所有する新しいリソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-128">Select a **Resource Group**, or create a new resource group to own the Key Vault.</span></span>
    - <span data-ttu-id="ce0c5-129">Key Vault の **[場所]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-129">Select a **Location** for the Key Vault.</span></span> <span data-ttu-id="ce0c5-130">VM の存在する場所を必ず選択してください。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-130">Make sure to select the location the VM is in.</span></span>
    - <span data-ttu-id="ce0c5-131">Standard または Premium のいずれかの価格レベルを選択できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-131">You can choose either Standard or Premium for the pricing tier.</span></span> <span data-ttu-id="ce0c5-132">主な違いは、Premium レベルではハードウェア暗号化のバックアップ キーを使用できる点です。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-132">The main difference is that the premium tier allows for Hardware-encryption backed keys.</span></span>

1. <span data-ttu-id="ce0c5-133">ディスク暗号化をサポートするには、アクセス ポリシーを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-133">You must change the Access policies to support Disk Encryption.</span></span> <span data-ttu-id="ce0c5-134">既定では、ポリシーに_自分の_アカウントが追加されます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-134">By default it adds _your_ account to the policy.</span></span>
    - <span data-ttu-id="ce0c5-135">**[アクセス ポリシー]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-135">Select **Access policies**</span></span>
    - <span data-ttu-id="ce0c5-136">**[高度なアクセス ポリシー]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-136">Click **Advanced access policies**.</span></span>
    - <span data-ttu-id="ce0c5-137">**[ボリューム暗号化に対して Azure Disk Encryption へのアクセスを有効にする]** をオンにします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-137">Check the **Enable access to Azure Disk Encryption for volume encryption**.</span></span>
    - <span data-ttu-id="ce0c5-138">必要に応じてアカウントを削除できます。ディスクの暗号化のみを目的として Key Vault を使用する場合、削除する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-138">You can remove your account if you like, it's not necessary if you only intend to use the Key Vault for disk encryption.</span></span>
    - <span data-ttu-id="ce0c5-139">**[OK]** をクリックして変更を保存します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-139">Click **OK** to save the changes.</span></span>

    ![Azure Disk Encryption オプションがオンになって強調表示された、Key Vault の詳細プロパティを示すスクリーンショット](../media/3-configure-access-policy.png)

1. <span data-ttu-id="ce0c5-141">**[作成]** をクリックして、新しい Key Vault を作成します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-141">Click **Create** to create the new Key Vault.</span></span>

## <a name="enabling-access-policies-in-the-key-vault"></a><span data-ttu-id="ce0c5-142">キー コンテナーでアクセス ポリシーを有効にする</span><span class="sxs-lookup"><span data-stu-id="ce0c5-142">Enabling access policies in the key vault</span></span>
<span data-ttu-id="ce0c5-143">Azure には、キー コンテナー内の暗号化キーまたはシークレットへのアクセス権を付与する必要があります。これにより、ボリュームをブートして復号化する際に、それらの情報を VM に提供できるようになります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-143">Azure needs access to the encryption keys or secrets in your key vault to make them available to the VM for booting and decrypting the volumes.</span></span> <span data-ttu-id="ce0c5-144">上記で **[高度なアクセス ポリシー]** を変更した際に、ポータルについてこのことを示しました。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-144">We covered this for the portal when we changed the **Advanced access policies** above.</span></span>

<span data-ttu-id="ce0c5-145">有効にできるポリシーは 3 つあります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-145">There are three policies you can enable.</span></span>
1. <span data-ttu-id="ce0c5-146">**ディスク暗号化** - Azure Disk Encryption で必須です。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-146">**Disk encryption** - Required for Azure Disk encryption.</span></span>
1. <span data-ttu-id="ce0c5-147">**デプロイ:** - (オプション) 仮想マシンの作成時など、このキー コンテナーがリソース作成時に参照される場合、Microsoft.Compute リソース プロバイダーがこのキー コンテナーからシークレットを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-147">**Deployment** - (Optional) Enables the Microsoft.Compute resource provider to retrieve secrets from this key vault when this key vault is referenced in resource creation, for example when creating a virtual machine.</span></span>
1. <span data-ttu-id="ce0c5-148">**テンプレートのデプロイ** - (オプション) テンプレートのデプロイでこのキー コンテナーが参照される場合、Azure Resource Manager がこのキー コンテナーからシークレットを取得できるようにします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-148">**Template deployment** - (Optional) Enables Azure Resource Manager to get secrets from this key vault when this key vault is referenced in a template deployment.</span></span>

<span data-ttu-id="ce0c5-149">ディスクの暗号化ポリシーを有効にする方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-149">Here's how to enable the disk encryption policy.</span></span> <span data-ttu-id="ce0c5-150">他の 2 つは似ていますが、異なるフラグが使用されています。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-150">The other two are similar but use different flags.</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <keyvault-name> -ResourceGroupName <resource-group> -EnabledForDiskEncryption
```

```azurecli
az keyvault update --name <keyvault-name> --resource-group <resource-group> --enabled-for-disk-encryption "true"
```

## <a name="encrypting-an-existing-vm-disk"></a><span data-ttu-id="ce0c5-151">既存の VM ディスクの暗号化</span><span class="sxs-lookup"><span data-stu-id="ce0c5-151">Encrypting an existing VM disk</span></span>

<span data-ttu-id="ce0c5-152">Key Vault をセットアップしたら、Azure CLI または Azure PowerShell を使用して VM を暗号化できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-152">Once you have the Key Vault setup, you can encrypt the VM using either Azure CLI or Azure PowerShell.</span></span> <span data-ttu-id="ce0c5-153">Windows VM を初めて暗号化する場合は、すべてのディスクを暗号化するか、OS ディスクのみを暗号化するかを選択できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-153">The first time you encrypt a Windows VM, you can choose to encrypt either all disks or the OS disk only.</span></span> <span data-ttu-id="ce0c5-154">Linux ディストリビューションによっては、データ ディスクのみが暗号化される場合があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-154">On some Linux distributions, only the data disks may be encrypted.</span></span> <span data-ttu-id="ce0c5-155">暗号化の対象となるには、Windows のディスクが NTFS ボリュームとしてフォーマットされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-155">To be eligible for encryption, your Windows disks must be formatted as NTFS volumes.</span></span>

> [!WARNING]
> <span data-ttu-id="ce0c5-156">暗号化をオンにするには、まずマネージド ディスクのスナップショットまたはバックアップを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-156">You must take a snapshot or a backup of managed disks before you can turn on encryption.</span></span> <span data-ttu-id="ce0c5-157">下記で指定する `SkipVmBackup` フラグにより、マネージド ディスクでバックアップが完了したことがツールに通知されます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-157">The `SkipVmBackup` flag specified below tells the tool that the backup is complete on managed disks.</span></span> <span data-ttu-id="ce0c5-158">バックアップがなければ、何らかの理由で暗号化が失敗した場合に VM を復旧できません。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-158">Without the backup, you will be unable to recover the VM if the encryption fails for some reason.</span></span>

<span data-ttu-id="ce0c5-159">PowerShell で、`Set-AzureRmVmDiskEncryptionExtension` コマンドレットを使用して暗号化を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-159">With PowerShell, use the `Set-AzureRmVmDiskEncryptionExtension` cmdlet to enable encryption.</span></span>

```powershell

Set-AzureRmVmDiskEncryptionExtension `
    -ResourceGroupName <resource-group> `
    -VMName <vm-name> `
    -VolumeType [All | OS | Data]
    -DiskEncryptionKeyVaultId <keyVault.ResourceId> `
    -DiskEncryptionKeyVaultUrl <keyVault.VaultUri> `
     -SkipVmBackup
```

<span data-ttu-id="ce0c5-160">Azure CLI を使用する場合は、`az vm encryption enable` コマンドレットを使用して暗号化を有効にします。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-160">For the Azure CLI, use the `az vm encryption enable` command to enable encryption.</span></span>

```azurecli
az vm encryption enable \
    --resource-group <resource-group> \
    --name <vm-name> \
    --disk-encryption-keyvault <keyvault-name> \
    --volume-type [all | os | data] \
    --skipvmbackup
```

## <a name="viewing-the-status-of-the-disk"></a><span data-ttu-id="ce0c5-161">ディスクの状態の表示</span><span class="sxs-lookup"><span data-stu-id="ce0c5-161">Viewing the status of the disk</span></span>

<span data-ttu-id="ce0c5-162">特定のディスクが暗号化されるかどうかを確認できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-162">You can check whether specific disks are encrypted or not.</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName <resource-group> -VMName <vm-name>
```

```azurecli
az vm encryption status --resource-group <resource-group> --name <vm-name>
```

<span data-ttu-id="ce0c5-163">これらのコマンドの両方により、指定された VM にアタッチされている各ディスクの状態が返されます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-163">Both of these commands will return the status of each disk attached to the specified VM.</span></span>

## <a name="decrypting-drives"></a><span data-ttu-id="ce0c5-164">ドライブの復号化</span><span class="sxs-lookup"><span data-stu-id="ce0c5-164">Decrypting drives</span></span>

<span data-ttu-id="ce0c5-165">`Disable-AzureRmVMDiskEncryption` を使用して、PowerShell で復号化できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-165">You can reverse the encryption through PowerShell using `Disable-AzureRmVMDiskEncryption`.</span></span>

```powershell
Disable-AzureRmVMDiskEncryption -ResourceGroupName <resource-group> -VMName <vm-name>
```

<span data-ttu-id="ce0c5-166">Azure CLI を使用する場合は、`vm encryption disable` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-166">For the Azure CLI, use the `vm encryption disable` command.</span></span>

```azurecli
az vm encryption disable --resource-group <resource-group> --name <vm-name>
```

<span data-ttu-id="ce0c5-167">これらのコマンドにより、指定された仮想マシンの all タイプのボリュームに対する暗号化が無効になります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-167">These commands disable encryption for volumes of type all for the specified virtual machine.</span></span> <span data-ttu-id="ce0c5-168">暗号化の場合と同様に、復号化するディスクを決定する`-VolumeType`パラメーター`[All | OS | Data]`を指定できます。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-168">Just like the encrypt version, you can specify a `-VolumeType` parameter `[All | OS | Data]` to decide what disks to decrypt.</span></span> <span data-ttu-id="ce0c5-169">指定されていない場合の既定値は `All` です。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-169">It defaults to `All` if not supplied.</span></span>

> [!WARNING]
> <span data-ttu-id="ce0c5-170">OS ディスクとデータ ディスクの両方が暗号化されている場合に Windows VM でデータ ディスク暗号化を無効にしようとしても、うまくいきません。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-170">Disabling data disk encryption on Windows VM when both OS and data disks have been encrypted doesn't work as expected.</span></span> <span data-ttu-id="ce0c5-171">代わりに、すべてのディスクの暗号化を無効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-171">You must disable encryption on all disks instead.</span></span>

<span data-ttu-id="ce0c5-172">これらのいくつかのコマンドを新しい VM で試してみましょう。</span><span class="sxs-lookup"><span data-stu-id="ce0c5-172">Let's try some of these commands out on a new VM.</span></span>