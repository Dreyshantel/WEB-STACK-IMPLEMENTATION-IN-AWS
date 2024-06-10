# WEB STACK IMPLEMENTATION (MERN STACK)
### Overview 
This project is a documentation guide to setting up MERN (MongoBD, Express.Js, React, Node.Js) stack. The MERN stack is a popular set of technologies used for building modern web application. It's Cost effective, scalable, reliable and secure.

### Components
- **MongoDB**: A NoSQL database that stores data in flexible, JSON-like documents. MongoDB is designed for scalability and ease of use, making it a popular choice for applications that require fast, unstructured data storage.
- **Express.Js**: A lightweight web application framework for Node.js that provides robust features for building web and mobile applications. Express simplifies the process of handling HTTP requests and responses, making it easier to build APIs and manage server-side logic.
- **React**:  A front-end library developed by Facebook for building user interfaces. React allows developers to create reusable UI components and manage the state of the application efficiently. It uses a virtual DOM to improve performance and provides a declarative approach to building interactive UIs.
- **Node.Js**: A JavaScript runtime built on Chrome's V8 JavaScript engine that allows developers to run JavaScript on the server side. Node.js is known for its event-driven, non-blocking I/O model, which makes it suitable for building scalable and high-performance applications.

### SETTING UP THE EC2 INSTANCE
1. **Launch an ec2 instance:**
   - Sign in to AWS management console
   - Navigate to ec2 dashboard
   - Click on "Launch instance" and choose ubuntu server 20.04 LTS as operating system
![Screenshot (146)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/d8914ca9-eb8e-4e1c-9b22-fba47598cf3a)
![Screenshot (147)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/7c6b5e7c-4e5c-4ef6-a2f8-c14bd749d414)

2. **Configure Instance Details:**
   - Choose instance type, network, subnet, and other required settings
3. **Add storage**
   - Allocate storage space according to your requirement
4. **Add tags**
   - Optionally, add tags for better organization
5. **Configure Security Group**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on port 80 (HTTP), 433 (HTTPS), and port 22 (SSH) from your IP address
![Screenshot (141)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/f1735ab5-b8b0-437e-b7e6-f6bb5b8b6c45)
6. **Review and launch**
   - Review the configuration and launch the instance
![Screenshot (151)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/caa932eb-212e-476e-931d-6fdb0356baa2)
7. **Connect to the instance:**
   - Use window terminal to connect to the instance via SSH.
8. **SSH to the instance**
   ![Screenshot (160)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/a5ae72a6-e4e2-4777-8c0e-c2b0ad4d8f30)

### INSTALL MERN STACK
1. **Update package repository**

   ```
   sudo apt update
   ```
![Screenshot (162)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/626f8fea-46eb-476d-bcd9-77242fe8e943)

3. **Upgrade Ubuntu**
   
   ```
   sudo apt upgrade
   ```
5. **Get the location of Node.js software from Ubuntu repositories.**

 ```
 curl fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![Screenshot (163)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/2b6717ec-abb2-4f44-9aa5-6b906e215424)
![Screenshot (164)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/d1807d4b-1162-42c2-b668-cc25f23e95d6)


6. **Insatll Node.Js**

   ```
   sudo apt-get install -y nodejs
   ```
   
  - **Note** The command above install both node.js and npm. NPM is a package manager for node, like apt for ubuntu, it is used to install node modules & packages and to manage dependency
![Screenshot (165)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/92edbd23-8e8a-48b5-afc0-d33fa9caebaa)

8. **Verify node installation**

   ```
   node -v
   ```
   ```
   npm -v
   ```
![Screenshot (167)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/fed77704-5318-44b9-9334-3315b9bc66a9)

### Application Code Setup
1. **Create a new directory for your To-Do project.**
```
mkdir Todo
```
3. **Verify that the Todo directory is created**
   ```
   ls
   ```
4. **Change directory to newly created Todo directory**
   ```
   cd Todo
   ```
5. **Initialise your project so that a new package.json file will be created**
   
   ```
   npm init
   ```
![Screenshot (168)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/18903aff-41b9-4712-b781-cd3002b6e6b9)

### Install Express.Js
1. **To use express, install it using npm:**
   
   ```
   npm install express
   ```
   ![Screenshot (169)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/5ea42456-42a9-4a25-805b-a8189e793300)

3. **Create a new index.js file**
   
   ```
   touch index.js
   ```
4. **Confirm that your index.js file was successfully created**
   
  ```
ls
  ```
4. **Install the dotenv module**

   ```npm install dotenv```
   ![Screenshot (170)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/41e5c5f0-55cd-4f08-bbf9-767b38716532)
5. **Open the index.js file**
   ```
   vim index.js
   ```

6. **Copy and paste the code below inside it**
```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

![Screenshot (172)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/ec1e2a0d-e292-4c5d-9fe9-09b9f5fd78ad)

7. **Now it's time to start our server to see if it works**

![Screenshot (173)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/7dbb2247-5a1a-4e62-9e1c-86072f4aa7c5)
If everything goes well, you should see server running on port 5000 in your terminal

8. **Now we need to edit our inbound rule for instance to allow traffic through port 5000:**
   ![Screenshot (174)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/cd2382d6-fed7-4033-a780-cf1c44998a74)
9. **Now open your brower and try access your server followed by port 5000:**
    ![Screenshot (175)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/6525cd50-3d9b-4701-a790-bc4c13b2355b)

### Routes
There are three actions that the ToDo application needs to be able to do:

Create a new task Display list of all task Delete a completed task Each task was associated with some particular endpoint and used different standard HTTP request methods: POST, GET, DELETE.

For each task, routes were created which defined various endpoints that the ToDo app depends on. 
1. **Create a folder Routes:
   ```
   mkdir routes
   ```
2. **Enter the Route folder**
   ```
   cd routes
   ```
3. **Create a file api.Js:**
   ```
   touch api.js
   ```
4. **Open the file and copy and paste the code below in it:**
   ```
   vim api.js
   ```
the code to copy
```
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

});

module.exports = router;
```

![Screenshot (176)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/4ea1d6b7-78b0-4dbe-b974-43385c3db5be)

### MODELS
The model in a JavaScript application is crucial for managing and encapsulating the data and business logic. It serves as the central component that the rest of the application interacts with, ensuring that data flows correctly and consistently throughout the system. Whether in a frontend, backend, or full-stack context, the model plays a key role in the application's architecture.
1. **Change directory to Todo and Install mongoose a nodejs package that makes working with mongodb easier:**
   ```
   npm install mongoose
   ```
   ![Screenshot (178)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/397747f9-46d9-4ce7-ba70-57ef09ccbb92)
2. **.Create a new folder models and change into it:**
   ```
   mkdir models
   cd models
   ```
3. **Create a new file todo.js, open it and copy and paste the following code into it:**
   ```
   touch todo.js
   vim todo.js
   ```
copy the code below into it
```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Create schema for todo
const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
  }
});

// Create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
![Screenshot (179)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/95220021-b5fb-4e82-b651-1c677aef0f68)

4. **4.Now we need to update the routes from the file api.js in the 'routes' directory: change to the directory routes and open the file api.js delete the code inside the file , copy and paste the code below into it**
 ```
   const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {
  // This will return all the data, exposing only the id and action field to the client
  Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next);
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
  } else {
    res.json({
      error: "The input field is empty"
    });
  }
});

router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({"_id": req.params.id})
    .then(data => res.json(data))
    .catch(next);
});

module.exports = router;
```

### MongoDB database
mLab is a popular cloud database service that provided managed MongoDB hosting. It is known for its ease of use, robust features, and support for developers looking to deploy and manage MongoDB databases without worrying about the underlying infrastructure. However, mLab was acquired by MongoDB Inc., and its services were integrated into MongoDB Atlas. we will sign up an account.
![Screenshot (180)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/7e935c4a-21be-487d-acd2-eaa479ff811f)
1. **create a cluster with AWS as a provider and select a region close to you**
![Screenshot (181)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/61705ddc-0dc4-4d1e-b429-49304e2ffa20)
2. **create a Database and collection**
![Screenshot (209)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/20973703-8752-43c3-94d8-a3ed74b1b9bb)
3. **Create a file in your Todo directory and name it .env, open the file:**
   ```
   touch .env
   vim .env
   ```
4. **Add connection string to connect to MongoDB database**
   ```
   DB = ‘mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority’
   ```
   ![Screenshot (211)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/3636caf7-093b-4303-a5ee-9313911e89c0)
5. **Update the index.js to reflect the use of .env so that Node.js can connect to the database.**
```
vim index.js
```
delete existing code and copy the below code
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

// Connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log(`Database connected successfully`))
  .catch(err => console.log(err));

// Since mongoose promise is deprecated, we override it with Node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
  console.log(err);
  next();
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
![Screenshot (186)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/ab84fd81-78e4-41ca-a2c9-42d0159d1573)

6. **Start your server**
   ```
   node index.js
  ```
