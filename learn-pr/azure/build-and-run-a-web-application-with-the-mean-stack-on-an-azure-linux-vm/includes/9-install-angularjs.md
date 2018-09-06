<span data-ttu-id="a2521-101">AngularJS は明確で簡潔な動的 Web アプリケーションを作成するためのフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="a2521-101">AngularJS is a framework for creating clear and succinct dynamic web applications.</span></span> <span data-ttu-id="a2521-102">HTML はコンテンツ テンプレート言語に使用しますが、HTML の拡張ではデータ バインディング構文を使用します。</span><span class="sxs-lookup"><span data-stu-id="a2521-102">You use HTML for your content template language, but extend your HTML with data-binding syntax.</span></span> <span data-ttu-id="a2521-103">データ バインディングと依存関係の挿入に AngularJS を使用すると、コンテンツの更新の管理で記述しなければならないコードの多くが不要になります。</span><span class="sxs-lookup"><span data-stu-id="a2521-103">Using AngularJS for data-binding and dependency-injection helps eliminate much of the code you would otherwise have to write to manage content updates.</span></span> <span data-ttu-id="a2521-104">ユーザーのコンテンツの更新はブラウザー内で行われるため、AngularJS を任意のサーバー側ホスティング テクノロジと組み合わせることができるようになります。</span><span class="sxs-lookup"><span data-stu-id="a2521-104">The updating of your user's content happens within the browser, allowing AngularJS to pair with any server-side hosting technology.</span></span>

## <a name="what-must-be-installed"></a><span data-ttu-id="a2521-105">インストールが必要なもの</span><span class="sxs-lookup"><span data-stu-id="a2521-105">What must be Installed?</span></span>

<span data-ttu-id="a2521-106">AngularJS はフロントエンドの JavaScript フレームワークであるため、使用できるようにする必要があるのは、アプリケーションにアクセスしているクライアントに対してのみです。</span><span class="sxs-lookup"><span data-stu-id="a2521-106">Because AngularJS is a front-end JavaScript framework it only needs to be made available to the clients accessing the application.</span></span> <span data-ttu-id="a2521-107">これはいくつかの方法で実現できます。</span><span class="sxs-lookup"><span data-stu-id="a2521-107">This can be achieved in several ways.</span></span>

1. <span data-ttu-id="a2521-108">コンテンツ配信ネットワーク (CDN) を使用して AngularJS を参照します。</span><span class="sxs-lookup"><span data-stu-id="a2521-108">Reference AngularJS via a content delivery network (CDN).</span></span>
1. <span data-ttu-id="a2521-109">npm をパッケージ マネージャーとして使用して、ホストされているコンテンツの AngularJS を Node.js パッケージとして扱います。</span><span class="sxs-lookup"><span data-stu-id="a2521-109">Serve AngularJS from your hosted content as a Node.js package using npm as a package manager.</span></span>
1. <span data-ttu-id="a2521-110">Bower パッケージとしてホストされているコンテンツから AngularJS を使用します。</span><span class="sxs-lookup"><span data-stu-id="a2521-110">Serve AngularJS from your hosted content as a Bower package.</span></span>

<span data-ttu-id="a2521-111">このモジュールでは、単なる簡略化のため CDN から直接 AngularJS を使用しています。</span><span class="sxs-lookup"><span data-stu-id="a2521-111">We will be using AngularJS directly from a CDN for this module simply for simplicity.</span></span> <span data-ttu-id="a2521-112">これにより、スクリプト ファイルの依存関係がサーバーのコンテンツ上で直接管理される代わりに CDN に渡されます。そのため、指定したリソースで使用される CDN の速度と地理的な分散によっては、ダウンロード速度も改善する場合があります。</span><span class="sxs-lookup"><span data-stu-id="a2521-112">This passes the script file dependency to the CDN instead of managing it on our server content directly, which can also improve download speeds based on the speed and geographical distribution of the CDN used for a given resource.</span></span>

> [!Note]
> <span data-ttu-id="a2521-113">AngularJS は Angular に先行するもので、Web アプリケーション プラットフォームを全く新しく書き換えたものです。</span><span class="sxs-lookup"><span data-stu-id="a2521-113">AngularJS is the predecessor to Angular, which was a complete re-write of the web application platform.</span></span> <span data-ttu-id="a2521-114">2 つのバージョンの多くの概念は似ていますが、現在これらは個別のプロジェクトになりました。</span><span class="sxs-lookup"><span data-stu-id="a2521-114">While many of the concepts are similar between the two versions, they are separate projects now.</span></span> <span data-ttu-id="a2521-115">Angular には 2.x.y 以降のバージョンが付けられ、AngularJS のバージョンは 1.x.y で終わります。</span><span class="sxs-lookup"><span data-stu-id="a2521-115">Angular will have versions of 2.x.y and higher, while AngularJS will have versions ending in 1.x.y.</span></span> <span data-ttu-id="a2521-116">AngularJS は引き続き Web アプリケーション シナリオで一般的に使用されます。</span><span class="sxs-lookup"><span data-stu-id="a2521-116">AngularJS is still a commonly used for web application scenarios.</span></span>

## <a name="how-to-install-angularjs-via-cdn"></a><span data-ttu-id="a2521-117">CDN を使用した AngularJS のインストール方法</span><span class="sxs-lookup"><span data-stu-id="a2521-117">How to install AngularJS via CDN</span></span>

    You don't really _install_ AngularJS; you just add a reference to the JavaScript file via a script tag in you HTML page. Since AngularJS is critical to any functionality of our web application, we include it within the `<head>` tag like this (as compared to later loading of JavaScript files toward the end of the `<body>` tag).

    ```html
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
    ```