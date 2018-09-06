<span data-ttu-id="a1383-101">MEAN スタックにおいて、**E** は Express コンポーネントを表します。</span><span class="sxs-lookup"><span data-stu-id="a1383-101">In the MEAN stack, the **E** represents the Express component.</span></span> <span data-ttu-id="a1383-102">Express は、Node.js 向けに構築された Web サーバーのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="a1383-102">Express is a web server framework built for Node.js.</span></span> <span data-ttu-id="a1383-103">要求のルーティングとヘルパー システムを処理し、Node.js ランタイムでホストされている Web アプリケーションの構築を簡単にするために設計されています。</span><span class="sxs-lookup"><span data-stu-id="a1383-103">It will handle our request routing and helper system, designed to simplify building web applications hosted on the Node.js runtime.</span></span> <span data-ttu-id="a1383-104">Express が主に行うのはルーティングですが、Web アプリケーションが HTTP Cookie を処理したりクエリ文字列データを要求したりする場合にも役立ちます。</span><span class="sxs-lookup"><span data-stu-id="a1383-104">While the core of Express is routing, it also helps web applications work with HTTP cookies and requst query string data.</span></span>

## <a name="what-must-be-installed"></a><span data-ttu-id="a1383-105">インストールが必要なもの</span><span class="sxs-lookup"><span data-stu-id="a1383-105">What must be Installed?</span></span>

<span data-ttu-id="a1383-106">Express は Node.js の Web アプリケーション フレームワークであるため、Node.js とノード パッケージ マネージャーが既にインストールされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="a1383-106">Because Express is a Node.js web application framework, we need to have Node.js and node package manager installed already.</span></span>

<span data-ttu-id="a1383-107">本稿執筆時点の Express のバージョンは `4.16.0` です。</span><span class="sxs-lookup"><span data-stu-id="a1383-107">At the time of this writing the Express version is `4.16.0`.</span></span>

## <a name="how-to-install-express"></a><span data-ttu-id="a1383-108">Express のインストール方法</span><span class="sxs-lookup"><span data-stu-id="a1383-108">How to install Express</span></span>

<span data-ttu-id="a1383-109">インストールも実行と同じくらい簡単です</span><span class="sxs-lookup"><span data-stu-id="a1383-109">The installation is as simple as running</span></span>

    ```bash
    npm install express
    ```

## <a name="summary"></a><span data-ttu-id="a1383-110">まとめ</span><span class="sxs-lookup"><span data-stu-id="a1383-110">Summary</span></span>

<span data-ttu-id="a1383-111">Express がインストールされると、HTTP 要求に応答するショートカットのパスが作成されます。</span><span class="sxs-lookup"><span data-stu-id="a1383-111">With Express installed, we have a shortcut path to responding to HTTP requests.</span></span> <span data-ttu-id="a1383-112">API および Web ページの応答データを処理するための要求のルーティングとハンドラーをすばやく設定できます。</span><span class="sxs-lookup"><span data-stu-id="a1383-112">We can quickly set up request routing and handlers for handling API and web page response data.</span></span>
