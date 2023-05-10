# Azure Load Balancer

Azure Load Balancer is a fully managed, Layer 4 (TCP/UDP) load balancing service that ensures high availability and reliability for your applications by distributing incoming traffic among healthy backend instances. Here's an overview of its key aspects:

## Types of Load Balancers

Azure Load Balancer comes in two types: Public and Internal. A Public Load Balancer distributes traffic originating from the internet to backend instances, while an Internal Load Balancer distributes traffic within a virtual network or between on-premises networks and Azure.

## Frontend IP Configuration

The frontend IP configuration defines the incoming traffic's entry point. A Public Load Balancer uses a public IP address, while an Internal Load Balancer uses a private IP address from the virtual network.

## Backend Pool

The backend pool consists of the instances (Virtual Machines) that receive the incoming traffic. You can configure the backend pool using VMs or VM scale sets.

## Health Probes

Health probes are used to monitor the health of backend instances. The Load Balancer sends periodic requests to the instances and, based on their responses, determines if they are healthy or not. Unhealthy instances are automatically removed from the backend pool.

## Load Balancing Rules

Load balancing rules define how incoming traffic should be distributed among the backend instances. You can configure rules based on the protocol (TCP/UDP), frontend and backend port numbers, and the distribution algorithm (hash-based or round-robin).

## Session Persistence

Azure Load Balancer supports session persistence, which maintains client connections to the same backend instance during a session. You can configure session persistence based on client IP or client IP and protocol.

## NAT Rules

Network Address Translation (NAT) rules allow you to forward incoming traffic to specific backend instances based on the destination port number. This is useful for scenarios like accessing backend VMs via Remote Desktop Protocol (RDP) or Secure Shell (SSH).

## High Availability Ports

High Availability Ports is a feature that enables you to load balance traffic across all ports simultaneously, without creating separate rules for each port. This is useful for scenarios that involve multiple ports or dynamic port ranges.

## Integration with Azure Services

Azure Load Balancer integrates with other Azure services, such as [Virtual Machines](../Compute/Azure%20Virtual%20Machines.md), [Virtual Machine Scale Sets](../Scalability%20and%20Performance/Scale%20Sets.md), and [Virtual Networks](./Azure%20Virtual%20Network.md), enabling you to build highly available and scalable applications.

## Monitoring and Diagnostics

Azure Load Balancer provides monitoring and diagnostics capabilities using [Azure Monitor](../Monitoring%20and%20Management/Azure%20Monitor.md), Azure Network Watcher, and logs. You can use these tools to analyze performance, troubleshoot issues, and optimize your load balancer configuration.