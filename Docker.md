# Docker
[Docker](https://www.docker.com/) is a platform for building, running and shipping applications  
I'm learning from these tutorials on YouTube:  
- [Docker tutorial for Beginners](https://www.youtube.com/watch?v=pTFZFxd4hOI) by Programming with Mosh 
- [Learn Docker in 1 Hour](https://www.youtube.com/watch?v=GFgJkfScVNU) by JavaScript Mastery 

If your code works on one machine, it can work on any other machine with Docker  
You don't have to say *"It works on my machine"* anymore  
With Docker, you can "pack" your application with everything it needs and run it on any machine  
For example, you are developing a website with Node 14 and MongoDB 4, you can make a package and run this package on any machine with Docker  
Docker will automatically install and run all the dependencies in isolated environment called *Container*  
This is also helpful if you are running multiple applications with different versions on software on one machine, now you can run them side by side   
Another benefit is, when you are finished using/developing one application, you can remove it with all it's dependencies in one go  

Docker helps consistently build, run and ship applications  

## Virtual Machines vs. Containers  
You might think that *container* is similar to a *Virtual Machine (VM)*  

The main difference is that VM needs a full operating system to run  
It makes VM slower to load and more intensive on resources  

Container uses the operating system of host  
This makes Containers more lightweight  

# Installation  
`yay -S docker` or go to [their website](https://docs.docker.com/get-docker/) and download it from there  

Check if everything is correct with `docker version` command  

You might get some errors, run these commands  
```shell
sudo systemctl enable docker  
sudo systemctl start docker  
sudo usermod -aG docker ${USER}  
```  
> Note: You might need to log out and log back in to apply changes  

Now, after running `docker version` you want to see something like this (both client and server running)  
```  
Client:
 Version:           27.1.1
 API version:       1.46
 Go version:        go1.22.5
 Git commit:        63125853e3
 Built:             Thu Jul 25 17:06:22 2024
 OS/Arch:           linux/amd64
 Context:           default

Server:
 Engine:
  Version:          27.1.1
  API version:      1.46 (minimum version 1.24)
  Go version:       go1.22.5
  Git commit:       cc13f95251
  Built:            Thu Jul 25 17:06:22 2024
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.7.20
  GitCommit:        8fc6bcff51318944179630522a095cc9dbf9f353.m
 runc:
  Version:          1.1.13
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

# Development Workflow
Imagine you have an application  
Now you need to *"dockerize"* it - add **Dockerfile** to it  

Dockerfile is a plain text file which provides information to Docker  
Now, Docker can make an **Image** from the app  

Image is like a lightweight, standalone, executable package  
Image contains everything our application need to run:  
- Cut-down OS  
- Runtime environment (Node for example)  
- Application files  
- Third-party libraries  
- Environment variables  

Once we have an Image, we tell Docker to run it inside **Container**  
Container is an environment for Image to run  
Container is a special type of process  
It has it's own file system, which is provided by the Image  

Once you have an Image, you can push it to **Docker Registry** - [Docker Hub](https://hub.docker.com/) (like GitHub to Git) so everyone else can use it  
Docker Hub is a registry for Docker images  

## Dockerfile
Once you have starting developing your application, you need to add `Dockerfile`   
Just add a new file called `Dockerfile`, the name has to be the same as you see there, no extension  
Now we will be typing code into the `Dockerfile`

First, you need to specify what container you are starting on, like a base container, like what container will you inherit from  
You can do this with `FROM` command  
```dockerfile
FROM linux
```

Now if you have JavaScript application, you are most likely using Node  
In this case, you don't need to start from `linux`, you can start from `node`, which is already based on `linux`  
```dockerfile
FROM node
```

You can specify what Linux distribution you want to use  
For example Alpine Linux is very small *(5MB !)*, so it might be a good consideration for Docker container (small image size, even faster load times)  
```dockerfile
FROM node:alpine
```

If you want to explore more containers, go to [hub.docker.com](https://hub.docker.com/)

Now we need to copy our application files with the `COPY` command  
With following command, we will copy all the files from our program into Image's `/app` directory  
The Image has a file system, and we will create a directory called `app` inside this filesytem
```dockerfile
FROM node:alpine
COPY . /app
```

Finally, you can use `CMD` function to run commands on Image  
```dockerfile
FROM node:alpine
COPY . /app
CMD node /app/index.js
```

If you don't want to use the absolute path for files, you can set work directory with `WORKDIR` command  
It's basically like `cd` on Linux  
```dockerfile
FROM node:alpine
COPY . /app
WORKDIR /app
CMD node index.js
```

Another example of `Dockerfile`  
```Dockerfile
FROM ubuntu:latest
RUN apt update && apt install -y coffee-maker
COPY . /coffee-recipe
RUN ./prepare-beans.sh
CMD ["./brew-coffee.sh"]
```

Another example of `Dockerfile`  
This one can be used to run React app  
```Dockerfile
FROM node:20-alpine

# make group and user which will only be able to run app. it's for security reasons  
RUN addgroup app && adduser -S -G app app  

# switch to user "app"
USER app

# change current working directory
WORKDIR /app

# copy package.json and package-lock.json
COPY package*.json ./

# change permissions of app folder
USER root
RUN chown -r app:app .
USER app

# install dependencies
RUN npm install

# copy all other files
COPY . .

# expose port 5173. this container will listen on this specific port  
EXPOSE 5173

# run the app
CMD npm run dev
```

## `.dockerignore`
This is very similar to `.gitignore` file  
It basically tells Docker, which files not to upload  

```dockerignore
node_modules/
```

## Building Docker Image  
Now, once you are done, you can go to terminal to Build Docker Image  
You can do this with `docker build` command, which looks like this:   
```shell
docker build -t <tag> <dockerfile_location>  

# for example
docker build -t hello-docker .
```

Tag is like a name for the Docker Image, which will identify it. It is set with `-t` flag  
At the end, you need to add location to the `Dockerfile`. Since we are in the same directory as the `Dockerfile`, you can just add `.`  

You might think that you will see a new file/files after building Docker Image, but it's not the case  
You project will look exactly the same as before  
How Docker stores files is complex and not needed to understand right now  

To see all Docker Images on your computer, type one of following command in your terminal  
```shell
docker images
docker image ls
```  

You can see `Repository`, `Tag`, `Image ID`, `Created` and `Size` there  

## Running Docker Image
```shell
docker run <name>

# for example
docker run hello-docker
```  

After entering this command, your application should run  

Now imagine we have for example a React app and we want to view it  
We specify that we want it to run on port `5173` and Node gave us `https://localhost:5173` as the address of the site  
But if we entered this address to our browser, it will not show anything  
Why? Because it the address of Docker Container, not our machine  
How to deal with this? We need to map the port of Docker Container to our machine when running the container   
It's as simple as this  
```shell
docker run -p <container>:<our-machine> <name>

# for example
docker run -p 5173:5173 react-app
```

> Note: If you try to run multiple containers on the same port, you will get an error. Check [the next chapter](#Stopping%20Docker%20Image) to find solution

If you have `Vite` app, you need to modify `package.json` file in order to make it work  
Just replace `..."scripts": {"dev": "vite"...` with `..."scripts": {"dev": "vite --host"...`

Now imagine you are developing your app and you make some changes to it  
You might see that nothing changes inside our container  
You can adjust the run command like this  
```shell
docker run -p 5173:5173 -v "$(pwd):/app" -v /app/node-modules react-app
```  
The `-v` flag (volume) will tell Docker that you want to mount your current directory (`pwd` command) to `/app` directory in Container  
This basically means that our local folder is linked to `/app` folder in Container, and any changes will be immediately reflected to Container  

Now the command gets pretty long, doesn't it?  
We can use Docker Compose to deal with this

### Docker Compose
First of all, you need to install Docker Compose  
`yay -S docker-compose` or visit [their website](https://docs.docker.com/compose/install/) and follow the steps there   

To use Docker Compose, just add `compose.yaml` file next to `Dockerfle`  
```yaml
services:
	web:
		build:
			context: .
		ports:
		 - 5173:5173
		volumes:
		 - .:/app
		 - /app/node_modules
```

Now instead of running the previous long `docker run ...` command, run `docker compose up`  
> Note: You might get `Permissioin Denied` error. Simply run `sudo docker compose up` instead  

#### Docker Compose example
```yaml
services:
  backend:
    build:
      context: ./backend/
    ports:
      - "5000:5000"
    volumes:
      - ./backend:/app
    environment:
      FLASK_ENV: development
  frontend:
    build:
      context: ./frontend/
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app
      - /app/node_modules
    depends_on:
      - backend
    command: npm run dev -- --host
```

```bash
docker-compose up --build
docker-compose down --rmi all --volumes --remove-orphans 
```



But this still doesn't fully solve the problem  
Whenever we change `package.json` file, we need to rebuild it  
Here, we can use Docker Compose Watch

### Docker Compose Watch
Docker Compose Watch can do these things
- Sync
- Rebuild
- Sync-Restart

Now I have no idea what was happening in the tutorial, so I will just write the whole `compose.yaml` file  
```yaml
# specify the version of docker-compose
version: "3.8"

# define the services/containers to be run
services:
  # define the frontend service
  # we can use any name for the service. A standard naming convention is to use "web" for the frontend
  web:
    # we use depends_on to specify that service depends on another service
    # in this case, we specify that the web depends on the api service
    # this means that the api service will be started before the web service
    depends_on: 
      - api
    # specify the build context for the web service
    # this is the directory where the Dockerfile for the web service is located
    build: ./frontend
    # specify the ports to expose for the web service
    # the first number is the port on the host machine
    # the second number is the port inside the container
    ports:
      - 5173:5173
    # specify the environment variables for the web service
    # these environment variables will be available inside the container
    environment:
      VITE_API_URL: http://localhost:8000

    # this is for docker compose watch mode
    # anything mentioned under develop will be watched for changes by docker compose watch and it will perform the action mentioned
    develop:
      # we specify the files to watch for changes
      watch:
        # it'll watch for changes in package.json and package-lock.json and rebuild the container if there are any changes
        - path: ./frontend/package.json
          action: rebuild
        - path: ./frontend/package-lock.json
          action: rebuild
        # it'll watch for changes in the frontend directory and sync the changes with the container real time
        - path: ./frontend
          target: /app
          action: sync

  # define the api service/container
  api: 
    # api service depends on the db service so the db service will be started before the api service
    depends_on: 
      - db

    # specify the build context for the api service
    build: ./backend
    
    # specify the ports to expose for the api service
    # the first number is the port on the host machine
    # the second number is the port inside the container
    ports: 
      - 8000:8000

    # specify environment variables for the api service
    # for demo purposes, we're using a local mongodb instance
    environment: 
      DB_URL: mongodb://db/anime
    
    # establish docker compose watch mode for the api service
    develop:
      # specify the files to watch for changes
      watch:
        # it'll watch for changes in package.json and package-lock.json and rebuild the container and image if there are any changes
        - path: ./backend/package.json
          action: rebuild
        - path: ./backend/package-lock.json
          action: rebuild
        
        # it'll watch for changes in the backend directory and sync the changes with the container real time
        - path: ./backend
          target: /app
          action: sync

  # define the db service
  db:
    # specify the image to use for the db service from docker hub. If we have a custom image, we can specify that in this format
    # In the above two services, we're using the build context to build the image for the service from the Dockerfile so we specify the image as "build: ./frontend" or "build: ./backend".
    # but for the db service, we're using the image from docker hub so we specify the image as "image: mongo:latest"
    # you can find the image name and tag for mongodb from docker hub here: https://hub.docker.com/_/mongo
    image: mongo:latest

    # specify the ports to expose for the db service
    # generally, we do this in api service using mongodb atlas. But for demo purposes, we're using a local mongodb instance
    # usually, mongodb runs on port 27017. So we're exposing the port 27017 on the host machine and mapping it to the port 27017 inside the container
    ports:
      - 27017:27017

    # specify the volumes to mount for the db service
    # we're mounting the volume named "anime" inside the container at /data/db directory
    # this is done so that the data inside the mongodb container is persisted even if the container is stopped
    volumes:
      - anime:/data/db

# define the volumes to be used by the services
volumes:
  anime:
```

Also, there is a [Link to GitHub for this project](https://github.com/adrianhajdin/docker-course/tree/main/mern-docker)  

To run this, you need to run 2 commands in 2 terminals  
```shell
docker compose up
```  

```shell
docker compose watch
```


## Stopping Docker Image
If you list all running Docker Images with `docker ps`, you might see a lot of Images, which might cause a problems sometimes  
To stop a Docker Image, it's as simple as this  
```shell
docker stop <name-or-id>

# for example
docker stop website 
docker stop c5a3b
```

If you have a lot of stopped containers, which you want to remove, you can run this command  
```shell
docker container prune
```  
> Note: This will remove all stopped containers  

You can also remove just specific container with this command  
```shell
docker rm <name-or-id>

# for example
docker rm website
docker rm aa7
```

If you try to remove running container, you will get an error  
You have to either stop the container first, or remove it by force  
```shell
docker rm aa7 --force
```

## Get Docker Image from Docker Hub
The command used to take an Image from Docker Hub to your local machine is `pull`  
You can pull any Image from there, either pre-made images like `ubuntu` or `node`, or your own Images
It's as simple as this  
```shell
docker pull <name>

# for example
docker pull ubuntu
docker pull cyprich/website
```  

You can verify and run the container with commands you have learned before   
```shell
docker images
docker run cyprich/website  
				```  

> Note: You can try it online with sites like [Play with Docker](https://labs.play-with-docker.com/), which allows you to make a virtual machine and run Docker image there  

## Publish Docker Image
It you want to put your own Docker Image to Docker Hub, you need to follow these steps  

First of all, you need to Login  
```shell
docker login
```
Just enter your username and password from Docker Hub and you should successfully Login  

Now run this commands
```shell
docker tag <local-image-name> <username>/<remote-image-name>
docker push <username>/<remote-image-name>

# example
docker tag website cyprich/website
docker push cyprich/website
```

Now if you went to [hub.docker.com/repositories](https://hub.docker.com/repositories/cyprich) and search for your username, you will see your container  
Keep in mind, that now anyone can use your container  

# Running Linux  
You can search for Ubuntu on [Docker Hub](https://hub.docker.com/)  
You can run it with following command  
```shell
docker run ubuntu
```

If you already have `ubuntu` image on your system, it will run, otherwise it will download it  

> Note: On the official site, there is `docker pull ubuntu` command, but this is like a shortcut  

By typing following command, you can list Docker processes on your system  
```shell
docker ps
```

You will (probably) not see anything right now, because the container that we ran stopped immediately  
To list all Docker processes, type the following command  
```shell
docker ps -a
```

Now you can see that you have "Exited" Ubuntu container  

To run container interactively, you can run this command  
```shell
docker run -it <name>

# in this case
docker run -it ubuntu
```

Now you are running Ubuntu  
You can enter commands and whatever  
