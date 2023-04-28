# Set up a reverse proxy

## Kestrel Server

Kestrel is a lightweight, cross-platform web server built for ASP.NET Core applications. It is based on the libuv library, which provides high-performance, event-driven I/O. Kestrel is the default web server for ASP.NET Core applications, and it can be used as an edge server (i.e., serving requests directly from the internet) or behind a reverse proxy.

While Kestrel is fast and efficient, it lacks some of the features and security hardening provided by more mature web servers like IIS, Nginx, and Apache HTTP Server. Therefore, it is generally recommended to use Kestrel in conjunction with a reverse proxy when hosting ASP.NET Core applications in production environments.

Note: In the following examples, we will use Linux.

## Nginx

1. Publish your application to a folder using one of the methods previously described ([Visual Studio, .NET CLI, or Visual Studio Code](./Publish%20to%20a%20folder.md)).

2. Install the .NET runtime on the target Linux machine by following the instructions for your specific distribution:

   https://docs.microsoft.com/en-us/dotnet/core/install/linux

3. Install Nginx on the target Linux machine using the package manager for your distribution (e.g., `apt`, `yum`, `dnf`, or `zypper`).

4. Configure Nginx to act as a reverse proxy for your ASP.NET Core application. To do this, create a new Nginx configuration file or modify the default one (usually located at `/etc/nginx/nginx.conf` or `/etc/nginx/sites-available/default`). Add a new `server` block or modify the existing one to include the following settings:

   ```
   server {
       listen        80;
       server_name   example.com;
       location / {
           proxy_pass         http://localhost:5000;
           proxy_http_version 1.1;
           proxy_set_header   Upgrade $http_upgrade;
           proxy_set_header   Connection keep-alive;
           proxy_set_header   Host $host;
           proxy_cache_bypass $http_upgrade;
           proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header   X-Forwarded-Proto $scheme;
       }
   }
   ```

   Replace `example.com` with your domain name and `http://localhost:5000` with the address and port where your application will be running.

5. Restart Nginx to apply the new configuration:

   ```
   sudo systemctl restart nginx
   ```

6. Start your application on the target Linux machine by running the following command in the published folder:

   ```
   dotnet <YourApp.dll>
   ```

   Replace `<YourApp.dll>` with the name of your application DLL file. Your application should now be running and listening on the configured address and port (e.g., `localhost:5000`).

7. Nginx will now serve your ASP.NET Core application, forwarding incoming HTTP requests to the app using the reverse proxy configuration.

The reverse proxy configuration enables you to leverage the performance and flexibility of Nginx while hosting your ASP.NET Core application. Nginx can handle multiple concurrent connections, load balancing, and SSL termination, among other features.

## Apache HTTP Server

Setting up a reverse proxy using Apache HTTP Server involves similar steps to the Nginx configuration . Here's how to set up Apache to act as a reverse proxy for your ASP.NET Core application:

1. Publish your application to a folder using one of the methods previously described ([Visual Studio, .NET CLI, or Visual Studio Code](./Publish%20to%20a%20folder.md)).

2. Install the .NET runtime on the target machine by following the instructions for your specific operating system:

   https://docs.microsoft.com/en-us/dotnet/core/install/

3. Install the Apache HTTP Server on the target machine using the package manager for your distribution (e.g., `apt`, `yum`, `dnf`, or `zypper`).

4. Enable the `proxy`, `proxy_http`, and `headers` modules by running the following commands:

   ```
   sudo a2enmod proxy
   sudo a2enmod proxy_http
   sudo a2enmod headers
   ```

5. Create a new Apache configuration file or modify the default one (usually located at `/etc/apache2/sites-available/000-default.conf` or `/etc/httpd/conf/httpd.conf`). Add a new `<VirtualHost>` block or modify the existing one to include the following settings:

   ```
   <VirtualHost *:80>
       ServerName example.com
       
       ProxyPreserveHost On
       ProxyPass / http://localhost:5000/
       ProxyPassReverse / http://localhost:5000/
       RequestHeader set X-Forwarded-Proto "http"
   </VirtualHost>
   ```

   Replace `example.com` with your domain name and `http://localhost:5000` with the address and port where your application will be running.

6. Restart Apache to apply the new configuration:

   ```
   sudo systemctl restart apache2
   ```

   or

   ```
   sudo systemctl restart httpd
   ```

7. Start your application on the target machine by running the following command in the published folder:

   ```
   dotnet <YourApp.dll>
   ```

   Replace `<YourApp.dll>` with the name of your application DLL file. Your application should now be running and listening on the configured address and port (e.g., `localhost:5000`).

8. Apache HTTP Server will now serve your ASP.NET Core application, forwarding incoming HTTP requests to the app using the reverse proxy configuration.

By setting up a reverse proxy with Apache HTTP Server, you can take advantage of its features, such as SSL/TLS termination, load balancing, and advanced access controls, while hosting your ASP.NET Core application with the Kestrel server.

## Comparison

Both Apache HTTP Server and Nginx are popular, widely-used web servers, and they can both be used as reverse proxies for your ASP.NET Core application. While they share some similarities, they also have notable differences in terms of architecture, performance, and configuration. Here's a comparison between Apache HTTP Server and Nginx:

**Architecture:**

- Apache HTTP Server uses a process-based or a hybrid (process and thread-based) architecture, depending on the chosen Multi-Processing Module (MPM). This architecture can result in increased memory usage under high loads, as each process or thread consumes memory.
- Nginx, on the other hand, uses an event-driven, asynchronous architecture. This allows it to efficiently handle a large number of concurrent connections with minimal memory overhead.

**Performance:**

- Nginx generally performs better than Apache HTTP Server, especially under high loads or when serving static files, due to its event-driven, asynchronous architecture. Nginx can handle many simultaneous connections with low memory usage, making it suitable for high-traffic websites or applications.
- Apache HTTP Server, while still performant, may consume more memory and CPU resources than Nginx under heavy loads. However, its performance can be optimized through various settings and modules.

**Configuration:**

- Apache HTTP Server uses a declarative configuration syntax, with various configuration files and sections (e.g., `<VirtualHost>`, `<Directory>`, and `<Location>`). This can make the configuration more readable and easier to manage, especially for complex setups.
- Nginx uses a more streamlined, block-based configuration syntax. While this can result in a more concise configuration file, it may be less intuitive for users who are new to Nginx or who have more complex configurations.

**Features:**

- Apache HTTP Server has a rich ecosystem of modules that can be used to extend its functionality. This allows you to easily add features like authentication, caching, URL rewriting, and more. However, some of these modules may come at the cost of increased memory and CPU usage.
- Nginx also supports a range of modules, but its core feature set is more focused on performance and efficiency. Some advanced features, like dynamic load balancing, are only available in the commercial version of Nginx (Nginx Plus).

**Community and Support:**

- Apache HTTP Server has been around for longer and has a larger user base, which can translate to more community support and resources, such as documentation, forums, and tutorials.
- Nginx, while having a smaller user base compared to Apache HTTP Server, has been growing in popularity and also has a strong community and support network.

Ultimately, the choice between Apache HTTP Server and Nginx depends on your specific needs, preferences, and the requirements of your ASP.NET Core application. Both web servers are capable of acting as reverse proxies and can provide a stable, secure, and performant environment for your application. You might choose Nginx if you prioritize performance and efficiency, while you might opt for Apache HTTP Server if you prefer its configuration style, available modules, or have more experience with it.