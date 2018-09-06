AngularJS は明確で簡潔な動的 Web アプリケーションを作成するためのフレームワークです。 HTML はコンテンツ テンプレート言語に使用しますが、HTML の拡張ではデータ バインディング構文を使用します。 データ バインディングと依存関係の挿入に AngularJS を使用すると、コンテンツの更新の管理で記述しなければならないコードの多くが不要になります。 ユーザーのコンテンツの更新はブラウザー内で行われるため、AngularJS を任意のサーバー側ホスティング テクノロジと組み合わせることができるようになります。

## <a name="what-must-be-installed"></a>インストールが必要なもの

AngularJS はフロントエンドの JavaScript フレームワークであるため、使用できるようにする必要があるのは、アプリケーションにアクセスしているクライアントに対してのみです。 これはいくつかの方法で実現できます。

1. コンテンツ配信ネットワーク (CDN) を使用して AngularJS を参照します。
1. npm をパッケージ マネージャーとして使用して、ホストされているコンテンツの AngularJS を Node.js パッケージとして扱います。
1. Bower パッケージとしてホストされているコンテンツから AngularJS を使用します。

このモジュールでは、単なる簡略化のため CDN から直接 AngularJS を使用しています。 これにより、スクリプト ファイルの依存関係がサーバーのコンテンツ上で直接管理される代わりに CDN に渡されます。そのため、指定したリソースで使用される CDN の速度と地理的な分散によっては、ダウンロード速度も改善する場合があります。

> [!Note]
> AngularJS は Angular に先行するもので、Web アプリケーション プラットフォームを全く新しく書き換えたものです。 2 つのバージョンの多くの概念は似ていますが、現在これらは個別のプロジェクトになりました。 Angular には 2.x.y 以降のバージョンが付けられ、AngularJS のバージョンは 1.x.y で終わります。 AngularJS は引き続き Web アプリケーション シナリオで一般的に使用されます。

## <a name="how-to-install-angularjs-via-cdn"></a>CDN を使用した AngularJS のインストール方法

    You don't really _install_ AngularJS; you just add a reference to the JavaScript file via a script tag in you HTML page. Since AngularJS is critical to any functionality of our web application, we include it within the `<head>` tag like this (as compared to later loading of JavaScript files toward the end of the `<body>` tag).

    ```html
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
    ```
