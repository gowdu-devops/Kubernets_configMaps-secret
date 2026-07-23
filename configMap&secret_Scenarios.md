## Scenario 1

Delete ConfigMap

kubectl delete cm app-config -n config-secret-demo

Expected

Existing Pods continue to run.
New Pods that require the ConfigMap may fail to start if they reference missing keys.
