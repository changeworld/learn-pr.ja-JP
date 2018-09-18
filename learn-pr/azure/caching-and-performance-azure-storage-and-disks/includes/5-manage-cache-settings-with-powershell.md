管理スクリプトを作成することは、ワークフローを最適化する強力な方法です。 共通する反復的なタスクを自動化することができます。 スクリプトは一度検証すれば毎回同じように実行されるため、エラーを削減できます。 前の演習では、VM の作成、データ ディスクの追加、キャッシュの変更を、すべて Azure portal を通じて行いました。 さまざまなリージョンの多くの VM でこれらのタスクを繰り返す必要がある場合は、どうしたらよいでしょう。 Azure PowerShell を使用しましょう。

## <a name="what-is-azure-powershell"></a>Azure PowerShell とは
Azure PowerShell は、PowerShell に追加し、Azure サブスクリプションに接続してリソースを管理することができるモジュールです。 Azure PowerShell を使用するには、PowerShell が機能している必要があります。 PowerShell は、シェル ウィンドウ、コマンド解析などのサービスを提供します。 Azure PowerShell で、Azure 固有のコマンドが追加されます。

たとえば Azure PowerShell には、Azure サブスクリプション内に仮想マシンを作成する **New-AzureRmVM** コマンドがあります。 使用するには、PowerShell アプリケーションを起動し、次の例のようなコマンドを発行します。

```powershell
New-AzureRmVm `
    -ResourceGroupName "CrmTestingResourceGroup" `
    -Name "CrmUnitTests" `
    -Image "UbuntuLTS"
    ...
```

Azure PowerShell は、Azure Cloud Shell 経由のブラウザー内、または Linux、Mac、または Windows 上のローカル インストールという 2 つの方法で使用できます。

## <a name="what-are-powershell-cmdlets"></a>PowerShell コマンドレットとは

PowerShell コマンドは**コマンドレット**と呼ばれています。 コマンドレットは 1 つの機能を操作するコマンドです。 **コマンドレット**と用語は "小さなコマンド" を意味します。 慣例で、コマンドレットの作成者には、コマンドレットをシンプルにすること、目的を 1 つだけにすることが推奨されています。

基本の PowerShell 製品には、セッションやバックグラウンド ジョブなどの機能で使用できるコマンドレットが付属します。 他の機能を操作するコマンドレットを得るには、PowerShell インストールにモジュールを追加します。 たとえば、サードパーティ モジュールには、FTP と連動するモジュール、オペレーティング システムを管理するモジュール、ファイル システムにアクセスするモジュールなどがあります。

コマンドレットには、動詞-名詞の命名規則が採用されています。たとえば、**Get-Process**、**Format-Table**、**Start-Service** のようになります。 動詞の選択にも規則があります。たとえば、データの取得は "get"、データの挿入または更新は "set"、データの書式設定は "format"、宛先への直接出力は "out" になります。

コマンドレットの作成者には、コマンドレットごとにヘルプ ファイルを含めることが推奨されています。 **Get-Help** コマンドレットを実行すると、コマンドレットのヘルプ ファイルが表示されます。

```
Get-Help <cmdlet-name> -detailed
```
## <a name="what-is-azurerm"></a>AzureRM とは

**AzureRM** は、Azure 機能で使用できるコマンドレットのセットを含む Azure PowerShell モジュールの正式名称です (名前の中の **RM** は **Resource Manager** という意味です)。 数百単位のコマンドレットが含まれ、あらゆる Azure リソースのほぼすべての面を制御できます。 リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。

## <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>Azure ディスク キャッシュを管理するための PowerShell コマンドレット

Azure PowerShell には、VM とディスクの管理に役立つ固有のコマンドレットがあります。 

次の演習で使用するコマンドレットの一部を次の表に示します。

|コマンド  |説明  |
|---------|---------|
|`Get-AzureRmVM`     |  仮想マシンのプロパティを取得します。       |        $myVM
|`Update-AzureRmVM`     |  Azure 仮想マシンの状態を更新します。       |        
|`New-AzureRmDiskConfig`     |  構成可能なディスク オブジェクトを作成します。       |        
|`Add-AzureRmVMDataDisk`     |  データ ディスクを仮想マシンに追加します。   |      


次の演習でこれらのコマンドレットを使用して、VM でキャッシュを管理してみましょう。
