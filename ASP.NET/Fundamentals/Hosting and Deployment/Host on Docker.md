# Host on Docker

**Note: This tutorial may be outdated. See updated examples** [here](https://github.com/dotnet/dotnet-docker/tree/main/samples)

To host an ASP.NET Core application on Docker, follow these steps:

1. **Create a Dockerfile:**
   
   In the root of your ASP.NET Core project, create a new file named `Dockerfile` (without any file extension). This file will contain instructions for building a Docker image of your application.

2. **Configure the Dockerfile:**

   Open the Dockerfile in a text editor and add the following content:

   ```dockerfile
   # Use the official Microsoft .NET SDK image as the base image
   FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build

   # Set the working directory within the container
   WORKDIR /src

   # copy csproj and restore as distinct layers
   COPY ./*.csproj .
   RUN dotnet restore --use-current-runtime  

   # Copy the project files into the container
   COPY . .

   # Publish the application to a folder within the container
   RUN dotnet publish --use-current-runtime --self-contained false --no-restore -o /app

   # Use the official Microsoft .NET runtime image as the base image for the final stage
   FROM mcr.microsoft.com/dotnet/aspnet:7.0

   # Set the working directory within the container
   WORKDIR /app

   # Copy the published files from the build stage into the container
   COPY --from=build /app .

   # Start the application
   ENTRYPOINT ["dotnet", "<YourApp.dll>"]
   ```

   Replace `<YourApp.dll>` with the name of your application's DLL file (e.g., `MyWebApp.dll`). This Dockerfile uses a multi-stage build process to optimize the image size and build time. The first stage (named "build") is based on the .NET SDK image and is responsible for building and publishing the application. The second stage is based on the .NET runtime image and contains the published files from the previous stage.

3. **Build the Docker image:**

   Open a terminal or command prompt, navigate to the root of your project (where the Dockerfile is located), and run the following command to build the Docker image:

   ```
   docker build -t <your-image-name> .
   ```

   Replace `<your-image-name>` with a name for your Docker image (e.g., `my-web-app`). Make sure to include the period at the end of the command, as it denotes the build context (the current directory).

4. **Run the Docker container:**

   After the Docker image has been built, you can create and run a container from it using the following command:

   ```
   docker run -d -p 8080:80 --name <your-container-name> <your-image-name>
   ```

   Replace `<your-container-name>` with a name for your Docker container (e.g., `my-web-app-container`) and `<your-image-name>` with the name you used when building the image. The `-d` flag runs the container in detached mode (in the background), and the `-p 8080:80` flag maps port 8080 on the host machine to port 80 on the container.

5. **Access the application:**

   Your ASP.NET Core application should now be running inside the Docker container and accessible via `http://localhost:8080` (or the IP address of the Docker host, if you're using Docker Toolbox or a remote host).

6. **Stop and remove the container:**

   When you're done using the container, you can stop and remove it using the following commands:

   ```
   docker stop <your-container-name>
   docker rm <your-container-name>
   ```

   Replace `<your-container-name>` with the name of your Docker container.

By hosting your ASP.NET Core application on Docker, you can create an isolated and reproducible environment that can be easily deployed and scaled across various platforms and environments. Docker containers encapsulate your application, its dependencies, and runtime, ensuring consistent behavior across development, testing, and production stages. This helps eliminate issues related to differing system configurations or dependencies and simplifies deployment and management.

Once you have your ASP.NET Core application running in a Docker container, you can take advantage of various container orchestration tools, such as Docker Compose, Kubernetes, or Azure Container Instances, to manage the deployment, scaling, and maintenance of your application. These tools can help you create and manage multi-container applications, automate deployment pipelines, and monitor the health and performance of your application in production.

Using Docker to host your ASP.NET Core application also allows you to leverage various cloud platforms and their container services, such as Amazon Web Services (AWS) Elastic Container Service (ECS), Google Cloud Platform (GCP) Kubernetes Engine, or Microsoft Azure Container Instances, to easily deploy, scale, and manage your application in the cloud.