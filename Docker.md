# Docker

### What is docker?

Docker helps developers build, share, run, and verify applications anywhere — without tedious environment configuration or management.

**Start using Docker online using the [Play with Docker](https://labs.play-with-docker.com/) tool.**

### Why docker?

- Better isolation in a single OS
- reduced environment variances
- increase the speed of change

Docker Desktop is the standard way of using and learning Docker on any machine except for servers. On servers, you do not install Docker Desktop but instead use Docker Engine.

Docker Desktop includes the docker engine, cli, compose, BuildKit, Kubernetes, scan, sbom, and more.

### 3 major ways to run containers

- locally (Docker Desktop)
- servers (Docker Enginer, K8s)
- PaaS (AWS Fargate)

---

#### To check the version of the docker

```sh
docker version
```

#### To check the docker configuration

```sh
docker info
```

#### Docker command line structure

old (still works): 

```sh
docker <command> (options)
```

new 

```sh
docker <command> <sub-command> (options)
```

---

### Image vs Container

- An Image is the application we want to run
- A Container is an instance of that image running as a process
- You can have many containers running off the same image
- Dockers default image registry is called Docker Hub

To start an nginx server on port 80 of your local machine using the nginx image and docker is

```sh
docker container run --publish 80:80 nginx
```

- Downloaded image 'nginx' from Docker Hub
- Started a new container from that image
- Opened port 80 on the host IP
- Routes that traffic to the container IP, port 80

```sh
docker container run --publish 80:80 --detach nginx // This will tell docker to run this in the background and free the terminal for other activities
```

To list all the running containers

```sh
docker container ls // old way: docker ps
```

To stop the container

```sh
docker container stop <container id/name>
```

To list all the containers running or stopped

```sh
docker container ls -a
```

`docker container run` always starts a new container

`docker container start <container id/name>` starts an existing stopped container

To get the logs of a running container

```sh
docker container logs <container id/name>
```

To remove containers

```sh
docker container rm <list of container id/name>
```

To force remove a running container

```sh
docker container rm -f <list of container id/name>
```

#### What happens in 'docker container run'?

- Looks for that image locally in the image cache, but doesn't find anything.
- Then look in the remote image repository (default to Docker Hub)
- Downloads the latest version (nginx:latest by default)
- Creates a new container based on that image and prepares to start
- Gives it a virtual IP on a private network inside docker engine
- Opens up port 80 on host and forwards to port 80 in container
- Starts the container by using the CMD in the image Dockerfile

### List processes inside a container

```sh
docker run --name mongo -d mongo

docker top mongo // this will list the processes running inside the mongo container
```

### Setting env variable for docker container

```sh
docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql // this will generate a random root password for the mysql db running inside the container
```

### What is going on in containers?

- `docker container top` - process list in one container
- `docker container inspect` - details of one container config
- `docker container stats` - performance stats for all containers

### Getting a Shell inside containers

- `docker container run -it` - start new container interactively
- `docker container exec -it` - run additional command in existing container

```sh
docker container run -it --name proxy nginx bash
```

The above command opens a bash shell inside the nginx container. Type `exit` to get out of the shell.

The `bash` [COMMAND] tells the nginx container to ignore the default Command and instead open a Bash Shell that is interactive.

```sh
docker container start -ai ubuntu // This command is used for an interactive shell in existing container
```
