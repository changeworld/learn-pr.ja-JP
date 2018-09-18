このユニットでは、引き続き、Linux 管理ツールを作成している会社の例を扱います。 Linux VM を使用して自社のソフトウェアを潜在顧客にテストしてもらう計画であることを思い出してください。 リソース グループの準備はできているので、次に VM を作成します。

会社では Linux の大規模なトレード ショーでブースを確保するための費用を支払いました。 デモンストレーション領域を設け、そこにはそれぞれ別々の Linux VM に接続された 3 台の端末を設置する計画です。 1 日の終わりに VM を削除して再作成します。そうすることで、VM は毎朝新しい状態で開始されます。 仕事の後、疲れているときに手動で VM を作成すると、間違いやすくなります。 VM の作成プロセスを自動化するための PowerShell スクリプトを記述します。

## <a name="write-a-script-that-creates-virtual-machines"></a>仮想マシンを作成するスクリプトを記述する

次の手順に従って、スクリプトを記述します。

1. **ConferenceDailyReset.ps1** という名前の新しいテキスト ファイルを作成します。

1. 変数内にパラメーターを取り込みます。

    ```powershell
    param([string]$resourceGroup)
    ```

1. ご自分の資格情報を使用して Azure に対する認証を受けます。

    ```powershell
    Connect-AzureRmAccount
    ```

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

1. ループの本体で、各 VM の名前を作成し、それを変数内に格納します。

    ```powershell
    $vmName = "ConferenceDemo" + $i
    ```

1. 次に、`$vmName` 変数を使用して VM を作成します。

   ```powershell
   New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
   ```

1. ファイルを保存します。

完成したスクリプトは次のようになります。

```powershell
param([string]$resourceGroup)

$adminCredential = Get-Credential -Message "Enter a username and password for the VM administrator."

Connect-AzureRmAccount

For ($i = 1; $i -le 3; $i++)
{
    $vmName = "ConferenceDemo" + $i

    New-AzureRmVm -ResourceGroupName $resourceGroup -Name $vmName -Credential $adminCredential -Location "East US" -Image UbuntuLTS
}
```

## <a name="execute-the-script"></a>スクリプトを実行する

PowerShell を起動し、スクリプト ファイルを保存してあるディレクトリに移動します。 スクリプトを実行するには、次のコマンドを実行します。

```powershell
.\ConferenceDailyReset.ps1 TrialsResourceGroup
```

スクリプトが終了するまでに数分かかる場合があります。 スクリプトが終了したら、正常に実行されたことを確認します。

<!---TODO: Update for sandbox?--->
1. ブラウザー内で、[Azure portal](https://portal.azure.com/?azure-portal=true) にサインインします。

1. 左側のナビゲーションで、**[リソース グループ]** をクリックします。

1. リソース グループの一覧で、**[TrialsResourceGroup]** をクリックします。 リソースの一覧に、新しく作成された VM とそれに関連するリソースが表示されます。

## <a name="summary"></a>まとめ
スクリプト パラメーターで指定したリソース グループ内に 3 つの VM を自動的に作成するスクリプトを記述しました。 このスクリプトは短くてシンプルですが、このスクリプトでは、ポータルを使用して手動で行うと完了するまで時間がかかるプロセスを自動化することができます。