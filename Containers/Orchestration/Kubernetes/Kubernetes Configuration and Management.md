# Kubernetes Configuration and Management

## kubectl command-line tool

`kubectl` is the primary command-line tool for interacting with a Kubernetes cluster. It allows you to create, update, delete, and inspect Kubernetes resources using the Kubernetes API. Some common `kubectl` commands include:

- `kubectl get`: List resources.

Example:

```
kubectl get pods
```

This command lists all the pods in the current namespace.

- `kubectl describe`: Show detailed information about a resource.

Example:

```
kubectl describe pod my-pod
```

This command shows detailed information about the pod named `my-pod`.

- `kubectl create`: Create a resource from a file or from stdin.

Example:

```
kubectl create -f my-namespace.yaml
```

This command creates a namespace using the configuration in the `my-namespace.yaml` file.

- `kubectl apply`: Apply a configuration change to a resource from a file or stdin.

Example:

```
kubectl apply -f my-deployment.yaml
```

This command applies the configuration in the `my-deployment.yaml` file, creating or updating the specified resources.

- `kubectl delete`: Delete resources.

Example:

```
kubectl delete pod my-pod
```

This command deletes the pod named `my-pod`.

- `kubectl logs`: Print the logs for a container in a pod.

Example:

```
kubectl logs my-pod
```

This command prints the logs for the first container in the pod named `my-pod`.

- `kubectl exec`: Execute a command on a container in a pod.

Example:

```
kubectl exec my-pod -- ls /app
```

This command runs `ls /app` in the first container of the pod named `my-pod`.


- `kubectl port-forward`: Forward one or more local ports to a pod.

Example:

```
kubectl port-forward my-pod 8080:80
```

This command forwards local port 8080 to port 80 on the pod named `my-pod`.

- `kubectl rollout`: Manage the rollout of a resource.

Example:

```
kubectl rollout status deployment/my-deployment
```

This command shows the rollout status of the deployment named `my-deployment`.

- `kubectl config`: Modify kubeconfig files.

Example:

```
kubectl config use-context my-context
```

This command switches to the context named `my-context` in the kubeconfig file.

## Kubernetes manifests (YAML files)

Kubernetes manifests are YAML files that describe the desired state of your application's resources. They define the resource type, metadata, and desired configuration. When you apply a manifest, the Kubernetes API server processes it and updates the corresponding objects in the cluster.

## Deploying and managing applications

To deploy an application, create a YAML manifest file describing the desired state of the application's resources, such as Deployments, Services, ConfigMaps, and Secrets. Apply the manifest using `kubectl apply -f <filename.yaml>`. Kubernetes will create or update the resources accordingly.

To manage the application, you can use `kubectl` to inspect and update the resources. For example, you can use `kubectl get pods` to list the running pods or `kubectl describe deployment <deployment-name>` to view detailed information about a Deployment.

## Updating and rolling back applications

To update an application, modify the corresponding YAML manifest file and reapply it with `kubectl apply`. For example, to update a container image, change the image field in the Deployment manifest and apply the change. Kubernetes will perform a rolling update, replacing the old pods with new ones using the updated image.

To roll back an application to a previous version, use the `kubectl rollout undo` command. For example, to roll back a Deployment, run `kubectl rollout undo deployment/<deployment-name>`.

## Scaling applications

To scale an application, update the `replicas` field in the Deployment manifest and apply the change with `kubectl apply`. Alternatively, you can use the `kubectl scale` command, such as `kubectl scale deployment/<deployment-name> --replicas=<number>`.

Kubernetes also supports automatic scaling using the Horizontal Pod Autoscaler (HPA). To configure HPA, create an HPA manifest specifying the target resource, the desired metric, and the target value. Kubernetes will automatically adjust the number of replicas to meet the target value.

## Configuring application environment variables and configuration files

To configure application environment variables, add the `env` field to the container specification in the Deployment manifest. You can define static values or reference ConfigMaps and Secrets.

To configure application configuration files, create a ConfigMap with the desired file content and mount it as a volume in the Deployment manifest. The file will be available to the container at the specified mount path.

## Managing application secrets and sensitive data

To manage sensitive data, such as credentials and API keys, use Kubernetes Secrets. Create a Secret manifest containing the key-value pairs of the sensitive data and apply it with `kubectl apply`. In the Deployment manifest, reference the Secret as an environment variable or mount it as a volume. Kubernetes will encrypt the Secret data at rest and in transit.

Example: Create a Secret with a username and password

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=  # base64 encoded value
  password: cGFzc3dvcmQ=  # base64 encoded value
```

To reference the Secret as environment variables in a Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 1
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
        env:
        - name: MY_USERNAME
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: username
        - name: MY_PASSWORD
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: password
```

To mount the Secret as a volume in a Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 1
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
        volumeMounts:
        - name: secret-volume
          mountPath: "/etc/secrets"
          readOnly: true
      volumes:
      - name: secret-volume
        secret:
          secretName: my-secret
```