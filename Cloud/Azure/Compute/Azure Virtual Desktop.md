# Azure Virtual Desktop

Azure Virtual Desktop (formerly known as Windows Virtual Desktop) is a desktop and app virtualization service that runs on the Azure cloud. It enables you to deploy and manage virtual desktops and applications for remote users, providing a flexible and secure remote work environment. Here's an overview of Azure Virtual Desktop:

## Multi-session Windows

Azure Virtual Desktop supports multi-session Windows, allowing multiple users to share a single Windows 10/11 virtual machine (VM), reducing the cost and complexity of deploying virtual desktops. Additionally, it supports single-session Windows 10/11 and Windows Server operating systems for specific use cases.

## App Virtualization

Azure Virtual Desktop allows you to virtualize and deliver applications to remote users independently of their virtual desktops. This enables you to manage and update applications centrally, while users can access them from their virtual desktops or other devices, such as PCs, tablets, or smartphones.

## Persistent and Non-Persistent Desktops

Azure Virtual Desktop supports both persistent and non-persistent virtual desktops. Persistent desktops maintain user data and settings between sessions, while non-persistent desktops reset to a clean state after each session. Non-persistent desktops are ideal for scenarios where users don't require a personalized desktop environment and can help reduce storage and management overhead.

## Profile Management

Azure Virtual Desktop integrates with FSLogix, a profile container solution that improves the user experience and performance of virtual desktops. FSLogix stores user profiles, including application settings and user data, in a separate container, which is attached to the virtual desktop during the user's session. This ensures a consistent and responsive user experience across sessions and devices.

## Security and Compliance

Azure Virtual Desktop leverages Azure's security features, such as Azure Active Directory (AAD) for authentication and access control, Azure Security Center for threat detection and monitoring, and Azure Private Link for secure, private network connectivity. Additionally, it supports various compliance standards, such as GDPR, HIPAA, and FedRAMP.

## Networking and Connectivity

Azure Virtual Desktop uses Azure Virtual Network (VNet) and Azure ExpressRoute for secure, high-performance network connectivity between virtual desktops and other Azure resources, as well as on-premises networks. It also supports Azure Load Balancer and Azure Traffic Manager for load balancing and traffic distribution.

## Monitoring and Management

Azure Virtual Desktop integrates with Azure Monitor for logging, monitoring, and troubleshooting, and provides a management portal for managing virtual desktops, applications, and user assignments. You can also use PowerShell, REST APIs, and SDKs for automation and integration with other tools and services.

## Cost Optimization

Azure Virtual Desktop offers flexible pricing options, such as pay-as-you-go and reserved instances, to help optimize costs based on your usage patterns. It also supports Azure Hybrid Benefit and Windows Server Subscriptions, allowing you to reuse your existing Windows licenses and reduce the cost of virtual desktops.