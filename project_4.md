Start a new ec2 instance to boot with a Ubuntu Server in AWS.
![alt text](/images/01.png)


Let's make sure Ubuntu is Updated and Upgraded to the latest.
```bash
sudo apt update && sudo apt upgrade
```
![alt text](/images/1.png)

Add required certificates
```bash
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
Reading package lists... Done/setup_12.x | sudo -E bash -
```
![alt text](/images/2.png)

Install NodeJS on the server.
```bash
sudo apt install -y nodejs
```
![alt text](/images/02.png)

## MongoDB

Next, Install MongoDB Community edition

To do this, when to first import the Import MongoDB GPG public key.
```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
```
![alt text](/images/3.png)

Install MongoDB Server

```bash
sudo apt install -y mongodb
```
![alt text](/images/03.png)

Start Mongodb Server and verify it is running.
```bash
sudo service mongodb start
```
```bash
sudo systemctl status mongodb
```
![alt text](/images/4.png)

Install Node package manger ***npm***
```bash
sudo apt install -y npm
```
![alt text](/images/04.png)

To help process JSON files, we will install body-parser package.
```bash
sudo npm install body-parser
```
![alt text](/images/5.png)

Create directory for that would hold the application - named Books.
```bash
mkdir Books && cd Books
```
In the Books directory, initialize the npm project.

```bash
npm init
```
![alt text](/images/05.png)

Add a server.js file

```bash
nano server.js
```
![alt text](/images/6.png)

## Express
Time to install Express and create routes to the server.

Create Schema using Mongoose package 
```bash
sudo npm install express mongoose
```
![alt text](/images/06.png)

In the Books directory, create a apps folder.

```bash
mkdir apps && cd apps
```
Create a file called routes.js
```bash
nano routes.js
```
```bash
var Book = require('./models/book');
module.exports = function(app) {
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
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```
Create a routes.js file in it.
![alt text](/images/7.png)

In the ‘apps’ folder, create a folder named models.
```bash
mkdir models && cd models
```
Create a file called book.js and add the code below.
```bash
nano books.js
```
```bash
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```
Access routes with Angular.
In the Books directory, create a folder called public and in that folder, create a file called script.js and add the script below, save and exit.
```bash
mkdir public && cd public
```
```bash
nano script.js
```
![alt text](/images/07.png)

In the public folder, create a index.html file and add the code below.

```bash
nano index.html
```
```bash
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
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
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```
In the Books directory, run server.js
```bash
node server.js
```
![alt text](/images/8.png)
