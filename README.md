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
  - **docker run -d -v \<host_path\:container_path\> \<image\>** - option that mounts volume into container
  - **docker run -d --network=\<network_name\> \<image\>** - option that connects container to network
  - **docker run -d -p \<host_port\:container_port\> \<image\>** - option that binds container's and host's ports
* **docker stop \<container_name\>** - stops running container
* **docker start \<container_name\>** - starts stopped container
* **docker ps** - lists all running containers
  - **docker ps -a** - lists all containers, including stopped ones
* **docker rm \<container_name\>** - removes container
* **docker stats** - displays real-time statistics of containers
* **docker inspect \<container_or_image_name\>** - displays detailed information about container or image
* **docker logs \<container_name\>** - displays container logs
* **docker exec \<container_name\> \<command\>** - executes command in container
  - **docker exec -it \<container_name\> /bin/bash** - opens interactive terminal in container
* **docker system prune** - removes unused Docker data - images, containers, networks, volumes
## Images
### Dockerfile
### Minimal base images
### Multi-stage builds
### Image commands
* **docker images** - lists all images
* **docker pull \<image\>** - pulls image from registry
* **docker rmi \<image\>** - removes image
* **docker build -t \<tag_name\> .** - builds Dockerfile into image, -t stands for new image tag name
## Storage
### Storage commands
* **docker volume ls** - lists all volumes
* **docker volume create \<volume_name\>** - creates volume
* **docker volume rm \<volume_name\>** - removes volume
## Networking
### Networking commands
* **docker network ls** - lists all networks
* **docker network create \<network_name\>** - creates network
* **docker network connect \<network_name\> \<container_name\>** - connects container to network
* **docker network disconnect \<network_name\> \<container_name\>** - disconnects container from network
## Registry
### Registry commands
* **docker login** - authenticates to a registry
* **docker tag \<image\> \<user/tag\:version\>** - tags image with given tag
* **docker push \<registry_url/user/tag\:version\>** - pushes image to registry
## Security
### Docker Scout
### User namespaces
### Docker Secrets
### Security commands
## Docker Compose
### Docker Compose commands
* **docker compose up** - creates and starts services defined in yaml docker-compose file
* **docker compose down** - stops and removes services defined in yaml docker-compose file
* **docker compose logs** - displays logs of docker-compose services
