# Kubernetes Network Policy Implementation (Ingress Only)

## Objective

Implement namespace-level isolation using Kubernetes NetworkPolicies.

Allowed Traffic:

* Ingress Controller → Frontend
* Frontend → Backend
* Backend → Database

All other traffic will be denied.

---

# Step 1: Label Namespaces

```bash
kubectl label namespace ecgcfrontend name=ecgcfrontend

kubectl label namespace ecgcbackend name=ecgcbackend

kubectl label namespace ecgcdatabase name=ecgcdatabase
```

Verify:

```bash
kubectl get ns --show-labels
```

---

# Step 2: Default Deny Ingress

## ecgcfrontend

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
  namespace: ecgcfrontend
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

## ecgcbackend

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
  namespace: ecgcbackend
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

## ecgcdatabase

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
  namespace: ecgcdatabase
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

---

# Step 3: Allow Ingress Controller → Frontend

Namespace: ecgcfrontend

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend-network-policy
  namespace: ecgcfrontend

spec:
  podSelector: {}

  policyTypes:
  - Ingress

  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: ingress-nginx
```

---

# Step 4: Allow Frontend → Backend

Namespace: ecgcbackend

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-network-policy
  namespace: ecgcbackend

spec:
  podSelector: {}

  policyTypes:
  - Ingress

  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: ecgcfrontend
```

---

# Step 5: Allow Backend → Database

Namespace: ecgcdatabase

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: database-network-policy
  namespace: ecgcdatabase

spec:
  podSelector: {}

  policyTypes:
  - Ingress

  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: ecgcbackend
```

---

# Traffic Flow After Applying Policies

```text
Internet
   |
   v
AWS ALB
   |
   v
NGINX Ingress Controller
   |
   v
Frontend Namespace
   |
   v
Backend Namespace
   |
   v
Database Namespace
```

---

# Allowed Traffic

✅ NGINX Ingress Controller → Frontend

✅ Frontend → Backend

✅ Backend → Database

---

# Blocked Traffic

❌ Frontend → Database

❌ Frontend → Other Namespaces

❌ Backend → Frontend

❌ Database → Frontend

❌ Database → Backend

❌ Any Namespace Not Explicitly Allowed

---

# Important Notes

### 1. This is an Ingress-Only Policy

```yaml
policyTypes:
- Ingress
```

No Egress policies are configured.

### 2. DNS Policy Not Required

Since Egress traffic is not restricted, DNS resolution through CoreDNS will continue to work without additional NetworkPolicies.

### 3. Network Policy Support Must Be Enabled

Verify that your EKS cluster uses a NetworkPolicy-capable CNI:

* Amazon VPC CNI with Network Policy enabled
* Calico
* Cilium

Verify:

```bash
kubectl get pods -n kube-system
```

### 4. Production Recommendation

Current policies allow traffic to all pods within the target namespace:

```yaml
podSelector: {}
```

For enhanced security, use pod labels and ports:

```yaml
podSelector:
  matchLabels:
    app: customer-api
```

```yaml
ports:
- protocol: TCP
  port: 8080
```

This follows the principle of least privilege and is recommended for production environments.
