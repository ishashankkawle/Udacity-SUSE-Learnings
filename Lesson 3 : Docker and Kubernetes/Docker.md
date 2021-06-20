# Docker
## Why Docker?
Docker create container images which are lightweight and have no dependency of host OS. Images contains the code of application and all of its dependencies. These images are OCI (Open Container Initiative) compliant images. OCI helps to standardize image formats and ensures that images can execute on OCI complient runtimes. 

## Architectural difference from traditional approach

 - ### Traditional Approach ( Applications running on Guest OS )
In traditional approach Applications run on Guest operation system. This guest operating systems cause lot of overhead over host operating system causing performance degradation.

![VM based architecture](https://github.com/ishashankkawle/Udacity-SUSE-Learnings/blob/9f712118e6b9f472d73a30fb5e7e5d4b11cfa1ba/Lesson%203%20:%20Docker%20and%20Kubernetes/Images/VM%20based%20architecture.png "VM based architecture")
 
 - ### Docker Approach
  When we use docker deployment technique, we install Docker Daemon on the top of Host OS. Docker daemon is completely different from Guest OS and provides runtime environment for containers. Hence the entire Guest OS layer get removed from architecture.

![Docker based architecture](https://github.com/ishashankkawle/Udacity-SUSE-Learnings/blob/9f712118e6b9f472d73a30fb5e7e5d4b11cfa1ba/Lesson%203%20:%20Docker%20and%20Kubernetes/Images/Docker%20based%20architecture.png "Docker based architecture")

## Creating Dockerfile
Docker creates image of an application using Dockerfile. This file contains steps to packer an application and all its dependencies.
Some common keywords in docker file:
|Keyword|Usage  |
|--|--|
|FROM|to set the base image <br> ```FROM nginx:alpine```|
|RUN|to execute a command <br> ```RUN echo "Hello world"``` |
|COPY|to copy folders|
|WORKDIR|to set workspace directory|
|EXPOSE|to Expose single or multiple ports <br> ``` EXPOSE 3000``` <br> ``` EXPOSE 80 443``` <br> ```EXPOSE 7000-8000```|
|CMD|defines default command to run when container is launched <br> ```CMD ["npm","start"]```

####   Sample Dockerfile structure:
```dockerfile
FROM python:3.8
LABEL maintainer="Shashank Kawle"
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
CMD [ "python", "app.py" ]
```

### Optimizing Builds with ```ONBUILD``` & Ignoring files with .dockerignore:
**ONBUILD** allow us to build image but application specific commands with prefix 'ONBUILD'  won't be executed until built image is used as base image. 
Example of optimized Dockerfile
```dockerfile
FROM node:7
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
ONBUILD COPY package.json /usr/src/app
ONBUILD RUN npm install
ONBUILD COPY . /usr/src/app
CMD ["npm" , "start"]
```
**.dockerignore** file is same as **.gitignore** file. We can ass files in .dockerignore which we want to exclude from build
  
## Basic Docker commands
General command structure: 
1. ```docker <commands> <options>```
2. ```docker <service-name> <command> <options>```

- To Create Docker Image: **docker build**
Syntax: ```docker build -t <image name> <path to docker file>```
	- -t  : Specify unique name to created image.  
> To keep build time minimum, Docker caches result of executing line in Dockerfile for use in future builds. If something has changed, Docker will invalidate current cache. If you don't want to use cache as a part of build, set  option ```-no-cache=true``` as a part of docker build command. 
  
 - To Run Docker Image: **docker run**
Syntax: ```docker run -d -p 443:80 <image name>```
	- -d  : Once created, run container in background
	-  -p 80:443  : Expose port 443 & bind it to port 80
	-  -e : Set value of environment variables at runtime 
	Eg. To launch container by setting NODE_ENV environment variable  to 'Production' 
 ``` docker run -d -p 2030:2030 -e NODE_ENV=Production my_image```
 
 > docker run command can also be used to execute any command inside the container as follows:
 > ```docker run my_image ls /app```
 
 - To tag Docker Image: **docker tag**
Syntax: ```docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]```

-	To list all available images on machine: **docker images**
-	To list all RUNNING containers: **docker ps**
> To format docker ps output use ```--format```. This allows pretty printing using GO language syntax.
> Example. ```docker ps --format '{{.Name}} container is using {{.Image}}'```
-	To get the logs of particular container: **docker logs**
Syntax: ```docker logs <container id>```
> You can get container id of container using ```docker ps``` command

- To login into DockerHub: **docker login**
> This in an interactive command. You must have docker hub account in order to use this command
- To pull image from DockerHub: **docker pull**
Syntax: ```docker pull NAME[:TAG]```
- To push image in DockerHub: **docker push**
Syntax: ```docker push NAME[:TAG]```
> You must be logged in  into dockerhub before using this command. Use ```docker login``` command to login into dockerhub.

## Advanced Docker Topics

### 1. Data Containers:

Data containers are the containers whose sole purpose is to be a place to store/manage data. Like other containers they are managed by host system. However dont run when you perform ```docker ps``` command.

#### To create data Container with base image busybox:
while  creating container provide ```-v``` option to define directory workspace where other containers will  read / save data.
 ``` docker create  -v /config --name=DataContainer busybox```
 or
 ```docker run -v /config --name=DataContainer" busybox```

#### To copy files in data container:
Use **docker  cp** command.
```docker cp temp.txt DataContainer:/config/``
This will copy temp.txt into config directory

#### To mount this container to other containers:
Use ```--volumes-from``` option while creating container instance 
``` docker run --volumes-from DataContainer  -d my_image```

#### To Export data containers:
``` docker export DataContainer > DataContainer.tar```

#### To Import data containers:
```docker import DataContainer.tar```

### 2. Docker Networks:
Docker networks helps containers to interconnect with each other. We can add /remove containers from network. 
#### To create Network:
with the name 'backend-network'
```docker network create backend-network```

#### To connect container with Network:
Use ```--net``` option. Here we are going to create two containers named 'ONE_REDIS' and 'TWO_ALPINE' from base images 'redis' & 'alpine'.
```
docker run -d --name=ONE_REDIS --net=backend-network redis
docker run -d --name=TWO_ALPINE --net=backend-network alpine
```

While using Docker Network, containers communicate via Embedded DNS server in docker. This DNS Server is assigned to all containers via ip: ```127.0.0.11``` & set in **resolve.conf** file  (/etc/resolve.conf).
When  container attempts to  access other container, DNS Server will return ip of target container. 

#### To perform Network Communication between two containers:
Ping from TWO_ALPINE to ONE_REDIS
```docker run --net=backend-network TWO_ALPINE ping ONE_REDIS```

#### To attach existing container to network:
Let's say we have already created container with name MY_CONTAINER before creating network. Then to attach this container to backend-network
```docker network connect backend-network MY_CONTAINER```

#### To list all networks: ```docker network ls```
#### To inspect containers attached to network: ```docker network inspect backend-network```
#### To disconnect container from network:
To disconnect MY_CONTAINER from backend-network:
```docker network disconnect backend-network MY_CONTAINER```

### 3.  AUTO-RESTART  Containers:
Docker can automatically retry to launch container specific number of times before it stops trying. 
The option [``` --restart=on-failure:#```] allows you to specify how many times docker should try again.
```docker run -d --name MY_CONTAINER --restart=on-failure:3 redis```
To ignore count limit and specify always restart, use [```--restart=always```]
``` docker run -d --name MY_CONTAINER --restart=always redis```

### 4. Docker Logs:
As we have seen in Basic commands we can access container outputs using **docker logs** command.
By default docker logs are outputted using *json-filelogger* meaning output is stored in JSON file on host. This can result into large files filling the disk. You can change log driver to move to different formats and destinations.
To change log driver use: ```--log-driver``` option.
```docker run -d --name=MY_CONTAINER --log-driver=syslog my_image```

#### To disable logging on containers:
Use [```--log-driver=none```] when container is launched.
```docker run -d --name=MY_CONTAINER --log-driver=none my_image```

### 5. Docker Labels:
Labels helps for operations like filtering the output of commands based on labels.

#### To add single label to container:
Use ```-l``` option
```docker run -d -l user=Shashank redis```
Where **user** is the label name and **Shashank** us label value.

We can assign multiple labels to container through file. To assign labels through file, use ```--label-file``` option.
##### Sample Label file :
```
user=Shashank
role=admin
```

command to be executed:
```docker run -d --label-file=labels.txt redis```

#### Labeling through Dockerfile
Use  ```LABEL```  keyword to add label in docker file
##### To add single label:
```dockerfile
LABEL user=Shashank
```
##### To add multiple labels:
Use ' **\\** '  between the labels.
```dockerfile
LABEL user=Shashank \ course=docker
```
 
##### To filter on Label:
To filter containers based on label in **docker ps** command
```docker ps --filter "label=user=Shashank"```
To filter Images
``` docker images --filter "label=provider=udacity"```

### 5. Real-Time Metrics:
To check real-time metrics of container, use **docker stats** command
##### For single container:
```docker stats MY_CONTAINER```

##### For multiple containers:
```docker ps -q | xargs docker stats```
where **|** passes output of left command to right command
and **xargs** treats incoming value as a parameter to command.
