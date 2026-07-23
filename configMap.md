# Kubernetes ConfigMap

## What is a ConfigMap?

A **ConfigMap** is used to store **non-sensitive configuration data** in Kubernetes.

### Examples

- Application Name
- Environment
- Database Host
- Database Port
- URLs
- Feature Flags

---

## ConfigMap YAML

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config
  namespace: demo

data:
  APP_NAME: nginx
  APP_ENV: dev
  DB_HOST: mysql
  DB_PORT: "3306"
```

Apply

```bash
kubectl apply -f configmap.yaml
```

Verify

```bash
kubectl get configmap -n demo
kubectl describe configmap app-config -n demo
```

---

## Use ConfigMap in Pod

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx
  namespace: demo

spec:
  containers:
  - name: nginx
    image: nginx

    env:
    - name: APP_ENV
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_ENV

    - name: DB_HOST
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: DB_HOST
```
