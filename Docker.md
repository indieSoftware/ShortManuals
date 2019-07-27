# Docker

## Install

Install from the Website:
[https://www.docker.com/get-started](https://www.docker.com/get-started)

Start the app icon and wait until the docker icon steadies out in the tool bar. 

Use a terminal window and get the current version of docker to see if it is correctly installed and running:

	docker version

Log in via toolbar icon if needed. If this doesn't work open terminal and run:

	docker login

Use the docker ID (not email) and the password to login via command line.

## Basic commands

Create and start the container from its image `hello-world` (and download it prior from Docker Hub if needed):

	docker run hello-world

The run command includes create and start. To create a container simply use:

	docker create hello-world

This writes a container ID which can be used to start the container:

	docker start -a CONTAINER_ID
	
Replace `CONTAINER_ID` with the ID printed by the create statement. The `-a` attaches the container's output to the console so the output is written to the console. Without the `-a` command use the logs command to get the log from a container:

	docker logs CONTAINER_ID

Show runnimg containers:

	docker ps

To stop a container (sending a SIGTERM, soft end):

	docker stop CONTAINER_ID

To kill the container and its process (sending a SIGKILL, hard end, gets automatically called 10 seconds after a stop command):

	docker kill CONTAINER_ID

Show all containers which have been run:

	docker ps --all

To clean up and delete the container processes and images from the device run the following with confirming by `y`:

	docker system prune

## Connecting

To execute additional commands into a container, e.g. to start the command `redis-cli` in a `redis` container:

	docker run redis
	docker ps
	docker exec -it CONTAINER_ID redis-cli

The command `-it` connects via `-i` the terminal input to the container's STDIN input port and via `-t` any auto-completion and formatting is supported.

Connect to a container with a command processor:

	docker exec -it CONTAINER_ID sh

Now it's possible to execute the command directly:

	redis-cli

To terminate the redis-cli use `ctrl + c`. To terminate the shell use `ctrl + d` or enter `exit`.

The shell command is also usable with run directly:

	docker run -it busybox sh

## Creating images

Create a folder, create a file named `Dockerfile` without any extension in the folder and open it with a text or coding editor.

An example Dockerfile:

	# Use an existing docker image as a base
	FROM alpine

	# Download and install a dependency
	RUN apk add --update redis

	# Tell the image what to do when it starts as a container
	CMD ["redis-server"]

Run in the folder the build command to create the image:

	docker build .

After successfully creating the image the image ID is provided which the can be used for a run command.

	docker run IMAGE_ID

To build the image with a name tag use

	docker build -t DOCKER_ID/IMAGE_NAME:latest .
	
where `DOCKER_ID` is your docker ID, `IMAGE_NAME` is the image's name to define and `latest` indicating the version of this image which is actually the tag. The `:latest` tag is the default so it's not necessary to add it to the command.

After that it's possible to run the image (using implicit the latest version):

	docker run DOCKER_ID/IMAGE_NAME

To create a new image out of a running container:

	docker commit -c 'COMMAND' CONTAINER_ID

The `CONTAINER_ID` is the ID of the currently running container. The `COMMAND` is the start command to save with the container into the image, e.g. `CMD["redis-server"]` from the Dockerfile.

`alpine` is a compact and small base image. To find different base images look at [https://hub.docker.com](https://hub.docker.com).

To make all local files in the current folder on the machine available to the image simply copy the files with the `Dockerfile` command:

	COPY ./ ./

For defining a working directory in the image in which all following commands are executed, e.g. from `/usr/app` add to the `Dockerfile`:

	WORKDIR /usr/app

To run a container with mapping requests to port 5000 on the machine to 8080 of the container use:

	docker run -p 5000:8080 IMAGE_ID_OR_NAME

## Docker-compose

An example compose file to put into `docker-compose.yml`:

	version: '3'
	services:
	  redis-server:
	    image: 'redis'
	  node-app:
	    build: .
	    ports:
	      - "4001:8081"

This creates two containers `redis-server` and `node-app` where the latter is build with the Dockerfile in the current folder. The port forwarding is for reaching the container from the local machine. Between containers the connection happens via the service name, e.g. `redis-server` in the node-app's `index.js`:

	const client = redis.createClient({
	  host: 'redis-server',
	  port: 6379
	});

So 6379 is the default port for redis over which the images communicate automatically via docker. When connecting via the local machine the port 4001 is used and gets mapped to 8081 for the node-app. So to connect to the node-app call in the browser (after starting up the composition):

	localhost:4001

Run and connect all containers via `docker-compose`:

	docker-compose up

To prior build the images add the flag `--build`.

For running the composition in the background use the `-d` flag:

	docker-compose up -d

For shutting all components again from the background use:

	docker-compose down

To specify a container's restart policy add to `docker-compose.yml`:

	  node-app:
		restart: always

There are different restart policies available:

 - "no": Never attempt to restart the container (quotes necessary).
 - always: Always restart the container when it stops for any reason. Usecase: Webserver with always availability.
 - on-failure: Restart only if the container stops with an error code. Usecase: Worker processes an ending task.
 - unless-stopped: Always restart unless the deverloper forcibly stop it.

To find running containers for a `docker-compose.yml`:

	docker-compose ps

## Volume

Use a developer docker file `Dockerfile.dev` (e.g. for the node.js demo project `create-react-app`):

	FROM node:alpine
	WORKDIR '/app'
	COPY package.json
	RUN npm install
	COPY . .
	CMD ["npm", "run", "start"]
	
To use the custom Dockerfile:

	docker build -f Dockerfile.dev .

To use references of the pwd (present working directory) instead of copying all files into an image:

	docker run -p 3000:3000 -v /app/node_modules -v $(pwd):/app IMAGE_ID

This maps the ports, excludes the node_modules from beeing mapped to the local machine, but the rest of the folder. Now every edit of the sources is immediately available to the container.

To simplify the command use a `docker-compose.yml` file:

	version: '3'
	services:
	  web:
	    build:
	      context: .
	      dockerfile: Dockerfile.dev
	    ports:
	      - "3000:3000"
	    volumes:
	      - /app/node_modules
	      - .:/app

Run the single container composition:

	docker-compose up

## Production

Multi-step `Dockerfile` for the production build:

	FROM node:alpine as builder
	WORKDIR '/app'
	COPY package.json .
	RUN npm install
	COPY . .
	RUN npm run build

	FROM nginx
	COPY --from=builder /app/build /usr/share/nginx/html

Create image & run it:

	docker build .
	docker run -p 8080:80 IMAGE_ID

