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
### Testing Backend code without frontend using RESTful API
Postman will be used to test the backend code . the endpoints were tested using POST , GET requets Set the header send a post request
![Screenshot (212)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/5fcf6af5-3e31-4d14-be96-50a2f78cb68c)
send a get request
![Screenshot (213)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/c9e71e40-bcb7-4a52-818f-1f28b05695ac)

### STEP:2 Frontend Creation
now is time to create the client-side of the application in the Todo folder of your application run the code below
```
npx create-react-app client
```
This command created a new folder in the Todo directory called client, where all the react code was added.

### Running a React App
Before testing the react app, the following dependencies needs to be installed in the project root directory. Install concurrently. It is used to run more than one command simultaneously from the same terminal window.
```
npm install concurrently --save-dev
```
![Screenshot (215)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/5ba3fab6-323f-4eab-b5c4-804b7ea23858)
Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
```
npm install nodemon --save-dev
```
![Screenshot (216)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/0538744c-d084-4a8e-a92f-d0db392e3e86)
In Todo folder open the package.json file, change the highlighted part of the below screenshot and replace with the code below:
```
"scripts": {
  "start": "node index.js",
  "start-watch": "nodemon index.js",
  "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
}
```
![Screenshot (217)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/6e330f8b-50ce-41b8-95f0-4ab9a73dcdb5)

### Change proxy package in package.json
change directory
```
cd client
```
Open the package.json file
```
vim package.json
```
Add the key value pair in the package.json file
```
“proxy”: “http://localhost:5000”
```
![Screenshot (244)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/265ace1c-ca30-43bb-a6ff-1c96226b91e4)
The whole purpose of adding the proxy configuration above is to make it possible to access the application directly from the browser by simply calling the server url like http://locathost:5000 rather than always including the entire path like http://localhost:5000/api/todos
Ensure you are inside the Todo directory, and simply do:
```
sudo npm run dev
```
![Screenshot (242)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/b224803a-5dae-4be2-b9bc-d273509ab2e5)
![Screenshot (243)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/74ac20cb-0d5f-4643-92db-59cf2d8b737b)
The app opened and started running on localhost:3000 Note: In order to access the application from the internet, TCP port 3000 had been opened on EC2.

### Creating React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For the Todo app, there are two stateful components and one stateless component. From Todo directory, run:
```
cd client
```
1. **change directory to src**
```
cd src
```
2. **Inside your src folder, create another folder called “components”**
```
mkdir components
```
3. **Inside the ‘components’ directory create three files “Input.js”, “ListTodo.js” and “Todo.js”.**
```
touch Input.js ListTodo.js Todo.js
```
![Screenshot (248)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/1718961a-efe2-4bf7-bd1f-fef5cb3b8f2c)
Open Input.js file
```
vim Input.js
```
Paste the following code in it.
```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: ""
  }

  handleChange = (event) => {
    this.setState({ action: event.target.value });
  }

  addTodo = () => {
    const task = { action: this.state.action };

    if (task.action && task.action.length > 0) {
      axios.post('/api/todos', task)
        .then(res => {
          if (res.data) {
            this.props.getTodos();
            this.setState({ action: "" });
          }
        })
        .catch(err => console.log(err));
    } else {
      console.log('Input field required');
    }
  }

  render() {
    let { action } = this.state;
    return (
      <div>
        <input type="text" onChange={this.handleChange} value={action} />
        <button onClick={this.addTodo}>add todo</button>
      </div>
    );
  }
}

export default Input;
```
![Screenshot (248)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/aa849642-61c2-4d83-8a07-0a8c5eeaa46d)
In oder to make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.
Move to the client folder
```
cd ../..
```
run the below command to Install Axios
```
npm install axios
```
![Screenshot (250)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/b5c7ecf0-6e78-450d-956b-7a7557166297)
Go to components directory
```
cd src/components
```
After that open the ListTodo.js
```
vim ListTodo.js
```
Copy and paste the following code:
```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {
  return (
    <ul>
      {
        todos && todos.length > 0 ? (
          todos.map(todo => {
            return (
              <li key={todo._id} onClick={() => deleteTodo(todo._id)}>
                {todo.action}
              </li>
            );
          })
        ) : (
          <li>No todo(s) left</li>
        )
      }
    </ul>
  );
}

export default ListTodo;
```
![Screenshot (251)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/774b2b86-ebc3-41a4-9d2e-de9b0139183e)

in the Todo.js file, write the following code
```
vim Todo.js
```
paste the code below
```
import React, { Component } from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {
  state = {
    todos: []
  }

  componentDidMount() {
    this.getTodos();
  }

  getTodos = () => {
    axios.get('/api/todos')
      .then(res => {
        if (res.data) {
          this.setState({
            todos: res.data
          });
        }
      })
      .catch(err => console.log(err));
  }

  deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if (res.data) {
          this.getTodos();
        }
      })
      .catch(err => console.log(err));
  }

  render() {
    let { todos } = this.state;
    return (
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos} />
        <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
      </div>
    );
  }
}

export default Todo;
```
![Screenshot (252)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/d8903ed1-0f53-4da3-9e58-2e48e7583f81)
 We need to make a little adjustment to our react code. Delete the logo and adjust our App.js to look like this Move to src folder
```
cd ..
```
Ensure to be in the src folder and run:
```
vim App.js
```
paste the code below
```
import React from 'react';
import Todo from './components/Todo';
import './App.css';

const App = () => {
  return (
    <div className="App">
      <Todo />
    </div>
  );
}

export default App;
```
![Screenshot (253)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/8d80d1bd-23a8-429b-8e5e-71799a3a7014)

In the src directory, open the App.css
```
vim App.css
```
copy and paste the code below
```
.App {
  text-align: center;
  font-size: calc(10px + 2vmin);
  width: 60%;
  margin-left: auto;
  margin-right: auto;
}

input {
  height: 40px;
  width: 50%;
  border: none;
  border-bottom: 2px #101113 solid;
  background: none;
  font-size: 1.5rem;
  color: #787a80;
}

input:focus {
  outline: none;
}

button {
  width: 25%;
  height: 45px;
  border: none;
  margin-left: 10px;
  font-size: 25px;
  background: #101113;
  border-radius: 5px;
  color: #787a80;
  cursor: pointer;
}

button:focus {
  outline: none;
}

ul {
  list-style: none;
  text-align: left;
  padding: 15px;
  background: #171a1f;
  border-radius: 5px;
}

li {
  padding: 15px;
  font-size: 1.5rem;
  margin-bottom: 15px;
  background: #282c34;
  border-radius: 5px;
  overflow-wrap: break-word;
  cursor: pointer;
}

@media only screen and (min-width: 300px) {
  .App {
    width: 80%;
  }

  input {
    width: 100%;
  }

  button {
    width: 100%;
    margin-top: 15px;
    margin-left: 0;
  }
}

@media only screen and (min-width: 640px) {
  .App {
    width: 60%;
  }

  input {
    width: 50%;
  }

  button {
    width: 30%;
    margin-left: 10px;
    margin-top: 0;
  }
}
```
![image](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/79fc5c57-c27e-4d69-9016-246757a22128)
In the src directory, open the index.css
```
vim index.css
```
copy and paste the code below
```
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}
```
![Screenshot (255)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/80c84b6c-253c-4872-aee9-98f76fe37308)
**Go to the Todo directory**
```
cd ../..
```
**Run the code below**
```
sudo npm run dev
```
![Screenshot (258)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/b538a266-46d7-45c0-99f6-4d7b3d372340)
![Screenshot (259)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/b3ca63fc-923e-421a-971f-cbb021bc54c8)
### Now our Todo App is ready for use and fully functional with creating a task , deleting a task and viewing all the task.
![Screenshot (260)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/eb76da90-8736-47d5-b7d2-1b1ea6e0c865)
### Conclusion:
Creating a Todo application using the MERN stack has been an extensive project, covering critical aspects of full-stack web development. Here’s what I’ve accomplished:

1. **Environment Setup and Configuration:** I established the groundwork by installing and configuring Node.js, MongoDB, and relevant dependencies.

2. **RESTful API Design and Implementation:** Leveraging Express.js, I developed a backend that effectively manages CRUD operations, enabling seamless todo management.

3. **MongoDB Integration:** Utilizing Mongoose, I defined schemas and models for structured interactions with the MongoDB database.

4. **Frontend Development with React:** Using React.js, I crafted a dynamic and responsive user interface, facilitating intuitive user interactions.

5. **Frontend-Backend Integration:** By configuring proxies and implementing asynchronous API calls, I successfully linked the frontend and backend, creating a fully functional application.

6. **State Management:** Within React components, I proficiently managed state to maintain data consistency and enhance user experience.

7. **Error Handling and Debugging:** Implementing robust logging and error-handling mechanisms ensured the application’s reliability and maintainability.

8. **API Testing:** Verified API functionality using tools like Postman to ensure seamless operation.

Through this project, I've gained valuable experience in building scalable, responsive web applications using the MERN stack.




