## Introduction

Docker is a software platform that allows developers to package, distribute, and run applications in a containerized 
environment. Containers are lightweight, portable, and self-contained environments that can run almost anywhere, 
from a developer's laptop to a cloud-based infrastructure.

### What is containerization ?

![Docker container](https://www.docker.com/wp-content/uploads/2021/11/docker-containerized-appliction-blue-border_2.png){ align=left }


## Key concepts & components
!!! tip
    
    The best way to be familiar with docker is to practice, also they provide a great [documentation](https://docs.docker.com)


### 1. Dockerfile
A Dockerfile is a text file that contains instructions on how to build a Docker image. The Docker image is a binary file 
that contains everything needed to run an application, including the code, libraries, and dependencies. 

### 2. Docker image 
A Docker image is a snapshot of a container, which includes the application code, libraries, and dependencies. 
Images can be built from a Dockerfile or pulled from a public or private Docker registry.

### 3. Docker registry 
A Docker registry is a repository for storing and sharing Docker images. Docker Hub is the most popular public Docker 
registry, but you can also use private registries for your organization's images.

### 4. Docker container
A Docker container is a lightweight, standalone executable package that includes everything needed to run an 
application, including the application code, libraries, and dependencies. Containers are isolated from the host 
system and from other containers, making them a secure way to run applications.

### 5. Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. With Docker Compose, you can define the services that make up your application, their configuration, and the network they should use to communicate with each other.

### 6. Docker Swarm
!!! info

    It's not very common to use it in a production environment, the standard is to use [kubernetes](https://kubernetes.io/)

Docker Swarm is a native clustering and orchestration tool for Docker. With Swarm, you can create and manage a cluster 
of Docker nodes, and deploy and manage Docker services across the cluster.

## Commands recap
!!! tip

    Once again, you don't need to remind all the commands, just practice and go through the documentation ! 



| Command | Description |
| --- | --- |
| docker run | Run a container |
| docker ps | List running containers |
| docker images | List available images |
| docker build | Build an image from a Dockerfile |
| docker push | Push an image to a remote registry |
| docker pull | Pull an image from a remote registry |
| docker stop | Stop a running container |
| docker rm | Remove a container |
| docker rmi | Remove an image |
| docker exec | Execute a command in a running container |
| docker logs | View the logs of a container |
