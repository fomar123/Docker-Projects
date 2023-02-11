Docker Projects

##### Developing with Docker
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

##### Docker compose:
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
