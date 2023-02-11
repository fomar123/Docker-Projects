Demo Projects

Developing with Docker
To start the application
Step 1: Create docker network
docker network create mongo-network 
Step 2: start mongodb
docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo    
Step 3: start mongo-express
docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express   
![image](https://user-images.githubusercontent.com/90075757/218236700-fd300b50-f86b-4081-8ebd-a958a984d9e7.png)
Step 4: open mongo-express from browser
http://localhost:8081
Step 5: create user-account db and users collection in mongo-express
Step 6: Start your nodejs application locally - go to app directory of project
cd app
npm install 
node server.js
Step 7: Access you nodejs application UI from browser
http://localhost:3000
![image](https://user-images.githubusercontent.com/90075757/218236750-90cbead7-b3f4-4775-b30f-b54f4c9a1df7.png)
