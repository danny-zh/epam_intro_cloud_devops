
# Docker Bootcamp

## 1. Theory

### 1.1 Main Theory part

Docker helps facilitate application development, testing and deployment. They can be run separately in containers without configuring the environment and scale and manage them via the containers orchestration system like kubernetes.

### Architecture Overview

Docker is based on a client-server architecture:

1. Client Docker CLI: Command line that provides interface for users to interact with docker daemon. The docker client communicates with the docker daemon usgin REST API via a unix socket or a network interface.
2. Docker Daemon: Service that listents to API calls from docker client and executes them. Docker daemon main functions are:
  - Listening to API calls
  - Managing docker objects such as images, containers, networks and volumes.
  - Comunicatig with other daemons such as containerd to manager docker services.
4. Docker Registry: Storage space that stores images from which containers are built. Docker is configure by default to search for images from the docker registry hub

### Docker workflow

1. Developer uses docker to build and push an image to the docker registry
2. Docker daemon builds the image
3. Docker daemon pushes the image to the docker registry
4. Developer runs a container from the built image
5. Docker pulls the image from the registry

### Docker components

#### Docker Infrastructure

1. Docker Daemon: provides details for container execution to daemon Containerd
2. Containerd: Manages the low level container tasks such as storage, image distribution, network etc. Communicates with service Shimc.
3. Shimc: Runs a process called Runc to start the container
4. Runc: It is a ligthweigth library for running containers. Starts the container and then goes away. 
5. Shimc: After starting, shimc manages the running container
6. Docker-proxy: Responsable for proxing container ports to host interface.

#### Docker Container

A container is something like a virtual machine. It can be run as a self-contained environment. A container has:

- Filesystem
- Any software installed on top on that filesystem
- Additional libraries, binaries and runtimes to run an application

A continer is a runnable instance of a image, docker container has all the necessary elements for an application to run, except the kernel. Docker provides the containers the use of the host kernel.

Since the main idea of containers is to create an isolated space to run applications, docker uses various technologies to provide isolation from the host and also from the application themselves. Some key isolation technologies docker uses are:

1. Namespaces: A process in the container should not see other processes running on the system. It should feel as if it were the only process.
2. Control Groups: A facility needed to manage system resources such as for example the amount of CPU, RAM container needs
3. Chroot: A process in the container should see only that part in the filesystem required by it.
5. Process Capabilities: A process should have enough permission to manage/use kernel
6. Virtual eth: A process in the container should have access to an ethernet device
7. Port Binding: A few containers can expose same ports and it should not have any issues.
8. Volumes: A service should have the ability to keep data, even if it goes down.
9. Docker Network: A service should be able to communicate with other services by IP/names

The most used isolation technologies are Namespaces, Control Groups and Chroot.

1. Namespaces: Docker provides a set of namespaces for each new container, each aspect of the container runs in its own namespace and it does not have access outside. Docker engines provides 6 namespaces:
   - MNT: Manages mount points, filesystems.
   - UTS: Isolating kernel and versions identifiers (Unix Timesharing System)
   - IPC: Access to manage IPC resources (Inter Process Communication)
   - PID: Process isolation
   - NET: Managing network interfaces
   - USER: User isolation

    An example of a command that allows a container to be run within the same namespace of another container is the following
    
    - `$ docker run --network running_container image` Runs new container in the network namespace of running_container
    - `$ docker run --pid running_container image` Runs new container in the pid namespace of running_container


2. Control groups: Allows docker to share avaiable hardware resources to the containers and if needed set up limits and contraints.
3. Chroot: Presents the idea that the container sees only a piece of a filesystem

#### Docker Image

A docker image is a inert, inmmutable file essentially representing the snapshot of a container

- Images are built using the `docker build` command that runs the script loaded in the DockerFile.
- Images can be stored in the docker registry and also in other well registries such as AWS, Azure and so on.
- Images are composed of layers of other images, this feature allows minimal network transfer of images. If some layers already exist on your host, Docker will not download them. Similarly when you push an image to a registry, only the layers that are not already present in the registry will be added. 

#### Docker Storage

Docker Storage drivers are responsible for managing the storage of docker containers and images on the local host. Different storage drivers offer different performance characteristics, features and compatibility with underlying storage technologies.

There are two approaches for storage drivers:

1. Union Filesystem (UnionFS): Allows files and directories of different layers to be transparently overlaid creating a single virtual filesystem. Files in the same path will be seen together in the virtual FS
2. Copy on-Write (CoW): Shares and copies files from layers to achieve maximun efficiency. In this approach all image layers become read only, to make modification to existing files, a docker container runs a separate thin layer for r/w purpose, this allows to update and create new files in the container. If a file or directory exist in a lower layer of the image and another layer needs to read access to it, the layer just simpliy reads the exsisting file in the containing layer. This is the sharing part. On the other hand if the writable layer needs to update a file existing in other layers, it just simply copies the file to its own layer and make the modifications. This is the copying part. Figure 1 shows an example of docker layers

The big distavantage of this approach is that if you need to modify large files, those files will be copied in the writable layer, then if you create a new image from the running container the new image size will increase. Thus, the best implementation of docker is to use a ligthweigth application with an image size of 100MB

<p align="center" dir="auto">
<img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/5c9fdd78-0f36-4b44-950a-5e20ef83a868" alt="Figure 1" width="600" height="300" style="max-width: 100%;"></a>
  <br>
  <em>Figure 1. Docker image layers example </em>
</p>

Regarding the storage drivers also referred to as graph drivers derived from this approaches we can find:

1. VFS (Virtual File System):
  - Does not support UnionFS or CoW
  - Not recommended for production due to poor performance
  - Valid for testing of the docker engine
2. Overlay2:
  - Based on UnionFS
  - The preferred storage driver for all linux distros
3. AUFS (Another Union File System):
  - Is an older storage driver that provides similar functionality to OverlayFS 
5. Devicemapper:
  - Device mapper works on block devices.
  - Supported on redhat
6. BTRFS
  - Based on CoW
  - Not supported by redhat
  - Requires BTRFS file system to be installed in the underlying storage device.
  - It offers advanced features like snapshots, copy-on-write, and checksums, making it suitable for certain use cases.


Devicemapper and BTRFS is what you should look for when running container in production environment. The difference between them is that Device Mapper works perfectly when you have prepared an underlying filesystem of the device. Btrfs requires the btrfs filesystem on the piece of your filesystem, where Docker will keep all files


### 1.2 Working with Docker. Docker Hub and Registry

 #### Docker Services

The docker daemon (server side) listens on unix socket /var/run/docker.sock for docker client API calls. The process is as follows:

1. The docker client issues the instruction to run a container by using command `$ docker run image_id|name`
2. Docker daemon
 - Pulls the image from registry if it does not exist locally
 - Creates new container
 - Allocates filesystem and mount the read/write layer for the container
 - Allocates bridge network interface
 - Allocated IP address
 - Executes process you indicated and captures the output.
3. After that you can manage your container with `$ docker exec -it container_id|name /bin/bash`

#### Docker commands

The following are some command for stating with docker

1. Pull an image
- `$ docker pull registry.example.com/myimage:tag` Pull an image from a registry to localhost. By default docker uses the docker registry to look for the requested image, if another repository is required you must spefified its location. You can request to pull an specific version of the image by using the tag, if no tag is given it will pull the latest version of the image
2. Push an image
- `$ docker push registry.example.com/myimage:tag` Push an image from a registry to localhost. By default docker uses the docker registry to look for the requested image, if another repository is required you must spefified its location. You can request to push an specific version of the image by using the tag, if no tag is given it will push the image as the latest version
3. Run container from image
- `$ docker run --name my_container -dp 192.168.1.1:80:80 image:tag echo "Hi"` The option --name means the name that will have the container, the option -d means detach mode and container will run the in the background, the optionn -p means over which port of localhost to publish the container's port. The last option is the command that the container will run once is ready.
4. Connect to a docker container:
- `$ docker exec -it container_id|name /bin/bash` The option -i mean interactive, the option -t means to provide a TTY interface to issue commands
5. List container and images
- `$ docker image ls` List installed images on localhost
- `$ docker search registry_name` Lista stored image from repo registry
- `$ docker container ls --all or docker ps --all` List all containers
6. Stop/remove container, remove image
- `$ docker stop container_id|name` Stops container using SIGTERM signal to gracefully terminate it.
- `$ docker rm container_id|name` Removes container
- `$ docker rm -f container_id|name` Stops and removes container
- `$ docker rmi image_id|name` Removes image
- `$ docker run -it --rm image_id /bin/sh` Runs container and removes it once has completed execution of task
7. Inspect container's information
- `$ docker inspect - "{{json .}} container_id|name` The format of the ouput can be changed according to specific needs. For example json. More in https://docs.docker.com/reference/cli/docker/inspect/
8. Launh container in the same namespace as other running container (advanced options)
- `$ docker run --network=container:existing_container_name_or_id your_image_name` Launch new container in the same network namespace as running container
- `$ docker run --pid=container:existing_container_name_or_id your_image_name` Launch new container in the same pid namespace as running container
9. See docker info:
- `$ docker info -f "{{json .}}"` Provides a lot of important information like how many containers in the system now, information about running, paused and stopped containers, images and server configuration and storage driver being used for running containers
10. Run a image from a modified container
- `$ docker commit contaner_id|name new_image_name` Commits the container changed status to a new image. This is rarely used but worth knowing
- `$ docker run new_image_name` Runs new container from new image derived from old container
11. Login to a remote repository to search, pull and push images
- `$ docker login remote.repository.com` Docker will ask for username and password
12. Search for an image in the defined repo:
- `$ docker search remote.repository.com/image:tag`
  
#### Docker Container Daemon Configuration

The docker daemon can be run with customized configuration. Such case is useful for example for debbuging. Dockerd can be configured either by json file or by flag parameters. The configuration json file is located under **etc/docker/daemon.json**. The following is an example that configures dockerd in debug mode, uses TLS, and listens for the traffic routed to 192.168.59.3 on port 2376

```
  {
        "debug": true,
        "tls": true,
        "tlscert": "/var/docker/server.pem",
        "tlskey": "/var/docker/serverkey.pem",
        "hosts": ["tcp://192.168.59.3:2376"]
  }
```

#### Creating Docker Images

In docker containers rely on images. An image is a combination of filesystems and parameters. Following are some commands to work with images:

1. List local docker images
- `$ docker images`
2. Create an image from a tarball
- `$ docker import image.tar imagename:tag`
3. Create an image from dockerfile
- `$ docker build -t mynginximage:tag .` The option -t means the name and tag for the image. The dot means that all content of this folder will be provided to the Docker daemon
4. Creates an image from a running container
- `$ docker commit contaner_id|name new_image_name` Commits the container changed status to a new image. This is rarely used but worth knowing
5. Create an image from stdin tarball
- `$ docker load -i image.tar`
6. Save an image or image to a tar file
- `$ docker save -o my_images.tar nginx:latest alpine:3.14` The option -o means the name of the output archive
7. Shows the history of an image
- `$ docker history busybox-`
8. Tag an image
-`$ docker tag nginx:latest myregistry.example.com/mynginx:1.0` This is normmally used to tag an image with a different name to be sent to another repository, the images when pulled will have in its tag the name of the repository from which was obtained.
9. Add a label to a container
- `$ docker run --label com.example.version=1.0 myimage` Labels are methadaa information that can be added to images, containers, volumes, networks, services etc that are particularly useful for documentation, resource organization and management and integration with other tools.
10. Remove all stopped containers:
- `$ docker container prune`

#### Create an image from dockerfile

An image is a set of layers. Figure 2 shows the image Ansible 2.6.2 was derived from image Ansible 2.6.1 and this in turn was derived from sbelakou/centos:latest image. Each new layer just keeps the delta, not the whole layers information. Management of base images is an important thing, and there are a lot of tricks about it. Frequently, the best way is to build an image from scratch or from a simple base image.

<p align="center" dir="auto">
<img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/c8ca29e9-8549-4b1e-a9d8-c1d04ddfd84c" alt="Figure 1" width="600" height="300" style="max-width: 100%;"></a>
  <br>
  <em>Figure 2. Docker image layers </em>
</p>

DockerFile is an script to provide instructions for an image construction. The following is an example of a dockerfile

```
    FROM python:3.8 
    WORKDIR /app 
    RUN pip install flask 
    ADD app.py /app/app.py 
    EXPOSE 80 
    CMD ["python","app.py"] 
```
From the previous example:

1. **FROM** directive defines the base image and version from docker hub. Customized images from other registries can also be used.
2. **WORKDIR** directive defines the working directory inside the container, in this case it will bee "/app". Subsequent instructions will be executed relative to this directory.
3. **RUN** directive runs commands specially for initializing or installing needed tools, binaries, frameworks and so on.
4. **ADD** directive copies files from localhost to working directory inside the container
5. **EXPOSE** directive is used by convention to document the port that will be used by the container to listen for calls. This actually does not publish the port to the host machine.
6. **CMD** directive specifies the optional default command to run when the Docker container starts if no command is given.

### 1.3 Working with Docker, Dockerfile and Running Containers

 
#### Working with docker file

Dockerfiles are scripts that contains commands to create a docker image. Another common commands are:

- **ENV** Sets environment variable for the container
- **ENTRYPOINT** Default command that always will be run when the containers are started. Basically configures a container that will run as an executable.
- **VOLUME** Creates a mount point for external mounted volumes
- **USER** Sets which user in the container to run the commands: RUN, ENTRYPOINT, CMD
- **ARG** Allows to define input arguments for the dockerfile when building the image
- **LABEL** applies key/value metadata to your images, containers or daemons.
- **HEALTHCHECK** Dockers applies the defined heatcheck to verify if the container service is running.

**ENTRYPOINT** and **CMD** commands are special because when **ENTRYPOINT** directive is used, it will be the default command that always be executed in the container when run. This is particullarly useful when using the container for a specified purpose such as for example a web server. The entrypoint directive will call the command, script or service to execute the task. The  **CMD** in this case will provide optional default arguments for the command defined in the entrypoint, if no command is passed to the docker when run.

**RUN** command will execute the given commands in a new layer on top of the current image and then commits the results. The resulting commited image will be used for the next directive in the dockerfile. 

#### The .dockerignore file

This file is located in the root of the directory from where the image will be builded. It helps to exclude what files and directories will be excluded from the building of the image.

#### Docker base images

When a docker image is created from a dockerfile, you can find the docker parent image there. A base image means a image with no parent image in the related dockerfile.

A docker base image can be built from:

- Scratch: Ultimate base image with 0 files and 0 size
- Busybox: Minimal Unix weighing in at 2.5 MB and around 10.000 files
- Alpine:latest: Alpine Linux, only 8 MB in size and has access to a package repository.
- Debian:jessie: Latest Debian is 122 MB and around 18.000 files.

An example of building an image from the scratch base image is as follows:

```
  FROM scratch 
  
  ADD centos.tar.gz / #This .tgz contains minimal CentOS OS
  
  RUN yum install -y epel-release && \
  yum  update -d && \
  yum clean all

  HEALTCHECK --interval 10s \ 
  --retries=5 --timeout=5s \
  CMD (ping -c 1 localhost && curl -f http://localhost ) || exit 1
  
  LABEL architecture="amd64"\
  OS="CentOS"

  CMD ["/bin/bash"]
```

#### Building an Image with Arguments and Multistage Build


1. Arguments: Dockerfile is considered to be static but you can add arguments which are key value pairs that act as variables, these arguments can alter the builing of the image. An example of a dockerfile using build arguments is shown below:

    The dockerfile:
    ``` 
    ARG BASE_IMAGE

    ARG user=jenkins #Assigned with default value if no one is given

    FROM ${BASE_IMAGE} 
    ```

    The command to build the image needs to specify the argument, if not specified the default assigned value in the dockerfile will be used.

    `$ docker build --build-arg BASE_IMAGE=ubuntu:16.04 .`

2. Multistage build: Allows you to define multiple build stages within a single Dockerfile. Each build stage represents a distinct phase of the build process. This is particularly useful to separate the processes for building and running, you do not have to create two Dockerfiles. Just create one with multiple stages. Below is an example how to create multistage builds:

    The dockerfile:
    ```
    # Stage 1: Build stage
    FROM node:14 AS build-stage
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    RUN npm run build

    # Stage 2: Production stage
    FROM nginx:alpine AS production-stage
    COPY --from=build-stage /app/build /usr/share/nginx/html
    ```
    Using multistage build can lead to more efficient and maintainable Dockerfiles, as it allows you to separate concerns and optimize each stage for its specific purpose. It also helps reduce the size of the final Docker image by discarding unnecessary build dependencies and intermediate files introduced during the build process.

    The command to build the image is    
    - `$ docker build -f dockerfile .` Builds image from all stages
    - `$ docker build --target build-stage -f dockerfile .` Builds image only from target stage


#### Running containers

When running a container from an image, different parameters can be pass into to the docker run command

- -d: Runs the container in detach mode (background) and prints the container's id
- -P: Publish exposed ports to random host ports
- -p: Publish exposed port to host specific ports
- -e: Use to set env variables for the container
- -v: Use to bind mount a volume
- -w: Use to set working directory inside the container
- --label: Use to add label to the container
- --rm: use it to automatically terminate the container whe it exits
- --entrypoint: Used to override the default entrypoint defined in the image
- --restart: Defines the restart policy of the container. If the containers exits or fails during execution you can use restart policy for docker to launch the container again. The restart policy specifies when to restart the container and optionally the maximum number of times it will do so.
    
    - `docker run -d image --restart=always|unless-stopped|on-failure[:times]`

### 1.4 Data Management in Docker

Data generated inside the container is isolated from host system and cannot be accessed directly. When the containers are terminated this data is lost. To 

1. Bind mounts: Provides a mapping of a file or directory between the host and the container. Processes in the host can direclty modify these files and directories, however docker cannot manage these bind mounts. There are two ways to create a bind mount being the second the preferred since it's most explicit.
    - `$ docker run -v /host_path/:/container_path my_image` The option -v maps the host_path to the container_path
    - `$ docker run --mount type=bind,source=/home/nginxlogs,targe=/var/log/nginx my_image` The option --mount specifies the directoy nginxlogs from the host will be mapped to that of /var/log/nginx from the container

    When using either -v or --mount options you can specify a set of comma separated key-value pairs to achieve further configurations for the bind mount, such as for example establishing the bind mount as read-only, this won't allow the container to write content in the bind mount.

    Bind mount can result benefical in cases where data needs to be shared between host machine and the container.

2. Volumes: These are docker objets that can store persistent data, the volumes are attached to the containers. Volumes can be shared between containers and are the preferred mechanism for saving data generated by and used by Docker containers. Docker manages the lifecycle of these volumes and are located at **/var/lib/docker/volumes**. Host processes should not modify these files direclty.
    - `$ docker volume create my_volume` Creates a volume
    - `$ docker volume ls` Lists all volumes
    - `$ docker volume inspect my_volume` Inspects a volume
    - `$ docker volume rm my_volume` Removes a volume
    - `$ docker volume prune` Removes all unused volumes
    - `$ docker run -v my_volume:/container_path my_image` The option -v attaches the docker volume to the container.


3. Tmpfs mounts: A piece of RAM that can be mounted to a container as a folder. Files created by the container in this folder are readonly and will exist as long as the container does. As opposed to volumes, tmps mounts cannot be share between containers.
    - `$ docker run -d -it --name nginx --mount type=tmpfs,target=/var/log/nginx/, tmpfs-mode=1770 -p 5050:80 nginx` The key type specifies tmps to be used, also only the target is specified which is the container's directory. Also the key tmps-mode specifies the permissions in octal form for the files created in this tmps folder. 

    An use case for tmps can be when there is no need to store the data in a bind mount/volume or to assure the container's performance if the app needs to write large volume of non persistent data.
  
  ### 1.5 Docker Basics. Networking. Docker Compose

  ### Networking


  In docker there are various network drivers to work with:

  1. Bridge: Is a default one. When you are running a container, it executes in the default network with the bridge driver. This is how your container can reach and can be reached from your side.
  2. Host: Means that instead of providing a virtual interface to the container, Docker will directly use a real Ethernet interface.
  3. Overlay: connects multiple containers and enables swarm services to communicate with each other.
  4. Macvlan: Allows managing MAC addresses inside a container, making the container appear as a physical device on your network. It is sometimes the best choice when dealing with legacy applications that are supposed to be directly connected to a physical network rather than routed through the Docker host network stack.
  5. Null: Is meant for disabling all networking and is usually used in conjunction with a custom network driver.

  From this drivers docker provides by default three available networking objects:

  1. Bridge: It isolates the networking namespace of the host and the container and provides a virtual interface for the container to use. It is the default networking mode when containers are run, this allows for containers located in the same bridge network to communicate with each other and with the host. Also it allows for automatic DNS resolution between containers just using container's name. Containers located in different bridge networks cannot communicate with each other directly.
  2. Host: This mode shares the networking namespace of the host with the container. With this mode the docker container uses the same host network stack without any isolation. Container service can be directly accesed without using -p flag when running the container. 
  3. None (derived from null): It isolates the networking namespace of the host and the container, it doesn't provide any virtual interface for the container. In this mode, containers have no networking connectivity. They are completely isolated from external networks. This can be useful in scenarios where you want to run a container without any network access.

In docker there can be only 1 networking object created from None and Host drivers, from bridge driver it can be created several network objects

1. Create a network object
  
    `$ docker create network --driver bridge my_network` 

2. List | Remove | Prune network object

    `$ docker network ls|remove|prune my_network`

3. Attach | detach network object to container

    `$ docker network connect|disconnect my_network my_container`

4. Launch container with a given network object

    `$ docker run --network my_network1 --network my_network2 my_image`

4. Check container network configuration

    `$ docker inspec my_container | jq ".[] .NetworkSettings.Networks"`

### Docker Compose

A docker compose file allows for multi container spin up, allowing to create volumes, networks and images in a single run. All options used within docker run command can be applied in a dockercompose file. Docker Compose is a tool for defining and running multi-container Docker applications. The Compose file is a YAML file defining services, networks and volumes. Using a single command allows you to create and start all the services from your configuration.

Docker compose use cases include:

1. Developement environment
2. Automated testing enviroments
3. Single host development

Docker compose privides the following features:

1. Multiple isolated environments on a single host
2. Only recreate containers that have changed

A docker compose file is a YAML file that usually includes the following 6 sections:

1. Splitting the app into services: For instance one service for the frontend and one service for the database, we use customized dockerfiles for each service.

Example of using dockerfile named **dockerfile_mariadb** for database service

  ```
  FROM mariadb #Image from where to pull

  RUN apt-get update && \
  
  apt-get install -y iputils-ping && \
  apt-get install -y mysql-client && apt-get clean
  ```

2. Pulling or building images: In the dockercompose file We can build the images that we have prepared in the dockerfiles by using the directive build, also we can use the image directive to just pull the image from the registry.

```
# First we define the service, all objects or configurations to be used by the service will be child items, i.e, need to be idented

database_service:

  # THIS WAY reads from specified dockerfile
  build: #
        context: . #Where to fin the docker file
        dockerfile: dockerfile_mariadb

  # OR THIS WAY reads image from registry
  image: mariadb #pull directly the image from the registry 
```

3. Configuring environment variables: We can use the directive environment or we can read variables from file

```
database_service:
  build: 
    context: .
    dockerfile: dockerfile_mariadb

  # THIS WAY reads env vars from file
  env_file: 
    - mariadb_env1 # The dash (-) means a item of a list
    - mariadb_env2

  # OR THIS WAY reads key value pairs
  environment: 
    - VAR1:value1
    - VAR2:value2
```

4. Configuring networking and ports: We can attach network objets the a given service and configure on which port they will liste

```
database_service:
  build: 
    context: .
    dockerfile: dockerfile_mariadb

  env_file: 
    - mariadb_env1

  networks:
    - dockercompose-frontend #Specifies the name of the network object

  ports:
    - "3306:3306" Listens on any host interface
    - "0.0.0.0:3306:3306" Listens on any host interface
    - "192.168.0.7:3306:3306" Listens on specifi host interface
   
# Objects need to be created outside all services definition
networks:
  dockercompose-frontend:

```

5. Setting up volumes: We can attach volume objects either bind or volume mounts to containers
```
database_service:
  build: 
    context: .
    dockerfile: dockerfile_mariadb

  env_file: 
    - mariadb_env1

  networks:
    - dockercompose-frontend #Specifies the name of the network object

  ports:
    - "3306:3306"

  volumes:
      - mysql:/var/lib/mysql # A volume mount
      - /opt/docker/dockercompose/task-13/:/backup # A bind mount

networks:
  dockercompose-frontend:

# Volume mounts also needs to be created outside all services definition
# Bind mounts are not needed to be created outside service definition

volumes:
  mysql:

```

6. Build and run: After docker compose is complete, it can be run by using command

- `$ docker compose up -d` Option -d means to run in detach mode

- `$ docker compose up -d --build ` Use to build or rebuild if dockerfile cofiguration or docker compose configuration file is changed.


```
database_service:
  build: 
    context: .
    dockerfile: dockerfile_mariadb

  env_file: 
    - mariadb_env1

  networks:
    - dockercompose-frontend #Specifies the name of the network object

  ports:
    - "3306:3306"

  volumes:
      - mysql:/var/lib/mysql
      - /opt/docker/dockercompose/task-13/:/backup 

networks:
  dockercompose-frontend:

volumes:
  mysql:
````
7. Other configurations: healtchecks, commands, entry points, so on