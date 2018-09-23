このユニットでは、引き続き、Linux 管理ツールを作成している会社の例を扱います。 Linux VM を使用して自社のソフトウェアを潜在顧客にテストしてもらう計画であることを思い出してください。 リソース グループの準備はできているので、次に VM を作成します。

会社では Linux の大規模なトレード ショーでブースを確保するための費用を支払いました。 デモンストレーション領域を設け、そこにはそれぞれ別々の Linux VM に接続された 3 台の端末を設置する計画です。 1 日の終わりに VM を削除して再作成します。そうすることで、VM は毎朝新しい状態で開始されます。 仕事の後、疲れているときに手動で VM を作成すると、間違いやすくなります。 VM の作成プロセスを自動化するための PowerShell スクリプトを記述します。

## <a name="write-a-script-that-creates-virtual-machines"></a>仮想マシンを作成するスクリプトを記述する

右側の Cloud Shell で次の手順に従って、次のスクリプトを記述します。

1. Cloud Shell 内のご自分のホーム フォルダーに切り替えます。

    ```powershell
    cd $HOME\clouddrive
    ```

1. **ConferenceDailyReset.ps1** という名前の新しいテキスト ファイルを作成します。

    ```powershell
    touch "./ConferenceDailyReset.ps1"
    ```

1. 統合エディターを開いて、**ConferenceDailyReset.ps1** ファイルを選択します。

    ```powershell
    code "./ConferenceDailyReset.ps1"
    ```
    > [!TIP]
    > vim、nano、および emacs のいずれかを使用する場合、統合された Cloud Shell ではこれらのエディターもサポートされています。

1. 変数の入力パラメーターをキャプチャすることから開始します。 次の行をスクリプトに追加します。

    ```powershell
    param([string]$resourceGroup)
    ```

    > [!NOTE]
    > 通常、`Connect-AzureRmAccount` を使用する資格情報を使用して Azure を認証する必要があります。これはスクリプトで行うことができます。 ただし、Cloud Shell 環境では、お客様は既に認証されているため、この認証は必要ありません。

1. VM の管理者アカウント用のユーザーとパスワードを求めるプロンプトを表示し、その結果を変数内に取り込みます。

    ```powershell
    $adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."
    ```

1. 3 回実行されるループを作成します。

    ```powershell
    For ($i = 1; $i -le 3; $i++) 
    {

    }
    ```

1. ループの本体で、各 VM の名前を作成し、それを変数内に格納してコンソールに出力します。

    ```powershell
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    ```

1. 次に、`$vmName` 変数を使用して VM を作成します。

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
   ```

1. ファイルを保存します。 エディターの右上隅にある [...] メニューを使用できます。 [保存] に対する共通のアクセス キーもあります。

完成したスクリプトは次のようになります。

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i
    Write-Host "Creating VM: " $vmName
    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a>スクリプトを実行する

[...] コンテキスト メニューからエディターを閉じます。

1. スクリプトを実行します。

    ```powershell
    .\ConferenceDailyReset.ps1 <rgn>[Sandbox resource group name]</rgn>
    ```
    
このスクリプトは、完了までに数分かかります。 完了したら、ご使用のリソース グループ内にあるリソースを確認することで、スクリプトが正常に実行されたことを確認します。

```powershell
Get-AzureRmResource -ResourceType Microsoft.Compute/virtualMachines
```

それぞれに一意の名前がある、3 つの VM が表示されます。

スクリプト パラメーターで指定したリソース グループ内に 3 つの VM を自動的に作成するスクリプトを記述しました。 このスクリプトは短くてシンプルですが、このスクリプトでは、ポータルを使用して手動で行うと完了するまで時間がかかるプロセスを自動化することができます。