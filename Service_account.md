# Kubernetes Service Account & RBAC Configuration for Lens Access

## What is a Service Account?

A Service Account is a Kubernetes identity used by applications, pods, or automation tools to communicate with the Kubernetes API Server.

In our ECGC project, we configured Lens access using a Service Account. We created a Service Account, assigned required permissions through RBAC, then generated a token, and added that token to a kubeconfig file. Then we imported the kubeconfig file into Lens, so users could securely access the Kubernetes cluster without sharing admin credentials.

---

# 1. Create Service Account

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: lens-user
  namespace: kube-system
```

Apply the Service Account:

```bash
kubectl apply -f lens-sa.yaml
```

---

# 2. Create ClusterRole

If you want read-only access to all namespaces:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: lens-readonly

rules:
- apiGroups: [""]
  resources:
    - pods
    - services
    - endpoints
    - configmaps
    - namespaces
    - nodes
    - persistentvolumeclaims
    - persistentvolumes
  verbs:
    - get
    - list
    - watch

- apiGroups: ["apps"]
  resources:
    - deployments
    - replicasets
    - daemonsets
    - statefulsets
  verbs:
    - get
    - list
    - watch

- apiGroups: ["batch"]
  resources:
    - jobs
    - cronjobs
  verbs:
    - get
    - list
    - watch

- apiGroups: ["networking.k8s.io"]
  resources:
    - ingresses
  verbs:
    - get
    - list
    - watch
```

Apply the ClusterRole:

```bash
kubectl apply -f lens-role.yaml
```

---

# 3. Create ClusterRoleBinding

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: lens-readonly-binding

subjects:
- kind: ServiceAccount
  name: lens-user
  namespace: kube-system

roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: lens-readonly
```

Apply the ClusterRoleBinding:

```bash
kubectl apply -f lens-binding.yaml
```

---

# 4. Generate Token

```bash
kubectl create token lens-user \
-n kube-system \
--duration=8760h
```

Copy the generated token.

---

# 5. Get CA Data

```bash
kubectl config view --raw --minify \
-o jsonpath='{.clusters[0].cluster.certificate-authority-data}'
```

Copy the complete CA data output.

---

# 6. Create Kubeconfig for Lens

```yaml
apiVersion: v1
kind: Config

clusters:
- name: neia-uat
  cluster:
    server: https://10.10.15.15:6443
    certificate-authority-data: <FULL_CA_DATA>

contexts:
- name: lens-user-context
  context:
    cluster: neia-uat
    user: lens-user

current-context: lens-user-context

users:
- name: lens-user
  user:
    token: <TOKEN>
```

Replace:

* `<FULL_CA_DATA>` with the CA data obtained in Step 5.
* `<TOKEN>` with the token generated in Step 4.

---

# 7. Validate Kubeconfig

```bash
kubectl --kubeconfig=lens-user-config.yaml get ns
```

Expected output:

```text
NAME
default
kube-system
monitoring
...
```

---

# 8. Import Cluster into Lens

1. Open Lens Desktop.
2. Click **Add Cluster**.
3. Select **Browse Kubeconfig**.
4. Choose `lens-user-config.yaml`.
5. Click **Add Cluster**.

The cluster should now be accessible from Lens.

---

# Verification Commands

Check Service Account:

```bash
kubectl get sa lens-user -n kube-system
```

Check ClusterRole:

```bash
kubectl get clusterrole lens-readonly
```

Check ClusterRoleBinding:

```bash
kubectl get clusterrolebinding lens-readonly-binding
```

Check Permissions:

```bash
kubectl auth can-i list pods \
--as=system:serviceaccount:kube-system:lens-user \
-A
```

Expected Output:

```text
yes
```

List all permissions:

```bash
kubectl auth can-i --list \
--as=system:serviceaccount:kube-system:lens-user
```

---

# Conclusion

Using Service Accounts with RBAC is a secure way to provide access to Kubernetes clusters. In our ECGC project, we configured Lens access by creating a Service Account, assigning permissions through RBAC, generating a token, and importing a kubeconfig file into Lens. This approach avoids sharing admin credentials and follows security best practices.
