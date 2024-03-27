
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
2- Overlay2:
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

In docker containers rely on images. An image is a combination of filesystems and parameters. Following are some command to work with images:

1. List local docker images
- `$ docker images`
2. Create an image from a tarball
- `$ docker import image.tar imagename:tag`
3. Create an image from dockerfile
- `$ docker build -t mynginximage DockerFilePath`
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

#### Working with The Image Layers

