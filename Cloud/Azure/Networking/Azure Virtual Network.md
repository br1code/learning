# Azure Virtual Network

Azure Virtual Network (VNet) is a fundamental networking service in Azure that enables you to create and manage isolated, private networks within the Azure cloud. It allows you to securely connect your Azure resources, on-premises infrastructure, and even other cloud environments. Here's a comprehensive overview:

## Definition

An Azure Virtual Network (VNet) is a logically isolated network within the Azure cloud, where you can deploy and manage your resources such as VMs, databases, and application services. It provides a secure and flexible environment for hosting and communicating between resources, and it enables you to create custom network topologies that meet your specific requirements.

## Address Space and Subnets

When creating a VNet, you need to define its address space, which is a range of private IP addresses based on the CIDR notation. Within the address space, you can create multiple subnets to segment and organize your resources. Each subnet has its own address range, which cannot overlap with other subnets within the same VNet.

## Network Security Groups (NSGs)

[Network Security Groups](./Azure%20Network%20Security%20Group.md) (NSGs) are used to control inbound and outbound traffic to resources within a VNet. NSGs contain a set of security rules that define allowed or denied traffic based on factors such as source and destination IP addresses, ports, and protocols. You can associate NSGs with subnets or individual resources, like VMs, to enforce network security policies.

## Virtual Network Peering

Virtual Network Peering allows you to connect two VNets within the same region or across regions, enabling resources in both VNets to communicate with each other using private IP addresses. Peering can be used for various scenarios, such as connecting multiple Azure subscriptions, sharing resources between environments, or enabling high-speed communication between resources in different regions.

## VNet-to-VNet and Site-to-Site VPN

VNet-to-VNet and Site-to-Site VPN connections enable you to create secure connections between your VNets and on-premises infrastructure. VNet-to-VNet VPNs allow you to connect VNets within Azure, while Site-to-Site VPNs enable you to connect your on-premises networks to Azure VNets. These connections use IPsec/IKE (Internet Protocol Security/Internet Key Exchange) protocols to provide encrypted and secure communication.

## ExpressRoute

[Azure ExpressRoute](./Azure%20ExpressRoute.md) is a dedicated private network connection between your on-premises infrastructure and Azure. It bypasses the public internet, providing lower latency, higher bandwidth, and more reliable connectivity compared to VPN connections. ExpressRoute is suitable for scenarios that require high performance, low latency, or enhanced security.

## Network Interface Cards (NICs)

A Network Interface Card (NIC) is a virtual representation of a network adapter that is attached to resources like VMs within a VNet. Each NIC is associated with a subnet and can be assigned a private IP address, public IP address, and one or more NSGs.

## Public IP Addresses and Load Balancers

Public IP addresses can be assigned to resources within a VNet to enable communication with the public internet. You can use [Azure Load Balancer](./Azure%20Load%20Balancer.md) (Layer-4) or [Azure Application Gateway](./Azure%20Application%20Gateway.md) (Layer-7) to distribute incoming traffic across multiple resources, improving performance, and providing high availability.

## Private Endpoints and Service Endpoints

Private Endpoints allow you to access Azure services, like Storage or SQL Database, over a private IP address within your VNet. Service Endpoints enable you to restrict access to Azure services from specific subnets within your VNet, improving security and reducing exposure to the public internet.

## Domain Name System (DNS)

By default, VNets use Azure-provided DNS to resolve hostnames within the network. You can also configure custom DNS settings or use your own DNS servers to meet your specific requirements. This enables you to implement custom domain names, internal name resolution, or integrate with your existing on-premises DNS infrastructure.

## Routing

Azure Virtual Networks use a system-defined route table to determine the path of network traffic between resources. You can also create custom route tables to override the default routing behavior and implement custom network topologies, such as forced tunneling or hub-and-spoke architectures. User-defined routes (UDRs) can be added to custom route tables to define specific paths for network traffic based on source and destination IP addresses, address prefixes, and next hop types.

## Network Virtual Appliances (NVAs)

Network Virtual Appliances (NVAs) are specialized virtual machines that perform network functions, such as firewalls, WAN optimization, or load balancing. NVAs can be deployed within a VNet to enhance network security, performance, or reliability, and can be integrated with other Azure services or on-premises infrastructure.

## Hybrid Connectivity

Azure Virtual Network supports hybrid connectivity scenarios, enabling you to integrate your cloud resources with your on-premises infrastructure. This can be achieved using VPN connections, [ExpressRoute](./Azure%20ExpressRoute.md), or Azure Arc-enabled services, depending on your performance, security, and management requirements.

## VNet Integration

VNet Integration enables you to securely access resources within a VNet from Azure services like [Azure App Services](../Compute/Azure%20App%20Services.md) or [Azure Functions](../Compute/Azure%20Functions.md). This allows you to host backend services, databases, or other resources within a VNet and access them from your applications running on Azure.

Understanding Azure Virtual Network is crucial for designing and managing secure, scalable, and high-performing network architectures in Azure. Make sure to explore the official Azure VNet documentation and related resources to learn more and apply this knowledge to your projects.