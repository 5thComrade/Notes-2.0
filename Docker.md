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

---

## Docker Networks

Each container is connected to a private virtual network "bridge".

Each virtual network routes through NAT firewall on host IP.

All containers on a virtual network can talk to each other without `-p`

The best practice is to create a new virtual network for each app

- network "my_web_app" for mysql and php/apache containers
- network "my_api" for mongo and nodejs containers

Docker follows something called, "Batteries included, but removable". Default works well in many cases, but easy to swap parts to customize it.

You could

- make new virtual networks
- attach containers to more than one virtual network
- skip virtual networks and use host IP
- Use different Docker network drivers to gain new abilities

`-p (--publish)`: Remember publishing ports is always in `HOST:CONTAINER` format.

```sh
# to see the port used by the container and the host

docker container port <container id/name>

# to get the IP address of the container

docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost
```

### My understanding of docker network

So when you spin up a new container(C1) on your host machine, this new container(C1) is by default connected to a virtual network. If we spin up another container(C2), this container(C2) is also connected to the same virtual network. So C1 and C2 can talk to each other even if we don't expose any ports in the containers.

Now lets say I want to open up a port in C1 and allow it to connect to my host machine, then we use the `-p` and expose the port to our machine. The communication between the host and the container will still be through the virtual network.

Since only C1 has an exposed port to the host, the host cannot reach the C2 container. But we now know C1 and C2 can talk to each other.

We can create multiple virtual networks and add containers to that network.

---

### CLI Management of Virtual Networks

`docker network ls` - list out the networks
`docker network inspect` - Inspect a network
`docker network create --driver` - create a network
`docker network connect` - attach a network to a container
`docker network disconnect` - detach a network from container

`--network bridge`: default docker virtual network which is NAT'ed behind the host IP.

```sh
# inspect a network and find containers attached to that network

docker network inspect <network name>
```

`--network host` - it gains performance by skipping virtual networks but sacrifices security of container model

```sh
# create a new virtual network called my_app_net

docker network create my_app_net
```

`network driver` - built-in or 3rd party extensions that give your virtual networks features

```sh
# connect a container to a new network

docker network connect <network id> <container id>

# disconnect a container to a new network

docker network disconnect <network id> <container id>
```

### Default Security

- Create your apps so frontend/backend sit on same Docker network
- Their inter-communication never leaves host
- All externally exposed ports closed by default
- You must manually expose via `-p`, which is better default security

