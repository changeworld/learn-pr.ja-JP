あなたは、世界的自動車レース会社に、監視および Web プラットフォーム全体を現代化するために雇われました。 会社は、既存の Linux サーバーを、最新のアーキテクチャ トレンドを活用するさまざまなクラウド ベースのインフラストラクチャに置き換えることを決定しました。 システムの一部は、Azure Functions を使用して Azure のサーバーレス プラットフォーム上で実行され、リアルタイムのレース データを処理して、統計情報、レース データ、および他の関連する分析済み情報を、データベースのクラスターにプッシュします。 会社は、前年に書き換えられたばかりの既存の Web サイトを残しながら、それをこの最新のデータ ストリームに接続することを望んでいます。

Web サイトは Linux を使用して Apache 上で実行しており、既に稼働しているため、あなたは Azure 仮想マシンを利用して Azure に直接移動することにします。 そうすれば、Web サイトはデータにアクセスでき、あなたの作業量は最低限で済みます。

## <a name="learning-objectives"></a>学習の目的

このモジュールでは、次のことを行います。

- Azure で仮想マシンに利用可能なオプションについて理解します。
- Azure portal を使用して Linux 仮想マシンを作成します。
- SSH を使用して実行中の Linux 仮想マシンに接続します。
- ソフトウェアをインストールし、Azure portal を使用して VM 上でネットワーク構成を変更します。