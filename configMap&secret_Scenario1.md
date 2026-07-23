# Scenario 1 : Delete ConfigMap
### Create a yaml files and solve that issue
```text
  ls -lrth
  ls
```
## <img width="293" height="27" alt="image" src="https://github.com/user-attachments/assets/95d97ae8-7aad-4229-9bf8-b8b1dba2bbc2" />
```text
cat namespace.yaml
```
## <img width="304" height="52" alt="image" src="https://github.com/user-attachments/assets/88bd619d-aeb8-4827-bd73-87950aab7726" />
```text
cat configmap.yaml
```
## <img width="326" height="130" alt="image" src="https://github.com/user-attachments/assets/81136c0a-c56b-4de5-91a2-385bb378e54e" />
```text
cat secret.yaml
```
## <img width="287" height="133" alt="image" src="https://github.com/user-attachments/assets/bca22d8c-dd8d-45c6-a6ba-10246972975e" />
```text
cat pod.yaml
```
## <img width="185" height="457" alt="image" src="https://github.com/user-attachments/assets/3f14a8d2-c49a-4502-8564-a7d3b512977a" />
```bash
kubectl get po -n demo
```
## <img width="612" height="96" alt="image" src="https://github.com/user-attachments/assets/cf265a92-ff76-44c4-8757-c42075676d1e" />

## Delete ConfigMap

```text
kubectl delete cm app-config -n demo
```
## <img width="779" height="311" alt="image" src="https://github.com/user-attachments/assets/8de72c26-085d-45e9-afda-3f537d7df240" />

## Expected

## Existing Pods continue to run.
### New Pods that require the ConfigMap may fail to start if they reference missing keys.
```text
kubectl get po -n demo
kubectl apply -f pod.yaml -n demo
```
## <img width="716" height="176" alt="image" src="https://github.com/user-attachments/assets/9ff0753d-4efb-4514-974c-2c1048826317" />


# Troubleshooting Flow
## 🔍 Step 1: Check the Pod Status

Run the following command:

```bash
kubectl get pods -n demo
```

Example Output:

# <img width="554" height="84" alt="image" src="https://github.com/user-attachments/assets/6ffdcc93-bc11-454e-8588-8fcee8eee7a0" />

---

## 🔍 Step 2: Describe the Pod (Most Important)

Run the following command:

```bash
kubectl describe pod nginx-1 -n demo
```

Go to the **Events** section at the bottom.

Example:

```text
Events:
Warning  Failed     Error: configmap "app-config" not found
```

# <img width="792" height="676" alt="image" src="https://github.com/user-attachments/assets/e8778d87-be74-4bbc-bd8f-79e3bd57d6d2" />
# <img width="817" height="647" alt="image" src="https://github.com/user-attachments/assets/58f450b0-2f80-4ff2-b908-aac28aa41d15" />

> **📌 Note:** The **Events** section tells you the exact reason why the Pod failed.

---

## 🔍 Step 3: Check Whether the ConfigMap Exists

Run the following command:

```bash
kubectl get configmap -n demo
```

Example:

```text
NAME         DATA   AGE
app-config   4      5m
```

If you don't see **app-config**, it has been deleted.

> **📌 Note:** If the ConfigMap does not exist, recreate it before creating or restarting the Pod.

---

> **📌 Step 4: Fix the Issue**

If the ConfigMap was deleted, recreate it:

```bash
kubectl apply -f configmap.yaml
```

Then delete the failed Pod:

```bash
kubectl delete pod nginx-1 -n demo
```

Create it again:

```bash
kubectl apply -f pod1.yaml
```
# <img width="940" height="161" alt="image" src="https://github.com/user-attachments/assets/b2f95dd7-1ec6-45ad-88a8-4b52093c5560" />

 

