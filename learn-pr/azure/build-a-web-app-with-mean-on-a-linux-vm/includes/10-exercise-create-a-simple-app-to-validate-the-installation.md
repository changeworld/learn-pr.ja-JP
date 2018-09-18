このユニットでは、Node.js でホストされるシンプルな AngularJS アプリケーションを作成します。ルーティングには Express を使用します。 バック エンドでは、MongoDB がデータ ストアとして機能します。 アプリケーションは書籍データベースです。本を一覧表示したり、追加したり、削除したりできます。

> [!Important]
> これは単純なアプリケーションです。 目的は、新たにインストールした MEAN スタックをテストすることです。 このアプリケーションは安全性が不十分であり、運用環境での使用に適していません。

## <a name="connect-to-the-vm"></a>VM に接続する

まだ VM に接続していない場合は、次のコマンドを実行します。 `<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご自分の VM のパブリック IP アドレスに置き換えます。

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="create-the-back-end"></a>バック エンドを作成する

1. 次のコマンドで新しいサンプル アプリケーションのためのフォルダー構造を作成します。

    ```bash
    mkdir ~/Books
    mkdir ~/Books/app
    mkdir ~/Books/public
    ```

    管理者ユーザーのホームで、プロジェクトのアプリとその依存関係を格納する目的で "Books" という名前のフォルダーを作成しました。 そのフォルダー内で、アプリケーションのすべてのソースとスクリプトを格納する目的で "アプリ" フォルダーを作成しました。 最後になりますが、妥当な HTTP 要求に直接提供する、クライアント側のすべてのファイルを保持するための "パブリック" フォルダーも作成します。

1. 自分の Web アプリケーションの利用者に返すコンテンツを決定するには、HTTP 要求のルーティングを処理するために **Express** をインストールします。

    次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして Express を追加します。

      ```bash
      npm install express
      ```

1. MongoDB と HTTP 要求ルーティングの間で書籍データをリレーするために **Mongoose** をインストールします。

    書籍情報の問い合わせは REST API 要求経由で行われます。 API と MongoDB の間のデータ転送を簡単にするために、Mongoose を使用します。 Mongoose はデータをモデル化するためのスキーマベースのシステムです。 さまざまな GET、POST、DELETE HTTP 要求でデータ モデルの一貫性を維持するために、サンプル アプリケーションでは Mongoose を使用します。

    次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして Mongoose を追加します。

      ```bash
      npm install mongoose
      ```

1. Express ルーティングで使用するための JSON 要求データを事前処理するために **body-parser** をインストールします。

    バック エンドでは、`body-parser` は Node.js と Express の間で、入ってくる JSON 要求データを解析するためのミドルウェアとして機能します。

    次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして `body-parser` を追加します。

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > 複数の npm パッケージをインストールするとき、このように 1 つのコマンドに全部含めることができます。
    >
    > ```bash
    > npm install express mongoose body-parser
    > ```

1. Mongoose を使用し、書籍 Web アプリケーション用のデータ モデル バック エンドを作成します。

    1. **Books** アプリケーション フォルダー内の**アプリ** フォルダーに、書籍の Mongoose ベースのデータ モデルを含める目的で **model.js** という名前の新しい JavaScript ファイルを作成します。

        ```bash
        nano ~/Books/app/model.js
        ```

    1. この新しいファイルに次のコードを貼り付け、Mongoose を使用して書籍スキーマを作成します。

        ```javascript
        var mongoose = require('mongoose');
        var dbHost = 'mongodb://localhost:27017/Books';
        mongoose.connect(dbHost,  { useNewUrlParser: true } );
        mongoose.connection;
        mongoose.set('debug', true);
        var bookSchema = mongoose.Schema( {
            name: String,
            isbn: {type: String, index: true},
            author: String,
            pages: Number
        });
        var Book = mongoose.model('Book', bookSchema);
        module.exports = Book // mongoose.model('Book', bookSchema);
        ```

        > [!TIP]
        > **nano** に現在のファイルを保存するには、**Ctrl**+**O** キーを押す必要があります。 **nano** を終了するには、**Ctrl**+**X** キーを押す必要があります。

        このコードは、ローカル VM の MongoDB サーバー上にある、"Books" という名前のデータベースに接続します。 次に、`bookSchema` 変数で定義されたスキーマを利用し、"Book" という名前のデータベース ドキュメントが作成されます。

1. さまざまな HTTP 要求を処理するためにアプリケーションの Express ルートを作成します。

    1. **アプリ** フォルダー内で、**routes.js** という名前の新しい JavaScript ファイルを作成します。

        ```bash
        nano ~/Books/app/routes.js
        ```

    1. この新しいファイルに次のコードを貼り付け、Express を使用してルートを確立します。

        ```javascript
        var path = require('path');
        var Book = require('./model');
        var routes = function(app) {
            app.get('/book', function(req, res) {
                Book.find({}, function(err, result) {
                    if ( err ) throw err;
                    res.json(result);
                });
            });
            app.post('/book', function(req, res) {
                var book = new Book( {
                    name:req.body.name,
                    isbn:req.body.isbn,
                    author:req.body.author,
                    pages:req.body.pages
                });
                book.save(function(err, result) {
                    if ( err ) throw err;
                    res.json( {
                        message:"Successfully added book",
                        book:result
                    });
                });
            });
            app.delete("/book/:isbn", function(req, res) {
                Book.findOneAndRemove(req.query, function(err, result) {
                    if ( err ) throw err;
                    res.json( {
                        message: "Successfully deleted the book",
                        book: result
                    });
                });
            });
            app.get('*', function(req, res) {
                res.sendFile(path.join(__dirname + '/public', 'index.html'));
            });
        };
        module.exports = routes;
        ```

        このコードにより、アプリケーションに 4 つのルートが作成されます。 最初の 3 つによって、誰かが API GET、POST、DELETE 要求を `/book` リソースに送信したときの動作が指定されます。 最後の 1 つは、要求元をインデックス ページに送信する包括的なルートです。

        Express では、ルート処理コードで HTTP 応答を直接提供できます。あるいは、ファイルから静的コンテンツを提供できます。 このサンプル Web アプリケーションでは両方を行っています。 書籍 API 要求の JSON データを使用して、および index.html ファイルから直接 HTML データを使用して応答します。

1. **Books** フォルダーで **server.js** ファイルを作成し、(Express ルートを使用し) Node.js ホスティングを構成します。

    1. アプリケーション ルート **Books** フォルダーで、**server.js** という名前の新しい JavaScript ファイルを作成します。

        ```bash
        nano ~/Books/server.js
        ```

    1. この新しいファイルに次のコードを貼り付け、Web アプリケーションを構成し、既定の HTTP ポートの待ち受けを開始します。

        ```javascript
        var express = require('express');
        var bodyParser = require('body-parser');
        var app = express();
        app.use(express.static(__dirname + '/public'));
        app.use(bodyParser.json());
        require('./app/routes')(app);
        app.set('port', 80);
        app.listen(app.get('port'), function() {
            console.log('Server up: http://localhost:' + app.get('port'));
        });
        ```

        このコードでは、Web アプリケーション自体が作成されます。 **public** という名前のフォルダー (次に作成) から静的ファイルを提供し、前の手順で定義したルートを使用します。

1. フロントエンド HTML を作成し、クライアント側 JavaScript アプリケーションを作成します。

    1. **public** コンテンツ フォルダー内で、**script.js** という名前の新しい JavaScript ファイルを作成します。

        ```bash
        nano ~/Books/public/script.js
        ```

    1. この新しいファイルに次のコードを貼り付け、Web サーバーとの通信を処理するようにクライアント側の Web アプリケーションを構成します。

        ```javascript
        var app = angular.module('myApp', []);
        app.controller('myCtrl', function($scope, $http) {
            var getData = function() {
                return $http( {
                    method: 'GET',
                    url: '/book'
                }).then(function successCallback(response) {
                    $scope.books = response.data;
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
            getData();
            $scope.del_book = function(book) {
                $http( {
                    method: 'DELETE',
                    url: '/book/:isbn',
                    params: {'isbn': book.isbn}
                }).then(function successCallback(response) {
                    console.log(response);
                    return getData();
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
            $scope.add_book = function() {
                var body = '{ "name": "' + $scope.Name +
                '", "isbn": "' + $scope.Isbn +
                '", "author": "' + $scope.Author +
                '", "pages": "' + $scope.Pages + '" }';
                $http({
                    method: 'POST',
                    url: '/book',
                    data: body
                }).then(function successCallback(response) {
                    console.log(response);
                    return getData();
                }, function errorCallback(response) {
                    console.log('Error: ' + response);
                });
            };
        });
        ```

        このクライアント側の AngularJS コードにより、コントローラー `myCtrl` を 1 つ含む新しい Angular アプリケーション `myApp` が作成されます。 ビューアーのブラウザーでアプリケーションが実行されると、データベースにある書籍一覧を取得する HTTP GET 要求が発行されます。

    1. また、**public** コンテンツ フォルダー内で、**index.html** という名前の新しい HTML ファイルを作成します。

        ```bash
        nano ~/Books/public/index.html
        ```

    1. この新しいファイルに次のマークアップを貼り付け、Web アプリケーションの HTML ユーザー インターフェイスを設定します。

        ```html
        <!doctype html>
        <html ng-app="myApp" ng-controller="myCtrl">
        <head>
            <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.7.2/angular.min.js"></script>
            <script src="script.js"></script>
        </head>
        <body>
            <div>
            <table>
                <tr>
                <td>Name:</td>
                <td><input type="text" ng-model="Name"></td>
                </tr>
                <tr>
                <td>Isbn:</td>
                <td><input type="text" ng-model="Isbn"></td>
                </tr>
                <tr>
                <td>Author:</td>
                <td><input type="text" ng-model="Author"></td>
                </tr>
                <tr>
                <td>Pages:</td>
                <td><input type="number" ng-model="Pages"></td>
                </tr>
            </table>
            <button ng-click="add_book()">Add</button>
            </div>
            <hr>
            <div>
            <table>
                <tr>
                <th>Name</th>
                <th>Isbn</th>
                <th>Author</th>
                <th>Pages</th>
                </tr>
                <tr ng-repeat="book in books">
                <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
                <td>{{book.name}}</td>
                <td>{{book.isbn}}</td>
                <td>{{book.author}}</td>
                <td>{{book.pages}}</td>
                </tr>
            </table>
            </div>
        </body>
        </html>
        ```

        このコードにより、新しい書籍データを送信し、データベースに既に保存されている全書籍を表示するための 4 つのフィールドを含むシンプルな HTML フォームが作成されます。 さまざまな `ng-` HTML 属性により、AngularJS コードが UI に接続されます。

1. 完全な書籍 Web アプリケーションをテストします。

    1. 次のコマンドを使用し、Node.js でアプリケーションを起動します。

        ```bash
        sudo node ~/Books/server.js
        ```

        これによりアプリケーションのバック エンドが起動し、ポート 80 で入ってくる HTTP 要求に対する待ち受けが開始されます。

    1. アプリケーションの機能をテストします。

        任意のブラウザーを開き、Azure VM のパブリック IP アドレスを URL として指定し、そこに移動します。

        ```bash
        http://<vm-public-ip>
        ```

        問題なければ、次のような画面が表示されるはずです。

        次は、MongoDB データベースに保管する書籍詳細を送信するためのユーザー インターフェイスのスクリーンショットです。


        ![Web ブラウザーのスクリーンショット。書籍を追加するためのデータ入力フォームを確認できます。](../media-draft/10-book-page.png)

    これで書籍を送信し、MongoDB データベースに保存できます。 同様に、データベースから読み込んだ書籍一覧をすべて表示できます。