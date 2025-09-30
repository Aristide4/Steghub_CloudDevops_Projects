## MERN Web Stack Implementation in AWS

### Introduction

The **MERN stack** is widely used for developing dynamic and feature-rich web applications. It consists of four key JavaScript-based technologies:

- **MongoDB**: A NoSQL document database offering flexible and scalable data storage.
- **Express.js**: A minimalistic web framework for Node.js, designed to simplify building APIs and web applications.
- **React.js**: A powerful library for building highly interactive user interfaces.
- **Node.js**: A JavaScript runtime that enables the execution of JavaScript code on the server side.

This guide offers a detailed walkthrough of how to set up and integrate each component of the MERN stack for building robust web applications.

## Step 0: Prerequisites

__1.__ A t3.micro EC2 instance with Ubuntu 24.04 LTS (HVM) was launched in the eu-north-1 region using the AWS Management Console.

![Launch Instance] <img width="1405" height="738" alt="image" src="https://github.com/user-attachments/assets/18c6acb2-9630-4976-ba27-3bf009db82bb" />

__2.__ An SSH key pair named __henrylearndevops__ earlier created was used to access the instance via port 22.

__3.__ The security group was configured with the following inbound rules:

- Allow HTTP traffic on port 80 from anywhere on the internet.
- Allow HTTPS traffic on port 443 from anywhere on the internet.
- Allow SSH traffic on port 22 from any IP address (default setting).

__4.__ After downloading the private SSH key, Windows Terminal was used to connect to the instance, using the following command:

```bash
ssh -i "lamp.pem" ubuntu@ec2-13-60-187-88.eu-north-1.compute.amazonaws.com
```

In this command, __username=ubuntu__ and __public IP address=ec2-13-53-214-3.eu-north-1.compute.amazonaws.com__.

## Step 1 - Backend Configuration

### 1. Update and Upgrade the Server’s Package Index

First, update and upgrade the server's package index to ensure you're working with the latest software versions.

```bash
sudo apt update
```
```bash
sudo apt upgrade -y
```
<img width="1156" height="302" alt="image" src="https://github.com/user-attachments/assets/d3afd2ff-3257-4df4-bda1-ca999aae7c3a" />

### 2. Add Node.js Repository

Retrieve the Node.js installation script from the Ubuntu repositories.

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
### 3. Install Node.js

Install Node.js along with npm using the following command:

```bash
sudo apt-get install -y nodejs
```

<img width="1407" height="602" alt="image" src="https://github.com/user-attachments/assets/322207ad-9dbd-41ca-b207-0341a10a1515" />


> **Note:** This command installs both Node.js and npm (Node Package Manager). npm is used to manage Node modules and packages, similar to how `apt` manages packages in Ubuntu. It also handles dependency conflicts.

### 4. Verify Installation

Confirm that both Node.js and npm have been installed correctly:

```bash
node -v        // Displays Node.js version
npm -v         // Displays npm version
``

## Application Code Setup

### 1. Create and Initialize the Project Directory

Create a directory for your to-do project and initialize it with npm:

```bash
mkdir Todo 
cd Todo
npm init
```
<img width="1270" height="627" alt="image" src="https://github.com/user-attachments/assets/d69a7f66-12b7-43a5-b516-79af502b3f6e" />


Initializing the project creates a `package.json` file, which contains metadata about your application, including the dependencies it needs to run. Follow the prompts, pressing "Enter" to accept the defaults, and confirm the creation of `package.json` by typing "yes".


### Install Express.js

Express.js is a lightweight framework for Node.js that simplifies web application development by abstracting complex tasks. It helps define routes in your application based on HTTP methods (GET, POST, etc.) and URLs.

#### 1. Install Express with npm

Install Express by running the following command:

```bash
npm install express
```
<img width="1222" height="241" alt="image" src="https://github.com/user-attachments/assets/2da4cf5c-29e7-430f-8bec-38f04d9ea24e" />


#### 2. Create and Verify `index.js`

Create an `index.js` file and confirm its existence with `ls`:

```bash
touch index.js && ls
```
<img width="962" height="77" alt="image" src="https://github.com/user-attachments/assets/96c843eb-9ac8-432e-8a23-0c80ed001b61" />


#### 3. Install dotenv Module

The `dotenv` module is used to load environment variables from a `.env` file into `process.env` in Node.js.

```bash
npm install dotenv
```
<img width="1134" height="155" alt="image" src="https://github.com/user-attachments/assets/cdbde1bb-096f-49fc-9507-d8b413c4a895" />


#### 4. Set Up `index.js`

Open the `index.js` file for editing:

```bash
vim index.js
```
Insert the following code:

```javascript
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
<img width="1193" height="454" alt="image" src="https://github.com/user-attachments/assets/e24a58e9-500e-4e61-abd4-65ba9d82422a" />


> **Note:** The application is configured to run on port 5000. This will be used when accessing the server through a browser.

#### 5. Start the Server

To check if the setup works, start the server:

```bash
node index.js
```
<img width="916" height="140" alt="image" src="https://github.com/user-attachments/assets/f6b85f68-fcb6-4416-830f-0acbe80d4673" />


Ensure that port 5000 is opened in your EC2 security group.

> **Added port 5000 to security group.**

<img width="1297" height="569" alt="image" src="https://github.com/user-attachments/assets/c3c7d6ca-da6d-4eee-883f-17afe6edf6cd" />

#### Access the Server

Access the Express server using your public IP followed by port 5000:

> http://13.60.187.88:5000

<img width="1233" height="295" alt="image" src="https://github.com/user-attachments/assets/51f8ec70-ec3e-4bd4-b328-a0b12f02ee97" />


---

## Routes

The To-Do application requires three key actions:
- Create a new task
- Display all tasks
- Delete a completed task

Each of these tasks corresponds to an HTTP request method: `POST`, `GET`, and `DELETE`.

#### 1. Create Routes

First, create a `routes` folder and an `api.js` file to define endpoints for the To-Do app:

```bash
mkdir routes && cd routes && touch api.js
```
<img width="948" height="139" alt="image" src="https://github.com/user-attachments/assets/a9a97664-d0c6-46bd-a3d1-cd102900879c" />


Open the `api.js` file and insert the following code:

```javascript
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => { });

router.post('/todos', (req, res, next) => { });

router.delete('/todos/:id', (req, res, next) => { });

module.exports = router;
```
<img width="601" height="367" alt="image" src="https://github.com/user-attachments/assets/70b4f1f5-9223-4e07-9da4-fb0b4faa1b60" />


---

## Models

A model is the backbone of a JavaScript application and is responsible for handling data logic. In this case, a model will define how our MongoDB database stores and retrieves tasks.

To manage this, we'll use Mongoose, a Node.js package that simplifies interactions with MongoDB.

#### 1. Install Mongoose

Navigate back to the root directory (`Todo`) and install Mongoose:

```bash
cd ..
npm install mongoose
```
<img width="1001" height="257" alt="image" src="https://github.com/user-attachments/assets/af222ff6-993f-4ffb-8a4b-8af87bc10d89" />


#### 2. Create Model Schema

Next, create a `models` folder and define the schema for a To-Do item in `todo.js`:

```bash
mkdir models && cd models && touch todo.js && ls
```
<img width="1053" height="160" alt="image" src="https://github.com/user-attachments/assets/eec8946b-6d2b-4138-8cf7-63f7676c372c" />


Open the `todo.js` file and paste the following code:

```javascript
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
#### 3. Update Routes with the Model

Update the `api.js` file to use the new Mongoose model. Open `api.js` and replace its content with the following code:

```bash
vim api.js
```

Replace with this code:

```javascript
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
  Todo.findOneAndDelete({ "_id": req.params.id })
    .then(data => res.json(data))
    .catch(next);
});

module.exports = router;
```
### MongoDB Database Setup

__mLab__ provides a Database-as-a-Service (DBaaS) solution for MongoDB. MongoDB offers two cloud database management components: mLab and Atlas. In 2018, MongoDB acquired mLab, and the two systems were merged. Now, __mLab.com__ redirects to the __MongoDB Atlas website__.

#### Step 1: Create a MongoDB Database and Collection on mLab

First, create a MongoDB cluster and select the AWS cloud provider in the Stockholm (eu-north-1) region.

<img width="1134" height="478" alt="image" src="https://github.com/user-attachments/assets/9df30577-d1c6-4b6c-9585-b62b665f3981" />


Choose the region for your cloud provider:

![Cloud Region](./images/db-cloud-provider-region.png)

#### Step 2: Create a `.env` File

In your project directory, create a `.env` file to store your MongoDB connection string.

```bash
touch .env && vim .env
```

Add the following connection string to the `.env` file, replacing `<username>`, `<password>`, `<network-address>`, and `<dbname>` with your actual MongoDB details:

```bash
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
```
#### Step 3: Update `index.js` to Use Environment Variables

Open `index.js` and update it to connect to the MongoDB database using the connection string from the `.env` file.

```bash
vim index.js
```

Delete the existing content and replace it with the following code:

```bash
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();
const port = process.env.PORT || 5000;

// Connect to the MongoDB database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('Database connected successfully'))
  .catch(err => console.log(err));

// Override mongoose promises with Node's promise
mongoose.Promise = global.Promise;

// Middleware for CORS
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

// Parse incoming requests
app.use(bodyParser.json());

// Routes
app.use('/api', routes);

// Error handling middleware
app.use((err, req, res, next) => {
  console.log(err);
  next();
});

// Start the server
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
By using environment variables in a `.env` file, you keep sensitive data like connection strings secure and follow best practices by separating configuration details from your code.

#### Step 4: Start Your Server

Start the server by running:

```bash
node index.js
```

<img width="1220" height="122" alt="image" src="https://github.com/user-attachments/assets/da61feb7-67ea-445d-b34f-35c0b148999d" />


You might see a deprecation warning. To silence it, you can remove `{ useNewUrlParser: true, useUnifiedTopology: true }` from the MongoDB connection code.


### Testing Backend Code Without a Frontend Using a RESTful API

Postman was used to test the backend functionality. The API endpoints were tested by sending the required data. For the endpoints that accept a request body, JSON was used, matching the fields defined in the backend code.

#### Step 1: Open Postman and Set the Header

The API was accessed using the following URL:

```bash
http://13.60.187.88:5000/api/todos
```

### Creating POST Requests to the API

Send a POST request to add new tasks to the To-Do list.

### Check the Database Collections

View the data stored in the MongoDB collections to verify the tasks were successfully created.

### Sending GET Requests to the API

A GET request retrieves all existing tasks from the To-Do application. The backend fetches these records from the database and returns them as a response to the GET request.

### Creating DELETE Requests to the API

Delete a task from the To-Do list by sending a DELETE request.

### Check the Database Collections Again

Verify the task was successfully removed from the database.

### Step 2 - Frontend Creation

Now it's time to create a user interface for the web client (browser) to interact with the application via the API.

#### 1. In the same root directory as your backend code (the Todo directory), run:

```bash
npx create-react-app client
```
<img width="581" height="715" alt="image" src="https://github.com/user-attachments/assets/a28e12e5-fdd9-4d2b-9c5e-5d41b3d0f841" />


This will create a new folder in the Todo directory named `client`, where all the React code will be placed.

### Running the React App

Before testing the React app, the following dependencies need to be installed in the project root directory.

- __Install concurrently__: Used to run multiple commands simultaneously from the same terminal.
```bash
npm install concurrently --save-dev
```
<img width="1197" height="259" alt="image" src="https://github.com/user-attachments/assets/d5200873-aef1-4198-bc3c-bfa8da7ac6cb" />


- __Install nodemon__: A tool to monitor server code and automatically restart the server whenever changes are detected.
```bash
npm install nodemon --save-dev
```
<img width="1019" height="199" alt="image" src="https://github.com/user-attachments/assets/5125a7df-eb4f-4496-8dc5-9a0b477ecffc" />


- In the Todo directory, open the `package.json` file and update the scripts section as shown below:
```bash
"scripts": {
  "start": "node index.js",
  "start-watch": "nodemon index.js",
  "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
}
```
### Configure Proxy in package.json

- Navigate to the “client” directory:
```bash
cd client
```

- Open the `package.json` file:
```bash
vim package.json
```
Add the following key-value pair to the `package.json` file:
```bash
"proxy": "http://localhost:5000"
```
The purpose of this proxy configuration is to allow the browser to access the server directly via `http://localhost:5000`, rather than needing to include the full path like `http://localhost:5000/api/todos`.

To run the app, ensure you're inside the Todo directory, and then execute:
```bash
npm run dev
```
<img width="1257" height="597" alt="image" src="https://github.com/user-attachments/assets/924d75e4-69c5-4859-bdb0-38b138181410" />

The app will start running on `localhost:3000`.

__Note__: TCP port 3000 was opened on the EC2 instance to allow internet access to the application.

<img width="1284" height="691" alt="image" src="https://github.com/user-attachments/assets/1ab1e905-3bd9-47d5-825f-aa4acdf6b9b2" />

## Creating React Components

One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For the Todo app, there are two stateful components and one stateless component. From Todo directory, run:

```bash
cd client
```

Move to the “src” directory
```bash
cd src
```

__2.__ __Inside your src folder, create another folder called “components”__

```bash
mkdir components
```
Move into the components directory
```bash
cd components
```

__3.__ __Inside the ‘components’ directory create three files “Input.js”, “ListTodo.js” and “Todo.js”.__
```bash
touch Input.js ListTodo.js Todo.js
```
#### Open Input.js file
```bash
vim Input.js
```
Paste in the following:

```javascript
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

In oder to make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

Move to the client folder
```bash
cd ../..
```
__Install Axios__
```bash
npm install axios
```
#### Go to components directory
```bash
cd src/components
```

#### After that open the ListTodo.js

```bash
vim ListTodo.js
```
Copy and paste the following code:

```javascript
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

#### Then in the Todo.js file, write the following code

```bash
vim Todo.js
```

```javascript
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
__We need to make a little adjustment to our react code. Delete the logo and adjust our App.js to look like this__

### Move to src folder
```bash
cd ..
```

Ensure to be in the src folder and run:
```bash
vim App.js
```

#### Copy and paste the following code

```javascript
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
####  In the src directory, open the App.css

```bash
vim App.css
```

Paste the following code into it

```css
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

#### In the src directory, open the index.css

```bash
cd ..
vim index.css
```

#### Copy and paste the code below:

```css
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

#### Go to the Todo directory
```bash
cd ../..
```

Run:
```bash
npm run dev
```

At this point, the To-Do app is ready and fully functional with the functionality discussed earlier: Creating a task, deleting a task, and viewing all the tasks.

### Conclusion

The MERN stack project successfully demonstrates the power of combining MongoDB, Express.js, React, and Node.js to build a modern full-stack web application. Through this project, we implemented a seamless integration between the frontend and backend, enabling smooth user interactions and efficient data handling.



