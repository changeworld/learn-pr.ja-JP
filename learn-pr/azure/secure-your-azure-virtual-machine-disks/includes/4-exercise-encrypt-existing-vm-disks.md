新興企業の財務管理アプリケーションを開発しているものとします。 このアプリケーションをホストするサーバー上のすべての OS とデータ ディスク間で Azure Disk Encryption (ADE) を実装することにしましたが、顧客のすべてのデータが保護されることを確認するには。 コンプライアンス要件の一部として、独自の暗号化キーの管理も自分で行う必要があります。

このユニットでは、既存の Windows Vm のディスクを暗号化し、Azure Key Vault を使用して暗号化キーを管理します。

> [!IMPORTANT] 
> この演習では、Azure PowerShell がコンピューターにインストールされていることを前提としています。 移動して[PowerShellGet での Windows での Azure PowerShell のインストール](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0)Azure PowerShell をインストールする方法についてはします。

## <a name="prepare-the-environment"></a>環境を準備する

新しいリソース グループ、Windows VM をデプロイすることで開始がされ、VM にデータ ディスクを追加します。

### <a name="deploy-windows-vm-using-the-azure-portal"></a>Azure portal を使用して Windows VM をデプロイします。

ここで作成して Windows VM をデプロイする Azure portal を使用します。 最初に、VM の基本的な情報を定義します。

1. ブラウザーで [Azure portal](http://portal.azure.com) に移動し、通常の資格情報でサインインします。

1. サイドバーで **[Virtual Machines]** をクリックし、**[仮想マシンの作成]** をクリックします。

1. [Compute] ブレードの **[お勧め]** セクションで、**[Windows Server]** をクリックします。

1. **[Windows Server]** ブレードで、**[Windows Server 2016 Datacenter]** をクリックします。

1. **[Windows Server 2016 Datacenter]** ブレードで、**[作成]** をクリックします。

1. **[基本]** ブレードで、**[名前]** ボックスに「**moneyappsvr01**」と入力します。

1. **[ユーザー名]** ボックスと **[パスワード]** ボックスに、このサーバーの管理者アカウントの名前とパスワードを入力します。

1. **[サブスクリプション]** ボックスで Azure サブスクリプションを選択します。

1. **リソース グループ**、**新規作成**です。 ボックスに「 **moneyapprg**します。

1. **[場所]** ドロップダウン リストで、近くのリージョンを選択します。

1. **[OK]** をクリックします。

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a>VM のサイズを選択して展開を開始する

> [!IMPORTANT]
> Basic レベルの Vm は、ADE をサポートされていないことに注意してください。

1. **サイズの選択**ブレードで、**標準**SKU など**B1s**します。 次に **[選択]** をクリックします。

1. **設定**ブレードで、**パブリック受信ポートを選択します。** 一覧で、 **RDP**。 下へスクロールし、をクリックして**OK**します。

1. **[作成]** ブレードで、**[作成]** をクリックします。

1. VM が展開されるまで待ってから、演習を続けます。

### <a name="add-a-data-disk-to-the-vm"></a>VM にデータ ディスクを追加する

1. 左側のメニューで **[すべてのリソース]** をクリックし、**moneyappsvr01** をクリックします。

1. **[仮想マシン]** ブレードの **[設定]** で、**[ディスク]** をクリックします。

1. **[ディスク]** ブレードで、OS ディスクの暗号化の状態が現在は **[有効になっていません]** であることを確認し、**[データ ディスクの追加]** をクリックします。

1. **[名前]** ボックスの一覧をクリックして、**[ディスクの作成]** をクリックします。

1. **[マネージド ディスクの作成]** ブレードで、**[名前]** ボックスに「**moneyappsvr01_data**」と入力します。

1. **[リソース グループ]** で **[既存のものを使用]** を選択し、一覧で **moneyapprg** を選択します。

1. **[作成]** をクリックします。

1. ディスクが作成されるまで待ってから、次に進みます。

1. **[ディスク]** ブレードで、**[保存]** をクリックします。 現在、データ ディスクの暗号化の状態のことに注意してください**有効になっていない**します。

## <a name="configure-disk-encryption-prerequisites"></a>ディスク暗号化の前提条件を構成します。

これで、すべてのディスク暗号化の前提条件を構成するのに Azure Disk Encryption の前提条件の構成スクリプトを使用します。 このスクリプトは作成し、VM と同じリージョン内のキー コンテナーを準備します。

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a>Azure Disk Encryption の前提条件のセットアップ スクリプトを準備します。

1. 移動して、 [Azure Disk Encryption の前提条件となるセットアップ スクリプト](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)GitHub ページ。

1. GibHub ページで、**[Raw]\(未加工\)** をクリックします。

1. ページで、すべてのテキストを選択し、Ctrl + C を使用して、ページ上のすべてのテキストをクリップボードにコピーする CTRL + A を使用します。

1. コンピューターには、次のようにクリックします。**開始**、を参照し、 **Windows PowerShell ISE**します。

1. 右クリックして**Windows PowerShell ISE**、 をクリック**管理者として実行**します。

1. Administrator: Windows PowerShell ISE ウィンドウで、をクリックして**ビュー**、順にクリックします**スクリプト ウィンドウに表示**。

1. コピーしたテキストをスクリプト ウィンドウに貼り付けます。

1. スクリプト ウィンドウで、次のコード ブロックを見つけます。

    ```powershell
    [Parameter(Mandatory = $false,
            HelpMessage="Name of the AAD application that will be used to write secrets to KeyVault. A new application with this name will be created if one doesn't exist. If this app already exists, pass aadClientSecret parameter to the script")]
    [ValidateNotNullOrEmpty()]
    [string]$aadAppName,
    ```
1. コード ブロックで、`$false` を `$true` に変更します。

1. **[ファイル]** をクリックし、**[名前を付けて保存]** をクリックして、スクリプトを保存するフォルダーに移動します。

1. **[ファイル名]** ボックスに「**ADEPrereqScript.ps1**」と入力し、**[保存]** をクリックします。

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a>Azure Disk Encryption の前提条件のセットアップ スクリプトを実行します。

1. PowerShell ISE でコンソール ウィンドウで、次のコマンドを入力およびキーを押して**Enter**:

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. PowerShell ISE でコンソール ウィンドウで、次のコマンドを入力およびキーを押して**Enter**:

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   **[実行ポリシーの変更]** ダイアログ ボックスが表示される場合は、**[すべてはい]** または **[はい]** (_[すべてはい]_ オプションが表示されない場合) をクリックします。

1. PowerShell ISE でコンソール ウィンドウで、次のコマンドを入力およびキーを押して**Enter**:

   ```powershell
   Login-AzureRmAccount
   ```

1. Azure の資格情報を入力します。

1. **SubscriptionId** の文字列を選択して、クリップボードにコピーします。

1. PowerShell ISE で次のようにクリックします。**ファイル**、 をクリックし、**実行**します。

1. コンソール ウィンドウで、 **resourceGroupName:** プロンプトで「 **moneyapprg**します。 キーを押します**Enter**します。

1. コンソール ウィンドウで、 **keyVaultName:** プロンプトで「 **moneyappkv**します。 キーを押します**Enter**します。

1. コンソール ウィンドウの **[location:]** というプロンプトで、VM の作成時に使用した場所を入力します。

1. コンソール ウィンドウの **[subscriptionId:]** というプロンプトで、お使いのサブスクリプション ID を貼り付けます。

1. **Moneyappkv** キー コンテナーが作成されます。 これが完了したら、(緑色の) 概要テキストを選択し、メモ帳にコピーします。

1. キーを押して**Enter**を続行します。

1. コンソール ウィンドウで、 **aadAppName:** プロンプトで「 **moneyapp**します。 キーを押します**Enter**します。

1. **moneyapp** Azure AD アプリケーションが作成されます。 これが完了したら、(緑色の) 概要テキストを選択し、メモ帳にコピーします。

1. キーを押して**Enter**を続行します。

### <a name="encrypt-your-vm-disks-with-powershell"></a>PowerShell を使用して VM ディスクを暗号化する

OS ディスクとデータ ディスクの暗号化の状態を確認します。

1. PowerShell ISE でコンソール ウィンドウで、次のコマンドを入力およびキーを押して**Enter**:

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > VM 名は、一重引用符で囲む必要があります。

1. PowerShell ISE のスクリプト ウィンドウで、次を入力しますコマンド、およびキーを押して**Enter**:。

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. **[Enable AzureDiskEncryption on the VM]\(VM で AzureDiskEncryption を有効にする\)** ダイアログ ボックスで **[はい]** をクリックし、暗号化が完了するまでに 10 ～ 15 分かかる場合があることを示すメッセージを確認します。

>[!IMPORTANT]
> この演習に進む前に、コマンドが完了するまで待機します。

### <a name="verify-the-encryption-status-of-your-vm-disks"></a>VM ディスクの暗号化の状態を確認する

Azure portal に切り替えます。 **ディスク**ブレード**moneyappsvr01**、OS とデータ ディスクのディスク暗号化の状態になっている**有効**します。
