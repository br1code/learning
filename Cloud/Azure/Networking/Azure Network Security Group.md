# Azure Network Security Group

Azure Network Security Group (NSG) is a security feature that allows you to control network traffic to and from Azure resources within a virtual network. NSGs act as a firewall, enabling you to define rules that permit or deny traffic based on factors like source and destination IP addresses, ports, and protocols. Here's an overview of its key aspects:

## Security Rules

NSG rules consist of the following components:
   - Direction: Inbound or outbound.
   - Priority: A numerical value between 100 and 4096, with lower values having higher priority.
   - Source/Destination: IP address, CIDR block, or service tag (predefined groups of IP addresses).
   - Protocol: TCP, UDP, or Any.
   - Source/Destination Port Range: Single port or range of ports.
   - Action: Allow or Deny.

## Default Rules

Azure NSGs include several default rules that cannot be deleted but can be overridden by creating custom rules with higher priority:
   - AllowVNetInBound: Allows all inbound traffic within the virtual network.
   - AllowAzureLoadBalancerInBound: Allows inbound traffic from Azure Load Balancer.
   - DenyAllInBound: Denies all other inbound traffic.
   - AllowVNetOutBound: Allows all outbound traffic within the virtual network.
   - AllowInternetOutBound: Allows outbound traffic to the internet.
   - DenyAllOutBound: Denies all other outbound traffic.

## Rule Processing

When processing traffic, NSGs evaluate rules in priority order (lowest to highest). The first matching rule is applied, and if no match is found, traffic is denied by default.

## Association

NSGs can be associated with either a subnet or a network interface. When associated with a subnet, the NSG rules apply to all resources within that subnet. When associated with a network interface, the rules apply only to the specific resource attached to that interface.

## Multiple NSGs

You can have multiple NSGs associated with a resource, such as one at the subnet level and another at the network interface level. In this case, both sets of rules are processed, starting with the subnet-level rules followed by the network interface-level rules.

## Service Tags

Azure provides service tags, which are predefined groups of IP addresses representing Azure services or specific regions. You can use service tags to simplify rule creation and management.

## Augmented Security Rules

Augmented security rules allow you to define larger, more complex rules by specifying multiple source or destination IP addresses, ports, or service tags in a single rule.

## Monitoring and Diagnostics

Azure NSGs provide monitoring and diagnostics capabilities using [Azure Monitor](../Monitoring%20and%20Management/Azure%20Monitor.md), Network Watcher, and diagnostic logs. You can use these tools to analyze traffic patterns, troubleshoot issues, and optimize your NSG configuration.