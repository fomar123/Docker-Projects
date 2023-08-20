# Docker Projects

# Developing with Docker: Setting Up a MongodDB Application
##### Step 1: Create Docker Network
Create a Docker network to enable container communication:

        docker network create mongo-network
        
##### Step 2: Start MongoDB Container
- Launch the MongoDB container with the following command:

         docker run -d -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo
  
##### Step 3: Start Mongo Express Container
- Begin the Mongo Express container while connecting it to the network:

         docker run -d -p 8081:8081 -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin -e ME_CONFIG_MONGODB_ADMINPASSWORD=password --net mongo-network --name mongo-express -e ME_CONFIG_MONGODB_SERVER=mongodb mongo-express

##### Step 4: Access Mongo Express
Open a web browser and enter the following URL to access the Mongo Express interface:
    
        http://localhost:8081

# Docker Compose: Running Multiple Docker Containers

Step 1: Create Docker Compose File

Create a docker-compose.yml file with the following contents:

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

 
      
##### Step 2: Start the Application
- Launch MongoDB and Mongo Express containers using the Docker Compose file:

      docker-compose up -d

- Access the Mongo Express interface by visiting http://localhost:8080 in your web browser.

##### Step 3: Set Up the MongoDB Database and Collection
- In the Mongo Express UI:
- Create a new database named "my-db."
- Inside "my-db," create a new collection named "users."

##### Step 4: Start Node Server
- Navigate to the app directory and set up the Node.js server:

       cd app
       npm install
       node server.js
  
##### Step 5: Access the Node.js Application
- Access the Node.js application from your browser:

  http://localhost:3000

# Building a Docker Image for Your Application:
##### Step 1: Create Dockerfile
- Create a Dockerfile with the following content:


      FROM node:13-alpine

      ENV MONGO_DB_USERNAME=admin
      ENV MONGO_DB_PWD=password

      RUN mkdir -p /home/app

      COPY . /home/app

      WORKDIR /home/app
    
     CMD ["node", "server.js"]
  
##### Step 2: Build the Docker Image
- Build a Docker image from your application using the following command:

       docker build -t my-app:1.0 .
  
- The dot "." at the end of the command signifies the location of the Dockerfile.

##### Step 3: Run the Docker Image
- Launch a container from the newly created Docker image:

        docker run my-app:1.0
   
   

# Pushing a Docker Image into a Private AWS ECR Repository:

##### Step 1: Create Private Docker Registry on Amazon ECR
- Create a private Docker registry in Amazon ECR. This establishes a repository where you can store your Docker images securely.

##### Step 2: Log into the Private Repository
- Use the docker login command to log in to your private repository on ECR. This step ensures proper authentication for image push operations.

##### Step 3: Build Your Docker Image
- Build your Docker image using the following command:

      docker build -t my-app:1.0 .
  
##### Step 4: Tag the Image for the ECR Repository
- Tag your built image with the ECR repository URI and the desired tag:

     docker tag my-app:1.0 <account_id>.dkr.ecr.<region>.amazonaws.com/my-app:1.0
  
- Replace <account_id> with your AWS account ID and <region> with the AWS region.

##### Step 5: Push the Image to ECR
- Push the tagged image to your ECR repository:

      docker push <account_id>.dkr.ecr.<region>.amazonaws.com/my-app:1.0

# Deploying Docker Containers on a Remote Server:

##### Step 1: Create mongo.yaml File
- Create a mongo.yaml file and configure the services as follows:


     version: '3'
     services:
       my-app:
        image: 931407886896.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0
        ports:
          - 3000:3000
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
  
##### Step 2: Update MongoDB Server URL in Node Code
- Adjust the MongoDB server URL in your Node.js application code. Change it from localhost to the MongoDB service name (mongodb) in the mongoUrlLocal variable:


       let mongoUrlLocal = "mongodb://admin:password@mongodb:27017";
  
##### Step 3: Start Docker Containers on Remote Server
- Initiate the Docker containers on the remote server using the docker-compose command:

     docker-compose -f mongo.yaml up let mongoUrlLocal "mongodb://admin:password@mongodb:27017";
  
##### Step 3 Start docker containers with docker-compose:

      docker-compose -f mongo.yaml up


# Docker Volumes – Persistent Database Volumes:

##### Utilise Docker volumes to ensure persistent storage for your database. Follow these steps:

##### Step 1: Define a Named Volume in Docker Compose File
- Specify a named volume in your docker-compose.yaml to ensure data persistence for your database container.

##### Step 2: Update docker-compose.yaml File

In your docker-compose.yaml file, integrate the containers while keeping data persistence in mind:

     version: '3'
     services:
       my-app:
        image: 931407886896.dkr.ecr.us-east-1.amazonaws.com/my-app:1.0
        ports:
          - 3000:3000
       mongodb:
         image: mongo
         ports:
           - 27017:27017
       environment:
           - MONGO_INITDB_ROOT_USERNAME=admin
           - MONGO_INITDB_ROOT_PASSWORD=password
       volumes:
           - mongodb_data:/data/db
      mongo-express:
        image: mongo-express
        restart: always
        ports:
          - 8080:8081
       environment:
          - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
          - ME_CONFIG_MONGODB_ADMINPASSWORD=password
          - ME_CONFIG_MONGODB_SERVER=mongodb

     volumes:
       mongodb_data:

# Running Nexus as a Docker Container on DigitalOcean Droplet:

##### Step 1: Create a New Droplet on DigitalOcean
- Create a new Droplet on DigitalOcean, which will serve as the host for your Nexus Docker container.

##### Step 2: Configure Firewall Rules
- Set up firewall rules to allow SSH access by opening port 22 on the Droplet.

##### Step 3: Install Docker on Droplet
- Install Docker on the Droplet using the following command:

      snap install docker
  
##### Step 4: Create Docker Volume for Nexus Data
- Create a Docker volume to ensure persistence of Nexus data:

      docker volume create --name nexus-data
  
##### Step 5: Run Nexus as Docker Container
- Run the Nexus Docker container with the necessary parameters using the following command:

      docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
  
##### Step 6: Access Nexus in Browser
- Access the Nexus instance in a web browser using the Droplet's IP address and port 8081:

      http://droplet_ip_address:8081



