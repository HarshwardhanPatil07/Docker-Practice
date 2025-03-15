# Docker-Practice

html,javascript,node.js,mondodb,mongo-express

---

## Running with Docker Commands (Without docker-compose)

### Prerequisites

- [Docker installed](https://docs.docker.com/get-docker/)

### Steps

### With Docker without using the docker-compose file

#### To start the application: run below commands

Step 1: Create docker network

    docker network create mongo-network 

Step 2: start mongodb 
  ```bash
  docker run -d -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=password \
  --name mongodb \
  --net mongo-network \
  mongo
  ```
 
  

Step 3: start mongo-express
   ```bash   
  docker run -d -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
  -e ME_CONFIG_MONGODB_SERVER=mongodb \
  --net mongo-network \
  --name mongo-express \
  mongo-express
  ```


Step 4: open mongo-express from browser

    http://localhost:8081

Step 5: create `user-account` _db_ and `users` _collection_ in mongo-express

Step 6: Start your nodejs application locally - go to `app` directory of project 

    npm install 
    node server.js
    
Step 7: Access you nodejs application UI from browser

    http://localhost:3000

### With Docker Compose

#### To start the application

Step 1: start mongodb and mongo-express

    docker-compose -f docker-compose.yaml up
    
_You can access the mongo-express under localhost:8080 from your browser_
    
Step 2: in mongo-express UI - create a new database "user-account"

Step 3: in mongo-express UI - create a new collection "users" in the database "user-account"       
    
Step 4: start node server 

    npm install
    node server.js
    
Step 5: access the nodejs application from browser 

    http://localhost:3000

To stop the container 

    docker-compose -f docker-compose.yaml down

#### To build a docker image from the application

    docker build -t my-app:1.0 .       
    
The dot "." at the end of the command denotes location of the Dockerfile.

Running the image
  docker run my-app:1.0

CLI of Container
  docker exec -it 307f8c51e365 /bin/sh

Setting the environment variables now

  ls
  env
  




