## Deploying a MERN Stack Application on AWS Cloud
A MERN stack consists of a collection of technologies used for the development of web applications. It comprises of MongoDB, Express, React and Node which are all JavaScript technologies used for creating full-stack applications and dynamic websites.

MongoDB: is a document-oriented NoSQL database technology used in storing data in the form of documents.

React: a javascript library used for building interactive user interface based on components.

Express: its a web application framework of Node js which is used for building server-side part of an appication and also creating Restful APIs.

Node: Node.js is an open-source, cross-platform runtime environment for building fast and scalable server-side and networking applications.
## Creating EC2 Instances
I log on to my AWS Cloud Services and create an EC2 Ubuntu VM instance MyMERN. When creating an instance, I choose keypair authentication and download private key(*.pem) on my local computer.
![ec2status](https://github.com/Oolabanji/test_/assets/136812420/1d1fa3b6-3721-4235-8f4f-73feced465dd)
On windows terminal, I cd into the directory containing the downloaded private key. and i run the below command to log into the instance via ssh:

ssh -i ben.pem ubuntu@172-31-38-140
Then i ran sudo apt update and sudo apt upgrade to update all default ubuntu dependencies to ensure compatibility during package installation.
![aptupdate](https://github.com/Oolabanji/test_/assets/136812420/8f067611-826f-44cb-becb-99a5c598284a)
![aptupgrade](https://github.com/Oolabanji/test_/assets/136812420/2cf1008d-d52b-4877-a45c-89958b220a11)
Next I installed nodejs, I got node js for the unbuntu repository using the following command. curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

Then I run a node install
sudo apt-get install -y nodejs
Then I run a node install
'sudo apt-get install -y nodejs'

![installnodejs](https://github.com/Oolabanji/test_/assets/136812420/f20ea54a-d23c-4159-97cf-5f9649a355d8)

![installnodejs2](https://github.com/Oolabanji/test_/assets/136812420/d751d27e-1ca4-4973-ba8f-b2f4b152ff7f)
I verify the node installation with the command below

'node -v'
I also verify the node installation with the command below

'npm -v' 
### Application Code Setup
I created a new directory for my To-Do project:

'mkdir Todo'
Inside the directory I instantiated the project using npm init. This enables javascript to install packages useful for spinning up our application.

![testnodejsandnpm](https://github.com/Oolabanji/test_/assets/136812420/c7837010-2bb5-4e9e-b41c-0cd4f9be8694)
### Express installation
I installed express which is nodejs framework and will be helpful when creating routes for our application via HTTP requests.
'npm install express'
![installexpress](https://github.com/Oolabanji/test_/assets/136812420/f4f27406-55e9-4d80-98a6-6d08eb08921d)
I created an index.js file which contain code useful for spinning up the express server, then I installed the dotenv module which is a module that loads environment variables from a .env file into process.env. The .env files are useful for hiding important credentials which shouldnt be exposed.
'npm install dotenv.'

Inside the index.js file, I  typed the following the code as seen in the image below

![indexjs](https://github.com/Oolabanji/test_/assets/136812420/f4c02dc3-ab2e-4be9-8ded-ca1017168615)
Now I started the server to see if it works. I open the terminal in the same directory as the index.js file and type:

'node index.js'
Allow the port as part of the inbound security rules in the EC2 instance to ensure that the server is visible via the internet.
![adjustsecurity](https://github.com/Oolabanji/test_/assets/136812420/f86ae7a2-2a65-4b71-95e1-8a909aeb9911)
I open up the browser and try to access your server’s Public IP or Public DNS name followed by port 5000:
http://3.17.206.59:5000
![welcomeexpress](https://github.com/Oolabanji/test_/assets/136812420/50898714-a014-4af0-bc10-22c2717c034f)
### Defining Routes For our Application
I created a routes folder which contained code pointing to the three main endpoints used in a todo application. This will contain the post,get and delete requests which will be helpful in interacting with our client_side and database via restful apis.

'mkdir routes'
'cd routes'
'touch api.js'
I opened the file with the command below

nano api.js
Copy below code in the file.

![apijs](https://github.com/Oolabanji/test_/assets/136812420/be648cef-632f-4d6d-a53f-ebed85746067)
### Creating models
Here I will be creating the models directory which will be used to define the database schema. A Schema is a blueprint of how our database will be structured which include other fields which may not be required to be stored in the database.

Inside the todo directory, I run npm install mongoose to install mongoose.

Then I created a models directory and then created a file in it todo.js, then write the below code inside the todo.js file
I opened the file created with nano todo.js then paste the code below in the file:

![todojs](https://github.com/Oolabanji/test_/assets/136812420/c297056c-08a5-42d0-b1b3-d8819b6e7852)
Since we have defined a schema for how our database should be structured, we then update the code in our api.js to fire specific actions when an endpoint is called.

![apijs1](https://github.com/Oolabanji/test_/assets/136812420/fab37a5b-e3b6-4e6f-8818-0b7fda64eb2c)
### Creating a MongoDB Database
I need a database where i will store  data. For this  maked use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, i signed up for a shared clusters free account, which is ideal for the use case. 
![mongoDB](https://github.com/Oolabanji/test_/assets/136812420/d6f2caa3-ad51-4ca7-80f4-641f6fc1ab8b)


![mern_db](https://github.com/Oolabanji/test_/assets/136812420/820d018a-e713-47c0-8f20-184b3ad049a6)
To connect mongoose(application_db) to the database service, I  connected to it using the connection credential provided by mLab
![mongodbconnect](https://github.com/Oolabanji/test_/assets/136812420/f904417d-c989-4e29-92d7-d184e4db9108)
I copy the connection code and save it inside a .env file which should be created in the parent todo directory

DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'

I changed the username, password,database name and network address to the one specified by me when creating the database and collection.

I update the code in the index.js as I need to point mongoose to the database service I created using mLab
![indexjs2](https://github.com/Oolabanji/test_/assets/136812420/b5e7602b-516b-4832-a4b5-25db54173a76)
Then I run node index.js to test our connection.

![nodeindexjs](https://github.com/Oolabanji/test_/assets/136812420/4c4f1dce-5cfc-4564-8603-a42c1caa9a12)

### Testing Backend Code Using Postman
So far, I have built the backend of the application and in order to test to see if it works without a frontend, I used postman to test the endpoints.

On Postman, I make a POST request to the database whilst specifying an action in the body of the request.


![postman](https://github.com/Oolabanji/test_/assets/136812420/4326876b-999f-4845-b333-7aa8b0c6929f)


![postman1](https://github.com/Oolabanji/test_/assets/136812420/d1ad3d38-4ccf-4888-8c61-16b90a05128e)

![mongos](https://github.com/Oolabanji/test_/assets/136812420/69c2456f-40fe-4b81-a0f9-3b6b4269e834)


![get](https://github.com/Oolabanji/test_/assets/136812420/7b530ce1-3c8e-4d1e-b876-2710843ef439)

### Creating Frontend
Since am done with the functionality I want from the backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, I use the create-react-app command to scaffold the app.

In the same root directory as the backend code, which is the Todo directory, run:
' npx create-react-app client'


![frontend](https://github.com/Oolabanji/test_/assets/136812420/e8f960d1-bc62-42dd-bbc1-36433f87ecb3)

I then installed concurrently and nodemon which are important packages used for the build up process. Concurrently ensures that multiple commands can be run at the same time on the same terminal.

'npm install concurrently --save-dev'
'npm install nodemon --save-dev'

![concurrently](https://github.com/Oolabanji/test_/assets/136812420/29820c9d-9ec2-4212-bb34-6243a747f47d)
I configured package.json to run the new installation

![packagejson](https://github.com/Oolabanji/test_/assets/136812420/95aa3bb5-3b25-49c1-ab56-ee15baa45111)
Configured proxy in package.json to ensure i access our site via using http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos.
![setproxy](https://github.com/Oolabanji/test_/assets/136812420/3d371edd-bc29-4baf-a0cd-59f90cdff92b)
Now i run npm run dev inside the todo directory and the server is spinned on port localhost:3000. Then we set inbound security group rule for port 3000.


![npmrundev](https://github.com/Oolabanji/test_/assets/136812420/4bf2b5b4-d207-4002-acf7-211364927f8c)
![port3000](https://github.com/Oolabanji/test_/assets/136812420/f6469746-a866-4b52-98f2-91bc4bc0be09)
I move into the client then src directory and then create a components directory which will contain files that contains our frontend code. Inside the components directory I create Input.js ListTodo.js Todo.js.

Insert the below code into the Input.js
![inputjs](https://github.com/Oolabanji/test_/assets/136812420/08a29efc-4b4a-47f4-822e-aaf2b758354b)

I ensured axios is installed in the client directory
'npm install axios'

In the ListTodo.js directory, i insert the following code

![ListTodojs](https://github.com/Oolabanji/test_/assets/136812420/00c423a5-093e-462f-8b19-5c43699a227e)
In the Todo.js directory, I insert the following code


![Todojs1](https://github.com/Oolabanji/test_/assets/136812420/3244a7cd-5bdc-4def-93da-8a86638e777b)
In the src folder , I created an App.js file which encapsulates all other components. Paste the below code
![Appjs](https://github.com/Oolabanji/test_/assets/136812420/c3d0b805-25c7-4217-86a9-0ddf91c46ee3)
In same directory, create an App.css and insert the following code

![Appcss](https://github.com/Oolabanji/test_/assets/136812420/836e04be-815c-4f38-9d47-de6b72e48217)

I created an index.css file and insert the below code

![indexcss](https://github.com/Oolabanji/test_/assets/136812420/acb38017-3554-421c-9233-7330048c2a99)
Lastly to the root directory todo and I run npm run dev. This builds up the application and spins it up.

![MyTodo](https://github.com/Oolabanji/test_/assets/136812420/301a44d4-2810-4b28-8ff3-d2e743f1468d)


