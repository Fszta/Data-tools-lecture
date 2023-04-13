## Docker in practice

Let's setup a local environment with the following components : 

* A database : postgresql
* An database administration platform : pgadmin
* A Business intelligence app : metabase


!!! danger

    This is a local setup, in a real life project, you'll not host your database in a docker container
    Also, metabase use a database to store its metadata, by default it comes with an sqllite, which must be replaced by
    another external database like postgres or mysql


## Docker, a simple way to setup your local environment

You can deploy any application using a very short command... with docker !

### Deploy a database
```bash
docker run -d \
    --name postgres \
    -p 5432:5432 \
    -e POSTGRES_PASSWORD=password \
    -v postgres:/var/lib/postgresql/data \
    postgres:15.2

```

Let's decompose this command : 

| Argument | Description |
| --- | --- |
| `docker run` | Command to start a new container |
| `-d` | Runs the container in the background (detached mode) |
| `--name postgres` | Assigns the name `postgres` to the new container |
| `-p 5432:5432` | Maps port 5432 on the host machine to port 5432 in the container |
| `-e POSTGRES_PASSWORD=password` | Sets an environment variable `POSTGRES_PASSWORD` with the value `password` inside the container |
| `-v postgres:/var/lib/postgresql/data` | Mounts a Docker volume named `postgres` to the container directory `/var/lib/postgresql/data` |
| `postgres:15.2` | Specifies the name and tag of the PostgreSQL Docker image to use for the new container |

Now you've a running postgres instance which is listening on port 5432, you can connect to it using `psql` or any client.


You can check it by running `docker ps` or directly in docker desktop if you prefer the UI.

You will have an output like this : 
```bash
CONTAINER ID   IMAGE           COMMAND                  CREATED         STATUS         PORTS                    NAMES
3ca9e24601b5   postgres:15.2   "docker-entrypoint.s…"   5 seconds ago   Up 4 seconds   0.0.0.0:5432->5432/tcp   postgres

```

!!! tip

    It's better to use the cli (the commands) instead of the UI, it will help you to become more familiar with docker


<br>
### Deploy pgadmin
For the exercise, let's say you want setup a web client to monitor and administrate your database.
Let's deploy pg admin !


```bash
docker run \
  -p 5050:80 \
  -e "PGADMIN_DEFAULT_EMAIL=email@example.com" \
  -e "PGADMIN_DEFAULT_PASSWORD=password" \
  -d dpage/pgadmin4:latest
```


To be sure you understand... let's describe the command again

| Argument | Description |
| --- | --- |
| `docker run` | Command to start a new container |
| `-p 5050:80` | Maps port 80 inside the container to port 5050 on the host machine |
| `-e "PGADMIN_DEFAULT_EMAIL=email@example.com"` | Sets an environment variable `PGADMIN_DEFAULT_EMAIL` with the value `email@example.com` inside the container |
| `-e "PGADMIN_DEFAULT_PASSWORD=password"` | Sets an environment variable `PGADMIN_DEFAULT_PASSWORD` with the value `password` inside the container |
| `-d` | Runs the container in the background (detached mode) |
| `dpage/pgadmin4:latest` | Specifies the name and tag of the pgAdmin4 Docker image to use for the new container |


!!! info

    You'll notice that this time, the version is set using :latest, this is a tag that refers to the most recent version
    of a Docker image. It's better to specify a version like dpage/pgadmin4:4.32 this would ensure that you always use the
    same version of the image, regardless of whether a new "latest" version is released.


<br>
Now, if you go to [localhost:5050]() you'll be able to access the pg admin interface. <br>

![pgadmin](https://tu-graz-library.github.io/docs-repository/services/images/pgadmin-login.png?raw=true)



If you perform a `docker ps` you'll see two containers, one for postgres and one for pgadmin


```bash
CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS         PORTS                                       NAMES
3ca9e24601b5   postgres:15.2      "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   postgres
a08f7a258dbf   dpage/pgadmin4:4.23 "/entrypoint.sh"         5 minutes ago   Up 5 minutes   0.0.0.0:5050->80/tcp, :::5050->80/tcp       pgadmin
```

<br>

### Deploy a BI platform, metabase
There are many BI platforms, however only few are open-source. In addition, metabase can be deployed in seconds... 
with docker.
<br>
```bash
docker run -d -p 3000:3000 --name metabase metabase/metabase
```
<br>
If you perform a docker ps you'll see 3 containers : 
```bash
CONTAINER ID   IMAGE               COMMAND                  CREATED         STATUS         PORTS                                       NAMES
3ca9e24601b5   postgres:15.2      "docker-entrypoint.s…"   5 minutes ago   Up 5 minutes   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   postgres
a08f7a258dbf   dpage/pgadmin4:4.23 "/entrypoint.sh"         5 minutes ago   Up 5 minutes   0.0.0.0:5050->80/tcp, :::5050->80/tcp       pgadmin
cc8a7f165c43   metabase/metabase:0.45 "/app/run_metabase.s…"  5 seconds ago    Up 4 seconds    0.0.0.0:3000->3000/tcp                     metabase
```

<br>
Metabase is now accessible on [localhost:3000](). You'll have to wait a bit of time to be able to access it and have 
a page like this:  <br><br>

![Metabase](https://kirelos.com/wp-content/uploads/2020/01/echo/metabase-ubuntu-18.04-welcome-page-min.png)

<br>
### All together, docker-compose

We've seen how to run single container application, using separated commands, but this is not very convenient...
As previously mentioned, there is a way to manage multi-container applications : `docker-compose`

!!! info

    You will not have to learn commands for both `docker` and `docker-compose`, basically you'll find the 
    same commands. Ex : `docker ps` become `docker-compose ps`

In order to use docker-compose, you need to write a `docker-compose.yml` file that describe each container you want to 
deploy.

Here is an example of a file describing our 3 previously deployed application : 

<br>

```yaml
version: "3"

services:
    postgres:
        image: postgres:14.2-alpine
        restart: always
        environment:
            POSTGRES_PASSWORD: postgres
            POSTGRES_USER: postgres
            POSTGRES_HOST: "0.0.0.0"
        volumes:
            - postgres:/var/lib/postgresql/data
    pgadmin:
        image: dpage/pgadmin4:4.23
        environment:
            PGADMIN_DEFAULT_EMAIL: admin@pgadmin.com
            PGADMIN_DEFAULT_PASSWORD: password
            PGADMIN_LISTEN_PORT: 80
        ports:
            - 8080:80
        volumes:
            - pgadmin:/var/lib/pgadmin
        depends_on:
            - postgres
    metabase:
      image: metabase/metabase
      ports:
        - "3000:3000"
      restart: always
volumes:
    postgres:
    pgadmin:

```

**Inside the folder** where you `docker-compose.yml` file is located, simply run `docker-compose up -d`.


!!! info 

     * The version "3" at the beginning of the file indicates that this is a Docker Compose file written in version 3 syntax.
     * The file defines three services: postgres, pgadmin, and metabase.
     * The postgres service runs the official PostgreSQL 14.2-alpine image, which is a lightweight version of the PostgreSQL database running on the Alpine Linux distribution. It sets environment variables for the database user and password, and also specifies a volume to store the database data.
     * The pgadmin service runs the dpage/pgadmin4:4.23 image, which is a web-based administration tool for PostgreSQL. It sets environment variables for the default email and password, and exposes port 8080 on the host machine to port 80 on the container. It also specifies a volume to store pgAdmin data, and depends on the postgres service to be running.
     * The metabase service runs the metabase/metabase image, which is a business intelligence and analytics tool. It exposes port 3000 on the host machine to port 3000 on the container, and sets the restart policy to always.
     * The volumes section defines two named volumes, postgres and pgadmin, which are used by the postgres and pgadmin services respectively to store data.

