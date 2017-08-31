# Docker meteor base app

This project contains a basic meteor application. The main goal of this project is just to Dockerize a basic todo app made with the React framework and create a deployment version of this app.

## Requirements in your host machine

You will need to have installed the following technologies:

 * Docker : https://docs.docker.com/engine/installation/
 * Docker Compose: https://docs.docker.com/compose/
 * Git : https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
 * Meteor : https://www.meteor.com/
 * Node.js: https://nodejs.org/en/
 * MongoDB: https://www.mongodb.com/

## Interesting links

Docker:

* Getting started with Docker: https://docs.docker.com/get-started/
* Getting started with Docker Compose:
* Cool tutorials about Docker by **Romin irani**: https://rominirani.com/docker-tutorial-series-a7e6ff90a023
* My Docker Cheat Sheet by **Aymen El amri** :https://medium.com/statuscode/dockercheatsheet-9730ce03630d

Meteor deployment:

* Nice tutorial about meteor deployment and Docker by **Project Ricochet**:https://projectricochet.com/blog/production-meteor-and-node-using-docker-part-i

Using Docker Compose:

* Getting started with Docker Compose: https://docs.docker.com/compose/gettingstarted/
* Docker Compose sintax: https://docs.docker.com/compose/compose-file/
* Meteor app docker compose by *juozapas*: https://github.com/domasx2/meteor-docker

# Starting with the Docker meteor deployment

Once the app is ready and we are happy the next step is the deployment. If you don't have the meteor app ready just follow this [guide](https://github.com/carlosym/docker_meteor_base_app/tree/master/app)

Important things to follow:

Is important to follow this file system structure, don't change the names of the folder if you don't want to have problems building the image:

* app (Meteor application)
* bin
* build
* config (Configuration files)
* --> development
* --> production

Let's go!

## Docker : Building the image

`chmod 777 docker_meteor_build.sh`

`./docker_meteor_build.sh`

## Docker : Running container in host machine (First approach) (Mongo in host machine) (Docker web app)

As you will know **Meteor** is a nice framework for creating real time applications with Javascript. In most of the cases **Meteor** use databases to store the data of  the app. Meteor mainly use MongoDB to store all the metadata.  In this first approach we will deploy our app in a Docker container and we will use a MongoDB database located in our host machine in a native way without using Docker to package the MongoDB.

### Running mongodB in the host machine

If you don't have mongo installed please [install it](https://docs.mongodb.com/manual/installation/) before. Let's go! if everything is ok.

Starting the **mongoDB** server

`mongod`

If everything went ok, you should see in the terminal something like *MongodB starting: pid ...*

If you want to stop the docker server in the host machine type this command:


### Runnig the web app container 

`chmod 777 docker_meteor_run.sh`

`./docker_meteor_run.sh`

**What is inside the script?**

`docker run -d --rm  -e ROOT_URL=http://localhost -e MONGO_URL=mongodb://10.80.29.27:27017/meteorbaseapp -p 8080:80  --name meteor-base-app carlosym1/docker_meteor_base_app`

Parameters:

* ( -it ) --> Run container in interactive mode
* ( --rm ) --> Remove volume and container after being executed
* ( -e ROOT_URL=http://localhost ) --> environment variable with app url
* ( -e MONGO_URL=mongodb://10.80.130.86:27017/meteor ) --> environment variable with mongodb url (!!! Check the ip of your host machine)
* ( -p ) exposing the container with the port 8080
* ( --name meteor-base-app ) name of the container

## Docker : Running container in host machine (Second approach) (Docker Mongo) (Docker web app)

In this second approach we will deploy our app in a Docker container and we will use a MongoDB database as a docker container. Then we will have two Docker containers the first for the database and the second one for the web app.

### Running the database container

Pulling the [mongo](https://hub.docker.com/_/mongo/) image from docker Hub:

`docker pull mongo`

Run mongo image:

`docker run --name some-mongo -v /Users/carlos/Documents/DevOps/mongo_db/data/db:/data/db -p 27017:27017 -d mongo`

### Runnig the web app container 

`chmod 777 docker_meteor_run.sh`

`./docker_meteor_run.sh`

**What is inside the script?**

`docker run -d --rm  -e ROOT_URL=http://localhost -e MONGO_URL=mongodb://10.80.29.27:27017/meteorbaseapp -p 8080:80  --name meteor-base-app carlosym1/docker_meteor_base_app`

Parameters:

* ( -it ) --> Run container in interactive mode
* ( --rm ) --> Remove volume and container after being executed
* ( -e ROOT_URL=http://localhost ) --> environment variable with app url
* ( -e MONGO_URL=mongodb://10.80.130.86:27017/meteor ) --> environment variable with mongodb url (!!! Check the ip of your host machine)
* ( -p ) exposing the container with the port 8080
* ( --name meteor-base-app ) name of the container

## Docker : Running container in host machine (Third approach) (Docker Compose)

The first and second were ok, we were able to launch mongo as a server installed in our host machine or as a server in a docker container, then we were able to launch our todo web app. This is something really nice but It can be improved an automatized using Docker Compose.

You can check the docker compose file [here](https://github.com/carlosym/docker_meteor_base_app/blob/master/docker-compose.yml), remember that you should change the ip, volumes and ports if is need it.

if you don't want to build the images of the web app you can comment the line 4 `build: .` with #. 

`chmod 777 docker_compose_run.sh`

`./docker_compose_run.sh`

or

`docker-compose up`








