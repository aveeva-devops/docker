# docker

Docker enables developers to easily pack, ship, and run any application as a lightweight, portable, self-sufficient container, which can run virtually anywhere. In nutshell, "Containers gives you instant application portability." 

![Alt text](Docker.jpg?raw=true "Title")

## Basic Commands

### Install Docker

Follow steps from https://www.docker.com/get-started 

### Docker build 

Docker build will create docker images

```
git clone https://github.com/aveeva-devops/docker 
javac Helloworld.java
docker build --tag "docker-hello-world:latest" .

```

--tag is name of docker image. This image can be used anywhere now.

### Docker pull

Docker pull is used to pull remote image

```
docker pull jenkinsci/slave
```


### Docker Run

#### docker run will create docker container from image.

```
docker run docker-hello-world:latest
docker run -it --rm -d -v "localdir":remotedir image_name
docker run -it --rm -d -v "/tmp":/home/user docker-hello-world:latest
docker run -it -d jenkinsci/slave
```

--rm will help to remove docker container once you are logged out of container
-d will execute docker in backend
-v volume mounted for localdir, where localdir is directory on host server


### Docker exec

#### Docker exec can be used to execute command on docker container, for instance, below command will login on container

```
docker exec -it name_of_container bash
docker exec -it jenkins bash
```

### Docker inspect

#### Docker inspect is used to inspect meta-deta of docker services, for example, it gives information on docker containers, images etc

```
docker inspect name_of_container
docker inspect image
docker inspect jenkins
docker inspect jenkinsci/slave
```

### Docker logs

#### Docker logs can be used to troubleshoot docker runtime issues, for example, docker logs dockerid will give your logs info for container

```
docker logs jenkins
docker logs container_id
```


### Docker Images

#### Docker images will help to work with docker images. Display, inspect, delete images

```
docker images (Display docker images)
docker inspect image_name (inspect properties of image)
docker rmi image_name (remove image)
```


### Docker networks

#### Everything about docker networks comes under docker networks. 

Check docker open ports

```
docker port container_name
```

Each docker connect with other docker container on a default network called bridge

Get IP Address of docker container

```
docker inspect --format '{{ .NetworkSettings.IPAddress }}' container_name
```

Check connectivity between two docker containers

```
docker exec -ti container_1 ping conatiner_2
```

## Docker day to day commands - Maintenance: 

#### Restart docker container

```
docker restart container_id
```

#### Stop docker container

```
docker stop container_id
```

#### Start docker container

```
docker start container_id
```

#### Bring up all stopped containers

```
docker start $(docker ps -a -q)
```

#### Find out docker base size

```
docker inspect --format {{.GraphDriver.Data.DeviceSize}} $(docker ps -lq)
```

#### Find out memory and CPU utilization of docker

```
for i in $(docker ps -q); do  echo $i; docker exec $i ps -eo pid,cmd,%mem,%cpu --sort=-%cpu | head -5;echo ======; done
```

#### Find out disk space of containers

```
for i in $(docker ps -a -q); do echo $i;docker exec -u 0 $i bash -c 'du -sch / 2> /dev/null'; echo "======";done
```

#### Find out network of container

```
docker inspect containerid -f "{{json .NetworkSettings.Networks }}"
```

#### Login as root:

```
docker exec -it -u 0 containerid bash
```

#### Check status of container

```
Docker inspect -f  '{{.State.Running}}' container_name
```

#### Check docker thinpool size

```
sudo /usr/sbin/lvs -o lv_name,data_percent,metadatapercent|awk '$1 == "docker-pool" {print $2,$3}'
```

### Clean-up Docker Space

#### Display all exited containers

```
docker ps  -f status=exited
```

#### Remove all exited containers

```
docker rm $( docker ps  -q -f status=exited )
```

#### Remove all dangling images

```
docker rmi $(docker images --quiet --filter "dangling=true")
```

#### Remove all dangling valumes

```
docker volume rm $( docker volume ls -q -f dangling=true)
```

### Reference and more details

https://www.docker.com/ 

Management Commands:
```
  builder     Manage builds
  config      Manage Docker configs
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes
  
  ```

Commands:
```
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  deploy      Deploy a new stack or update an existing stack
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes
  
  ```
