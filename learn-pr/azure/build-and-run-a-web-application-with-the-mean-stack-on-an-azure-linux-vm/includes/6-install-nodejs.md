<span data-ttu-id="be71b-101">ここでは、Web アプリケーションから MongoDB データ ストアに通信します。</span><span class="sxs-lookup"><span data-stu-id="be71b-101">We will be talking to our MongoDB data store from a web application.</span></span> <span data-ttu-id="be71b-102">MEAN スタックの場合、これは Node.js オープンソース JavaScript ランタイムのインストールを意味します。</span><span class="sxs-lookup"><span data-stu-id="be71b-102">For the MEAN stack, this means installing the Node.js open-source JavaScript runtime.</span></span> <span data-ttu-id="be71b-103">Node.js は、Web アプリケーションとそのコンテンツのサーバー側ホストとして機能します。</span><span class="sxs-lookup"><span data-stu-id="be71b-103">Node.js will act as our server-side host for our web application and its content.</span></span>

## <a name="what-must-be-installed"></a><span data-ttu-id="be71b-104">インストールが必要なもの</span><span class="sxs-lookup"><span data-stu-id="be71b-104">What must be Installed?</span></span>

<span data-ttu-id="be71b-105">Node.js には推奨される 2 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="be71b-105">There are two recommended versions of Node.js available:</span></span>

- <span data-ttu-id="be71b-106">**Long Term Support (LTS)** - 運用環境のほとんどのユーザーに推奨されるバージョンです</span><span class="sxs-lookup"><span data-stu-id="be71b-106">**Long Term Support (LTS)** - a version recommended for most users, and for production environments</span></span>
- <span data-ttu-id="be71b-107">**Current** - 最新の機能が含まれるバージョンですが、リリース サイクルの間に重大な変更が導入される場合があります。</span><span class="sxs-lookup"><span data-stu-id="be71b-107">**Current** - a version that contains the latest features, but can introduce breaking changes between release cycles.</span></span>

<span data-ttu-id="be71b-108">このモジュールでは、Node.js LTS、バージョン 8.11.4 (本稿執筆時点) を使用します。</span><span class="sxs-lookup"><span data-stu-id="be71b-108">In this module we are going to use Node.js LTS, version 8.11.4 as of this writing.</span></span>

## <a name="how-to-install-nodejs"></a><span data-ttu-id="be71b-109">Node.js のインストール方法</span><span class="sxs-lookup"><span data-stu-id="be71b-109">How to install Node.js</span></span>

<span data-ttu-id="be71b-110">Node.js はほとんどのプラットフォームでインストールできますが、引き続き、MEAN スタック コンポーネントを Ubuntu Linux 上でインストールしていきます。</span><span class="sxs-lookup"><span data-stu-id="be71b-110">Node.js can be installed on most platforms, but we will continue to install MEAN stack components on Ubuntu Linux.</span></span> <span data-ttu-id="be71b-111">他のオペレーティング システムでの Node.js のインストール方法の詳細については、「[Node.js instructions for installing it via various package managers](https://Node.js.org/en/download/package-manager/)」(パッケージ マネージャーを使用した Node.js のインストール) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="be71b-111">For information on installing Node.js on other operating systems, check out the [Node.js instructions for installing it via various package managers](https://Node.js.org/en/download/package-manager/).</span></span>

<span data-ttu-id="be71b-112">Ubuntu Linux では、**apt-get** パッケージ マネージャーを使用して Node.js をインストールします。</span><span class="sxs-lookup"><span data-stu-id="be71b-112">On Ubuntu Linux, you use the **apt-get** package manager to install Node.js.</span></span>
