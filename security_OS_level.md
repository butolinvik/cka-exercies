#### 
<details>

```bash  
# User nad groups
kubectl run nginx --image=nginx --command -oyaml --dry-run=client >nginx.yaml -- sh -c 'sleep 1d'  
k exec -it nginx -- bash  
id  
touch file.sh  
ls -lh file.sh  
exit
# Заходим в доки, добавляем security context на уровне containers and repeate above

#ТЕперь пробуем делать security context на уровне image. runAsNonRoot  
# Privileged Containers  privileged enable disable


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
  #securityContext:
   # runAsUser: 1000
   # runAsGroup: 3000
  containers:
  - command:
    - sh
    - -c
    - sleep 1d
    image: nginx
    name: nginx
    resources: {}
    securityContext:
      privileged: true
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
</details>

#### Pod Security Policy
<details>



</details>