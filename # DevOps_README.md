# DevOps_README
Instructions for DevOps Aspirants about DevOps Tools

It contains documentation about installation and popular commands for specific tools. 

# Docker Instructions 

## Installation on Mac
To Install Docker on Mac, follow the instructions at below link.

https://docs.docker.com/docker-for-mac/install/

## Installation on Windows
To install Docker on Windows, follow the intructions at below link.

https://docs.docker.com/docker-for-windows/install/

## Installation on Linux Distribution
To install Docker on Linux, follow the instructions at below link.

Centos : https://docs.docker.com/engine/install/centos/

Ubantu : https://docs.docker.com/engine/install/ubuntu/

Debian : https://docs.docker.com/engine/install/debian/

Fedora : https://docs.docker.com/engine/install/fedora/


## Docker Hub
To access Docker Hub, Please visit below url

https://hub.docker.com/

## Docker Commands

> 1. To check Docker Version :
 ```bash
  docker --version
  
 ```
 
> 2. To Start the Docker on Linux : 
```bash
  sudo service docker start
  
 ```
 
> 3. To Stop the Docker on Linux : 
```bash
  sudo service docker stop
  
 ```
 
> 4. To check the running containers :
```bash
  docker ps
  
 ```
 
> 5. To check all the containers (running and terminated) :
```bash
  docker ps -a
  
 ```
 
> 6. To check docker images : 
```bash
  docker images
  
 ```
 
> 7. To remove an docker image :
```bash
  docker rmi $image_id
  
 ```
 
> 8. To build a docker image from Dockerfile : 
```bash
  docker build .
  docker build -t $imageName:tagName .
  docker build -t my-test-app:1.0.0 .
  
 ```
 
 > 9. To Login to Docker Hub Repository : 
  ```bash
   docker login
   
 ```
 
 > 10. To Push an image to DockerHub Repository :
  ```bash
   docker push $dockerHubId/$imageId:$tagName
   docker push uzairrafq/my-test-app:1.0.0
   
 ```
 
 > 11. To Pull an image from DockerHub Repository :
  ```bash
   docker pull $dockerHubId/$imageId:$tagName
   docker pull uzairrafq/my-test-app:1.0.0
   
 ```
 
 > 12. To Run an Docker image :
  ```bash
   docker run -p $entryPort:$PortToBeMapped -d $dockerId/$imageId:$tagName
   docker run -p 8888:8080 -d uzairrafq/my-test-app:1.0.0
   
 ```

## Dockerfile Keywords

> 1. FROM : It tells docker from which base image, you want to create your image from.
Usage : 
```bash
FROM <image>
FROM <image>:<tag>
```
> 2. RUN : This command is used to run the instructions against the image. 
Usage : 
```bash
RUN <command>
```
> 3. CMD : CMD is used to provide default arguments for the ENTRYPOINT instruction, both the CMD and ENTRYPOINT instructions should be specified with the JSON array format.
Note : Please note that there can only be one CMD in a Dockerfile.
Usage : 
```bash
CMD <command> <param1> <param2> (shell form)
```

> 4. MAINTAINER : It is used to specify the maintainer of the Dockerfile. In our case, we just specify the email id.
Usage : 
```bash
MAINTAINER <name>
```

> 5. ADD : Adds new files, directories, or remote file URLs from <src> and adds them to the filesystem of the image at the path <dest>.
Usage : 
 
```bash
ADD <src>  <dest>
```

> 6. ENTRYPOINT : Allows you to configure a container that will run as an executable.
Usage : 

```bash
ENTRYPOINT <command> <param1> <param2> (shell form)
```

> 7. ENV : The ENV instruction sets the environment variable <key> to the value <value>.
Usage : 
 
```bash
ENV <key> <value>
```

> 8. EXPOSE : It Informs Docker that the container listens on the specified network port(s) at runtime.
Usage : 

```bash
EXPOSE <port> [<port> ...]
```

> 9. USER : The USER instruction sets the user name or UID to use when running the image and for any RUN, CMD and ENTRYPOINT instructions that follow it in the Dockerfile.
Usage : 

```bash
USER <username | UID>
```

> 10. VOL : Creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers.
Usage : 

```bash
VOLUME <path>
```

> 11. COPY : Copies new files or directories from <src> and adds them to the filesystem of the image at the path <dest>.
Usage : 
 
```bash
COPY <src> [<src> ...] <dest>
```

> 12. WORKDIR : Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD instructions that follow it.
Usage : 

```bash
WORKDIR </path/to/workdir>
```

> 13. ARG : Defines a variable that users can pass at build-time to the builder with the docker build command.
Usage : 

```bash
ARG <name>[=<default value>]
```



## Dockerfile Example

```bash
FROM openjdk:8-jdk-alpine

VOLUME /tmp

EXPOSE 8888

ARG JAR_FILE=/target/*.jar

COPY ${JAR_FILE} app.jar

RUN echo "Creation of your docker image is in progress, please hold on for a moment"

ENTRYPOINT ["java", "-jar", "app.jar"]

MAINTAINER "uzairrafq@gmail.com"
```

# Docker Compose file : 
```
version: '3'
services:
  docker-mysql:
    restart: always
    container_name: docker-mysql
    image: mysql
    environment:
      MYSQL_DATABASE: book_manager
      MYSQL_ROOT_PASSWORD: root
      MYSQL_ROOT_HOST: '%'
    volumes:
      - ./sql:/docker-entrypoint-initdb.d
    ports:
      - "6033:3306"
    healthcheck:
      test: "/usr/bin/mysql --user=root --password=root--execute \"SHOW DATABASES;\""
      interval: 2s
      timeout: 20s
      retries: 10
  my-app:
    restart: on-failure
    build: ./
    ports:
    - 8888:8080
    environment:
      WAIT_HOSTS: mysql:3306
    depends_on:
    - docker-mysql
```

# Docker Mysql Container Connection :

https://phoenixnap.com/kb/mysql-docker-container

# Docker Networking : 

1. Every installation of the Docker Engine automatically includes three default networks. You can list them:

```
$ docker network ls

NETWORK ID          NAME                DRIVER
18a2866682b8        none                null
c288470c46f6        host                host
7b369448dccb        bridge              bridge
```


2. The network named bridge is a special network. Unless you tell it otherwise, Docker always launches your containers in this network. Try this now:
```
$ docker run -itd --name=networktest ubuntu

74695c9cea6d9810718fddadc01a727a5dd3ce6a69d09752239736c030599741
```

3. Inspecting the network is an easy way to find out the container’s IP address.
```
$ docker network inspect bridge
```

4. You can remove a container from a network by disconnecting the container. To do this, you supply both the network name and the container name. You can also use the container ID. In this example, though, the name is faster.
```
$ docker network disconnect bridge networktest
```

While you can disconnect a container from a network, you cannot remove the builtin bridge network named bridge. Networks are natural ways to isolate containers from other containers or other networks. So, as you get more experienced with Docker, create your own networks.


## Create your own bridge network
```
$ docker network create -d bridge my_bridge
```
The -d flag tells Docker to use the bridge driver for the new network. You could have left this flag off as bridge is the default value for this flag.

Use the docker network rm command to remove a user-defined bridge network. If containers are currently connected to the network, disconnect them first.
```
$ docker network rm my_bridge
```

## Connect a container to a user-defined bridge🔗

When you create a new container, you can specify one or more --network flags. This example connects a Nginx container to the my-net network. It also publishes port 80 in the container to port 8080 on the Docker host, so external clients can access that port. Any other container connected to the my-net network has access to all ports on the my-nginx container, and vice versa.
```
$ docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest
  ```
To connect a running container to an existing user-defined bridge, use the docker network connect command. The following command connects an already-running my-nginx container to an already-existing my-net network:
```
$ docker network connect my-net my-nginx
```
To disconnect a running container from a user-defined bridge, use the docker network disconnect command. The following command disconnects the my-nginx container from the my-net network.
```
$ docker network disconnect my-net my-nginx
```


# Dockerizing a Node.js Project

# Steps 1 to Dockerize a Node.js Project

 # Create a Dockerfile:
Create a file named Dockerfile in the root directory of your Node.js project and add the following content:

# Use an official Node.js runtime as a parent image
FROM node:16

# Set the working directory inside the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json into the container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code into the container
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Define the command to run the app
CMD [ "npm", "start" ]


# Step 2: Docker Image Build Karna
Using a Dockerfile, build an image.

 docker build -t my-node-app .

# Step 3: Running a Docker Container

Run The Docker Container

$ docker run -p 3000:3000 -d my-node-app

# Docker Compose for Node.js Project

# Create docker-compose.yml file

version: '3'
services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      NODE_ENV: development
    command: npm start

  mongodb:
    image: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db

volumes:
  mongodb_data:

 # Docker Compose Commands

 1. Build and Run

   $ docker-compose up --build

 2. Container Stop Command

   $ docker-compose down


# Folder Structure for Node.js Project
Make sure that your project folder structure is correct:

go
Copy code
my-node-app/
│
├── Dockerfile
├── docker-compose.yml
├── package.json
├── package-lock.json
├── node_modules/
└── src/
    ├── index.js
    └── app.js
 
# Common Commands for Node.js Docker Setup
Dependencies update karna:

docker-compose run app npm install

Logs check karna:

docker-compose logs app 

In this way, you can easily Dockerize your Node.js project and use it for a multi-container setup with Docker Compose.


# Dockerizing a Python Project:

 If you have a Python project, you will need to create a Dockerfile to Dockerize it. Below is an example of how to Dockerize a Python application.

# Step 1: Creating the Dockerfile To Dockerize:

  Python application, you need to create a Dockerfile that sets up the Python environment and runs your code. For a simple Python project, the Dockerfile will look something like this:

  # Base image select karein
FROM python:3.9-slim

# App directory create karein

WORKDIR /app

# Required dependencies ko install karne ke liye requirements.txt file ko copy karein
COPY requirements.txt ./

# Python dependencies install karein

RUN pip install --no-cache-dir -r requirements.txt

# Application ka code copy karein
COPY . .

# Port expose karein (Agar aapka app kisi specific port pe listen kar raha ho)
EXPOSE 5000

# Default command set karein
CMD ["python", "app.py"]

# Step 2: Create a Requirements

 File You will need to specify the dependencies of your Python project in a requirements.txt file, such as:

 Flask==2.0.1
requests==2.25.1

# Step 3: Build Docker Image
Build an image using this Dockerfile.

docker build -t my-python-app .

# Step 4: Run Docker Container

docker run -p 5000:5000 -d my-python-app

# Docker Compose for Python Project

# Create docker-compose.yml

version: '3'
services:
  app:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
      - /app/__pycache__
    environment:
      FLASK_ENV: development
    depends_on:
      - postgres

  postgres:
    image: postgres:13
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:


 # Docker Compose Commands

 1. Build and Run

   $ docker-compose up --build

 2. Container Stop Command

   $ docker-compose down

# Folder Structure for Python Project

Make sure that your project folder structure is correct:

my-python-app/
│
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── app.py
└── __init__.py

# Common Commands for Python Docker Setup
# Update Dependencies:

docker-compose run app pip install -r requirements.txt

# Check logs:

docker-compose logs app



In this way, you can easily Dockerize your Python project and use it for a multi-container setup with Docker Compose.



# Git Installation
To install git follow the instructions mentioned in the below url for Windows, Linux and Mac OS.
 ```bash
 https://www.atlassian.com/git/tutorials/install-git
```
Once the installation is done please follow the basic configuration as mentioned below.
```bash
1) check git version > git --version
2) configure username > git config --global user.name = "your name"
3) configure email > git config --global user.email = "your email"
```
You are ready to go !!!

# Git Commands and Descriptions

## git --version
this commands gives you the installed version of the git in your machine.

## Creating a git repository
> To create a directory
 ```bash
  mkdir <directory name>
  
```
> To Change the directory
```bash
cd <directory name>

 ``` 
## Git Commands 
> Converts a directory into a local git repository.
  ```bash
  git init      - converts a directory into a git repository
 ```
 
 > To add a single or muiltiple files to staging area
  ```bash
  git add fileName or git add .  
  
 ```
 
 > To commit the changes in the local repository.
  ```bash
  git commit -m "commit message" 
 ```

> To list the status of the files in staging area.
```bash
 git status  
 ```


> To configure the global username and email id

  ```bash
   git config --global user.name = "Uzair Rafiq"
   git config --global user.email - "uzairrafq@gmail.com"
   
   git config user.name = "Uzair Rafiq"
   git config user.email = "uzairrfq@gmail.com"
 ```
 
### Connect and add Remote Repository.
 > To connect a local repository to a specific remote repository.
 ```bash
 git remote add origin <ssh url>
```
 
> To Clone a remote git repository into a local git repository.
```bash
 git clone <repo name> 
```

> To Pull the changes from master branch of the remote git repository.
```bash
git pull origin master 
```
 
> To Push the changes to master branch of the remote git repository.
```bash
git push origin master 
```
  
### Branching 

> To List all the branches
```bash
git branch
```

> To create a new branch
```bash
git branch $branchName
```

> To delete an existing branch 
```bash
git branch -d $branchName
```

> To merge a child branch to master
```bash
git merge $branchName
```
  
### Advance git commands
> To save the changes, when they are not in state of commit.
```bash
git stash
```

> To stash changes and clearing up the directory
```bash
git stash -u
```

> To List the stashes created.
```bash
git stash list
```

> To get the stashed work back.
```bash
git stash apply
```

> To get the context and history of logs of a repository
```bash
git log
```
 
 > To Copy and Store a set of commit outside of our repository.
 ```bash
git rebase
```

> To revert back to the previous version of the file.
```bash
git revert
```
 


## Pre-requisite : 
Jenkins runs on Java, So please make sure you have JDK1.8 and respective JRE installed on your system.


# Jenkins Installation

In order to install Jenkins on your windows machine, Please follow instructions at below url.

  https://www.guru99.com/download-install-jenkins.html

Once the Jenkins is installed, access Jenkins using url 

  http://localhost:8080

# Any Operating system 

Please follow instructions at below link.

https://jenkins.io/download/

To run Jenkins.war, go to the specific folder and run

Java -jar jenkins.war

Access Jenkins at

http://localhost:8080



# CI/CD
# Steps for End to End Pipeline Automation
Pre-requisite :

JDK1.8 or +
Eclipse Neon or +
Jenkins latest

1. Build and Test the Application.
Open the Command Prompt and check Java installation.

java -version
Navigate to the project folder, where the pom.xml file resides (if you have one) or where your Spring Boot application resides. 

Run the following command to build the application (use Gradle, or a custom script, instead of Maven).
For Gradle:

gradle build
Or if you have a custom script, you can run it, for example:

./build.sh
3. Upload the Code to the GitHub Repository.

Create an account on https://www.github.com

Login to the Account.

Create a public repository.

Copy the repository URL to clone. (e.g., it looks like this: https://github.com/uzairrafq/DevOps.git)
Open the command prompt on your machine and navigate to the project folder.
Initialize the local git repository. (Inside the project folder, run the following commands)

git init

git config --global user.name "your name"

git config --global user.email "your email address that you have used to login to GitHub"

git add .

git commit -m "Code committed to local repository"

git remote add origin <the git repository URL that you have copied above>

git push origin master
Note: Check the repository on github.com. Your application code should be there in your GitHub repository.

4. Creating Jenkins Pipeline.
To launch Jenkins, go to the folder where jenkins.war is placed and run the following command.

java -jar jenkins.war
Go to the browser and open http://localhost:8080
Login with the admin user and password that you created during the first login after the installation process.
Note: If not, please follow steps at: https://www.guru99.com/download-install-jenkins.html

Click on New Item > Choose Pipeline Project > Give any name (e.g., ci-pipeline) > hit Enter.
Click on configure > Click on the last tab Pipeline.
Note: Pipeline scripts need to be written in Groovy script. A template is shown below.
groovy

node{
	stage('git checkout'){
		// code for checkout
	}
	
	stage('build'){
		// code for build (e.g., Gradle or custom script)
	}
	
	stage('test'){
		// code for test (e.g., run tests)
	}
	
	stage('install'){
		// code for install (e.g., deploy or package)
	}
}
Once the pipeline code is done, click on save and Build Now.
If it is a blue button, it means success. If it's a red button, it means fail. Click on the button to see the logs and identify errors.


































