# Docker CLI and commands

## Basic commands

- `docker run`: Creates and starts a new container from a specified image. It accepts various options, such as `-it` (interactive mode), `-d` (detached mode), and `-p` (port mapping).
  Example: `docker run -it -p 8080:80 nginx`

- `docker ps`: Lists all running containers. Use `-a` flag to display all containers, including stopped ones.
  Example: `docker ps -a`

- `docker stop`: Stops a running container by providing the container ID or name.
  Example: `docker stop container_id`

- `docker start`: Starts a stopped container using its ID or name.
  Example: `docker start container_id`

- `docker rm`: Removes a stopped container. Use `-f` flag to force-remove a running container.
  Example: `docker rm container_id`

- `docker rmi`: Removes an image by providing its ID or name. It can only remove images not used by any container.
  Example: `docker rmi image_id`

- `docker logs`: Fetches the logs of a specified container.
  Example: `docker logs container_id`

- `docker inspect`: Provides detailed information about the specified container, image, or volume in JSON format.
  Example: `docker inspect container_id`

## Dockerfile: syntax, instructions, and best practices

A Dockerfile is a text file containing a set of instructions to build a Docker image. Each instruction creates a new layer in the image. Some common instructions include:

- `FROM`: Specifies the base image to build upon. This is usually the first instruction in a Dockerfile.
  Example: `FROM node:14`

- `RUN`: Executes a command in a new layer, typically used for installing packages or configuring the environment.
  Example: `RUN apt-get update && apt-get install -y git`

- `COPY`: Copies files or directories from the host system to the image.
  Example: `COPY . /app`

- `WORKDIR`: Sets the working directory for subsequent instructions (e.g., `RUN`, `COPY`).
  Example: `WORKDIR /app`

- `EXPOSE`: Informs Docker that the container will listen on the specified network ports at runtime.
  Example: `EXPOSE 80`

- `CMD`: Provides default executable and arguments for the container, which can be overridden during `docker run`.
  Example: `CMD ["npm", "start"]`

- `ENTRYPOINT`: Specifies the executable for the container, with any arguments provided during `docker run` being appended to the ENTRYPOINT command.
  Example: `ENTRYPOINT ["npm"]`

### Best practices

1. Use an appropriate base image: Start with a minimal base image and only include the necessary components for your application.
2. Optimize layer creation: Combine multiple related `RUN` instructions into a single one using `&&` to reduce layers and build time.
3. Use `.dockerignore`: Create a `.dockerignore` file to exclude unnecessary files and directories from the build context.
4. Cache dependencies: Copy dependencies separately from the application code to leverage Docker's build cache and speed up builds.
5. Use multi-stage builds: Use multiple `FROM` instructions to create intermediate images for building and final images for running, reducing the final image size.

## Building, tagging, and pushing Docker images

- Building: Use `docker build` command with `-t` flag to specify the image name and tag. Provide the build context (usually `.`) as the last argument.
  Example: `docker build -t my-image:1.0 .`

- Tagging: Use `docker tag` to create a new tag for an existing image or to assign a new name and tag to an image. The command format is `docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]`.
  Example: `docker tag my-image:1.0 myrepo/my-image:1.0`

- Pushing: Use `docker push` command to upload your image to a registry like Docker Hub. Ensure you are logged in with `docker login` before pushing. The format is `docker push REPOSITORY[:TAG]`.
  Example: `docker push myrepo/my-image:1.0`

## Running containers with custom settings:

- Port mapping: Use the `-p` or `--publish` flag to map container ports to host ports. The format is `-p HOST_PORT:CONTAINER_PORT`.
  Example: `docker run -p 8080:80 nginx`

- Environment variables: Use `-e` or `--env` flag to pass environment variables to the container.
  Example: `docker run -e MY_VAR=value my-image`

- Volumes: Use `-v` or `--volume` flag to mount host directories or named volumes to the container. The format is `-v HOST_DIR:CONTAINER_DIR` or `-v VOLUME_NAME:CONTAINER_DIR`.
  Example: `docker run -v /data:/app/data my-image`

- Network: Use `--network` flag to connect the container to a specific Docker network.
  Example: `docker run --network my-network my-image`

- Detached mode: Use `-d` or `--detach` flag to run the container in the background.
  Example: `docker run -d my-image`

- Interactive mode: Use `-it` or `--interactive --tty` flags to run the container with an interactive shell.
  Example: `docker run -it ubuntu bash`

These are just a few examples of the settings you can customize when running Docker containers. By understanding and leveraging these options, you can configure containers to meet the specific requirements of your application and environment.