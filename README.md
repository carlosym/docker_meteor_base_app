# Docker meteor base app

This project contains a basic meteor application. The main goal of this project is just to Dockerize a basic todo app made with the React framework and create a deployment version of this app.

## Requirements in your host machine

You will need to have installed the following technologies:

 * Docker : https://docs.docker.com/engine/installation/
 * Git : https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
 * Meteor : https://www.meteor.com/
 * Node.js: https://nodejs.org/en/
 * MongoDB: https://www.mongodb.com/

## Interesting links

Docker:

* Getting started with Docker: https://docs.docker.com/get-started/
* Cool tutorials about Docker by **Romin irani**: https://rominirani.com/docker-tutorial-series-a7e6ff90a023
* My Docker Cheat Sheet by **Aymen El amri** :https://medium.com/statuscode/dockercheatsheet-9730ce03630d

Meteor deployment:

* Nice tutorial about meteor deployment and Docker by **Project Ricochet**:https://projectricochet.com/blog/production-meteor-and-node-using-docker-part-i

# Starting with the Docker meteor deployment

Once the app is ready and we are happy the next step is the deployment. If you don't have the meteor app ready just follow this [guide](link to the guide)

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

`chmod 777 build.sh`

`./build.sh`

## Docker : Running container in host machine (First approach)

As you will know **Meteor** is a nice framework for creating real time applications with Javascript. In most of the cases **Meteor** use databases to store the data of  the app. Meteor mainly use MongoDB to store all the metadata.  In this first approach we will deploy our app in a Docker container and we will use a MongoDB database located in our host machine in a native way without using Docker to package the MongoDB.

### Running mongodB in the host machine

If you don't have mongo installed please [install it](https://docs.mongodb.com/manual/installation/) before. Let's go! if everything is ok.

Starting the **mongoDB** server

`mongod`

If everything went ok, you should see in the terminal something like *MongodB starting: pid ...*

### Runnig the web app container 

`./run.sh`

**What is inside the script?**

`docker run -d --rm  -e ROOT_URL=http://localhost -e MONGO_URL=mongodb://10.80.29.27:27017/meteorbaseapp -p 8080:80  --name meteor-base-app carlosym1/docker_meteor_base_app`

Parameters:

* ( -it ) --> Run container in interactive mode
* ( --rm ) --> Remove volume and container after being executed
* ( -e ROOT_URL=http://localhost ) --> environment variable with app url
* ( -e MONGO_URL=mongodb://10.80.130.86:27017/meteor ) --> environment variable with mongodb url (!!! Check the ip of your host machine)
* ( -p ) exposing the container with the port 8080
* ( --name meteor-base-app ) name of the container


