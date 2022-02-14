# Start Docker

Start Docker is created to help us to quickly work with docker. This project consists of:
- [Getting Started](#getting-started)
  - [Installing Docker](#installing-docker)
  - [Running Docker](#running-docker)
- [Working with Dockerfile](#working-with-dockerfile)
  - [Specifying the base image](#specifying-the-base-image)
  - [Available commands](#available-commands)
- [Creating an image from a newly created Dockerfile](#creating-an-image-from-a-newly-created-dockerfile)
- [Publishing the image to the Docker HUB](#publishing-the-image-to-the-docker-hub)

## Getting Started

### Installing Docker
  - Install [Docker](https://docs.docker.com/get-docker/) or install Docker by executing [this script](https://github.com/rhexa/apt-apps/blob/main/docker.sh) on Debian based Operating System
  - Check if Docker has been succesfully installed by running `docker -v` on the terminal
  - To run docker without sudo privilege, we need to add our user to a docker group. On debian based OS we can run.
```
sudo usermod -aG docker <username>
```
  - Log off the session and check if the user has been succesfully added to the docker group
```
 groups 
```

### Running Docker
- Create a new directory for Docker project
```
mkdir <docker-project-directory>
cd <docker-project-directory>
```
- Browse the desired docker image to run on [Docker Repository](https://hub.docker.com/). 
- Run the image by executing `docker run <repo/image-name>` command.

## Working with Dockerfile
Docker file is a docker config file to define and build a custom image.

### Specifying the base image
In this example we are using node JS image [Node JS image](https://hub.docker.com/_/node)
```
# FROM <image>:<tag-name>
FROM node:14.17-alpine
```
### Available commands
The available commands can be found on [Docker docs](https://docs.docker.com/engine/reference/builder/)

Frequently used commands:
- `RUN <command>` to run shell command
- `COPY <source> <destination>` copy files from host to the container
- `WORKDIR <path>` change the working directory
- `EXPOSE <port-number>` expose the port (only notification)
- `CMD ["<command>","<arguments>"]` starting point command

Example of the Dockerfile
```
# specify the node base image with your desired version node:<version>
FROM node:14.17-alpine

# replace this with your application's default port
# EXPOSE 8888

# copy the web files to docker
RUN mkdir -p /home/app
COPY ./app /home/app

WORKDIR /home/app
RUN npm install

EXPOSE 3000

CMD ["npm", "start"]
```

## Creating an image from a newly created Dockerfile
In this example the docker file is inside the current working directory.

Run this command to build the image
```
# docker build . --tag <repo>/<version>:<tag>
docker build . --tag myrepo/myapp:0.0.1
```
Check if the image succesfully build by running.
```
docker images
```

## Publishing the image to the Docker HUB
First we need to login to our account by creating the access token from [Docker Hub](https://hub.docker.com/settings/security) account. Once the token created, we can authenticate ourself by running.
```
# interactive mode
docker login

# uninteractive mode
docker login -u <username> -p <password>
```
Now we can push our image to docker with the latest tag hub by running.

```
# docker image push <repo>/<app>:<version>
docker image push rhexa/node:<tag>
```