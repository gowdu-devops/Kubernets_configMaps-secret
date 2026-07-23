# Scenario 1
## <img width="293" height="27" alt="image" src="https://github.com/user-attachments/assets/95d97ae8-7aad-4229-9bf8-b8b1dba2bbc2" />
## <img width="304" height="52" alt="image" src="https://github.com/user-attachments/assets/88bd619d-aeb8-4827-bd73-87950aab7726" />
## <img width="326" height="130" alt="image" src="https://github.com/user-attachments/assets/81136c0a-c56b-4de5-91a2-385bb378e54e" />
## <img width="287" height="133" alt="image" src="https://github.com/user-attachments/assets/bca22d8c-dd8d-45c6-a6ba-10246972975e" />
## <img width="185" height="457" alt="image" src="https://github.com/user-attachments/assets/3f14a8d2-c49a-4502-8564-a7d3b512977a" />
## <img width="612" height="96" alt="image" src="https://github.com/user-attachments/assets/cf265a92-ff76-44c4-8757-c42075676d1e" />

## Delete ConfigMap

kubectl delete cm app-config -n config-secret-demo

## Expected

Existing Pods continue to run.
New Pods that require the ConfigMap may fail to start if they reference missing keys.
