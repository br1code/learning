# Set up a process manager

To publish and run an ASP.NET Core application using a process manager like IIS or Nginx, you'll need to follow these steps:

**IIS (Internet Information Services) on Windows:**

1. Publish your application to a folder using one of the methods previously described ([Visual Studio, .NET CLI, or Visual Studio Code](./Publish%20to%20a%20folder.md)).

2. Install the .NET Hosting Bundle on the target Windows machine. The bundle includes the .NET runtime, IIS support, and the ASP.NET Core Module (ANCM). You can download the .NET Hosting Bundle from the following link:
   
   https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer

3. After installing the .NET Hosting Bundle, restart the IIS to apply the changes.

4. Open the IIS Manager on the Windows machine.

5. In the IIS Manager, right-click on "Sites" in the "Connections" pane and choose "Add Website".

6. In the "Add Website" dialog, provide a site name, set the "Physical path" to the folder where you published the application, and configure the "Binding" with the desired IP address, port, and hostname.

7. Click "OK" to create the new website.

8. The IIS will now serve your ASP.NET Core application, forwarding incoming HTTP requests to the app using the ASP.NET Core Module.

By setting up a process manager like IIS, you can ensure that your ASP.NET Core application runs smoothly, efficiently manages resources, and benefits from the advanced features provided by these web servers.

The ASP.NET Core Module (ANCM) acts as a native IIS module and provides efficient request processing by forwarding incoming requests to the application. It also enables you to use the IIS Manager for configuring your application, SSL certificates, and other settings.

Using a process manager like IIS improves the reliability and scalability of your application, ensuring that it remains responsive under varying loads and can recover gracefully from errors or crashes.