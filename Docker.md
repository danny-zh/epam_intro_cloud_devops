
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

The following are the key components of docker

1. Docker Daemon: provides details for container execution to daemon Containerd
2. Containerd: Manages the low level container tasks such as storage, image distribution, network etc. Communicates with service Shimc.
3. Shimc: Runs a process called Runc to start the container
4. Runc: It is a ligthweigth library for running containers. Starts the container and then goes away. 
5. Shimc: After starting, shimc manages the running container
6. Docker-proxy: Responsable for proxing container ports to host interface.


