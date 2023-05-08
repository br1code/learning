# Docker Basics

## Docker architecture

Docker follows a client-server architecture. The Docker architecture comprises three primary components: the Docker Client, the Docker Daemon, and Docker Registries. The Docker Client and Docker Daemon communicate using a REST API, Unix sockets, or network interfaces.

- **Docker Client** is the user interface to interact with Docker. Developers and administrators use the Docker CLI or other tools to send commands to the Docker Daemon, which processes them and manages Docker objects such as images, containers, networks, and volumes.

- **Docker Daemon** (dockerd) is a background service that runs on the host system, managing Docker objects and performing operations such as building images, running containers, and managing networks and storage. It can also communicate with other Docker Daemons to manage distributed Docker environments.

- **Docker Registries** are servers that store and distribute Docker images. Docker Registries use a simple RESTful API to manage images, and they support standard Docker commands such as pull, push, and search.

## Docker components

- Docker Client: As mentioned earlier, the Docker Client is the primary means of interaction with Docker. Users issue commands to build, pull, push, run, and manage containers using the Docker CLI or other compatible tools.

- Docker Daemon: The Docker Daemon is responsible for managing Docker objects and processing commands sent by the Docker Client. It listens for API requests and performs the necessary actions to create and manage containers, images, networks, and volumes.

- Docker Images: Docker Images are the building blocks of containers. An image is a lightweight, stand-alone, and executable software package that includes everything needed to run a piece of software, including the code, runtime, libraries, environment variables, and config files. Images are created from a set of instructions written in a Dockerfile. Images can be stored and distributed using Docker Registries.

- Docker Containers: A Docker Container is a running instance of a Docker Image. Containers provide an isolated environment to run applications, encapsulating all dependencies and runtime configurations. Containers can be created, started, stopped, and removed using Docker commands or APIs.

## Docker Hub and registries

Docker Hub is a cloud-based registry service provided by Docker Inc. It allows you to store and distribute Docker Images publicly or privately. Docker Hub serves as a central repository for sharing and distributing images worldwide. It also provides a set of official images maintained by Docker Inc. and the community, which serve as a foundation for building custom images.

In addition to Docker Hub, there are other registries available, such as Google Container Registry, Amazon Elastic Container Registry, and Azure Container Registry. You can also deploy your own private registry using the open-source Docker Registry.

Docker registries store images as a collection of layers. Each layer represents a set of filesystem changes resulting from the instructions in the Dockerfile. When pulling or pushing images, Docker only transfers the layers that are not already present on the source or destination, reducing bandwidth usage and transfer times.

## Installing Docker on different operating systems

Docker is available for various operating systems, including Linux, macOS, and Windows. The installation process varies depending on the OS:

- Linux: Docker can be installed on many Linux distributions, such as Ubuntu, Debian, CentOS, and Fedora, using package managers like apt, yum, or dnf. Docker provides official installation scripts and guides for these distributions.

- macOS: Docker Desktop for Mac is the recommended way to install Docker on macOS. It is available as a downloadable package from the Docker website and includes Docker Engine, Docker CLI, Docker Compose, and other tools.

- Windows: Docker Desktop for Windows is the recommended installation method for Windows 10 and Windows Server 2016 or later. Docker Desktop includes Docker Engine, Docker CLI, Docker Compose, and other tools. For older Windows versions, Docker Toolbox, which uses Oracle VM VirtualBox to run Docker in a Linux VM, is an alternative.