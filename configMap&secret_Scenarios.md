# Scenario 1
```bash
## <img width="293" height="27" alt="image" src="https://github.com/user-attachments/assets/95d97ae8-7aad-4229-9bf8-b8b1dba2bbc2" />
```
```bash
## <img width="304" height="52" alt="image" src="https://github.com/user-attachments/assets/88bd619d-aeb8-4827-bd73-87950aab7726" />
```
```bash
## <img width="326" height="130" alt="image" src="https://github.com/user-attachments/assets/81136c0a-c56b-4de5-91a2-385bb378e54e" />
```
```bash
## <img width="287" height="133" alt="image" src="https://github.com/user-attachments/assets/bca22d8c-dd8d-45c6-a6ba-10246972975e" />
```
```bash
## <img width="185" height="457" alt="image" src="https://github.com/user-attachments/assets/3f14a8d2-c49a-4502-8564-a7d3b512977a" />
```
```bash
## <img width="612" height="96" alt="image" src="https://github.com/user-attachments/assets/cf265a92-ff76-44c4-8757-c42075676d1e" />
```

## Delete ConfigMap

kubectl delete cm app-config -n demo
```bash
## <img width="779" height="311" alt="image" src="https://github.com/user-attachments/assets/8de72c26-085d-45e9-afda-3f537d7df240" />
```

## Expected

Existing Pods continue to run.
New Pods that require the ConfigMap may fail to start if they reference missing keys.
```bash
## <img width="716" height="176" alt="image" src="https://github.com/user-attachments/assets/9ff0753d-4efb-4514-974c-2c1048826317" />
```

# Troubleshooting Flow
## 🔍 Step 1: Check the Pod Status

Run the following command:

```bash
kubectl get pods -n config-secret-demo
```

Example Output:

```text
NAME          STATUS                         READY
nginx-pod-2   CreateContainerConfigError     0/1
```

---

## 🔍 Step 2: Describe the Pod (Most Important)

Run the following command:

```bash
kubectl describe pod nginx-pod-2 -n config-secret-demo
```

Go to the **Events** section at the bottom.

Example:

```text
Events:
Warning  Failed     Error: configmap "app-config" not found
```

> **📌 Note:** The **Events** section tells you the exact reason why the Pod failed.

---

## 🔍 Step 3: Check Whether the ConfigMap Exists

Run the following command:

```bash
kubectl get configmap -n config-secret-demo
```

Example:

```text
NAME         DATA   AGE
app-config   4      5m
```

If you don't see **app-config**, it has been deleted.

> **📌 Note:** If the ConfigMap does not exist, recreate it before creating or restarting the Pod.

---

## ⚠️ Step 4: Fix the Issue

If the ConfigMap was deleted, recreate it:

```bash
kubectl apply -f configmap.yaml
```

Then delete the failed Pod:

```bash
kubectl delete pod nginx-pod-2 -n config-secret-demo
```

Create the Pod again:

```bash
kubectl apply -f pod2.yaml
```
> **📌 Step 4: Fix the Issue**

If the ConfigMap was deleted, recreate it:

```bash
kubectl apply -f configmap.yaml
```

Then delete the failed Pod:

```bash
kubectl delete pod nginx-pod-2 -n config-secret-demo
```

Create it again:

```bash
kubectl apply -f pod2.yaml
```

 

