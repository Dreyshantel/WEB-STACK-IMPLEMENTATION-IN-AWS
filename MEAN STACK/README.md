# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS
### Overview
This project is a documentation guide to setting up MEAN (MongoDB, Express, Angular, and Nodejs) stack. The MEAN stack is a popular full-stack JavaScript framework used for developing web applications.
### Components
1. **MongoDB** (Document Database): Stores and allow to retrieve data.
2. **Express** (Back-end application framework): Make request to database to read and write
3. **Angular** (Front-end application framework): Handle server and client requests
4. **Nodejs** (Javascript run time enviroment): Accept requests and displays answers to end users

### Setting up the EC2 Instance
1. **Launch an ec2 instance:**
   - Sign in to AWS management console
   - Navigate to EC2 dashboard.
   - Click on "Launch instance" and choose Ubuntu Server 20.04 LTS as the operating system
![Screenshot (302)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/3bc03066-5b0c-4256-9d98-fb611b862154)

2. **Configure Instance Details:**
   - Choose instance type, network, subnet, and other required settings.
   - Add Storage
   - Allocate storage space for according to your requirement
3. **Add Tags:**
   - Optionally, add tags for better organization
4. **Configure Security Group**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on port 80 (HTTP, 433 (HTTPS), 22 (SHH) from your IP address
     ![Screenshot (306)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/bae9f7bd-a0df-469d-91b6-260a0ad80399)

5. **Review and launch:**
   - Review the configuration and launch the instance.
     ![Screenshot (304)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/037cd845-4d61-4975-b3a0-f7c6a4d6b73c)
6. **Connect to the Instance**
   - use Windows terminal to connect to the instance via SSH.
![Screenshot (305)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/745fad7c-4871-49b4-853c-2c1b244113bd)
7. **SSH into the instance:**
```
ssh -i "Steghub_keypair.pem" ubuntu@ec2-51-21-134-189.eu-north-1.compute.amazonaws.com
```
   
### Installing MEAN Stack
**In this project we are going to implement a simple book register**
### Step 1: Install Nodejs

1 **Update package repository:**
```
sudo apt update
```
![Screenshot (307)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/971ff9c6-709a-47d4-8119-65a01b62b674)
2. **Upgrade Ubuntu**
```
sudo apt upgrade
```
3. **Add certificates**
```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![Screenshot (308)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/c03a1ea9-5ae4-4755-85ad-39d73b590d5c)

![Screenshot (309)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/07f29df0-24ea-45ec-8f3f-247235577e7e)


4. **Install NodeJs on the server:**
```
sudo apt-get install nodejs -y
```
![Screenshot (310)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/5126343d-c3a2-48c7-8096-680626c12f3f)

5. **Verify installation**
 ```
  node -v        // Gives the node version
  npm -v        // Gives the node package manager version
 ```
![Screenshot (312)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/ff8b79dc-66a9-4715-aa69-90355d89fd0a)

### Step 2: Installing MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

To import the MongoDB public GPG key, run the following command:
```
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```
**By running the command below, you ensure that your system can retrieve and install MongoDB packages from the official MongoDB repository.**
```
echo "deb [arch=amd64] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```
**Update packages and install MongoDB**
```
sudo apt-get update
sudo apt install -y mongodb
```
![Screenshot (313)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/fba12831-65d4-4da5-a067-718900ad7f99)
**Start the server**
```
sudo systemctl start mongod
```
**Enable MongoDB**
```
sudo systemctl enable mongod
```
**Check the status of mongodb**
```
sudo systemctl status mongod
```
![Screenshot (314)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/c11704e9-49a1-4bb6-a0dc-19b07ad1ad6e)
**Install npm - Node package manager**
```
sudo apt install -y npm
```
Install 'body-parser package We need 'body-parser' package to help us process JSON files passed in requests to the server.
```
sudo npm install body-parser
```
**Create a folder named 'Books'**
```
mkdir Books
```
**change directory to the Books folder**
```
cd Books
```
**In the Books directory, Initialize npm project**
```
npm init
```
![Screenshot (315)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/66ed21ee-f4bb-4502-9125-858bc990cde8)
**Add and open a file to it named server.js**
```
touch server.js && vim server,js
```
**Copy and paste the web server code below into the server.js file**
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose'); // Make sure mongoose is installed and required
const path = require('path'); // To handle static file serving
const app = express();

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.error('MongoDB connection error:', err));

// Middleware
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, 'public')));

// Routes
require('./apps/routes')(app);

// Start the server
app.set('port', 3300);
app.listen(app.get('port'), () => {
  console.log('Server up: http://localhost:' + app.get('port'));
});
```
![Screenshot (316)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/73da23bc-df69-4cec-89be-192fed94907c)

### Step 3: Install Express and set up routes to the server
Express will be used to pass book information to and from our MongoDB database. Install express and mongoose
```
sudo npm install express mongoose
```
![Screenshot (318)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/34a56246-c5f9-4bf5-8ecb-8ef90f784cc7)
In 'Books' folder, create a folder named apps
```
mkdir apps
```
enter the apps folder
```
cd apps
```
**Create and open a file named routes.js**
```
vi routes.js
```
copy and paste the following code into it

```
 const Book = require('./models/book');
const path = require('path');

module.exports = function(app) {
  // Get all books
  app.get('/book', async (req, res) => {
    try {
      const books = await Book.find({});
      res.json(books);
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Add a new book
  app.post('/book', async (req, res) => {
    try {
      const book = new Book({
        name: req.body.name,
        isbn: req.body.isbn,
        author: req.body.author,
        pages: req.body.pages
      });
      const result = await book.save();
      res.json({
        message: "Successfully added book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Update a book
  app.put('/book/:isbn', async (req, res) => {
    try {
      const updatedBook = await Book.findOneAndUpdate(
        { isbn: req.params.isbn },
        req.body,
        { new: true }
      );
      if (!updatedBook) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully updated the book",
        book: updatedBook
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Delete a book
  app.delete('/book/:isbn', async (req, res) => {
    try {
      const result = await Book.findOneAndRemove({ isbn: req.params.isbn });
      if (!result) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Serve static files
  app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../public', 'index.html'));
  });
};
```

![Screenshot (319)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/8e0a8158-26b4-425b-9fd4-a827e906d3bf)
In the 'apps' folder, create a folder named models
```
mkdir models
```
enter the models folder
```
cd models
```
Create and open a file named book.js
![Screenshot (323)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/ada79fed-5eda-4c52-a8c2-7b4a2079b77a)

```
vi book.js
```
copy the code below into it:
```
 const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  name: { type: String, required: true },
  isbn: { type: String, required: true, unique: true },
  author: { type: String, required: true },
  pages: { type: Number, required: true }
});

module.exports = mongoose.model('Book', bookSchema);
```
![Screenshot (322)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/5b9217b2-9fea-4f18-b93b-34ad0ceeaec4)

### Step 4 - Access the routes with AngularJS
AngularJS will be used to connect our web page with Express and perform actions on our book register Change directory to the books
```
cd ../..
```
Create and open a folder named public
```
mkdir public && cd public
```
Add and open a file named script.js
```
vi script.js
```
Copy and paste the Code below (controller configuration defined) into the script.js file.
```
 var app = angular.module('myApp', []);

app.controller('myCtrl', function($scope, $http) {
  // Get all books
  function getAllBooks() {
    $http({
      method: 'GET',
      url: '/book'
    }).then(function successCallback(response) {
      $scope.books = response.data;
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  }

  // Initial load of books
  getAllBooks();

  // Add a new book
  $scope.add_book = function() {
    var body = {
      name: $scope.Name,
      isbn: $scope.Isbn,
      author: $scope.Author,
      pages: $scope.Pages
    };
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
      // Clear the input fields
      $scope.Name = '';
      $scope.Isbn = '';
      $scope.Author = '';
      $scope.Pages = '';
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Update a book
  $scope.update_book = function(book) {
    var body = {
      name: book.name,
      isbn: book.isbn,
      author: book.author,
      pages: book.pages
    };
    $http({
      method: 'PUT',
      url: '/book/' + book.isbn,
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Delete a book
  $scope.delete_book = function(isbn) {
    $http({
      method: 'DELETE',
      url: '/book/' + isbn
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };
});
```
![Screenshot (324)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/2dda166a-253f-41fa-9c27-439d1f20860b)
**In 'public' folder, create and open a file named index.html**
```
vi index.html
```
copy the code below into it.
```
 <!DOCTYPE html>
<html ng-app="myApp" ng-controller="myCtrl">
<head>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
  <script src="script.js"></script>
  <style>
    /* Add your custom CSS styles here */
  </style>
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
    <div ng-if="successMessage">{{ successMessage }}</div>
    <div ng-if="errorMessage">{{ errorMessage }}</div>
  </div>
  <hr>
  <div>
    <table>
      <tr>
        <th>Name</th>
        <th>Isbn</th>
        <th>Author</th>
        <th>Page</th>
        <th>Action</th>
      </tr>
      <tr ng-repeat="book in books">
        <td>{{ book.name }}</td>
        <td>{{ book.isbn }}</td>
        <td>{{ book.author }}</td>
        <td>{{ book.pages }}</td>
        <td><button ng-click="del_book(book)">Delete</button></td>
      </tr>
    </table>
  </div>
</body>
</html>
```
![Screenshot (325)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/035f7164-6a34-48a3-b1b7-6439b3cd0596)
Change the directory back to books
```
cd ..
```
Start the server by running this command
```
node server.js
```
![Screenshot (327)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/803a8ab2-9bf8-437e-96bb-8f598f4c0934)
The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally
**Edit the inbound rule to allow all traffic into port 3300**

