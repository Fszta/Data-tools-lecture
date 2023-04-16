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
Docker Swarm is a native clustering and orchestration tool for Docker. With Swarm, you can create and manage a cluster 
of Docker nodes, and deploy and manage Docker services across the cluster.

!!! info

    It's not very common to use it in a production environment, the standard is to use [kubernetes](https://kubernetes.io/)


## How to build a docker image for my application ?
!!! tip

    The convention is to provide the configuration in a `Dockerfile` file, however you can give choose another name 
    instead of the default one.     

To distribute your application with docker, you basically need to provide the following information and assets:

* On which environment my image must be based on ? It can be python, ubuntu. This is done using the `FROM` keyword
* What will be the working directory ? This is done using `WORKDIR` keyword
* Provide the source files (for example your python code) of the application and any needed file. This is done using 
`COPY` keyword. It works as follows : `COPY <src> <dest>` 
* Provide the command that must be run during the build process (for example installing python dependencies). 
This is done using `RUN` keyword
* Specify the port that your container will listen on at runtime. For example, if you deploy an API listening on port
`8080`, you need to expose this port to make it accessible from outside the container
* Finally, you need to specify the command to run when a container is started. This is done using ``CMD`` keyword and 
has the following syntax : `CMD ["executable","param1","param2"]` 


There are many other keywords that can be used, for example `ENV` to set environment variables or `ARG` to handle input
argument, you can find the list in [docker documentation]((https://docs.docker.com/engine/reference/builder/))


### Example - Docker image for a flask application
Assuming you have the following hello-world flask api, in a file `app.py` : 

```python
from flask import Flask

app = Flask(__name__)


@app.route('/')
def hello():
    return 'Hello, World!'


if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)

```

Your `requirements.txt` contains : 
```txt
Flask==2.2.3
```


Here the corresponding `Dockerfile` : 

```Dockerfile
# Use the official Python base image
FROM python:3.9

# Set the working directory of the container
WORKDIR /app

# Copy the requirements file containing your dependencies list into the container
COPY requirements.txt .

# Install necessary dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the code of the app (there is only one file), can copy all current dir using `.`
COPY app.py .

# Expose port 5000, the default port used by flask
EXPOSE 5000

# Start the Flask app
CMD ["python", "app.py"]

```
Now, you can build the docker image (the following command will work if you are in the directory containing your 
Dockerfile) : `docker build -t flask-app-example .`

You should have an output like :

```bash
Sending build context to Docker daemon  123.45kB
Step 1/5 : FROM python:3.9
3.9: Pulling from library/python
...
Status: Downloaded newer image for python:3.9
 ---> 1234567890ab
Step 2/5 : WORKDIR /app
 ---> Running in 9876543210ba
Removing intermediate container 9876543210ba
 ---> fedcba987654
Step 3/5 : COPY requirements.txt .
 ---> 0123456789cd
Step 4/5 : RUN pip install --no-cache-dir -r requirements.txt
...
Successfully installed Flask-1.1.2 ...
Step 5/5 : COPY app.py .
 ---> 9876543210dc
Removing intermediate container 9876543210dc
 ---> fedcba987654
Successfully built fedcba987654
Successfully tagged flask-app-example:latest

```

You can list the docker images on your machine with `docker image ls`. You must be able to see the previously created image : 
```bash
REPOSITORY              TAG       IMAGE ID       CREATED       SIZE
flask-app-example       latest    fedcba987654   2 hours ago   123MB
```

!!! info

    As we've not set any tag, the default `latest` will be used, it indicates that this is the latest version.


#### Run your application !
You can now run your application using `docker run -p 5000:5000 -d flask-app-example`


#### Conclusion

If you want to run your application in any cloud environment, you need to push the docker image into
a docker registry. Each cloud provider has its own container registry service. You can do it manually from your local 
machine using the cli of your cloud provider, however it's important to keep in mind that you will not do it
manually in a real project. The process of build and distribution of the docker image will be done in your
`CI/CD` pipeline.

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


!!! warning

    In this page, we only cover the basics of docker, the best way to be more confortable with docker is to practice 
    through your student projects !