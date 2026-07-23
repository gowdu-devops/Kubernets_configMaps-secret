# Kubernetes Secret

## What is a Secret?

A **Secret** is used to store **sensitive information**.

### Examples

- Passwords
- API Keys
- Tokens
- Certificates
- SSH Keys

---

## Encode Values

```bash
echo -n admin | base64
```

Output

```text
YWRtaW4=
```

```bash
echo -n Admin@123 | base64
```

Output

```text
QWRtaW5AMTIz
```

---

## Secret YAML

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: db-secret
  namespace: demo

type: Opaque

data:
  username: YWRtaW4=
  password: QWRtaW5AMTIz
```

Apply

```bash
kubectl apply -f secret.yaml
```

Verify

```bash
kubectl get secret -n demo
kubectl describe secret db-secret -n demo
```

---

## Use Secret in Pod

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

    - name: DB_USER
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: username

    - name: DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-secret
          key: password
```

---

# Verify Inside the Pod

```bash
kubectl exec -it nginx -n demo -- env
```

Expected Output

```text
APP_NAME=nginx
APP_ENV=dev
DB_HOST=mysql
DB_PORT=3306
DB_USER=admin
DB_PASSWORD=Admin@123
```
