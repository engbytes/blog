---
title: "Docker 101"
date: 2020-05-28T12:04:13+05:30
draft: false
---

#### WHAT IS DOCKER ?

To put it simply, Docker is a way of packing & distributing software.

#### WHY DO WE NEED DOCKER ?
* Let's say your are building a Python application using Django. You can provide list of Python dependencies/packages needed for the project inside requirements.txt file.
* Let's say one of the dependency is celery. For celery to work it will need a backend ( let's say we are using rabbitmq as backend for celery)
* For the project to work on a system you will have to install rabbitmq separately.
* Now Imagine your colleague wants to set up this project locally on his/her laptop. They will have to
    * Step 1 : Download the code from repo.
    * Step 2 : Install dependencies using requirements.txt.
    * Step 3 : Install rabbitmq on the system manually.
   
    __Step 3__ is where the problem starts. For e.g.
    
    1. You might have instructions for installing rabbitmq on linux but your friend is using mac.
    2. Let's say if they ended up installing latest version but your app requires specific version of rabbitmq (latest version is not supported).

    
There are many more reasons but let's say for now it's clear that we might need a better way to handle these problems.

``` SO HOW DOES DOCKER SOLVE ABOVE PROBLEMS ? ```

Sure, we will surely discuss that but before we jump to that, let's understand a bit more about how Docker works.

#### HOW DOCKER WORKS ?

__Docker consists of three main parts__

---
1. Dockerfile

    A Dockerfile is a file that you create which produces a Docker image when you build it.
    
2. Docker Image
    
    An image is a read-only template with instructions for creating a Docker container. You can find pre-built images for
    commonly used softwares on dockerhub
    for e.g : [https://hub.docker.com/_/rabbitmq] (https://hub.docker.com/_/rabbitmq)
    
    Also you can build Docker images from Docker file using command below
    
    ```docker build -f <docker_file_path> -t <name:tag>```


3. Docker Container

    A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. 

    For e.g. You can start a container is command below
    
    ``` docker run -d <dockerimage_name> ```

---


 
#### So what if we have more than one docker containers?

##### docker-compose

Docker Compose is an official tool that helps you manage multiple Docker containers by letting you define everything through a docker-compose.yml file.


___

> Coming back to the solution of our problem. How do we help our colleague install the project locally
on his/her computer without any hassle using Docker?

Well all you have to do is add the below file to your code repo.

```docker-compose.yml```

{{< highlight yaml>}}

version: '3.3'

services:
  my_rabbitmq:
    image: rabbitmq:3.7.8-alpine

    ports:
      - "6672:5672"
      - "16672:15672"

{{< / highlight >}}


Anyone who wants to test out the application can checkout the django repo and run


1. pip install -r requirements.txt ( to install all necessary Python packages )
2. docker-compose up ( to install all other dependencies, in our case rabbitmq )


### MISCELLANEOUS

__Some frequently used commands for docker containers__

1. ```docker ps```

    * Shows all the docker containers running in the background

2. ```docker stop <container_id>```

    * stops the container ( alive but not running)

3. ```docker start <container_id>```

    * starts the container ( that was in stop state)

4. ```docker rm -f <container_id/name>```

    * force remove/kill/delete a container

5. ```docker ps -a```

    * shows all the docker containers that are in stopped stage or exited stage

6. ```docker stats <container_id>```

    * Shows resource utilisation by the container ( CPU, Memory etc )

7. ```docker logs <container_id>```

    * Fetch the logs of a container

8. ```docker exec -it <container_id> bash```

    * Run a command in a running container
    




