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
