# Docker-DevOps-Essentials
## If I'll find any other feature to be useful - it'll surely be added!
## What is containerization?
Containerization is the process of running applications in lightweight, isolated environments that share the host operating system kernel. Containers contain only the application and its dependencies. At the host operating system level, containerization works by creating an isolated group of processes, which are applications running in containers, where communication between these processes is possible. Containerization works through a container engine that runs on the host system. Containers are lightweight, launched quickly, portable, efficient and scalable in relation to virtual machines. Still, they're less isolated from host OS kernel than VM's. Docker is a platform of containerization.
## Basics
* **Dockerfile** - a script containing a series of instructions on how to build a Docker image
* **Image** - a lightweight, standalone, and executable package that includes everything needed to run a piece of software
* **Container** - a runtime instance of a Docker image
### Basic Docker commands
* **docker run -d \<image\>** - runs container based on given image, -d stands for detached meaning it runs in background
  - **docker run -d --name \<name\> \<image\>** - option that names container
  - **docker run -d -e \<VARIABLE=value\> \<image\>** - option that sets environment variables container
  - **docker run -d --env-file \<name\> \<image\>** - option that reads environment variables from given file
  - **docker run -d -v \<volume_name_or_host_path\:container_path\> \<image\>** - option that mounts volume into container
  - **docker run -d --network=\<network_name\> \<image\>** - option that connects container to network
  - **docker run -d -p \<host_port\:container_port\> \<image\>** - option that binds container's and host's ports
* **docker stop \<container_name\>** - stops running container
* **docker start \<container_name\>** - starts stopped container
* **docker ps** - lists all running containers
  - **docker ps -a** - lists all containers, including stopped ones
* **docker rm \<container_name\>** - removes container
* **docker stats** - displays real-time statistics of containers
* **docker inspect \<container_name\>** - displays detailed information about container or image
* **docker logs \<container_name\>** - displays container logs
* **docker exec \<container_name\> \<command\>** - executes command in container
  - **docker exec -it \<container_name\> /bin/bash** - opens interactive terminal in container
* **docker system prune** - removes unused Docker data - images, containers, networks, volumes
## Images
### Dockerfile
Basic Dockerfile instructions:
* **FROM [--platform=\<platform\>] \<image\> [AS \<name\>]** - determines base image that will be used by new image, in multi-stage builds it creates new build from base image
* **RUN [OPTIONS] \<command\>** - executes build shell commands
* **COPY [OPTIONS] \<src\> \<dest\>** - copies files and directories from host to container
* **ADD [OPTIONS] \<src\> \<dest\>** - similar to COPY but it also works with remote resources and automatically unzips archives
* **WORKDIR /path/to/workdir** - sets working directory for next instructions
* **ENV \<key\>=\<value\> [\<key\>=\<value\>]** - sets environment variables
* **EXPOSE \<port\> [\<port\>/\<protocol\>]** - defines ports in container that application will listen on
* **CMD ["executable", "param1", "param2"]** - specifies default command that will be run after container launch, can be overwritten by user
* **ENTRYPOINT ["executable", "param1", "param2"]** - similar to CMD but harder to overwrite, application entrypoint
* **VOLUME /data** - creates volume mount
* **LABEL \<key\>=\<value\> [\<key\>=\<value\>]** - adds metadata to an image
* **USER \<user\>[:\<group\>]** - sets user and group id to use as the default user and group
* **ARG \<name\>[=\<default value\>] [\<name\>[=\<default value\>]]** - defines variable that can be passed at build-time to the builder with the docker build command using the --build-arg <varname>=<value> flag\
\
Example Dockerfile:
```
FROM <base_image>

ARG <name>

LABEL <key> <value>

USER <username>

WORKDIR <working_directory_path>

COPY <source_path> <destination_path>

RUN <command>

ENV <ENVIRONMENT_VARIABLE>=<value>

EXPOSE <port_number>

ENTRYPOINT ["command", "in", "parts"]
```
### Minimal base images
It is worth using minimal base images when possible, reducing unnecessary data, increasing the efficiency and speed of a container, and reducing the container attack surface.\
Popular minimal images:
* **Alpine Linux** - light Linux distribution, for common Linux usage
* **Scratch** - completely empty image, for applications that do not require OS
* **Distroless** - by Google, images that contain only the application and its dependencies, without the shell, system packages, or tools.
### Multi-stage builds
Multi-stage builds allow for separating different parts of image creation. Products of stages can be copied to stages after them. Benefits of doing so are: removing unnecessary dependencies, tools, temporary files, etc. that were only needed during application building phase, not in runtime phase, so it optimizes final image size and container efficiency, launch speed and attack surface.\
Example Dockerfile using Multi-stage build:
```
FROM <base_build_image> AS builder

WORKDIR <working_directory_path>

COPY <source_path> <destination_path>

RUN <build_command>

#---------------------------------------------------
FROM <minimal_base_image>

WORKDIR <working_directory_path>

COPY --from=builder <source_path> <destination_path>

ENTRYPOINT ["run", "command"]
```
### Multi-platform builds
A multi-platform build refers to a single build invocation that targets multiple different operating system or CPU architecture combinations. When building images, this lets you create a single image that can run on multiple platforms, such as linux/amd64, linux/arm64, and windows/amd64.\
To use multi-platform build you need to create special builder:
```
docker buildx create --name <name> --driver <driver> --bootstrap --use
```
Possible drivers are: docker-container, kubernetes, remote.\
To build multi-platform image use:
```
docker buildx build --platform <platforms> -t <tag> .
```
Platforms can be also declared in Dockerfile by using:
```
FROM --platform=$TARGETPLATFORM <image>
```
where $TARGETPLATFORM variable is determining desired platform of base image.
### Image commands
* **docker images** - lists all images
* **docker pull \<image\>** - pulls image from registry
* **docker rmi \<image\>** - removes image
* **docker build -t \<tag_name\> .** - builds Dockerfile into image, -t stands for new image tag name
* **docker buildx build --platform \<platforms\> -t \<tag\> .** - builds Dockerfile into multi-platform image, --platform stands for desired image platforms, -t stands for new image tag name
* **docker inspect \<image_name\>** - displays detailed information about image
## Storage
Volumes are persistent data stores for containers, created and managed by Docker. You can create a volume explicitly using the docker volume create command, or Docker can create a volume during container or service creation.
### Storage commands
* **docker volume ls** - lists all volumes
* **docker volume create \<volume_name\>** - creates volume
* **docker volume rm \<volume_name\>** - removes volume
* **docker volume inspect \<volume_name\>** - displays detailed information about volume
## Networking
Container networking refers to the ability for containers to connect to and communicate with each other, or to non-Docker workloads.
### Networking commands
* **docker network ls** - lists all networks
* **docker network create \<network_name\>** - creates network
* **docker network connect \<network_name\> \<container_name\>** - connects container to network
* **docker network disconnect \<network_name\> \<container_name\>** - disconnects container from network
* **docker network inspect \<network_name\>** - displays detailed information about network
## Registry
An image registry is a centralized location for storing and sharing your container images. It can be either public or private. Docker Hub is a public registry that anyone can use and is the default registry.
### Registry commands
* **docker login** - authenticates to a registry
* **docker tag \<image\> \<user/tag\:version\>** - tags image with given tag
* **docker push \<registry_url/user/tag\:version\>** - pushes image to registry
## Security
### Docker Scout
Container images consist of layers and software packages, which are susceptible to vulnerabilities. These vulnerabilities can compromise the security of containers and applications. Docker Scout is a solution that analyzes images and compiles an inventory of components, also known as a Software Bill of Materials (SBOM). The SBOM is matched against a continuously updated vulnerability database to pinpoint security weaknesses.\
To enroll organization with Docker Scout:
```
docker scout enroll <organization_name>
```
To enable Docker Scout for Docker Hub image repository:
```
docker scout repo enable --org <organization_name> <organization_name>/<repository_name>
```
To analyse image vulnerabilities:
```
docker scout cves <image>
```
### Running containers as a non-privileged user
By default, a container runs as a root user. Containers running as a non-privileged user have limited permissions, which increases host security and container isolation. It is especially important in production environments.\
This problem can be solved by adding new non-privileged user and group in container and set new user with USER Dockerfile instruction, for example:
```
FROM ubuntu:latest
RUN apt-get -y update
RUN groupadd -r user && useradd -r -g user user
USER user
```
## Docker Compose
Docker Compose is a tool for defining and running multi-container applications.\
Compose simplifies the control of your entire application stack, making it easy to manage services, networks, and volumes in a single, comprehensible YAML configuration file. Then, with a single command, you create and start all the services from your configuration file.\
Compose configuration consists of services, volumes and networks. Services represent containers that application consists of. Volumes are defined persistent data stores. Networks allow containers for communication by services names. Docker Compose automatically creates network for application stack to allow it's components to communicate.\
Example Docker Compose YAML file:
```
version: '3'
services:
  <service_name>:
    image: <user/image_name:version>
    ports:
      - <host_port_number>:<container_port_number>
    volumes:
      - /host/path/to/volume:/container/path/to/volume
    networks:
      - <network_name>

  <service_name>:
    image: <user/image_name:version>
    environment:
      <ENVIRONMENT_VARIABLE_1>: <value1>
      <ENVIRONMENT_VARIABLE_2>: <value2>
    volumes:
      - <volume_name>:/container/path/to/volume
    networks:
      - <network_name>

volumes:
  <volume_name>:

networks:
  <network_name>:
```
### Docker Compose commands
* **docker compose up** - creates and starts services defined in yaml docker-compose file
* **docker compose down** - stops and removes services defined in yaml docker-compose file
* **docker compose logs** - displays logs of docker-compose services
