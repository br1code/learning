# Publish to a folder

Publishing an ASP.NET Core application to a folder involves creating a self-contained deployment or a framework-dependent deployment, which can be later hosted on a compatible web server or runtime. In this explanation, we will focus on the process of publishing the application to a folder using Visual Studio, .NET CLI, and Visual Studio Code.

1. **Using Visual Studio:**

To publish an ASP.NET Core application to a folder using Visual Studio, follow these steps:

a. Open your project in Visual Studio.

b. Right-click on the project in the Solution Explorer and select "Publish".

c. In the "Pick a publish target" window, choose "Folder".

d. Select a folder where you want to publish the application, or create a new one.

e. (Optional) In the "Configuration" dropdown, select the desired configuration (e.g., "Release").

f. Click "Publish". Visual Studio will build and publish the application to the specified folder.

2. **Using .NET CLI:**

To publish an ASP.NET Core application to a folder using .NET CLI, follow these steps:

a. Open a command prompt or terminal.

b. Navigate to the project directory (the one containing the .csproj file).

c. Run the following command, replacing `<OUTPUT_FOLDER>` with the desired output folder path, and `<RUNTIME_IDENTIFIER>` with the target runtime identifier (e.g., `win-x64`, `linux-x64`, or `osx-x64`):

For framework-dependent deployment:
```
dotnet publish -c Release -o <OUTPUT_FOLDER>
```

For self-contained deployment:
```
dotnet publish -c Release -r <RUNTIME_IDENTIFIER> -p:PublishSingleFile=true --self-contained true -o <OUTPUT_FOLDER>
```

d. The .NET CLI will build and publish the application to the specified folder.

3. **Using Visual Studio Code:**

To publish an ASP.NET Core application to a folder using Visual Studio Code, you'll need to use the .NET CLI commands mentioned earlier, but you can do it from the Visual Studio Code terminal.

a. Open your project in Visual Studio Code.

b. Open the terminal in Visual Studio Code by clicking on "Terminal" > "New Terminal" in the top menu.

c. Follow the steps mentioned in the ".NET CLI" section above.

Once the application is published to a folder, you can deploy it to a server or runtime compatible with the selected runtime identifier. The server will need to have the appropriate version of the .NET runtime installed (for framework-dependent deployments) or a compatible OS (for self-contained deployments).

The published folder will contain all the required files to run the application, including the executable (for self-contained deployments), DLLs, configuration files, and static assets. To start the application, you'll need to configure the web server to forward incoming HTTP requests to the application or use a process manager to keep the application running in the background.

---

Note: To run the published app locally, run `dotnet <ApplicationName>.dll` from the publish folder.