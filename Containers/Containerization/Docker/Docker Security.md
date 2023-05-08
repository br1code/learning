# Docker Security

## Docker security features:

1. Namespace isolation: Docker uses Linux namespaces to isolate containers from each other and the host system. Each container gets its own process, network, mount, user, and IPC namespaces.

2. Control groups (cgroups): Docker uses cgroups to limit and allocate resources, such as CPU, memory, and I/O, to containers. This helps prevent a single container from consuming too many resources and affecting other containers or the host system.

3. Capabilities: Docker drops many Linux capabilities by default, limiting the privileges of processes running inside containers. This reduces the potential impact of container breakout or exploitation.

4. Seccomp: Docker uses Seccomp (Secure Computing Mode) profiles to limit the system calls available to containers, reducing the attack surface.

5. AppArmor/SELinux: Docker supports AppArmor and SELinux, which are mandatory access control (MAC) systems that enforce security policies and limit the actions of processes running inside containers.

6. Docker Content Trust: Docker Content Trust provides a mechanism for signing and verifying images, ensuring that the images you use come from trusted sources and have not been tampered with.

## Security best practices

1. Use minimal and trusted base images: Start with minimal base images, like Alpine Linux, and use images from trusted sources, such as official repositories or vendors.

2. Keep images up-to-date: Regularly update your images and their dependencies to address security vulnerabilities.

3. Limit container capabilities: Use the `--cap-drop` and `--cap-add` options to remove unnecessary capabilities and grant only the required ones to your containers.

   Example: `docker run --cap-drop=all --cap-add=NET_BIND_SERVICE my-image`

4. Use read-only file systems: Mount your container's file system as read-only, if possible, to prevent unauthorized modifications.

   Example: `docker run --read-only my-image`

5. Use user namespaces: Enable user namespace remapping to map the root user inside the container to a non-root user on the host system, reducing the risk of privilege escalation.

6. Limit resource usage: Use cgroups to limit the resources available to containers, preventing resource exhaustion attacks.

   Example: `docker run --memory=256m --cpus=1 my-image`

7. Secure container networks: Use network segmentation to isolate containers and restrict communication between them. Apply network policies and firewall rules to control traffic.

8. Enable Docker Content Trust: Enable Docker Content Trust to ensure the integrity and authenticity of images.

   Example: `export DOCKER_CONTENT_TRUST=1`

c. Scanning images for vulnerabilities:

1. Use Docker Scan: Docker Scan is a built-in vulnerability scanner for Docker images, powered by Snyk. It checks your images for known security vulnerabilities in the base image and its dependencies.

   Example: `docker scan my-image`

2. Use third-party scanners: You can also use third-party scanning tools, like Clair, Anchore Engine, or Aqua Security, to scan your images for vulnerabilities. These tools can provide additional features, like policy enforcement and continuous monitoring.