<span data-ttu-id="fea99-101">この演習では、Node.js でホストされるシンプルな AngularJS アプリケーションを作成します。ルーティングには Express を使用します。</span><span class="sxs-lookup"><span data-stu-id="fea99-101">In this exercise, you are going to create a simple AngularJS application hosted in Node.js and using Express for routing.</span></span> <span data-ttu-id="fea99-102">バックエンドでは、MongoDB がデータ ストアとして機能します。</span><span class="sxs-lookup"><span data-stu-id="fea99-102">On the back-end, MongoDB will serve as your data store.</span></span> <span data-ttu-id="fea99-103">アプリケーションは書籍データベースです。本を一覧表示したり、追加したり、削除したりできます。</span><span class="sxs-lookup"><span data-stu-id="fea99-103">The application is a book database, where you will be able to list, add, and delete books.</span></span>

> [!Important]
> <span data-ttu-id="fea99-104">このアプリケーションは非常にシンプルなアプリケーションであり、その唯一の目的は新しくインストールした MEAN スタックをテストすることです。</span><span class="sxs-lookup"><span data-stu-id="fea99-104">This application is a very simple application whose sole purpose is to test the newly installed MEAN stack.</span></span> <span data-ttu-id="fea99-105">このアプリケーションは安全性が不十分であり、運用環境での使用に適していません。それを踏まえた上でご利用ください。</span><span class="sxs-lookup"><span data-stu-id="fea99-105">This application is not sufficiently secure or ready for production use, so please use it accordingly.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="fea99-106">VM に接続する</span><span class="sxs-lookup"><span data-stu-id="fea99-106">Connect to the VM</span></span>

<span data-ttu-id="fea99-107">まだ VM に接続していない場合は、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="fea99-107">If you aren't still connected to your VM, run the following command.</span></span> <span data-ttu-id="fea99-108">`<vm-admin-username>` と `<vm-public-ip>` のプレースホルダーを、管理者ユーザー名とご利用の VM のパブリック IP アドレスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="fea99-108">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="create-the-back-end"></a><span data-ttu-id="fea99-109">バックエンドを作成する</span><span class="sxs-lookup"><span data-stu-id="fea99-109">Create the back-end</span></span>

1. <span data-ttu-id="fea99-110">次のコマンドで新しいサンプル アプリケーションのためのフォルダー構造を作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-110">Create a folder structure for your new sample application with the following command.</span></span>

    ```bash
    mkdir ~/Books
    mkdir ~/Books/app
    mkdir ~/Books/public
    ```

    <span data-ttu-id="fea99-111">管理者ユーザーのホームで、プロジェクトのアプリとその依存関係を格納する目的で "Books" という名前のフォルダーを作成しました。</span><span class="sxs-lookup"><span data-stu-id="fea99-111">In your admin user's home location, you created a folder called "Books" to contain your project's app and its dependencies.</span></span> <span data-ttu-id="fea99-112">そのフォルダー内で、アプリケーションのすべてのソースとスクリプトを格納する目的で "アプリ" フォルダーを作成しました。</span><span class="sxs-lookup"><span data-stu-id="fea99-112">Within that folder, you created an "app" folder to contain all your application resources and scripts.</span></span> <span data-ttu-id="fea99-113">最後になりますが、妥当な HTTP 要求に直接提供する、クライアント側のすべてのファイルを保持するための "パブリック" フォルダーも作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-113">Finally, we will also create a "public" folder to hold all of the client-side files that will be served up directly to appropriate HTTP requests.</span></span>

1. <span data-ttu-id="fea99-114">HTTP 要求のルーティングを処理するために **Express** をインストールします。自分の Web アプリケーションの利用者に返すコンテンツを決定します。</span><span class="sxs-lookup"><span data-stu-id="fea99-114">Install **Express** to handle routing of your HTTP requests, deciding what content to return to a user of your web application.</span></span>

    <span data-ttu-id="fea99-115">次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして Express を追加します。</span><span class="sxs-lookup"><span data-stu-id="fea99-115">Run the following command to add Express as a package for your web application to use.</span></span>

        ```bash
        npm install express
        ```

1. <span data-ttu-id="fea99-116">MongoDB と HTTP 要求ルーティングの間で書籍データをリレーするために **Mongoose** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="fea99-116">Install **Mongoose** to help relay your book data between MongoDB and the HTTP request routing.</span></span>

    <span data-ttu-id="fea99-117">書籍情報の問い合わせは REST API 要求経由で行われます。</span><span class="sxs-lookup"><span data-stu-id="fea99-117">The book information will be queried via REST API requests.</span></span> <span data-ttu-id="fea99-118">API と MongoDB の間のデータ転送を簡単にするために、Mongoose を使用します。</span><span class="sxs-lookup"><span data-stu-id="fea99-118">To simplify the transfer of data in and out of MongoDB to our API, we will use Mongoose.</span></span> <span data-ttu-id="fea99-119">Mongoose はデータをモデル化するためのスキーマベースのシステムです。</span><span class="sxs-lookup"><span data-stu-id="fea99-119">Mongoose is a schema-based system for modeling data.</span></span> <span data-ttu-id="fea99-120">さまざまな GET、POST、DELETE HTTP 要求でデータ モデルの一貫性を維持するために、サンプル アプリケーションでは Mongoose を使用します。</span><span class="sxs-lookup"><span data-stu-id="fea99-120">We will be using it in our sample application to keep our data models consistent through the various GET, POST, and DELETE HTTP requests.</span></span>

    <span data-ttu-id="fea99-121">次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして Mongoose を追加します。</span><span class="sxs-lookup"><span data-stu-id="fea99-121">Run the following command to add Mongoose as a package for your web application to use.</span></span>

        ```bash
        npm install mongoose
        ```

1. <span data-ttu-id="fea99-122">Express ルーティングで使用するための JSON 要求データを事前処理するために **body-parser** をインストールします。</span><span class="sxs-lookup"><span data-stu-id="fea99-122">Install **body-parser** to pre-process JSON request data for use in our Express routing.</span></span>

    <span data-ttu-id="fea99-123">バックエンドでは、body-parser は Node.js と Express の間で、入ってくる JSON 要求データを解析するためのミドルウェアとして機能します。</span><span class="sxs-lookup"><span data-stu-id="fea99-123">On the back-end, body-parser will serve as a middleware between Node.js and Express for parsing incoming JSON request data.</span></span>

    <span data-ttu-id="fea99-124">次のコマンドを実行し、自分の Web アプリケーションが使用するパッケージとして body-parser を追加します。</span><span class="sxs-lookup"><span data-stu-id="fea99-124">Run the following command to add body-parser as a package for your web application to use.</span></span>

        ```bash
        npm install body-parser
        ```

    > [!Note]
    > <span data-ttu-id="fea99-125">複数の npm パッケージをインストールするとき、このように 1 つのコマンドに全部含めることもできます。</span><span class="sxs-lookup"><span data-stu-id="fea99-125">When installing multiple npm packages, you can also include them all in a single command such as this.</span></span>
    > ```bash
    > npm install express mongoose body-parser
    > ```

1. <span data-ttu-id="fea99-126">Mongoose を使用し、書籍 Web アプリケーション用のデータ モデル バックエンドを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-126">Create the data model back-end for your books web application using Mongoose.</span></span>

    1. <span data-ttu-id="fea99-127">**Books** アプリケーション フォルダー内の **アプリ** フォルダー内で、書籍の Mongoose ベースのデータ モデルを含める目的で **model.js** という名前の新しい JavaScript ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-127">Within the **app** folder within your **Books** application folder, create a new JavaScript file called **model.js** to contain your book's Mongoose-based data model.</span></span>

        ```bash
        nano ~/Books/app/model.js
        ```

    1. <span data-ttu-id="fea99-128">この新しいファイルに次のコードを貼り付け、Mongoose を使用して書籍スキーマを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-128">Paste the following code into this new file to create our book schema using Mongoose.</span></span>

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

        > [!Note]
        > <span data-ttu-id="fea99-129">**nano** に現在のファイルを保存するには、**Ctrl**+**O** を押す必要があります。**nano** を終了するには、**Ctrl**+**X** を押す必要があります。</span><span class="sxs-lookup"><span data-stu-id="fea99-129">To save the current file in **nano** you need to press **Ctrl**+**O**, and to exit **nano** you need to press **Ctrl**+**X**</span></span>

        <span data-ttu-id="fea99-130">このコードは、ローカル VM の MongoDB サーバー上にある、`Books` という名前のデータベースに接続します。</span><span class="sxs-lookup"><span data-stu-id="fea99-130">This code is connecting to a database called `Books` on the local VM's MongoDB server.</span></span> <span data-ttu-id="fea99-131">次に、`bookSchema` 変数で定義されたスキーマを利用し、`Book` という名前のデータベース ドキュメントが作成されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-131">It then creates a database document called `Book` with the schema defined by the `bookSchema` variable.</span></span>

1. <span data-ttu-id="fea99-132">さまざまな HTTP 要求を処理するためにアプリケーションの Express ルートを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-132">Create the Express routes for the application to handle the various HTTP request.</span></span>

    1. <span data-ttu-id="fea99-133">**アプリ** フォルダー内で、**routes.js** という名前の新しい JavaScript ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-133">Within the **app** folder, create a new JavaScript file called **routes.js**.</span></span>

        ```bash
        nano ~/Books/app/routes.js
        ```

    1. <span data-ttu-id="fea99-134">この新しいファイルに次のコードを貼り付け、Express を使用してルートを確立します。</span><span class="sxs-lookup"><span data-stu-id="fea99-134">Paste the following code into this new file to establish the routes using Express.</span></span>

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

        <span data-ttu-id="fea99-135">このコードにより、アプリケーションに 4 つのルートが作成されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-135">This code will create four routes for our application.</span></span> <span data-ttu-id="fea99-136">最初の 3 つによって、誰かが API GET、POST、DELETE 要求を `/book` リソースに送信したときの動作が指定されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-136">The first three specify what to do when someone sends an API GET, POST, or DELETE request to the `/book` resource.</span></span> <span data-ttu-id="fea99-137">最後の 1 つは、要求元をインデックス ページに送信する包括的なルートです。</span><span class="sxs-lookup"><span data-stu-id="fea99-137">The last one is a catch all route to send the requester to the index page.</span></span>

        <span data-ttu-id="fea99-138">Express では、ルート処理コードで HTTP 応答を直接提供できます。あるいは、ファイルから静的コンテンツを提供できます。</span><span class="sxs-lookup"><span data-stu-id="fea99-138">Express can serve up HTTP responses directly in the route handling code, or it can serve up static content from files.</span></span> <span data-ttu-id="fea99-139">このサンプル Web アプリケーションでは両方行います。書籍 API 要求に JSON データで応答し、index.html ファイルから直接、HTML データで応答します。</span><span class="sxs-lookup"><span data-stu-id="fea99-139">We are doing both in this sample web application, responding with JSON data for book API requests and responding with HTML data direct from the index.html file.</span></span>

1. <span data-ttu-id="fea99-140">**Books** フォルダーで **server.js** ファイルを作成し、(Express ルートを使用し) Node.js ホスティングを構成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-140">Create a **server.js** file in the **Books** folder to configure the Node.js hosting (using the Express routes).</span></span>

    1. <span data-ttu-id="fea99-141">アプリケーション ルート **Books** フォルダーで、**server.js** という名前の新しい JavaScript ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-141">Back in the application root **Books** folder, create a new JavaScript file called **server.js**.</span></span>

        ```bash
        nano ~/Books/server.js
        ```

    1. <span data-ttu-id="fea99-142">この新しいファイルに次のコードを貼り付け、Web アプリケーションを構成し、既定の HTTP ポートの待ち受けを開始します。</span><span class="sxs-lookup"><span data-stu-id="fea99-142">Paste the following code into this new file to configure your web application and start listening to the default HTTP port.</span></span>

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

        <span data-ttu-id="fea99-143">このコードでは、Web アプリケーション自体が作成されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-143">This code creates the web application itself.</span></span> <span data-ttu-id="fea99-144">**public** という名前のフォルダー (次に作成) から静的ファイルを提供し、前の手順で定義したルートを使用します。</span><span class="sxs-lookup"><span data-stu-id="fea99-144">It will serve static files from a folder named **public** (created next) and will use the routes defined in the previous step.</span></span>

1. <span data-ttu-id="fea99-145">フロントエンド HTML を作成し、クライアント側 JavaScript アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-145">Create the front-end HTML and client-side JavaScript application.</span></span>

    1. <span data-ttu-id="fea99-146">**public** コンテンツ フォルダー内で、**script.js** という名前の新しい JavaScript ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-146">Within the **public** content folder, create a new JavaScript file called **script.js**.</span></span>

        ```bash
        nano ~/Books/public/script.js
        ```

    1. <span data-ttu-id="fea99-147">この新しいファイルに次のコードを貼り付け、Web サーバーとの通信を処理するようにクライアント側の Web アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-147">Paste the following code into this new file to configure your client-side web application to handle communicating with your web server.</span></span>

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

        <span data-ttu-id="fea99-148">このクライアント側の AngularJS コードにより、コントローラー `myCtrl` を 1 つ含む新しい Angular アプリケーション `myApp` が作成されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-148">This client-side AngularJS code creates a new angular application `myApp` containing one controller `myCtrl`.</span></span> <span data-ttu-id="fea99-149">ビューアーのブラウザーでアプリケーションが実行されると、データベースにある書籍一覧を取得する HTTP GET 要求が発行されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-149">When the application is run in the viewer's browser, it will issue an HTTP GET request to retrieve the list of books in the database.</span></span>

    1. <span data-ttu-id="fea99-150">また、**public** コンテンツ フォルダー内で、**index.html** という名前の新しい HTML ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fea99-150">Also within the **public** content folder, create a new HTML file called **index.html**.</span></span>

        ```bash
        nano ~/Books/public/index.html
        ```

    1. <span data-ttu-id="fea99-151">この新しいファイルに次のマークアップを貼り付け、Web アプリケーションの HTML ユーザー インターフェイスを設定します。</span><span class="sxs-lookup"><span data-stu-id="fea99-151">Paste the following mark-up into this new file to set up your web application's HTML user interface.</span></span>

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

        <span data-ttu-id="fea99-152">このコードにより、新しい書籍データを送信し、データベースに既に保存されている全書籍を表示するための 4 つのフィールドを含むシンプルな HTML フォームが作成されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-152">This code will create a simple HTML form with four fields to submit new book data and a table to display all the books already stored in the database.</span></span> <span data-ttu-id="fea99-153">さまざまな `ng-` HTML 属性により、AngularJS コードが UI に接続されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-153">The various `ng-` HTML attributes will wire up the AngularJS code to the UI.</span></span>

1. <span data-ttu-id="fea99-154">完全な書籍 Web アプリケーションをテストします。</span><span class="sxs-lookup"><span data-stu-id="fea99-154">Test the full books web application.</span></span>

    1. <span data-ttu-id="fea99-155">次のコマンドを使用し、Node.js でアプリケーションを起動します。</span><span class="sxs-lookup"><span data-stu-id="fea99-155">Start the application with Node.js with the following command.</span></span>

        ```bash
        sudo node ~/Books/server.js
        ```

        <span data-ttu-id="fea99-156">これによりアプリケーションのバックエンドが起動し、ポート 80 で入ってくる HTTP 要求に対する待ち受けが開始されます。</span><span class="sxs-lookup"><span data-stu-id="fea99-156">This will start the back-end of our application, which will then start listening on port 80 for incoming HTTP requests.</span></span>

    1. <span data-ttu-id="fea99-157">アプリケーションの機能をテストします。</span><span class="sxs-lookup"><span data-stu-id="fea99-157">Test the application functionality.</span></span>

        <span data-ttu-id="fea99-158">任意のブラウザーを開き、Azure VM のパブリック IP アドレスを URL として指定し、そこに移動します。</span><span class="sxs-lookup"><span data-stu-id="fea99-158">Open your preferred browser, and navigate to the public IP address of your Azure VM as the URL.</span></span>

        ```bash
        http://<vm-public-ip>
        ```

        <span data-ttu-id="fea99-159">問題なければ、次のような画面が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="fea99-159">If everything is in order, you should see a screen similar to this:</span></span>

        <span data-ttu-id="fea99-160">次は、MongoDB データベースに保管する書籍詳細を送信するためのユーザー インターフェイスのスクリーンショットです。</span><span class="sxs-lookup"><span data-stu-id="fea99-160">The following screenshot displays the user interface to submit book details for storage in the MongoDB database.</span></span>


        ![Web ブラウザーのスクリーンショット。書籍を追加するためのデータ入力フォームを確認できます。](../media-draft/10-book-page.png)

    <span data-ttu-id="fea99-162">これで書籍を送信し、MongoDB データベースに保存できます。</span><span class="sxs-lookup"><span data-stu-id="fea99-162">You should now be able to submit books to save to the MongoDB database.</span></span> <span data-ttu-id="fea99-163">同様に、データベースから読み込んだ書籍一覧をすべて表示できます。</span><span class="sxs-lookup"><span data-stu-id="fea99-163">As well, you can see the full list of books loaded from the database.</span></span>