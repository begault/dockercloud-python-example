# 1. Install Docker & verify its state

## Installation
https://docs.docker.com/engine/installation/

## State check
Run docker run hello-world in a terminal
( You may need to update you ~/.docker/config.json file rights using the chown command) 

# 2. Build the app

## 2.1. Setup the container

Find all what's inside the different files here:
https://docs.docker.com/get-started/part2/#dockerfile

### Container environment
``` touch dockerfile ```

The dockerfile define what goes on in the environment inside the container.

### The app

``` touch requirements.txt ```

This file contains a list of all the additional required libraries to install to run our app.

The command ``` pip install -r requirements.txt ``` will install these libraries. 

``` touch app.py ```

This file contains our app code. Its purpose is to print the environment variable ```NAME```, as well as the output of a call to socket.gethostname().

## 2.2. Build the app

``` docker build -t friendlyhello . ``` .
This command creates a Docker image. 
Its identifier will be ```friendlyhello``` (with option -t).
The location is the current folder, but it can be updated by overwriting the ```.``` character. 

To know where the built image is stored, we can use the command : ``` docker images ```. 

## 2.3. Run the app

``` docker run -p 4000:80 friendlyhello ```.

This command runs the app:
- 4000 is the machine port where the container port 80 is mapped. (option -p) 
- The app is reachable on the browser at ``` http://localhost:4000 ``` 
- Other possibility : ``` curl http://localhost:4000 ```

Run the app in detached mode : 
``` docker run -d -p 4000:80 friendlyhello ```.

To find the container ID : ```docker ps```.

To end the detached process : ```docker stop THE_CONTAINER_ID```

## 2.4. Share the image

### Log in on Docker

```docker login ```

### Tag the image

Let's identify our app location:
``` docker tag friendlyhello username/get-started:part1 ```

With :
- friendlyhello : the name of the app
- username : the username of the account
- get-started : the repository of the image
- part1 : the tag / identifier of the image

The new image is visible with the following command : ``` docker image ls ```

### Publish the image

``` docker push username/get-started:part1 ``` is used to upload the new image on the repository. 

The new image is visible on ``` https://hub.docker.com/ ```. 

### Run the image locally

``` docker run -p 4000:80 username/repository:tag ```

This will pull the image locally if needed and run the app on the machine.

# 3. Use services

## 3.1. Define a service

``` touch docker-compose.yml ```

[This file](https://docs.docker.com/get-started/part3/#your-first-docker-composeyml-file) define how Docker containers should behave in production.

The docker-compose.yml file asks Docker to do the following tasks:
- Pull the image uploaded previously.
- Run 5 instances of the image as a service called ```web```. Each instance is limited to use a maximum of 10% of the machine CPU (across all cores) and 50MB of RAM.
- Directly restart containers if one of them fails.
- Map port 80 on the host to ```web```’s port 80.
- Ask ```web```’s containers to share port 80 via a load-balanced network called ```webnet```.
- Define the ```webnet``` network with the default settings.
