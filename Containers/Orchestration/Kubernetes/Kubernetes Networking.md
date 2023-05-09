# Kubernetes Networking

## Pod networking

Each Pod in a Kubernetes cluster is assigned a unique IP address, which allows Pods to communicate with each other directly. The IP addresses are managed by the Container Network Interface (CNI) plugins, which are responsible for allocating IPs and configuring the network stack for each Pod. Some popular CNI plugins include Calico, Flannel, and Weave.

The CNI ensures that the Pod's IP is routable within the cluster and enables communication between Pods across different nodes. It also ensures that Pods can communicate with the cluster's control plane components, such as the API server and kubelet.

## Service discovery and DNS

Kubernetes provides built-in service discovery using DNS. Services are assigned a DNS name that follows the pattern `<service-name>.<namespace>.svc.cluster.local`. By default, Pods in the same namespace can access the Service using the short name `<service-name>`. Pods in other namespaces need to use the fully-qualified domain name (FQDN).

When a Pod tries to access a Service using its DNS name, the cluster's internal DNS server (usually CoreDNS) resolves the name to the Service's cluster IP. The cluster IP is a virtual IP that forwards traffic to the Pods backing the Service, distributing load among the available Pods.

## Network policies

Network policies allow you to define rules that control the flow of traffic between Pods. They act as a firewall, allowing you to permit or deny traffic based on the source and destination IP addresses, ports, and labels.

To use network policies, you need to have a CNI plugin that supports this feature, such as Calico or Cilium. To create a network policy, you define a `NetworkPolicy` resource with the desired rules. For example:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: trusted-app
    ports:
    - protocol: TCP
      port: 80
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: external-service
    ports:
    - protocol: TCP
      port: 443
```

This example policy allows incoming traffic to Pods with the label `app: my-app` only from Pods with the label `app: trusted-app` on port 80. It also allows outgoing traffic from the selected Pods to Pods with the label `app: external-service` on port 443.

## Ingress controllers

Ingress controllers manage external access to the services in a cluster. They provide features like load balancing, SSL/TLS termination, and path-based routing. An Ingress resource defines the rules for routing external traffic to the appropriate Services.

Ingress controllers are implemented as a separate component in the cluster, such as Nginx, HAProxy, or Traefik. When you create an Ingress resource, the Ingress controller watches for changes and updates its configuration accordingly.

Here's an example of an Ingress resource:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: my-app.example.com
    http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: my-api-service
            port:
              number: 80
      - path: /web
        pathType: Prefix
        backend:
          service:
            name: my-web-service
            port:
              number: 80
  tls:
  - hosts:
    - my-app.example.com
    secretName: my-tls-secret
```

In this example, the Ingress resource routes external traffic to two different services based on the requested path. Requests to `my-app.example.com/api` are directed to the `my-api-service` on port 80, while requests to `my-app.example.com/web` are directed to the `my-web-service` on port 80. The Ingress also specifies a TLS configuration using the `my-tls-secret` Secret, which allows SSL/TLS termination at the Ingress controller.