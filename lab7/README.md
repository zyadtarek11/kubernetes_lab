# lab7

Q1) What is RBAC in Kubernetes, and why is it important?

Role-Based Access Control (RBAC) is a method for regulating access to computer or network resources based on the roles of individual users within an enterprise. In Kubernetes, RBAC is important for enforcing policies and ensuring that users and applications have only the permissions they need.

Q2) What are the main components of RBAC in Kubernetes? Describe the purpose of each.

- Role: Defines a set of permissions within a namespace.
- ClusterRole: Defines a set of permissions cluster-wide.
- RoleBinding: Grants permissions defined in a Role to a user or set of users within a namespace.
- ClusterRoleBinding: Grants permissions defined in a ClusterRole to a user or set of users cluster-wide.

Q3) How do Roles differ from ClusterRoles in Kubernetes?

Roles are namespace-specific, while ClusterRoles are cluster-wide and can be used in any namespace.

Q4) What is a RoleBinding in Kubernetes, and how does it differ from a ClusterRoleBinding?

A RoleBinding grants permissions defined in a Role to a user or set of users within a specific namespace. A ClusterRoleBinding grants permissions defined in a ClusterRole to a user or set of users across the entire cluster.

Q5) How can you list all the Roles and RoleBindings in a specific namespace?

Use the following commands:
```
kubectl get roles -n <namespace>
kubectl get rolebindings -n <namespace>
```

Q6) How do you create a Role that allows a user to read secrets only in a specific namespace? Provide a YAML example.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: <namespace>
    name: read-secrets
rules:
- apiGroups: [""]
    resources: ["secrets"]
    verbs: ["get", "list"]
```

Q7) Explain how ClusterRoleBindings can be used to grant permissions across the entire Kubernetes cluster.

ClusterRoleBindings bind ClusterRoles to users, groups, or service accounts, granting them permissions across all namespaces in the cluster.

Q8) What are Subjects in RBAC, and what types of subjects can be used in RoleBindings or ClusterRoleBindings?

Subjects are entities that can be granted permissions. Types of subjects include Users, Groups, and ServiceAccounts.

Q9) How can you check the permissions of a particular user or service account in a Kubernetes cluster?

Use the `kubectl auth can-i` command:
```
kubectl auth can-i <verb> <resource> --as=<user>
```

Q10) What is the significance of the aggregate-to-admin, aggregate-to-edit, and aggregate-to-view labels in Kubernetes RBAC?

These labels are used to aggregate multiple ClusterRoles into a single role, simplifying the management of permissions.

Q11) How does Kubernetes RBAC integrate with external identity providers (e.g., LDAP, OIDC)?

Kubernetes can integrate with external identity providers using authentication plugins, allowing users to authenticate with their existing credentials.

Q12) How do you troubleshoot RBAC permission errors in Kubernetes? What are some common issues?

Check Role and RoleBinding configurations, ensure the correct namespace is specified, and use `kubectl auth can-i` to verify permissions.

Q13) Explain how to grant temporary elevated privileges to a user in Kubernetes. What are the security implications?

Create a temporary RoleBinding or ClusterRoleBinding. Security implications include the risk of privilege escalation and potential misuse of elevated permissions.

Q14) What is the difference between using RBAC and Kubernetes' native ServiceAccount permissions for pods?

RBAC provides fine-grained access control, while ServiceAccount permissions are more coarse-grained and typically used for pod-level access.

Q15) How can you restrict access to certain Kubernetes API groups or resources using RBAC? Provide a YAML example.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: <namespace>
    name: restricted-access
rules:
- apiGroups: ["<apiGroup>"]
    resources: ["<resource>"]
    verbs: ["<verb>"]
```

Q16) Write a YAML definition for a ClusterRole that allows listing all pods in any namespace.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
    name: list-pods
rules:
- apiGroups: [""]
    resources: ["pods"]
    verbs: ["list"]
```

Q17) Create a Role that allows a user to create, update, and delete deployments only within the dev namespace.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: dev
    name: manage-deployments
rules:
- apiGroups: ["apps"]
    resources: ["deployments"]
    verbs: ["create", "update", "delete"]
```

Q18) Set up a RoleBinding that assigns the view role to a user named john in the testing namespace. Provide the YAML.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: view-binding
    namespace: testing
subjects:
- kind: User
    name: john
    apiGroup: rbac.authorization.k8s.io
roleRef:
    kind: Role
    name: view
    apiGroup: rbac.authorization.k8s.io
```

Q19) Deploy a ClusterRoleBinding that grants the edit role to a service account named developer in all namespaces.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
    name: edit-binding
subjects:
- kind: ServiceAccount
    name: developer
    namespace: default
roleRef:
    kind: ClusterRole
    name: edit
    apiGroup: rbac.authorization.k8s.io
```

Q20) Write a command to check if a user named alice has permission to delete pods in the production namespace.

```
kubectl auth can-i delete pods --namespace=production --as=alice
```

Q21) Write a YAML definition for a Role named pod-executor in the ci-cd namespace that allows creating, listing, and executing Pods. Bind this role to a service account named pipeline-sa using a RoleBinding.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: ci-cd
    name: pod-executor
rules:
- apiGroups: [""]
    resources: ["pods"]
    verbs: ["create", "list", "exec"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: pod-executor-binding
    namespace: ci-cd
subjects:
- kind: ServiceAccount
    name: pipeline-sa
    namespace: ci-cd
roleRef:
    kind: Role
    name: pod-executor
    apiGroup: rbac.authorization.k8s.io
```

Q22) Create a Role named "persistent-volume-access" in the storage namespace that grants permissions to create and delete PersistentVolumeClaims. Assign this role to a service account named storage-admin using a RoleBinding.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
    namespace: storage
    name: persistent-volume-access
rules:
- apiGroups: [""]
    resources: ["persistentvolumeclaims"]
    verbs: ["create", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
    name: persistent-volume-access-binding
    namespace: storage
subjects:
- kind: ServiceAccount
    name: storage-admin
    namespace: storage
roleRef:
    kind: Role
    name: persistent-volume-access
    apiGroup: rbac.authorization.k8s.io
```

Q23) Create a ClusterRole named readonly-cluster that grants read-only access to all resources across the cluster. Then, create a ClusterRoleBinding that assigns this role to a user named alice.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
    name: readonly-cluster
rules:
- apiGroups: [""]
    resources: ["*"]
    verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
    name: readonly-cluster-binding
subjects:
- kind: User
    name: alice
    apiGroup: rbac.authorization.k8s.io
roleRef:
    kind: ClusterRole
    name: readonly-cluster
    apiGroup: rbac.authorization.k8s.io
```