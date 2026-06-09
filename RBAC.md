# Kubernetes RBAC Interview Notes

## RBAC

Role --> Which resource & What action.
RoleBinding --> Role (pod resource) & whom user (developer1).

RBAC stands for Role-Based Access Control used to control who can access cluster resources and what actions they can perform.

If any requirement comes from development side, they want to view the access of pods and deployments.

So, we use RBAC.

In RBAC, we define Role or ClusterRole & RoleBinding or ClusterRoleBinding.

In role, we configure Which resources & What actions they can perform & in RoleBinding
we map the Role to users, service accounts.

## Allow access to Pods and Services

Permissions: apiGroups: [""]

- Get --> View a specific pod/service
- List --> View all pods/services
- Watch --> Continuously monitor changes

## Allow access to Deployments

Permissions: apiGroups: ["apps"]

- Get --> View a deployment
- List --> View all deployments
- Watch --> Monitor deployment changes

## Create a Role

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: ecgcbackend
  name: developer-role

rules:
- apiGroups: [""]
  resources: ["pods","services"]
  verbs: ["get","list","watch"]

- apiGroups: ["apps"]
  resources: ["deployments"]
  verbs: ["get","list","watch"]

kubectl apply -f role.yaml

## Create a RoleBinding

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: developer-binding
  namespace: ecgcbackend

subjects:
- kind: User
  name: developer1

roleRef:
  kind: Role
  name: developer-role
  apiGroup: rbac.authorization.k8s.io

## Cluster-Wide Access

If you want access across all namespaces, use ClusterRole and ClusterRoleBinding.

## ClusterRole

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: readonly-role

rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get","list","watch"]

## ClusterRoleBinding

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: readonly-binding

subjects:
- kind: User
  name: developer1

roleRef:
  kind: ClusterRole
  name: readonly-role
  apiGroup: rbac.authorization.k8s.io

## Difference Between Role, RoleBinding, ClusterRole and ClusterRoleBinding

If we want to give access only within a specific namespace, we use Role and RoleBinding.

If we want to give access across all namespaces in the cluster, we use ClusterRole and ClusterRoleBinding.

## Create a Service Account

apiVersion: v1
kind: ServiceAccount
metadata:
  name: jenkins-sa
  namespace: dev
