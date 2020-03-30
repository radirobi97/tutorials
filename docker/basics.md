# Docker basics

#### Architecture
![architecture](./images/docker_one.PNG)
<br/>
<br/>
All docker containers use the resources of the host computer.
<br/>
![architecture](./images/docker_two.PNG)

#### Commands to run/start/stop
- `docker login`: login to DockerHUB */dockerhub is the default but it can be changed/*
- `docker images`: lists out images from your local filesystem
- `docker run [image]`: creates a container from the image and starts the container
- `docker run --name [name_to_container] [image]`: creates a container from the image and start the container with the given name
- `docker start [name|id]`: it starts a currently not running container
- `docker stop [name|id]`: it stops the container
- `docker ps`: lists out running containers
- `docker ps -a`: lists out running and also not running containers
- `docker rm [name|id]`: remove the container
- `docker run -p 8080:80 [image]`: if docker container use a given port we should expose that, in this example we expose dockers port 80 to our server port 8080
- `docker run -d [image]`: creates a container from the image and starts the container in the background
- `docker exec [name_of_container] [command]`: runs a command in a running container
- `docker commit [name_of_container]`: creates an image from a container **not recommended**

#### Builds related things
- `docker build -t [this_will_be_the_name_of_image] [build_context]`: builds our image
- `docker build -f [custom_dockerfile_name]  [build_context]`: builds an image from a dockerfile with custom name

### Dangling images
If we build an image with the same name, the older one losts his tag. Its called a **dangling** image.<br/>
To list out dangling images and delete:
- `docker images --filter "dangling=True"`
- `docker rmi $(docker images -q --filter "dangling=True")`

### Volumes
Is it possible to attach local files, folders, etc to a docker container. This is what volumes for. It useful because any changes made in a volume will be propagated into the container.

`docker run -p 8080:80 -v /home/users/src:/var/www/html`:<br/> it mounts a local folder to the container.

If we want some folder inside the container **/var/www/html/** NOT to be mounted we can use the `-v` without semicolon. <br/>
`docker run -p 8080:80 -v /var/www/html/not_to_be_mounted -v /home/users/src:/var/www/html`: it mounts a local folder to the container

### Multi stage Builds
Is it possible to use more than one base images in one dockerfile. For example, if we use outpout of the first stage as input to the second. See below.
```yml
FROM node:alpine as builder #we identify this phase with the AS keyword
......

FROM nginx                  #this is the second stage
COPY --from=builder /app/build /usr/share/nginx/html
```
