#### Создать под, который будет разворачиваться на определенной ноде
<details>

```bash
kubectl run nginx --image=nginx --dry-run=client -oyaml >nginx.yaml
```
* Поле nodeName имя ноды
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
  nodeName: cks-worker2
status: {}
```
  
</details>
