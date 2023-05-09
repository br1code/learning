# Kubernetes Core Concepts

## Nodes and clusters

A node is a worker machine in a Kubernetes cluster that hosts containerized applications. Nodes can be physical machines or virtual machines. A cluster is a group of nodes working together to provide a highly available and fault-tolerant environment for running containerized applications. The cluster's control plane manages the nodes and ensures that the desired state of the applications is maintained.

## Kubernetes API and API objects

The Kubernetes API is a RESTful interface that allows you to create, update, and delete Kubernetes objects. API objects represent the desired state of your cluster and include resources like pods, services, and deployments. The API server is the central component that processes and validates RESTful requests and updates the corresponding objects in the underlying data store (etcd).

## Pods, ReplicaSets, and Deployments

- Pods: A pod is the smallest deployable unit in Kubernetes and can contain one or more containers. Containers within a pod share the same network namespace and can communicate using `localhost`.

- ReplicaSets: A ReplicaSet is a higher-level abstraction that ensures a specified number of pod replicas are running at any given time. If a pod fails, the ReplicaSet will create a new one to maintain the desired count. ReplicaSets use label selectors to identify the pods they manage.

- Deployments: A Deployment is an even higher-level abstraction that manages ReplicaSets and provides declarative updates for pods. Deployments allow you to easily perform rolling updates, rollbacks, and scale applications.

## Services and Load Balancers

- Services: A Kubernetes service is an abstraction that defines a logical set of pods and a policy to access them. It provides a stable IP address and DNS name, allowing communication between different components in the cluster and exposing applications to external clients.

- Load Balancers: Load balancers distribute network traffic across multiple pods. In Kubernetes, you can create a service of type LoadBalancer to provision an external load balancer for your application, distributing traffic among the selected pods.

## ConfigMaps and Secrets

- ConfigMaps: ConfigMaps are Kubernetes objects that store non-sensitive configuration data. They allow you to decouple configuration from the container image, making it easier to manage and update. ConfigMaps can be mounted as volumes or exposed as environment variables to containers.

- Secrets: Secrets are Kubernetes objects that store sensitive data, such as credentials and API keys. Like ConfigMaps, Secrets can be mounted as volumes or exposed as environment variables to containers, but they are encrypted at rest and in transit.

## Persistent Volumes and Persistent Volume Claims

- Persistent Volumes (PV): PVs are used to represent physical storage resources in a cluster. They can be provisioned manually by an administrator or dynamically using StorageClasses.

- Persistent Volume Claims (PVC): PVCs are requests for storage by users, specifying the storage size and access mode. Kubernetes binds PVCs to PVs based on their specifications, and the resulting volume can be mounted to a pod.

## Ingress

Ingress is a Kubernetes component that manages external access to the services in a cluster. It provides load balancing, SSL termination, and name-based virtual hosting. Ingress controllers, such as NGINX or HAProxy, are responsible for implementing the Ingress rules.

## Namespaces

Namespaces are used to divide cluster resources among multiple users or projects. They provide a scope for resource names, allowing you to create resources with the same name in different namespaces. Some resources, such as nodes and persistent volumes, are not namespaced and are shared across the entire cluster.

Examples:

- To create a namespace, you can use a YAML file:

```yaml
apiVersion: v1
kind: Namespace
metadata:
    name: my-namespace
```

Then apply the YAML file using `kubectl`:

```bash
kubectl apply -f namespace.yaml
```

- To create a Deployment in a specific namespace:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  namespace: my-namespace
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image
        ports:
        - containerPort: 80
```

- To create a service in the same namespace:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: my-namespace
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```

To interact with Kubernetes resources in a specific namespace, you can use the `--namespace` flag with `kubectl`:

```bash
kubectl get pods --namespace my-namespace
```