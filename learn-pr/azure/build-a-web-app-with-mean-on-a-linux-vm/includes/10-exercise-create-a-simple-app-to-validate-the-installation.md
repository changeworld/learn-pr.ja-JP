<span data-ttu-id="32c2d-101">このユニットでは、Node.js でホストされるシンプルな AngularJS アプリケーションを作成します。ルーティングには Express を使用します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-101">In this unit, you're going to create a simple AngularJS application hosted in Node.js and use Express for routing.</span></span> <span data-ttu-id="32c2d-102">バック エンドでは、MongoDB がデータ ストアとして機能します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-102">On the back end, MongoDB will serve as your data store.</span></span> <span data-ttu-id="32c2d-103">アプリケーションは書籍データベースです。本を一覧表示したり、追加したり、削除したりできます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-103">The application is a book database, where you will be able to list, add, and delete books.</span></span>

> [!Important]
> <span data-ttu-id="32c2d-104">これは単純なアプリケーションです。</span><span class="sxs-lookup"><span data-stu-id="32c2d-104">This is a simple application.</span></span> <span data-ttu-id="32c2d-105">目的は、新たにインストールした MEAN スタックをテストすることです。</span><span class="sxs-lookup"><span data-stu-id="32c2d-105">Its purpose is to test the newly installed MEAN stack.</span></span> <span data-ttu-id="32c2d-106">このアプリケーションは安全性が不十分であり、運用環境での使用に適していません。</span><span class="sxs-lookup"><span data-stu-id="32c2d-106">This application is not sufficiently secure or ready for production use.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="32c2d-107">アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="32c2d-107">Create the application</span></span>

<span data-ttu-id="32c2d-108">最初に、アプリケーションのコード、スクリプト、HTML ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-108">First, we're going to create the code, script and HTML files for our application.</span></span> <span data-ttu-id="32c2d-109">Cloud Shell のエディターでこれを行い、その後、VM にファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="32c2d-109">We'll do this in the Cloud Shell editor then copy the files to the VM.</span></span>

1. <span data-ttu-id="32c2d-110">Cloud Shell で、VM に ssh で接続している場合、`exit` を使用して Cloud Shell ファイルシステムに戻ります。</span><span class="sxs-lookup"><span data-stu-id="32c2d-110">In Cloud Shell, if you are still SSHed into your VM, use `exit` to return to the Cloud Shell filesystem.</span></span>

    ```bash
    exit
    ```

1. <span data-ttu-id="32c2d-111">アプリケーションのフォルダーとファイルを作成し、それを Cloud Shell エディターで開きます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-111">Create the folders and files for your application and open them in the Cloud Shell editor.</span></span>

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

    <span data-ttu-id="32c2d-112">これで、プロジェクトのアプリとその依存関係を含めるための "Books" という名前のフォルダーが作成されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-112">This creates a folder called "Books" to contain your project's app and its dependencies.</span></span> <span data-ttu-id="32c2d-113">そのフォルダー内で、アプリケーションのすべてのソースとスクリプトを格納する目的で "アプリ" フォルダーを作成しました。</span><span class="sxs-lookup"><span data-stu-id="32c2d-113">Within that folder, you created an "app" folder to contain all your application resources and scripts.</span></span> <span data-ttu-id="32c2d-114">最後になりますが、妥当な HTTP 要求に直接提供する、クライアント側のすべてのファイルを保持するための "パブリック" フォルダーも作成します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-114">Finally, we will also create a "public" folder to hold all of the client-side files that will be served up directly to appropriate HTTP requests.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="32c2d-115">アプリケーションを作成する</span><span class="sxs-lookup"><span data-stu-id="32c2d-115">Create the application</span></span>

1. <span data-ttu-id="32c2d-116">Mongoose データ モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-116">Create the Mongoose data model.</span></span> <span data-ttu-id="32c2d-117">エディターで、`app/model.js` を開き、次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-117">In the editor, open `app/model.js` and paste in the following code.</span></span>

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
    > <span data-ttu-id="32c2d-118">エディターでファイルにコードを貼り付けたり、コードを変更したりした場合は、その後に必ず [...] メニューまたはアクセス キー (Windows と Linux では <kbd>Ctrl + S</kbd>、macOS では <kbd>Cmd + S</kbd>) を使用して保存してください。</span><span class="sxs-lookup"><span data-stu-id="32c2d-118">Whenever you paste or change code into a file in the editor, make sure to save afterwards using the "..." menu, or the accelerator key (<kbd>Ctrl+S</kbd> on Windows and Linux, <kbd>Cmd+S</kbd> on macOS).</span></span>

    <span data-ttu-id="32c2d-119">このコードは、ローカル VM の MongoDB サーバー上にある、"Books" という名前のデータベースに接続します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-119">This code is connecting to a database called "Books" on the local VM's MongoDB server.</span></span> <span data-ttu-id="32c2d-120">次に、`bookSchema` 変数で定義されたスキーマを利用し、"Book" という名前のデータベース ドキュメントが作成されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-120">It then creates a database document called "Book" with the schema defined by the `bookSchema` variable.</span></span>

2. <span data-ttu-id="32c2d-121">HTTP 要求を処理する Express ルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-121">Create the Express routes that will handle HTTP requests.</span></span> <span data-ttu-id="32c2d-122">エディターで `app/routes.js` を開き、次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-122">Open `app/routes.js` in the editor and paste in the following code.</span></span>

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

    <span data-ttu-id="32c2d-123">このコードにより、アプリケーションに 4 つのルートが作成されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-123">This code will create four routes for our application.</span></span> <span data-ttu-id="32c2d-124">最初の 3 つによって、誰かが API GET、POST、DELETE 要求を `/book` リソースに送信したときの動作が指定されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-124">The first three specify what to do when someone sends an API GET, POST, or DELETE request to the `/book` resource.</span></span> <span data-ttu-id="32c2d-125">最後の 1 つは、要求元をインデックス ページに送信する包括的なルートです。</span><span class="sxs-lookup"><span data-stu-id="32c2d-125">The last one is a catch-all route to send the requester to the index page.</span></span>

    <span data-ttu-id="32c2d-126">Express では、ルート処理コードで HTTP 応答を直接提供できます。あるいは、ファイルから静的コンテンツを提供できます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-126">Express can serve up HTTP responses directly in the route handling code, or it can serve up static content from files.</span></span> <span data-ttu-id="32c2d-127">このサンプル Web アプリケーションでは両方を行っています。</span><span class="sxs-lookup"><span data-stu-id="32c2d-127">We are doing both in this sample web application.</span></span> <span data-ttu-id="32c2d-128">書籍 API 要求の JSON データを使用して、および index.html ファイルから直接 HTML データを使用して応答します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-128">We respond with JSON data for book API requests and with HTML data direct from the index.html file.</span></span>

3. <span data-ttu-id="32c2d-129">アプリケーションをホストするための Express サーバーを作成します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-129">Create the Express server to host the application.</span></span> <span data-ttu-id="32c2d-130">エディターで `server.js` を開き、次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-130">Open `server.js` in the editor and paste in the following code:</span></span>

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

    <span data-ttu-id="32c2d-131">このコードでは、Web アプリケーション自体が作成されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-131">This code creates the web application itself.</span></span> <span data-ttu-id="32c2d-132">**public** という名前のフォルダー (次に作成) から静的ファイルを提供し、前の手順で定義したルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-132">It will serve static files from a folder named **public** (created next) and will use the routes defined in the previous step.</span></span>

4. <span data-ttu-id="32c2d-133">クライアント側 JavaScript アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-133">Create the client-side JavaScript application.</span></span> <span data-ttu-id="32c2d-134">エディターで `public/script.js` を開き、次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-134">Open `public/script.js` in the editor and paste in this code:</span></span>

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

    <span data-ttu-id="32c2d-135">このクライアント側の AngularJS コードにより、コントローラー `myCtrl` を 1 つ含む新しい Angular アプリケーション `myApp` が作成されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-135">This client-side AngularJS code creates a new angular application `myApp` containing one controller `myCtrl`.</span></span> <span data-ttu-id="32c2d-136">ビューアーのブラウザーでアプリケーションが実行されると、データベースにある書籍一覧を取得する HTTP GET 要求が発行されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-136">When the application is run in the viewer's browser, it will issue an HTTP GET request to retrieve the list of books in the database.</span></span>

5. <span data-ttu-id="32c2d-137">アプリのユーザー インターフェイスを作成します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-137">Create the user interface for the app.</span></span> <span data-ttu-id="32c2d-138">エディターで `public/index.html` を開き、次のコードを貼り付けます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-138">Open `public/index.html` in the editor and paste in this code:</span></span>

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

    <span data-ttu-id="32c2d-139">このコードにより、新しい書籍データを送信し、データベースに既に保存されている全書籍を表示するための 4 つのフィールドを含むシンプルな HTML フォームが作成されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-139">This code will create a simple HTML form with four fields to submit new book data and a table to display all the books already stored in the database.</span></span> <span data-ttu-id="32c2d-140">さまざまな `ng-` HTML 属性により、AngularJS コードが UI に接続されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-140">The various `ng-` HTML attributes will wire up the AngularJS code to the UI.</span></span>

6. <span data-ttu-id="32c2d-141">ファイルの編集はこれで完了です。</span><span class="sxs-lookup"><span data-stu-id="32c2d-141">We're done editing files.</span></span> <span data-ttu-id="32c2d-142">ファイルが全部保存されていることを確認し、次のコマンドを実行してファイルを VM にコピーします。</span><span class="sxs-lookup"><span data-stu-id="32c2d-142">Make sure you have saved all of them, then run the following command to copy them to the VM.</span></span> <span data-ttu-id="32c2d-143">パスワードを求められたら、入力します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-143">Enter your password when prompted.</span></span>

    ```bash
    scp -r ~/Books <vm-admin-username>@<vm-public-ip>:~/Books
    ```

## <a name="install-node-packages"></a><span data-ttu-id="32c2d-144">Node パッケージをインストールする</span><span class="sxs-lookup"><span data-stu-id="32c2d-144">Install Node packages</span></span>

1. <span data-ttu-id="32c2d-145">VM に再度 SSH で接続します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-145">SSH back into your VM.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="32c2d-146">ディレクトリを `Books` ディレクトリに変更します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-146">Change directories to the `Books` directory.</span></span>

    ```bash
    cd ~/Books
    ```

1. <span data-ttu-id="32c2d-147">自分の Web アプリケーションの利用者に返すコンテンツを決定するには、HTTP 要求のルーティングを処理するために **Express** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="32c2d-147">Install **Express** to handle routing of your HTTP requests, to decide what content to return to a user of your web application.</span></span>

    <span data-ttu-id="32c2d-148">次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして Express を追加します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-148">Run the following command to add Express as a package for your web application to use.</span></span>

    ```bash
    npm install express
    ```

1. <span data-ttu-id="32c2d-149">MongoDB と HTTP 要求ルーティングの間で書籍データをリレーするために **Mongoose** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="32c2d-149">Install **Mongoose** to help relay your book data between MongoDB and the HTTP request routing.</span></span>

    <span data-ttu-id="32c2d-150">書籍情報の問い合わせは REST API 要求経由で行われます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-150">The book information will be queried via REST API requests.</span></span> <span data-ttu-id="32c2d-151">API と MongoDB の間のデータ転送を簡単にするために、Mongoose を使用します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-151">To simplify the transfer of data in and out of MongoDB to our API, we will use Mongoose.</span></span> <span data-ttu-id="32c2d-152">Mongoose はデータをモデル化するためのスキーマベースのシステムです。</span><span class="sxs-lookup"><span data-stu-id="32c2d-152">Mongoose is a schema-based system for modeling data.</span></span> <span data-ttu-id="32c2d-153">さまざまな GET、POST、DELETE HTTP 要求でデータ モデルの一貫性を維持するために、サンプル アプリケーションでは Mongoose を使用します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-153">We will be using it in our sample application to keep our data models consistent through the various GET, POST, and DELETE HTTP requests.</span></span>

    <span data-ttu-id="32c2d-154">次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして Mongoose を追加します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-154">Run the following command to add Mongoose as a package for your web application to use.</span></span>

      ```bash
      npm install mongoose
      ```

1. <span data-ttu-id="32c2d-155">Express ルーティングで使用するための JSON 要求データを事前処理するために **body-parser** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="32c2d-155">Install **body-parser** to pre-process JSON request data for use in our Express routing.</span></span>

    <span data-ttu-id="32c2d-156">バック エンドでは、`body-parser` は Node.js と Express の間で、入ってくる JSON 要求データを解析するためのミドルウェアとして機能します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-156">On the back end, `body-parser` will serve as a middleware between Node.js and Express for parsing incoming JSON request data.</span></span>

    <span data-ttu-id="32c2d-157">次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして `body-parser` を追加します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-157">Run the following command to add `body-parser` as a package for your web application to use.</span></span>

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > <span data-ttu-id="32c2d-158">複数の npm パッケージをインストールするとき、このように 1 つのコマンドに全部含めることができます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-158">When installing multiple npm packages, you can include them all in a single command such as this:</span></span>
    >
    > ```bash
    > npm install express mongoose body-parser
    > ```

## <a name="test-the-application"></a><span data-ttu-id="32c2d-159">アプリケーションをテストする</span><span class="sxs-lookup"><span data-stu-id="32c2d-159">Test the application</span></span>

1. <span data-ttu-id="32c2d-160">次のコマンドを使用し、Node.js でアプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-160">Start the application with Node.js with the following command.</span></span>

    ```bash
    sudo node server.js
    ```

    <span data-ttu-id="32c2d-161">これによりアプリケーションのバック エンドが起動し、ポート 80 で入ってくる HTTP 要求に対する待ち受けが開始されます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-161">This will start the back end of our application, which will then start listening on port 80 for incoming HTTP requests.</span></span>

1. <span data-ttu-id="32c2d-162">アプリケーションの機能をテストします。</span><span class="sxs-lookup"><span data-stu-id="32c2d-162">Test the application functionality.</span></span>

    <span data-ttu-id="32c2d-163">任意のブラウザーを開き、Azure VM のパブリック IP アドレスを URL として指定し、そこに移動します。</span><span class="sxs-lookup"><span data-stu-id="32c2d-163">Open your preferred browser, and navigate to the public IP address of your Azure VM as the URL.</span></span>

    ```bash
    http://<vm-public-ip>
    ```

    <span data-ttu-id="32c2d-164">問題なければ、次のような画面が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="32c2d-164">If everything is in order, you should see a screen similar to this:</span></span>

    <span data-ttu-id="32c2d-165">次は、MongoDB データベースに保管する書籍詳細を送信するためのユーザー インターフェイスのスクリーンショットです。</span><span class="sxs-lookup"><span data-stu-id="32c2d-165">The following screenshot displays the user interface to submit book details for storage in the MongoDB database.</span></span>

    ![Web ブラウザーのスクリーンショット。書籍を追加するためのデータ入力フォームを確認できます。](../media/10-book-page.png)

    <span data-ttu-id="32c2d-167">これで書籍を送信し、MongoDB データベースに保存できます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-167">You should now be able to submit books to save to the MongoDB database.</span></span> <span data-ttu-id="32c2d-168">同様に、データベースから読み込んだ書籍一覧をすべて表示できます。</span><span class="sxs-lookup"><span data-stu-id="32c2d-168">As well, you can see the full list of books loaded from the database.</span></span>