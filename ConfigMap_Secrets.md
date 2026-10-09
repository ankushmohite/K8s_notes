# Kubernetes ConfigMap and Secret

## 1. ConfigMap

A ConfigMap is used to store non-sensitive data, such as application username and environment variables.

**Manifest file (`configmap.yaml`)**

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_USERNAME: admin
  APP_ENV: production
```

## 2. Secret

A Secret is used to store sensitive information, such as passwords, tokens, and database credentials.

**Manifest file (`secret.yaml`)**

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
stringData:
  DB_USERNAME: dbadmin
  DB_PASSWORD: change-me
```

## Apply the Manifests

```bash
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
```
