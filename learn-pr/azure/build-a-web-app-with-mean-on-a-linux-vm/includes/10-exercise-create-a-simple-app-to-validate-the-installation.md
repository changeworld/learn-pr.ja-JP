In this unit, you're going to create a simple AngularJS application hosted in Node.js and use Express for routing. On the back end, MongoDB will serve as your data store. The application is a book database, where you will be able to list, add, and delete books.

> [!Important]
> This is a simple application. Its purpose is to test the newly installed MEAN stack. This application is not sufficiently secure or ready for production use.

## Connect to the VM

If you aren't still connected to your VM, run the following command. Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## Create the back end

1. Create a folder structure for your new sample application with the following command.

    ```bash
    mkdir ~/Books
    mkdir ~/Books/app
    mkdir ~/Books/public
    ```

    In your admin user's home location, you created a folder called "Books" to contain your project's app and its dependencies. Within that folder, you created an "app" folder to contain all your application resources and scripts. Finally, we will also create a "public" folder to hold all of the client-side files that will be served up directly to appropriate HTTP requests.

1. Install **Express** to handle routing of your HTTP requests, to decide what content to return to a user of your web application.

    Run the following command to add Express as a package for your web application to use.

      ```bash
      npm install express
      ```

1. Install **Mongoose** to help relay your book data between MongoDB and the HTTP request routing.

    The book information will be queried via REST API requests. To simplify the transfer of data in and out of MongoDB to our API, we will use Mongoose. Mongoose is a schema-based system for modeling data. We will be using it in our sample application to keep our data models consistent through the various GET, POST, and DELETE HTTP requests.

    Run the following command to add Mongoose as a package for your web application to use.

      ```bash
      npm install mongoose
      ```

1. Install **body-parser** to pre-process JSON request data for use in our Express routing.

    On the back end, `body-parser` will serve as a middleware between Node.js and Express for parsing incoming JSON request data.

    Run the following command to add `body-parser` as a package for your web application to use.

      ```bash
      npm install body-parser
      ```

    > [!TIP]
    > When installing multiple npm packages, you can include them all in a single command such as this:
    >
    > ```bash
    > npm install express mongoose body-parser
    > ```

1. Create the data model back end for your books web application using Mongoose.

    1. In the **app** folder within your **Books** application folder, create a new JavaScript file called **model.js** to contain your book's Mongoose-based data model.

        ```bash
        nano ~/Books/app/model.js
        ```

    1. Paste the following code into this new file to create our book schema using Mongoose.

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
        > To save the current file in **nano**, you need to press **Ctrl**+**O**. To exit **nano**, you need to press **Ctrl**+**X**.

        This code is connecting to a database called "Books" on the local VM's MongoDB server. It then creates a database document called "Book" with the schema defined by the `bookSchema` variable.

1. Create the Express routes for the application to handle the various HTTP requests.

    1. Within the **app** folder, create a new JavaScript file called **routes.js**.

        ```bash
        nano ~/Books/app/routes.js
        ```

    1. Paste the following code into this new file to establish the routes using Express.

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

        This code will create four routes for our application. The first three specify what to do when someone sends an API GET, POST, or DELETE request to the `/book` resource. The last one is a catch-all route to send the requester to the index page.

        Express can serve up HTTP responses directly in the route handling code, or it can serve up static content from files. We are doing both in this sample web application. We respond with JSON data for book API requests and with HTML data direct from the index.html file.

1. Create a **server.js** file in the **Books** folder to configure the Node.js hosting (using the Express routes).

    1. Back in the application root **Books** folder, create a new JavaScript file called **server.js**.

        ```bash
        nano ~/Books/server.js
        ```

    1. Paste the following code into this new file to configure your web application and start listening to the default HTTP port.

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

        This code creates the web application itself. It will serve static files from a folder named **public** (created next) and will use the routes defined in the previous step.

1. Create the front-end HTML and client-side JavaScript application.

    1. Within the **public** content folder, create a new JavaScript file called **script.js**.

        ```bash
        nano ~/Books/public/script.js
        ```

    1. Paste the following code into this new file to configure your client-side web application to handle communicating with your web server.

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

        This client-side AngularJS code creates a new angular application `myApp` containing one controller `myCtrl`. When the application is run in the viewer's browser, it will issue an HTTP GET request to retrieve the list of books in the database.

    1. Also within the **public** content folder, create a new HTML file called **index.html**.

        ```bash
        nano ~/Books/public/index.html
        ```

    1. Paste the following markup into this new file to set up your web application's HTML user interface.

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

        This code will create a simple HTML form with four fields to submit new book data and a table to display all the books already stored in the database. The various `ng-` HTML attributes will wire up the AngularJS code to the UI.

1. Test the full books web application.

    1. Start the application with Node.js with the following command.

        ```bash
        sudo node ~/Books/server.js
        ```

        This will start the back end of our application, which will then start listening on port 80 for incoming HTTP requests.

    1. Test the application functionality.

        Open your preferred browser, and navigate to the public IP address of your Azure VM as the URL.

        ```bash
        http://<vm-public-ip>
        ```

        If everything is in order, you should see a screen similar to this:

        The following screenshot displays the user interface to submit book details for storage in the MongoDB database.


        ![Screenshot of a web browser showing the data-entry form to add a book.](../media-draft/10-book-page.png)

    You should now be able to submit books to save to the MongoDB database. As well, you can see the full list of books loaded from the database.