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

Running the image:
    docker run my-app:1.0

CLI of Container:
    docker exec -it 307f8c51e365 /bin/sh

Setting the environment variables now

    ls
    env

### AWS ECR(Elastic container registry)

1. login to aws with root user
2. go to ecr and create repository 
3. Make sure you have image in your device of container
4. AWS provides the login command use from website itself of ecr repository. Prerequisite is AWS CLI and Credentials configured
5. Docker Tag command now to recognize which image to push to AWS
6. you can check the new image copy by docker images
7. docker push now with aws command
8. if you do changes in current docker file or code.. Go for building new image
docker build -t my-app:1.1 .
repeat the same process to  Docker tag step 5. docker tag my-app:1.1 <AWS_ACCOUNT_ID>.dkr.ecr.<AWS_REGION>.amazonaws.com/my-app:1.1
9. docker login is done only once.

![AWS ECR](app/images/AWS%20ECR.png)


### Deploying on the dev server
![AWS ECR](app/images/Deploying%20on%20dev%20server.png)

1. AWS ECR has image my-app and Docker hub for two MongoDB images

2. Do changes as suggested in below image in docker-compose file
![Changes](app/images/Changes%20in%20docker-compose%20file.png)

3. Also added in server.js for mongodb
// use when starting application as docker container
let mongoUrlDocker = "mongodb://admin:password@mongodb";

4. Replace all the mongoUrlLocal in server.js by mongoUrlDocker and build image again
such as it is in:  MongoClient.connect(mongoUrlLocal, mongoClientOptions, function (err, client) {

5. create the docker compose file in the development server, we don't have it now, by vim docker-compose.yaml and paste the same content as we have in github repo file and 

6. docker-compose -f docker-compose.yaml up

# Now testers and other developers will be able to access the development and try out the application you just created





