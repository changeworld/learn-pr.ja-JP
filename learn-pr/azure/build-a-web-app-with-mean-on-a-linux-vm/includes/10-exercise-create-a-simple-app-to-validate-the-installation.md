このユニットでは、Node.js でホストされるシンプルな AngularJS アプリケーションを作成します。ルーティングには Express を使用します。 バック エンドでは、MongoDB がデータ ストアとして機能します。 アプリケーションは書籍データベースです。本を一覧表示したり、追加したり、削除したりできます。

> [!Important]
> これは単純なアプリケーションです。 目的は、新たにインストールした MEAN スタックをテストすることです。 このアプリケーションは安全性が不十分であり、運用環境での使用に適していません。

## <a name="create-the-application"></a>アプリケーションを作成する

最初に、アプリケーションのコード、スクリプト、HTML ファイルを作成します。 Cloud Shell のエディターでこれを行い、その後、VM にファイルをコピーします。

1. Cloud Shell で、VM に ssh で接続している場合、`exit` を使用して Cloud Shell ファイルシステムに戻ります。

    ```bash
    exit
    ```

1. アプリケーションのフォルダーとファイルを作成し、それを Cloud Shell エディターで開きます。

    ```bash
    cd ~
    mkdir Books
    mkdir Books/app
    mkdir Books/public
    touch Books/app/model.js
    touch Books/app/routes.js
    touch Books/server.js
    touch Books/public/script.js
    touch Books/public/index.html
    code Books
    ```

    これで、プロジェクトのアプリとその依存関係を含めるための "Books" という名前のフォルダーが作成されます。 そのフォルダー内で、アプリケーションのすべてのソースとスクリプトを格納する目的で "アプリ" フォルダーを作成しました。 最後になりますが、妥当な HTTP 要求に直接提供する、クライアント側のすべてのファイルを保持するための "パブリック" フォルダーも作成します。

## <a name="create-the-application"></a>アプリケーションを作成する

1. Mongoose データ モデルを作成します。 エディターで、`app/model.js` を開き、次のコードを貼り付けます。

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

    > [!IMPORTANT]
    > エディターでファイルにコードを貼り付けたり、コードを変更したりした場合は、その後に必ず [...] メニューまたはアクセス キー (Windows と Linux では <kbd>Ctrl + S</kbd>、macOS では <kbd>Cmd + S</kbd>) を使用して保存してください。

    このコードは、ローカル VM の MongoDB サーバー上にある、"Books" という名前のデータベースに接続します。 次に、`bookSchema` 変数で定義されたスキーマを利用し、"Book" という名前のデータベース ドキュメントが作成されます。

2. HTTP 要求を処理する Express ルートを作成します。 エディターで `app/routes.js` を開き、次のコードを貼り付けます。

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

3. アプリケーションをホストするための Express サーバーを作成します。 エディターで `server.js` を開き、次のコードを貼り付けます。

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

4. クライアント側 JavaScript アプリケーションを作成します。 エディターで `public/script.js` を開き、次のコードを貼り付けます。

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

5. アプリのユーザー インターフェイスを作成します。 エディターで `public/index.html` を開き、次のコードを貼り付けます。

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

6. ファイルの編集はこれで完了です。 ファイルが全部保存されていることを確認し、次のコマンドを実行してファイルを VM にコピーします。 パスワードを求められたら、入力します。

    ```bash
    scp -r ~/Books <vm-admin-username>@<vm-public-ip>:~/Books
    ```

## <a name="install-node-packages"></a>Node パッケージをインストールする

1. VM に再度 SSH で接続します。

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. ディレクトリを `Books` ディレクトリに変更します。

    ```bash
    cd ~/Books
    ```

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

## <a name="test-the-application"></a>アプリケーションをテストする

1. 次のコマンドを使用し、Node.js でアプリケーションを起動します。

    ```bash
    sudo node server.js
    ```

    これによりアプリケーションのバック エンドが起動し、ポート 80 で入ってくる HTTP 要求に対する待ち受けが開始されます。

1. アプリケーションの機能をテストします。

    任意のブラウザーを開き、Azure VM のパブリック IP アドレスを URL として指定し、そこに移動します。

    ```bash
    http://<vm-public-ip>
    ```

    問題なければ、次のような画面が表示されるはずです。

    次は、MongoDB データベースに保管する書籍詳細を送信するためのユーザー インターフェイスのスクリーンショットです。

    ![Web ブラウザーのスクリーンショット。書籍を追加するためのデータ入力フォームを確認できます。](../media/10-book-page.png)

    これで書籍を送信し、MongoDB データベースに保存できます。 同様に、データベースから読み込んだ書籍一覧をすべて表示できます。