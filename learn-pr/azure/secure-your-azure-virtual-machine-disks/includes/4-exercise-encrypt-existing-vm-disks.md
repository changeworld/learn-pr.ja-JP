新興企業の財務管理アプリケーションを開発しているものとします。 顧客のすべてのデータが保護されるようにするため、このアプリケーションをホストするサーバー上のすべての OS ディスクとデータ ディスクに ADE を実装することにしました。 コンプライアンス要件の一部として、独自の暗号化キーの管理も自分で行う必要があります。

このユニットでは、既存の Windows VM 上のディスクを暗号化し、独自の Azure キー コンテナーを使用して暗号化キーを管理します。

> [!IMPORTANT] 
> この演習では、Azure PowerShell がお使いのコンピューターにインストールされていてるものとします。Azure PowerShell をインストールする方法については、「[PowerShellGet を使用した Windows への Azure PowerShell のインストール](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-6.7.0)」をご覧ください。

## <a name="prepare-the-environment"></a>環境を準備する

最初に、Windows VM を新しいリソース グループに展開した後、VM にデータ ディスクを追加します。

### <a name="deploy-windows-vm-using-azure-portal"></a>Azure portal を使用して Windows VM を展開する

ここでは、Azure portal を使用して Windows VM を作成および展開します。 最初に、VM の基本的な情報を定義します。

1. ブラウザーで [Azure portal](http://portal.azure.com) に移動し、通常の資格情報でサインインします。

1. サイドバーで **[Virtual Machines]** をクリックし、**[仮想マシンの作成]** をクリックします。

1. [Compute] ブレードの **[お勧め]** セクションで、**[Windows Server]** をクリックします。

1. **[Windows Server]** ブレードで、**[Windows Server 2016 Datacenter]** をクリックします。

1. **[Windows Server 2016 Datacenter]** ブレードで、**[作成]** をクリックします。

1. **[基本]** ブレードで、**[名前]** ボックスに「**moneyappsvr01**」と入力します。

1. **[ユーザー名]** ボックスと **[パスワード]** ボックスに、このサーバーの管理者アカウントの名前とパスワードを入力します。

1. **[サブスクリプション]** ボックスで Azure サブスクリプションを選択します。

1. **[リソース グループ]** で **[新規作成]** を選択し、ボックスに「**moneyapprg**」と入力します。

1. **[場所]** ドロップダウン リストで、近くのリージョンを選択します。

1. **[OK]** をクリックします。

### <a name="choose-a-size-for-the-vm-and-start-the-deployment"></a>VM のサイズを選択して展開を開始する

> [!IMPORTANT]
> Basic レベルの VM は、ADE をサポートしていないことを思い出してください

1. **[サイズの選択]** ブレードで、**[B1s]** などの **[Standard]** を選択してから、**[選択]** をクリックします。

1. **[設定]** ブレードの **[パブリック受信ポートを選択]** の一覧で **[RDP]** をクリックし、下にスクロールして **[OK]** をクリックします。

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

1. **[ディスク]** ブレードで、**[保存]** をクリックします。 データ ディスクの暗号化の状態が現在は **_[有効になっていません]_** であることに注意してください。

## <a name="configure-disk-encryption-pre-requisites"></a>ディスク暗号化の前提条件を構成する

次に、Azure Disk Encryption の前提条件構成スクリプトを使用して、ディスク暗号化のすべての前提条件を構成します。 このスクリプトは、VM と同じリージョンにキー コンテナーを作成して準備します。

### <a name="prepare-the-azure-disk-encryption-prerequisite-setup-script"></a>Azure Disk Encryption の前提条件セットアップ スクリプトを準備する

1. [Azure Disk Encryption 前提条件セットアップ スクリプト](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1)の GitHub ページに移動します。

1. GibHub ページで、**[Raw]\(未加工\)** をクリックします。

1. Ctrl + A キーを押してそのページのテキストすべてを選択し、Ctrl + C キーを押して、選択したテキストをクリップボードにコピーします。

1. お使いのコンピューターで **[スタート]** をクリックし、**[Windows PowerShell ISE]** を参照します。

1. **[Windows PowerShell ISE]** を右クリックし、**[管理者として実行]** をクリックします。

1. [Administrator: Windows PowerShell ISE]\(管理者: Windows PowerShell ISE\) ウィンドウで、**[表示]** をクリックし、**[Show Script Pane]\(スクリプト ウィンドウの表示\)** をクリックします。

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

### <a name="run-the-azure-disk-encryption-prerequisite-setup-script"></a>Azure Disk Encryption の前提条件セットアップ スクリプトを実行する

1. PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、**Enter** キーを押します。

   ```console
   cd <path to your folder containing ADEPrereqScript.ps1>
   ```

1. PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、**Enter** キーを押します。

   ```powershell
   Set-ExecutionPolicy Unrestricted
   ```

   **[実行ポリシーの変更]** ダイアログ ボックスが表示される場合は、**[すべてはい]** または **[はい]** (_[すべてはい]_ オプションが表示されない場合) をクリックします。

1. PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、**Enter** キーを押します。

   ```powershell
   Login-AzureRmAccount
   ```

1. Azure の資格情報を入力します。

1. **SubscriptionId** の文字列を選択して、クリップボードにコピーします。

1. PowerShell ISE で **[ファイル]** をクリックし、**[実行]** をクリックします。

1. コンソール ウィンドウの **[resourceGroupName:]** というプロンプトで、「**moneyapprg**」と入力して、**Enter** キーを押します。

1. コンソール ウィンドウの **[keyVaultName:]** というプロンプトで、「**moneyappkv**」と入力して、**Enter** キーを押します。

1. コンソール ウィンドウの **[location:]** というプロンプトで、VM の作成時に使用した場所を入力します。

1. コンソール ウィンドウの **[subscriptionId:]** というプロンプトで、お使いのサブスクリプション ID を貼り付けます。

1. **Moneyappkv** キー コンテナーが作成されます。 これが完了したら、(緑色の) 概要テキストを選択し、メモ帳にコピーします。

1. **Enter** キーを押して続行します。

1. コンソール ウィンドウの **[aadAppName:]** というプロンプトで、「**moneyapp**」と入力して、**Enter** キーを押します。

1. **moneyapp** Azure AD アプリケーションが作成されます。 これが完了したら、(緑色の) 概要テキストを選択し、メモ帳にコピーします。

1. **Enter** キーを押して続行します。

### <a name="encrypt-your-vm-disks-with-powershell"></a>PowerShell を使用して VM ディスクを暗号化する

OS ディスクとデータ ディスクの暗号化の状態を確認します。

1. PowerShell ISE コンソール ウィンドウで、次のコマンドを入力して、Enter キーを押します。

    ```powershell
    $vmName = 'moneyappsvr01'
    ```

    > [!NOTE]
    > VM 名は、一重引用符で囲む必要があります。

1. PowerShell ISE スクリプト ウィンドウで、次のコマンドを入力して、Enter キーを押します。

    ```powershell
    Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $resourceGroupName -VMName $vmName -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType All
    ```

1. **[Enable AzureDiskEncryption on the VM]\(VM で AzureDiskEncryption を有効にする\)** ダイアログ ボックスで **[はい]** をクリックし、暗号化が完了するまでに 10 ～ 15 分かかる場合があることを示すメッセージを確認します。

>[!IMPORTANT]
> コマンドが完了するまで待ってから、この演習を続けます。

### <a name="verify-the-encryption-status-of-your-vm-disks"></a>VM ディスクの暗号化の状態を確認する

Azure portal に切り替えて、**moneyappsvr01** の **[ディスク]** ブレードで、OS ディスクとデータ ディスクのディスク暗号化の状態が **[有効]** になっていることを確認します。
