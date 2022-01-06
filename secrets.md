#### Создать 2 secrets и добавить к созданному поду
* Прибиндить как файловую систему
* Прибиндить как env var
<details>  
```bash  
kubectl run nginx --image=nginx --dry-run=client -oyaml >nginx.yaml  
kubectl create secret generic sec --from-literal=user=admin  
kubectl create secret generic sec1 --from-literal=pass=PassWord
```
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
    volumeMounts:
    - name: sec
      mountPath: "/etc/secrets"
      readOnly: true
    env:
      - name: PASS
        valueFrom:
          secretKeyRef:
            name: sec1
            key: pass
  volumes:
  - name: sec
    secret:
      secretName: sec
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```  
```bash  
kubectl exec nginx -- cat /etc/secrets/user  
kubectl exec nginx -- env  
```
</details>
  