# Kubernetes ConfigMap

## What is a ConfigMap?

A **ConfigMap** is a Kubernetes object used to store **non-sensitive configuration data** as key-value pairs. It helps separate application configuration from the application code, making configuration changes easier without modifying the application image.

---

# Why ConfigMap?

### Without ConfigMap

```yaml
env:
- name: DB_HOST
  value: mysql.default.svc.cluster.local
```

If the database host changes, you must edit every Deployment YAML.

### With ConfigMap

```yaml
env:
- name: DB_HOST
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: DB_HOST
```

Only the ConfigMap needs updating.

---

# ConfigMap Architecture

```text
Application
      │
      ▼
ConfigMap
      │
      ▼
Configuration Data
```

---

# Create ConfigMap (Imperative)

```bash
kubectl create configmap app-config \
--from-literal=APP_ENV=dev \
--from-literal=DB_HOST=mysql.default.svc.cluster.local \
--from-literal=PORT=8080
```

### Verify

```bash
kubectl get configmap

kubectl describe configmap app-config
```

---

# Create ConfigMap (YAML)

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: app-config

data:
  APP_ENV: dev
  DB_HOST: mysql.default.svc.cluster.local
  PORT: "8080"
```

### Apply

```bash
kubectl apply -f configmap.yaml
```

---

# Using ConfigMap as Environment Variables

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx-config

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

---

# Using Entire ConfigMap

```yaml
envFrom:
- configMapRef:
    name: app-config
```

All keys become environment variables.

---

# ConfigMap as Volume

```yaml
volumes:
- name: config-volume
  configMap:
    name: app-config

containers:
- name: nginx
  image: nginx

  volumeMounts:
  - name: config-volume
    mountPath: /etc/config
```

### Verify

```bash
kubectl exec -it nginx-config -- ls /etc/config

kubectl exec -it nginx-config -- cat /etc/config/APP_ENV
```

---

# Examples of ConfigMap Data

- Application Name
- Environment (Dev, Test, Prod)
- Database Host
- Database Port
- API URLs
- Feature Flags

---

# Why do we use ConfigMap?

- Stores application configuration separately.
- No need to rebuild the Docker image when configuration changes.
- Easy to manage environment-specific settings.
- Same application image can be used across Dev, Test, and Production.

---

# Kubernetes Secret

## What is a Secret?

A **Secret** is a Kubernetes object used to store **sensitive information securely**.

Secrets should be used instead of ConfigMaps for confidential data.

---

# Why Secret?

Instead of:

```yaml
DB_PASSWORD: admin123
```

Use:

```yaml
DB_PASSWORD:
  valueFrom:
    secretKeyRef:
```

---

# Secret Architecture

```text
Application
      │
      ▼
Secret
      │
      ▼
Password / API Key
```

---

# Create Secret (Imperative)

```bash
kubectl create secret generic db-secret \
--from-literal=username=admin \
--from-literal=password=admin123
```

---

# Verify Secret

```bash
kubectl get secret

kubectl describe secret db-secret

kubectl get secret db-secret -o yaml
```

Values are Base64 encoded.

---

# Create Secret (YAML)

Generate Base64 values:

```bash
echo -n admin | base64

echo -n admin123 | base64
```

Example:

```text
admin      → YWRtaW4=
admin123   → YWRtaW4xMjM=
```

YAML

```yaml
apiVersion: v1
kind: Secret

metadata:
  name: db-secret

type: Opaque

data:
  username: YWRtaW4=
  password: YWRtaW4xMjM=
```

---

# Secret as Environment Variables

```yaml
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

# Secret as Volume

```yaml
volumes:
- name: secret-volume
  secret:
    secretName: db-secret

containers:
- name: app
  image: nginx

  volumeMounts:
  - name: secret-volume
    mountPath: /etc/secret
```

### Verify

```bash
kubectl exec -it app -- ls /etc/secret

kubectl exec -it app -- cat /etc/secret/password
```

---

# Examples of Secret Data

- Database Username
- Database Password
- API Keys
- Tokens
- SSL Certificates
- SSH Keys

---

# Why do we use Secret?

- Keeps sensitive information separate from application code.
- Prevents passwords from being stored directly in Pod or Deployment YAML.
- Supports mounting as environment variables or files.
- Improves application security.

---

# ConfigMap vs Secret

| ConfigMap | Secret |
|------------|---------|
| Stores non-sensitive data | Stores sensitive data |
| Plain text values | Base64 encoded values (when using `data`) |
| Used for configuration | Used for passwords, tokens, certificates |
| Stores URLs, ports, hostnames | Stores credentials and confidential data |
| Can be mounted as Environment Variables | Can be mounted as Environment Variables |
| Can be mounted as Volumes | Can be mounted as Volumes |

---

# Interview Points

## ConfigMap

- Used to store application configuration.
- Stores non-sensitive data.
- Easy to update configuration.
- Used for hostnames, ports, URLs, and environment names.

## Secret

- Used to store sensitive information.
- Stores passwords, API keys, tokens, and certificates.
- Values in the `data` field are Base64 encoded.
- Base64 is encoding, **not encryption**.

---

# What Issues Can Occur with ConfigMaps in Kubernetes?

- Configuration changes are **not automatically reflected** in running Pods when used as environment variables. Pods usually need to be restarted.
- Storing sensitive data in ConfigMaps is **not secure** because the data is stored in plain text.
- If a ConfigMap is deleted accidentally, dependent Pods may fail to start or function correctly.
- Large ConfigMaps can impact etcd storage and cluster performance.
- Updating a ConfigMap does not automatically trigger a Deployment rollout.
- Missing or incorrect ConfigMap references can cause Pod startup failures.
- Version management becomes difficult if ConfigMaps are modified manually without tracking changes.
- Environment-specific ConfigMaps (Dev, Test, Prod) must be managed carefully to avoid configuration mismatches.

---

# Best Practice

- Use **ConfigMap** for non-sensitive configuration.
- Use **Secret** for sensitive information.
- Never store passwords in ConfigMaps.
- Restart or roll out Pods after updating ConfigMaps if environment variables are used.
- Enable **Encryption at Rest** for Kubernetes Secrets in production.
- Use RBAC to restrict access to ConfigMaps and Secrets.
