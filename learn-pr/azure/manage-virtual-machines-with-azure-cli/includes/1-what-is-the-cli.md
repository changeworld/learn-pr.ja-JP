Azure Virtual Machines からマネージド Web サイトまでの大規模な Azure リソース セットを管理することは、IT 部門ではよくあることです。

1 回限りのタスクでは Azure portal を簡単に使用できますが、複数のものを作成、変更、または削除する必要がある場合、さまざまなブレード間を移動するのに時間がかかります。 ここでコマンド ラインが役立ちます。コマンドを迅速かつ効率的に発行し、さらにスクリプトを使用して反復的なタスクを実行することもできます。 Azure では、Azure PowerShell と Azure CLI という 2 つの異なるコマンド ライン ツールを使用することができます。

これらのツールのいずれかを使用して、スクリプトを記述してクラウド サービスの状態を確認したり、新しい構成をデプロイしたりできます。また、ファイアウォールでポートを開いたり、仮想マシンに接続して設定を変更したりすることができます。 Windows 管理者は Azure PowerShell を好む傾向にありますが、開発者と Linux 管理者は多くの場合、Azure CLI を使用します。

このモジュールでは、Azure CLI を使用して、Azure でホストされている仮想マシンを作成および管理することに重点を置きます。 Azure CLI の概要、そのインストール方法、および Azure サブスクリプションの操作方法については、**CLI を使用した Azure サービスの制御**に関するトレーニング モジュールを確認してください。

## <a name="what-is-the-azure-cli"></a>Azure CLI とは

Azure CLI は、Azure リソースを管理するための Microsoft のクロスプラットフォーム コマンド ライン ツールです。 このツールは macOS、Linux、および Windows で利用でき、ブラウザーで [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) を使って利用することもできます。

Azure CLI は、コマンド ラインからまたはスクリプトで仮想マシンやディスクなどの Azure リソースを管理するのに役立ちます。 それでは作業を開始し、Azure Virtual Machines でどのようなことができるかを見ていきましょう。

## <a name="learning-objectives"></a>学習の目的

このモジュールでは、次のことを行います。

- Azure CLI を使用して仮想マシンを作成します
- Azure CLI を使用して仮想マシンのサイズを変更します。
- Azure CLI を使用して基本的な管理タスクを実行します。
- SSH と Azure CLI を使用して実行中の VM に接続します。

## <a name="prerequsites"></a>前提条件

- **CLI を使用した Azure サービスの制御**に関するモジュールで得られる Azure CLI ツールの基本的な理解。