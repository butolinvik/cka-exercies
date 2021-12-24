#### Service account
* Создать сервис аккаунт
* Посмотреть описание secret, который создался после создания аккаунта
* Запустить pod с образом nginx и созданным сервисаккаунтом
* Где внутри пода хранятся данные о сервиаккаунте?
<details>
```bash

kubectl create serviceaccount testsvc  
kubectl get serviceaccounts,secret  
kubectl describe secrets testsvc-token-76df2  
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
  serviceAccountName: testsvc
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```bash
mount | grep secret  
ls /run/secrets/kubernetes.io/serviceaccount/  
curl https://kubernetes -kv  

```
</details>

* Настроить serviceaccount права для чтения подов, редактирования, удаления и т.д.

<details>

```bash
kubectl auth can-i delete pod --as testsvc  
kubectl auth can-i delete pod --as system:serviceaccount:default:testsvc  
kubectl create clusterrolebinding tessvc --clusterrole=edit --serviceaccount default:testsvc


</details>