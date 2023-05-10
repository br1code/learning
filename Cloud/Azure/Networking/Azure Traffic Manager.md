# Azure Traffic Manager

Azure Traffic Manager is a DNS-based load balancing service that distributes incoming traffic across multiple endpoints (services) based on configurable routing rules. It helps improve application availability and responsiveness by directing users to the nearest or best-performing endpoint. Here's an overview of its key aspects:

## Traffic Routing Methods

Azure Traffic Manager supports several routing methods to distribute traffic:
- Priority: Routes traffic to the primary endpoint, and only to secondary endpoints if the primary is unavailable.
- Weighted: Distributes traffic based on assigned weights (percentages) for each endpoint.
- Performance: Routes traffic to the endpoint with the lowest latency, based on the user's geographical location.
- Geographic: Directs traffic to specific endpoints based on the user's geographical region.
- Multivalue: Returns multiple healthy endpoints in a DNS response, enabling the client to choose one.
- Subnet: Routes traffic based on the user's IP address, allowing for IP-based traffic control.

## Endpoints

Traffic Manager supports various endpoint types, such as Azure App Services, Azure Virtual Machines, Azure Load Balancers, and external endpoints (non-Azure services). You can combine different endpoint types within the same Traffic Manager profile.

## Health Probes

Traffic Manager uses health probes to monitor the availability of endpoints. It sends periodic DNS queries to each endpoint and, based on their responses, determines if they are healthy or not. Unhealthy endpoints are automatically removed from DNS responses.

## DNS Time to Live (TTL)

The TTL determines how long the DNS response should be cached by clients and DNS resolvers. Lower values allow for faster failover to healthy endpoints but increase the number of DNS queries. You can configure the TTL based on your application's requirements.

## Nested Profiles

Traffic Manager supports nesting profiles, enabling you to create complex routing schemes. For example, you can combine the Performance routing method at the top level with Priority routing at the next level to achieve both low-latency and failover capabilities.

## Traffic View

Traffic View is a feature that provides insights into the traffic patterns and performance of your Traffic Manager profile. You can use this information to optimize your routing configuration and improve user experience.

## Integration with Azure Services

Azure Traffic Manager integrates with other Azure services, such as Azure App Service, Virtual Machines, and Load Balancers, enabling you to build highly available and responsive applications.

## Monitoring and Diagnostics

Azure Traffic Manager provides monitoring and diagnostics capabilities using Azure Monitor, logs, and alerts. You can use these tools to analyze performance, troubleshoot issues, and optimize your Traffic Manager configuration.