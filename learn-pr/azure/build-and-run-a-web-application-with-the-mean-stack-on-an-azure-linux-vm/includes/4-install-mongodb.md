ほとんどのアプリケーションにはデータ ストアが必要です。 MongoDB (MEAN スタックの "M") は、最も人気のある NoSQL データ ストア ソリューションの 1 つであり、無料のオープン ソースです。 NoSQL データ ストアの場合、SQL Server や MySQL などのリレーショナル データベースのように、事前に定義された方法でデータを構築する必要はありません。

MongoDB では、厳格なデータ構造を必要としない JSON に似たドキュメントにデータが格納されます。 次に、MongoDB との対話には、JavaScript Object Notation (JSON) として送信されるクエリとコマンドを使用します。

## <a name="what-must-be-installed"></a>インストールが必要なもの

MongoDB には 2 つのバージョンがあります。

- MongoDB Community Server
- MongoDB Enterprise Server

このモジュールでは、Web アプリケーションのデータ ストアとして MongoDB Community Server (執筆時点でバージョン 3.6) を使用します。

## <a name="how-to-install-mongodb"></a>MongoDB をインストールする方法

MongoDB は Linux、macOS、Windows にインストールできます。 このモジュールでは Ubuntu Linux VM 上にインストールしますが、推奨されるパッケージ マネージャーは OS とディストリビューションによって異なります。 すべてのオペレーティング システムのインストール プロセスについては、[MongoDB のインストール ドキュメント](https://docs.mongodb.com/manual/administration/install-community/)を参照してください。

ここでは、Ubuntu VM にインストールするために **apt-get** パッケージ マネージャーを使用します。
