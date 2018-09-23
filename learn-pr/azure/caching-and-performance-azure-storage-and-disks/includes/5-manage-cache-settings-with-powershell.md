管理スクリプトを作成することは、ワークフローを最適化する強力な方法です。 共通する反復的なタスクを自動化することができます。 スクリプトは一度検証すれば毎回同じように実行されるため、エラーを削減できます。 前の演習では、VM の作成、データ ディスクの追加、キャッシュの変更をすべて Azure portal を通じて行いました。 さまざまなリージョンの多くの VM でこれらのタスクを繰り返す必要がある場合は、どうしたらよいでしょう。 それは Azure PowerShell で行うことができます。

> [!TIP]
> Azure PowerShell の詳細は、「**Automate Azure Tasks with PowerShell**」 (PowerShell で Azure タスクを自動化する) モジュールで扱います。 PowerShell のインストール、構成、使用に関する詳細は同モジュールでご確認ください。

## <a name="what-is-azure-powershell"></a>Azure PowerShell とは

Azure PowerShell は、Azure サブスクリプションに接続し、リソースを管理するためのプラットフォームに依存しないコマンドライン ツールです。 コマンドライン ツール サポートを提供する **PowerShell** と Azure で使用するコマンド ("コマンドレット" と呼ばれています) を提供する **AzureRM** の組み合わせになっています。 

Azure PowerShell には、Azure リソースのほとんどの側面を操作できるコマンドレットが用意されています。 リソース グループ、ストレージ、仮想マシン、Azure Active Directory、コンテナー、機械学習などを操作できます。 これらすべての詳細については他のトレーニング モジュールで扱います。

### <a name="powershell-cmdlets-for-managing-azure-disk-caching"></a>Azure ディスク キャッシュを管理するための PowerShell コマンドレット

Azure PowerShell には、VM とディスクの管理に役立つ固有のコマンドレットがあります。

|コマンド  | 説明 |
|---------|-------------|
| `Get-AzureRmVM`         | 仮想マシンのプロパティを取得します。       |
| `Update-AzureRmVM`      | Azure 仮想マシンの状態を更新します。  |
| `New-AzureRmDiskConfig` | 構成可能なディスク オブジェクトを作成します。             |
| `Add-AzureRmVMDataDisk` | データ ディスクを仮想マシンに追加します。          |

以上のコマンドで、Azure portal で行ったあらゆるタスクを実行できます。 それでは VM で試してみましょう。
