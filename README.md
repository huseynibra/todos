# Docker starter with Node JS, React and MongoDB. All in a single repo

## Overview

In this repo we define three **Docker**  container(services) **frontend**,  **backend** and **DB**. Our **backend** is on 
**NodeJS**, frontend is on **ReactJS** and we used **MongoDB** as our **DB**. These all containers will run on your local machine/computer. 
You can see your code changes on a local machine to the running containers of your docker images. In addition to this, not only your code changes reflect to your container, your every **CRUD** operation to **DB** will also saved inside your local machine.
 
It's sound good  :ok_hand: , so let's start :runner:

## Prerequisite

For this demo to work, [Docker](http://docker.com) and [git](https://git-scm.com/) needs to be installed on your local(host) machine.

## GuideLine Steps

#### 1. Clone the repo
We used two DockerFiles one for frontend and other for backend. 
First we will get through the `Dockerfile` of frontend and backend then `docker-compose file`.

#### 2. Snippet of backend(Node.js)`DockerFile`

You will find this `DockerFile` file in the root directory of the project.

```bash
FROM node:10

#Argument that is passed from docer-compose.yaml file
ARG NODE_PORT

#Echo the argument to check passed argument loaded here correctly
RUN echo "Argument port is : $NODE_PORT"

# Create app directory
WORKDIR /usr/src/app

#COPY . .
COPY . .

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
RUN npm install


#In my case my app binds to port NODE_PORT so you'll use the EXPOSE instruction to have it mapped by the docker daemon:

EXPOSE ${NODE_PORT}

CMD npm run dev
```

##### 2.1 Explanation of backend(Node.js) `DockerFile`

- The first line tells Docker to use another Node image from the [DockerHub](https://hub.docker.com/). We’re using the official Docker image for Node.js and it’s version 10 image.

- On second line we declare argument `NODE_PORT` which we will pass it from `docker-compose`.

- On third line we log to check argument is successfully read 

- On fourth line we sets a working directory from where the app code will live inside the Docker container.

- On fifth line, we are copying/bundling our code working directory into container working directory on line three.

- On line seven, we run npm install for dependencies in container on line four.

- On Line eight, we setup the port, that Docker will expose when the container is running. In our case it is the port which we define inside `.env` file, read it from `docker-compose` then passed as a argument to the (backend)`DockerFile`.

- And in last, we tell docker to execute our app inside the container by using node to run `npm run dev. It is the command which I registered in __package.json__ in script section.

###### :clipboard: `Note: For development purpose I used __nodemon__ , If you need to deploy at production you should change CMD from __npm run dev__ to __npm start__.`

#### 3. Snippet of frontend(ReactJS)`DockerFile`

You will find this `DockerFile` inside **frontend** directory. 

```bash
# Create image based on the official Node image from dockerhub
FROM node:10

#Argument that is passed from docer-compose.yaml file
ARG FRONT_END_PORT

# Create app directory
WORKDIR /usr/src/app

#Echo the argument to check passed argument loaded here correctly
RUN echo "Argument port is : $FRONT_END_PORT"

# Copy dependency definitions
COPY package.json /usr/src/app

# Install dependecies
RUN npm install

# Get all the code needed to run the app
COPY . /usr/src/app

# Expose the port the app runs in
EXPOSE ${FRONT_END_PORT}

# Serve the app
CMD ["npm", "start"]

