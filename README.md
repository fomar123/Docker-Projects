# Docker Projects

# Developing with Docker
To start the application: 

##### Step 1: Create docker network
docker network create mongo-network 

##### Step 2: start mongodb
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    

##### Step 3: start mongo-express
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   

##### Step 4: open mongo-express from browser
http://localhost:8081

##### Step 5: create user-account db and users collection in mongo-express

##### Step 6: Start your nodejs application locally - go to app directory of project
cd app
npm install 
node server.js

##### Step 7: Access you nodejs application UI from browser
http://localhost:3000

# Docker compose: Running multiple Docker Contaniners
##### Create docker compose file: 
version: '3'
services:
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
      
  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8080:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb

##### To start the application
##### Step 1: start mongodb and mongo-express
docker-compose -f mongo.yaml up

You can access the mongo-express under localhost:8080 from your browser
##### Step 2: in mongo-express UI - create a new database "my-db"
##### Step 3: in mongo-express UI - create a new collection "users" in the database "my-db"
##### Step 4: start node server
cd app
npm install
node server.js
##### Step 5: access the nodejs application from browser
http://localhost:3000

# Build Docker Image: 
##### Step 1: Create dockerfile:
FROM node:13-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

RUN mkdir -p /home/app

COPY . /home/app

CMD ["node", "server.js"]


##### Step 2: Build a docker image from the application
docker build -t my-app:1.0 .       
The dot "." at the end of the command denotes location of the Dockerfile.
##### Step 3: Started newly created Docker Image
   docker run my-app:1.0
   
   

# Private Docker Repository - Push Docker Image into a private registry on AWS

##### Step 1: Created private Docker Registry on Amazon ECR

##### Step 2: Log into private repository = docker login

##### Step 3: Build your Docker image using the following command: docker build -t my-app:1.0

##### Step 4: After the build is completed, tag your image so you can push the image to this repository: docker tag my-app:1.0 931407886896.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0

##### Step 5 Run the following command to push this image to your newly created AWS repository: docker push 931407886896.dkr.ecr.us-east-1.amazonaws.com/my-app:latest


# Deploy Docker Containers on a Remote Server 
##### Step 1 create a mongo.yaml file and add my-app container to a list containers (mongodb and mongo-express: 
version: '3'
services: my-app:
image: 931407886896.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0
ports: - 3000:3000
mongodb:
image: mongo
ports: - 27017:27017
environment: - MONGO_INITDB_ROOT_USERNAME=admin
- MONGO_INITDB_ROOT_PASSWORD=password
mongo-express:
image: mongo-express
restart: always
ports:
- 8080:8081
environment:
- ME_CONFIG_MONGODB_ADMINUSERNAME=admin - ME_CONFIG_MONGODB_ADMINPASSWORD=password - ME_CONFIG_MONGODB_SERVER=mongodb

##### Step 2 Change mongodb server url from localhost to mongodb service name in Node Code : let mongoUrlLocal "mongodb://admin:password@mongodb:27017";
##### Step 3 Start docker containers with docker-compose: docker-compose -f mongo.yaml up


# Docker Volumes – Persist database volumes

##### Step 1: Define a Named Volume in Docker Compose File
##### Step 2:  Create a docker-compose.yaml file with the following with the following containers to the list of containers:
version: '3'
services: my-app:
image: 931407886896.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0
ports: - 3000:3000
mongodb:
image: mongo
ports: - 27017:27017
environment: - MONGO_INITDB_ROOT_USERNAME=admin
- MONGO_INITDB_ROOT_PASSWORD=password
mongo-express:
image: mongo-express
restart: always
ports:
- 8080:8081
environment:
- ME_CONFIG_MONGODB_ADMINUSERNAME=admin - ME_CONFIG_MONGODB_ADMINPASSWORD=password - ME_CONFIG_MONGODB_SERVER=mongodb


##### Step 3: Start the container with the following command: docker-compose -f docker-compose.yaml up
##### Step 4: Start the node.js application with npm run start 
##### Step 5: Edit the docker-compose.yaml file  and add  the docker volumes, add the following configuration: 
volumes:

 mongo-data:/data/db
 
 volumes:
 
  mongo-data:
  
   driver: local
  
##### Step 6: restart docker-conpose : docker-compose -f docker-compose.yaml down and then docker-compose -f docker-compose.yaml up

# Run Nexus as Docker Container on DigitalOcean Droplet

##### Step 1: Create a new droplet on digital oceans
##### Step 2: Configure firewalls rule to open port 22 for SSHing
##### Step 3: Install docker on droplet using command:  snap install docker 
##### Step 4: Create docker volume to persist Nexus data, use command docker volume create --name nexus-data
##### Step 5: Run Nexus as Docker container with necessary parameters. Use command : docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
##### Step 6: Accessed Nexus in browser with IP address and port 8081


