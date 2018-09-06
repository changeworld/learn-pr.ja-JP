ここでは、Web アプリケーションから MongoDB データ ストアに通信します。 MEAN スタックの場合、これは Node.js オープンソース JavaScript ランタイムのインストールを意味します。 Node.js は、Web アプリケーションとそのコンテンツのサーバー側ホストとして機能します。

## <a name="what-must-be-installed"></a>インストールが必要なもの

Node.js には推奨される 2 つのバージョンがあります。

- **Long Term Support (LTS)** - 運用環境のほとんどのユーザーに推奨されるバージョンです
- **Current** - 最新の機能が含まれるバージョンですが、リリース サイクルの間に重大な変更が導入される場合があります。

このモジュールでは、Node.js LTS、バージョン 8.11.4 (本稿執筆時点) を使用します。

## <a name="how-to-install-nodejs"></a>Node.js のインストール方法

Node.js はほとんどのプラットフォームでインストールできますが、引き続き、MEAN スタック コンポーネントを Ubuntu Linux 上でインストールしていきます。 他のオペレーティング システムでの Node.js のインストール方法の詳細については、「[Node.js instructions for installing it via various package managers](https://Node.js.org/en/download/package-manager/)」(パッケージ マネージャーを使用した Node.js のインストール) を参照してください。

Ubuntu Linux では、**apt-get** パッケージ マネージャーを使用して Node.js をインストールします。
