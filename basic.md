#### Создать  под nginx имеративным и декларативным методом с образом nginx:1.16
<details>  

* Imperative
``` bash
kubectl run nginx --image=nginx:1.16
```
* Declarative
``` bash
kubectl run nginx --image=nginx:1.16 --dry-run=client -oyaml >nginx.yaml
kubectl create -f nginx.yaml
```
</details>

#### Поменять образ пода на nginx:1.17
<details>

``` bash
# Один варинат
kubectl edit pod nginx
# Второй вариант
vim nginx.yaml
kubectl replace -f nginx.yaml --force
```
</details>

#### Replicaset. Создать с именем nginx1, c образом nginx:1.17, c тремя репликами 
<details>

``` yaml
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx1
  labels:
    tier: nginx1
spec:
  replicas: 3
  selector:
    matchLabels:
      tier: nginx1
  template:
    metadata:
      labels:
        tier: nginx1
    spec:
      containers:
      - name: nginx1
        image: nginx:1.17
---
```

</details>

#### Deployments. Развернуть Deployments nginx c образом nginx, с 3 репликами декларативным и императивным способом
<details>

* Imperative
``` bash
kubectl create deployment nginx --image=nginx --replicas=3
```
* Declarative
``` bash
kubectl create deployment nginx --image=nginx --replicas=3 --dry-run=client -oyaml >nginx.yaml
kubectl create -f nginx.yaml
```
</details>

#### Поменять образ созданного deployments с nginx на nginx:1.16/. 
* Просмотреть историю изменений.
*  Вернуться после измений образ на начальныйю
*   Расширить реплики до 5.
*  Уменьшить количество хранимой истории изменений до 3. Подсказка(параметр .spec.revisionHistoryLimit)
<details>

``` bash
 kubectl get deployments.apps
 kubectl set image deployment.v1.apps/nginx nginx=nginx:1.17
 # or
 kubectl set image deployment/nginx nginx=nginx:1.17
 # or
 kubectl eidt deployment/nginx
``` 
* check
``` bash
kubectl rollout status deployment/nginx
#or
kubectl rollout status deployment nginx
kubectl get rs
# History
kubectl rollout history deployment/nginx
# Подробности ревизии
kubectl rollout history deployment nginx --revision 1
# Возврат к первой ревизии
kubectl rollout undo deployment nginx --to-revision=1
# Увеличиваем реплики до 5
kubectl scale deployment nginx --replicas=5
# Меняем количество хранимых ревизий
.spec.revisionHistoryLimit: 3
```
</details>
