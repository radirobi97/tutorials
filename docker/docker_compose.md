# Docker compose
Docker compose works on a single machine, does not work on a computer cluster. **Docker Swarm** is capable to cooperate with multiple computers.

Containers used in docker-compose can talk to eachother using NAME of the proper service.

#### Commands
- `docker-compose build`: builds the docker-compose.yml
- `docker-compose up`: starts the compose. If there is a difference between the current and the desire state (state what is defined in Dockerfile), it will apply the changes and rebuild the modifed parts.
- `docker-compose up --build`: it rebuilds images inside docker-compose.yml
- `docker network ls`: lists out networks used in docker compose
- `docker-compose ps`: lists out containers
- `docker-compose down`: stop all containers inside docker-compose
- `docker-compose start`: start services
- `docker-compose stop`: stop services
- `docker-compose restart`: restart services

#### Structore of a docker-compose.yml file
```yml
version: '3' #version of compose

services:    
  app:                                #name of the service
    build: .                          #we want to use our Dockerfile to build an image, this is the folder where our files located
    image: takacsmark/flask-redis:1.0 #this will be the name of the image
    restart: alwas                    #restart policies can be defines, it determines when to restart a given container
    environment:                      #setting environtment variables
      - FLASK_ENV=development
    ports:
      - 5000:5000
    networks:
      - mynet

  redis:
    image: redis:4.0.11-alpine        #here we are using an existing image
    networks:                         #network used by the service
      - mynet
    volumes:
      - mydata:/data
    command: ["bash"]                 #it makes possible to overwrite the default command of the image

networks:                             #here we define our network
  mynet:

volumes:
  mydata:
```

If our dockerfile has a custom name we should use the following:
```yml
build:
  context: .                             #where our files located
  dockerfile: Dockerfile.dev             #custom name of our Dockerfile
```

#### Variables
It is possible to define variable in a **.env** file. These variables can be used then in the **docker-compose.yml** file. We can refer to them as the following:
- `${[NAME_OF_VARIABLE]}`
