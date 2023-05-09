# Kubernetes Security

## Role-Based Access Control (RBAC)

Role-Based Access Control (RBAC) is a mechanism in Kubernetes for managing access to resources based on roles and permissions. RBAC allows you to define fine-grained permissions for users, groups, or service accounts, controlling their access to the Kubernetes API.

RBAC uses the following main resources:

- Role: Defines permissions for resources within a single namespace.
- ClusterRole: Defines permissions for resources across the entire cluster.
- RoleBinding: Binds a Role to a set of users, groups, or service accounts within a namespace.
- ClusterRoleBinding: Binds a ClusterRole to a set of users, groups, or service accounts across the entire cluster.

Here's an example of a Role and RoleBinding configuration:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: my-namespace
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: my-namespace
subjects:
- kind: User
  name: my-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

In this example, a Role named `pod-reader` is created in the `my-namespace` namespace, granting permissions to read Pod resources. The RoleBinding `read-pods` binds the `pod-reader` Role to a user named `my-user`.

## Network policies

Network policies in Kubernetes are used to control traffic between Pods and their access to external resources. By default, Pods within a cluster can communicate with each other without restrictions. Network policies allow you to restrict or allow specific types of traffic based on the Pod's labels and the source or destination IP addresses.

Here's an example of a NetworkPolicy configuration:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-network-policy
  namespace: my-namespace
spec:
  podSelector:
    matchLabels:
      app: my-app
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: another-app
    ports:
    - protocol: TCP
      port: 80
```

In this example, a NetworkPolicy is created in the `my-namespace` namespace. It selects Pods with the label `app: my-app` and allows ingress traffic only from Pods with the label `app: another-app` on TCP port 80.

## Pod security policies

Pod Security Policies (PSPs) are a cluster-level resource that controls security-sensitive aspects of Pod specifications. PSPs enforce restrictions on what a Pod can do, such as running containers as non-root users, restricting the use of privileged containers, and limiting the use of host resources.

Here's an example of a PodSecurityPolicy configuration:

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: my-psp
spec:
  privileged: false
  allowPrivilegeEscalation: false
  runAsUser:
    rule: MustRunAsNonRoot
  seLinux:
    rule: RunAsAny
  fsGroup:
    rule: RunAsAny
  supplementalGroups:
    rule: RunAsAny
  volumes:
  - 'configMap'
  - 'emptyDir'
  - 'secret'
```

In this example, a PodSecurityPolicy named `my-psp` is created. This policy prevents the use of privileged containers and disallows privilege escalation. It enforces that containers must run as non-root users and allows the use of `configMap`, `emptyDir`, and `secret` volume types.

To enforce PodSecurityPolicy, you must enable the PodSecurityPolicy admission controller and grant the necessary permissions via RBAC.

## Secrets management

Kubernetes Secrets are used to manage sensitive data, such as API keys, passwords, and certificates. Secrets can be stored as key-value pairs and are encrypted at rest when stored in etcd. They can be mounted as files or exposed as environment variables to the containers running in a Pod.

Here's an example of creating a Secret:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=  # Base64 encoded value
  password: cGFzc3dvcmQ=  # Base64 encoded value
```

In this example, a Secret named `my-secret` is created with the `username` and `password` key-value pairs. The values are base64 encoded.

To use the Secret in a Pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: my-image
    env:
    - name: USERNAME
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: username
    - name: PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: password
    volumeMounts:
    - name: secret-volume
      mountPath: "/etc/secret"
  volumes:
  - name: secret-volume
    secret:
      secretName: my-secret
```

In this example, the `my-secret` Secret is exposed to the `my-container` container as environment variables and mounted as a volume at `/etc/secret`.

   e. Security best practices

Here are some Kubernetes security best practices:

1. Use Role-Based Access Control (RBAC) to limit access to resources and enforce the principle of least privilege.
2. Implement network policies to control traffic between Pods and limit exposure to potential threats.
3. Use PodSecurityPolicies to enforce security best practices for Pods.
4. Store sensitive data in Secrets and limit access to them using RBAC.
5. Keep your Kubernetes components and applications up-to-date with the latest security patches.
6. Run containers as non-root users and avoid using privileged containers whenever possible.
7. Enable and configure admission controllers to enforce security policies and validate requests.
8. Use third-party tools like kube-bench or Kubesec to audit your cluster's security configuration.
9. Regularly monitor logs and metrics for signs of security incidents or performance issues.
10. Implement proper resource and request limits to protect your cluster from resource exhaustion and denial-of-service attacks.
