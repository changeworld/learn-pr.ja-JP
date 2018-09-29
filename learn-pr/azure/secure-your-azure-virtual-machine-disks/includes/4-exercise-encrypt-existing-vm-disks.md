<span data-ttu-id="529cf-101">新興企業の財務管理アプリケーションを開発しているものとします。</span><span class="sxs-lookup"><span data-stu-id="529cf-101">Suppose you are developing a financial management application for new business startups.</span></span> <span data-ttu-id="529cf-102">顧客のすべてのデータが保護されるようにするため、このアプリケーションをホストするサーバー上のすべての OS ディスクとデータ ディスクに Azure Disk Encryption (ADE) を実装することにしました。</span><span class="sxs-lookup"><span data-stu-id="529cf-102">You want to ensure that all your customers' data is secured, so you have decided to implement Azure Disk Encryption (ADE) across all OS and data disks on the servers that will host this application.</span></span> <span data-ttu-id="529cf-103">コンプライアンス要件の一部として、独自の暗号化キーの管理も自分で行う必要があります。</span><span class="sxs-lookup"><span data-stu-id="529cf-103">As part of your compliance requirements, you also need to be responsible for your own encryption key management.</span></span>

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

<span data-ttu-id="529cf-104">このユニットでは、既存の仮想マシン上のディスクを暗号化し、独自の Azure Key Vault を使用して暗号化キーを管理します。</span><span class="sxs-lookup"><span data-stu-id="529cf-104">In this unit, you'll encrypt disks on an existing virtual machine, and manage the encryption keys using your own Azure Key Vault.</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="529cf-105">環境を準備する</span><span class="sxs-lookup"><span data-stu-id="529cf-105">Prepare the environment</span></span>

<span data-ttu-id="529cf-106">まず、Azure 仮想マシンで新しい Windows VM をデプロイします。</span><span class="sxs-lookup"><span data-stu-id="529cf-106">We'll start by deploying a new Windows VM in an Azure Virtual Machine.</span></span>

### <a name="deploy-windows-vm"></a><span data-ttu-id="529cf-107">Windows VM をデプロイする</span><span class="sxs-lookup"><span data-stu-id="529cf-107">Deploy Windows VM</span></span>

<span data-ttu-id="529cf-108">Azure PowerShell を使用して、新しい Windows 仮想マシンを作成してデプロイします。</span><span class="sxs-lookup"><span data-stu-id="529cf-108">Use the Azure PowerShell to create and deploy a new Windows virtual machine.</span></span>

1. <span data-ttu-id="529cf-109">まず、新しいリソースを配置する場所を決定します。</span><span class="sxs-lookup"><span data-stu-id="529cf-109">Start by deciding where to place the new resources.</span></span> <span data-ttu-id="529cf-110">次のリストから、自分に近い場所を選択します。</span><span class="sxs-lookup"><span data-stu-id="529cf-110">Select a location near you from the following list.</span></span>

    <span data-ttu-id="529cf-111"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]</span><span class="sxs-lookup"><span data-stu-id="529cf-111"><!-- Resource selection --> [!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]</span></span>
    

1. <span data-ttu-id="529cf-112">選択した場所を保持する PowerShell 変数を定義します。</span><span class="sxs-lookup"><span data-stu-id="529cf-112">Define a PowerShell variable to hold the selected location.</span></span> <span data-ttu-id="529cf-113">ここでは "米国東部" として定義されています。これを任意の場所に変更します。</span><span class="sxs-lookup"><span data-stu-id="529cf-113">It's defined as "East US" here, change it to your preferred location.</span></span>

    ```powershell
    $location = "eastus"
    ```
    
    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

1. <span data-ttu-id="529cf-114">次に、_リソース グループ_と VM の_名前_を取り込むためのさらに便利な変数をいくつか定義します。</span><span class="sxs-lookup"><span data-stu-id="529cf-114">Next, define a few more convenient variables to capture the _name_ of the VM and the _resource group_.</span></span> <span data-ttu-id="529cf-115">ここでは事前に作成されたリソース グループを使用することに注意してください。通常は、`New-AzureRmResourceGroup` を使用してサブスクリプションで_新しい_リソース グループを作成します。</span><span class="sxs-lookup"><span data-stu-id="529cf-115">Note that we are using the pre-created resource group here, normally you would create a _new_ resource group in your subscription using `New-AzureRmResourceGroup`.</span></span>

    ```powershell
    $vmName = "fmdata-vm01"
    $rgName = "<rgn>[sandbox Resource Group]</rgn>"
    ```
    
1. <span data-ttu-id="529cf-116">`New-AzureRmVm` を使用して、新しい仮想マシンを作成します。</span><span class="sxs-lookup"><span data-stu-id="529cf-116">Use `New-AzureRmVm` to create a new virtual machine.</span></span>
    
    ```powershell
    New-AzureRmVm `
        -ResourceGroupName $rgName `
        -Name $vmName `
        -Location $location `
        -OpenPorts 3389
    ```
    
    <span data-ttu-id="529cf-117">Cloud Shell でプロンプトが表示されたら、VM 用のユーザー名とパスワードを入力します。</span><span class="sxs-lookup"><span data-stu-id="529cf-117">Enter a username and password for the VM when you are prompted by the Cloud Shell.</span></span> <span data-ttu-id="529cf-118">これは、VM 用に作成される初期アカウントとして使用されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-118">This will be used as the initial account created for the VM.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="529cf-119">一連のオプションを指定しなかったため、このコマンドではいくつかの既定値が使用されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-119">This command will use some defaults since we didn't supply a bunch of options.</span></span> <span data-ttu-id="529cf-120">つまり、サイズが _Standard_DS1_v2_ の _Windows 2016 Server_ が作成されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-120">Specifically, this will create a _Windows 2016 Server_ image with the size to _Standard_DS1_v2_.</span></span> <span data-ttu-id="529cf-121">VM サイズを指定する場合、Basic レベルの VM では ADE がサポートされないことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="529cf-121">Remember that the Basic tier VMs do not support ADE if you decide to specify the VM size.</span></span>

1. <span data-ttu-id="529cf-122">VM のデプロイが完了したら、変数に VM の詳細を取り込みます。</span><span class="sxs-lookup"><span data-stu-id="529cf-122">Once the VM finishes deploying, capture the VM details in a variable.</span></span> <span data-ttu-id="529cf-123">この変数を使用して、作成内容を確認できます。</span><span class="sxs-lookup"><span data-stu-id="529cf-123">You can use this variable to explore what was created.</span></span>

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```
    
1. <span data-ttu-id="529cf-124">VM に OS ディスクが接続されていることがわかります。</span><span class="sxs-lookup"><span data-stu-id="529cf-124">You can see the OS disk attached to the VM:</span></span>

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
        
1. <span data-ttu-id="529cf-125">OS ディスク (およびすべてのデータ ディスク) の現在の暗号化の状態を確認します。</span><span class="sxs-lookup"><span data-stu-id="529cf-125">Check the current status of encryption on the OS disk (and any data disks).</span></span>

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  `
        -ResourceGroupName $rgName `
        -VMName $vmName
    ```

    <span data-ttu-id="529cf-126">ご覧のとおり、ディスクは現在_暗号化されていません_。</span><span class="sxs-lookup"><span data-stu-id="529cf-126">As you can see the disks are current _unencrypted_.</span></span> 

    ```output
    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : NotEncrypted
    OsVolumeEncryptionSettings :
    ProgressMessage            : No Encryption extension or metadata found on the VM
    ```
    
<span data-ttu-id="529cf-127">これを変更してみましょう。</span><span class="sxs-lookup"><span data-stu-id="529cf-127">Let's change that.</span></span>
    
## <a name="encrypt-the-vm-disks-with-azure-disk-encryption"></a><span data-ttu-id="529cf-128">Azure Disk Encryption で VM ディスクを暗号化する</span><span class="sxs-lookup"><span data-stu-id="529cf-128">Encrypt the VM disks with Azure Disk Encryption</span></span>

<span data-ttu-id="529cf-129">このデータを保護する必要があるため、ディスクを暗号化してみます。</span><span class="sxs-lookup"><span data-stu-id="529cf-129">We need to protect this data, so let's encrypt the disks.</span></span> <span data-ttu-id="529cf-130">次のようないくつかの手順を行う必要があることを思い出してください。</span><span class="sxs-lookup"><span data-stu-id="529cf-130">Recall that there are several steps we need to perform:</span></span>

1. <span data-ttu-id="529cf-131">キー コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="529cf-131">Create a key vault.</span></span>
1. <span data-ttu-id="529cf-132">ディスク暗号化をサポートするようにキー コンテナーを設定します。</span><span class="sxs-lookup"><span data-stu-id="529cf-132">Set the key vault up to support disk encryption.</span></span>
1. <span data-ttu-id="529cf-133">キー コンテナーに格納されているキーを使用して、VM ディスクを暗号化するように Azure に指示します。</span><span class="sxs-lookup"><span data-stu-id="529cf-133">Tell Azure to encrypt the VM disks using the key stored in the Key Vault.</span></span>

> [!TIP]
> <span data-ttu-id="529cf-134">ここでは手順を個別に見ていきますが、ご自分のサブスクリプションでこのタスクを実行するときは、このモジュールの「まとめ」でリンクされている便利な PowerShell スクリプトを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="529cf-134">We're going to walk through the steps individually, but when you're doing this task in your own subscription, you can use a handy PowerShell script which is linked in the Summary of this module.</span></span>

### <a name="create-a-key-vault"></a><span data-ttu-id="529cf-135">キー コンテナーを作成する</span><span class="sxs-lookup"><span data-stu-id="529cf-135">Create a key vault</span></span>

<span data-ttu-id="529cf-136">Azure Key Vault を作成するには、サブスクリプションでサービスを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="529cf-136">To create an Azure Key Vault, we need to enable the service in our subscription.</span></span> <span data-ttu-id="529cf-137">これは 1 回限りの要件です。</span><span class="sxs-lookup"><span data-stu-id="529cf-137">This is a one-time requirement.</span></span>

> [!TIP]
> <span data-ttu-id="529cf-138">サブスクリプションによっては、`Register-AzureRmResourceProvider` コマンドレットを使用して **Microsoft.KeyVault** プロバイダーを有効にする必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="529cf-138">Depending on your subscription, you might need to enable the **Microsoft.KeyVault** provider with the `Register-AzureRmResourceProvider` cmdlet.</span></span> <span data-ttu-id="529cf-139">Azure サンドボックス サブスクリプションではこれは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="529cf-139">This is not necessary in the Azure sandbox subscription.</span></span>

1. <span data-ttu-id="529cf-140">新しいキー コンテナーの名前を決めます。</span><span class="sxs-lookup"><span data-stu-id="529cf-140">Decide on a name for your new key vault.</span></span> <span data-ttu-id="529cf-141">この名前は一意である必要があります。数字、文字、ダッシュで構成された 3 から 24 文字の範囲で設定できます。</span><span class="sxs-lookup"><span data-stu-id="529cf-141">It must be unique and can be between 3 and 24 characters, composed of numbers, letters, and and dashes.</span></span> <span data-ttu-id="529cf-142">末尾に乱数をいくつか追加して、以下の "1234" を置き換えてみてください。</span><span class="sxs-lookup"><span data-stu-id="529cf-142">Try adding some random numbers to the end, replacing the "1234" below.</span></span>

    ```powershell
    $keyVaultName = "mvmdsk-kv-1234"
    ```
        
1. <span data-ttu-id="529cf-143">`New-AzureRmKeyVault` を使用して Azure Key Vault を作成します。</span><span class="sxs-lookup"><span data-stu-id="529cf-143">Create an Azure Key Vault with `New-AzureRmKeyVault`.</span></span>
    - <span data-ttu-id="529cf-144">ご自身の VM と同じリソース グループ_および_場所に配置されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="529cf-144">Make sure it's placed in the same resource group _and_ location as your VM.</span></span>
    - <span data-ttu-id="529cf-145">ディスクの暗号化で使用するために Key Vault を有効にします。</span><span class="sxs-lookup"><span data-stu-id="529cf-145">Enable the Key Vault for use with disk encryption.</span></span> 
    - <span data-ttu-id="529cf-146">一意の Key Vault 名を指定します。</span><span class="sxs-lookup"><span data-stu-id="529cf-146">Specify a unique Key Vault name.</span></span>

    ```powershell
    New-AzureRmKeyVault -VaultName $keyVaultName `
        -Location $location `
        -ResourceGroupName $rgName `
        -EnabledForDiskEncryption
    ```

    <span data-ttu-id="529cf-147">このコマンドから、アクセス権を持つユーザーがいないことを示す警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-147">You will get a warning from this command about no users having access.</span></span>

    ```output
    WARNING: Access policy is not set. No user or application have access permission to use this vault. This can happen if the vault was created by a service principal. Please use Set-AzureRmKeyVaultAccessPolicy to set access policies.
    ```

    <span data-ttu-id="529cf-148">ここでは VM 用の暗号化キーを格納するコンテナーを使用するだけで、ユーザーがこのデータにアクセスする必要はないため、問題はありません。</span><span class="sxs-lookup"><span data-stu-id="529cf-148">This is ok since we are just using the vault to store the encryption keys for the VM and users won't need to access this data.</span></span>

### <a name="encrypt-the-disk"></a><span data-ttu-id="529cf-149">ディスクを暗号化する</span><span class="sxs-lookup"><span data-stu-id="529cf-149">Encrypt the disk</span></span>

<span data-ttu-id="529cf-150">ディスクを暗号化する準備はほとんどできています。</span><span class="sxs-lookup"><span data-stu-id="529cf-150">We are almost ready to encrypt the disks.</span></span> <span data-ttu-id="529cf-151">暗号化する前に、バックアップの作成に関する警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-151">Before we do, a warning about creating backups.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="529cf-152">運用システムを使用している場合は、Azure Backup を使用するか、スナップショットを作成して、マネージド ディスクのバックアップを作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="529cf-152">If this were a production system, we would need to perform a backup of the managed disks - either using Azure Backup, or by creating a snapshot.</span></span> <span data-ttu-id="529cf-153">スナップショットは Azure portal で、またはコマンドラインを使用して作成できます。</span><span class="sxs-lookup"><span data-stu-id="529cf-153">You can create snapshots in the Azure portal, or through the command line.</span></span> <span data-ttu-id="529cf-154">PowerShell で、`New-AzureRmSnapshot` コマンドレットを使用します。</span><span class="sxs-lookup"><span data-stu-id="529cf-154">In PowerShell, use the `New-AzureRmSnapshot` cmdlet.</span></span> <span data-ttu-id="529cf-155">これはシンプルな演習であり、完了時にこのデータを破棄するため、この手順はスキップします。</span><span class="sxs-lookup"><span data-stu-id="529cf-155">Since this is a simple exercise and we're going to throw this data away when you're done, we're going to skip this step.</span></span> 

1. <span data-ttu-id="529cf-156">まず、Key Vault の情報を保持する変数を定義します。</span><span class="sxs-lookup"><span data-stu-id="529cf-156">Start by defining a variable to hold the Key Vault information.</span></span>

    ```powershell
    $keyVault = Get-AzureRmKeyVault `
        -VaultName $keyVaultName `
        -ResourceGroupName $rgName
    ```

1. <span data-ttu-id="529cf-157">次に、`Set-AzureRmVmDiskEncryptionExtension` コマンドレットを使用して、VM ディスクを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="529cf-157">Then, use the `Set-AzureRmVmDiskEncryptionExtension` cmdlet to encrypt the VM disks.</span></span>
    - <span data-ttu-id="529cf-158">`VolumeType` パラメーターを使用して、暗号化するディスク (_[すべて]_ | _[OS]_ | _[データ]_) を指定できます。</span><span class="sxs-lookup"><span data-stu-id="529cf-158">The `VolumeType` parameter allows you to specify which disks to encrypt: [_All_ | _OS_ | _Data_].</span></span> <span data-ttu-id="529cf-159">既定では _[すべて]_ に設定されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-159">It will default to _All_.</span></span> <span data-ttu-id="529cf-160">Linux の場合、データ ディスクを暗号化できるのは一部のディストリビューションのみとなります。</span><span class="sxs-lookup"><span data-stu-id="529cf-160">You can only encrypt data disks for some distributions of Linux.</span></span>
    - <span data-ttu-id="529cf-161">マネージド ディスクに `SkipVmBackup` フラグを指定する必要があります。そうしないと、スナップショットがないため、コマンドは失敗します。</span><span class="sxs-lookup"><span data-stu-id="529cf-161">You have to supply the `SkipVmBackup` flag for managed disks or the command will fail because there is no snapshot.</span></span>

    ```powershell
    Set-AzureRmVmDiskEncryptionExtension `
        -ResourceGroupName $rgName `
        -VMName $vmName `
        -VolumeType All `
        -DiskEncryptionKeyVaultId $keyVault.ResourceId `
        -DiskEncryptionKeyVaultUrl $keyVault.VaultUri `
        -SkipVmBackup
    ```

1. <span data-ttu-id="529cf-162">このコマンドレットでは、VM をオフラインにする必要があることと、タスクが完了するまで数分かかる場合があることを示す警告が表示されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-162">The cmdlet will warn you that the VM must be taken offline, and that the task may take several minutes to complete.</span></span> <span data-ttu-id="529cf-163">先に進んで、作業を続行します。</span><span class="sxs-lookup"><span data-stu-id="529cf-163">Go ahead and let it continue.</span></span>

    ```output
    Enable AzureDiskEncryption on the VM
    This cmdlet prepares the VM and enables encryption which may reboot the machine and takes 10-15 minutes to
    finish. Please save your work on the VM before confirming. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    ```
    
1. <span data-ttu-id="529cf-164">完了したら、暗号化の状態をもう一度確認します。</span><span class="sxs-lookup"><span data-stu-id="529cf-164">Once it's complete, check the encryption status again.</span></span>

    ```powershell
    Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
    ```

    <span data-ttu-id="529cf-165">この時点で OS ディスクが暗号化されているはずです。</span><span class="sxs-lookup"><span data-stu-id="529cf-165">Now the OS disk should be encrypted.</span></span> <span data-ttu-id="529cf-166">Windows に表示される、接続されたデータ ディスクもすべて暗号化されます。</span><span class="sxs-lookup"><span data-stu-id="529cf-166">Any attached data disks that are visible to Windows will also be encrypted.</span></span>

    ```output
    OsVolumeEncrypted          : Encrypted
    DataVolumesEncrypted       : NoDiskFound
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : Provisioning succeeded
    ```

> [!NOTE]        
> <span data-ttu-id="529cf-167">暗号化後に追加された新しいディスクは、自動的に暗号化_されません_。</span><span class="sxs-lookup"><span data-stu-id="529cf-167">New disks added after encryption will _not_ be automatically encrypted.</span></span> <span data-ttu-id="529cf-168">`Set-AzureRmVMDiskEncryptionExtension` コマンドレットを再実行して、新しいディスクを暗号化することができます。</span><span class="sxs-lookup"><span data-stu-id="529cf-168">You can re-run the `Set-AzureRmVMDiskEncryptionExtension` cmdlet to encrypt new disks.</span></span> <span data-ttu-id="529cf-169">既にディスクが暗号化されている VM にディスクを追加する場合は、必ず、新しいシーケンス番号を指定してください。</span><span class="sxs-lookup"><span data-stu-id="529cf-169">Make sure to provide a new sequence number if you add disks to a VM that has already had disks encrypted.</span></span> <span data-ttu-id="529cf-170">また、オペレーティング システムに表示されないディスクは暗号化されません。ディスクを正しくパーティション分割し、書式設定し、マウントして Bitlocker 拡張機能で確認できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="529cf-170">In addition, disks that are not visible to the operating system will not be encrypted - the disk must be properly partitioned, formatted, and mounted to be seen by the Bitlocker extension.</span></span>