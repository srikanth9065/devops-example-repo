# Project Dockerize

Finally we are moving from a monolith application development model to systems built upon microservices.

This means that we need a new development environment and build system where the teams working in the same main project, but with different services can separate their code and run it on different servers!

For this purpose we have chosen Docker in conjunction with our own setup for automating Docker.

This Readme will help you get up and running. Your DevOps-team, that will support you along the way, consists of:

* Mohamed (Msharshar21)
* Nazir (Naaziir)
* Oskar (Knuttas)
* Srikanth (srikanth9065)
* Mia (Entitet)

## Branching and Docker

This Github-repo is setup to serve you with the containers you need. The **main branch** is used to branch out from when we create a base branch for a Microservice, but other than that you don't need to mind it. The **docker branch** will serve you with the tools you need to run your development environment in containers.

Then we are in familiar territory. Your Microservice has a main branch. Regard this as a traditional main branch for a project and don't merge further than this. From this branch create your dev and feature branches as usual. 

The nameing convention used is: 

*name-of-microservice-main*

*name-of-microservice-dev* 

*name-of-microservice-name-of-feature*


## Getting started
Checkout the **docker branch** and run the following command in your terminal:

```
./create-docker-tools.sh
```

This will give you two shell scripts (that are git-ignored and thus available in all branches):

```
# start all Docker containers
./start
```

```
# stop all docker containers
./stop
```

(You will also see a git ignored folder called docker-tools. There is *no need* for you to work in this folder.)

The first time you run ```./start``` it will fail. Scroll through the output in your teminal and you'll find an SSH-key and instructions how to proceed. The script will continue failing until you've added the generated SSH-key to your Github account.

If there has been updates pertaining to the docker tools you need to run ```./create-docker-tools.sh``` again.

## Do I need to know how to use Docker?

We recommend you use the tool Docker desktop for easy access to the containers without the need to run specific commands in your terminal. The only specific command we feel you need to know at this stage is: 
```docker system prune --all```. Running this gives you a clean slate when it comes to everything Docker has been cacheing and is a great first step in troubleshooting.

But this is about it, the scripts mentioned above will handle everything for you.


## The setup needed in a branch

In a branch we need to specify what kind of a container we want to be created and for this we need a Dockerfile. The ```./start``` script needs to know which containers you want to be started and on which port the should run and for this a dockerSettings.json file is needed.

### How to create a Dockerfile
Name the file **Dockerfile**. In this you specify:
* a base image (FROM) 
* and a command to run (CMD) when the server starts.

Example:

```
# start with a debian node image
FROM node:16.15-buster

# run necessary start commands
CMD npm install && node index
```

**Important:** The above example is very simple as much of what a Dockerfile needs to handle is already handled through the shellscripts. Therefore do not specify a WORK DIR, although most guides about how to write a Dockerfile ask you to. It will be set by the shellscript to where the code for your branch is checked out within Dockers container/named volume systems automatically.

### How to create a dockerSettings.json file
The dockerSettings.json should contain info about which branches you want to create containers from (your own one and other branches with services you want to communicate with) and on which port they should be running. It's important not to run on the same port as another of our microservices. Here is an example:

```json
[
  "exact-branchname-of-your-microservice",
  [
    4000
  ],
  "exact-branchname-of-your-database",
  [
    4001
  ],

  "exact-branchname-of-another-microservice",
  [
    4002
  ]
]
```

### Important! Listen to the environment variable PORT when you start your service!

The system sends an environment variable called PORT to your container (each branch runs in a container that you setup by writing a Dockerfile in your branch).

Start your service on this port!

### I don't know how to start my service on a specific port

Since you are in control of your microservice and its technology stack it is up to you investigate how to start in on a particular port, but here are some suggestion for technologies we know are going to be used in this project

#### React using the Vite development server

In your **config.vite.js** file:

```js
export default defineConfig({
  plugins: [react()],
  server: {
    // use process.env.PORT
    // to read the environment variable
    port: process.env.PORT
  }
})
```

#### Node.js/Express

```js
// Where you start your Express server
app.listen(process.env.PORT)
```

#### For database containers etc
Create a separate branch with your Dockerfile (and backup like SQL-dumps etc).

Refer to the documentation about the container you are using (MySQL, MariaDB, MongoDB etc) for how to start the db server on a particular port!

**Important!** If the server/service needs a command line argument rather than an environment variable to set the port it is starting on -  refer to the Docker documentation on how to read environment variables in your Dockerfile and pass them along as command line arguments in your start CMD!