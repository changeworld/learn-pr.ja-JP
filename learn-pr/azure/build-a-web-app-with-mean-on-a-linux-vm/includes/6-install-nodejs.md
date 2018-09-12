ここでは、Web アプリケーションから MongoDB データ ストアに通信します。 MEAN スタックの場合、これは Node.js オープンソース JavaScript ランタイムのインストールを意味します。 Node.js は、Web アプリケーションとそのコンテンツのサーバー側ホストとして機能します。

## <a name="nodejs-versions"></a>Node.js のバージョン

Node.js には、使用できるお勧めのバージョンが 2 つあります。

- **Long Term Support (LTS)** - 運用環境のほとんどのユーザーに推奨されるバージョンです。
- **Current** - 最新の機能が含まれるバージョンですが、リリース サイクルの間に重大な変更が導入される場合があります。

このモジュールでは、Node.js LTS、バージョン 8.11.4 (本稿執筆時点) を使用します。

## <a name="how-to-install-nodejs"></a>Node.js のインストール方法

Node.js は、ほとんどのプラットフォームにインストールできます。 Ubuntu Linux に引き続き MEAN スタック コンポーネントをインストールする予定です。 他のオペレーティング システムで Node.js をインストールする方法の詳細については、「[Node.js instructions for installing it via various package managers (パッケージ マネージャーを使用した Node.js のインストール)](https://nodejs.org/en/download/package-manager/)」を参照してください。

Ubuntu Linux では、**apt-get** パッケージ マネージャーを使用して Node.js をインストールします。