この演習では、仮想マシンを作成した後、ポータルと Azure PowerShell を使用してそのサイズを変更します。

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-vm-with-powershell"></a>PowerShell を使用して VM を作成する

1. Azure PowerShell で `New-AzureRmVm` コマンドレットを使用して、VM を作成してみましょう。
    - **<rgn>[サンドボックス リソース グループ名]</rgn>** というリソース グループを使用します。
    - これに "my-test-vm" という名前を付けます。
    - **[サイズ]** パラメーターの _Standard_DS2_v2_ を渡します。
    - Azure サンドボックスで使用できる次のリストから、自分に近い場所を選択します。 コピーと貼り付けを使用する場合は、次のコマンド例の値を必ず変更してください。

        [!include[](../../../includes/azure-sandbox-regions-note.md)]

    - `Get-Credential` コマンドレットを使用し、次のように結果を `Credential` パラメーターに指定します。

       資格情報の入力を求められたら、ユーザー名として LocalAdmin、パスワードとして Adm1nPa$$word を使用します。

    ```powershell
    New-AzureRmVm `
        -ResourceGroupName <rgn>[sandbox resource group name]</rgn> `
        -Name my-test-vm `
        -Credential (Get-Credential) `
        -Size "Standard_DS2_v2" `
        -Location eastus
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


1. デプロイが完了するまで待ってから、演習を続行します。

## <a name="resize-using-the-portal"></a>ポータルを使用してサイズを変更する

1. サンドボックスをアクティブ化したときと同じアカウントを使用して、[サンドボックスの Azure portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) にサインインします。

1. 左側のバーから **[リソース グループ]** を選択します。

1. <rgn>[サンドボックス リソース グループ名]</rgn> というリソース グループを選択し、概要で **[my-test-vm]** 仮想マシン オブジェクトをクリックします。

1. 仮想マシンの **[概要]** では、VM の現在のサイズが、先ほど作成した _Standard DS2 v2 (2 vCPU、7 GB メモリ)_ になっていることに注目してください。

1. **[設定]** セクションで、**[サイズ]** を選択します。

1. **[サイズの選択]** ブレードで、**F2s_v2** というサイズを探してみてください。 VM は現在実行中であり、そのサイズが別のファミリのものであるため、利用可能なサイズのリストには表示されません。 利用可能な VM サイズをすべて表示するには、VM の停止が必要になる場合があります。

1. この VM の実行中に現在利用できるサイズを選択してみましょう。 引き続き **[サイズの選択]** ブレードで、_DS3_v2 Standard_ を選んでから **[選択]** をクリックします。 仮想マシンのサイズ変更についての通知に注目してください。

1. **[概要]** パネルで、VM のサイズが _Standard DS3 v2 (4 vCPU、14 GB メモリ)_ に変更されたことを確認します。

## <a name="resize-using-powershell"></a>PowerShell を使用してサイズを変更する

1. Azure portal で、上部のツール バーにある Cloud Shell ボタンをクリックして、Azure Cloud Shell を開きます。

    Cloud Shell ウィンドウの左上で、Cloud Shell が Bash ではなく確実に PowerShell を使用するように設定します。

1. 次のコマンドレットを使用して、利用可能な仮想マシン サイズのリストを取得します。

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    ```

1. 次のコマンドレットを使用して、仮想マシンのサイズを _DS2_v2_ に戻します。

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName <rgn>[sandbox resource group name]</rgn> -VMName my-test-vm
    $vm.HardwareProfile.VmSize = "Standard_DS2_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName <rgn>[sandbox resource group name]</rgn>
    ```

1. PowerShell コマンドが完了するのを待っている間に、**[my-test-vm]** ブレードの **[更新]** ボタンをクリックします。 仮想マシンが再起動し、サイズの変更が反映されることがわかります。

この演習では、仮想マシンを作成し、2 つの異なるツールを使用してサイズを変更しました。 覚えておくとよいヒントは、仮想マシンの実行中は目的のサイズを使用できない可能性があるということです。仮想マシンを停止すると、より多くのサイズを選択できます。