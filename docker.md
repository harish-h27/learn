
### why we use docker ?
docker makes it really easy to install & run software without worrying about setup or dependencies

### tools under docker ecosystem
    1. Docker Client
    2. Docker Server
    3. Docker Machine
    4. Docker images
    5. Docker Hub
    6. Docker Compose


Docker Client and Docker Server connected to each other, docker server is going to create and manages the container, but we are not going 
to directly communicate with Docker server , we communicate with Docker Client which is going to internally communicate with Dokcer Server


### docker image is a blue print for docker container
when we take a image & turn it into a container using docker run command what happens ?
ans: the kernel is going to isolate a little section of the hard drive & make it available only for this container

namespacing in operating system and docker containers works in same manner


### docker containers contains a startup command


### how is docker running on you computer ?
when you installed a docker on your computer, you installed a linux virtual machine
all the containers are going to be created inside that linux virtual machine

### you can assume every docker conatiner has operating system of its own, it has separte disk portion it file system contains contents of OS


### every container will be isolated , they are not connected to each other


```{r, engine='zsh', count_lines}
docker run hello-world <download hello-world image if it is not exist in local and create a container out of it >
```


```{r, engine='zsh', count_lines}
docker run <image_name> <command>
ex: docker run busybox ls
ex: docker run busybox echo "hello world"
```






```{r, engine='zsh', count_lines}
docker ps <list all running containers> 
flags:
    -a, --all             Show all containers (default shows just running)
    -q, --quiet           Only display container IDs
    -s, --size            Display total file sizes
```

```{r, engine='zsh', count_lines}
docker create busybox (download the image)
```


```{r, engine='zsh', count_lines}
docker start busybox(start a container from the downloaded image)
```


```{r, engine='zsh', count_lines}
docker run = docker create + docker start
```



```{r, engine='zsh', count_lines}
docker system prune (remove all the containers)
```



```{r, engine='zsh', count_lines}
docker logs <container id or name> (display the logs of the container)
flags: 
    -f, --follow         Follow log output
```

```{r, engine='zsh', count_lines}
docker stop <container id or name>
```


```{r, engine='zsh', count_lines}
docker exec -it <container id or name> <command>
ex: docker exec -it orderer1.example.com bash

instead of bash we can also use (zsh, powershell, sh)

the purpose of the -it flag
    -i -> attach my terminal to redis-cli container & a input device
    -t -> show all the input & output text prettier

```


Dockerfile is a configuration to define how our container should behave
Dockerfile -> Docker Client -> Docker Server -> Usable Image


 ### stpes to create a Dockerfile

 1. specify a base image

 2. run some commands to install additional programs 

 3. specify a command to run on container startup


 ### Dockerfile  example

 
```yaml
    # use an existing docker image as a base
    FROM alpine
    # download and install a dependency
    RUN apk add --update redis
    RUN apk add --update gcc
    # tell the image what to do when it starts as a container
    CMD ["redis-server"]
```

apk add --update redis -> apk(apache package manager it is a program that comes pre installed on the alpine image)


```{r, engine='zsh', count_lines}
docker build ./
```

```{r, engine='zsh', count_lines}
docker build -f Dockerfile.dev
```

### tagging an image (giving a name to docker image(your custom image) instead of id)

```{r, engine='zsh', count_lines}
docker build -t <docker_user_id>:/<custom_image_name>:<version> <current_folder_path>
ex: docker build -t harish2724/redis:latest . # this command only builds the image to run it use below command

docker run harish2724/redis
```


## docker file for nodejs

Dockerfile
```yaml
    FROM node:alpine
    WORKDIR /usr/app (where all the below commands will be executed)
    COPY ./(local machine) ./(insidecontainer)
    RUN npm install
    CMD ["npm", "start"]
```

## port mapping from contanier to local machine
```{r, engine='zsh', count_lines}
docker run -p 8080:3000 <container_id>
```
when any one makes request to port 8080 in local machine it will be forwarded into port 3000 inside the container



## docker volume
-> docker volume is kind like a port mapping
-> we will be not making copy of our files to the container, we will just referencing docker container to local folder


```{r, engine='zsh', count_lines}
docker run -p 3000:3000 -v (local_folder or file) <image_id>
ex:  docker run -p 3000:3000 -v $(pwd):/app <image_id>
```

## Docker compose 

-> it is a separte CLI that get installed along with docker
-> it is used to startup multiple docker containers at the same time 
-> it automates some of long-winded arguments we were passing to 'docker run'


all the services(containers) in docker-compose.yaml file will be created under the same network & they are going to have free access to communicate to each other in any way that they want


# starting docker-compose.yaml file
```{r, engine='zsh', count_lines}
docker-compose up 
flag:
    -d -> starting containers in the background


docker-compose down

```


## example docker-compose.yaml

```yaml
version: "3"
services:
    redis-server:
        image: 'redis'
    node-app:
        restart: always (when this container fails it will always restarts, other falgs are no(dont restart), on-failue(only restart if the contanier failes with the error code, unless-stopped(always restart unless developer stops it forcibly)))
        build: ./
        ports: 
            - "4001:8081"
        volumes:
            - /app/node_modules
            - ./node_modules:/app/node_modules

    react-app:
        build: 
            dockerfile: Dockerfile.dev (name of the file we are using to build)
            context: ./server (folder path of the Dockerfile.dev)
```








## internal referencing in Dockerfile 
```yaml
    FROM node:alpine as builder(it is like alias)
    WORKDIR '/app'
    COPY package.json
    RUN npm install
    COPY . .
    RUN npm run build

    FROM nginx
    COPY --from=builder /app/build /usr/share/nginx/html
    (copy everything from /app/build folder of builder(image) to /usr/share/nginx/html folder of nginx image)
```


## best way to declare inside docker-compose file

```yaml
  peer0.org1.example.com:
    container_name: peer0.org1.example.com
    image: hyperledger/fabric-peer:latest
    environment:
      - FABRIC_LOGGING_SPEC=INFO
      - CORE_PEER_TLS_ENABLED=true
    volumes:
        - ../organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/msp:/etc/hyperledger/fabric/msp
    working_dir: /opt/gopath/src/github.com/hyperledger/fabric/peer
    command: peer node start
    ports:
      - 7051:7051
    networks:
      - test
```

