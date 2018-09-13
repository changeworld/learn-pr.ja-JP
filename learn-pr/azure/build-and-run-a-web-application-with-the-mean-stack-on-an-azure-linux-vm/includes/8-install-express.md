<span data-ttu-id="7f332-101">MEAN スタックにおいて、**E** は Express コンポーネントを表します。</span><span class="sxs-lookup"><span data-stu-id="7f332-101">In the MEAN stack, the **E** represents the Express component.</span></span> <span data-ttu-id="7f332-102">Express は、Node.js 向けに構築された Web サーバーのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="7f332-102">Express is a web server framework that's built for Node.js.</span></span> <span data-ttu-id="7f332-103">要求のルーティングおよびヘルパー システムが処理されます。</span><span class="sxs-lookup"><span data-stu-id="7f332-103">It will handle our request routing and helper system.</span></span> <span data-ttu-id="7f332-104">Node.js ラインタイムでホストされる Web アプリケーションのビルドを簡素化するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="7f332-104">It's designed to simplify building web applications that are hosted on the Node.js runtime.</span></span> <span data-ttu-id="7f332-105">Express で主に行われるのはルーティングですが、Web アプリケーションで HTTP Cookie を処理したり、クエリ文字列データを要求したりする場合にも役立ちます。</span><span class="sxs-lookup"><span data-stu-id="7f332-105">While the core of Express is routing, it also helps web applications work with HTTP cookies and request query string data.</span></span>

## <a name="express-version"></a><span data-ttu-id="7f332-106">Express バージョン</span><span class="sxs-lookup"><span data-stu-id="7f332-106">Express version</span></span>

<span data-ttu-id="7f332-107">Express は Node.js の Web アプリケーション フレームワークであるため、Node.js とノード パッケージ マネージャーが既にインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="7f332-107">Because Express is a Node.js web application framework, we need to have Node.js and node package manager installed already.</span></span>

<span data-ttu-id="7f332-108">本稿執筆時点の Express のバージョンは `4.16.0` です。</span><span class="sxs-lookup"><span data-stu-id="7f332-108">At the time of this writing, the Express version is `4.16.0`.</span></span>

## <a name="how-to-install-express"></a><span data-ttu-id="7f332-109">Express のインストール方法</span><span class="sxs-lookup"><span data-stu-id="7f332-109">How to install Express</span></span>

<span data-ttu-id="7f332-110">インストールは次のコマンドを実行するだけなので簡単です。</span><span class="sxs-lookup"><span data-stu-id="7f332-110">The installation is as simple as running the following command:</span></span>

   ```bash
   npm install express
   ```

## <a name="summary"></a><span data-ttu-id="7f332-111">まとめ</span><span class="sxs-lookup"><span data-stu-id="7f332-111">Summary</span></span>

<span data-ttu-id="7f332-112">Express がインストールされると、HTTP 要求に応答するショートカットのパスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="7f332-112">With Express installed, we have a shortcut path to responding to HTTP requests.</span></span> <span data-ttu-id="7f332-113">API および Web ページの応答データを処理するための要求のルーティングとハンドラーをすばやく設定できます。</span><span class="sxs-lookup"><span data-stu-id="7f332-113">We can quickly set up request routing and handlers for handling API and web page response data.</span></span>